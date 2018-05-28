# 语义蕴含服务-SSM

## 环境检查

语义蕴含引擎需要支持FMA指令，如果CPU不支持这个指令集，会导致语义蕴含无法正常启动。

### CPU是否支持FMA指令检查

积和熔加运算\(Fused Multiply-Add，FMA\)，也就是可以在同一条指令里同时执行加法和乘法运算，可提高浮点计算速度和数字精确度，改善矢量和标量工作流的执行。语义蕴含引擎ssm依赖fma指令集。

#### 检查是否支持

通过命令`cat /proc/cpuinfo |grep fma`

**支持输出**

```bash
[root@i-06CEBB0F ~]# cat /proc/cpuinfo |grep fma
flags        : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx lm constant_tsc unfair_spinlock pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm xsaveopt fsgsbase bmi1 avx2 smep bmi2 invpcid
flags        : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx lm constant_tsc unfair_spinlock pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm xsaveopt fsgsbase bmi1 avx2 smep bmi2 invpcid
flags        : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx lm constant_tsc unfair_spinlock pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm xsaveopt fsgsbase bmi1 avx2 smep bmi2 invpcid
flags        : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx lm constant_tsc unfair_spinlock pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm xsaveopt fsgsbase bmi1 avx2 smep bmi2 invpcid
flags        : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx lm constant_tsc unfair_spinlock pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm xsaveopt fsgsbase bmi1 avx2 smep bmi2 invpcid
flags        : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx lm constant_tsc unfair_spinlock pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm xsaveopt fsgsbase bmi1 avx2 smep bmi2 invpcid
flags        : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx lm constant_tsc unfair_spinlock pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm xsaveopt fsgsbase bmi1 avx2 smep bmi2 invpcid
flags        : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx lm constant_tsc unfair_spinlock pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm xsaveopt fsgsbase bmi1 avx2 smep bmi2 invpcid
[root@i-06CEBB0F ~]#
```

**不支持输出**

```bash
[root@iengine-dev soft]# cat /proc/cpuinfo |grep fma
[root@iengine-dev soft]#
```

## 转写引擎授权

语义蕴含引擎需要授权`SSM,语义蕴含引擎`,通过skylock平台进行授权，

```text
见"Skylock授权首次导入授权步骤说明.md"
```

## 添加ES插件

放入语义蕴含插件

```bash
[root@sis01 plugins]# ls
model-compare  sql  tuling-ssm-esplugin-5.4.3.zip  x-pack
[root@sis01 plugins]# ls
model-compare  sql  x-pack
[root@sis01 plugins]# unzip tuling-ssm-esplugin-5.4.3.zip 
Archive:  tuling-ssm-esplugin-5.4.3.zip
   creating: tuling-ssm-esplugin-5.4.3/
  inflating: tuling-ssm-esplugin-5.4.3/plugin-descriptor.properties  
  inflating: tuling-ssm-esplugin-5.4.3/commons-codec-1.10.jar  
  inflating: tuling-ssm-esplugin-5.4.3/fastjson-1.2.44.jar  
  inflating: tuling-ssm-esplugin-5.4.3/tuling-ssm-esplugin-5.4.3.jar  
[root@sis01 plugins]# ls
model-compare  sql  tuling-ssm-esplugin-5.4.3  tuling-ssm-esplugin-5.4.3.zip  x-pack
[root@sis01 plugins]# rm -rf tuling-ssm-esplugin-5.4.3.zip 
[root@sis01 plugins]# ls
model-compare  sql  tuling-ssm-esplugin-5.4.3  x-pack
[root@sis01 plugins]#
```

> plugin插件目录不能放置压缩文件，tuling-ssm-esplugin-5.4.3.zip如果不删掉会导致es无法正常启动

重启es

```bash
[root@sis01 plugins]# cd /iflytek/server/es-5.4.0/
[root@sis01 es-5.4.0]# ls
elasticsearch-5.4.0  elasticsearch-head-master  es.sh  es-sql-site  kibana-5.4.0-linux-x86_64  license.json  node-v6.11.0-linux-x64  setup
[root@sis01 es-5.4.0]# ./es.sh  stop
 =========================================== 
 -- ES 5.4.0  stop ..                    -- 
 ------------------------------------------- 
 the es is stoped. pid:951 
 the header is stoped. pid:1058 
./es.sh: line 115:  1058 Killed                  nohup grunt server > header.log 2>&1
 the esql is stoped. pid:1127 
./es.sh: line 115:  1127 Killed                  node node-server.js > sql.log 2>&1
 the kibana is stoped. pid:1172 
 =========================================== 
[root@sis01 es-5.4.0]# ./es.sh: line 115:  1172 Killed                  nohup ./kibana > kibana.log 2>&1

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
 elasticsearch pid:5875 
 header pid:5949 
 esql pid:5962 
 kibana pid: 
 =========================================== 
[root@sis01 es-5.4.0]#
```

## 放置引擎文件

放置引擎文件至目录/iflytek/engine/lib

```bash
[root@sis01 iflytek]# mkdir -p  /iflytek/engine/lib
[root@sis01 iflytek]# cd /iflytek/engine/lib/
[root@sis01 lib]# ls
[root@sis01 lib]# ls
ssm_1.0.0.0003.zip
[root@sis01 lib]# 
[root@sis01 lib]# unzip ssm_1.0.0.0003.zip 
Archive:  ssm_1.0.0.0003.zip
   creating: ssm/
   creating: ssm/Resource/
   ................................
   ................................
inflating: ssm/x64/Release/tinyxml.o  
  inflating: ssm/x64/Release/tinyxmlerror.o  
  inflating: ssm/x64/Release/tinyxmlparser.o  
  inflating: ssm/x64/Release/ws_com_utils.o  
[root@sis01 lib]# ls
ssm  ssm_1.0.0.0003.zip
[root@sis01 lib]# 
[root@sis01 lib]# rm -rf ssm_1.0.0.0003.zip 
[root@sis01 lib]# ls
ssm
[root@sis01 lib]#
```

> 文件解压后要注意iFlySSM\_Linux.cfg的文件内容
>
> ```bash
> [root@sis01 conf]# cd /iflytek/engine/lib/ssm/conf
> [root@sis01 conf]# ls
> iFlySSM_Linux.cfg
> [root@sis01 conf]# vim iFlySSM_Linux.cfg
> ```
>
> 其中home\_path = /iflytek/engine/lib/ssm/bin值需要和ssm引擎的目录保持一致，否则会导致无法正常启动。

## 放置tuling-ssm组件

```bash
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  xmanager
[root@sis01 plugin]# 
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-ssm-build-2.0.0-SNAPSHOT-Build_1003.zip  xmanager
[root@sis01 plugin]# unzip tuling-ssm-build-2.0.0-SNAPSHOT-Build_1003.zip 
Archive:  tuling-ssm-build-2.0.0-SNAPSHOT-Build_1003.zip
   creating: tuling-ssm-build-2.0.0-SNAPSHOT/
  inflating: tuling-ssm-build-2.0.0-SNAPSHOT/doc/tuling-ssm.zk.config  
   ................................
   ................................
    extracting: tuling-ssm-build-2.0.0-SNAPSHOT/lib/jcifs-1.3.17.jar  
 extracting: tuling-ssm-build-2.0.0-SNAPSHOT/lib/commons-logging-1.0.4.jar  
 extracting: tuling-ssm-build-2.0.0-SNAPSHOT/lib/android-json-0.0.20131108.vaadin1.jar  
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-ssm-build-2.0.0-SNAPSHOT  tuling-ssm-build-2.0.0-SNAPSHOT-Build_1003.zip  xmanager
[root@sis01 plugin]# rm -rf tuling-ssm-build-2.0.0-SNAPSHOT-Build_1003.zip 
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-ssm-build-2.0.0-SNAPSHOT  xmanager
[root@sis01 plugin]# mv tuling-ssm-build-2.0.0-SNAPSHOT/ tuling-ssm
[root@sis01 plugin]# ls
ant  readme.txt  sleuth  tuling-ssm  xmanager
```

## 赋予执行权限

```bash
[root@sis01 conf]# cd /iflytek/server/skynet/plugin/tuling-ssm/conf
[root@sis01 conf]# ls
tuling.ssm.sh
[root@sis01 conf]# chmod +x *.sh
[root@sis01 conf]# ls
tuling.ssm.sh
[root@sis01 conf]#
```

## 导入zk配置

```bash
[root@sis01 doc]# cd /iflytek/server/skynet/plugin/tuling-ssm/doc 
[root@sis01 doc]# ls
RELEASES.md  tuling-ssm.zk.config
[root@sis01 doc]#
```

tuling-ssm.zk.config里面的文件内容127.0.0.1替换为192.168.83.240，通过zkui工具导入配置文件。

## 分配服务

通过xmanager管理界面分配服务，点击操作按钮，点击服务分配，系统下来框选择能力平台-语义蕴含2.0，选择 \[SSM\]语义蕴含\[Rest\]右侧的“+”按钮。然后点击确定按钮即可。

## 查看日志

选择服务，右击查看日志，即可以看到对应的日志文件。

