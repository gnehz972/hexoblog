---
title: 极简Flutter学习笔记
tags:
  - flutter
  - dart
category:
  - flutter
abbrlink: '8342713'
date: 2019-03-13 21:13:24
---

极简flutter学习笔记
<!-- more -->

## FAQ
1. 支持热更新（hotfix，打patch）吗 [issue](https://github.com/flutter/flutter/issues/14330)
2. AndroidX support [issue](https://github.com/flutter/flutter/issues/23586)

## 下载安装flutter sdk，配置环境变量
1. 下载解压flutter sdk，将对应bin目录（xxx/flutter/bin）加入PATH环境变量
2. 有android开发环境：android sdk（加入PATH环境变量），android studio，android真机或模拟器
3. 验证命令：
    - 验证：flutter docter(如果没反应，应该是网络问题，终端设置代理到外面就好)  
    - 升级：flutter upgrade   
    - 查看版本：flutter --version
4. android studio：安装Dart和Flutter插件，重启，创建flutter应用，run，hot reload [链接](https://flutter.dev/docs/development/tools/android-studio)
5. vscode：安装Dart和Flutter扩展,创建应用（Ctrl+Shift+P查看->命令面板->Flutter:New Project）,run（Shift+F9）,hot reload [链接](https://flutter.dev/docs/development/tools/vs-code)
6. [官方链接](https://flutter.dev/docs/get-started/install/linux)

## 在现有的native app里集成flutter组件
1. app主体部分是native开发，部分feature采用flutter，见闲鱼的详情页
2. flutter sdk提供FlutterFragment，里面包含一个FlutterView，flutter代码就跑在这个容器里
3. native客户端传一个String类型的route到flutter组件，flutter组件的main入口处理分发，以导航到flutter实现的不同页面
4. native和flutter相互调用，传值  [链接](https://medium.com/flutter-io/flutter-platform-channels-ce7f540a104e)
    - MethodChannel 方法直接调用，双向
    - EventChannel 事件监听，native发，flutter收
5. [官方链接](https://github.com/flutter/flutter/wiki/Add-Flutter-to-existing-apps)

## dart基础语法学习
- [链接](http://dart.goodev.org/guides/language/language-tour)


## 工程基本结构，yaml配置构建
```
├── android  //生成的android包装工程
├── build   //编译中间文件存放位置
├── ios    //生成的ios包装工程
├── lib
│   ├── main.dart  //flutter程序入口
│   └── second.dart
├── pubspec.lock   //解析包依赖配置文件后，将满足条件的包依赖存放在该文件，工程实际上使用的该文件内容里的配置
├── pubspec.yaml   //包依赖配置文件，三方包放dependencies下面
├── README.md
└── test
    └── widget_test.dart
```

## TODO
- [ ] flutter里log及debug方式
- [ ] widget原理及使用，自定义widget
- [ ] 应用及组件生命周期
- [ ] 页面绘制流程
- [ ] 触屏事件分发流程
- [ ] 常用导航，布局ui组件
- [ ] flutter动画
- [ ] 页面路由框架
- [ ] 图片框架，网络框架，持久化框架，序列化框架
- [ ] 工程化管理框架，开发范式


## 学习资源
- [awesome-flutter](https://github.com/Solido/awesome-flutter)
- [flutter-in-action](https://github.com/flutterchina/flutter-in-action)
- [阿里咸鱼框架fish-redux](https://github.com/alibaba/fish-redux)
