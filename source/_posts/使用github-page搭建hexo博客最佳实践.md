---
title: 使用github page搭建hexo博客最佳实践
date: 2017-07-02 16:16:26
tags: 
- hexo
- tools
- h5
- blog
categories:
- tools
---
使用github page搭建hexo博客最佳实践。源文章和生成的h5分开管理，推送到各自的仓库。
<!-- more -->
**环境：**MacOs Git NodeJs
**1. 创建github page repo [github page](https://pages.github.com/)**
>注意repo name必须为你的用户名＋.github.io(*eg: gnehz972.github.io*)
>![image.png](http://upload-images.jianshu.io/upload_images/6722536-f59e83eae1bf73bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**2. 安装hexo**
```
sudo npm install -g hexo-cli  
```
**3. 创建以repo命名的hexo工程**
```
hexo init gnehz972.github.io
```
**4. 本地验证**
```
hexo server    
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```
**5. 创建新文章**
```
hexo new "first hexo blog"
INFO  Created: ~/dev/hexo/gnehz972.github.io/source/_posts/first-hexo-blog.md
```
**6. 修改配置文件_config.yml，配置部署信息**
```
deploy:
  type: git
  repo: https://github.com/gnehz972/gnehz972.github.io.git
  branch: master
```
**7. 部署到github page**
```
hexo generate
hexo deploy
```
**8. 验证部署，访问 [https://gnehz972.github.io/](https://gnehz972.github.io/)**
**9. md源文件管理**
hexo部署到站点的全是渲染过后的html文件，原始md文件没有上传。没有原始md文件，万一丢失或者想要在公司其他电脑上写文章，将很不方便。这里我又新建了一个[github repo](https://github.com/gnehz972/hexoblog)，将md文件纳入管理。
```
cd gnehz972.github.io
vim .gitignore
```
修改.gitignore文件
```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```
添加到git管理
```
git init 
git remote add origin https://github.com/gnehz972/hexoblog.git
git push -u origin master 
git add .
git commit  -am "add source"
git push origin master
```
之后写hexo blog步骤就是
```
hexo new "blog"
hexo generate
hexo deploy
//back up original md file
git commit -am "back up source file"
git push origin master
```
**10. 主题管理**
添加light主题
```
git submodule add https://github.com/hexojs/hexo-theme-light.git themes/light
git commit -am "add light"
git submodule init
git submodule update
```
修改_config.yml配置文件theme
```
theme: light
```
**11. FAQ**
>安装hexo出现 "Cannot find module './build/Release/DTraceProviderBindings'"
[reference](https://github.com/hexojs/hexo/issues/1922)
解决方案（类似重装，莫名奇妙就好了－－＃）：
$ npm install hexo --no-optional
if it doesn't work
try
$ npm uninstall hexo-cli -g
$ npm install hexo-cli -g

