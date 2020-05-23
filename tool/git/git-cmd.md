# git 恢复删除的远程分支

在一次项目开发时候，误删一个远程分支，如何恢复呢？

### 查看reflog, 找到最后一次commitid

```shell
git reflog --date=iso
```

reflog是reference log的意思，也是引用log, 记录HEAD在各个分支上的移动轨迹。选项```--date=iso```，标识标准时间格式展示。

### 切出分支

```shell
git checkout -b recovery_branch_name commitid
```

### 提交远程仓库

```shell
git push  origin recovery_branch_name 
```

