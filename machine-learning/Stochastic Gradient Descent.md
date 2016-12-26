## 随机梯度下降
[网页链接](http://scikit-learn.org/stable/modules/sgd.html#classification)

SGD已成功应用于大规模的文本分类和自然语言处理中并经常遇到数据稀疏机器学习问题

##### 优点：

1. 高效
2. 易于实现的（很多机会进行代码调整）

##### 缺点：

1. SGD 需要许多超参数，如正则化参数和迭代次数。
2. SGD 对特征缩放敏感。

### 分类

[SGDClassifier](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.SGDClassifier.html#sklearn.linear_model.SGDClassifier)支持不同的损失函数和处罚进行分类

至于其他的分类，SGD必须配备两个数组：训练样本 X of size[n_samples, n_features]，训练样本的目标值(标签) Y of size [n_samples] 

SGD 将训练数据拟合成线性模型。成员`coef_`持有模型参数；成员`intercept_`持有偏距（又名失调或偏置）：

[SGDClassifier.decision_function](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.SGDClassifier.html#sklearn.linear_model.SGDClassifier.decision_function)：得到样本与超平面的距离

### 回归

[SGDRegresso](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.SGDRegressor.html#sklearn.linear_model.SGDRegressor)r实现支持不同的损失函数和处罚，以适应线性回归模型纯随机梯度下降学习程序。`SGDRegressor`是非常适合有大量训练样本（> 10.000）的回归问题，对于其他的问题，我们建议的`Ridge`， `Lasso`或`ElasticNet`








