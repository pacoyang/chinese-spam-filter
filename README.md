# 朴素贝叶斯分类器在中文垃圾邮件分类上的实现
## 介绍
朴素贝叶斯算法是一种简单有效的分类算法，被广泛运用在文本分类与垃圾邮件、信息过滤中。

## 原理
朴素贝叶斯算法基于**贝叶斯定理**与**条件独立**的假设，其中“朴素”指的就是条件独立假设。

贝叶斯定理简单来说就是这样一条概率公式：

![](https://latex.codecogs.com/gif.latex?P(A|B)=\\frac{P(B|A)P(A)}{P(B)})


其中 ![](https://latex.codecogs.com/gif.latex?P(A|B)) 指的是B发生的情况下A发生的可能性。

<!-- more -->

为了更好理解，我们代入**特征**与**分类**的概念到这个公式中，假设B指的是样本的某个类别C, A指的是样本某个特征 ![](https://latex.codecogs.com/gif.latex?w_{i})，那么这个概率公式就变成了

![](https://latex.codecogs.com/gif.latex?P(w_{i}|C)=\\frac{P(C|w_{i})P(w_{i})}{P(C)})

对式子做以下解释：


- ![](https://latex.codecogs.com/gif.latex?P(w_{i}|C))：代表类别 C 下特征 ![](https://latex.codecogs.com/gif.latex?w_{i}) 出现的条件概率，由于是在类别C下的取值，所以称作C的后验概率。

- ![](https://latex.codecogs.com/gif.latex?P(w_{i}))：代表所有样本中特征 ![](https://latex.codecogs.com/gif.latex?w_{i}) 出现的条件概率，由于不考虑任何C方面的因素，所以称作先验概率。

- ![](https://latex.codecogs.com/gif.latex?P(C|w_{i}))：代表特征 ![](https://latex.codecogs.com/gif.latex?w_{i}) 下出现是类别 C 的条件概率，由于是在特征 ![](https://latex.codecogs.com/gif.latex?w_{i}) 下的取值，所以称作特征 <img src="https://latex.codecogs.com/gif.latex?w_{i}" /> 的后验概率。

- ![](https://latex.codecogs.com/gif.latex?P(C))：就是类别C的先验概率


接下来，我们把样本的特征展开来，由 ![](https://latex.codecogs.com/gif.latex?w_{i}) 变成 ![](https://latex.codecogs.com/gif.latex?(w_{1},w_{2},...,w_{n}))，变成多个特征，那么公式就演化成了：

![](https://latex.codecogs.com/gif.latex?P((w_{1},w_{2},...,w_{n})|C)=\\frac{P(C|(w_{1},w_{2},...,w_{n}))P((w_{1},w_{2},...,w_{n}))}{P(C)})

如果我们想求某个特征下的样本属于类别C的条件概率，那将上述式子变换一下就变成了：

![](https://latex.codecogs.com/gif.latex?P(C|(w_{1},w_{2},...,w_{n}))=\\frac{P((w_{1},w_{2},...,w_{n})|C)P(C)}{P((w_{1},w_{2},...,w_{n}))})


这时，我们假设每个特征都是条件独立的，那么式子就变成了

![](https://latex.codecogs.com/gif.latex?P(C|(w_{1},w_{2},...,w_{n}))=\\frac{P(w_{1}|C)P(w_{2}|C)...P(w_{n}|C)P(C)}{P((w_{1},w_{2},...,w_{n}))})

实际中，我们只关心分式中分子的部分，由于分母所代表的特征在样本的概率，并不依赖于类别C，对于其他类别这个概率都是一样的，所以我们可以忽略掉它。


所以求某个样本属于C的概率，最终趋向于：

![](https://latex.codecogs.com/gif.latex?P(C|(w_{1},w_{2},...,w_{n})){\\varpropto}P(w_{1}|C)P(w_{2}|C)...P(w_{n}|C)P(C))

### 利用朴素贝叶斯实现中文垃圾邮件过滤
我们通过以下步骤来实现一个现实可用的垃圾邮件过滤器
1. 根据数据集产生训练集与测试集
2. 提取文本数据，对文本进行分词，过滤停用词、数字、特殊符号。
3. 统计词汇，产生词典
4. 训练分类器


### 数据集
我们使用 [滑铁卢大学](https://plg.uwaterloo.ca/~gvcormac/treccorpus06/) 提供的中文邮件数据集，数据集共有超过60000封中文邮件，并且已经做好了标注。
停用词列表使用 github 上整理的停用词库，[中文停用词](https://github.com/dongxiexidian/Chinese/blob/master/stopwords.dat) 。
我们选用其中30000封中文邮件，29400封用作训练集，600用作测试集。


### 实验结果
最后统计得出对测试集的预测准确率高达0.9766666666666667。
