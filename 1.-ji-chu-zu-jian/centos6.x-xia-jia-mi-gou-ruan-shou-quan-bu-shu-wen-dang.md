# CentOS6.X下加密狗软授权部署文档

## CentOS6.X下加密狗软授权部署文档

## CentOS6.X下加密狗软授权部署文档

> 描述了系统CentOS6.X下加密狗软授权部署过程

### 依赖组件

> 此项操作需要CentOs系统的**root**用户权限。

### 下载

```bash
    root@iengine-dev ~]# wget http://192.168.65.191:8082/installs/aksusbd/aksusbd-2.5.1.tar.gz
```

### 安装步骤

#### 1. 安装32位依赖库

> 加密狗软授权安装需要依赖32依赖库，对于不同的Linux平台安装方法不同。

```bash
    yum install xulrunner.i686
```

#### 2. 调整安装包位置并解压

> 移动安装包至/iflytek目录

```bash
[root@iengine-dev ~]# mv aksusbd-2.5.1.tar.gz  /iflytek/
[root@iengine-dev ~]# cd /iflytek/
[root@iengine-dev ~]# tar -zxvf aksusbd-2.5.1.tar.gz
```

#### 3. 安装加密狗程序

> 进入/iflytek/aksusbd-2.5.1目录执行安装操作，加密狗驱动程序安装完成后，安装完后在/var/hasplm目录下会生产一个haspvlib\_85015.so文件，如果haspvlib\_85015.so文件生产则说明安装成功

```bash
    [root@iengine-dev ~]# cd /iflytek/aksusbd-2.5.1
    [root@iengine-dev aksusbd-2.5.1]#./dinst
        Copy AKSUSB daemon to /usr/sbin ...
        Copy WINEHASP daemon to /usr/sbin ...
        Copy HASPLMD daemon to /usr/sbin ...
        Copy start-up script to /etc/init.d ...
        Link HASP SRM runtime environment startup script to system startup folder
        Starting HASP SRM runtime environment...
        Starting AKSUSB daemon: .
        Starting WINEHASP daemon: .
        Starting HASPLM daemon: .
        Copying VLIB...
        Done
```

#### 4. 上传授权文件

> 上传引擎软授权文件“sx.v2c”到服务器的/iflytek/aksusbd-2.5.1目录下

```bash
    [root@iengine-dev ~]# cd /iflytek/aksusbd-2.5.1
    [root@iengine-dev aksusbd-2.5.1]# ls
    aksusbd     dinst  hasplmd  hasp.rules   haspvlib_85015.so  winehasp
    aksusbd.rc  dunst  haspman  hasp_update  sx.v2c
    [root@iengine-dev aksusbd-2.5.1]#
```

#### 5. 导入授权文件

> 软授权文件后缀为.v2c,通过软授权可以实现和硬件加密狗一样的效果， 但是软授权使用在一台机器最多只能使用3次，超过了3次要么使用硬狗，要么做新的操作系统重新安装应用。

```bash
    [root@iengine-dev aksusbd-2.5.1]# cd /iflytek/aksusbd-2.5.1-i386
    [root@iengine-dev aksusbd-2.5.1]# ./hasp_update u /iflytek/aksusbd-2.5.1/sx.v2c
```

#### 6. 验证授权是否成功

> 验证加密狗驱动是否安装成功，出现下述信息则说明授权成功

```bash
    [root@iengine-dev aksusbd-2.5.1]# cd /iflytek/aksusbd-2.5.1
    [root@iengine-dev aksusbd-2.5.1]# ./haspman
    iFly HaspMan 4.0.0.1002 Copyright 1999-2005 by iFly Info.Tek. All rights reserved.
    Dongle ID                         :    2147483647
    ------------------------------------------------------------------------
    Dongle Proudcer                   :    Aladdin
    Dongle Eletric Type               :    SL
    Memory Size                       :    4032(Bytes)
    Real-Clock                        :    noneDongle Protection Format          :    Format used by Intp40
    Product counts                    :    1
    ********************
    Intp40 Product No.1:
        Product Name                  :    next-g
        Product Number                :    20161109
        Product Version               :    3.0.0
        Product Licence               :    4
        Product Expired Time Disabled.    Product Run Count Disabled.
        Product Run Time Disabled.
        Support Voices                :    *
        Support Packages              :    All Packages supported
    Total 1 dongles found.
```

### 配置说明

> 如果需要开启web页面访问，需要做额外的配置，默认情况下不支持。

#### 1. 下载配置文件

> 将hashlm.ini配置文件放置到/etc/hasplm目录

```bash
[root@iengine-dev ~]# cd /etc/hasplm/
[root@iengine-dev hasplm]# ls
[root@iengine-dev hasplm]# 
[root@iengine-dev hasplm]# wget http://192.168.65.191:8082/installs/aksusbd/hasplm.ini
[root@iengine-dev hasplm]# ls
hasplm.ini
[root@iengine-dev hasplm]#
```

#### 2. 配置访问方式

> 通过vim命令修改配置文件hashlm.ini配置文件，其中ACCremote=0表示禁用远程访问，ACCremote=1则表示开启远程访问，文件内容示例如下。

```bash
    [root@iengine-dev hasplm]# cat hasplm.ini 
    ;*************************************************************************
    ;*
    ;* Sentinel License Manager configuration file
    ;*
    ;* Version 18.0 1.55506 at localhost.localdomain
    ;* Thu, 21 Jul 2016 06:52:25 GMT
    ;*
    ;*************************************************************************



    [SERVER]
    name = localhost.localdomain
    pagerefresh = 3
    linesperpage = 20
    ACCremote = 1

    enabledetach = 0
    reservedseats = 0
    reservedpercent = 0
    detachmaxdays = 14
    commuter_delete_days = 7

    requestlog = 0
    loglocal = 0
    logremote = 0
    logadmin = 0
    errorlog = 0
    rotatelogs = 0
    access_log_maxsize = 0 ;kB
    error_log_maxsize = 0 ;kB
    zip_logs_days = 0
    delete_logs_days = 0
    pidfile = 0
    passacc = 0

    accessfromremote = 1
    accesstoremote = 1
    bind_local_only = 0  ; 0=all adapters, 1=localhost only


    [UPDATE]
    download_url = www.safenet-inc.com/hasp/language_packs/end-user
    update_host = www3.safenet-inc.com
    language_url = /hasp/language_packs/end-user/


    [REMOTE]
    broadcastsearch = 1
    aggressive = 0
    serversearchinterval = 30


    [ACCESS]


    [USERS]


    [LOGPARAMETERS]
    text = {timestamp} {clientaddr}:{clientport} {clientid} {method} {url} {function}({functionparams}) result({statuscode}){newline}

    [root@iengine-dev hasplm]#
```

#### 3. 重启服务生效配置

> 通过系统服务形式重启授权服务

```bash
[root@iengine-dev aksusbd-2.5.1]# service aksusbd restart
Stopping HASPLM daemon:                                    [  OK  ]
Stopping WINEHASP daemon:                                  [  OK  ]
Stopping AKSUSB daemon:                                    [  OK  ]
Starting AKSUSB daemon:                                    [  OK  ]
Starting WINEHASP daemon:                                  [  OK  ]
Starting HASPLM daemon:                                    [  OK  ]
[root@iengine-dev aksusbd-2.5.1]#
```

### 日常维护

> 软授权在安装时已经注册为系统服务，系统服务名称为**aksusbd**,可以通过service命令进行管理

#### 1. 启动授权服务

```bash
[root@iengine-dev aksusbd-2.5.1]# service aksusbd start
Starting AKSUSB daemon:                                    [  OK  ]
Starting WINEHASP daemon:                                  [  OK  ]
Starting HASPLM daemon:                                    [  OK  ]
[root@iengine-dev aksusbd-2.5.1]#
```

#### 2. 停止授权服务

```bash
[root@iengine-dev aksusbd-2.5.1]# service aksusbd stop
Stopping HASPLM daemon:                                    [  OK  ]
Stopping WINEHASP daemon:                                  [  OK  ]
Stopping AKSUSB daemon:                                    [  OK  ]
[root@iengine-dev aksusbd-2.5.1]#
```

#### 3. 重启授权服务

```bash
[root@iengine-dev aksusbd-2.5.1]# service aksusbd restart
Stopping HASPLM daemon:                                    [  OK  ]
Stopping WINEHASP daemon:                                  [  OK  ]
Stopping AKSUSB daemon:                                    [  OK  ]
Starting AKSUSB daemon:                                    [  OK  ]
Starting WINEHASP daemon:                                  [  OK  ]
Starting HASPLM daemon:                                    [  OK  ]
[root@iengine-dev aksusbd-2.5.1]#
```

### 注意事项

> 软授权使用在一台机器最多只能使用3次，超过了3次要么使用硬狗，要么做新的操作系统重新安装应用。

### 健康检查

> 如果配置了远程访问，可以通过浏览器 [http://192.168.65.3:1947](http://192.168.65.3:1947) 打开网页可以查看授权是否过期和是否授权。

