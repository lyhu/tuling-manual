# CentOS7.x 文件句柄设置

在Linux下，我们使用ulimit -n命令可以看到单个进程能够打开的最大文件句柄数量(socket连接也算在里面)。系统默认值1024。对于一般的应用来说(象Apache、系统进程)1024完全足够使用。但是如何象squid、mysql、java等单进程处理大量请求的应用来说就有点捉襟见肘了如果单个进程打开的文件句柄数量超过了系统定义的值，就会提到“too many files open”的错误提示。

## 知识学习

### 查看系统默认的最大文件句柄梳理
查看Linux系统默认的最大文件句柄数，系统默认是1024。
``` bash
# ulimit -n
1024
```
### 查看Linux系统默认的最大文件句柄数详细说明

```bash
# ulimit -a
core file size          (blocks, -c) 0  
data seg size           (kbytes, -d) unlimited  
scheduling priority             (-e) 30  
file size               (blocks, -f) unlimited  
pending signals                 (-i) 30605  
max locked memory       (kbytes, -l) 40000  
max memory size         (kbytes, -m) unlimited  
open files                      (-n) 1024
    
pipe size            (512 bytes, -p) 8  
POSIX message queues     (bytes, -q) 819200  
real-time priority              (-r) 65  
stack size              (kbytes, -s) 8192  
cpu time               (seconds, -t) unlimited  
max user processes              (-u) 30605  
virtual memory          (kbytes, -v) unlimited  
file locks                      (-x) unlimited 
```

### 查看某个应用的进程PID
```
# ps aux| grep mysql 
mysql     5474  0.0  0.6 8704208 396136 ?      Sl    7Ղ26 156:54 /usr/sbin/mysqld --basedir=/usr --datadir=/var/lib/mysql --plugin-dir=/usr/lib/mysql/plugin --log-error=/var/log/mysql/error.log --pid-file=/var/run/mysqld/mysqld.pid --socket=/var/run/mysqld/mysqld.sock
root      8199  0.0  0.0  14096   968 pts/3    S+   14:24   0:00 grep --color=auto mysql

```
### 查看某个进程打开的文件句柄数量
```bash
# lsof -n | grep 5474 -c
6480
```
### 查看系统所有进程以及各进程打开的文件句柄数量
```bash
root@ubuntu:~# lsof -n|awk '{print $2}'|sort|uniq -c|sort -nr
 100015 24446
  73220 10515
  30618 27008
  30618 26943
  28130 27187
  22176 19084
  15565 26891
  13685 4083
   9360 24143
   9000 19529
   7440 16600
   6480 5474
   6250 18940
   4158 13083
   3869 14017
   1798 18817
    252 14497
    189 31639
    108 29620
     92 625
     86 10695
     86 10685
     86 10678
      3 92
      3 91
      3 10
      1 PID
root@ubuntu:~# 
```
> 第一列是打开的句柄数，第二列是进程号
> 如果想分页展示的话可以使用lsof -n |awk '{print $2}'|sort|uniq -c |sort -nr|more命令

### 查看单个进程的详细信息
```bash
# ps -aef|grep 24446
root      8302  8165  0 14:31 pts/3    00:00:00 grep --color=auto 24446
root     24446     1  0 10Ղ23 ?       02:04:39 java -Xms128M -Xmx2G -Djava.ext.dirs=/iflytek/skynet/lib:/iflytek/skynet/plugin/engine/lib: -Dfile.encoding=UTF-8 -Dcluster_group_name=skynet -Duser.timezone=GMT+0800 -Dzookeeper_servers=10.1.186.101:2181 -Dzookeeper_sessionTimeout=20000 -Dzookeeper_connectionTimeout=5000 -DSKYNET_HOME=/iflytek/skynet -DVERSION=1.3.1-SNAPSHOT -Djava.library.path=.:../lib/sigar -jar /iflytek/skynet/plugin/xmanager/lib/skynet-xmanager-1.3.1-SNAPSHOT.jar --address= --port=
```
## 解决方案

### 环境变量
编辑`/etc/profile`文件，在文件尾部添加
```bash
# vim /etc/profile

ulimit -c unlimited
ulimit -u 10000
ulimit -n 102400
ulimit -m unlimited
ulimit -s unlimited
ulimit -v unlimited
ulimit -t unlimited
```
> ulimit -c unlimited 将设定core文件的最大值，单位为区块，大小不限制
> ulimit -u 10000  将某linux用户的最大进程数设为10000个
> ulimit -n 102400 将每个进程可以打开的文件数目加大到102400
> ulimit -m unlimited 最大内存大小不限制
> ulimit -s unlimited 堆栈大小不限制
> ulimit -v unlimited 虚拟内存不限制
> ulimit -t unlimited CPU 时间不限制

### 系统配置
编辑`/etc/security/limits.conf`文件，在文件尾部添加
```bash
# vim /etc/security/limits.conf

*        soft    noproc  10240
*        hard    noproc  10240
*        soft    nofile  10240
*        hard    nofile  10240
root     soft    noproc  10240
root     hard    noproc  10240
root     soft    nofile  10240
root     hard    nofile  10240

```
> 对于ubuntu操作系统，必须设置最后四行，否则不生效
> 修改完了，要退出终端，重新打开xshell重新登录


### 系统最大文件句柄数
编辑`/etc/sysctl.conf`文件，在文件尾部添加
```
fs.file-max=655350
```
运行命令/sbin/sysctl -p 使配置生效

> fs.file-max=655350 #限制整个系统最大文件句柄数
> 修改完了，要退出终端，重新打开xshell重新登录

