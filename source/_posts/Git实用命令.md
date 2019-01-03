---
title: Git实用命令
abbrlink: cefffd1f
date: 2017-07-05 23:46:32
tags:
---
git实用命令，快捷键收录。
<!-- more -->
## config
- 配置用户名和邮箱
 `git config --global user.name "username"`
 `git config --global user.email "mailbolx@163.com"`
- git配置代理(看ss配置)
`git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'`
- 取消代理
`git config --global --unset http.proxy`
- git打开颜色开关
 `git config --global color.ui true`

## branch
- 设置本地dev分支track远程分支origin/master
`git branch -u origin/master dev`
- 切到上一个工作分子
`git checkout -`
- 查看本地分支track的远程分支
`git branch -vv`
- checkout远程分支
`git checkout -t origin/dev  //-t参数, 创建和远程分支一样名字的本地分支`
- 删除远程分支
`git push [远程名] :[分支名]`
`git push origin :serverfix`

## commit
- 获取两个branch/commit (分支 / 提交) 共同的祖先
`git merge-base commit1 commit2`
- 暂存提交/签出暂存
`git stash/git stash pop`
- 合并多次提交，生成patch
`git diff HEAD~2..HEAD > my-patch.diff 
git format-patch HEAD~2..HEAD --stdout > changes.patch
git format-patch -x --stdout > patch-ddmmyyy.patch
git diff tag1 tag2 > the-patch.diff`
- 查看改变的文件列表
`git diff tag1 tag2 --stat`
- 仅查看指定文件/文件夹改变
`git log -- "some file/dir"`
`git diff tag1 tag2 -- some/file/name`

## tag
- 在当前位置添加tag
`git tag -a v1.4 -m "v1.4"`
- 为commit添加tag
`git tag -a v1.2 9fceb02`
- 推送tag到远程分支
`git push origin --tags`
`git push origin 标签名`
- 删除本地标签：
`git tag -d 标签名`
- 删除远程标签：
`git push origin :refs/tags/标签名`

## submodule
- 添加submodule
`git submodule add https://github.com/hexojs/hexo-theme-light.git themes/light
git commit -am "add submodule"
git submodule init
git submodule update`
