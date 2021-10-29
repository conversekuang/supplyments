

决策树：【也就是为了选一个特征让数据分得尽量开！】https://www.cnblogs.com/fionacai/p/5894142.html

​	**实际上就是寻找最纯净的划分方法**，这个最纯净在数学上叫**纯度**，纯度通俗点理解就是目标变量要分得足够开（y=1的和y=0的混到一起就会不纯）。

信息增益：**表示得知特征X的信息而使得类Y的信息的不确定性减少的程度，特征可以降低数据的不确定性**

发展过程：ID3 -> C4.5 -> Cart;



1. ID3算法**使用信息增益**； Gain(D,A) = H(D) – H(D|A)。代表的是不确定性减少的程度，所以越大越好。

   Gain(D,A)定义为集合D的信息熵H(D)与特征A给定条件下D的信息条件熵H(D|A)之差。

   

2. C4.5算法**使用信息增益率；** IGR =  Gain(D,A)  / H(D) 。

   

3. CART算法**使用基尼系数;**    

   基尼(gini)系数：总体内部包含越混乱，基尼系数越大；内部纯度越高，基尼系数越小。

   ![img](https://i.loli.net/2021/02/01/PpdkJow6RzvXhgm.png)

   ​	k为某节点中包含样本的种类数目；

   ​	pi为某节点中某类样本数目 / 该节点中样本总数。

---



https://blog.csdn.net/mr_robert/article/details/88924175



demo：

​	https://zhuanlan.zhihu.com/p/133838427

​	https://shuwoom.com/?p=1452







DecisionTree 调用predict_proba()方法输出的是测试数据的概率但是发现预测的都是到的都是1和0.

https://blog.csdn.net/xiongchengluo1129/article/details/80227724



因为决策树的叶子节点都是同类，所以预测某类的结果都是100%或者0，只有是和不是两种可能。所以不存在其余概率。而函数predict_proba出现其他概率值前提是，叶子节点的样本是非同类型。所以要设置某一条件让节点在某一条件下就不再进行分裂。min_samples_leaf 参数当节点的样本数少于某一值时候不再进行分裂，当做叶子节点即可。而当决策到该节点时候，按照样本的比例返回概率。



min_samples_leaf 参数，当分到该节点的数量大于1时候，就确定为一个节点。

```
The minimum number of samples required to be at a leaf node.
A split point at any depth will only be considered if it leaves at
least ``min_samples_leaf`` training samples in each of the left and
right branches.  This may have the effect of smoothing the model,
especially in regression.
```

min_samples_leaf 越大所以分的越不纯净，概率就会分散，min_samples_leaf 越小，分的相对纯净，概率就不会太分散。

```
例如，如果min_samples_split = 5，并且内部节点有7个样本，则允许拆分。但是，我们说分裂产生了两片叶子，一个带有1个样本，另一个带有6个样本。如果为min_samples_leaf = 2，则将不允许分割（即使内部节点有7个样本），因为生成的叶子之一将少于叶子节点处所需的最小样本数。
```

因为分散不分散和设置threshold门限也有关系。



```
The predicted class probability is the fraction of samples of the same class in a leaf.
```





![image-20210225093622411](https://i.loli.net/2021/02/25/lLMZiAjGOsVkgof.png)





![image-20210225093704795](https://i.loli.net/2021/02/25/7d9YpoOqSvXNPAt.png)





---

过拟合：更注重细节，在训练集上表现好，但是测试集上表现不好。

欠拟合：在训练集合上表现不佳，可能存在模型简单。



为了避免过拟合：

前剪枝：在生成树的时候就通过阈值能判定是否继续生成树。

后剪枝：先生成树，在删除一些子树。



Decision Tree 过拟合：

https://blog.csdn.net/qq_41577045/article/details/79844709



class_weight：当参数比例不均衡

min_samples_leaf： 当最小叶子节点的样本数量不少于某个值

max_depth ： 最大的深度

【通过best depth进行选择】



Sklearn决策树中调参的参数

https://www.cnblogs.com/juanjiang/p/11003369.html





DecisionTreeClassifier 调参

https://www.cnblogs.com/pinard/p/6056319.html