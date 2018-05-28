# ActiveMQ部署

## 版本号

5.14.5

## 解压压缩包

```text
[root@sis01 server]# pwd
/iflytek/server
[root@sis01 server]# unzip activemq.zip 
Archive:  zkui2.0.zip
   creating: zkui2.0/[root@sis01 server]# cd /iflytek/server/
[root@sis01 server]# tar -zxvf activemq-5.14.5.tar.gz 
apache-activemq-5.14.5/
apache-activemq-5.14.5/LICENSE
.............................................
.............................................
.............................................
apache-activemq-5.14.5/lib/activemq-broker-5.14.5.jar
apache-activemq-5.14.5/lib/geronimo-j2ee-management_1.1_spec-1.0.1.jar
apache-activemq-5.14.5/lib/geronimo-jta_1.0.1B_spec-1.0.1.jar
[root@sis01 server]# 
[root@sis01 server]# mv apache-activemq-5.14.5/ activemq
[root@sis01 server]# rm -rf activemq-5.14.5.tar.gz
```

## 启动activemq

```text
[root@sis01 bin]# pwd
/iflytek/server/activemq/bin
[root@sis01 bin]# ls
activemq  activemq-diag  activemq.jar  env  linux-x86-32  linux-x86-64  macosx  wrapper.jar
[root@sis01 bin]# ./activemq start
INFO: Loading '/iflytek/server/activemq//bin/env'
INFO: Using java '/iflytek/java/jdk1.8.0_71/bin/java'
INFO: Starting - inspect logfiles specified in logging.properties and log4j.properties to get details
INFO: pidfile created : '/iflytek/server/activemq//data/activemq.pid' (pid '40406')
```

## 验证activemq

```text
[root@sis01 bin]# ./activemq status
INFO: Loading '/iflytek/server/activemq//bin/env'
INFO: Using java '/iflytek/java/jdk1.8.0_71/bin/java'
ActiveMQ is running (pid '40406')
[root@sis01 bin]#
```

## 网页验证

通过浏览器访问网址 [http://192.168.83.240:8161/](http://192.168.83.240:8161/) 点击[Manage ActiveMQ broker](http://192.168.83.240:8161/admin/)输入账号和口令admin/admin即可查看相关队列消息。

