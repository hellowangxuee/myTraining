## Kernel Approximation

[网页链接](http://scikit-learn.org/stable/modules/kernel_approximation.html)

该子模块包含近似的对应特定内核功能的映射，因为它们是用于支持向量机中的例子（[见支持向量机](http://scikit-learn.org/stable/modules/svm.html#svm)）。以下特征函数对输入进行非线性变换，它可以作为线性分类或其他算法的基础。

使用近似明确特征以映射相比[核技巧](https://en.wikipedia.org/wiki/Kernel_trick)的优点，是一个明确的映射可能更适合在线学习和能显著减少大型数据集的学习成本，这使得使用的特征映射含蓄。核心化标准支持向量机不能很好地扩展到大型数据集，但使用近似核映射，可以使用更有效的线性支持向量机。特别是，内核近似映射与` SGDClassifier`组合可以使对大型数据集的非线性学习成为可能。

由于没有大量的经验工作中使用近似的嵌入，建议在可能的情况来比较准确的核方法的结果


### Nystroem 方法核近似

所述Nystroem方法，如在实施[Nystroem](http://scikit-learn.org/stable/modules/generated/sklearn.kernel_approximation.Nystroem.html#sklearn.kernel_approximation.Nystroem)为内核的低秩近似的一般方法。它由在其内核评估的数据上子采样实现这一点。默认Nystroem使用`rbf`的内核，但它可以使用任何核函数或一个预先计算的内核矩阵。使用的样本数-也是计算出的特征的维数-由参数给出n_components。

### 径向基函数核

所述[RBFSampler](http://scikit-learn.org/stable/modules/generated/sklearn.kernel_approximation.RBFSampler.html#sklearn.kernel_approximation.RBFSampler)构建一个近似映射的径向基函数内核，也被称为随机厨房水槽 [RR2007](http://scikit-learn.org/stable/modules/kernel_approximation.html#rr2007) 。该转化可用于明确建模内核映射，例如线性SVM之前应用线性算法。



映射依赖于一个蒙特卡洛近似内核值。`fit`函数执行蒙特卡罗抽样，而`transform`方法执行数据的映射。因为该过程的固有的随机性，不同的调用`fit`函数结果可能会不同。

该`fit`函数有两个参数： `n_components`，这是特征转换的目标维度和`gamma`时，RBF内核的参数。较高`n_components`将导致内核更好的近似，并会产生结果与一个内核SVM产物的更相似。需要注意的是“fitting”的特征函数实际上并不依赖于给`fit`的数据。仅依赖使用的数据的维数。关于该方法的细节可参见[RR2007] 。

对于一个给定的值`n_components` `RBFSampler`常常比 `Nystroem` 更不准确。虽然 `RBFSampler` 计算成本低，利用较大特征的空间更高效的。




