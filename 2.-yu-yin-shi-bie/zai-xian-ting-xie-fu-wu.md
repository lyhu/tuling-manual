# 在线听写服务

## 上传转写引擎

```bash
[root@sis01 lib]# cd /iflytek/engine/lib/
[root@sis01 lib]# ls
nlu  ssm  tools  xtts10
[root@sis01 lib]# mkdir ast
[root@sis01 lib]# ls
ast  nlu  ssm  tools  xtts10
[root@sis01 lib]# cd ast/
[root@sis01 ast]# ls
[root@sis01 ast]#
```

此时上传引擎资源到/iflytek/engine/lib/ast目录

```bash
[root@sis01 ast]# cd /iflytek/engine/lib/ast/
[root@sis01 ast]# ls
ast_chin  ast_tibe  ast_uyhu  conf
[root@sis01 ast]#
```

## 上传转写资源

创建目录/iflytek/engine/res

```bash
[root@sis01 res]# mkdir -p  /iflytek/engine/res/ast
[root@sis01 res]# cd /iflytek/engine/res/ast
[root@sis01 res]#
```

上传资源文件

```bash
[root@sis01 res]# cd /iflytek/engine/res/ast
[root@iengine-dev ast]# ls
ast_20170417_chin  ast_20180103_uyhu  ast_20180105_chin  ast_20180319_tibe  ast_20180410_chin  
[root@iengine-dev ast]#
```

## 插件包安装

拷贝ast插件到skynet的plugin目录

```bash
[root@sis01 res]# cd /iflytek/server/skynet/plugin/
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-nlu  tuling-ssm  tuling-tts  tuling-usp  xmanager
[root@sis01 plugin]#
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-ast-cloud-2.0.0-SNAPSHOT-Build_1007.zip  tuling-nlu  tuling-ssm  tuling-tts  tuling-usp  xmanager
[root@sis01 plugin]# unzip tuling-ast-cloud-2.0.0-SNAPSHOT-Build_1007.zip 
[root@sis01 plugin]# rm -rf tuling-ast-cloud-2.0.0-SNAPSHOT-Build_1007.zip 
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-ast-cloud-2.0.0-SNAPSHOT  tuling-nlu  tuling-ssm  tuling-tts  tuling-usp  xmanager
[root@sis01 plugin]# mv tuling-ast-cloud-2.0.0-SNAPSHOT/ tuling-ast
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-ast  tuling-nlu  tuling-ssm  tuling-tts  tuling-usp  xmanager
[root@sis01 plugin]#
```

## 赋予可执行权限

```bash
[root@sis01 plugin]# cd /iflytek/server/skynet/plugin/
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-ast  tuling-nlu  tuling-ssm  tuling-tts  tuling-usp  xmanager
[root@sis01 plugin]# cd tuling-ast/
[root@sis01 plugin]# cd tuling-ast/
[root@sis01 tuling-ast]# ls
conf  doc  lib
[root@sis01 tuling-ast]# cd conf/
[root@sis01 conf]# ls
patch_package.cfg  tuling-ast.sh
[root@sis01 conf]# chmod +x *.sh
```

## 新建数据库

```bash
[root@sis01 conf]# cd /iflytek/server/skynet/plugin/tuling-ast/doc 
[root@sis01 doc]# ls
RELEASES.md  tuling-ast.sql  tuling-ast.zk.config  tuling-ast.zk.config.bak
```

## 导入zk配置

找到zk配置文件tuling-ast.zk.config

```bash
[root@sis01 conf]# cd /iflytek/server/skynet/plugin/tuling-ast/doc 
[root@sis01 doc]# ls
RELEASES.md  tuling-ast.sql  tuling-ast.zk.config  tuling-ast.zk.config.bak
```

调整部分如下

```text
/skynet/plugin/tuling-ast/setting=mysql_url=jdbc:mysql://127.0.0.1:3306/tuling-ast?characterEncoding=utf8&useSSL=false
/skynet/plugin/tuling-ast/setting=mysql_xuser=root
/skynet/plugin/tuling-ast/setting=mysql_ypwd=iflytek
```

tts.zk.config里面的文件内容127.0.0.1替换为192.168.83.240，通过zkui工具导入配置文件。

## 分配服务

通过xmanager管理界面分配服务，点击操作按钮，点击服务分配，系统下来框选择能力平台-在线听写2.0，选择 右侧的\[AST\]在线听写会话服务\[中文\]、\[AST\]在线听写会话服务\[维语\]、\[AST\]在线听写会话服务\[藏语\]、\[AST-PATCH\]资源管理服务、 \[AST-ROUTE\]会话路由服务“+”按钮。然后点击确定按钮即可。

## 查看日志

选择服务，右击查看日志，即可以看到对应的日志文件。

## 小包打包支持

上传pack文件夹至/iflytek/engine/lib目录

```bash
[root@sis01 data]# cd /iflytek/engine/lib/
[root@sis01 lib]# ls
ast  nlu  ssm  tools  xtts10
[root@sis01 lib]# 
[root@sis01 lib]# ls
ast  nlu  pack ssm  tools  xtts10
[root@sis01 lib]#
```

拷贝文件patch\_package.cfg至/iflytek/server/skynet/bin目录

```bash
[root@sis01 conf]# cd /iflytek/server/skynet/plugin/tuling-ast/conf
[root@sis01 conf]# ls
patch_package.cfg  tuling-ast.sh
[root@sis01 conf]# cp /iflytek/server/skynet/plugin/tuling-ast/conf/patch_package.cfg  /iflytek/server/skynet/bin/
[root@sis01 conf]# cd /iflytek/server/skynet/bin/
[root@sis01 bin]# ls
ant-env.cmd     ant-server.sh   ant-worker.sh     ant-xmanager.sh  freeSSHd.exe  log                readme.txt
ant-server.cmd  ant-worker.cmd  ant-xmanager.cmd  deamon.sh        iflysoft.log  patch_package.cfg
[root@sis01 bin]#
```

> 此项文件不拷贝会导致小包功能无法使用

## 分配服务

通过xmanager管理界面分配服务，点击操作按钮，点击服务分配，系统下来框选择能力平台-在线听写2.0，选择 右侧的\[AST-PACK\]资源打包服务右侧的“+”按钮。然后点击确定按钮即可。

## 查看日志

选择服务，右击查看日志，即可以看到对应的日志文件。

