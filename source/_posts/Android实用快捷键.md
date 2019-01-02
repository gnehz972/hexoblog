---
title: Android实用快捷键
date: 2017-07-04 23:58:46
tags: 
- Android
- tools
categories:
- tools
---
Android开发实用快捷键收录，adb,aapt,android,fastboot
命令行工具
<!-- more -->
## adb
- 查看屏幕尺寸/density
  ```adb shell wm size/density```
- 查看应用启动时间
```adb shell am start -W -n "packagename/absoluteMainActivityname"```
- 查看当前focused Activity
 ```adb shell dumpsys activity activities|grep -i focus```
- 设置log tag level
  ```adb shell setprop log.tag.Email VERBOSE```
- dump应用内存占用
```adb shell dumpsys meminfo packagename```
- 查看包信息
 ```adb shell dumpsys package packagename```
- 输入文字
 ```adb shell input text "dddd"```
- 查看包安装路径 
  ```adb shell pm list packages -f |grep Email```
- 跑monkey
 ```adb shell monkey  -p com.android.email --throttle 150 -v -s 3500 300000```
- 查看手机ip
  ```adb shell netcfg```
- 重新挂载system为rw(root手机刷debug版本系统可行，或者启动一个可以读写的模拟器)
```
adb disable-verity
adb reboot
adb root
adb shell mount  //查看挂载点
adb shell mount -o remount,rw  -t ext4  /dev/block/dm-0 /system```

## aapt
- 查看apk版本信息
```aapt dump badging app-release.apk```
- 查看apk权限信息
```aapt dump permissions app-release.apk```
- 导出apk string内容
```aapt d --values resources ~/temp/EmailRes.apk >~/temp/email_string.txt```

## emulator
- 列举所有可用模拟器
```emulator -list-avds```
- 启动带shell输出模拟器
```emulator -avd xxx -shell```
- 启动一个可以将/system挂载为rw的模拟器
```emulator -avd 3.7_WVGA_Nexus_One_API_23 -writable-system```

## android
- 命令行产看可以安装更新的sdk
```android list sdk```
- 服务器中命令行使用代理更新指定SDK(jenkin服务器中可能没有界面)
```android update sdk --no-ui --filter 2 --proxy-host mirrors.neusoft.edu.cn  --proxy-port 80 -s```

## fastboot
- fastboot刷机
```
adb reboot bootloader
fastboot flash boot boot.img
fastboot flash system system.img
fastboot flash userdata userdata.img
fastboot flash custpack custpack.img
fastboot reboot```

## others
- 命令行查看apk签名  
```keytool -list -printcert -jarfile app.apk```
- tcpdump抓包
```/data/local/tcpdump -p -vv -s 0 -w /sdcard/capture.pcap```
```-s，截取的包字节长度，默认情况下tcpdump会展示96字节的长度，要获取完整的长度可以用-s0或者-s1600
-w 表示抓取的包保存的文件路径
-v，展示更多的有用信息，还可以用-vv -vvv增加信息的展示量。
-p 将网络接口设置成非混杂模式```
