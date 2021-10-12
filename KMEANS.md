

**Novel Detection**：

python的kenchi库：

kenchi :   This is a scikit-learn compatible library for anomaly detection.



异常检测：包含异常值检测和新颖性检测

- Outlier detection

  1. FastABOD [[8\]](https://pypi.org/project/kenchi/#kriegel08)

  2. LOF [[2\]](https://pypi.org/project/kenchi/#breunig00) (scikit-learn wrapper)

  3. KNN [[1\]](https://pypi.org/project/kenchi/#angiulli02),  [[12\]](https://pypi.org/project/kenchi/#ramaswamy00)

  4. OneTimeSampling [[14\]](https://pypi.org/project/kenchi/#sugiyama13)

  5. HBOS [[5\]](https://pypi.org/project/kenchi/#goldstein12)

- Novelty detection

  1. OCSVM [[13\]](https://pypi.org/project/kenchi/#scholkopf01) (scikit-learn wrapper)

  2. MiniBatchKMeans
  3. IForest [[10\]](https://pypi.org/project/kenchi/#liu08) (scikit-learn wrapper)

  4. PCA
  5. GMM (scikit-learn wrapper)
  6. KDE [[11\]](https://pypi.org/project/kenchi/#parzen62) (scikit-learn wrapper)
  7. SparseStructureLearning [[6\]](https://pypi.org/project/kenchi/#ide09)





https://blog.csdn.net/lyxleft/article/details/89344219

http://blog.sina.com.cn/s/blog_7103b28a0102w5ss.html

|      |          | 异常值检测(outlier detection)  | 新颖点检测(Novelty detection) |      |
| ---- | -------- | ------------------------------ | ----------------------------- | ---- |
| 1    | 训练数据 | 包含异常值                     | 不受异常值的污染              |      |
| 2    |          | 没有clean data可以用于训练模型 | 有明确的clean data用于训练    |      |
| 3    |          | 无监督学习                     | 半监督学习                    |      |



```
1. outlier detection：The training data contains outliers which are defined as observations that are far from the others. 

2. novelty detection：The training data is not polluted by outliers and we are interested in detecting whether a new observation is an outlier.

3. Outlier detection is then also known as unsupervised anomaly detection and novelty detection as semi-supervised anomaly detection


OCSVM:
The nu parameter, also known as the margin of the One-Class SVM, corresponds to the probability of finding a new, but regular, observation outside the frontier



```



Novelty and Outlier Detection 官方的解释。

https://scikit-learn.org/stable/modules/outlier_detection.html



为了防止混淆，KMEANS和KNN是不一样的。不要弄错的。



```
from kenchi.outlier_detection import MiniBatchKMeans
from sklearn.cluster import MiniBatchKMeans as _MiniBatchKMeans
#内部初始化还是调用的sklean，所以就是介绍说的，
```





