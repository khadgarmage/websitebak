---
layout:     post
title:      "软件管理"
subtitle:   "System"
date:       2017-01-19 16:00:00
author:     "Khadgar"
header-img: "img/post-bg-re-vs-ng2.jpg"
header-mask: 0.3
catalog:    true
tags:
    - 系统管理
    - OPS
---

## 包安装
#### RPM & DPKG
Linux系统安装软件的方式有几种，编译安装，rpm，dpkg等，其中主流的是rpm和dpkg。
|Distribution代表|软件管理机制|使用指令|线上升级机制 |
| ------------ |:---:| ---------------:|-----------: |
|Red Hat/Fedora| RPM | rpm, rpmbuild   |YUM (yum)    |
|Debian/Ubuntu | DPKG|   dpkg          |APT (apt-get)|

#### RPM & SRPM
RPM是直接编译好的软件格式，可以直接安装，但是要求安装环境和RPM编辑环境一致或者相当才可以安装。SRPM是带有源码的软件，可以通过修改配置，在安装环境下先编译成RPM文件，然后再进行安装可以解决这个问题。

#### RPM命名介绍
> xx.rpm   <==RPM的格式，已经变编译切打包完成的rpm软件;    
> xx.src.rpm <==SRPM的格式，包括源码，需要编译成RPM，才能安装。

> test -        3.11   -     5        .el7.x86_64  .rpm  
> 软件名称    软件版本 发布次数 平台 文档后缀  

i386, i586, i686, noarch, x86_64表示平台，其中noarch表示不区分平台  

#### RPM安装路径
|路径|说明|
|----|----|
|/etc|配置文件目录|
|/usr/bin|可执行文件|
|/usr/lib|可执行文件使用的动态链接库|
|/usr/share/doc|软件使用说明文档|
|/usr/share/man|man page文档|

#### RPM命令
```
# 安装
rpm -ivh /mnt/xxx/test.rpm

# 网络安装
rpm -ivh http://web.com/mnt/xxx/test.rpm

# --prefix /usr/local 指安装到/usr/local而非正规目录

# 升级更新
rpm -Fvh /mnt/xxx/test.rpm

# 查询
rpm -qa | grep xx

# 查询配置文件路径
rpm -qc httpd

# 重建资料库
rpm --rebuilddb

```

#### YUM
yum（全称为 Yellow dog Updater, Modified）是一个在Fedora和RedHat以及SUSE中的Shell前端软件包管理器。基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软体包，无须繁琐地一次次下载、安装。yum提供了查找、安装、删除某一个、一组甚至全部软件包的命令。

###### 命令
```
# 安装 -y提示确定自动输入y
rpm install -y php70

# 查找软件
yum search raid

# 查看软件功能 
yum info mdadm

# 列出所有软件
yum list

# 列出所有需要升级的软件
yum list updates

# 查询配置文件路径
rpm -qc httpd

# 重建资料库
rpm --rebuilddb

```

###### 配置
```
[root@study ~]# vim /etc/yum.repos.d/CentOS-Base.repo
[base]
name=CentOS-$releasever - Base
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```
* [base]：软件库的名字，中括号必须有，里面的名称随意取，但是不能重复。
* name：名字而已。
* mirrorlist=：列出软件库可以使用的镜像列表，如果设置yum会自动挑选镜像。
* baseurl=：可以指定的固定软件库地址。
* enable=1：1启用，0不启用
* gpgcheck=1：指定是否需要查阅 RPM 档桉内的数位签名

修改完配置需要执行yum clean all，使得配置修改生效~

###### 软件库
* 每个系统都会有官方和第三方提供的软件库，如CentOS库的链接
https://www.centos.org/docs/5/html/yum/sn-yum-maintenance.html
https://wiki.centos.org/AdditionalResources/Repositories  
* 其中PHP常用的YUM Repository如下:  
webtatic https://webtatic.com/  
remi http://rpms.remirepo.net/  
基于稳定性考虑，建议官方列表里推荐的remi

## 编译安装
```./configure && make && make install```
* configure 根据系统环境和选项配置，生成Makefile文件
```
--srcdir=DIR
源代码文件所在目录，默认为configure脚本所在目录或其父目录。
--prefix=PREFIX
体系无关文件的顶级安装目录PREFIX ，默认值一般是 /usr/local 或 /usr/local/pkgName
--exec-prefix=EPREFIX
体系相关文件的顶级安装目录EPREFIX ，默认值一般是 PREFIX
--bindir=DIR
用户可执行文件的存放目录DIR ，默认值一般是 EPREFIX/bin
--sbindir=DIR
系统管理员可执行目录DIR ，默认值一般是 EPREFIX/sbin
--libexecdir=DIR
程序可执行目录DIR ，默认值一般是 EPREFIX/libexec
--datadir=DIR
通用数据文件的安装目录DIR ，默认值一般是 PREFIX/share
--sysconfdir=DIR
只读的单一机器数据目录DIR ，默认值一般是 PREFIX/etc
--sharedstatedir=DIR
可写的体系无关数据目录DIR ，默认值一般是 PREFIX/com
--localstatedir=DIR
可写的单一机器数据目录DIR ，默认值一般是 PREFIX/var
--libdir=DIR
库文件的安装目录DIR ，默认值一般是 EPREFIX/lib
--includedir=DIR
C头文件目录DIR ，默认值一般是 PREFIX/include
--oldincludedir=DIR
非gcc的C头文件目录DIR ，默认值一般是 /usr/include
--infodir=DIR
Info文档的安装目录DIR ，默认值一般是 PREFIX/info
--mandir=DIR
Man文档的安装目录DIR ，默认值一般是 PREFIX/man
```

