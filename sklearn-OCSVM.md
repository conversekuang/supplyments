

> sklearn库中的OneClassSVM类

https://scikit-learn.org/stable/modules/generated/sklearn.svm.OneClassSVM.html



kenchishi 是OCSVM类





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



chat：-1.0008627914670236              -1.00086

email: -1.0016618865125797               -1.00166

transfer: -1.0002112471539533           -1.0002

web: -1.0003101183156633                   1.0003



这是之前的，

chat 【-1054.481589620274， -1054.3341987544957】  threshold  1054.3886010549718

email【-3619.002311855918， -3618.779374008295】 3619.0045135177643

transfer 【-538.2240983688007, -538.1582219304873】 538.2271388919067

voip【-2797.779884992074, -2797.5437679448364】2797.6597502875243

web【-8954.956107092792， -8954.533036264702】8954.799853940698





http://rvlasveld.github.io/blog/2013/07/12/introduction-to-one-class-support-vector-machines/

