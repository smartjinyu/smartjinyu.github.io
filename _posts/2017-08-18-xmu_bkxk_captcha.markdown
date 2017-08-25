---
layout: post
title:  "识别厦门某校选课系统验证码的一个思路(Tesseract + TensorFlow)"
date:   2017-08-18 01:00:00
categories: Machine Learning
excerpt: 识别厦门某校选课系统验证码
---

## 前言

曾经写过一个[脚本]，在先到先得的选课阶段循环 Post 数据来抢课。这次选课想写个监控工具，某些课程一旦有人退课，立即自动补选该课程，为此需要识别[选课系统]的验证码，正好学了点 Machine Learning 的内容，权当半个 Course Project 吧。本项目基于 Python 3.5.4 编写。

[脚本]:https://smartjinyu.com/python/2016/09/15/XMU_BKXK.html
[选课系统]:http://bkxk.xmu.edu.cn/xsxk/login.html

## 识别过程

### 需求分析

稍微分析了一下网站代码，似乎没有明显的漏洞可以绕过验证码，网站的验证码图片如下，主要的障碍在于两条干扰线，而每个字符的位置和扭曲程度都相对固定，无需太多处理。稍加验证可以发现，此网站的验证码只有数字023456789(没有数字1)，且均为黑白图片。

![sample1](\img\2017-08-18\sample1.jpg)
![sample2](\img\2017-08-18\sample2.jpg)
![sample3](\img\2017-08-18\sample3.jpg)


### 图片预处理

预处理阶段的主要任务是干掉那两条干扰线。由于干扰线和数字的粗细相仿，使用判断相邻像素的滤波法可能导致误判。个人采取的思路是求出两条干扰线的方程后消除干扰线，要注意在干扰线与数字的交叉处应当保留，否则会破坏数字的图形。求干扰线方程，可以用 Hough 变换来实现，在本例中，由于直线的起点和终点固定，直接在第一个 column 和倒数某个 column 遍历即确定端点。消除干扰线后即可轻松地把验证码分割为四张图片，每张一个数字。

预处理后的效果如下(分割前)：

![sample1_processed](\img\2017-08-18\sample1_pro.jpg)
![sample2_processed](\img\2017-08-18\sample2_pro.jpg)
![sample3_processed](\img\2017-08-18\sample3_pro.jpg)

这部分的代码可以参见 [processImg.py]

[processImg.py]:https://github.com/smartjinyu/xmuBKXK_captcha/blob/master/processImg.py

### 使用 Tesseract 获取标记数据

把分割后的图片丢进 Tessoract，限定 whitelist 为 023456789，本以为验证码识别到这里就完成了，但是测试下来识别率只有 60.0% 左右，也就是单字符识别率 88.0%。虽然在实际应用时问题不大，但是本着探(zhe)索(teng)的精神，希望能通过 Machine Learning 的方法，把识别率提高到 95% 以上。

虽然 Tesseract 的识别率堪忧，但是通过把识别结果 POST 到选课系统网站尝试登录，根据网站返回的结果，可以判断是否识别正确。利用这一点我们可以自动获取足够的 labeled data 供 ML 进行学习。我大概跑了两天，抓取了 27w 左右标记过的验证码，按照 4:1 的比例划分 training set 和 validation set 的话，每个字符能有 8w 以上的训练样本。

![tesseract_result](\img\2017-08-18\tesseract_result.png)

这部分代码可以参见 [downloadCaptchas.py]

[downloadCaptchas.py]:https://github.com/smartjinyu/xmuBKXK_captcha/blob/master/downloadCaptchas.py

### 数据处理

在获取标记的验证码后，我们使用 [processImg.py] 将数据分割为单个字符，并分成 training set 和 validation set。请注意大量的小文件会占用巨大的磁盘空间，并且我建议在 SSD 上进行数据存储和处理以加快小文件的访问速度。然后使用 [build_tfrecords.py] 脚本，将数据转化为 TFRecords 文件，这是 TensorFlow 的标准数据文件格式。


[processImg.py]:https://github.com/smartjinyu/xmuBKXK_captcha/blob/master/processImg.py

[build_tfrecords.py]:https://github.com/smartjinyu/xmuBKXK_captcha/blob/master/build_tfrecords.py

### 训练 TensorFlow 模型

Machine Learning 的框架我选的是 TensorFlow，有 Google 爸爸这条大腿，文档齐全，社区活跃，将来少不了跟它打交道。这两天花了不少时间看官方文档和例程，弄懂了其基本的使用方法。在本例中，我们参考 MNIST 样例，使用前面准备好的 TFRecords 文件作为输入样本，因为 CNN (Convolutional Neural Network) 尚未学过，采用较为简单的 `softmax` 函数作为 cost function，只要样本量充足，效果还是令人满意的。

![model_code](\img\2017-08-18\model_code.png)

在训练过程中，在 steps = 1000 之后，loss 趋向于 0，validation accuracy 基本固定在1.0。保存训练好的 model，登录选课系统进行实验，几次实验的识别率都在 99.9% 以上，速度也快于 Tesseract，单就识别效果而言令人相当满意。

![TensorFlow](\img\2017-08-18\tensorflow_result.png)

这部分训练模型的代码可以参见 [train_models.py]

[train_models.py]:https://github.com/smartjinyu/xmuBKXK_captcha/blob/master/train_models.py

## 开源项目

本项目遵循 Apache v2 License 开放全部源代码，你可以在我的 [GitHub Repository] 获取代码，并按照 README 的步骤自行训练 model。如有任何建议或问题，请不吝提出 issues 或 PR。

同时，GitHub Repository 里面已经包含一份我训练好的 model. You can use it out of box. 调用只需一条命令


    Import ocrCaptcha
    result = ocrRawCaptcha(Image)

效果如下
![sample_cmd](\img\2017-08-18\sample_cmd.png)

[GitHub Repository]:https://github.com/smartjinyu/xmuBKXK_captcha

## 参考资料
- [常见验证码的弱点与验证码识别 乌云知识库]
- [Tesseract 文档]
- TensorFlow [文档]和[示例]

[常见验证码的弱点与验证码识别 乌云知识库]:http://drops.wooyun.org/tips/141
[Tesseract 文档]:https://github.com/tesseract-ocr/tesseract
[文档]:https://www.tensorflow.org/get_started/
[示例]:https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/

## 后记
本项目完成后，实现前言中所说的自动捡漏工具已经没有技术难度，只是看过课表后，越发觉得厦门某校这些课程完全不值得我去写个工具抢。互联网上有许多更加优秀的学习资源，比如：

- [CS229 - Machine Learning by Andrew Ng, Stanford University]  大名鼎鼎的 ML 入门课，相比 Coursera 上那门极简版，这门课有完整的数学推导。此课程是 ML 领域绝大多数课程的先修课程，目前大约刷完一半。

- [CS231n - Convolutional Neural Networks for Visual Recognition by Fei-Fei Li, Stanford University] 讲 CNN 在 Computer Vision 中的应用，也是一门经典课程。我应当会在完成 CS229 后开始此课程。

- [Probabilistic Graphical Models, Coursera] 据说是 Coursera 上最早的三门课程之一，讲概率图模型，ML 的重要理论基础，刚刚开坑。

如果有同学愿意一起刷这些课程，我将非常高兴。当然如有大佬实现了那个自动捡漏工具，也请发我一份咯。

[CS229 - Machine Learning by Andrew Ng, Stanford University]:https://see.stanford.edu/Course/CS229

[CS231n - Convolutional Neural Networks for Visual Recognition by Fei-Fei Li, Stanford University]:http://cs231n.stanford.edu/

[Probabilistic Graphical Models, Coursera]:https://www.coursera.org/learn/probabilistic-graphical-models
