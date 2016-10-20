---
layout: post
title:  "博客启用评论功能啦！"
date:   2016-04-24 00:34:20
categories: Blog
author: smartjinyu
image: /img/2016-04-24/disqus.jpg
excerpt: 成功为大金鱼's Blog启用了评论功能（Powered By Disqus）！欢迎留下您的看法。
analytics: true
---
其实Disqus的评论功能已经内置在Material Design Lite的模板中，抽空去注册了账号，稍作修改，配置一下，启用了这个功能，你应该会在每篇文章的底部看到如下图所示的评论区：


![Disqus](/img/2016-04-24/comment.png)

不过Disqus在国内的某些地区被墙（比如厦门），因此您的浏览器中Comment下面空空如野。解决方案很简单，翻越那堵墙。使用VPN的同学可以直接使用VPN访问，使用Shadowsocks的同学，请在SwitchyOmega中添加"*.disquscdn.com"、"*.disqus.com"两条域名通配符规则，让它们通过Shadowsocks访问。刷新后便可以显示评论区。

![switchyomega](/img/2016-04-24/swtichyomega.png)

为什么明知被墙还选用Disqus呢？一方面，对国内的替代品如多说、友言有一种天然的恐惧与不信任，国外的产品相对自由一些；另一方面，墙的存在不是我们进行自我审查的借口，翻越它、无视它，访问这个博客的，翻越围墙应当是基本技能了吧，因此没有必要放弃国外的优秀产品。

###### 关于功能的一点说明：

- 评论支持Disqus、Facebook、Twitter、Google四种方式登录账号，如果出现登录失败，请尝试清空cookies。
- 评论同样支持匿名发表，在评论区，点击右侧"SIGN UP WITH DISQUS"后，勾选"I'd rather post as a guest"便可以匿名发表评论，不过根据Disqus的设置，匿名评论必须要经过我的审核才能显示，暂时没有找到关闭的选项。
- 留言板功能有空我会加上的，有问题欢迎在评论区提出。