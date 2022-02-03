# [MacOS升级python](https://cloud.tencent.com/developer/article/1610063)
查看当前系统的python的版本
```shell
python -V
```
显示的版本是
```shell
➜  ~ python -V
Python 2.7.10
```
python官网下载: https://www.python.org/downloads/
下载完成后是一个pkg的安装包，默认安装路径是:
```shell
/Library/Frameworks/Python.framework/Versions/3.9.10
```
安装完成后进入终端，输入open .barsh_profile, 添加环境变量
```shell
alias python=/Library/Frameworks/Python.framework/Versions/3.9/bin/python3.9
```
修改完成后保存退出，并在终端执行
```shell
source ~/.bash_profile
```
作用使设置立即生效。这时候再查看python的版本
```shell
Last login: Tue Feb  1 12:01:45 on ttys002
➜  ~ python -V
Python 3.9.10
```