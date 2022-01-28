

循环神经网络

主要预测和处理序列数据。

与NN和CNN不同，RNN在每层之间的节点是无连接的。



---

语言模型：在已知单词的情况下，推断下一个单词是什么。条件概率模型

神经语言模型：用神经网络的语言模型

衡量语言模型的指标：复杂度（perplexity）

​	PTB数据集：语料库。1w单词。NLP常用的标准，便于模型之前的衡量。

相同的词汇表和预处理，在该前提下，才能比较模型的性能。

预处理：将频率从高到低进行排序，选取前1w个，如果出现不存在的用`<unk>`代替。

词汇表：1w个



PTB batching：

1. padding填充。找到每个batch中最长的句子，保证单个batch内句子长度一样。不必batch和batch一致。
2. 有几个batch将文章分几个部分，每个部分内部都是顺序的。目的是保存上下文信息。

---

神经网络机器翻译：



seq2seq模型：就是利用rnn。组成是两个rnn网络，一个是编码器。一个是解码器。编码器将内容压缩成固定长度，解码器再将固定长度还原句子。



WMT数据集：机器翻译领域的重要公开数据集，标准数据集。

WMT是没有进行分割词汇。没有空格。PTB是有空格分好的。

分割工具：moses切词工具。github开源。

可对中文和英文进行切词。



---

seq2seq：将句子压缩到固定长度的向量，难以存储足够的信息。

attention机制引入：允许解码器随时查阅输入句子的单词和片段，不需要中间存储。



**seq2seq+attention机制**。不完全依赖于上一个时刻隐藏时刻，可以读取到和该位置最相关的原文信息。

引入attention后，断开了编码器与解码器的连接，靠attention就能连接。

下面就是引入attention之前和之后的区别。

<img src = "https://s2.loli.net/2022/01/18/JguAMCWSBoPeH2k.png" style ="zoom:25%">

<img src = "https://s2.loli.net/2022/01/18/R5KpyAGz9S1BjW8.png" style ="zoom:23%">

注意力机制的优点：

高效获取信息的方式

解码器可以主动查询最相关的信息

缩短信息流动的距离。（rnn的话需要一层层的往上找，但是attention直接一步就能找到。）



---

之前的attention机制都是和其他方法结合的，发现有好的性能但是要和复杂网络结合。因此想独立出来只依靠attention机制，就是transformer。【google 2017年 文件 attention is all  you need】



Transformer：可以不使用cnn或者rnn，完全使用attention在不同层之间传递信息。

1. 在机器翻译领域有比较好的应用。
2. cnn+attention在图像上面有比较好的应用。



attention mechanism: 又称为内部注意力机制，是一种将单个序列的不同位置关联起来以计算序列表示的注意力机制。



**Encoder**: 6层，每层有2个子层，第一个层是多头注意力。第二个是简单的全连接的前馈网络。

每个子层使用残差连接【a residual connection】，然后进行层归一化。

每个自层的输出是：LayerNorm(x + Sublayer(x))

也就是每一层都是add & norm。



**Decoder**：6层。除了每个编码器层中的两个子层之外，解码器还插入了第三个子层，该子层对编码器堆栈的输出执行多头注意力。我们在每个子层周围使用残差连接，然后进行层归一化。



**注意力函数**可以描述为将查询Q和一组键值K,V对映射到输出Output，其中查询、键、值和输出都是向量。 

输出Output计算为值的加权和 = function(Q, K)算出来的weight然后对Value进行求和。



**缩放点乘的注意力:**

两种常见的attention 函数是：加性 和 乘性。理论上两者复杂度是一致的。实际上，点乘是更快，空间效率更高。

本文用的是乘性，多了一个缩放因子。【假设q，k都是标准正态分布，当有$`d_{k}`点积，那么方差为`$d_{k}$`，标准差就是根号`$d_{k}$`，所以缩放因子就是这个】
$$
Attention(Q,K,V ) = softmax( \frac{QK^{T}}{\sqrt{d_{k}}}    )V
$$
**多头注意力**：

与其采用一个单独的attention function输入d维的key，val和query，发现用不同线性投影线性地将query，keys和vals分别映射到dk，dk，dv维，进行h次更有效。

得到一个dv维的输出值。在进行合并，再一次映射，最后得到结果。

【相当于把原来的一个拆分为多个一起做，然后再合并。】



多头注意力机制允许模型在不同位置加入不同表示子空间的信息。单头注意力。平均后会抑制该点。
$$
MultiHead(Q; K; V ) = Concat(head1,...,headh)W^O \\
where head_i = Attention(QW_iQ, KW_iK, VW_iV )
$$


**FFN**

除了注意力层，每一程还有一个前馈网络。
$$
FFN(x) = max(0, xW_1 + b_1)W_2 + b_2
$$


Transformer使用多头注意力在3个方面：

- 在**“编码器-解码器注意力”层**中，查询来自前一个解码器层，记忆键和值来自编码器的输出。 这允许解码器中的每个位置参与输入序列中的所有位置。 这模仿了序列到序列模型中典型的编码器-解码器注意机制
- **编码器**包含自注意力层。 在自注意力层中，所有的键、值和查询都来自同一个地方，在这种情况下，是编码器中前一层的输出。 编码器中的每个位置都可以关注编码器上一层的所有位置
- 类似地，**解码器**中的自注意力层允许解码器中的每个位置关注解码器中包括该位置的所有位置。 我们需要防止解码器中的信息向左流动，以保持自回归特性。 我们通过屏蔽掉（设置为 +$\infinite$）softmax 输入中与非法连接对应的所有值来在缩放的点积注意力内部实现这一点。



Positional Encoding: 位置编码

​	由于我们的模型不包含递归和卷积，为了让模型利用序列的顺序，我们必须注入一些关于序列的相对或绝对位置的信息。在输入中嵌入位置编码。

​	pos是位置，i是维度。

​	[一文读懂Transformer模型的位置编码 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/106644634)





---

对于Attention is all you need文章的深度解读：

[The Annotated Transformer (harvard.edu)](http://nlp.seas.harvard.edu/2018/04/03/attention.html)

[The Illustrated Transformer – Jay Alammar – Visualizing machine learning one concept at a time. (jalammar.github.io)](http://jalammar.github.io/illustrated-transformer/)



位置编码这一核心部分的解读信息：

[Transformer Architecture: The Positional Encoding - Amirhossein Kazemnejad's Blog](https://kazemnejad.com/blog/transformer_architecture_positional_encoding/)



解析文章： 

https://www.jianshu.com/p/b1030350aadb

https://mp.weixin.qq.com/s/RLxWevVWHXgX-UcoxDS70w