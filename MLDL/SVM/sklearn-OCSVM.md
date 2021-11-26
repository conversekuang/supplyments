



### 单分类算法简介

**场景&需求**：通常我们判断样本是不是异常，但是造成异常的原因有很多，可能无法识别每一种异常。而且异常样本收集困难。那么只对正常样本训练，只需要对正常样本进行检测即可。



**单样本检测**：只需要判断和回答该样本是否属于该类即可。



**单分类算法**：

​	1. One Class Learning 比较经典的算法是One-Class-SVM。

​	One-Class-SVM算法的思路非常简单，就是寻找一个超平面将样本中的正例圈出来，预测就是用这个超平面做决策，在圈内的样本就认为是正样本。适用场景：由于核函数计算比较耗时，在海量数据的场景用的并不多。

<center><img src="https://i.loli.net/2021/11/26/XAkV819SFLps2qi.png" style="zoom:50%; align:center" ></center>

2. 基于神经网络的算法

<center><img src="https://img2018.cnblogs.com/blog/1226410/201904/1226410-20190424112328960-676305797.png" style="zoom:80%; align:center" ></center>



​		深度学习中广泛使用的自编码算法可以应用在单分类的问题上，自编码是一个BP神经网络，网络输入层和输出层是一样，中间层数可以有多层，中间层的节点个数比输出层少，最简单的情况就是中间只有一个隐藏层，如下图所示，**由于中间层的节点数较少，这样中间层相当于是对数据进行了压缩和抽象**，实现无监督的方式学习数据的抽象特征。



### 单分类、二分类、多分类的区别



- 单分类：

  但是单分类问题在**工业界广泛存在**，由于每个企业刻画用户的数据都是有限的，很多二分类问题很难找到负样本，即使用一些排除法筛选出负样本，负样本也会不纯，不能保证负样本中没有正样本。所以在只能定义正样本不能定义负样本的场景中，使用单分类算法更合适。

- 二分类：

  其区别就是在二分类问题中，**训练集中就由两个类的样本组成**，训练出的模型是一个二分类模型；而OneClassClassification中的训练样本只有一类，因此训练出的分类器将不属于该类的所有其他样本判别为“不是”即可，而不是由于属于另一类才返回“不是”的结果。

- 多分类：

  



[Python机器学习笔记：异常点检测算法——One Class SVM - 战争热诚 - 博客园 (cnblogs.com)](https://www.cnblogs.com/wj-1314/p/10701708.html)







### python相关库：

> sklearn库中的OneClassSVM类

​	https://scikit-learn.org/stable/modules/generated/sklearn.svm.OneClassSVM.html



> kenchishi库中的OCSVM类

kenchishi 是sklearn库中的OneClassSVM类的子类，因为kenchishi.OCSVM中就是OneClassSVM的实现。

所以OneClassSVM与OCSVM是没有区别的。



class OCSVM(BaseOutlierDetector):

- predict (self, X=None, threshold=None):

  实现是通过**decision_function**计算得到。

  根据输入的X样本，得到预测值。是否是异常值。threshold可以传入参数。

  

- decision_function(self, X=None, threshold=None)

  决策函数：**self.score_samples(X) + threshold**

  最后正值代表内部，负值代表外部。

  

- score_samples(self, X=None)：

  — **anomaly_score(X)**  异常分数的相反数



- anomaly_score (self, X=None, normalize=False) 

  计算异常分数



- predict_proba

  计算的是anomaly_score 



---



Introduction to One-class Support Vector Machines【英文介绍】

http://rvlasveld.github.io/blog/2013/07/12/introduction-to-one-class-support-vector-machines/



---

OneClassSVM

sklearn的OCSVM分类器有参数gamma和nu，常见调参

- 核函数：**kernel** **{‘linear’, ‘poly’, ‘rbf’, ‘sigmoid’, ‘precomputed’}, default=’rbf’**

- gamma:   ‘rbf’, ‘poly’ and ‘sigmoid’的核函数系数

  ​				  gamma = 1 / (n_features * np.std(X))    

  ​				  就是： 1 /  输入样本的标准差*特征数量  

  ​				  gamma = 1 / ( n_features * $\sigma$ )    

- nu:  训练误差部分的上限和支持向量部分的下限，取值在（0，1）之间，默认是0.5



参数调整：

[SVM简介及sklearn参数 - Solong1989 - 博客园 (cnblogs.com)](https://www.cnblogs.com/solong1989/p/9620170.html)



现实场景的实战：

​	由于一分类只关注该类自身的样本情况，当该分类器出现将异常值预测为正常时，可以对出现问题的分类器调整OneClassSVM的gamma和nu参数。区分异常与正常样本的距离。

​	nu的影响比较大，其次gamma。

