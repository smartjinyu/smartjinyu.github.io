---
layout: post
title:  "Smartjinyu's Blog 全新改版啦！"
date:   2016-10-19 01:00:00
categories: others
excerpt: 涣然一新的博客上线啦！
---

由于种种原因，第一版的博客存在诸多缺陷，比如缺乏HTTPS加密、文章较多时浏览体验不佳等，这两天抽空对博客进行了改版，在界面美观度、易用性等方面应该会有很大提高。

![website](/img/2016-10-19/website.png)

#### 关于模板

![logo](/img/logo.png)

鉴于自己审美和美工方面极度欠缺，加上对Javascript、Html等前端的技能不熟悉，本次改版依照了DONGChuan的[Yummy-Jekyll]进行搭建，博客使用Jekyll生成，搭建在Github Pages上。在修改模板的过程中，遇到一些环境配置问题，解决过程参考了[此篇博客]，在此一并表示感谢。

[Yummy-Jekyll]:https://github.com/DONGChuan/Yummy-Jekyll
[此篇博客]:http://knightcodes.com/miscellaneous/2016/09/13/fix-github-metadata-error.html

#### 关于博客

![Guide](/img/2016-10-19/guide.png)

改版之后的博客不再采用原先的Material Design，以标题栏的形式将博客分为Home、Open-Source、Blog、About四个部分，其中Open-Source部分直接与[我的Github账号]联动，同时也在Home页面以侧边栏的形式展示Popular Repositories，感觉本渣的Github完全拿不出手，以后还当加倍努力。(又是Flag)

[我的Github账号]:https://github.com/smartjinyu

#### 关于HTTPS

基于数据传输的安全性、对抗运营商的流氓劫持、Google的结果排名等诸多因素的考量，本次改版为博客加上了HTTPS加密，并且强制启用。由于博客搭建在Github Pages上，在选择SSL证书方面存在诸多限制，目前的解决方案是使用Cloudflare的证书，Flexible方案免费，使用namesever将流量转到CF的服务器上，实现从客户端到CF服务器的加密，不过从CF的服务器到原始服务器这一段依旧是不加密的，基于博客现有的需要和技术限制，暂且认为可以接受。启用HTTPS的过程参考了CF的[Official Guide] and [This blog]。由于域名没有备案，无法使用CF与百度云合作的国内CDN节点，经测试，厦门移动会连接到Hong Kong的服务器，latency在50ms-80ms左右，尚可接受，但是电信会重定向到Los Angeles的CF服务器，延迟可能超过150ms，还望谅解。

![ssl](/img/2016-10-19/ssl.png)


[Official Guide]:https://support.cloudflare.com/hc/en-us/articles/201720164-Sign-up-planning-guide
[This blog]:https://sheharyar.me/blog/free-ssl-for-github-pages-with-custom-domains/

#### 感言

在前端方面我是彻头彻尾的菜鸟，虽然你看到博客相比原始模板没有太大的改变，但我也不得不试图理解整个网站的框架结构。几天的捣腾对Javascript、HTML、bower等大抵有了些了解，学习了SVG图标的相关知识，虽然还远谈不上入门了，但至少撩起了门帘。

改版初期本打算使用WordPress搭建一个全功能的动态网站，但是在选购VPS(现有的这台还有他用)、数据库管理、前端开发、网站的安全维护等方面感到力不从心，先使用比较省心的Github Pages，待以后才学有所长进后(点亮以下技能后)，再来完整地尝试建站。

目前正在点的技能:

- Computer Network
- DataBase and SQL
- Usage of Linux

将要点的技能:

- Javascipt
- HTML\CSS
- ETC..

(但愿以上不全是Flag)

如果你有好的建议或者发现的网站Bug，欢迎留言提出。
