---
title: Road 2 NLP- Word Embedding词向量（GloVe）
date: 2019-06-22 12:57:38
tags:
  - GloVe
  - NLP
categories:
  - NLP
---

**参考资料**

>* GloVe开山之作：《GloVe: Global Vectors for Word Representation》，作者Jeffrey Pennington（Stanford NLP）
>* CSDN博客文章：[理解GloVe模型（+总结）](https://blog.csdn.net/u014665013/article/details/79642083)



个人觉得这篇文章：[理解GloVe模型（+总结）](https://blog.csdn.net/u014665013/article/details/79642083)，已经讲的很好了…我直接看这文章弄懂了GloVe原理的。有了上一篇Word2vec基础，GloVe原理不是很难：

[Road 2 NLP- Word Embedding词向量（Word2vec）](https://eajack.github.io/2019/06/21/Road%202%20NLP-%20Word%20Embedding%E8%AF%8D%E5%90%91%E9%87%8F%EF%BC%88Word2vec%EF%BC%89/)

按照GloVe原文可知，GloVe综合2类Word Representation模型优点：

```
The two main model families for learning word vectors are: 
1) global matrix factorization methods,such as latent semantic analysis (LSA) (Deerwester et al., 1990) and 
2) local context window methods, such as the skip-gram model of Mikolov et al. (2013c).

GloVe = global matrix factorization methods + local context window methods
```



