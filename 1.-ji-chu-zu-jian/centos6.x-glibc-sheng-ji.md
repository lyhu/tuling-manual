# CentOS6.x Glibc升级

> 描述了系统CentOS6.X下Glibc升级过程

## 依赖组件

> 此项操作需要CentOs系统的**root**用户权限。

## 下载
> 创建临时软件目录/iflytek/soft目录

```bash
	[root@iengine-dev ~]# mkdir -p /iflytek/soft
	[root@iengine-dev ~]# cd /iflytek/soft/
	[root@iengine-dev soft]# ls
	[root@iengine-dev soft]# 
	[root@iengine-dev soft]# wget http://192.168.65.191:8082/installs/glibc-2.14/glibc-2.14.tar.gz
	[root@iengine-dev soft]# wget http://192.168.65.191:8082/installs/glibc-2.14/libstd.so.zip
	[root@iengine-dev soft]# wget http://192.168.65.191:8082/installs/glibc-2.14/libner_iFlyWS_Engine.so 
```

## 安装步骤

### 1. 解压glibc-2.14.tar.gz 

```bash
	[root@iengine-test soft]# tar -zxvf glibc-2.14.tar.gz 
```
### 2. 编译安装glibc-2.14

```bash
	root@iengine-test soft]# cd glibc-2.14
	[root@iengine-test glibc-2.14]# mkdir build
	[root@iengine-test glibc-2.14]# cd build/
	[root@iengine-test build]# export LD_LIBRARY_PATH=/lib64
	[root@iengine-test build]#  env | grep LD_LIBRARY_PATH
	LD_LIBRARY_PATH=/lib64
	[root@iengine-test build]# 
	[root@iengine-dev build]# ../configure --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin --disable-sanity-checks
	[root@iengine-dev build]# make
	[root@iengine-dev build]# make install
	primary library!
	make[1]: *** [install] Error 1
	make[1]: Leaving directory `/iflytek/soft/glibc-2.14'
	make: *** [install] Error 2
```

### 3. 验证安装

```bash
	[root@iengine-dev build]#  getconf -a |grep glibc -i
	GNU_LIBC_VERSION                   glibc 2.14
	[root@iengine-dev build]# 
```

### 4. 设置软链

```bash
	[root@iengine-dev build]# cd ../../
	[root@iengine-dev soft]# ls
	glibc-2.14  glibc-2.14.tar.gz  libstd.so.zip
	[root@iengine-dev soft]# 
	
	[root@iengine-dev soft]# env | grep LD_LIBRARY_PATH
	LD_LIBRARY_PATH=/lib64
	[root@iengine-dev soft]#
	[root@iengine-dev soft]# unzip libstd.so.zip  -d /lib64/
	Archive:  libstd.so.zip
	  inflating: /lib64/libstdc++.so.6   
	  inflating: /lib64/libstdc++.so.6.0.17  
	[root@iengine-dev soft]# rm -f /usr/lib64/libstdc++.so.6
	[root@iengine-dev soft]# ln -s /lib64/libstdc++.so.6 /usr/lib64/libstdc++.so.6
	[root@iengine-dev soft]# 
```
### 5. 验证依赖
> 验证glibc是否升级成功，可以通过libner_iFlyWS_Engine.so文件进行检查

```bash
chmod 755 libner_iFlyWS_Engine.so 
[root@iengine-dev soft]# ldd -v libner_iFlyWS_Engine.so 
	linux-vdso.so.1 =>  (0x00007ffd6c97b000)
	libpthread.so.0 => /lib64/libpthread.so.0 (0x00002b3f68c51000)
	libdl.so.2 => /lib64/libdl.so.2 (0x00002b3f68e6f000)
	libboost_regex.so.1.42.0 => ./libboost_regex.so.1.42.0 (0x00002b3f69073000)
	libjsoncpp.so => ./libjsoncpp.so (0x00002b3f6934d000)
	libstdc++.so.6 => /lib64/libstdc++.so.6 (0x00002b3f69591000)
	libgcc_s.so.1 => /lib64/libgcc_s.so.1 (0x00002b3f69898000)
	libc.so.6 => /lib64/libc.so.6 (0x00002b3f69aae000)
	/lib64/ld-linux-x86-64.so.2 (0x0000003b19c00000)
	libm.so.6 => /lib64/libm.so.6 (0x00002b3f69e3b000)

	Version information:
	./libner_iFlyWS_Engine.so:
		libgcc_s.so.1 (GCC_3.0) => /lib64/libgcc_s.so.1
		libdl.so.2 (GLIBC_2.2.5) => /lib64/libdl.so.2
		libpthread.so.0 (GLIBC_2.2.5) => /lib64/libpthread.so.0
		libc.so.6 (GLIBC_2.14) => /lib64/libc.so.6
		libc.so.6 (GLIBC_2.4) => /lib64/libc.so.6
		libc.so.6 (GLIBC_2.2.5) => /lib64/libc.so.6
		libstdc++.so.6 (GLIBCXX_3.4.14) => /lib64/libstdc++.so.6
		libstdc++.so.6 (GLIBCXX_3.4.5) => /lib64/libstdc++.so.6
		libstdc++.so.6 (CXXABI_1.3) => /lib64/libstdc++.so.6
		libstdc++.so.6 (GLIBCXX_3.4.15) => /lib64/libstdc++.so.6
		libstdc++.so.6 (GLIBCXX_3.4) => /lib64/libstdc++.so.6
	/lib64/libpthread.so.0:
		ld-linux-x86-64.so.2 (GLIBC_2.3) => /lib64/ld-linux-x86-64.so.2
		ld-linux-x86-64.so.2 (GLIBC_2.2.5) => /lib64/ld-linux-x86-64.so.2
		ld-linux-x86-64.so.2 (GLIBC_PRIVATE) => /lib64/ld-linux-x86-64.so.2
		libc.so.6 (GLIBC_2.14) => /lib64/libc.so.6
		libc.so.6 (GLIBC_2.3.2) => /lib64/libc.so.6
		libc.so.6 (GLIBC_PRIVATE) => /lib64/libc.so.6
		libc.so.6 (GLIBC_2.2.5) => /lib64/libc.so.6
	/lib64/libdl.so.2:
		ld-linux-x86-64.so.2 (GLIBC_PRIVATE) => /lib64/ld-linux-x86-64.so.2
		libc.so.6 (GLIBC_PRIVATE) => /lib64/libc.so.6
		libc.so.6 (GLIBC_2.2.5) => /lib64/libc.so.6
	./libboost_regex.so.1.42.0:
		libgcc_s.so.1 (GCC_3.0) => /lib64/libgcc_s.so.1
		libpthread.so.0 (GLIBC_2.2.5) => /lib64/libpthread.so.0
		libc.so.6 (GLIBC_2.14) => /lib64/libc.so.6
		libc.so.6 (GLIBC_2.3.4) => /lib64/libc.so.6
		libc.so.6 (GLIBC_2.2.5) => /lib64/libc.so.6
		libc.so.6 (GLIBC_2.4) => /lib64/libc.so.6
		libstdc++.so.6 (GLIBCXX_3.4.11) => /lib64/libstdc++.so.6
		libstdc++.so.6 (CXXABI_1.3) => /lib64/libstdc++.so.6
		libstdc++.so.6 (GLIBCXX_3.4.15) => /lib64/libstdc++.so.6
		libstdc++.so.6 (GLIBCXX_3.4) => /lib64/libstdc++.so.6
	./libjsoncpp.so:
		libgcc_s.so.1 (GCC_3.0) => /lib64/libgcc_s.so.1
		libpthread.so.0 (GLIBC_2.2.5) => /lib64/libpthread.so.0
		libc.so.6 (GLIBC_2.14) => /lib64/libc.so.6
		libc.so.6 (GLIBC_2.4) => /lib64/libc.so.6
		libc.so.6 (GLIBC_2.2.5) => /lib64/libc.so.6
		libstdc++.so.6 (GLIBCXX_3.4.5) => /lib64/libstdc++.so.6
		libstdc++.so.6 (CXXABI_1.3) => /lib64/libstdc++.so.6
		libstdc++.so.6 (GLIBCXX_3.4) => /lib64/libstdc++.so.6
	/lib64/libstdc++.so.6:
		libm.so.6 (GLIBC_2.2.5) => /lib64/libm.so.6
		ld-linux-x86-64.so.2 (GLIBC_2.3) => /lib64/ld-linux-x86-64.so.2
		libgcc_s.so.1 (GCC_4.2.0) => /lib64/libgcc_s.so.1
		libgcc_s.so.1 (GCC_3.3) => /lib64/libgcc_s.so.1
		libgcc_s.so.1 (GCC_3.0) => /lib64/libgcc_s.so.1
		libc.so.6 (GLIBC_2.3) => /lib64/libc.so.6
		libc.so.6 (GLIBC_2.3.2) => /lib64/libc.so.6
		libc.so.6 (GLIBC_2.2.5) => /lib64/libc.so.6
	/lib64/libgcc_s.so.1:
		libc.so.6 (GLIBC_2.4) => /lib64/libc.so.6
		libc.so.6 (GLIBC_2.2.5) => /lib64/libc.so.6
	/lib64/libc.so.6:
		ld-linux-x86-64.so.2 (GLIBC_PRIVATE) => /lib64/ld-linux-x86-64.so.2
		ld-linux-x86-64.so.2 (GLIBC_2.3) => /lib64/ld-linux-x86-64.so.2
	/lib64/libm.so.6:
		libc.so.6 (GLIBC_PRIVATE) => /lib64/libc.so.6
		libc.so.6 (GLIBC_2.2.5) => /lib64/libc.so.6
[root@iengine-dev soft]# 
```

## 配置说明

> 升级glic2.14不需要额外做其他配置。

## 日常维护

> glibc2.14升级成功后，不需要日常维护。

## 注意事项

> glibc升级应在操作系统刚装完就做，这样可以尽可能的减少不必要的问题。

## 健康检查
> 可以通过getconf -a |grep glibc -i和ldd -v libner_iFlyWS_Engine.so 命令进行检查
