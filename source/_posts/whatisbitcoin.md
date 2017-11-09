---
title: 比特币是什么
date: 2017-10-23 22:36:05
categories: Bitcoin
tags:
    - 比特币
    - 定义
author: Tiny熊
---
  对于比特币也许一千个人有一千种理解。本文作为入门篇（写给完全没有了解过比特币概念的新手，老手可忽略），我尽量用简单易懂的语言来介绍比特币。
  到底什么是比特币，它到底是怎么运行的呢。

<!-- more -->
## 比特币是什么
> 比特币是一种基于分布式网络的数字货币。
> 比特币系统（广义的比特币）则是用来构建这种数字货币的网络系统，是一个分布式的点对点网络系统。

本文主要讲解狭义的比特币概念。

### 数字货币是什么
凯恩斯在《货币论》上讲，货币可以承载债务，价格的一般等价物。货币的本质是等价物，它可以是任何东西，如：一张纸，一个数字，只要人们认可它的价值。人民币，美元等作为国家信用货币，其价值由国家主权背书。而数字货币是一种不依赖信用和实物的新型货币，它的价值由大家的共识决定。比特币就是一种数字货币。（我们在网银，微信，支付宝的金额，准确来讲，它是信用货币的数字化，不是数字货币，不过央行也在研究比特币，准备发行数字货币）

### 运行原理
大家知道，在银行系统的数据库里记录着跟我们身份id对应的财产，下文称这样的记录为账本，如张三的卡10月1日转入1w, 余额10w。
比特币系统也同样有这样的账本，不同银行由单一的组织负责记录,比特币的记账由所有运行系统的人（即节点，可以简单理解为一台电脑）共同参与记录，每个节点都保存（同步）一份完整的账本。
同时使用简单多数原则，来保证账本的一致性。举个例子：如果有人在自己电脑上把自己的余额从1万改为1百万，他这个账本和大多数人的账本不一致，就会被比特币系统认为是无效的。


比特币使用区块链技术来支撑整个系统的运行，有兴趣的同学，可以详细阅读下这几篇博文：{% post_link whatbc 区块链记账原理 %} 、 {% post_link bitcoin-own 比特币所有权问题 %}、{% post_link bitcoin-pow 比特币如何挖矿 %}


[深入浅出区块链](http://learnblockchain.cn/) - 系统学习区块链，打造最好的区块链技术博客