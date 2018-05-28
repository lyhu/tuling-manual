# Skynet部署

## 解压压缩包

```text
[root@sis01 server]# ls
activemq  es-5.4.0  skynet-boot-build-2.0.0-SNAPSHOT-Build_1006.zip  zkui2.0  zookeeper
[root@sis01 server]# unzip skynet-boot-build-2.0.0-SNAPSHOT-Build_1006.zip 
Archive:  skynet-boot-build-2.0.0-SNAPSHOT-Build_1006.zip
   creating: skynet-boot-build-2.0.0-SNAPSHOT/
 extracting: skynet-boot-build-2.0.0-SNAPSHOT/plugin/ant/lib/joda-time-2.9.9.jar  
 extracting: skynet-boot-build-2.0.0-SNAPSHOT/plugin/ant/lib/android-json-0.0.20131108.vaadin1.jar  
.........................................................
.........................................................
extracting: skynet-boot-build-2.0.0-SNAPSHOT/lib/reflections-0.9.10.jar  extracting: skynet-boot-build-2.0.0-SNAPSHOT/lib/commons-cli-1.4.jar  
extracting: skynet-boot-build-2.0.0-SNAPSHOT/lib/jasypt-1.9.0.jar 
[root@sis01 server]#
```

## 重命名文件夹名称

```text
[root@sis01 server]# rm -rf skynet-boot-build-2.0.0-SNAPSHOT-Build_1006.zip 
[root@sis01 server]# mv skynet-boot-build-2.0.0-SNAPSHOT/ skynet
[root@sis01 server]# ls
activemq  es-5.4.0  skynet  zkui2.0  zookeeper
[root@sis01 server]#
```

## 赋予可执行权限

```text
[root@sis01 bin]# cd /iflytek/server/skynet/bin/
[root@sis01 bin]# ls
ant-env.cmd     ant-server.sh   ant-worker.sh     ant-xmanager.sh  freeSSHd.exe
ant-server.cmd  ant-worker.cmd  ant-xmanager.cmd  deamon.sh        readme.txt
[root@sis01 bin]# chmod +x *.sh
[root@sis01 bin]# ls
ant-env.cmd     ant-server.sh   ant-worker.sh     ant-xmanager.sh  freeSSHd.exe
ant-server.cmd  ant-worker.cmd  ant-xmanager.cmd  deamon.sh        readme.txt
[root@sis01 bin]#
```

## 调整配置

编辑配置文件

```text
[root@sis01 conf]# cd /iflytek/server/skynet/conf/
[root@sis01 conf]# ls
application.properties  keystore.p12  skynet.properties  skynet二次开发手册.pdf
[root@sis01 conf]# vim skynet.properties
```

文件内容如下

```text
skynet.zookeeper.cluster_name=skynet
skynet.zookeeper.server_list=127.0.0.1:2181
skynet.zookeeper.session_timeout=20000
skynet.zookeeper.connection_timeout=30000
```

编辑skynet.zookeeper.server\_list，当存在多个时用英文半角逗号分隔

## 导入zk配置

找到配置文件skynet.zk.config

```text
[root@sis01 doc]# cd /iflytek/server/skynet/doc/
[root@sis01 doc]# ls
JSON-handle_0.5.6.crx  README.md  release.md  skynet.zk.config  skynet二次开发手册.md  skynet部署手册（2.0.0版本以上）.md
[root@sis01 doc]#
```

编辑配置文件skynet.zk.config 1. 替换localhost为192.168.83.240 2. 替换127.0.0.1为192.168.83.240

## 启动skynet服务

```text
[root@sis01 doc]# cd /iflytek/server/skynet/bin/
[root@sis01 bin]# ls
ant-env.cmd     ant-server.sh   ant-worker.sh     ant-xmanager.sh  freeSSHd.exe
ant-server.cmd  ant-worker.cmd  ant-xmanager.cmd  deamon.sh        readme.txt
[root@sis01 bin]# ./ant-xmanager.sh 
del the jar: /iflytek/server/skynet/plugin/ant/lib/joda-time-2.9.9.jar
del the jar: /iflytek/server/skynet/plugin/ant/lib/xpp3_min-1.1.4c.jar
..........................................................
..........................................................
try starting skynet-cloud-xmanager
----------------
skynet-cloud-xmanager started.
logging to /iflytek/server/skynet/log/skynet-cloud-xmanager-2.0.0-SNAPSHOT.log
----------------
```

## 验证skynet服务

打开浏览器访问网页[http://192.168.83.240:2230](http://192.168.83.240:2230) 输入账号和口令admin/admin

## 注册服务器

skynet登录成功后进入首页，点击注册服务器按钮，输入当前主机IP,ssh端口，root用户名,密码，备注信息

## 启动集群

点击启动集群然后输入START即可完成集群的启动

## 启动服务器

点击操作，选择启动服务器，然后输入START即可完成服务器的启动

