## 集成方法
使用机器学习方法解决问题时，有较多模型可供选择。 
一般的思路是先根据数据的特点，快速尝试某种模型，选定某种模型后， 再进行模型参数的选择（当然时间允许的话，可以对模型和参数进行双向选择）
因为不同的模型具有不同的特点， 所以有时也会将多个模型进行组合，以发挥"三个臭皮匠顶一个诸葛亮的作用"， 
这样的思路， 反应在模型中，主要有两种思路：Bagging和Boosting  
### Bagging
Bagging 可以看成是一种圆桌会议， 或是投票选举的形式，其中的思想是："群众的眼光是雪亮的"，可以训练多个模型，之后将这些模型进行加权组合，
一般这类方法的效果，都会好于单个模型的效果。 在实践中， 在特征一定的情况下，大家总是使用Bagging的思想去提升效果。
例如kaggle上的问题解决，因为大家获得的数据都是一样的，特别是有些数据已经过预处理。  
Bagging算法可以认为每个分类器的权重都一样。  
### Boosting
在Bagging方法中，我们假设每个训练样本的权重都是一致的；
而Boosting算法则更加关注错分的样本，越是容易错分的样本，约要花更多精力去关注。
对应到数据中，就是该数据对模型的权重越大，后续的模型就越要拼命将这些经常分错的样本分正确。 
最后训练出来的模型也有不同权重，所以boosting更像是会整，级别高，权威的医师的话语权就重些。  
### 套袋元估计
在集成学习中，算法的装袋方法用原始训练集的随机子集来构造黑盒估算器的几个实例，
然后聚集它们各自的预测，以形成最后的预测算法。这些方法被用来作为一种方法来减小估计器的方差，通过引入随机化到其构造过程，然后聚集。
在许多情况下，相对于一个单一的模型，装袋的方法构造了一个很简单的方法来提高，没有使得必须适应下面的基底的算法。
为他们提供一种方式，以减少过度拟合，套袋方法在有较强的和复杂的模型中效果较好（例如，充分开发决策树），
相反boosting方法通常在弱模型工作较好（例如，浅决策树）  
### 随机树木森林
其实从直观角度来解释，每棵决策树都是一个分类器（假设现在针对的是分类问题），
那么对于一个输入样本，N棵树会有N个分类结果。而随机森林集成了所有的分类投票结果，将投票次数最多的类别指定为最终的输出，这就是一种最简单的 Bagging 思想。  
#### 特点
1. 在当前所有算法中，具有极好的准确率/It is unexcelled in accuracy among current algorithms；
2. 能够有效地运行在大数据集上/It runs efficiently on large data bases；
3. 能够处理具有高维特征的输入样本，而且不需要降维/It can handle thousands of input variables without variable deletion；
4. 能够评估各个特征在分类问题上的重要性/It gives estimates of what variables are important in the classification；
5. 在生成过程中，能够获取到内部生成误差的一种无偏估计/It generates an internal unbiased estimate of the generalization error as the forest building progresses；
6. 对于缺省值问题也能够获得很好得结果/

#### 随机森林的生成
随机森林中有许多的分类树。我们要将一个输入样本进行分类，我们需要将输入样本输入到每棵树中进行分类。
打个形象的比喻：森林中召开会议，讨论某个动物到底是老鼠还是松鼠，每棵树都要独立地发表自己对这个问题的看法，也就是每棵树都要投票。
该动物到底是老鼠还是松鼠，要依据投票情况来确定，获得票数最多的类别就是森林的分类结果。森林中的每棵树都是独立的，
99.9%不相关的树做出的预测结果涵盖所有的情况，这些预测结果将会彼此抵消。少数优秀的树的预测结果将会超脱于芸芸“噪音”，做出一个好的预测。
将若干个弱分类器的分类结果进行投票选择，从而组成一个强分类器，这就是随机森林bagging的思想（关于bagging的一个有必要提及的问题：
bagging的代价是不用单棵决策树来做预测，具体哪个变量起到重要作用变得未知，所以bagging改进了预测准确率但损失了解释性。）。  
#### 随机森林中的每棵树的按照生成规则
1. 如果训练集大小为N，对于每棵树而言，随机且有放回地从训练集中的抽取N个训练样本（这种采样方式称为bootstrap sample方法），作为该树的训练集；
从这里我们可以知道：每棵树的训练集都是不同的，而且里面包含重复的训练样本（理解这点很重要）。  
2. 如果每个样本的特征维度为M，指定一个常数m<<M，随机地从M个特征中选取m个特征子集，每次树进行分裂时，从这m个特征中选择最优的；  
3. 每棵树都尽最大程度的生长，并且没有剪枝过程。  

##### 为什么要随机抽样训练集？
如果不进行随机抽样，每棵树的训练集都一样，那么最终训练出的树分类结果也是完全一样的，这样的话完全没有bagging的必要  
##### 为什么要有放回地抽样？
如果不是有放回的抽样，那么每棵树的训练样本都是不同的，都是没有交集的，这样每棵树都是"有偏的"，都是绝对"片面的"（当然这样说可能不对），
也就是说每棵树训练出来都是有很大的差异的；而随机森林最后分类取决于多棵树（弱分类器）的投票表决，这种表决应该是"求同"，
因此使用完全不同的训练集来训练每棵树这样对最终分类结果是没有帮助的，这样无异于是"盲人摸象"。  

#### 袋外错误率（oob error）

上面我们提到，构建随机森林的关键问题就是如何选择最优的m，要解决这个问题主要依据计算袋外错误率oob error（out-of-bag error）。  
随机森林有一个重要的优点就是，没有必要对它进行交叉验证或者用一个独立的测试集来获得误差的一个无偏估计。
它可以在内部进行评估，也就是说在生成的过程中就可以对误差建立一个无偏估计。
我们知道，在构建每棵树时，我们对训练集使用了不同的bootstrap sample（随机且有放回地抽取）。
所以对于每棵树而言（假设对于第k棵树），大约有1/3的训练实例没有参与第k棵树的生成，它们称为第k棵树的oob样本。  
oob误分率是随机森林泛化误差的一个无偏估计，它的结果近似于需要大量计算的k折交叉验证。  
[详细教程及例子](http://www.cnblogs.com/maybe2030/p/4585705.html)

### AdaBoost
[sklearn.ensemble](http://scikit-learn.org/stable/modules/classes.html#module-sklearn.ensemble)包含了流行的boosting中AdaBoost算法
### 梯度推进树
梯度推进树 或渐变提振回归树（GBRT）是任意损失函数的boosting泛化。GBRT是可同时用于回归和分类问题。梯度推进树模型应用在各种领域，包括网络搜索排名和生态环境使用。  