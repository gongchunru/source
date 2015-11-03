title: 关于 **OS X** El Capitan 系统升级后的Java配置
date: 2015-10-25 20:59:38
tags: mac
tags: jdk
---

现在的 OS X 都已经不再默认安装Java了，所以升级之后我们需要重新装一下Java，以便之后的工作可以顺利进行。去官网下载Java，安装后路径有可能有变，然后重新在`.zshrc`中配置JAVA_HOEM就可以了。

但是我在启动zookeeper的时候发现出现了问题，`zkServer.sh start` 命令可以显示正常启动了，可我去使用的时候发现没有服务，zookeeper没有启动！！

<!-- more -->

打印出zookeeper的日志，配置文件路径在：`bin/zkEnv.sh`

```sh
if [ "x${ZOO_LOG_DIR}" = "x" ]
then
    ZOO_LOG_DIR="/Users/gongchunru/Documents/coding/asiainfo/zookeeper-3.4.6/logs/zkEnv"
fi

if [ "x${ZOO_LOG4J_PROP}" = "x" ]
then
    ZOO_LOG4J_PROP="INFO,CONSOLE"
fi

```
找到`zookeeper.out` 发现Java的路径和配置的不一样，这个时候发现，我们的系统配置环境

```bash
export PATH
export CLASSPATH
```
他们呢的顺序颠倒了，PATH一定要在后边，不然读取的还是以前的JAVA_HOEM路径。调整一下路径，发现正常启动了。

```bash

export JAVA_HOME=/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home
export M2=/Users/gongchunru/Documents/coding/learn/maven/apache-maven-3.3.3
export CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:.
export PATH=$JAVA_HOME/bin:$M2/bin:$PATH:.
```
