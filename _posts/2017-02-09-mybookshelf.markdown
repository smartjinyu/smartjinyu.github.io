---
layout: post
title:  "Bookshelf v1.6 更新 --开源的书籍管理应用"
date:   2017-02-09 01:00:00
categories: Android
excerpt: 开源的实体书籍管理应用
---

English Version [Here]

[Here]:https://github.com/smartjinyu/MyBookshelf/blob/master/README.md

## 开发背景

从家里到学校，我的每一个书架上都堆满了各种杂书，需要寻找時却往往手忙脚乱乃至无迹可寻，常常发生购买相同书籍的事情，迫切需要一款应用来管理实体书籍。试过国内的几款应用后，发现都有恼人的社交功能，且无法关闭。个人以为自己的书单是一件相当隐私的事，不应该未经同意被拿出去被人四处评价，正好[初学 Android 开发]，便利用寒假时间自己写了一款。

[初学 Android 开发]:https://smartjinyu.com/android/2016/10/25/Begin_to_learn_android_again.html

## 应用介绍

Bookshelf - 开源的实体书籍管理工具，您的电子书架

本软件用于管理实体书籍，将书籍扫描添加到本应用中，即可记录书籍信息、笔记、阅读状态、标签等，完善的搜索功能，让您不再需要翻箱倒柜，不再错买同样的书籍，轻松管理多个实体书架。
本应用在 GPL v3 协议下开放全部源代码。

爱范儿的推荐文章: [http://www.ifanr.com/app/798137]

[http://www.ifanr.com/app/798137]:http://www.ifanr.com/app/798137

特点：
- Material Design 界面设计
- 没有多余的社交等功能，专为管理书籍而生
- 完全开源，无隐私忧虑
- 支持单个、批量、手动等多种模式添加书籍
- 支持自动从网络服务中获取书籍详情*
- 支持导出书单为 .csv 文件
- 完善的标签、书架系统
- 支持数据备份与还原
- 无广告

*目前仅支持 Douban.com 和 OpenLibrary.org, 后续会加入更多网络服务支持

## 更新日志

### 2020-10-19 v1.6 更新

1. 修复无法从豆瓣获取信息的问题
2. 支持显示每个书架上的图书数目
3. 优化备份/恢复流程，不再需要“存储”权限
4. 问题修复和性能提升

### 2019-12-16 v1.5 更新

1. 问题修正和性能提升


### 2019-03-30 v1.5 更新

1. 问题修正和性能提升


### 2018-05-19 v1.4 更新

1. 根据 Google 要求添加隐私政策说明
2. 移除捐赠功能
3. 更新依赖库、targetSdk 版本
4. 问题修正和性能提升

本次更新主要为遵守 Google Play 政策的维护性更新。


### 2017-03-11 v1.3 更新

1. 支持 App shortcuts (7.1+)
2. 支持导出为 .csv 文件
3. 支持手动更换书籍封面
4. 支持手动添加书籍
5. 捐赠方式优化
6. 新增使用条款
7. 错误修正和性能提升

### 2017-02-19 v1.2 更新
1. 支持 Android Auto Backup for Apps 特性 (6.0+)
2. 新增 OpenLibrary.org 的 API
3. 问题修正与性能提升

### 2017-02-18 v1.1 更新
1. 新增“备份与还原”功能 
2. 问题修正与性能提升

### 2017-02-09 v1.0 发布
最初版本



## 应用截图

![Screenshot1](\img\2017-02-09\1.png)
![Screenshot2](\img\2017-02-09\2.png)
![Screenshot3](\img\2017-02-09\3.png)
![Screenshot4](\img\2017-02-09\4.png)
![Screenshot5](\img\2017-02-09\5.png)


## 开源项目

本应用在 GPL v3 协议下开放全部源代码，您可以从[我的 Github Repository]获取代码。

[我的 Github Repository]:https://github.com/smartjinyu/MyBookshelf

## 应用下载

[Google Play]

[酷安]

[Google Play]:https://play.google.com/store/apps/details?id=com.smartjinyu.mybookshelf
[酷安]:http://coolapk.com/apk/com.smartjinyu.mybookshelf