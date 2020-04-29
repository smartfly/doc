## panic&recover源码解析

painc数据结构：[src/runtime/runtime2.go](https://github.com/golang/go/blob/master/src/runtime/runtime2.go)

```go
type _panic struct {
	argp      unsafe.Pointer // pointer to arguments of deferred call run during panic; cannot move - known to liblink
	arg       interface{}    // argument to panic
	link      *_panic        // link to earlier panic
	pc        uintptr        // where to return to in runtime if this panic is bypassed
	sp        unsafe.Pointer // where to return to in runtime if this panic is bypassed
	recovered bool           // whether this panic is over
	aborted   bool           // the panic was aborted
	goexit    bool
}
```

```runtime.gopanic```在[src/runtime/panic.go](https://github.com/golang/go/blob/master/src/runtime/panic.go)中，其实现：

```go
func gopanic(e interface{}) {
	gp := getg()
	...
	var p _panic
	p.arg = e
	p.link = gp._panic
	gp._panic = (*_panic)(noescape(unsafe.Pointer(&p)))
    
	for {
		d := gp._defer
		if d == nil {
			break
		}

		// defer...
		...
		d._panic = (*_panic)(noescape(unsafe.Pointer(&p)))

		p.argp = unsafe.Pointer(getargp(0))
		reflectcall(nil, unsafe.Pointer(d.fn), deferArgs(d), uint32(d.siz), uint32(d.siz))
		p.argp = nil

		// recover...
		if p.recovered {
			...
			mcall(recovery)
			throw("recovery failed") // mcall should not return
		}
	}

	preprintpanics(gp._panic)

	fatalpanic(gp._panic) // should not return
	*(*int)(nil) = 0      // not reached
}
```

无法恢复的恐慌fatalpanic其源代码: [src/runtime/panic.go](https://github.com/golang/go/blob/master/src/runtime/panic.go)

```go
func fatalpanic(msgs *_panic) {
	pc := getcallerpc()
	sp := getcallersp()
	gp := getg()
	var docrash bool

	systemstack(func() {
		if startpanic_m() && msgs != nil {
		    ...
			printpanics(msgs)
		}

		docrash = dopanic_m(gp, pc, sp)
	})

	systemstack(func() {
		exit(2)
	})

	*(*int)(nil) = 0
}
```

递归打印函数```printpanics```，其源码: [src/runtime/panic.go](https://github.com/golang/go/blob/master/src/runtime/panic.go)

```go
// Print all currently active panics. Used when crashing.
// Should only be called after preprintpanics.
func printpanics(p *_panic) {
	if p.link != nil {
		printpanics(p.link)
		if !p.link.goexit {
			print("\t")
		}
	}
	if p.goexit {
		return
	}
	print("panic: ")
	printany(p.arg)
	if p.recovered {
		print(" [recovered]")
	}
	print("\n")
}
```

恢复```gorecover```，其源代码:[src/runtime/panic.go](https://github.com/golang/go/blob/master/src/runtime/panic.go)

```go
func gorecover(argp uintptr) interface{} {
	// Must be in a function running as part of a deferred call during the panic.
	// Must be called from the topmost function of the call
	// (the function used in the defer statement).
	// p.argp is the argument pointer of that topmost deferred function call.
	// Compare against argp reported by caller.
	// If they match, the caller is the one who can recover.
	gp := getg()
	p := gp._panic
	if p != nil && !p.goexit && !p.recovered && argp == uintptr(p.argp) {
		p.recovered = true
		return p.arg
	}
	return nil
}
```

```gopanic```, 其源代码：[src/runtime/panic.go](https://github.com/golang/go/blob/master/src/runtime/panic.go)

```go
func gopanic(e interface{}) {
	...
	for {
		// defer...
		...
		pc := d.pc
		sp := unsafe.Pointer(d.sp) // must be pointer so it gets adjusted during stack copy
		freedefer(d)
		
		// recover...
		if p.recovered {
			atomic.Xadd(&runningPanicDefers, -1)

			gp._panic = p.link
			for gp._panic != nil && gp._panic.aborted {
				gp._panic = gp._panic.link
			}
			if gp._panic == nil { 
				gp.sig = 0
			}

			gp.sigcode0 = uintptr(sp)
			gp.sigcode1 = pc
			mcall(recovery)
			throw("recovery failed") 
		}
	}
    ...
}
```

```recovery```， 其源代码: [src/runtime/panic.go](https://github.com/golang/go/blob/master/src/runtime/panic.go)

```go
func recovery(gp *g) {
	// Info about defer passed in G struct.
	sp := gp.sigcode0
	pc := gp.sigcode1

	// d's arguments need to be in the stack.
	if sp != 0 && (sp < gp.stack.lo || gp.stack.hi < sp) {
		print("recover: ", hex(sp), " not in [", hex(gp.stack.lo), ", ", hex(gp.stack.hi), "]\n")
		throw("bad recovery")
	}

	// Make the deferproc for this d return again,
	// this time returning 1. The calling function will
	// jump to the standard return epilogue.
	gp.sched.sp = sp
	gp.sched.pc = pc
	gp.sched.lr = 0
	gp.sched.ret = 1
	gogo(&gp.sched)
}
```

```gogo```, 其源代码: [src/runtime/asm_amd64.s](https://github.com/golang/go/blob/master/src/runtime/asm_amd64.s)

```go
// func gogo(buf *gobuf)
// restore state from Gobuf; longjmp
TEXT runtime·gogo(SB), NOSPLIT, $16-8
	MOVQ	buf+0(FP), BX		// gobuf
	MOVQ	gobuf_g(BX), DX
	MOVQ	0(DX), CX		// make sure g != nil
	get_tls(CX)
	MOVQ	DX, g(CX)
	MOVQ	gobuf_sp(BX), SP	// restore SP
	MOVQ	gobuf_ret(BX), AX
	MOVQ	gobuf_ctxt(BX), DX
	MOVQ	gobuf_bp(BX), BP
	MOVQ	$0, gobuf_sp(BX)	// clear to help garbage collector
	MOVQ	$0, gobuf_ret(BX)
	MOVQ	$0, gobuf_ctxt(BX)
	MOVQ	$0, gobuf_bp(BX)
	MOVQ	gobuf_pc(BX), BX
	JMP	BX

```

```gobuf```, 其源代码:[src/runtime/runtime2.go](https://github.com/golang/go/blob/master/src/runtime/runtime2.go)

```go
type gobuf struct {
	// The offsets of sp, pc, and g are known to (hard-coded in) libmach.
	//
	// ctxt is unusual with respect to GC: it may be a
	// heap-allocated funcval, so GC needs to track it, but it
	// needs to be set and cleared from assembly, where it's
	// difficult to have write barriers. However, ctxt is really a
	// saved, live register, and we only ever exchange it between
	// the real register and the gobuf. Hence, we treat it as a
	// root during stack scanning, which means assembly that saves
	// and restores it doesn't need write barriers. It's still
	// typed as a pointer so that any other writes from Go get
	// write barriers.
	sp   uintptr
	pc   uintptr
	g    guintptr
	ctxt unsafe.Pointer
	ret  sys.Uintreg
	lr   uintptr
	bp   uintptr // for GOEXPERIMENT=framepointer
}
```

```walkexpr```, 其源代码: [src/cmd/compile/internal/gc/walk.go](https://github.com/golang/go/blob/master/src/cmd/compile/internal/gc/walk.go)

```go
const(
	OPANIC       // panic(Left)
	ORECOVER     // recover()
	...
)
...
func walkexpr(n *Node, init *Nodes) *Node {
    ...
	switch n.Op {
	default:
		Dump("walk", n)
		Fatalf("walkexpr: switch 1 unknown op %+S", n)

	case ONONAME, OINDREGSP, OEMPTY, OGETG:
	case OTYPE, ONAME, OLITERAL:
	    ...
	case OPANIC:
		n = mkcall("gopanic", nil, init, n.Left)

	case ORECOVER:
		n = mkcall("gorecover", n.Type, init, nod(OADDR, nodfp, nil))
	...
}
```

## Example

在go源代码[src/net/http/server.go](https://github.com/golang/go/blob/master/src/net/http/server.go)中存在一个样例：

```go
func (c *conn) serve(ctx context.Context) {
	c.remoteAddr = c.rwc.RemoteAddr().String()
	ctx = context.WithValue(ctx, LocalAddrContextKey, c.rwc.LocalAddr())
	defer func() {
		if err := recover(); err != nil && err != ErrAbortHandler {
			const size = 64 << 10
			buf := make([]byte, size)
			buf = buf[:runtime.Stack(buf, false)]
			c.server.logf("http: panic serving %v: %v\n%s", c.remoteAddr, err, buf)
		}
		if !c.hijacked() {
			c.close()
			c.setState(c.rwc, StateClosed)
		}
	}()

	if tlsConn, ok := c.rwc.(*tls.Conn); ok {
	...
	}
}
```



## 参考资料

[深入理解 Go panic and recover](https://eddycjy.com/posts/go/panic/2019-05-21-panic-and-recover/)

[Goroutine和Panic](https://blog.csdn.net/chenguolinblog/article/details/90665080)

