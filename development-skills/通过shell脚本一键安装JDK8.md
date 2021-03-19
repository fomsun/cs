---
title: 通过shell脚本一键安装JDK8
tags:
  - Java
  - Shell
  - Linux
category:
  - Java
abbrlink: 63927
date: 2021-03-19 16:28:10
---
> 难得闲下来，总结一下了最近的脚本，本文主要是给大家介绍一下快速安装jdk的方法

## 背景
作为一只老鸟，每次项目运行环境搭建，都需要安装JDK，配置环境变量等，做一些重复的工作。这样的事情可能对于刚接触Linux的人来说是很乐意做的，但是接触多了，总是觉得做这样的事情没有多大意义。因此，写一个自动执行的脚本势在必行，不仅可以省时省力，还可以顺带锻炼一下自己的shell脚本编程能力。

## 安装包
安装前我们需要准备一个安装包，拿Linux 64位举个例子，安装包也可以从官网下载。
链接:https://pan.baidu.com/s/1Ps8GC8XPSsu6utW9nV3QAQ  
密码:9wfy

## 脚本

```
#!/bin/bash
 
JAVA_HOME=''
jdkDir=/usr/local/jdk/
 
#校验解压JDK的文件夹
if [ -e $jdkDir ]
then 
	echo "$jdkDir文件夹已存在,执行被迫终止!"
	exit
fi
 
echo "检查压缩包是否存在..."
if [ -f $1 ]
then
	mkdir $jdkDir
	echo "开始解压..."
	tar -zxvf $1 -C $jdkDir
	for file in $jdkDir*
	do
		if [ -d $file ]
		then
			JAVA_HOME=$file
		fi
	done
	echo "JAVA_HOME=$JAVA_HOME"	
 
else
	echo 文件不存在:$1
	exit
fi
 
#修改环境变量
 
echo "export JAVA_HOME=$JAVA_HOME" >> /etc/profile
echo "export PATH=\$PATH:\$JAVA_HOME/bin" >> /etc/profile
echo "export CLASSPATH=.:\$JAVA_HOME/lib/dt.jar:\$JAVA_HOME/lib/tools.jar" >> /etc/profile
 
echo "jdk 安装配置完成"
echo `java -version`
```

## 执行命令
上面脚本可以用`vi`保存脚本文件`jdk_install.sh`，然后执行如下命令，参数只需传递一个，就是你要解压的jdk压缩包的存放路径:
```
sh ./installj8.sh ./jdk-8u281-linux-x64.tar.gz
```


## 验证

脚本正常执行后，我们可以执行以下命令进行验证：
```
java -version
```
如果提示没有该命令，可以执行如下命令，刷新配置后再进行验证：
```
source /etc/profile 
```

