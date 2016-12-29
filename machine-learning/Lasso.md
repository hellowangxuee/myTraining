## Lasso
Tibshirani(1996)提出了Lasso(The Least Absolute Shrinkage and Selectionator operator)算法。
这种算法通过构造一个惩罚函数获得一个精炼的模型；通过最终确定一些指标的系数为零，LASSO算法实现了指标集合精简的目的。
这是一种处理具有复共线性数据的有偏估计。  
### 基本思想
在回归系数的绝对值之和小于一个常数的约束条件下，使残差平方和最小化，从而能够产生某些严格等于0的回归系数，得到解释力较强的模型  
Lasso是一个线性模型，用于评估稀少系数下的数据类型。当参数值较少时Lasso算法非常有用，可以有效降低数据误差。  
在python中Lasso的实现方法也很方便，只需要从linear_model线性模型中调用Lasso参数即可  
Scikit learn包提供了两个通过交叉验证的方法来设置Lasso alpha参数：LassoCV and LassoLarsCV。  

[`LassoCV`](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LassoCV.html#sklearn.linear_model.LassoCV)
：对于高维数据集，如果包含很多共线回归量，是一个非常合适的算法。  
[`LassoLarsCV`](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LassoLarsCV.html#sklearn.linear_model.LassoLarsCV):
基于最小角度回归算法Least Angle Regression，在识别更相关的alpha值上具有更大的优势，因为如果训练样本比观察样本小时，它在运行速度上比Lasso算法要快很多。

