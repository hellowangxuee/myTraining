## ElasticNet
[`ElasticNet`](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.ElasticNet.html#sklearn.linear_model.ElasticNet)
是用L1和L2先验作为正则化训练的线性回归模型。这种组合允许用于学习一个稀疏模型，其中少数的权重是非零的，就像`Lasso`，
同时仍保持`Ridge`的正规化特性。我们使用l1_ratio参数控制L1和L2的凸组合。  
当存在彼此相关的多个特征时，`ElasticNet`是有用的。`Lasso`可能随机选择其中一个，而`ElasticNet`很可能选择两个。
`Lasso`和`Ridge`之间的折衷的实际优点是它允许Elastic-Net继承一些Ridge在旋转下的稳定性。  
[`ElasticNetCV`](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.ElasticNetCV.html#sklearn.linear_model.ElasticNetCV)
可以用于通过交叉验证设置参数`alpha` and `l1_ratio`。
### Multi-task Elastic Net
[`MultiTaskElasticNet`](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.MultiTaskElasticNet.html#sklearn.linear_model.MultiTaskElasticNet)
是一个弹性网模型，用于联合估计多个回归问题的稀疏系数。Y is a 2D array, of shape (n_samples, n_tasks)。
约束是所选的特征对于所有回归问题都相同。  
类[`MultiTaskElasticNetCV`](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.MultiTaskElasticNetCV.html#sklearn.linear_model.MultiTaskElasticNetCV)
可用于通过交叉验证设置参数`alpha` and `l1_ratio`。  
