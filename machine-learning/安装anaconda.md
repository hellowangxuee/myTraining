anaconda是一个非常齐全的python发行版本，最新的版本提供了多达195个流行的python包，包含了我们常用的numpy、scipy等等科学计算的包。
--------
### 安装教程
1. [下载地址](https://www.continuum.io/downloads)，可下载对应系统的版本
2. 安装第三方库
因为anaconda 已经包含了 scikit-learn ，所以可以直接使用
3. 进行第一个测试
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
