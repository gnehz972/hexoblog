---
title: 给媳妇儿做项目之OA排班表
tags:
  - 默认
category:
  - 默认
date: 2022-05-07 22:14:42
---

最近帮媳妇儿做了个排班表管理应用，可以在日历上直接安排排班信息，同时可以生成汇总的excel表。简单记录下过程，为了凸显项目的高大上，给项目命名叫OA排班表（dodge

<!-- more -->
需求
--
1. [x] 能按天按人头排班，好修改(Calendar页面修改)
2. [x] 排班汇总信息能导出excel（存数据库crud，Excel操作）
3. [x] 可视化每人的排班情况（Calenader+Chart）

技术分析
--
- 客户端 vs web
轻量级应用，web永远是使用最方便的

- React vs NextJs
需要后端持有数据库密钥，选NextJs，前后一把梭

- Mongo 
轻量级数据完全可以满足，有free tier


框架
--
- TypeScript
- NextJs
- MongoDb
- Material UI
- React Big Calendar
- ExcelJs
- Recharts
- Vercel

设计
--
用Material UI搭积木

Coding
--
- 配置脚手架
  - NextJs template
  - Linting & Prettier setup
  - Folder structure modify
- 构建主页面结构，页面侧边栏+对应页面
- 添加calendar页面
  - Calendar页面
  - 添加排班日程
    - 日程数据类型设计(start,end,name,type)
    - 日程增删改
  - 添加Mongodb
    - 申请实例并配置密钥
    - 绑定日程增删改
- 添加数据统计Chart页面
  - Chart图表选择
  - 数据查询并汇总呈现
- 添加导出Excel页面
  - 数据查询并后端生成excel
  - 添加下载交互

代码参看 [github](https://github.com/gnehz972/workday-next)

部署
--
  - 注册Vercel账号并创建部署实例，绑定github repo
  - 配置预置的组员信息到环境变量
  - 配置Mongodb相关密钥到环境变量
  - commit自动触发部署到 [work-day](https://workday-next.vercel.app/)

交付
--
媳妇儿看了下，媳妇儿表示很喜欢，表示还是excel更香(doge
