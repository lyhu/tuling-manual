# ES5.4部署

## 版本

es-5.4.0.tar.gz

## 安装步骤

### 解压压缩包

```text
[root@sis01 server]# cd /iflytek/server/
[root@sis01 server]# tar -zxvf es-5.4.0.tar.gz
[root@sis01 server]# rm -rf es-5.4.0.tar.gz 
[root@sis01 server]# cd es-5.4.0/
[root@sis01 es-5.4.0]# ls
elasticsearch-5.4.0        es.sh        kibana-5.4.0-linux-x86_64  node-v6.11.0-linux-x64
elasticsearch-head-master  es-sql-site  license.json               setup
[root@sis01 es-5.4.0]#
```

### ES命令说明

```text
[root@sis01 es-5.4.0]# ./es.sh 
 =========================================== 
 -- ES 5.4.0  help ..                    -- 
 ------------------------------------------- 
Usage:  elastic.sh [OPTIONS]
    Options:
            setup    init env. add user and config es head esql env.
        start    start elasticsearch and kibana,header,esql service .
        stop    stop elasticsearch and kibana,header,esql service .
        status  show elk status.
        license  authorization elk license . 
        help    print usage 

 =========================================== 
[root@sis01 es-5.4.0]#
```

### 安装ES配置

此项操作仅第一次时需要

```text
[root@sis01 es-5.4.0]# ./es.sh  setup
sed: can't read /etc/security/limits.d/90-nproc.conf: No such file or directory
fs.file-max = 655350
vm.max_map_count = 655360
 begin inti env ------------------- 
 add user elastic ... 
 add user ok.------------------- 
 config node npm grunt ... 
nodejs.version: v6.11.0
npm.version: 3.10.10
grunt.version: grunt-cli v1.2.0
 config node npm grunt ok.------------------ 

 config es host:192.168.83.240 port:9233 ... 
 config es host and port ok.---------- 

 config es head ... 
 config es head: http://192.168.83.240:9133 ok. 
 config esql ... 
 config esql http://192.168.83.240:9430 ok. 

 config kibana ... 
 config kibana ok. 
 config finished.----------------- 
[root@sis01 es-5.4.0]#
```

### 启动ES

```text
[root@sis01 es-5.4.0]# ./es.sh start
 =========================================== 
 -- ES 5.4.0  start ..                   -- 
 ------------------------------------------- 
 Starting elasticsearch  ...   
/etc/profile: line 82: ulimit: max user processes: cannot modify limit: Operation not permitted
 The elasticsearch service is started: http://192.168.83.240:9233 
 Starting header  ...   
 The elasticsearch header is started: http://192.168.83.240:9133 
 Starting esql  ...   
 The elasticsearch sql is started: http://192.168.83.240:9430 
 Starting kibana  ...   
 The kibana service is started: http://192.168.83.240:5631 
 ------------------------------------------- 
 elasticsearch pid:40958 
 header pid:375 
 esql pid:389 
 kibana pid: 
 =========================================== 
[root@sis01 es-5.4.0]#
```

### ES授权

此项操作仅第一次时需要

```text
[root@sis01 es-5.4.0]# ./es.sh license
 =========================================== 
 -- ES 5.4.0  license ..                 -- 
 ------------------------------------------- 
 the default password is: changeme 
Enter host password for user 'elastic':
{"acknowledged":true,"license_status":"valid"}[root@sis01 es-5.4.0]#
```

> 密码此时输入elastic

ES重启

```text
[root@sis01 es-5.4.0]# ./es.sh  stop
 =========================================== 
 -- ES 5.4.0  stop ..                    -- 
 ------------------------------------------- 
 the es is stoped. pid:40958 
 the header is stoped. pid:375 
./es.sh: line 115:   375 Killed                  nohup grunt server > header.log 2>&1
 the esql is stoped. pid:389 
./es.sh: line 115:   389 Killed                  node node-server.js > sql.log 2>&1
 the kibana is stoped. pid:436 
 =========================================== 
[root@sis01 es-5.4.0]# ./es.sh: line 115:   436 Killed                  nohup ./kibana > kibana.log 2>&1

[root@sis01 es-5.4.0]# ./es.sh  start
 =========================================== 
 -- ES 5.4.0  start ..                   -- 
 ------------------------------------------- 
 Starting elasticsearch  ...   
/etc/profile: line 82: ulimit: max user processes: cannot modify limit: Operation not permitted
 The elasticsearch service is started: http://192.168.83.240:9233 
 Starting header  ...   
 The elasticsearch header is started: http://192.168.83.240:9133 
 Starting esql  ...   
 The elasticsearch sql is started: http://192.168.83.240:9430 
 Starting kibana  ...   
 The kibana service is started: http://192.168.83.240:5631 
 ------------------------------------------- 
 elasticsearch pid:951 
 header pid:1058 
 esql pid:1127 
 kibana pid: 
 =========================================== 
[root@sis01 es-5.4.0]#
```

### 查看状态

```text
[root@sis01 es-5.4.0]# ./es.sh status
 =========================================== 
 -- ES 5.4.0  status ..                  -- 
 ------------------------------------------- 
 the es     pid:951     endpoint:    http://192.168.83.240:9233 
 the header     pid:1058     endpoint:    http://192.168.83.240:9133 
 the esql     pid:1127     endpoint:    http://192.168.83.240:9430 
 the kibana     pid:1172     endpoint:    http://192.168.83.240:5631 
 =========================================== 
[root@sis01 es-5.4.0]#
```

### ES验证

访问网址：[http://192.168.83.240:9233/可以看到如下返回：](http://192.168.83.240:9233/可以看到如下返回：)

```text
{
    "name": "node-01",
    "cluster_name": "elastic-x",
    "cluster_uuid": "iBiLKfVaT9mbv7045d9QtA",
    "version": {
        "number": "5.4.0",
        "build_hash": "780f8c4",
        "build_date": "2017-04-28T17:43:27.229Z",
        "build_snapshot": false,
        "lucene_version": "6.5.0"
    },
    "tagline": "You Know, for Search"
}
```

```text

```

