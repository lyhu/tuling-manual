# 语义解析服务-NLU

## 解压压缩包

```
[root@sis01 lib]# cd /iflytek/engine/lib/
[root@sis01 lib]# ls
ast_chin  log  ssm  tools  xtts10
[root@sis01 lib]# ls
ast_chin  log  nlu_3.5.0.1195.zip  ssm  tools  xtts10
[root@sis01 lib]# unzip nlu_3.5.0.1195.zip 
Archive:  nlu_3.5.0.1195.zip
   creating: nlu/
 extracting: nlu/version.txt         
   creating: nlu/bin/
  inflating: nlu/bin/nli_2018-05-18_11-42-28.log 
  ..................................................
  ..................................................
  inflating: nlu/res_pack_zh_cn_mandarin_1/ifly_nli.irf  
  inflating: nlu/res_pack_zh_cn_mandarin_1/ifly_build.irf  
[root@sis01 lib]# 
[root@sis01 lib]# rm -rf nlu_3.5.0.1195.zip 
[root@sis01 lib]# ls
ast_chin  log  nlu  ssm  tools  xtts10
[root@sis01 lib]# cd nlu
[root@sis01 nlu]# ls
bin  res_pack_en_us  res_pack_zh_cn_cantonese  res_pack_zh_cn_mandarin  res_pack_zh_cn_mandarin_1  version.txt
[root@sis01 nlu]# rm -rf res_pack_zh_cn_mandarin_1/
[root@sis01 nlu]# ls
bin  res_pack_en_us  res_pack_zh_cn_cantonese  res_pack_zh_cn_mandarin  version.txt
[root@sis01 nlu]# 
```

## 替换资源

替换语义资源到res_pack_zh_cn_mandarin目录

```
[root@sis01 nlu]# cd /iflytek/engine/lib/nlu/res_pack_zh_cn_mandarin/
[root@sis01 res_pack_zh_cn_mandarin]# ls
ifly_build.irf  ifly_nli.irf
[root@sis01 res_pack_zh_cn_mandarin]# 

```

## 放置tuling-ssm组件

```
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-ssm  tuling-tts  xmanager
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-nlu-build-2.0.0-SNAPSHOT-Build_1005.zip  tuling-ssm  tuling-tts  xmanager
[root@sis01 plugin]# unzip tuling-nlu-build-2.0.0-SNAPSHOT-Build_1005.zip 
Archive:  tuling-nlu-build-2.0.0-SNAPSHOT-Build_1005.zip
   creating: tuling-nlu-build-2.0.0-SNAPSHOT/
  inflating: tuling-nlu-build-2.0.0-SNAPSHOT/doc/tuling-nlu.sql  
  inflating: tuling-nlu-build-2.0.0-SNAPSHOT/conf/tuling-nlu.sh  
 ............................................................. 
 ............................................................. 
 extracting: tuling-nlu-build-2.0.0-SNAPSHOT/lib/commons-httpclient-3.1.jar  
[root@sis01 plugin]# 
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-nlu-build-2.0.0-SNAPSHOT  tuling-nlu-build-2.0.0-SNAPSHOT-Build_1005.zip  tuling-ssm  tuling-tts  xmanager
[root@sis01 plugin]# rm -rf tuling-nlu-build-2.0.0-SNAPSHOT-Build_1005.zip 
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-nlu-build-2.0.0-SNAPSHOT  tuling-ssm  tuling-tts  xmanager
[root@sis01 plugin]# mv tuling-nlu-build-2.0.0-SNAPSHOT/ tuling-nlu
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-nlu  tuling-ssm  tuling-tts  xmanager
[root@sis01 plugin]#

```

## 赋予执行权限

```
[root@sis01 conf]# cd /iflytek/server/skynet/plugin/tuling-nlu/conf
[root@sis01 conf]# ls
tuling-nlu.sh
[root@sis01 conf]# chmod +x *.sh
[root@sis01 conf]# ls
tuling-nlu.sh
[root@sis01 conf]# 
```

## 新建数据库

创建数据库tuling-nlu，找到初始化脚本tuling-nlu.sql

```
[root@sis01 doc]# cd /iflytek/server/skynet/plugin/tuling-nlu/doc
[root@sis01 doc]# ls
tuling-nlu.sql  tuling-nlu.zk.config
[root@sis01 doc]# 
```

## 导入zk配置

```
[root@sis01 doc]# cd /iflytek/server/skynet/plugin/tuling-nlu/doc 
[root@sis01 doc]# ls
tuling-nlu.sql  tuling-nlu.zk.config
[root@sis01 doc]# 
```

tuling-ssm.zk.config里面的文件内容细节如下

```
/skynet/plugin/tuling-nlu/setting=_jdbc_config={\n    "url": "jdbc:mysql://{$ref@setting:mysql_url}/tuling-nlu?characterEncoding=utf8&useSSL=false",\n    "driver": "com.mysql.jdbc.Driver",\n    "userName": "{$ref@setting:mysql_xuser}",\n    "password": "{$ref@setting:mysql_ypwd}",\n    "InitialSize": "50",\n    "MaxActive": "16300",\n    "MaxIdle": "16300",\n    "MinIdle": "1000",\n    "MaxWait": "10000",\n    "pageSize": "100"\n}
/skynet/plugin/tuling-nlu/setting=mysql_url=192.168.83.240:3306
/skynet/plugin/tuling-nlu/setting=mysql_xuser=root
/skynet/plugin/tuling-nlu/setting=mysql_ypwd=iflytek
```

通过zkui工具导入配置文件。

> 数据库连接jdbc:mysql://{$ref@setting:mysql_url}/tuling-nlu?characterEncoding=utf8&useSSL=false

## 分配服务

通过xmanager管理界面分配服务，点击操作按钮，点击服务分配，系统下来框选择能力平台-语义解析2.0，选择  [NLU]语义解析服务[Rest]“+”按钮。然后点击确定按钮即可。

## 查看日志

选择服务，右击查看日志，即可以看到对应的日志文件。 
