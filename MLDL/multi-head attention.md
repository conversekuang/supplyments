

Encoder-Decoder框架

Encoder-Decoder是一个模型构架，是一类算法统称，并不是特指某一个具体的算法。

编码（encode）由一个编码器将输入序列转化成**一个固定维度的稠密向量**，解码（decode）阶段将这个激活状态生成目标译文。

<img src ="https://img-blog.csdnimg.cn/20200312151759961.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA2MjY5Mzc=,size_16,color_FFFFFF,t_70" style="zoom:70%">



缺点：信息丢失

道编码器和解码器之间有一个共享的向量（上图中的向量c），来传递信息，而且它的长度是固定的。这会产生一个信息丢失的问题，也就是说，编码器要将整个序列的信息压缩进一个固定长度的向量中去。

1. Encoder无法表示整个输入序列，先输入内容会被后输入内容稀释。
2. Decoder没有获取足够输入，解码准确度降低。

由于**基础的Encoder-Decoder模型**链接Encoder和Decoder的组件仅仅是一个固定大小的状态向量，这就使得Decoder无法直接无关注输入信息的更多细节。

**Attention模型**的特点是Encoder不再将整个输入序列编码为固定长度的中间向量，而是编码成一个【向量序列】。



<img src ="https://img-blog.csdnimg.cn/20200312154707474.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA2MjY5Mzc=,size_16,color_FFFFFF,t_70" style="zoom:70%">



​	语义编码C是由句子Source的每个单词经过Encoder编码产生的，这意味着不论是生成哪个单词，y1,y2还是y3。**其实句子Source中任意单词对生成某个目标单词yi来说影响力都是相同的，这是为何说这个模型没有体现出注意力的缘由**。这类似于人类看到眼前的画面，但是眼中却没有注意焦点一样。这样不符合人类认知事物的原理。所以我们引入Attention机制。

​	我们的图中，出现了C1,C2,C3。C1,C2,C3分别对应了y1,y2,y3，这样我们的输出值的表达式也改变了。
$$
Attention(Query_j,Source)=\sum_{i=1}^NSimilarity(Query_j,Key_i)*Value_i
$$
​	此时给定Target中的某个元素Query, 通过计算Query和各个Key的相似性或者相关性，，得到每个Key对应Value的权重系数，然后对Value进行加权求和，即得到了最终的Attention数值





from:

​	https://blog.csdn.net/u010626937/article/details/104819570

​	https://www.cnblogs.com/huangyc/p/10409626.html