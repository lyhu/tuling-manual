# ZKUI部署

## 版本号
2.0

## 解压压缩包
```
[root@sis01 server]# pwd
/iflytek/server
[root@sis01 server]# unzip zkui2.0.zip 
Archive:  zkui2.0.zip
   creating: zkui2.0/
  inflating: zkui2.0/.zip            
  inflating: zkui2.0/README.md       
  inflating: zkui2.0/config.cfg      
  inflating: zkui2.0/start.sh        
  inflating: zkui2.0/stop.sh         
  inflating: zkui2.0/zkui-2.0-SNAPSHOT-jar-with-dependencies.jar  
  inflating: zkui2.0/zkui.h2.db      
  inflating: zkui2.0/zkui.log        
[root@sis01 server]# rm -rf zkui2.0.zip
```

## 赋权可执行权限

```
[root@sis01 server]# cd zkui2.0/
[root@sis01 zkui2.0]# ls
config.cfg  README.md  start.sh  stop.sh  zkui-2.0-SNAPSHOT-jar-with-dependencies.jar  zkui.h2.db  zkui.log
[root@sis01 zkui2.0]# chmod +x *.sh
```

## 编辑配置文件

```
[root@sis01 zkui2.0]# vim config.cfg
```

```
#Server Port
serverPort=9090
#Comma seperated list of all the zookeeper servers
zkServer=localhost:2181
#Http path of the repository. Ignore if you dont intent to upload files from repository.
scmRepo=http://myserver.com/@rev1=
#Path appended to the repo url. Ignore if you dont intent to upload files from repository.
scmRepoPath=//appconfig.txt
#if set to true then userSet is used for authentication, else ldap authentication is used.
ldapAuth=false
ldapDomain=mycompany,mydomain
userSet = {"users": [{ "username":"admin" , "password":"manager","role": "ADMIN" },{ "username":"appconfig" , "password":"appconfig","role": "USER" }]}
#ldap authentication url. Ignore if using file based authentication.
ldapUrl=ldap://<ldap_host>:<ldap_port>/dc=mycom,dc=com
#Specific roles for ldap authenticated users. Ignore if using file based authentication.
ldapRoleSet={"users": [{ "username":"domain\\user1" , "role": "ADMIN" }]}
#Set to prod in production and dev in local. Setting to dev will clear history each time.
env=prod
jdbcClass=org.h2.Driver
jdbcUrl=jdbc:h2:zkui
jdbcUser=root
jdbcPwd=manager
#If you want to use mysql db to store history then comment the h2 db section.
#jdbcClass=com.mysql.jdbc.Driver
#jdbcUrl=jdbc:mysql://localhost:3306/zkui
#jdbcUser=root

```

修改zkServer配置项即可，如果是集群，将所有的server地址都填写在zkServer中，使用逗号隔开。

这里调整配置如下：zkServer=192.168.83.240:2181

> 用户登录口令信息见配置项userSet

## 启动zkui2.0

```
[root@sis01 zkui2.0]# ls
config.cfg  README.md  start.sh  stop.sh  zkui-2.0-SNAPSHOT-jar-with-dependencies.jar  zkui.h2.db  zkui.log
[root@sis01 zkui2.0]# ./start.sh 
INFO: ./zkui-2.0-SNAPSHOT-jar-with-dependencies.jar is running! pid=39778
[root@sis01 zkui2.0]# jps
39778 zkui-2.0-SNAPSHOT-jar-with-dependencies.jar
39901 Jps
39454 QuorumPeerMain
[root@sis01 zkui2.0]# 

```



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
