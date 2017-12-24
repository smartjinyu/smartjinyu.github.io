---
layout: post
title:  "我的 CS231n(Spring 2017) 学习之路"
date:   2017-12-23 01:00:00
categories: Machine Learning
excerpt: 学习 CS231n 的一个总结
---

## 前言

Stanford University 的 [CS231n: Convolutional Neural Networks for Visual Recognition] 是大多数深度学习的入门者都会参观膜拜的课程。本学期我大约花费九周的闲暇时间，粗略看过 Spring 2017 版本的 CS231n，受益匪浅。仅以此文作为学习过程的简单回顾和对这门课程的致谢。

[CS231n: Convolutional Neural Networks for Visual Recognition]:http://cs231n.stanford.edu/

## 声明

本文的图片大多撷取自 CS231n 课程网站、Slides 或本文所提及的论文之中，版权归属原作者所有，如需使用请向原作者获取授权。

## 课程介绍

如课程名称所述，此课程讲述当前大火的 Neural Network 在 Computer Vision 领域的应用，教师团队由 Fei-Fei Li 女神和她的两位 Phd Justin Johnson 和 Serena Yeung 组成（然而 Fei-Fei 只在第一节课出现过）。课程的所有资料包括 lecture videos 均可在课程主页和 [YouTube] 上获取。除课堂讲授外，设计良好的 Programming Assignements 也迫使我亲手实现一些看起来容易、实则需要处处留意的功能(比如看似 trivial 的 Backpropgation)。课程的 Slides 和 Notes 及其中提及的论文，亦是 Computer Vision 各个细分领域良好的入门读物。

![Instructors And TAs](\img\2017-12-24\instructors&TAs.png)
(图片改编自 CS231n 课程网站)

按照课程的 [Syllabus] 和三个 Assignment 的安排，我把这门课程分为三个部分，Lecture 1 - 3 介绍了简单的背景知识，Lecture 4 - 9 为对 Convolutional Neural Network 的详细说明，Lecture 10 - 16 则是 CNN 更广泛的应用。

我的 Assignment 使用 GPL v3 协议开源在[我的 GitHub] 上，欢迎参考交流。

[YouTube]:https://www.youtube.com/playlist?list=PL3FW7Lu3i5JvHM8ljYj-zLfQRF3EO8sYv
[Syllabus]:http://cs231n.stanford.edu/syllabus.html
[我的 GitHub]:https://github.com/smartjinyu/CS231n_assignments

### Part 1 Lecture 1 - 3

第一部分为 Deep Learning 背景知识的简单介绍，除了课程信息外，介绍了 softmax 和 svm 两种 loss function、SGD 等 Optimization 方法，以及 K-Nearest Neighbor 和 Linear Classifier 两种简单的 Image Classifier。这部分内容如果有 Andrew Ng 的 [CS229: Machine Learning] 作为先修课程的话，应当相当熟悉，仅仅是个回顾而已。

![Part 1](\img\2017-12-24\part1.png)
(图片撷取自 CS231n 课程网站)


[Assignment 1] 要求实现 KNN、Softmax、SVM 和一个 Two-layer 的 Neural Network，难度不大，可以用来熟悉一下 Python/Numpy + Jupyter 的编程环境。

[CS229: Machine Learning]:https://see.stanford.edu/Course/CS229
[Assignment 1]:http://cs231n.github.io/assignments2017/assignment1/

### Part 2 Lecture 4 - 9

第二部分详细介绍了本课程的主角 —— Convolutional Neural Network，包括每个 layer 的作用(Conv layer, Pooling layer, FC layer 等)、Backpropagation 的计算、优化的训练方法(如 Adam, Momentum, Dropout, Batch-Normalization 等)、训练 CNN 的注意事项(如 paramters 的 initialization, tune hyperparameters 的方法等)，内容相当详尽。在每个 Lecture 之后都提供了相应的 Notes，基本涵盖了 Lecture 里提到的所有内容并加以补充。Lecture 8 介绍了常见的 Deep Learning Framework(如 TensorFlow, Caffe, Pytorch等)， Lecture 9 介绍了几个著名的 CNN Architecture(AlexNet, VGGNet, GoogLeNet, ResNet)，大多为历年 ILSVRC 竞赛的获胜者。

![Part 2](\img\2017-12-24\part2.png)
(图片撷取自 CS231n 课程网站)


本部分阅读了如下几篇论文(这部分的 Notes 相当详尽，因此这部分仅读了很少的几篇文章)：
- [Alex Krizhevsky, Ilya Sutskever, Geoffrey E. Hinton:ImageNet classification with deep convolutional neural networks]

    这篇论文提出了 AlexNet 的 CNN Architecture，在 ILSVRC-2012 中吊打传统 ML 方法近十个百分点，引爆了2013年以来的这波 Deep Learning 的热潮，以后几乎每篇 CNN 方面的论文都会在第一部分致敬一下。虽然文中的大多数方法今天已经很常见，但是此文必须得膜拜一番。

- [Sergey Ioffe, Christian Szegedy: Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift]

    本文引入了 Batch Normalization 方法，在训练过程中在某些 layer 后对 activation 进行 Noramlization (和 scale and shift)，可以减轻 Backpropagtion 过程中的 Gradient Vanishing or Exploding 现象。在 PA2 中可以发现，引入 BN Layer 后，CNN 对于 parameters 的 initial value 要求大大下降。

- [Nitish Srivastava, Geoffrey Hinton: Dropout: A Simple Way to Prevent Neural Networks from Overfitting]

    本文中提出了名叫 Dropout 的方法，即在训练时随机关闭一些 neuron，这样做一方面减轻了 overfit 现象，另一方面相当于一次性训练了多个结构相似的 CNN 在 test time 进行平均，可以将 ImageNet 上的识别率提升3%左右。

- [Christian Szegedy, Wei Liu, Yangqing Jia: Going Deeper with Convolutions]

    本文提出了名叫 GoogLeNet 的 CNN Architecture,为 ILSVRC-2014 的冠军。本文除了关心 accuracy 外，也格外关注计算量的减少，为此提出了 Inception modules,并以此为基础组合得到了 GoogLeNet。

其他文章诸如 Adam 方法、ResNet 等只是粗略扫过，没有认真细读，故不在此列举。


[Alex Krizhevsky, Ilya Sutskever, Geoffrey E. Hinton:ImageNet classification with deep convolutional neural networks]:https://dl.acm.org/citation.cfm?id=2999257

[Sergey Ioffe, Christian Szegedy: Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift]:https://arxiv.org/abs/1502.03167

[Nitish Srivastava, Geoffrey Hinton: Dropout: A Simple Way to Prevent Neural Networks from Overfitting]:http://jmlr.org/papers/v15/srivastava14a.html

[Christian Szegedy, Wei Liu, Yangqing Jia: Going Deeper with Convolutions]:https://research.google.com/pubs/pub43022.html



[Assignment 2] 要求实现 Fully-Connected Network, Convolutional Neural Network, Batch-Normalization Layer, Dropout 等。作业中使用 Numpy 写每一种 layer (affine, fc, conv, max_pool, dropout, bn 等等) 的 forward 和 backward 过程，然后组合成常见的 layer_utils 再组合成简单的 CNN，并且体验一番玄学的调参。

虽然这部分内容看 Lecture 和 Notes 大都不算难，但是实现起来还是有不少坑要趟。比如 Backpropagation 过程，对于数学系的学生，求个导数应用链式法则当是小菜一碟，但是真正代码实现的时候却很容易出现各式各样的问题；有些内容搞懂原理，进行数学推导都不难，但是要用 vectorized 的方法实现又要花费很多脑筋。

个人强烈建议 PA2 要花些时间认真完成，对于透彻理解 CNN 的细节大有裨益。每个小部分 TA 都设置了 Numerical check，帮助检查代码实现是否正确。

另外，PA2 的最后一部分需要选择 TensorFlow 或 Pytorch 完成。个人选择的是 TensorFlow，初学阶段对于 tf 这种先构造 Computaion Graph 再执行的方式很不习惯，不过熟悉之后相比用 Numpy 手写简直太幸福了。

[Assignment 2]:http://cs231n.github.io/assignments2017/assignment2/

### Part 3 Lecture 10 - 16

第三部分为 CNN 的应用，不少内容是当前研究的前沿领域，这部分在 Spring 2017 版本的 CS231n 中得到了幅度不小的更新。到此为止不再像之前一样有详尽的 Notes 提供，有疑问只能自己啃论文了。这部分花费了我相当多的时间(其中被一个长达五周的期中考试阻隔)，值得进行一个相对详尽的总结。

#### Lecture 10 Recurrent Neural Networks 

本讲介绍 RNN 及其变种 LSTM，并介绍它们在 Image Captioning 中的应用。RNN 相比普通的 CNN 引入了时间序列，每一个 time step 的输入既有来自外界的输入，又有来自上一个 time step 的输出。RNN 可以用来处理 sequence data，被应用于 Natural Language Translation, Image Captioning, Video Captioning 等领域。而 LSTM 则在 RNN 的基础上，改进了 RNN 的 Gradient Vanishing/Exploding 问题。 在 [Assignment 3] 中有使用 RNN/LSTM 进行 Image Captioning 的任务。

本部分读论文一篇：

- [Zachary C. Lipton, John Berkowitz, Charles Elkan: A Critical Review of Recurrent Neural Networks for Sequence Learning]

    非常好的综述文章，详尽介绍了 Vanilla RNN, LSTM 和 BRNN 的架构及应用，对初学者相当友好。

[Zachary C. Lipton, John Berkowitz, Charles Elkan: A Critical Review of Recurrent Neural Networks for Sequence Learning]:https://arxiv.org/abs/1506.00019

[Assignment 3]:http://cs231n.github.io/assignments2017/assignment3/

#### Lecture 11 Detection and Segmentation 

本讲介绍 Object Detection, Localization, Segamentation 等，讲述了 Transpose Convolution、R-CNN、Fast/Faster R-CNN 等架构。因为在 PA3 中没有要求，故暂时没有深入钻研。

#### Lecture 12 Visualizing and Understanding 

本讲介绍 Feature Visualization(包括 Saliency maps, Class Visualization 等)、Adversarial Examples 和 Style Transfer，内容量很大，且这三个部分在 PA3 中都有相应的代码需要完成。

Feature Visualization 部分读论文三篇：

- [Matthew D Zeiler, Rob Fergus: Visualizing and Understanding Convolutional Networks]

    本文使用 Deconvolution Network 进行 Feature Visualization，并在 ImageNet 上进行测试并对结果进行了说明。

- [Jason Yosinski, Jeff Clune, Anh Nguyen, Thomas Fuchs, Hod Lipson: Understanding Neural Networks Through Deep Visualization]

    本文中提供了一个软件可以实时地对 CNN 进行 Visualization，并比较几种不同的 regularization 方法及其效果。

- [Karen Simonyan, Andrea Vedaldi, Andrew Zisserman
:Deep Inside Convolutional Networks: Visualising Image Classification Models and Saliency Maps]

    本文提出了叫做 Saliency Map 的模型，属于 gradient-based 方法。通过计算经过 CNN 后的 scores 关于初始图片每个 pixel 的导数，并在每个像素点取各个 channel 的最大绝对值获得 Saliency Map。所得结果直观地表示了初始图像的哪些部分对于 CNN 判断其所属的 class 有较为的重要的作用。
    ![Saliency Map](\img\2017-12-24\saliency_map.png)
    (图片撷取自本论文)

这部分似乎更加偏重于 intuition 而不是严格证明，可能是我学力有限的缘故，总觉得读过论文后不少地方将信将疑。但无论如何，Feature Visualization 为研究被称为 black-box 的神经网络提供了一个思路。

Adversarial Examples 部分读论文一篇：

- [Christian Szegedy, Wojciech Zaremba, Ilya Sutskever: Intriguing properties of neural networks]

    在本文中作者提出了存在让 CNN misclassify 的 adversarial examples，即对于一张 predicted correctly 的图像，加上一个精心构造的微小"noise"(变化常常是肉眼不可见的)，就能使 CNN 给它一个错误的 label，甚至给出较高的 confidence。作者提出这种现象对于不同 Training set 和不同架构的 CNN 都普遍存在，可能 CNN 的一个 flaw。PA3 中有一个作业通过 Gradient Ascent 的方法构造这样的样本。
    ![Adversarial Examples](\img\2017-12-24\adversarial_examples.png)
    (图片撷取自本论文)

Style Transfer 部分读论文两篇：

- [Leon A. Gatys, Alexander S. Ecker, Matthias Bethge: Texture Synthesis Using Convolutional Neural Networks]

    本文介绍了利用 CNN 进行 Texture Synthesis, 即把图片的具体内容丢弃而保留图片的样式，即“舍其形而取其神”。具体方法是将原图A和 white noise B 送入同一个 CNN，计算每一层 feature 的 Gram Matrix，取两者之差的 Frobenius Norm 进行加权平均，使用 Gradient descent 更新图片B得到 Texture。

- [Leon A. Gatys, Alexander S. Ecker, Matthias Bethge: Image Style Transfer Using Convolutional Neural Networks]

    本文为上一篇论文的延续，作者提出可以用 CNN 实现将图片A的 Style 和图片 B 的 Content 结合在一起，得到一张新的图片，“取他人之神成我之形”。方法是除了上文中提到的 Style 的 loss 外，再引入一个 Content 的 loss(即每层 featrue map 之差的 Frobenius Norm)，并将他们加权平均，同样使用 SGD 进行训练。使用这种方法可以得到许多 incredible 的图片。
    ![Style Transfer](\img\2017-12-24\style_transfer.png)
    (图片撷取自本论文)

PA3 中有实现 Style Transfer 的任务。另外上文中提出的 Style Transfer 方法需要相当大的计算量，无法用于 interactive application，而 Justin Johnson (本课程的讲师之一)等提出了[基于 Perceptual Loss 的方法]，可以极大地提升计算效率。

[Matthew D Zeiler, Rob Fergus: Visualizing and Understanding Convolutional Networks]:https://arxiv.org/abs/1311.2901

[Jason Yosinski, Jeff Clune, Anh Nguyen, Thomas Fuchs, Hod Lipson: Understanding Neural Networks Through Deep Visualization]:https://arxiv.org/abs/1506.06579

[Karen Simonyan, Andrea Vedaldi, Andrew Zisserman
:Deep Inside Convolutional Networks: Visualising Image Classification Models and Saliency Maps]:
https://arxiv.org/abs/1312.6034

[Christian Szegedy, Wojciech Zaremba, Ilya Sutskever: Intriguing properties of neural networks]:
https://arxiv.org/abs/1312.6199

[Leon A. Gatys, Alexander S. Ecker, Matthias Bethge: Texture Synthesis Using Convolutional Neural Networks]:
https://arxiv.org/abs/1505.07376

[Leon A. Gatys, Alexander S. Ecker, Matthias Bethge: Image Style Transfer Using Convolutional Neural Networks]:
http://ieeexplore.ieee.org/document/7780634/

[基于 Perceptual Loss 的方法]:https://arxiv.org/abs/1603.08155

#### Lecture 13 Generative Models 

本讲介绍 Generative Models，属于 Unsupervised Learning，根据是否显式地求出 Model Density，又有如下分类：
![Generative Models](\img\2017-12-24\generative_models.png)
(图片撷取自课程 Slides，原始出处见右下角)

本讲主要介绍 PixelRNN/PixelCNN, Variational Autoencoder 和 GANs(Generative Adversarial Nets)。由于目前个人只学习过基本的概率论课程，数理统计、随机过程、高等概率论等都没有修习，加上最近时间有限，这部分仅以完成 PA3 的 GAN 部分为目标，没有细究。

本部分读论文四篇：
- [Ian J. Goodfellow, Jean Pouget-Abadie, Mehdi Mirza: Generative Adversarial Networks]
    
    本文提出了 GAN 模型，即网络中由一个 Generator 生成与原始数据分布相似的样本，一个 Discriminator 负责区分 Generator 生成的样本和真实样本，通过求解这样一个 min-max game 来让 Generator 生成的样本尽可能与原始数据拥有相同的分布。
    
    ![Vanilla GANs](\img\2017-12-24\vanilla_gans.png)
    (图片撷取自本论文)

    这篇论文引发了近几年大热研究的 GAN 的热潮，由于 GAN 训练困难，故后来提出了各种各样的改进模型及训练方法。

- [Xudong Mao, Qing Li, Haoran Xie, Raymond Y.K. Lau: Least Squares Generative Adversarial Networks]
    
    本文提出了 Least Squares GANs，即将 loss function 修改如下
    ![LSGANs](\img\2017-12-24\LSGANs.png)
    (图片撷取自本论文)
    作者认为这样的修改会使得训练过程更加稳定，最后生成的图片也更加 realistic。

- [Alec Radford, Luke Metz, Soumith Chintala: Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks]

    本文提出了 DCGANs 模型，即在某些策略下将 CNN 用于 GANs 以获取更好的结果，并且支持 vector arithmetic。

- [Ian Goodfellow: NIPS 2016 Tutorial: Generative Adversarial Networks]

    本文是 Goodfellow (提出 GANs 的那位大牛) 在 NIPS 2016 上所做的 Tutorial，虽然长达五十多页，但是读起来比论文轻松很多，详细地比较了其他 Generative Models 与 GANs 的优缺点、对 GANs 的详细介绍、训练 GANs 的 Tips 以及回答一些 FAQ，非常值得一读。


到目前为止 GANs 还是一个大火的研究领域，每年都有大量新的模型结构、训练方法等提出，时间有限，只能浅尝辄止了，下面两个 GitHub Repository 整理了 GANs 的模型和论文，有兴趣的可以一看: [the-gan-zoo],[AdversarialNetsPapers].


[Ian J. Goodfellow, Jean Pouget-Abadie, Mehdi Mirza: Generative Adversarial Networks]:https://arxiv.org/abs/1406.2661

[Xudong Mao, Qing Li, Haoran Xie, Raymond Y.K. Lau: Least Squares Generative Adversarial Networks]:https://arxiv.org/abs/1611.04076

[Alec Radford, Luke Metz, Soumith Chintala: Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks]:https://arxiv.org/abs/1511.06434

[Ian Goodfellow: NIPS 2016 Tutorial: Generative Adversarial Networks]:https://arxiv.org/abs/1701.00160

[the-gan-zoo]:https://github.com/hindupuravinash/the-gan-zoo

[AdversarialNetsPapers]:https://github.com/zhangqianhui/AdversarialNetsPapers

#### Lecture 14 Deep Reinforcement Learning 

本讲介绍 Deep Reinforcement Learning，部分内容同 Andrew Ng 的 CS229 中的 Reinforcement Learning 相仿。基于前面介绍的原因，暂时只看过 lecture，有个大概印象，没有深入钻研。

#### Lecture 15 Efficient Methods and Hardware for Deep Learning

本讲是 Invited Talk，讲者是 [Song Han] 老师，本科毕业于北京清华大学(非 native speaker 还是很容易听出来的)，在 Stanford University 获得 MS 和 Phd 学位，Lecture 中提到的 Deep Compression 方法、EIE 处理器等都由他提出。

本讲主要介绍如何高效地设计、训练神经网络，从算法上的减少参数、减少浮点数位数等，到硬件上 GPU 和专用处理器的改进设计等，都做了简要的介绍，对于 Nvidia 从 kepler 到 Maxwell 到 Pascal 到 Volta 历代 GPU 介绍、Google TPU v1 和 Cloud TPU 的讲解尤其让人兴奋(呼唤我攒钱买丹炉)。

本部分读论文一篇：

- [Forrest N. Iandola, Song Han, Matthew W. Moskewicz: SqueezeNet: AlexNet-level accuracy with 50x fewer parameters and <0.5MB model size]

    本文提出了 SqueezeNet 模型，从设计之处就考虑减少参数数量，通过结合 1*1 conv、在 input channel 上使用 3*3 的 filter、延迟 downsample 的时间，得到的模型拥有与 AlexNet 相仿的 accuracy，但是只用了 50x fewer 的 parameters，如果再结合 Deep Compression 方法,则可以达到 510x fewer，也就是不到 0.5MB 的 model size。

[Song Han]:https://stanford.edu/~songhan/
[Forrest N. Iandola, Song Han, Matthew W. Moskewicz: SqueezeNet: AlexNet-level accuracy with 50x fewer parameters and <0.5MB model size]:https://arxiv.org/abs/1602.07360

#### Lecture 16 Adversarial Examples and Adversarial Training

本讲依旧是 Invited Talk，讲者是 Ian Goodfellow 大牛。讲 Adversarial Examples 的成因, Adversarial Attack 等，可参考他的 [Explaining and Harnessing Adversarial Examples] 这篇论文。

[Explaining and Harnessing Adversarial Examples]:https://arxiv.org/abs/1412.6572

## 后记
![Assigments](\img\2017-12-24\assignments.png)

作为一门面向本科生的深度学习的入门课程，CS231n 确实不负盛名，从 Lecture 到 Notes 到 Assignment 都做了精心地准备，除了基本的 CNN 外，CNN 的众多应用也做了简要的介绍，有兴趣的人完全可以寻找其中一个方向深入钻研下去。虽然花了我将近一个学期的时间学习(有这学期课业太忙的缘故)，但绝对物有所值。

作为数学专业的本科生，专业课所学的内容大多在十八十九世纪就已定型，遇到问题的第一反应是找一本更好的教科书，但是这一轮的深度学习热潮从2013年才爆发，大多数领域的研究还在如火如荼地进行，除了 Goodfellow 的 [Deep Learning] 等少数几本外，还没有得到大家认可的教科书，因此有疑问只能依靠自己啃论文来解决。同样由于这个领域的年轻，许多学界、业界的最新成果，在原理上并没有超出课程所讲的内容太多，完全可以自己找来开开眼界，这在数学领域是很难想象的。

相比在 Stanford University 现场聆听这门课程，我的学习过程中缺少了 Midterm 和 Final Exam，也没有机会做 Course Project。写这篇博客的过程中，把大多数的 Slides,Notes 和论文都做了简要的回顾，权当一次复习。

由于个人学力有限，许多论文中的细节不能非常精准地掌握，待我修习过大多数的统计学课程后再来回顾此课程，应当会有不同的感受。

[Deep Learning]:http://www.deeplearningbook.org/

## 致谢

写此文的另一目的，是要表达对 Stanford University 和 CS231n 课程团队(Instructor, TAs, Invited Speakers, Students) 的感谢，他们将课程资料完整免费地在互联网上公开，让我这个地处中国东南小岛上的学生也可以领略到优秀的课程(他们这样做大概不是为了创建什么国家精品课程吧)。

在完成作业的过程中，部分地方参考了互联网上他人的解法，在[我的作业]中的相应位置都有注明；在本文中提及或未能提及的论文、博客、回答等都对我的学习产生有利的影响，在此都一并表示感谢。

[我的作业]:https://github.com/smartjinyu/CS231n_assignments