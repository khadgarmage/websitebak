---
layout:     post
title:      "软件管理"
subtitle:   "Software"
date:       2017-02-05 16:00:00
author:     "Khadgar"
header-img: "img/post-bg-re-vs-ng2.jpg"
header-mask: 0.3
catalog:    true
tags:
    - 软件
    - 依赖管理
    - PHP
---

## 包管理工具
软件开发过程中，会用到第三方库，但是这些第三方库或者框架往往又依赖其他第三方库或者框架，如果手工管理依赖关系会非常复杂，因为不仅要下载而且还要处理包之间的兼容关系，并且如果要进行更新版本那更是痛苦。  
其主要功能就是安装及安装依赖，更新，卸载等操作。

#### 主流包管理工具

|语言|包管理工具|配置文件|
|:------------ |:---|:---------------|
|Node.js| NPM | package.json   |
|前端JS | Bower |  bower.json  |
|Java | Maven |  pom.xml  |
|Python | Pip |  pip.conf(ini)  |
|Ruby | Gem |  .gemspec  |
|Php | Composer |  composer.json  |


#### PHP包管理工具
PHP包管理工具有PEAR，PECL，Composer。
* PEAR  
  Php Extension Application Repository php 扩展和应用仓库，为 php 的工具类库。
* PECL  
  PHP Extension Community Library php 的 C 扩展仓库，即 php 的 so 格式的扩展
* Composer  
  PHP依赖管理工具
###### PEAR VS PECL
PEAR是用PHP写的库，PECL是用C写的PHP扩展库。  

```
#这是一个安装 pear 的 php 发行包文件
wget http://pear.php.net/go-pear.phar

#执行安装pear和pecl
php go-pear.phar

#pear安装DB
pear install DB

#pecl安装Redis,生成redis.so，加入到php.ini即可
pecl install redis

```  

###### PEAR VS Composer
两者都可以用来管理PHP软件包，安装、更新以及卸载。
PEAR对于包的维护者来说，比较麻烦。所以很多代码已经过期了；此外相比Composer，PEAR的安装软件包比较少；使用Composer，可以基于每个项目或者全局安装软件包，而PEAR只能全局安装，如果需要不同版本的话可能造成冲突；Composer通过配置可以安装PEAR扩展包.

## Composer
#### 安装
Composer要求PHP环境必须是5.3.2+才能运行。
```
 curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer
```
#### 使用
在项目目录下创建一个 composer.json 文件，指明依赖，比如，你的项目依赖 monolog：  

```
{
    "require": {
        "monolog/monolog": "1.2.*"
    }
}
```  

* 安装  
composer install
* 自动加载  
Composer 提供了自动加载的特性，require 'vendor/autoload.php'，需要在代码初始化时增加这行代码。

延伸阅读 https://getcomposer.org/

