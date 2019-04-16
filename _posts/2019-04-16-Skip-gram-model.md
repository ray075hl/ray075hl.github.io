---
layout: post
date: 2019-04-16 12:00:00
img: sparse_matrix.jpg
tags: [machine learning]
title: Skip Gram Model
---

## Intro

Skip Gram Model是**Word2Vec**中的一种方法. 将one-hot-encode的形式编码为密集的向量, 并且能够体现词与词之间的相似性.



## The Model

**模型的输入是:**  one-hot-encode的向量.

**模型的输出是:**  one-hot-encode的向量.

训练样本的产生方式如下:

![](../assets/img/2019-04-16/training_data.png)

例如:  (fox, quick),   ***fox***为输入,  ***quick***为输出.

**模型结构如下:**

![](../assets/img/2019-04-16/skip_gram_net_arch.png)

## Detail

模型分为三层:

* 输入层
* 隐层(无激励函数)
* 输出层(Softmax)

假设输入是$$I \in R^{1 \times 10000}$$, 隐层输出为$$H \in R^{1 \times 300}$$,  输出为 $$O \in R^{1 \times 10000}$$.

以上图为例:

假设输入样本 (fox, quick),   ***fox***为输入,  隐层的输出可以看作是对***fox***一词的编码(由10000维编成了300维), Softmax后对应的***quick***节点的输出,可以解读为, ***fox***出现后, 在window size中出现词***quick***的**概率**. 

### 直觉解释 

>**如果两个词的意思比较相似,例如"engine"和"transmission".  那么engine 和 transmission 所处的上下文应该是差不多的, 例如这两个词周围可能经常出现"car", "plane", "fly", "machine"之类的词. **
>
>**即训练样本大概是这样的**
>
>**("engine", "car"), ("engine", "plane"), ("engine", "fly"), ("engine", machine")**
>
>**("transmission", "car"), ("transmission", "plane"), ("transmission", "fly"), ("transmission", machine")**
>
>**即"engine"作为输入时, "car", "plane", "fly", "machine" 等输出结点的概率值较大.**
>
>**而"transmission"作为输入时, "car", "plane", "fly", "machine" 等输出结点的概率值也较大.**
>
>**这就意味着隐层对"engine", "transmission"两个词的编码应该是相近的.**



## Cite

所有图片和文字均来自:

[http://mccormickml.com/2016/04/19/word2vec-tutorial-the-skip-gram-model/](http://mccormickml.com/2016/04/19/word2vec-tutorial-the-skip-gram-model/)

