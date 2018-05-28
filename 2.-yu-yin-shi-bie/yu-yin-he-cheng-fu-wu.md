# 语音合成服务


## 解压压缩包

```
[root@sis01 lib]# cd /iflytek/engine/lib/
[root@sis01 lib]# ls
ast_chin  log  ssm
[root@sis01 lib]# ls
ast_chin  log  ssm  xtts10-192.168.65.3-2018-05-25.tar.gz
[root@sis01 lib]# 
[root@sis01 lib]# tar -zxvf xtts10-192.168.65.3-2018-05-25.tar.gz 
xtts10/
xtts10/bin/
xtts10/bin/libaucodec.so
xtts10/bin/libboost_date_time.so.1.54.0
.......................................................
.......................................................
.......................................................
xtts10/dongle/Sentinel_HASP_Linux_Run-time_Installer_script_v1.15/haspvlib_85015.so
xtts10/dongle/Sentinel_HASP_Linux_Run-time_Installer_script_v1.15/aksusbd
[root@sis01 lib]# ls
ast_chin  log  ssm  xtts10  xtts10-192.168.65.3-2018-05-25.tar.gz
[root@sis01 lib]# rm -rf xtts10-192.168.65.3-2018-05-25.tar.gz 
[root@sis01 lib]# ls
ast_chin  log  ssm  xtts10
[root@sis01 lib]# 
```

## 引擎授权申请

TTS授权需要向公司杨蕾leiyang6申请，申请的信息格式如下

```
语音合成 
授权类型：试用软授权
授权期限：180天
授权产品： xtts10
产品版本：1.0.0
授权路数：30路
授权资源包：xiaoyan，xiaofeng
```

## 引擎授权

引擎包解压后需要进行授权操作。

安装依赖环境(加密狗)

```
[root@sis01 yum-repo]# yum install -y  xulrunner.i686
```

执行安装操作

```
[root@sis01 yum-repo]# cd /iflytek/engine/lib/xtts10/dongle/Sentinel_HASP_Linux_Run-time_Installer_script_v1.15
[root@sis01 yum-repo]# 
[root@sis01 yum-repo]# ls
aksusbd  aksusbd.rc  default.v2c  dinst  dunst  hasplmd  haspman  hasp.rules  hasp_update  hasp_update.update  haspvlib_85015.so  winehasp  xtts10.v2c
[root@sis01 yum-repo]#
[root@sis01 Sentinel_HASP_Linux_Run-time_Installer_script_v1.15]# ./dinst 
Copy AKSUSB daemon to /usr/sbin ...
Copy WINEHASP daemon to /usr/sbin ...
Copy HASPLMD daemon to /usr/sbin ...
Copy start-up script to /etc/init.d ...
Link HASP SRM runtime environment startup script to system startup folder
Starting HASP SRM runtime environment ... 
Starting aksusbd (via systemctl):  Warning: aksusbd.service changed on disk. Run 'systemctl daemon-reload' to reload units.
                                                           [  OK  ]
```

授权导入

```
[root@sis01 Sentinel_HASP_Linux_Run-time_Installer_script_v1.15]# ./hasp_update u xtts10.v2c 
press ENTER
[root@sis01 Sentinel_HASP_Linux_Run-time_Installer_script_v1.15]# 

```

检查授权

```
[root@sis01 Sentinel_HASP_Linux_Run-time_Installer_script_v1.15]# ./haspman 
iFly HaspMan 4.0.0.1002 Copyright 1999-2005 by iFly Info.Tek. All rights reserved.

Dongle ID                 		:	2147483647
------------------------------------------------------------------------
Dongle Proudcer           		:	Aladdin
Dongle Eletric Type       		:	SL
Memory Size               		:	4032(Bytes)
Real-Clock                		:	noneDongle Protection Format  		:	Format used by Intp40
Product counts            		:	1
********************
Intp40 Product No.1:
	Product Name              	:	xtts10
	Product Number            	:	
	Product Version           	:	1.0.0
	Product Licence           	:	30
	Product Expired Time Disabled.	Product Run Count Disabled.
	Product Run Time Disabled.
	Support Voices            	:	*
	Support Packages          	:	xiaofeng;xiaoyan

Total 1 dongles found.

[root@sis01 Sentinel_HASP_Linux_Run-time_Installer_script_v1.15]# 
```

## ffmpeg工具安装

安装ffmpeg工具，将tools.zip文件拷贝到/iflytek/engine/lib，进行解压

```
[root@sis01 lib]# ls
ast_chin  log  ssm  xtts10
[root@sis01 lib]# ls
ast_chin  log  ssm  tools.zip  xtts10
[root@sis01 lib]# unzip tools.zip 
Archive:  tools.zip
  inflating: tools/ffmpeg            
[root@sis01 lib]# ls
ast_chin  log  ssm  tools  tools.zip  xtts10
[root@sis01 lib]# rm -rf tools.zip 
[root@sis01 lib]# ls
ast_chin  log  ssm  tools  xtts10
[root@sis01 lib]# cd tools/
[root@sis01 lib]# chmod 755 ffmpeg
[root@sis01 tools]# ls
ffmpeg
[root@sis01 tools]#
[root@sis01 lib]# cd /iflytek/engine/lib/
[root@sis01 lib]# ls
ast_chin  log  ssm  tools  xtts10
[root@sis01 lib]# 
```

## 插件包安装

拷贝tts插件到skynet的plugin目录

```
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-ssm  xmanager
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-ssm  tuling-tts-build-2.0.0-SNAPSHOT-Build_1001.zip  xmanager
[root@sis01 plugin]# unzip tuling-tts-build-2.0.0-SNAPSHOT-Build_1001.zip 
Archive:  tuling-tts-build-2.0.0-SNAPSHOT-Build_1001.zip
   creating: tuling-tts-build-2.0.0-SNAPSHOT/
  inflating: tuling-tts-build-2.0.0-SNAPSHOT/conf/tuling.tts.sh  
 extracting: tuling-tts-build-2.0.0-SNAPSHOT/lib/tuling-tts-service-2.0.0-SNAPSHOT.jar  
 extracting: tuling-tts-build-2.0.0-SNAPSHOT/lib/tuling-tts-engine-2.0.0-SNAPSHOT.jar  
 extracting: tuling-tts-build-2.0.0-SNAPSHOT/lib/tuling-common-2.0.0-SNAPSHOT.jar  
 extracting: tuling-tts-build-2.0.0-SNAPSHOT/lib/okio-1.13.0.jar  
 extracting: tuling-tts-build-2.0.0-SNAPSHOT/lib/plexus-utils-3.0.24.jar  
 extracting: tuling-tts-build-2.0.0-SNAPSHOT/lib/jasypt-1.9.0.jar  
 extracting: tuling-tts-build-2.0.0-SNAPSHOT/lib/commons-io-2.4.jar  
 extracting: tuling-tts-build-2.0.0-SNAPSHOT/lib/android-json-0.0.20131108.vaadin1.jar  
 extracting: tuling-tts-build-2.0.0-SNAPSHOT/lib/spring-boot-configuration-processor-1.5.9.RELEASE.jar  
   creating: tuling-tts-build-2.0.0-SNAPSHOT/doc/
 extracting: tuling-tts-build-2.0.0-SNAPSHOT/lib/okhttp-3.8.1.jar  
  inflating: tuling-tts-build-2.0.0-SNAPSHOT/doc/tts.zk.config  
 extracting: tuling-tts-build-2.0.0-SNAPSHOT/lib/jcifs-1.3.17.jar  
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-ssm  tuling-tts-build-2.0.0-SNAPSHOT  tuling-tts-build-2.0.0-SNAPSHOT-Build_1001.zip  xmanager
[root@sis01 plugin]# rm -rf tuling-tts-build-2.0.0-SNAPSHOT-Build_1001.zip 
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-ssm  tuling-tts-build-2.0.0-SNAPSHOT  xmanager
[root@sis01 plugin]# 
[root@sis01 plugin]# mv tuling-tts-build-2.0.0-SNAPSHOT/ tuling-tts
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-ssm  tuling-tts  xmanager
[root@sis01 plugin]# 
```

## 赋予可执行权限

```
[root@sis01 plugin]# cd /iflytek/server/skynet/plugin/
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-ssm  tuling-tts  xmanager
[root@sis01 plugin]# cd tuling-tts/
[root@sis01 tuling-tts]# ls
conf  doc  lib
[root@sis01 tuling-tts]# cd conf/
[root@sis01 conf]# ls
tuling.tts.sh
[root@sis01 conf]# chmod +x *.sh

```

## 导入zk配置

```
[root@sis01 doc]# cd /iflytek/server/skynet/plugin/tuling-tts/doc 
[root@sis01 doc]# ls
tts.zk.config
[root@sis01 doc]#  
```

tts.zk.config里面的文件内容127.0.0.1替换为192.168.83.240，通过zkui工具导入配置文件。

## 分配服务

通过xmanager管理界面分配服务，点击操作按钮，点击服务分配，系统下来框选择能力平台-语音合成系统2.0，选择 [TTS]语音合成[Rest]右侧的“+”按钮。然后点击确定按钮即可。

## 查看日志

选择服务，右击查看日志，即可以看到对应的日志文件。






