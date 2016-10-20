---
layout: post
title:  "写了个名叫'厦门大学本科生绩点计算工具'的玩具"
date:   2016-09-09 03:38:20
categories: Python
author: smartjinyu
image: /img/2016-09-09/cover.jpg
excerpt: 学了点Python,没事写了个小工具玩
analytics: true
---

前天刷知乎，突然觉得爬虫很好玩，就花了一天学了学爬虫和Python的基础，有Request这种库(HTTP for Humans),简单的模拟登录、保存cookie等都不在话下(现成的轮子多就是爽)。
下图已笑疯:
![shot](/img/2016-09-09/requests.JPG)

想到敝校的那个破烂教务系统里只有科目的成绩，没有绩点，每次都得手动计算麻烦至极，就写了这个小工具，输入教务系统的账号密码后会打印所有主修科目的成绩、绩点，各个学期的绩点以及全部学期的总绩点。

![screenshot](/img/2016-09-09/screenshot.png)

没什么技术含量，练习一下Python的基础语法和简单的爬虫技巧罢了，倒是每次用自己的成绩去测试代码，低得可怜的绩点总是刺伤本学渣的内心。

在Ubuntu 16.04环境下测试通过，Windows下没作尝试，可能中文GBK编码的问题会导致乱码。

代码开源在[我的Github]上

[我的Github]:https://github.com/smartjinyu/XMUGPA


