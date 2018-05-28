# Zookeeper部署

## 版本号
3.4.5

## 部署模式
### Standalone模式

#### 调整配置文件
```
[root@sis01 server]# tar -zxvf zookeeper.tar.gz
[root@sis01 server]# rm -rf zookeeper.tar.gz
[root@sis01 zookeeper]# pwd
/iflytek/server/zookeeper
[root@sis01 zookeeper]# cd /iflytek/server/zookeeper/
[root@sis01 zookeeper]# echo > zookeeper.out
```
编辑文件/iflytek/server/zookeeper/conf/zoo.cfg
```
[root@sis01 conf]# vim /iflytek/server/zookeeper/conf/zoo.cfg
```
调整配置项server.1为server.1=sis01:2888:3888
> 其中dataDir配置项默认为：/home/zookeeper
#### 新建myid文件
创建目录/home/zookeeper
```
[root@sis01 conf]# mkdir /home/zookeeper
[root@sis01 conf]# echo "1" > /home/zookeeper/myid
[root@sis01 conf]# 
```
#### 启动zk服务
```
[root@sis01 bin]# cd /iflytek/server/zookeeper/bin
[root@sis01 bin]# ls
README.txt  zkCleanup.sh  zkCli.cmd  zkCli.sh  zkEnv.cmd  zkEnv.sh  zkServer.cmd  zkServer.sh  zookeeper.out
[root@sis01 bin]# ./zkServer.sh start
JMX enabled by default
Using config: /iflytek/server/zookeeper/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED
[root@sis01 bin]# 
```
#### 验证zk服务
```
[root@sis01 bin]# cd /iflytek/server/zookeeper/bin
[root@sis01 bin]# ls
README.txt  zkCleanup.sh  zkCli.cmd  zkCli.sh  zkEnv.cmd  zkEnv.sh  zkServer.cmd  zkServer.sh  zookeeper.out
[root@sis01 bin]# ./zkServer.sh status
JMX enabled by default
Using config: /iflytek/server/zookeeper/bin/../conf/zoo.cfg
Mode: standalone
[root@sis01 bin]# 
```
至此Standalone模式服务端安装完成。

