---
title: 编译aosp源码，刷user-debug开发机之nexus 6p
tags:
  - 刷机
  - aosp
categories:
  - android
abbrlink: c3525f2d
date: 2019-01-01 15:28:46
---

最近想研究下竞品app的页面，想在真机上使用hierarchyviewer工具看看，再dump数据库看看(刚升级sdk build tool到28，hierarchyviewer已经被移除了。。老实用studio上的view inspector吧)。竞品是release包，手上也没有root机器，怎么办呢，想起以前debug开发机的便利，果断刷机。手上正好有个nexus 6p，完全可以自己编译aosp源码，刷个user-debug的开发机。系统都可以debug，装在上面app就随便揉捏了。
<!-- more -->

## 准备
1. Ubuntu 18.04.1 LTS   
2. 笔记本(4核i5,8G内存，256G ssd)预留空余磁盘160G以上，源码加编译结果占用了148G，open-jdk8环境
3. 手机nexus 6p (angler)  
4. 访问google的能力  https://source.android.com/setup/build/requirements

## 步骤  
1. 下载repo
https://source.android.com/setup/build/downloading

2. 下载源码  
注意手机支持的分支,直接检出  
```
repo init -u https://android.googlesource.com/platform/manifest -b android-8.1.0_r47
repo sync -j4
```
  查看repo当前检出分支 
  git --git-dir .repo/manifests/.git/ branch -a
  失败了不要紧，重新sync就好，之前下载的有缓存不会重头开始的

3. 下二进制文件（vendor的驱动）
注意驱动对应的build number，而build bumber和之前检出的分支对应 **OPM7.181005.003	android-8.1.0_r47**
对应关系查看https://source.android.com/setup/start/build-numbers
华为的和高通的两个文件都要下载 解压后运行对应的sh文件，会在aosp的根目录生成vendor目录
下载地址：https://developers.google.com/android/drivers

4. 改环境,使用openjdk
```
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 
export JRE_HOME=$JAVA_HOME/jre 
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib 
export PATH=$JAVA_HOME/bin:$PATH 
export LC_ALL=C
```
5. 编译源码
```
build/envsetup.sh 
lunch aosp_angler-userdebug 
make -j4
```

6. 刷机 
```
export ANDROID_PRODUCT_OUT=your-aosp-path/out/target/product/angler 
fastboot flashall -w
```


## 问题
1. ninja: build stopped: subcommand failed ninja failed with: exit status 1 
解决：可能是本地环境问题 export LC_ALL=C 
ref：https://stackoverflow.com/questions/51324238/aosp-build-stopped-subcommand-failedhttps://www.programering.com/a/MDM3UzNwATQ.html 
> LC_ALL=C is to remove all localized settings, make the correct execution.

2. /bin/bash: xmllint: command not found 
解决：安装xmllint 
> sudo apt-get install libxml2-utils

3. Build with Jack .... Out of memory error GC overhead limit exceeded. Try increasing heap size with java option '-Xmx'.   
解决：修改jack-server的配置文件ref：http://www.2net.co.uk/blog/jack-server.html
> vim ~/.jack-settings 
添加一行：
> JACK_SERVER_VM_ARGUMENTS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx4096m" 
重启下jack-server:
> prebuilts/sdk/tools/jack-admin kill-server 
> prebuilts/sdk/tools/jack-admin start-server

4. flash完后不断重启  
可能原因:
    - binary文件只下了一个 
    - 没有下载对应build numer的binary文件 
    - 编译因为错误中断过，继续编译引起的  
解决：
可能某些中间文件不完全或受损，把out文件夹删掉后再整编一次