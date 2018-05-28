# 统一文件存储USP部署

## Fastdfs安装部署

### 上传安装包

上传fastdfs安装包FastDFS_v5.08.tar.gz和libfastcommon-master.zip

```
[root@sis01 doc]# cd /iflytek/soft/
[root@sis01 soft]# ls
[root@sis01 soft]# ls
FastDFS_v5.08.tar.gz  libfastcommon-master.zip
[root@sis01 soft]# 
```

###  解压压缩包

```
[root@sis01 soft]# tar -zxvf FastDFS_v5.08.tar.gz
[root@sis01 soft]# unzip libfastcommon-master.zip
01 soft]# ls
FastDFS  FastDFS_v5.08.tar.gz  libfastcommon-master  libfastcommon-master.zip
[root@sis01 soft]# rm -rf FastDFS_v5.08.tar.gz
[root@sis01 soft]# rm -rf libfastcommon-master.zip 
[root@sis01 soft]# ls
FastDFS  libfastcommon-master
[root@sis01 soft]# 
```

### 安装libfastcommon-master

```
[root@sis01 soft]# cd libfastcommon-master/
[root@sis01 libfastcommon-master]# ls
HISTORY  INSTALL  libfastcommon.spec  make.sh  php-fastcommon  README  src
[root@sis01 libfastcommon-master]# ./make.sh 
[root@sis01 libfastcommon-master]# ./make.sh install
```

### 安装FastDFS

```
[root@sis01 soft]# cd FastDFS/
[root@sis01 FastDFS]# ls
client  conf             fastdfs.spec  init.d   make.sh     README.md   stop.sh  test
common  COPYING-3_0.txt  HISTORY       INSTALL  php_client  restart.sh  storage  tracker
[root@sis01 FastDFS]# ./make.sh 
[root@sis01 FastDFS]# ./make.sh  install
```

### 创建存储目录

```
[root@sis01 iflytek]# mkdir -p /iflytek/data/fdfs/tracker
[root@sis01 iflytek]# mkdir -p /iflytek/data/fdfs/storage
[root@sis01 iflytek]# mkdir -p /iflytek/data/fdfs/client
[root@sis01 iflytek]# mkdir -p /iflytek/data/fdfs/nginx-module
[root@sis01 iflytek]# cd /iflytek/data/
[root@sis01 data]# ls
fdfs
[root@sis01 data]# cd fdfs/
[root@sis01 fdfs]# ls
storage  tracker  client nginx-module
[root@sis01 fdfs]# 
```

### 配置tracker

```
[root@sis01 FastDFS]# cd /etc/fdfs/
[root@sis01 fdfs]# ls
client.conf.sample  storage.conf.sample  tracker.conf.sample
[root@sis01 fdfs]# cp tracker.conf.sample  tracker.conf
[root@sis01 fdfs]# ls
client.conf.sample  storage.conf.sample  tracker.conf  tracker.conf.sample
[root@sis01 fdfs]# vim tracker.conf
```

配置配置文件

```
base_path=/iflytek/data/fdfs/tracker
max_connections=4000
reserved_storage_space = 8%
```

### 启动tracker

```
[root@sis01 fdfs]# /usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf start
[root@sis01 fdfs]# 
```

验证tracker

```
[root@sis01 fdfs]# netstat -ltnp | grep 22122
tcp        0      0 0.0.0.0:22122           0.0.0.0:*               LISTEN      29786/fdfs_trackerd 
[root@sis01 fdfs]#
```


### 配置storage

```
[root@sis01 FastDFS]# cd /etc/fdfs/
[root@sis01 fdfs]# ls
client.conf.sample  storage.conf.sample  tracker.conf  tracker.conf.sample
[root@sis01 fdfs]# cp storage.conf.sample  storage.conf
[root@sis01 fdfs]# ls
client.conf.sample  storage.conf  storage.conf.sample  tracker.conf  tracker.conf.sample
[root@sis01 fdfs]#
[root@sis01 fdfs]# vim storage.conf
```

配置配置文件

```
base_path=/iflytek/data/fdfs/storage
max_connections=4000
store_path0=/iflytek/data/fdfs
tracker_server=192.168.83.240:22122
http.server_port=9092
```

### 启动storage

```
[root@sis01 fdfs]# /usr/bin/fdfs_storaged /etc/fdfs/storage.conf start
[root@sis01 fdfs]# 
```

### 验证storage

```
[root@sis01 fdfs]# netstat -lntp | grep 23000
tcp        0      0 0.0.0.0:23000           0.0.0.0:*               LISTEN      29932/fdfs_storaged 
[root@sis01 fdfs]# 

```

### 验证文件上传

配置client验证文件能否正常上传

```
[root@sis01 fdfs]# cd /etc/fdfs/
[root@sis01 fdfs]# ls
client.conf.sample  storage.conf  storage.conf.sample  tracker.conf  tracker.conf.sample
[root@sis01 fdfs]# cp client.conf.sample  client.conf
[root@sis01 fdfs]# ls
client.conf  client.conf.sample  storage.conf  storage.conf.sample  tracker.conf  tracker.conf.sample
[root@sis01 fdfs]# vim client.conf
```

编辑配置文件

```
base_path=/iflytek/data/fdfs/client
tracker_server=192.168.83.240:22122
#include http.conf
```

从源码包中拷贝http.conf到/etc/fdfs/目录

```
[root@sis01 fdfs]# cd /etc/fdfs/
[root@sis01 fdfs]# ls
client.conf  client.conf.sample  storage.conf  storage.conf.sample  tracker.conf  tracker.conf.sample
[root@sis01 fdfs]# cp /iflytek/soft/FastDFS/conf/http.conf  /etc/fdfs/
[root@sis01 fdfs]# ls
client.conf  client.conf.sample  http.conf  storage.conf  storage.conf.sample  tracker.conf  tracker.conf.sample
[root@sis01 fdfs]# 
```

编辑http.conf文件

```
http.anti_steal.token_check_fail=/etc/fdfs/anti-steal.jpg
```

拷贝anti-steal.jpg图片

```
[root@sis01 fdfs]# cp /iflytek/soft/FastDFS/conf/anti-steal.jpg  /etc/fdfs/
[root@sis01 fdfs]# ls
anti-steal.jpg  client.conf  client.conf.sample  http.conf  storage.conf  storage.conf.sample  tracker.conf  tracker.conf.sample
[root@sis01 fdfs]#
```

拷贝mime.types

```
[root@sis01 fdfs]# cp /iflytek/soft/FastDFS/conf/mime.types  /etc/fdfs/
[root@sis01 fdfs]# ls
anti-steal.jpg  client.conf  client.conf.sample  http.conf  mime.types  storage.conf  storage.conf.sample  tracker.conf  tracker.conf.sample
[root@sis01 fdfs]# 
```

验证图片上传

```
[root@sis01 fdfs]# /usr/bin/fdfs_test /etc/fdfs/client.conf upload anti-steal.jpg 
This is FastDFS client test program v5.08

Copyright (C) 2008, Happy Fish / YuQing

FastDFS may be copied only under the terms of the GNU General
Public License V3, which may be found in the FastDFS source kit.
Please visit the FastDFS Home Page http://www.csource.org/ 
for more detail.

[2018-05-28 21:28:13] DEBUG - base_path=/iflytek/data/fdfs/client, connect_timeout=30, network_timeout=60, tracker_server_count=1, anti_steal_token=0, anti_steal_secret_key length=0, use_connection_pool=0, g_connection_pool_max_idle_time=3600s, use_storage_id=0, storage server id count: 0

tracker_query_storage_store_list_without_group: 
	server 1. group_name=, ip_addr=192.168.83.240, port=23000

group_name=group1, ip_addr=192.168.83.240, port=23000
storage_upload_by_filename
group_name=group1, remote_filename=M00/00/00/wKhT8FsMA-2APyl-AABdrZgsqUU135.jpg
source ip address: 192.168.83.240
file timestamp=2018-05-28 21:28:13
file size=23981
file crc32=2553063749
example file url: http://192.168.83.240/group1/M00/00/00/wKhT8FsMA-2APyl-AABdrZgsqUU135.jpg
storage_upload_slave_by_filename
group_name=group1, remote_filename=M00/00/00/wKhT8FsMA-2APyl-AABdrZgsqUU135_big.jpg
source ip address: 192.168.83.240
file timestamp=2018-05-28 21:28:14
file size=23981
file crc32=2553063749
example file url: http://192.168.83.240/group1/M00/00/00/wKhT8FsMA-2APyl-AABdrZgsqUU135_big.jpg
[root@sis01 fdfs]#
```

此时编辑client.conf文件

```
http.tracker_server_port=9092
```

再次执行文件上传操作

```
[root@sis01 fdfs]# /usr/bin/fdfs_test /etc/fdfs/client.conf upload anti-steal.jpg 
This is FastDFS client test program v5.08

Copyright (C) 2008, Happy Fish / YuQing

FastDFS may be copied only under the terms of the GNU General
Public License V3, which may be found in the FastDFS source kit.
Please visit the FastDFS Home Page http://www.csource.org/ 
for more detail.

[2018-05-28 21:31:09] DEBUG - base_path=/iflytek/data/fdfs/client, connect_timeout=30, network_timeout=60, tracker_server_count=1, anti_steal_token=0, anti_steal_secret_key length=0, use_connection_pool=0, g_connection_pool_max_idle_time=3600s, use_storage_id=0, storage server id count: 0

tracker_query_storage_store_list_without_group: 
	server 1. group_name=, ip_addr=192.168.83.240, port=23000

group_name=group1, ip_addr=192.168.83.240, port=23000
storage_upload_by_filename
group_name=group1, remote_filename=M00/00/00/wKhT8FsMBJ2ALV8SAABdrZgsqUU734.jpg
source ip address: 192.168.83.240
file timestamp=2018-05-28 21:31:09
file size=23981
file crc32=2553063749
example file url: http://192.168.83.240:9092/group1/M00/00/00/wKhT8FsMBJ2ALV8SAABdrZgsqUU734.jpg
storage_upload_slave_by_filename
group_name=group1, remote_filename=M00/00/00/wKhT8FsMBJ2ALV8SAABdrZgsqUU734_big.jpg
source ip address: 192.168.83.240
file timestamp=2018-05-28 21:31:09
file size=23981
file crc32=2553063749
example file url: http://192.168.83.240:9092/group1/M00/00/00/wKhT8FsMBJ2ALV8SAABdrZgsqUU734_big.jpg
[root@sis01 fdfs]# 
```

### 安装Nginx和模块

上传nginx-1.10.3.tar.gz和fastdfs-nginx-module_v1.16.tar.gz

```
[root@sis01 soft]# ls
FastDFS  libfastcommon-master
[root@sis01 soft]# 
[root@sis01 soft]# clear
[root@sis01 soft]# ls
FastDFS  fastdfs-nginx-module_v1.16.tar.gz  libfastcommon-master  nginx-1.10.3.tar.gz
[root@sis01 soft]# 
```

解压文件

```
[root@sis01 soft]# ls
FastDFS  fastdfs-nginx-module_v1.16.tar.gz  libfastcommon-master  nginx-1.10.3.tar.gz
[root@sis01 soft]# tar -zxvf fastdfs-nginx-module_v1.16.tar.gz 
[root@sis01 soft]# tar -zxvf nginx-1.10.3.tar.gz
[root@sis01 soft]# rm -rf fastdfs-nginx-module_v1.16.tar.gz 
[root@sis01 soft]# rm -rf nginx-1.10.3.tar.gz 
[root@sis01 soft]# ls
FastDFS  fastdfs-nginx-module  libfastcommon-master  nginx-1.10.3
[root@sis01 soft]# 
```

安装基础编译和依赖包

```
[root@sis01 soft]# yum install -y gcc-c++
[root@sis01 soft]# yum install -y pcre pcre-devel
[root@sis01 soft]# yum install -y zlib zlib-devel
[root@sis01 soft]# yum install -y openssl openssl-devel
```

调整配置项

```
[root@sis01 soft]# cd fastdfs-nginx-module/
[root@sis01 fastdfs-nginx-module]# cd src/
[root@sis01 src]# ls
common.c  common.h  config  mod_fastdfs.conf  ngx_http_fastdfs_module.c
[root@sis01 src]# vim config 
[root@sis01 src]# 
```

>`修改``CORE_INCS``为下面配置，去除``“local”``，这个是``fastdfs``的安装库位置`

config默认值为

```
CORE_INCS="$CORE_INCS /usr/local/include/fastdfs /usr/local/include/fastcommon/"
```

调整后值为

```
CORE_INCS="$CORE_INCS /usr/include/fastdfs /usr/include/fastcommon/"
```

编译安装Nginx

```
[root@sis01 nginx-1.10.3]# ./configure --prefix=/iflytek/server/nginx-fdfs  --add-module=/iflytek/soft/fastdfs-nginx-module/src
[root@sis01 nginx-1.10.3]# make && make install
```

调整nginx配置文件

```
[root@sis01 nginx-1.10.3]# cd /iflytek/server/nginx-fdfs/
[root@sis01 nginx-fdfs]# ls
conf  html  logs  sbin
[root@sis01 nginx-fdfs]# cd conf/
[root@sis01 conf]# ls
fastcgi.conf          fastcgi_params          koi-utf  mime.types          nginx.conf          scgi_params          uwsgi_params          win-utf
fastcgi.conf.default  fastcgi_params.default  koi-win  mime.types.default  nginx.conf.default  scgi_params.default  uwsgi_params.default
[root@sis01 conf]# vim nginx.conf
```

编辑配置

```
listen       9092;
```

追加内容如下

```
location /group1/M00 {
        root /iflytek/data/fdfs/data;
        ngx_fastdfs_module;
}
```

> 此时storage配置文件storage.conf中的配置项为store_path0=/iflytek/data/fdfs，而nginx这里配置的是/iflytek/data/fdfs/data此时需要特殊注意。
>
> 

### 启动Nginx

```
[root@sis01 nginx-fdfs]# cd sbin/
[root@sis01 sbin]# ls
nginx
[root@sis01 sbin]# ./nginx  
ngx_http_fastdfs_set pid=33388
[root@sis01 sbin]# 
```

### 配置mod_fastdfs.conf文件

````
[root@sis01 src]# cd /iflytek/soft/fastdfs-nginx-module/src
[root@sis01 src]# ls
common.c  common.h  config  mod_fastdfs.conf  ngx_http_fastdfs_module.c
[root@sis01 src]# cp mod_fastdfs.conf /etc/fdfs/
[root@sis01 src]# cd /etc/fdfs/
[root@sis01 fdfs]# ls
anti-steal.jpg  client.conf  client.conf.sample  http.conf  mime.types  mod_fastdfs.conf  storage.conf  storage.conf.sample  tracker.conf  tracker.conf.sample
[root@sis01 fdfs]# 
[root@sis01 fdfs]# vim mod_fastdfs.conf
````

编辑文件内容

```
base_path=/iflytek/data/fdfs/nginx-module
tracker_server=192.168.83.240:22122
group_name=group1
url_have_group_name = true
store_path0=/iflytek/data/fdfs
```

> 这里的store_path0和storage.conf中的store_path0相同

### 重启fastdfs和nginx

```
[root@sis01 fdfs]# /usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf restart
[root@sis01 fdfs]# /usr/bin/fdfs_storaged /etc/fdfs/storage.conf restart
[root@sis01 fdfs]# /iflytek/server/nginx-fdfs/sbin/nginx  -s reload	
```

### 下载文件

通过浏览器访问地址http://192.168.83.240:9092/group1/M00/00/00/wKhT8FsMBJ2ALV8SAABdrZgsqUU734_big.jpg，此时发现可以正常查看图片，则表示fastdfs及其nginx模块部署成功。

## usp组件部署

### 放置插件包

```
[root@sis01 plugin]# cd /iflytek/server/skynet/plugin/
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-nlu  tuling-ssm  tuling-tts  xmanager
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-nlu  tuling-ssm  tuling-tts  tuling-usp-build-2.0.0-SNAPSHOT-Build_1003.zip  xmanager
[root@sis01 plugin]# 
```

### 解压插件包

```
[root@sis01 plugin]# unzip tuling-usp-build-2.0.0-SNAPSHOT-Build_1003.zip 
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-nlu  tuling-ssm  tuling-tts  tuling-usp-build-2.0.0-SNAPSHOT  tuling-usp-build-2.0.0-SNAPSHOT-Build_1003.zip  xmanager
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-nlu  tuling-ssm  tuling-tts  tuling-usp-build-2.0.0-SNAPSHOT  tuling-usp-build-2.0.0-SNAPSHOT-Build_1003.zip  xmanager
[root@sis01 plugin]# rm -rf tuling-usp-build-2.0.0-SNAPSHOT-Build_1003.zip 
[root@sis01 plugin]# mv tuling-usp-build-2.0.0-SNAPSHOT/ tuling-usp
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-nlu  tuling-ssm  tuling-tts  tuling-usp  xmanager
[root@sis01 plugin]# 
```

导入zk配置

```
[root@sis01 tuling-usp]# cd /iflytek/server/skynet/plugin/tuling-usp/
[root@sis01 tuling-usp]# ls
conf  doc  lib
[root@sis01 tuling-usp]# cd doc/
[root@sis01 doc]# ls
RELEASES.md  tuling-usp.zk.config
[root@sis01 doc]#
```

编辑配置文件

```
"nginxUrl": "http://127.0.0.1:8090",\n    "trackers": "127.0.0.1:22122"
```

调整为

```
"nginxUrl": "http://192.168.83.240:9092",\n    "trackers": "192.168.83.240:22122"
```

调整完毕后通过zkui导入相关配置。

### 分配服务

通过xmanager管理界面分配服务，点击操作按钮，点击服务分配，系统下来框选择能力平台-分布式文件存储系统2.0，选择  [USP]文件存储服务右侧的“+”按钮。然后点击确定按钮即可。

### 查看日志

选择服务，右击查看日志，即可以看到对应的日志文件。 


