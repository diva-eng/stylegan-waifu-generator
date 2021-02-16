# stylegan-waifu-generator
StyleGAN老婆生成器

Generate your waifu with styleGAN [English](README.md)

## 介绍
这是我无聊的时候做的一个小项目，原本是想自己去理解一下stylegan2到底是什么原理，最后变成了一个挺有趣的小项目。

## 数据
这个网络的训练数据来源于[gwern](https://www.gwern.net/About)

```
Anonymous, The Danbooru Community, & Gwern Branwen; “Danbooru2020: A Large-Scale Crowdsourced and Tagged Anime Illustration Dataset”, 2020-01-12. Web. Accessed [DATE] https://www.gwern.net/Danbooru2020
```
我也花了一些时间自己写了一些脚本来清理这些数据，主要就是去掉一些无效的数据（黑白，nsfw，分辨率低）。danbooru2019的数据量非常大，在处理前整个数据差不多要有2tb左右，处理之后的头部有效数据只有18GB （512px）。之后数据被处理成一个lmdb的数据库方便训练，lmdb数据里也存有不同尺寸的数据，从8px到1024px。最后的lmdb数据库在426gb左右。

数据样品:

![数据样品](res/2238231-0.jpg)

## 训练
我直接使用了现有的 [stylegan2-pytorch](https://github.com/rosinality/stylegan2-pytorch)。主要是因为自己还是在理解stylegan2是如何运作的，我也相应的改变了一下训练脚本的参数和数据加载的方式。具体训练了多少个iter记不住了，我记得应该是在80万到一百万之间。

我是用的是一块RTX Titan在Ubuntu 18.04 上用cuda 10.2

## 结果

![256px](res/000017.png)

## Control
这个项目其实没有太多我自己原创的东西，因为为了学习很多东西都是用的网上现有的代码。唯一的区别就是我想设计一个可以自己控制生成结果的系统。在stylegan里面输入的latent vector貌似可以来控制生成结果，如果在同样的环境运行是可以出一样的结果的。我用rust写了一个小工具[rust-genome](https://github.com/r1cebank/genome) 来生成很长的一个哈希值来储存网络使用的latent vector这样可以很容易的存储和传输，使用同样的哈希值可以获得同样的结果。这样我们既可以控制结果还可以无数次的重复同一结果。

生成的基因哈希值:

```
c092c23a008000033f1fc413bdba659fbed62d56befa86f93e787d0a3f89028d3d4bf2673f8a221ebef5605bbda472dd3f678906be82855b3f9493b8c............020353ebf8c074ac02c3b9e3fc24ff03f49dbf0563f8bdf467743f8fca4dc0095918bf379b733e8382ec3ce89666bf2b9dbe3e08f23ebe8fe6a0bf3ab1b9bf7b6a743df1f45c
```

使用基因生成的图片:

![使用基因生成的图片](res/000.png)

这样做不但可以保证同一基因生成出同样的结果，我们还可以进行特征继承，特征混合，下面是一个使用两个基因特征让计算机去填补转换过程的例子。

![转换](res/out.gif)

## 训练好的模型
等我弄明白如何分享超级大的模型之后我会在这里发布如何去下载我已训练好的模型pt文件。

## 特别感谢
Danbooru 2019 dataset

Stylegan2-pytorch
