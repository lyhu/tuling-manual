# CentOS7.x本地YUM配置

## 创建系统光盘存放位置

```text
[root@sis01 iflytek]# cd /iflytek/
[root@sis01 iflytek]# 
[root@sis01 iflytek]# ls
engine  java  server
[root@sis01 iflytek]# mkdir yum-repo
[root@sis01 iflytek]# ls
engine  java  server  yum-repo
```

## 上传光盘文件

```text
[root@sis01 yum-repo]# cd /iflytek/yum-repo/
[root@sis01 yum-repo]# ls

[root@sis01 yum-repo]# ls
CentOS-7-x86_64-Everything-1611.iso
```

> 一定要确保这个ISO文件上传完毕后再进行下面的操作。

## 创建挂载目录

```text
[root@sis01 iflytek]# mkdir /media/CentOS7
```

## 挂载iso文件

```text
[root@sis01 yum.repos.d]# mount -t iso9660 -o loop /iflytek/yum-repo/CentOS-7-x86_64-Everything-1611.iso  /media/CentOS7
mount: /dev/loop0 is write-protected, mounting read-only
```

## 设置开机自动挂载光盘镜像

```text
[root@sis01 yum-repo]# vim /etc/fstab
```

> fstab 输入错误会使主机无法正常启动而进入emergency mode紧急状态

在文件的最后一行添加如下内容

```text
/iflytek/yum-repo/CentOS-7-x86_64-Everything-1611.iso /media/CentOS7 defaults,ro,loop 0 0
```

## 配置yum源

备份文件夹

```text
[root@sis01 etc]# cp -r  /etc/yum.repos.d/  /etc/yum.repos.d.20180522.bak/
[root@sis01 etc]# cd  /etc/yum.repos.d/
[root@sis01 yum.repos.d]# ls
CentOS-Base.repo  CentOS-CR.repo  CentOS-Debuginfo.repo  CentOS-fasttrack.repo  CentOS-Media.repo  CentOS-Sources.repo  CentOS-Vault.repo
[root@sis01 yum.repos.d]# rm -rf *
[root@sis01 yum.repos.d]# ls
[root@sis01 yum.repos.d]#
```

创建本地yum源文件

```text
[root@sis01 yum.repos.d]#  touch /etc/yum.repos.d/CentOS7-Localsource.repo
[root@sis01 yum.repos.d]#  vim /etc/yum.repos.d/CentOS7-Localsource.repo
```

编辑文件内容如下

```text
[CentOS7-Localsource]
name=CentOS7
baseurl=file:///media/CentOS7
enabled=1
gpgcheck=0
```

## 生成本地yum缓存

```text
[root@sis01 yum.repos.d]# yum makecache
Loaded plugins: fastestmirror, langpacks
CentOS7-Localsource                              | 3.6 kB  00:00:00     
(1/4): CentOS7-Localsource/group_gz              | 155 kB  00:00:00     
(2/4): CentOS7-Localsource/primary_db            | 5.6 MB  00:00:00     
(3/4): CentOS7-Localsource/filelists_db          | 6.6 MB  00:00:00     
(4/4): CentOS7-Localsource/other_db              | 2.4 MB  00:00:00     
Loading mirror speeds from cached hostfile
Metadata Cache Created
[root@sis01 yum.repos.d]#
```

