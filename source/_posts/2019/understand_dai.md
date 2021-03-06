---
title: 理解去中心化 稳定币 DAI
date: 2019-03-19 20:44:58
permalink: understand_dai
categories: 以太坊
tags:
    - 稳定币
    - DAI
author: Tiny熊
---


随着摩根大通推出JPM Coin 稳定币，可以预见稳定币将成为区块链落地的一大助推器。
坦白来讲，对于一个程序员的我来讲（不懂一点专业经济和金融），理解DAI的机制，真的有一点复杂。耐心看完，必有收获。


<!--  more  -->

## 为什么需要稳定币

如果一个货币其价值时刻在剧烈波动，就无法作为一个日常支付和交易的货币，谁也无法承担今天发的工资，第二天就跌掉了三分之一。

在币价高度不稳定时，在不退出加密货币市场的情况下，稳定币就可以提供价值保值。

通常发行稳定币的方式是通过资产担保来发行，像USDT、TUSD等就是通过美元资产来担保发行等额稳定币，如银行存款1亿美元就发行1亿USDT， 既通过锚定法币来实行稳定性。

> USDT 因审计不公开，经常被质疑超发，如1亿美元担保发行1.5亿USDT，就会导致0.5亿USDT无法兑换美元。这也是为什么在监管下发行的稳定币，如TUSD、GUSD有逐步取代USDT的趋势。


本文的主角 DAI 同样是通过**资产抵押**发行， DAI 是通过**抵押数字资产**发行，**去中心化**发行。

注意加粗的两个关键字**抵押数字资产**和**去中心化**，它是用一套称之为Maker的智能合约发行的，其背后的团队为MakerDAO。

> Maker目前只支持抵押ETH，后面可能会加入其它代币。
> DAO (Decentralized Autonomous Organization): 去中心化的自治组织

我们都知道数字资产的价值是有很大波动的， 那么Maker怎么来确保 1 DAI = 1 USD的呢？


##  稳定币 DAI的发行

Maker体系中有一个实现了抵押贷款逻辑的智能合约（CDP）， 当我们抵押（发送）ETH到智能合约，合约根据当时ETH的价值，计算一个**折扣**后，发行对应的DAI（符合ERC20标准的代币）。

> 以太价格获取Maker采用的是中心化方案，从各大交易所获取再加权平均。

为了方便理解，类比抵押屋产贷款，我们把房子作为抵押品向银行贷款，ETH就相当于房子，智能合约相当于银行，DAI 相当于贷款拿到的钱。银行给我们贷款时，银行也会对房子的价值打一个折扣。

这个折扣在Maker系统中称之为**抵押率**，这是一个很重要的概念，大家务必理解。

我们给他一个数学定义： `抵押率 = 抵押物的价值 / 放贷的价值`。

如果房子价值200万，抵押率为200%， 银行就只能给我们贷款100万，这个大家应该能够理解。
同样，假设以太币现在价值200美元，抵押率为200％，那么把1个以太币（200美元）发送到CDP智能合约，就可以获得发行的100个DAI。


在抵押ETH生成DAI的同时，合约会为我们生成一张**CDP借贷凭证**，它记录着借贷关系及金额，并且抵押ETH会一直锁定在合约里，在还清100个DAI时，ETH将归还我们。 就像银行扣押房子直到我们还清贷款一样。


到这里，DAI的发行应该明白了。

### 套现保值

DAI的这种抵押贷款逻辑非常有意思 ，它生成的CDP借贷凭证提供给我们一个**套现保值**的手段。假如你有一大笔以太在手里， 而你又急需一笔资金怎么办？ 那么抵押生成DAI是获得资金的一个绝佳选择。如果在交易所把币卖掉换成稳定币，会失去以太的所有权，币价上涨时就无法换回对应的以太。

例如：目前 ETH 价格约为 130 美元， 按200%的抵押率， 1000个以太可以抵押生成6.5万个DAI，即可以获得6.5万美元资金，假设一年之后，ETH价格涨到到500 美元，只需要偿还6.5万个DAI（美元）及一点利息就可以赎回1000个以太（价值50万美元）。


## DAI是如何保持稳定的？

依靠抵押美元发行的USDT、TUSD，能保持价值相对稳定很容易理解，靠抵押ETH的DAI如何保持稳定呢？

分两种情况：如果 ETH 升值， 意味着 DAI 有更足够的抵押（更高的抵押率，担保更充足），这不会有太大影响。如果DAI的交易价格超过1美元，Maker也会激励用户创造更多的DAI（目标利率反馈机制）。

> 目标利率反馈机制（TRFM）：不过最重要的是以下几点：当DAI的交易价格超过1美元时，智能合约会激励人们生成DAI。当DAI的交易价格不到1美元时，智能合约会激励人们赎返DAI。


如果 ETH 价值下降则复杂一些，回到抵押屋产贷款的类比，如果我们的房子价值下降，银行会要求我们追加抵押物或及时还款，Maker也是一样，始终要求DAI是超额抵押的。

如果资产下跌到一定值（如抵押率150%），并且原抵押人没有追加抵押物或偿还（部分）DAI，合约会自动启动**清算（liquidated）**，之前抵押的以太币被拍卖，直到从CDP合约借出的DAI被还清。

还是前面的类比，价值200万房子，抵押率200%，贷款了100万，在房子下跌到150万时，银行就会拍卖房子，清除这笔贷款。 Maker也是使用这种方式从市面上回购DAI用来偿还给CDP。

简单总结：
**Maker始终要求DAI是超额抵押的**，当系统发现有部分资产存在风险时，就会对风险过高的资产进行清算，它会首先清算抵押率低于 150% 的CDP借贷凭证，而为了防止清算持有人必须往CDP借贷凭证存入更多ETH或偿还DAI来提高抵押率。


现在我们来看 [MakerDao抵押借款的界面](https://cdp.makerdao.com/)就清晰了，以下截图是抵押1 ETH 生成60个DAI：

![](https://img.learnblockchain.cn/2019/15530688001807.jpg!wl)

Collateralization ratio 抵押率为 228%， Liquidation price 清算价格为90 美金。

### 清算

关于清算也许还有几点需要了解：

1. 在发生清算后， 就再也无法通过偿还DAI来取回之前抵押的ETH了（CDP借贷凭证会关闭）。
2. 清算发生时，会扣除一部分的罚金（13%的罚金）和手续费。
3. 拍卖ETH得到的DAI  会被销毁， 就像用户偿还DAI 被销毁一样。
4. 拍卖偿还DAI后， 剩余的资产用户可以拿回。
5. Maker系统中有一个专门负责清算的合约。


## MKR 应对暴跌

上面一有一个前提，不管如果 DAI 都是超额抵押， 如果以太价格急剧下跌，抵押品的价值达不到借出的DAI的价值时，这时启动清算，将由Mkr持有者负责回购。

Mkr 是Maker系统中的权益代币， Mkr持有者是系统的收益者，获取借款利息及罚金等。

还是前面的类比，如果价值200万房子， 突然跌倒100万以下， 这时候在公开市场拍卖，市场是没有买家出100万以上购买房子的，那么银行将启用自有资金回购。

相当于损失的价值转嫁到Mkr持有者，价格波动是没发消灭的，它只能转移，DAI的价格波动性实际由CDP 借贷凭证持有者和Mkr持有者共同承担。


## 一点拓展

DAI 由于它的超额抵押借款机制，是一个很好的杠杆做多工具。

如果我们预期以太币会上涨，我们可以把前面1000个以太抵押生成6.5万个DAI，再此购买以太进行抵押，多次操作之后，可能获得数倍的增值。


为了写这边文章，拓展我不少金融领域知识，以前一直不理解做多做空（因为我不炒股、不炒币），现在把我的理解做一个记录，供参考：

### 做多

做多就是看好其上涨而买入，杠杆做多则是借钱买入。
上面就是借DAI（美元）买入以太，借来的6.5万个DAI（美元），按130美元一个以太，可以购买到500个以太，如果一个月后以太涨到200美元， 500个以太就是10万美元，还掉6.5万美元后，相当于**凭空**赚了3.5万美元。

### 做空

做空就是认为其下跌而卖出，同样也可以借别人的卖出。
现在130美元一个以太，我认为以太会下跌到100美元，于是我向交易所借了1000个币卖掉获得13万美元，如果真下跌到100美元，就用10万美元换1000个币还给交易所，这样我**凭空**赚了3万美元。


## 参考文章：

[DAI 白皮书](https://makerdao.com/zh-CN/whitepaper)
[DAI 稳定币的通俗解释](https://www.chainnews.com/articles/858804412113.htm)


如果你对稳定币感兴趣，我们可以一起交流，我的微信：xlbxiong 备注：稳定币。

加入[知识星球](https://learnblockchain.cn/images/zsxq.png)，和一群优秀的区块链从业者一起学习。
[深入浅出区块链](https://learnblockchain.cn/) - 系统学习区块链，打造最好的区块链技术博客。






