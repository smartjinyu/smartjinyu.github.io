---
layout: post
title:  "关于隐写术(Steganography)的简单总结"
date:   2016-11-29 01:00:00
categories: CTF
excerpt: 玩了两天隐写术，对相关原理、工具的简单总结
---

十二月初在厦大有个网络安全方面的邀请赛，承蒙同学不弃，组队入坑。竞赛采用典型的CTF(Capture the flag) 竞赛模式，之前对此一无所知，这两天看了些隐写术的相关内容，感觉这方面成体系资料不多，简单总结一下，大神请飘过。

## 1. 入门介绍

- [Wikipedia Steganograhy 词条] 可以用来简单了解一下隐写术的原理和历史
- [隐写术总结 - 乌云知识库] 此文很好地结合了隐写术的基本原理和实战操作，推荐阅读(乌云网目前无法打开，使用网页快照或搜索转载文章即可)
- [实验吧课程-CTF新手入门之隐写术实例讲解] 几个很短的视频，可以观摩一下各种工具的使用
- [《数据隐藏技术揭秘》] 一本很薄的小书，介绍了隐写术的相关技术和工具，难度不大，比较系统，可以一读


[Wikipedia Steganograhy 词条]:https://en.wikipedia.org/wiki/Steganography
[隐写术总结 - 乌云知识库]:drops.wooyun.org/tips/4862
[实验吧课程-CTF新手入门之隐写术实例讲解]:http://www.shiyanbar.com/courses/detail/344.
[《数据隐藏技术揭秘》]:https://book.douban.com/subject/25837887/

## 2. 习题演练

可以找各种 CTF 竞赛的真题，也可以在攻防平台上练习，入门阶段我是在实验吧网站上进行的练习。

## 3. 常用工具

进行隐写文件的分析用到的工具大多比较小众，在此略作总结。此处仅列举本人目前使用过的工具，这个 [Github Repository] 里有更加完整的集合。

[Github Repository]:https://github.com/smartjinyu/ctf-tools

- **Winhex**、**UltraEdit**: 用于查看文件的二进制\十六进制代码
- **StegSolve**: 神器中的神器，能够对常见的图片格式进行偏移、LSB 提取、帧提取、像素偏移、数据提取，对两张图片进行结合等等，覆盖了常见的分析需求。[下载地址]，需要 Java 运行环境。
![StegSolve](\img\2016-11-29\stegsolve.jpg)

[下载地址]:http://www.caesum.com/handbook/Stegsolve.jar

- **Binwalk**: 常用工具，可以进行文件的分析和提取。使用说明可参看[这篇博客]，在 Linux 下运行 `sudo apt-get install binwalk` 即可安装。
![binwalk](\img\2016-11-29\binwalk.png)

  - `binwalk filename` 分析文件构成
  - `binwalk -e filename` 自动解压已知文件格式
  - `binwalk -D=[extension] filename` 根据后缀名解压，如 `-D=jpeg` 等 [例题]

[这篇博客]:http://www.freebuf.com/sectool/15266.html
[例题]:http://www.shiyanbar.com/ctf/1766

- **Stegdetect**: 用于检测 JPEG 文件中是否包含隐藏内容并尝试分析隐藏内容通过哪个隐写工具嵌入，官网打不开，网上找到一份[编译好的 Windows 版本]。使用方式为：
    - `stegdetect [-nqV] [-s <float>] [-d <num>] [-t <tests>] [file.jpg ...]`
    - -q 仅显示可能包含隐藏内容的图像。
    - -n 启用检查 JPEG 文件头功能，以降低误报率。如果启用，所有带有批注区域的文件将被视为没有被嵌入信息。如果 JPEG 文件的 JFIF 标识符中的版本号不是1.1，则禁用 OutGuess 检测。
    - -s 修改检测算法的敏感度，该值的默认值为1。检测结果的匹配度与检测算法的敏感度成正比，算法敏感度的值越大，检测出的可疑文件包含敏感信息的可能性越大。
    - -d 打印带行号的调试信息。
    - -t 设置要检测哪些隐写工具（默认检测 jopi），可设置的选项如下：
        - j 检测图像中的信息是否是用 jsteg 嵌入的。
        - o 检测图像中的信息是否是用 outguess 嵌入的。
        - p 检测图像中的信息是否是用 jphide 嵌入的。
        - i 检测图像中的信息是否是用 invisible secrets 嵌入的。
    - -V 显示软件版本号。

[编译好的 Windows 版本]:http://ftp.mirrorservice.org/sites/ftp.wiretapped.net/pub/security/steganography/stegdetect/

- **Outguess**: 用于提取 JPEG 文件中使用 Outguess 算法的加入的隐藏信息，官网挂了一堆代理都打不开，网上找到一份编译好的[ Windows 版本]。常用命令：
    - `./outguess -r test.jpg output.txt`  用于还原 JPG 图片中的隐藏信息 [例题]

[ Windows 版本]:http://www-uxsup.csx.cam.ac.uk/pub/windows/cygwin/x86/release/outguess/
[例题]:http://www.shiyanbar.com/ctf/1931

- **JPHS**: 用于对 JPEG 文件进行 Jhide 算法的隐写或提取，详细介绍及软件下载可以参见[这篇博客]。

[这篇博客]:https://www.hackfun.org/CTF/jphide-steganography.html

-  **MP3Stego**: 用于对 MP3 音频文件进行隐写、提取等操作，下载及使用[参见此网页]，常用命令：
    - `encode -E hidden_text.txt -P pass svega.wav svega_stego.mp3` 用于写入隐藏信息
    - `decode -X -P pass svega_stego.mp3` 用于提取隐藏在音频文件中的信息 [例题]

[参见此网页]:http://www.petitcolas.net/steganography/mp3stego/
[例题]:http://www.shiyanbar.com/ctf/58

- **MSU Stego**: 用于对 AVI 文件进行隐写\提取操作，介绍见[官网]

[官网]:http://www.compression.ru/video/stego_video/index_en.html

- **其他工具**
    - **QR Reader**: 用于在 Windows 下进行 QR Code 的扫描，可以自定义参数
    - **Advanced Archive Password Recovery**: 爆破压缩文档密码
    - **[MD5 解密]**: 在线工具，用于解密 MD5 字符串
    - **[在线加密解密]**: 亦是在线工具，支持各种加密协议


[MD5 解密]:http://www.dmd5.com/md5-decrypter.jsp
[在线加密解密]:http://encode.chahuo.com/

## 4. 值得注意的题目

太过基础的题目不再列举，此处记录一些我认为值得注意的题目，大多比较简单。

### 4.1 JPG 文件格式

一个完整的 JPG 文件由 `FF D8` 开头，`FF D9`结尾，图片浏览器会忽略 `FF D9` 以后的内容，因此可以在 JPG 文件中加入其他文件。 `Binwalk` 中提到的那个例题可以尝试不适用 `Binwalk` 提供的自动分拆功能，而使用 `Winhex` 手动分解。

### 4.2 Gif 文件格式

详细格式可以参阅 [GIF 文档]，注意开头的 `GIF8` 四位，例题可见1中给出的隐写术总结中的相关内容。

[GIF 文档]:http://dev.gameres.com/Program/Visual/Other%20/GIFDoc.htm

### 4.3 Zip 伪加密

有时候得到一个加密的 ZIP 文件，但没有任何关于密码的线索，可以考虑是否为伪加密的 ZIP 文件，[此题]即是很好的例子，相关说明可见[这篇博客]。

[此题]:http://www.shiyanbar.com/ctf/716
[这篇博客]:http://blog.csdn.net/etf6996/article/details/51946250

### 4.4 双图问题

对于两张图片的问题，可用 `StegSolve` 对双图进行各种操作(SUB、XOR、AND)等等，看是否获取有用信息，可能与加解密、二维码等综合。[例题]。

[例题]:http://www.shiyanbar.com/ctf/1926

### 4.5 F5 隐写术

一种隐写算法，可以由 `Stegdetect· 检测出，[Github] 上有工具提取密文，也可参看[此例题]。

[Github]:https://github.com/matthewgao/F5-steganography
[此例题]:http://www.shiyanbar.com/ctf/1938

### 4.6 加密算法

有时候分析出来的 flag 经过加密，简单的 Base64、MD5 等不再赘述，[这篇博客]比较全面地总结了隐写术中用到的加密算法，可以参看。文中还提供了各种加密方式的解密工具。

[这篇博客]:http://www.secbox.cn/hacker/ctf/8078.html

## 5. 简单总结

看起来隐写术是个非常有趣的智力游戏，涉及密码学、文件格式、编程能力等的综合运用，目前我也仅在入门阶段，上述内容是个极简略极浅显的总结，适用于初学者快速入门，很多内容比如编程解法、Photoshop 解法、视频隐写等均未涉及，随着钻研的深入，我会逐步加入以上内容的总结。