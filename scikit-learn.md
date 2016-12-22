## An introduction to machine learning with scikit-learn

### 加载一个示例数据集
```python
    >>> from sklearn import datasets
    >>> iris = datasets.load_iris()
    >>> digits = datasets.load_digits()
    >>> print(digits.data)
    [[  0.   0.   5. ...,   0.   0.   0.]
     [  0.   0.   0. ...,  10.   0.   0.]
     [  0.   0.   0. ...,  16.   9.   0.]
     ...,
     [  0.   0.   1. ...,   6.   0.   0.]
     [  0.   0.   2. ...,  12.   0.   0.]
     [  0.   0.  10. ...,  12.   1.   0.]]
```
`digits.data`：提供可访问的对数字样本进行分类的特征

`digits.target`：提供真实的数据集	，即我们试图学习的每个数字图像对应的数字

`sklearn.svm.SVC`：分类器的实例类，，支持向量分类，构造器包含两个参数。
```python
    >>> from sklearn import svm
    >>> clf = svm.SVC(gamma=0.001, C=100.)
```
我们把我们的估计实例叫做clf，因为它是一个分类器，必须适应模型，也就是说，它必须从模型中学习。我们可以通过训练集的拟合方法实现。作为训练集，我们使用我们的数据集的所有图像除了最后一个。我们选择这个训练集的[ -1 ]：Python的语法，产生一个包含所有但不包含最后一个实体的digits.data数组
```python
    >>> clf.fit(digits.data[:-1],digits.target[:-1])
    SVC(C=100.0, cache_size=200, class_weight=None, coef0=0.0,
      decision_function_shape=None, degree=3, gamma=0.001, kernel='rbf',
      max_iter=-1, probability=False, random_state=None, shrinking=True,
      tol=0.001, verbose=False)
```
现在可以预测新的值，我们可以用分类器预测数字数据集中最后一个图像的数字是什么，而不是用来训练分类器的.： 
```python
    >>> clf.predict(digits.data[-1:])
    array([8])
```
正如你所看到的，这是一个具有挑战性的任务：图像分辨率差。你同意分类吗？ 这个分类问题的一个完整的例子可以作为一个例子，你可以运行和研究：识别[手写数字](http://scikit-learn.org/stable/auto_examples/classification/plot_digits_classification.html#sphx-glr-auto-examples-classification-plot-digits-classification-py)
### 模型的持久性

可以用Python的内置持久模型保存在scikit模型，即pickle：
```python
    >>> from sklearn import svm
    >>> from sklearn import datasets
    >>> clf = svm.SVC()
    >>> iris = datasets.load_iris()
    >>> X, y = iris.data, iris.target
    >>> clf.fit(X, y)  
    SVC(C=1.0, cache_size=200, class_weight=None, coef0=0.0,
      decision_function_shape=None, degree=3, gamma='auto', kernel='rbf',
      max_iter=-1, probability=False, random_state=None, shrinking=True,
      tol=0.001, verbose=False)
    
    >>> import pickle
    >>> s = pickle.dumps(clf)
    >>> clf2 = pickle.loads(s)
    >>> clf2.predict(X[0:1])
    array([0])
    >>> y[0]
    0
```
 对scikit而言，可以采用`joblib`更换`pickle`更有趣（`joblib.dump` & `jolib.load`），这对大数据更有效，但只能pickle到磁盘而不是一个字符串： 
```python
    >>> from sklearn.externals import joblib
    >>> joblib.dump(clf,'filename.pkl')
    ['filename.pkl', 'filename.pkl_01.npy', 'filename.pkl_02.npy', 'filename.pkl_03.npy', 'filename.pkl_04.npy', 'filename.pkl_05.npy', 'filename.pkl_06.npy', 'filename.pkl_07.npy', 'filename.pkl_08.npy', 'filename.pkl_09.npy', 'filename.pkl_10.npy', 'filename.pkl_11.npy']
```
以后你可以载入`pickle`模型（可能在另一个Python程序）与：
```python
    >>> clf = joblib.load('filename.pkl')
```
`joblib.dump` and `joblib.load` 函数接受文件对象而不是文件名。在数据持久化中`joblib`的提供更多的信息。

### 习俗

scikit估计器遵循一定的规则，使他们的行为更可预测

#### 类型构造

除非另有说明，输入将被认为`float64`： 
```python
    >>> import numpy as np
    >>> from sklearn import random_projection
    
    >>> rng = np.random.RandomState(0)
    >>> X = rng.rand(10, 2000)
    >>> X = np.array(X, dtype='float32')
    >>> X.dtype
    dtype('float32')
    
    >>> transformer = random_projection.GaussianRandomProjection()
    >>> X_new = transformer.fit_transform(X)
    >>> X_new.dtype
    dtype('float64')
```
在这个例子中，  X is `float32` ，但是被  `fit_transform(X)`转化为`float64`

回归的目标被转化为`float64`，分类目标保持原型
```python
    >>> from sklearn import datasets
    >>> from sklearn.svm import SVC
    >>> iris = datasets.load_iris()
    >>> clf = SVC()
    >>> clf.fit(iris.data, iris.target)  
    SVC(C=1.0, cache_size=200, class_weight=None, coef0=0.0,
      decision_function_shape=None, degree=3, gamma='auto', kernel='rbf',
      max_iter=-1, probability=False, random_state=None, shrinking=True,
      tol=0.001, verbose=False)
    
    >>> list(clf.predict(iris.data[:3]))
    [0, 0, 0]
    
    >>> clf.fit(iris.data, iris.target_names[iris.target])  
    SVC(C=1.0, cache_size=200, class_weight=None, coef0=0.0,
      decision_function_shape=None, degree=3, gamma='auto', kernel='rbf',
      max_iter=-1, probability=False, random_state=None, shrinking=True,
      tol=0.001, verbose=False)
    
    >>> list(clf.predict(iris.data[:3]))  
    ['setosa', 'setosa', 'setosa']
```
在这里，第一`predict()`返回一个整型数组，因为`iris.target`（一个整数数组）被用于`fit`。第二`predict()`返回一个字符串数组，因为`iris.target_names`是拟合数据。 

### 改装和更新参数

估计器的超参数可以通过`sklearn.pipeline.pipeline.set_params`方法更新，通过多次调用`fit()`函数来覆盖以前学习的`fit()`内容。
```python
    >>> import numpy as np
    >>> from sklearn.svm import SVC
    
    >>> rng = np.random.RandomState(0)
    >>> X = rng.rand(100, 10)
    >>> y = rng.binomial(1, 0.5, 100)
    >>> X_test = rng.rand(5, 10)
    
    >>> clf = SVC()
    >>> clf.set_params(kernel='linear').fit(X, y)  
    SVC(C=1.0, cache_size=200, class_weight=None, coef0=0.0,
      decision_function_shape=None, degree=3, gamma='auto', kernel='linear',
      max_iter=-1, probability=False, random_state=None, shrinking=True,
      tol=0.001, verbose=False)
    >>> clf.predict(X_test)
    array([1, 0, 1, 1, 0])
    
    >>> clf.set_params(kernel='rbf').fit(X, y)  
    SVC(C=1.0, cache_size=200, class_weight=None, coef0=0.0,
      decision_function_shape=None, degree=3, gamma='auto', kernel='rbf',
      max_iter=-1, probability=False, random_state=None, shrinking=True,
      tol=0.001, verbose=False)
    >>> clf.predict(X_test)
    array([0, 0, 0, 1, 0])
```
在这里，估计器通过`SVC()`被构造之后，默认的内核`rbf`是第一次改变成线性，并返回`rbf`重新拟合估计器，进行二次预测。 

### 多类多标签的拟合

当使用多类分类器，学习和预测任务的执行依赖于目标数据拟合后的格式： 
```python
    >>> from sklearn.svm import SVC
    >>> from sklearn.multiclass import OneVsRestClassifier
    >>> from sklearn.preprocessing import LabelBinarizer
    
    >>> X = [[1, 2], [2, 4], [4, 5], [3, 2], [3, 1]]
    >>> y = [0, 0, 1, 1, 2]
    
    >>> classif = OneVsRestClassifier(estimator=SVC(random_state=0))
    >>> classif.fit(X, y).predict(X)
    array([0, 0, 1, 1, 2])
```
在上述情况下，分类器拟合一个多标签的一维数组和predict()方法。提供相应的多类预测。它也可以拟合二维数组的二进制标签指标：
```python
    >>> y = LabelBinarizer().fit_transform(y)
    >>> classif.fit(X, y).predict(X)
    array([[1, 0, 0],
           [1, 0, 0],
           [0, 1, 0],
           [0, 0, 0],
           [0, 0, 0]])
```
在这里，分类器`fit()`二维二进制表示Y，使用`labelbinarizer`。在这种情况下`predict()`返回表示相应的多标签预测的二维数组。

请注意，第四和第五个实例返回零点，表明他们不适合三个标签上任何一个。对于多类的输出，分配多个标签也是可能的： 
```python
    >> from sklearn.preprocessing import MultiLabelBinarizer
    >> y = [[0, 1], [0, 2], [1, 3], [0, 2, 3], [2, 4]]
    >> y = preprocessing.MultiLabelBinarizer().fit_transform(y)
    >> classif.fit(X, y).predict(X)
    array([[1, 1, 0, 0, 0],
           [1, 0, 1, 0, 0],
           [0, 1, 0, 1, 0],
           [1, 0, 1, 1, 0],
           [0, 0, 1, 0, 1]])
```
在这种情况下，分类器拟合每个分配多个标签的实例。`multilabelbinarizer`用于进行二值化处理的多标签二维阵列fit。作为一个结果，`predict()`返回对每个实例多个预测标签的二维数组。
