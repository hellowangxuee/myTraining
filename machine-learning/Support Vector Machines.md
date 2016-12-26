## 支持向量机
支持向量机（SVM）是一套用于监督学习方法的分类， 回归和异常值检测。  
### 优点：
1. 高维空间仍旧有效。
2. 当维数大于样品的数量仍有效。
3. 决策函数（称为支持向量）中使用训练点的子集，所以它也有记忆效率。
4. 通用性：决策函数可以指定不同[内核函数](http://scikit-learn.org/stable/modules/svm.html#svm-kernels)
。普通内核已经被提供，但也可以指定自定义内核。  

### 缺点:
1. 如果特征的数目大于样品的数量大得多，该方法很可能会有很差的性能。
2. 支持向量机不直接提供的概率估计，这些使用昂贵的五倍交叉验证（见计算[得分和概率](http://scikit-learn.org/stable/modules/svm.html#scores-probabilities)）

### 分类
[SVC](http://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html#sklearn.svm.SVC)，
[NuSVC](http://scikit-learn.org/stable/modules/generated/sklearn.svm.NuSVC.html#sklearn.svm.NuSVC)，
[LinearSVC](http://scikit-learn.org/stable/modules/generated/sklearn.svm.LinearSVC.html#sklearn.svm.LinearSVC)
能够对数据集执行多类分类的类。  

SVC 和 NuSVC 是类似的方法，但是接受稍有不同的参数集，并有不同的[数学公式](http://scikit-learn.org/stable/modules/svm.html#svm-mathematical-formulation)。
另一方面，LinearSVC 为线性核的情况下支持向量分类的另一种实现。需要注意的是 LinearSVC不接受的关键字`kernel`，因为这被认为是线性的。
它还缺少一些 SVC和NuSVC 中的成员，比如：`support_`。  
至于其他的分类，SVC，NuSVC并 LinearSVC 输入为两个array数组：训练样本array X of size[n_samples, n_features]，
类标签（字符串或整数）array y of size[n_samples]  

被`fit`后，该模型然后可以用于预测`predict`新值  

支持向量机决策函数取决于训练数据的一些子集，被称为支持向量。这些支持向量的某些属性可以在成员中找到`support_vectors_`，`support_`并且 `n_support`  
### 多类分类
SVC 和 NuSVC 实现“一对一”的方式（Knerr等，1990）用于多类分类。如果 `n_class` 是类号码，然后`n_class * (n_class - 1) / 2`
分类器被并且每个训练数据来自两个类。为了提供与其他分类器一致的界面，`decision_function_shape`选项提供合并“一对一”分类器结果  
另一方面，LinearSVC 实现了“一对多”多级的策略，从而训练n_class模型。如果只有两个类，只有一个模型被训练  

注意，LinearSVC还实现了一个可选的多类的策略，即所谓的多类SVM通过Crammer和Singer，配制通过使用选项`multi_class='crammer_singer'`。
这种方法是一致的，这对“一对多”分类是不正确的。在实践中，“一对多”分类通常是优选的，因为其结果是大多相似，但运行时间显著较小。  
### 得分和概率
SVC 的方法`decision_function`为每个样品给出了每个类的分数（或在二进制的情况下每个样品的单个分数）。
当构造选项`probability`被设置为True，类别成员概率的估计（方法`predict_proba`和`predict_log_proba`）被启用。
在二类情况下，概率使用普拉特缩放校准，在 SVM 的分数上的logistic回归，拟合通过对训练数据的附加交叉验证。在多类的情况下，可以由一个来扩展。  
### 不平衡问题
在其中希望给予某些类或某些个别样本关键字的设置重要性，可以使用`class_weight`和`sample_weight`。  
