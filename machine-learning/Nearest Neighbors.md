## 最近邻
[`sklearn.neighbors`](http://scikit-learn.org/stable/modules/classes.html#module-sklearn.neighbors)
基于邻居的方法对无监督和监督学习方法提供功能。无监督的近邻是许多其他的学习方法的基础，尤其是多方面的学习和谱聚类的基础。
监督邻居学习基于两种形式：有离散标记中的数据的分类，和有连续标签数据的回归。  
### 无监督近邻
[`NearestNeighbors`](http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.NearestNeighbors.html#sklearn.neighbors.NearestNeighbors)
实现了无监督近邻学习。它作为一个三种不同的近邻算法统一的接口：`BallTree`，`KDTree`，和基于[sklearn.metrics.pairwise](http://scikit-learn.org/stable/modules/classes.html#module-sklearn.metrics.pairwise)的`a brute-force algorithm`算法。
邻居搜索算法的选择是通过关键字'algorithm'来控制，它必须是` ['auto', 'ball_tree', 'kd_tree', 'brute']`中的一个 。
当默认值`auto`被传递，该算法试图为训练数据的匹配最佳算法。  
### KDTree and BallTree Classes
使用[`KDTree`](http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KDTree.html#sklearn.neighbors.KDTree)
或[`BallTree`](http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.BallTree.html#sklearn.neighbors.BallTree)
直接类找到最近的邻居。这在[`NearestNeighbors`](http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.NearestNeighbors.html#sklearn.neighbors.NearestNeighbors)
封装好了。球树和KD树具有相同的接口。  
### 最近邻分类
它并不试图构建一个通用的内部模型，只是存储训练数据的实例。
分类是对每个点的近邻的简单多数表决来计算：查询点被分配具有该点的近邻中最代表的数据类。  
scikit学习工具两种不同近邻分类器： 
[`KNeighborsClassifier`](http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html#sklearn.neighbors.KNeighborsClassifier)
基于每个查询的点k个最近的邻居实现学习，其中的近邻ķ是由用户指定的一个整数值。
[`RadiusNeighborsClassifier`](http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.RadiusNeighborsClassifier.html#sklearn.neighbors.RadiusNeighborsClassifier)
基于训练点 r 半径范围的邻居数实现学习 其中的R是由用户指定的一个浮点值。  
在数据不均匀采样的情况下，基于半径的邻居分类`RadiusNeighborsClassifier`是一个更好的选择  
### 最近邻回归
基于邻居的回归可以在数据标签连续的而不是离散变量的情况下使用。分配给一个查询点的标签是基于其最近邻的标签的平均值。  
scikit 实现两种不同的邻居回归：
[`KNeighborsRegressor`](http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsRegressor.html#sklearn.neighbors.KNeighborsRegressor)
[`RadiusNeighborsRegressor`](http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.RadiusNeighborsRegressor.html#sklearn.neighbors.RadiusNeighborsRegressor)
