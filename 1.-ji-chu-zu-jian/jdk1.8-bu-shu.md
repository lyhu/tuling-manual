# CentOS7.X下JDK1.8部署文档
=====================

描述了系统CentOS7.X下JDK1.8部署过程和注意事项

## 依赖组件

1. `vim`编辑器

### 下载

```bash
$ wget http://192.168.65.191:8082/installs/java/jdk/jdk-8u112-linux-x64.tar.gz
```
## 安装步骤

### 环境检查

CentOS7.X安装默认安装gnome版本时自带了`JDK`，在安装时，需要检查老版本是否存在，如果存在则需要移除`OpenJDK`
```bash
[root@localhost bin]# java -version
openjdk version "1.8.0_102"
OpenJDK Runtime Environment (build 1.8.0_102-b14)
OpenJDK 64-Bit Server VM (build 25.102-b14, mixed mode)
[root@localhost bin]#
```
### 移除OpenJDK

我们可以通过` rpm -qa |grep java`命令查询已经安装的`OpenJDK`版本，通过相关的命令移除已安装的版本。
```bash
[root@localhost bin]# rpm -qa |grep java
java-1.8.0-openjdk-1.8.0.102-4.b14.el7.x86_64
tzdata-java-2016g-2.el7.noarch
javapackages-tools-3.4.1-11.el7.noarch
java-1.8.0-openjdk-headless-1.8.0.102-4.b14.el7.x86_64
java-1.7.0-openjdk-headless-1.7.0.111-2.6.7.8.el7.x86_64
java-1.7.0-openjdk-1.7.0.111-2.6.7.8.el7.x86_64
python-javapackages-3.4.1-11.el7.noarch
[root@localhost bin]# 
[root@localhost bin]# rpm -e --nodeps java-1.8.0-openjdk-1.8.0.102-4.b14.el7.x86_64
[root@localhost bin]# rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.102-4.b14.el7.x86_64
[root@localhost bin]# rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.111-2.6.7.8.el7.x86_64
[root@localhost bin]# rpm -e --nodeps java-1.7.0-openjdk-1.7.0.111-2.6.7.8.el7.x86_64
[root@localhost bin]# rpm -qa |grep java
tzdata-java-2016g-2.el7.noarch
javapackages-tools-3.4.1-11.el7.noarch
python-javapackages-3.4.1-11.el7.noarch
```
> 注：如下软件包不需要移除：
> 1. tzdata-java-2016g-2.el7.noarch
> 2. javapackages-tools-3.4.1-11.el7.noarch
> 3. python-javapackages-3.4.1-11.el7.noarch

### 解压OracleJDK

```bash
$ mkdir -p /usr/java/
$ tar -zxvf jdk-8u112-linux-x64.tar.gz -C /usr/java/
```
### 配置环境变量

使用vim或vi编辑 */etc/profile* 文件，追加以下3行
```bash
export JAVA_HOME=/usr/java/jdk1.8.0_112
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH 
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$JAVA_HOME:$PATH
```
通过`source`命令立刻生效
```bash
$ source /etc/profile
```
### 指定全局JDK

alternatives是Linux下的一个功能强大的命令。只能在root权限下执行。如系统中有几个命令功能十分类似，却又不能随意删除，那么可以用 alternatives 来指定一个全局的设置。alternatives常用于同一个系统中安装同一软件的多个版本。
```bash
[root@localhost java]# alternatives --install /usr/bin/java java /usr/java/jdk1.8.0_112/bin/java 300
[root@localhost java]# alternatives --install /usr/bin/java javac /usr/java/jdk1.8.0_112/bin/javac 300
[root@localhost java]# alternatives --config java

There is 1 program that provides 'java'.

  Selection    Command
-----------------------------------------------
*+ 1           /usr/java/jdk1.8.0_112/bin/java

Enter to keep the current selection[+], or type selection number: 1
```

## 配置说明

无

## 日常维护

无

## 注意事项

无

## 健康检查

```bash
$ java -version
java version "1.8.0_112"
Java(TM) SE Runtime Environment (build 1.8.0_112-b15)
Java HotSpot(TM) 64-Bit Server VM (build 25.112-b15, mixed mode)
```

