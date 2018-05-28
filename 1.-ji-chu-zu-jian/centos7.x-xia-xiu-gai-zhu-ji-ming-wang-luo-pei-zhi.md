# CentOS7.x下修改主机名网络配置

描述了系统CentOS7.X下如何修改**主机名**和验证方式。

## 依赖组件

此项操作需要CentOs系统的**root**用户权限。

## 修改步骤

### 查看当前主机名

```bash
[root@iengine-dev ~]# hostname
iengine-dev
[root@iengine-dev ~]#
```

> 如果当前主机名称为localhost.localdomain
>
> ```text
> [root@localhost ~]# hostname
> localhost.localdomain
> [root@localhost ~]#
> ```
>
> 则需要修改主机名称
>
> 方案一、
>
> ```bash
> [root@localhost ~]# hostnamectl set-hostname iengine-dev
>
> [root@localhost ~]# hostname
>
> iengine-dev
>
> [root@localhost ~]# 
>
> [root@localhost ~]# vim /etc/hostname
>
> iengine-dev
> ```
>
> 方案二、
>
> 修改配置文件 /etc/hostname 保存退出，然后重启机器。
>
> ```text
> vi /etc/hostname 
> iengine-dev
> ```

### 执行ping主机名

```bash
[root@iengine-dev ~]# ping iengine-dev
PING localhost (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.021 ms
64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.017 ms
64 bytes from localhost (127.0.0.1): icmp_seq=3 ttl=64 time=0.017 ms
64 bytes from localhost (127.0.0.1): icmp_seq=42 ttl=64 time=0.017 ms
^C
--- localhost ping statistics ---
42 packets transmitted, 42 received, 0% packet loss, time 41172ms
rtt min/avg/max/mdev = 0.016/0.016/0.025/0.005 ms
[root@iengine-dev ~]#
```

如果出现如下情况，请执行下一步操作，完成配置

```bash
[root@iengine-dev ~]# ping iengine-dev
ping: unknown host iengine-dev
[root@iengine-dev ~]#
```

### 修改hosts配置

配置主机名称ip地址映射，通过vim命令编辑/etc/hosts，追加之前通过hostname命令出来的主机名，不需要重启服务器即可。

```bash
[root@iengine-dev ~]# vim /etc/hosts
```

原始文件内容如下： 127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4 ::1 localhost localhost.localdomain localhost6 localhost6.localdomain6 追加主机名后文件内容如下： 127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4 ::1 localhost localhost.localdomain localhost6 localhost6.localdomain6

192.168.65.3 iengine-dev

保存并退出。

### 验证hosts配置是否配置

执行命令ping命令进行验证，输入如下类似结果，则说明配置生效

```bash
[root@iengine-dev ~]# ping iengine-dev
PING localhost (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.021 ms
64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.017 ms
64 bytes from localhost (127.0.0.1): icmp_seq=3 ttl=64 time=0.017 ms
64 bytes from localhost (127.0.0.1): icmp_seq=42 ttl=64 time=0.017 ms
^C
--- localhost ping statistics ---
42 packets transmitted, 42 received, 0% packet loss, time 41172ms
rtt min/avg/max/mdev = 0.016/0.016/0.025/0.005 ms
[root@iengine-dev ~]#
```

## 配置说明

Linux系统在向DNS服务器发出域名解析请求之前会查询/etc/hosts文件，如果里面有相应的记录，就会使用hosts里面的记录。由此，/etc/hosts于设置hostname是没直接关系的，仅仅当你要在本机上用新的hostname来映射自己的IP时候才会用到/etc/hosts文件

## 注意事项

添加主机名需要在/etc/profile文件的127.0.0.1和::1后面均添加主机名，且新添加的主机名和已有的数据用半角空格做分隔

