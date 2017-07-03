---
title: Vim常用快捷键
date: 2017-07-03 21:48:09
tags:
---
##  导航
- h  左
- j  下
- k  上
- l  右
- w 下一个词。 (word)
- b  上一个词。 (backword)
- ctrl + n :  下一个候选
- ctrl + p :  上一个候选
- shift + 4: 跳到当前行的末尾( shift + 4为 $ , 这是正则表达式中 末尾的意思）
- 0:  跳到当前行的行首： 0
- gg: 第一行
- shift + g: 末行。
- ctrl + f: 向下一屏（f = forward)
- ctrl + b: 向上一屏（b = backward)
- g; :跳到 上一次编辑的地方
- g, :跳到 下一次编辑的地方
- ctrl + o: 快速返回上一次编辑的文件 ( o 意为 outer )
- ctrl + i: 快速返回下一次编辑的文件 ( i 意为 inner )

## 搜索
- /\cxxx 忽略大小写搜索 
- /\Cxxx 精确搜索
- n 继续搜索下一个
- shift+n 搜索前一个
- %s/原来的字符串/新字符串/  全局替换 

## 选择
shift + v， 然后上下左右移动。

## 编辑
- i: 在光标前输入(insert)
- a: 在光标后输入 (append / after)
- shift+i: 在行首增加内容
- shift+a: 在行末增加内容
- o: 在光标下行增加内容
- shift+o: 在光标上行增加内容
- x: 删掉一个字母
- dw db: 删掉一个单词
- dd: 删掉一行： dd
- 删掉多行： shift +v, 然后 x 或者 d
- yw: 复制一个单词
- yy: 复制当前行
- p: 粘贴
- u: 撤销
- ctrl+z: 暂存
- fg: 恢复暂存 
- q:  退出
- wq:  保存退出
