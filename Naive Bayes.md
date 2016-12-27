## 朴素贝叶斯
贝叶斯方法是一套有监督学习算法，基于贝叶斯定理假设每对特征之间相互独立  
朴素贝叶斯分类器已经在许多现实世界的情况下工作的非常好，
著名的文档分类和垃圾邮件过滤相当奏效。它们需要少量训练数据估算必要的参数。  
### 优点
1. 相比复杂的方法朴素贝叶斯学习和分类可以非常快
2. 有助于减轻来自维数灾难引起的问题

### 缺点
虽然朴素贝叶斯被称为一个体面的分类，它被称为是一个坏的估计，
所以对概率输出 `predict_proba` 不应太认真对待。  

### 高斯朴素贝叶斯
[`GaussianNB`](http://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.GaussianNB.html#sklearn.naive_bayes.GaussianNB)
实现了高斯朴素贝叶斯算法进行分类。特征的可能性被假定服从高斯分布  
### 多项朴素贝叶斯

[`MultinomialNB`](http://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.MultinomialNB.html#sklearn.naive_bayes.MultinomialNB)
对多项式分布数据实现了朴素贝叶斯算法，是文本分类中使用的两个经典朴素贝叶斯变体之一
### 伯努利贝叶斯
[`BernoulliNB`](http://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.BernoulliNB.html#sklearn.naive_bayes.BernoulliNB)
针对服从多元伯努利分布分布的数据实现朴素贝叶斯训练和分类算法
