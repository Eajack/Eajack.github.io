---
title: Road 2 NLP- Word Embedding词向量（FastText）
date: 2019-07-01 15:26:38
tags:
  - FastText
  - NLP 
categories:
  - NLP
mathjax: true
---

# 1. 参考资料

>* 论文1：FastText1词向量，[《Enriching Word Vectors with Subword Information》](https://github.com/Eajack/NLP-Papers/blob/master/Word%20Embedding%E8%AF%8D%E5%90%91%E9%87%8F/FastText/Enriching%20Word%20Vectors%20with%20Subword%20Information.pdf)，作者Bojanowski et al. （包括Mikolov），FAIR（Facebook AI Research）
>* 论文2：FastText2文本分类模型，[《Bag of Tricks for Efficient Text Classification》](https://github.com/Eajack/NLP-Papers/blob/master/Word%20Embedding%E8%AF%8D%E5%90%91%E9%87%8F/FastText/Bag%20of%20Tricks%20for%20Efficient%20Text%20Classification.pdf)，作者Joulin et al. （包括Mikolov），FAIR（Facebook AI Research）
>* 博客文：[《fastText 源码分析》](https://heleifz.github.io/14732610572844.html)
>* [FastText源代码（C++）](https://github.com/facebookresearch/fastText/tree/master/src)

主要参考资料如上，其实还有其他博客文，然而很多博客文都是互相抄袭的……而且很多都是讲解**FastText文本分类模型**，而非**FastText词向量**，前者基于后者建模。FastText文本分类模型原理简单易懂，然而词向量的训练原理有某些地方讲的很含糊。基于以上参考资料，我只能做出个人理解。（因为看源码看了很久，还是感觉没能解决我的核心疑惑……）

FastText其实是包括2个东西的：

* FastText词向量（PS：和Word2vec、GloVe一样，FastText词向量也属于**静态词向量**），对应论文1

* FastText文本分类模型，对应论文2

虽说本系列文章主题是：**Word Embedding词向量**，但是由于FastText特殊性，这里一起讲**FastText文本分类模型**。

FastText的最大优点：快速。

```
《Efficient estimation of word representations in vector space》摘要部分：
We can train fastText on more than one billion words in less than ten minutes using a standard multicore CPU, and classify half a million sentences among 312K classes in less than a minute.
```

# 2. FastText原理1：词向量训练

关于FastText词向量资料，原论文[《Enriching Word Vectors with Subword Information》](https://github.com/Eajack/NLP-Papers/blob/master/Word%20Embedding%E8%AF%8D%E5%90%91%E9%87%8F/FastText/Enriching%20Word%20Vectors%20with%20Subword%20Information.pdf)的原理部分提及得相当简略。

FastText词向量框架是基于Word2vec框架的，区别主要如下：

前者利用n-grams向量之和作为词向量（**key point & 疑惑点**）

![图1- FastText词向量表示](https://raw.githubusercontent.com/Eajack/NLP-Papers/master/Word%20Embedding%E8%AF%8D%E5%90%91%E9%87%8F/FastText/%E5%9B%BE1.PNG)

即单词w对应1个n-grams集合（原论文为提取3-grams ~ 6-grams的所有子串），每个字串会有对应的向量，因此该单词w的词向量 = 所有n-grams字串向量求和。

关于n-grams，原论文有以下例子：

![图2- n-grams](https://raw.githubusercontent.com/Eajack/NLP-Papers/master/Word%20Embedding%E8%AF%8D%E5%90%91%E9%87%8F/FastText/%E5%9B%BE2.PNG)

这里，以`where`为例，其对应的3-grams子串集合`G={"<wh","whe","her","ere","re>","<where>"}`，其中每一个子串对应1个向量表示，则`where`词向量则为求和。同理对于中文，举例`苏格拉底`的3-grams子串集合`G_1={"<苏格","苏格拉","格拉底","拉底>,"<苏格拉底>"}`。

以上原理都不难理解，关键在于：**原论文貌似没给出子串向量是如何获得！**当然n-grams向量理应是训练得到，然而如何训练，网络结构如何，原论文没提及，只是说基于Word2vec架构。Word2vec原理如下文：

[Road 2 NLP- Word Embedding词向量（Word2vec）](https://eajack.github.io/2019/06/21/Road%202%20NLP-%20Word%20Embedding%E8%AF%8D%E5%90%91%E9%87%8F%EF%BC%88Word2vec%EF%BC%89/)

本人一直卡在**如何训练n-grams子串向量**这问题上。涉及资料主要是：博客文[《fastText 源码分析》](https://heleifz.github.io/14732610572844.html)、[FastText源代码（C++）](https://github.com/facebookresearch/fastText/tree/master/src)。经过多次分析猜测（由于本人最后还是放弃通读C++源码…我看了挺久，但还是不知道如何训练n-grams子串获得向量的，只发现了**如何获取子串string**代码）。

**FastText默认Skip-Gram模型，对比Word2vec的Skip-Gram模型，有以下个人理解**（由于画图较为复杂，此处文字描述）：

![图3- Skip-Gram](https://raw.githubusercontent.com/Eajack/NLP-Papers/master/Word%20Embedding%E8%AF%8D%E5%90%91%E9%87%8F/Word2vec/%E5%9B%BE3.PNG)

如上图为Word2vec原Skip-Gram模型。

* 输入层：输入为当前单词w的n-grams的index集合，从输入向量矩阵$W_{V×N}$中挑出对应n-grams向量求和，作为输入层的$x_{k}$。**因此，FastText的输入层和Word2vec的区别在于：前者取n-gram求和作为词向量，后者取One-Hot词向量；前者输入向量矩阵$W_{V×N}$，其N为所有语料的n-grams总数，后者则为语料word总数。**
* 隐含层&输出层：Word2vec & FastText一致。

因此，FastText词向量训练框架 & Word2vec不一致仅在于输入层。（PS：个人理解）

# 3. FastText原理2：文本分类模型

FastText文本分类模型和词向量训练框架差不多，有了词向量的原理基础，该模型可以很简单地用下图总结：

![图4- FastText文本分类模型](https://raw.githubusercontent.com/Eajack/NLP-Papers/master/Word%20Embedding%E8%AF%8D%E5%90%91%E9%87%8F/FastText/%E5%9B%BE3.PNG)

如上图，FastText文本分类模型仅3层。

* 输入层：句子的n-grams表示向量（**PS：此处的n-grams向量为经过FastText词向量训练获得**）
* 隐藏层：n-grams向量求平均（**PS：FastText词向量训练则是求和**）
* 输出层：softmax层（**PS：沿用Word2vec的Hierarchical Softmax**）

由此可以看出，关键是输入层的**n-grams向量**，而这个便是FastText词向量训练步骤。所以，FastText文本分类模型包括2步：**1- FastText词向量训练；2- FastText文本分类模型构建**。