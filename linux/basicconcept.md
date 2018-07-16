## Linux 系统下的一些基本概念

### 文件夹的权限
对于文件夹的权限，可以这样来理解，首先文件夹也是一种文件，其中的文件的文件名就是文件的内容，而文件夹的具体信息就是它执行时的信息。
r 权限：读取文件夹内文件或文件夹名的权限。
x 权限：进入改文件夹，读取其他信息的权限
w 权限：修改文件夹内文件或文件夹的名字和其他信息的权限。


### 常用命令

#### 用户管理
useradd userdel passwd groups usermod chmod chown 

#### 了解命令信息
whatis info  man  where  whereis  which 

#### 查找
locate  find  grep 

#### 查看
ls  cat   head  tail  diff  less more

#### 链接
`ln sourcefile targetfile`   硬链接 
`ln -s sourcefile targetfile` 软链接

#### 文本
awk sed grep sort uniq tr cut paste wc 

#### 磁盘
df tar du mount mkfs

#### 进程管理
ps top pgrep lsof kill pmap 

#### 性能监控
free vmstat  watch  iostat 

#### 网络工具
netstat wget curl route host ping nslookup traceroute ssh scp sftp 

#### 系统以及IPC管理
lsipc  uname arch date strace ipcs ipcrm 

更多命令参考[Linux 命令大全](http://www.runoob.com/linux/linux-command-manual.html) 以及 Linux Shell Cookbook