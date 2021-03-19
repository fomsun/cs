---
title: 通过shell脚本一键安装python3
tags:
  - Python
  - Shell
category:
  - Python
sidebar:
  left:
    sticky: true
  right:
    sticky: true
abbrlink: 62773
date: 2020-10-10 10:02:57
cover: https://img.mynamecoder.com/hacking-1685092_640.jpg
---

>  本文主要介绍python3的安装，通过python3.6.4源码安装，并且兼容python2， 实现python2与python3共存。文中包括python3的安装介绍以及安装脚本。

## 吐槽

因为红帽系列默认没有安装python3，而debian早就已经自带了python3。在centos服务器上安装python3，因此写了一个一键安装脚本供大家参考。
<!-- more -->
## 安装

一键安装python3脚本，脚本如下：
```shell
yum install -y wget epel-release xz gcc zlib zlib-devel openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel
if [[ ! -s /usr/bin/python3 ]]; then
        wget http://file.aionlife.xyz/source/download?id=5b9e7227dc72d90ebb47023a -O Python-3.6.4.tar.xz  #https://www.python.org/ftp/python/3.6.4/Python-3.6.4.tar.xz(这个地址国内下载比较慢，所以这里换成另一个地址 )
        tar -Jxvf Python-3.6.4.tar.xz
        cd Python-3.6.4
        ./configure --prefix=/usr/python3.6
        make&&make install
        ln -s /usr/python3.6/bin/python3 /usr/bin/python3
        mkdir ~/.pip
        echo -e "[global]\nindex-url = http://mirrors.aliyun.com/pypi/simple/\n[install]\ntrusted-host = mirrors.aliyun.com" > ~/.pip/pip.conf
        ln -s /usr/python3.6/bin/pip3 /usr/bin/pip3  
fi
```

>  tips: 也可以根据所安装的版本更换安装包的下载链接，注意脚本中的文件名也要同步修改。

上面脚本可以用`vi`保存脚本文件`installpy3.sh`，然后执行如下命令：

```
sh installpy3.sh
```

## 验证
经过大约几分钟的等待，脚本执行安装完毕，我们就可以分别执行`python3`和`pip3`进行验证，如出现命令提示即表示安装成功。