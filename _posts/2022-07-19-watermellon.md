---
layout: post
title: watermellon book
author: huyi
---
<head>
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script type="text/javascript" id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js">
</script>
</head>
<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>
[toc]
----
# chap1 绪论
## 1.1 引言
* **learning algorithm:** 从数据中产生模型的算法
* 有些文献用“模型”指全局性结果，e.g.决策树；用“模式”指代局域性结果。

## 1.2 基本术语
* **数据集 dataset**：
  很多个事件/对象对应的数据
    >e.g.(色泽：绿，敲声：响)，(色泽：绿，敲声：响)

* **示例 instance/ 样本 sample**：
  数据集中关于一个事件/对象的描述

* **属性 attribute/ 特征 feature**：
  反映事件或对象在某方面的表现或性质的事项
  >e.g. 色泽，敲声

* **属性值 attribute value**

* **属性空间(attribute space)/ 样本空间(sample space)**
  把attribute作为坐标轴，则它们张成一个用于描述对象空间，每个对象对应这个空间中的一个点，一个instance称为一个特征向量(feature vector)

* $D=\{x_1,x_2,…，x_m\}$表示有m个示例的数据集，每个示例由d个属性描述，$x_i=(x_{i1};x_{i2},…，x_{id})$.
* d是x_i的 **维数**
* **hypothesis**-->**ground truth**
* **label**
* **样例 example**：
  有 label 的 sample，一般用$(\textbf{x}_i,y_i)$表示第i个样例
    * 标记空间$y_i \in \mathcal{Y}$

  ----

* **classification**
  预测值是离散值
  * binary classification
  * multi-class classification

* **regression**
  预测值是连续值

* **clustering**
  unsupervised 的代表。

  前两个任务是supervised的代表

## 1.3 假设空间
* 机器学习是 **归纳学习(inductive learning)**
  * 广义的归纳学习：从样例中学习
  * 狭义的归纳学习：概念学习/概念形成
    * 最基本的： 布尔概念学习
* 可以把学习过程看作一个在所有 hypothesis 组成的空间中进行搜索的过程，搜索目标是找到fit training set 的 hypothesis
  * 可能可以找到多个和训练集一致的 hypothesis --> 假设集合 --> **版本空间 version space**

### 1.4 归纳偏好

？ 要解决的问题：如何在版本空间中进行选择 ？
--> **归纳偏好 inductive bias**：机器学习算法在学习过程中对某种类型假设的偏好
* 一般性原则来引导算法建立正确的偏好：
  * 奥卡姆剃刀原理
    >需要注意奥卡姆剃刀原理有着在不同情况下有着不同的诠释，譬如说两个看起来差不多的假设哪个更简单这个问题，在不同模型中是不一样的
* NFL定理 (No Free Lunch Theorem :stuck_out_tongue_winking_eye:)
  >假设样本空间 $\mathcal{X}$ 和假设空间 $\mathcal{H}$ 都是离散的。令 $P(h|X,\mathcal{L}_a)$ 代表算法$\mathcal{L}_a$ 基于训练数据$X$产生假设h的概率，再令f代表我们希望学习的真实目标函数。$\mathcal{L}_a$的训练集外误差：
  \[E_{ote}(\mathcal{L}_a|X,f)=\sum\limits_{h}\sum\limits_{x\in\mathcal{X}-X}\mathbb{I}(h(x)\neq f(x))P(h|X,\mathcal{L}_a)\]
  ![image.png](https://s2.loli.net/2022/07/19/oq7RlWPnBxHVJXi.png)

  :thought_balloon: <font color=NavyBlue>f 作为希望学习的真实目标函数，在这边取所有的可能函数，这并不能说明所有算法针对某一问题误差恒定，而是说，某个算法必不可能在所有问题上取得小误差，擅长一些问题便必然不擅长另一些 ~</font>

  >上面的式子说明的问题是总误差与学习算法无关！
  >-->脱离具体问题，空谈什么算法更好没有意义！

### chap1 习题
#### 1.1

<table align="center">
  <tr><th align="center">编号</th><th align="center">色泽</th><th align="center">根蒂 </th><th align="center">声音</th><th align="center">好瓜？</th></tr>
  <tr><td>1</td><td>青绿</td><td>蜷缩</td><td>浊响</td><td>是</td></tr>
  <tr><td>2</td><td>乌黑</td><td>稍蜷</td><td>沉闷</td><td>否</td></tr>

</table>

**上述表格对应的版本空间？假设三个attributes都只有三种书上给出的取法？**

:thinking:
| 编号 | 色泽 | 根蒂 | 声音 |
| ---- | ---- | ---- | ---- |
| 1    | 青绿 | 蜷缩 | 浊响 |
| 2    | *    | 蜷缩 | 浊响 |
| 3    | 青绿 | *    | 浊响 |
| 4    | 青绿 | 蜷缩 | *    |
| 5    | *    | *    | 浊响 |
| 6    | 青绿 | *    | *    |
| 7    | *    | 蜷缩 | *    |
这里假定不包含非A的操作。

#### 1.2
每个attribute：$3+3+1=7$,$7*7*7+1$

#### 1.3
选择假设使得训练错误累计最小。

# chap2 模型评估与选择
## 2.1 经验误差与过拟合
* **错误率 error rate：**
  分类错误样本数占总数的比例：$E=a/m$
* **精度 accuracy:**
  $1-a/m$
* **训练误差(经验误差) training error/ 泛化误差 generalization error**
  * 实际训练中，我们通常只能找一个训练误差极小的learner，但通常经验误差很小的leaner很可能泛化误差不小
* **过拟合 overfitting/ 欠拟合 underfitting：**
  过拟合是无法避免的，只能尽量减轻--> $P\neq NP$

## 2.2 评估方法
### 2.2.1 留出法
把数据集分成两个互斥集合——训练集$S$ 和测试集$T$
* 需要保证测试集和训练集数据分布尽可能一致 --> 分层采样
* 一般用 $2/3$ 或 $4/5$ 的样本用于训练
### 2.2.2 交叉验证法（cross validation）
* **k折交叉验证 k-fold cross validation**
  * 先把数据集$D$分为k个大小相似的互斥子集。
  * 每个子集数据分布尽量一致 --> 分层采样
  * 每次用k-1个子集的并集作为训练集，剩下的子集作为测试集
  * 这样就可以获得k组训练/测试集
  * 可以进行k次训练/测试
  * 最终返回k次测试结果的均值
  * 叫 **k折交叉验证 k-fold cross validation**
  * 通常用10(5,20)
* **p次k折交叉验证**
  * 将数据集$D$划分为k个子集有多种方式，随机使用不同的划分p次，最后的评估结果是p次交叉验证结果的均值
  * 常用“10次10折交叉验证”
* **留一法 Leave-One-Out (LOO)**
  * 每一折里面只有一个sample
  * 评估比较精确
  * 但是训练复杂度在dataset比较大的时候比较高

### 2.2.3 自助法（bootstrapping）
适用于数据集较小，无法有效划分训练集和测试集的时候
### 2.2.4 调参与最终模型
**validation set：** 模型评估与选择中用于评估测试的数据集

## 2.3 性能度量 (performance measure)
给定样例$D=\{(\textbf{x}_1,y_1),…,(\textbf{x}_m,y_m)\}$,需要量化f(x)与y的差别。
* 回归任务中，常用“均方误差”(mean squared error):
\[E(f;D)=\frac{1}{m}\sum\limits_{i=1}^{m}(f(x_i)-y_1)^2\]

* 连续的均方误差\[E(f;D)=\int_{x\sim D}(f(x)-y)^2p(x)\rm{d}x\]

### 2.3.1 错误率与精度
对样例$D$，
* 分类错误率
  * \[E(f;D)=\frac{1}{m}\sum\limits_{i=1}^{m}\mathbb{I}(f(x_i)\neq y_i)\]
  * \[E(f,D)=\int_{x\sim D}\mathbb{I}(f(x_i)\neq y_i)p(x)\rm{d}x\]
* 精度
  * \[acc(f;D)=\frac{1}{m}\sum\limits_{i=1}^{m}\mathbb{I}(f(x_i)= y_i)=1-E\]
  * 连续谱情况也是$1-E$

### 2.3.2 查准率、查全率与 F1
问题：
1. 检索出的信息中有多少比例是用户感兴趣的？
2. 用户感兴趣的信息中有多少被检索出来了？

  --> **查准率 precision**、**查全率 recall**
  下面这个表叫 **二分类混淆矩阵**
<table>
	<tr>
	    <td rowspan=2>真实情况</th>
	    <td colspan="2">预测结果</th>
	</tr >
	<tr >
	    <td>正例</td>
	    <td>反例</td>
	</tr>
	<tr>
	    <td>正例</td>
	    <td>TP（真正例）</td>
      <td>FN（假反例）</td>
	</tr>
	<tr>
	    <td>反例</td>
	    <td>FP（假正例）</td>
      <td>TN（真反例）</td>
	</tr>
</table>

* **查准率 precision**
  \[P=\frac{TP}{TP+FP}\]
* **查全率 recall**
  \[R=\frac{TP}{TP+FN}\]
* 通常查准率和查全率是矛盾的，一个高一个就低
* **PR图**
  根据learner的预测结果对样例进行排序，排在前面的是learner认为“最可能”是正例的样本，排在最后的是learner认为“最不可能”是正例的样本。按此顺序逐个把样本作为正例进行预测按此顺序逐个把样本作为正例进行预测，则每次可以计算出当前的查全率、查准率 --> P是纵轴，R是横轴，则可以作出PR图。
  <br>过（0，1）和（1，0）
* **通过PR图判断learner的性能：**
  * 若一个学习器的P-R曲线被另一个学习器完全“包住”，则可断言后者性能优于前者 --> 若有交叉？
  * **平衡点 Break-Even Point (BEP)**
    “查准率=查全率”时的取值，越高越好
  * **F1度量**
    \[F_1=\frac{2\times P\times R}{P+R}=\frac{2\times TP}{样例总数+TP-TN}\]
    基于P和R的`调和平均`定义：
    \[\frac{1}{F_1}=\frac{1}{2}(\frac{1}{P}+\frac{1}{R})\]
  * 更一般地，**$F_{\beta}$**
    \[\frac{1}{F_{\beta}}=\frac{1}{1+\beta^2}(\frac{1}{P}+\frac{\beta^2}{R})\]
    $\beta>1$时，查全率有更大影响；
    $\beta<1$时，查准率有更大影响。
* 多个二分类混淆矩阵的情况
  例如多次训练/测试，或在多个训练集上训练/测试
  * **宏查准率 macro-P**、**宏查全率macro-R**、**宏F1**
    在多个二分类混淆矩阵上计算出P、R，然后取平均。
    \[macro-P=\frac{1}{n}\sum_{i=1}^{n}P_i\]
    \[macro-R=\frac{1}{n}\sum_{i=1}^{n}R_i\]
    \[macro-F_1=\frac{2\times macroP\times macroR}{macroP+macroR}\]
  * **微查准率 micro-P**、**微查全率micro-R**、**微F1**

### 2.3.3 ROC 与 AUC
<font color=NavajoWhite size=10>to be continued</font>

# chap3 线性模型
## 3.1 基本形式
给定由d个属性描述的示例$\textbf{x}=(x_1,…,x_d)$，线性模型试图学得一个通过属性线性组合来预测的函数，
\[f(\bm{x})=w_1x_1+w_2x_2+…+w_dx_d+b\]
向量形式写作：
\[f(\bm{x})=\bm{w}^T\bm{x}+b\]
## 3.2 线性回归
\[D=\{(\bm{x_1},y_1),…,(\bm{x_m},y_m)\}\]
我们要找一个$f(x_i)$