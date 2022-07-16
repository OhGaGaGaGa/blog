# 统计学习

![image-20220520204158609](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202205202041678.png)

可约误差和不可约误差

解决 Variance：更多的数据、正则化（regularization）
正则化主要是使拟合的model更加平滑，用来减少输入数据的影响。在损失函数中加入正则化项

解决 Bias：可以增加更多的feature作为输入，可以选择一个更加复杂的model

## Ch 3 回归分析

1. 为什么单独检验时，对某个系数的检验是显著不为0的；但是把整体建立多元回归方程之后，得到的系数对其检验，就不能拒绝它为0的假设了？或者，为什么单独检验时，得到的是正相关关系；但是多元检验之后得到的就是负相关了？

   因为多元回归中的系数，是假定其他因素固定时，这个变量对因变量的影响；如果一元回归，那没有固定其他变量的影响，会没办法揭示自变量之间的内在联系，造成对系数的判断失误。

## Ch 4 分类

### 常用分类方法

- Logistic Regression
- Linear Discriminant Analysis 线性判别分析
- Quadratic Discriminant Analysis 平方判别分析
- Naïve Bayes
- K-Nearest Neighbors
- Poisson Regression
- Generalized Additive Models (in Chapter 7)
- Random Forests (in Chapter 8)
- Boosting (in Chapter 8)
- Support Vector Machine (in Chapter 9)

### 为什么不使用 Quantitative 的线性回归？

回归问题中，$Y$ 是 Quantitative 的，定量的；分类问题中，$Y$ 是 Qualitative 的，定性的。

假设是多分类问题，那如果给定了一个 Qualitative 关系，就确定了各个分类他们之间的顺序，同时也确定了他们之间的间隔（影响权重）是相等的；但是实际中，这种顺序或者这种权重是无来由的，也是无意义的，所以我们不能把他当作 Quantitative 变量。

即使是二分类问题，也不可以，因为使用线性回归可能没办法对所有 $x$ 都得到有意义的 $\widehat{P(Y|X=x)}$，可能出现预测值超出分类范围的情况。

![image-20220410141159687](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202204101412790.png)

### Logistic Regression

#### Binomial Logistic Regression

处理二分类问题，想要从 $X$ 确定分类 $Y$，如果使用传统的线性回归 $Y = \beta_0 + \beta_1 X$ 会面临两个问题：

1. 因变量 $Y$ 本身只能取 $0,1$ 两个值，但是上面的线性回归算出的 $Y$ 是连续的。

所以我们可以通过估计出一个合理的 $\widehat{P(Y=1|X=x)}$ 的方式，估计出它之后其实就可以确定 $Y$，它可以视作 $x$ 的函数，下文中记作 $p(x)$。

我们可以根据 $p(x)$ 来判定分到哪一类，这个判定的标准不一定是 $p(x)>0.5$，可能是其他任意的。

那么，如果是线性回归，我们把模型优化为
$$
p(X) = \beta_0 + \beta_1X
$$
但是这样会出现第二个问题

2. 线性回归中，$p(X)$ 的取值范围并非 $[0,1]$。

所以我们需要改变回归形式，不妨设置回归方程为 $\mathbb{R}\to (0,1)$ 的非线性的 Logistic 函数/ Sigmoid 函数
$$
logistic(x) = \frac{1}{1 + e^{-x}} = \frac{e^x}{1 + e^x}
$$
则模型设为
$$
p(X) = \frac{e^{\beta_0 + \beta_1 X}}{1 + e^{\beta_0 + \beta_1 X}}
$$
之后，再去估计这里的 $\beta_0, \beta_1$。

我们使用最小二乘的方法来得到这里的 $\hat\beta_0, \hat\beta_1$：
$$
e^{-(\beta_0 + \beta_1 X)} = \frac{1}{p(x)} - 1 = \frac{1-p(x)}{p(x)}\\
\mathrm{logit}(p(x))\xlongequal{\triangle}\ln(\frac{p(x)}{1-p(x)}) = \beta_0 + \beta_1 X
$$
我们把左边的函数称为 log odds 或 logit，它是单调递增的，它是 Logistic 函数的反函数。

<iframe src="https://www.desmos.com/calculator/th6fbcopfs?embed" width="100" height="400" style="border: 1px solid #ccc" frameborder=0></iframe>

所以，当 $\beta_1>0$ 时，$X$ 的增大会导致 $\beta_0 + \beta_1X$ 增大，进而意味着 $p(x)$ 时在增大的；反之，负相关。

而实质上，对于任意的 $\mathbb{R}\to [0,1]$ 的函数，我们都可以构造出这样的回归。

例如，如果设标准正态分布的分布函数为 $\Phi(x)$，那么我们可以取 $p(X) = \Phi(\beta_0 + \beta_1 X)$，这被称为 Probit 回归模型（又称多元概率比回归模型）；如果取双对数变换 $f(x) = \log(-\log(1-x))$ 取代 logic 函数，则有 $\log(-\log(1-p(X))) = \beta_0 + \beta_1 X$，$p(X) = 1 - e^{-\exp(\beta_0 + \beta_1 X)}$。

而对于参数的估计，我们可以采用两种方式：LSE, MLE。下面假设各个 $x_i$ 之间都是独立的。

如果使用**最小二乘估计**，较为实用的情形为，我们已经有了 $p(x)$ 和 $x$ 的数据。这尤其适用于可以对同一组 $x$ 进行多次观测，进而使用频率估计自变量为 $x_i$ 时分类为 1 的概率 $p(x_i)$，此时把这些概率的估计记为 $\hat{p}(x_i)$。这被称为“分组数据情形”，在这种情况下，我们仅需使用 $\hat{p}(x)$ 代替 $p(x)$ 后，再对其进行变换即可直接最小二乘。

需要注意的是，这里的最小二乘实际上应为广义最小二乘（GLSE），因为对于每一个不同的 $x_i$，我们都对其观测了 $n_i$ 次，这些 $n_i$ 不一定是相等的，这导致每条数据之间并不是同方差的。可以通过 Delta 方法证明，当 $\min\{n_i\}$ 充分大时，我们可以用 $\hat{v}_i = 1/(n_i\hat{p}(x_i)(1-\hat{p}(x_i)))$ 来作为 GLSE 中使用数据 $x_i$ 时，$e_i$ 的方差。

而大部分情况下，我们无法对同一组 $x$ 进行多次观察，也就无法得到 $\hat{p}(x)$，我们仅有 $X=x_i$ 时的分类是 0 还是 1。所以，这种情况下，最小二乘估计不便使用，我们使用**极大似然估计**来解决 $\hat\beta_0, \hat\beta_1$。

显然，$y_i\sim B(1, p(x_i))$，$f(y_i) = p(x_i)^{y_i}(1-p(x_i))^{1-y_i}, y_i = 0,1$，则似然函数及对数似然函数为
$$
L(p(x_i)) = \prod_{i}p(x_i)^{y_i}(1-p(x_i))^{1-y_i}\\
l(x_i) = \sum_i [y_i\log p(x_i) + (1-y_i)\log (1-p(x_i))]
$$
把 $p(X) = \dfrac{e^{\boldsymbol{X}'\boldsymbol{\beta}}}{1 + e^{\boldsymbol{X}'\boldsymbol{\beta}}}$ 代入有
$$
l(\boldsymbol{\beta}) = \sum_i[y_i\boldsymbol{x_i}'\boldsymbol{\beta} - \log(1 + \exp(\boldsymbol{x_i}'\boldsymbol{\beta}))]
$$
对 $\boldsymbol{\beta}$ 求导，令导函数为 $0$，有
$$
\frac{\partial l(\boldsymbol{\beta})}{\partial \boldsymbol{\beta}} = \sum_i(y_i-\frac{e^{\boldsymbol{x_i}'\boldsymbol{\beta}}}{1 + e^{\boldsymbol{x_i}'\boldsymbol{\beta}}})\boldsymbol{x_i}=0
$$
可通过迭代求数值解。

#### Multinomial Logistic Regression

多分类问题中，我们有两种处理办法：基准类/Softmax 编码

**基准类法**

假设总共有 $K$ 个类，为 $1, 2, \cdots, K$，则
$$
P(Y = k|X = x) = \begin{cases}
\dfrac{e^{\boldsymbol{x}'\boldsymbol{\beta}_k}}{1 + \sum_{i=1}^{K-1}e^{\boldsymbol{x}'\boldsymbol{\beta}_i}}&, \text{for }k = 1, \cdots, K-1\\
\dfrac{1}{1 + \sum_{i=1}^{K-1}e^{\boldsymbol{x}'\boldsymbol{\beta}_i}}&, \text{for }k = K\\
\end{cases}
$$
即为（log odd 或 logic 的形式）
$$
\ln (\dfrac{P(Y = k|X = x)}{P(Y = K|X = x)}) = \boldsymbol{x}'\boldsymbol{\beta}_k
$$
这样计算出来的 $\beta_{k,i}$ 代表第 $i$ 维变化时，分类到 $k$ 相比于分类到 $K$ 的影响的可能性。

基准类需要严谨选取，因为这和我们分析系数的结论息息相关；但同时又是不重要的，因为无论基准类是什么，我们得到的结论（如进行预测等）总是一致的，因为我们实际应用时只是利用这个概率作为一个 Score，去比较相对大小。

**Softmax 编码**

![image-20220411162714347](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202204111627419.png)

## Ch 5 重采样方法

Validation Set: 把训练集分成两份，一份作为训练集，一份作为验证集

缺陷：对错误率的验证估计值可能变化很大；有一部分数据去作为验证集了，没有应用在训练中

### 交叉验证 Cross Validation

如果没有 Test Data Set，可以试着从 Train Data 里 Resample

**Leave One Out Cross Validation LOOCV**

每次找出来一个作为 Validation，用其他的 Train，然后计算每一次的 MSE；最终结果是把所有的 MSE 加起来除以个数。

缺陷：太慢

对于最小二乘线性/多项式回归

**k-fold Cross Validation**

相当于总共分成 k 份



有时需要在意估计的 MSE 的值，但有时只需要 MSE 的最小值点

从减少偏差的角度来看，很明显，LOOCV比k-fold CV更可取。然而，我们知道，在估计过程中，偏差并不是唯一值得关注的来源；我们还必须考虑程序的差异。结果表明，当k＜n时，LOOCV的方差大于k倍CV。为什么会这样？当我们执行LOOCV时，我们实际上是对n个拟合模型的输出进行平均，每个模型都在几乎相同的观测集上进行训练；因此，这些输出彼此高度（正）相关。相反，当我们在k<n的情况下执行k倍CV时，我们平均k拟合模型的输出，这些模型之间的相关性稍低

### 引导 Bootstrap

从训练数据集里抽样，算出估计的值，然后把待估值取 Mean，并计算标准差得到近似置信区间。

## Ch 6 线性回归模型的子集选择与模型正则化

### Subset Selection

最优子集选择：把所有的子集美剧出来，最后选 RSS min的。

向前子集选择：每次选一个，使得 RSS 减少最多

向后子集选择：每次尝试剔除一个，选择RSS最小的；要求 $n\geq p$，但是向前的就即使 $n<p$ 都行。

![image-20220616095537629](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202206160955756.png)

### Ridge Regression

L2 正则化
$$
\min \text{RSS} + \lambda \sum_{j=1}^p\beta_j^2
$$

要注意，并没有 $\beta_0$。缺陷：它只能把系数尽可能趋于0，而不是精确设置为0，即不能排除任何一个变量。
$$
\hat{\beta}(k) = (X'X+kI)^{-1}X'Y
$$

### Lasso

L1 正则化
$$
\min \text{RSS} + \lambda \sum_{j=1}^p|\beta_j|
$$

$\lambda$ 很大的时候，可以实现某些系数为 0。

![image-20220616090057032](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202206160900159.png)

### L1 正则化和 L2 正则化的区别（Lasso & Ridge）

![img](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202207161019563.jpeg)

https://www.zhihu.com/column/c_171321929

- 为什么 L1 易于取到 0？

  假设损失函数记为
  $$
  L(w) = f(w) + C|w|
  $$
  要让损失函数在 0 点取 $\min$，由于 0 点是间段的，所以只需使得 $L'(0^-)\times L'(0^+)<0$ 即可，即
  $$
  (f'(0)+C)\times (f'(0) - C) < 0\Leftrightarrow C>|f'(0)|
  $$
  而 L2 正则化中，由于连续性，当且仅当 $L'(0)=0$ 时，在可以在 0 处 取 $\min$，这只对应了一点，而 L1 正则化对应的是一整个区间。

- 从 Bayes 的角度理解线性回归 MLE 的 L1 正则化和 L2 正则化

  ![image-20220716102832986](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202207161028114.png)

  L1 正则化即对应这里的 $w$ 符合标准拉普拉斯分布，$p(w) = \dfrac{1}{2}\exp(-|x|)$；L2 正则化对应这里的 $w\sim N(0,1)$。

### PCR

PCR 表现不是很好的原因：数据是由多个影响因素共同生成的，如果数据是仅由少量几个影响因素生成点，那就会表现的更好。

岭回归可以视为连续版本的PCR

## Ch 7 非线性回归

多项式回归：$X, X^2, X^3$

Step functions：分段线性回归

Regression splines：综合前两种的回归

Smoothing splines, Local regression

Generalized additive models

**多项式回归**

怎样画 95% 置信区间？取每个点处的回归函数的 2 倍标准差作为界，假设误差为 $N(0,1)$，满足 $2\sigma$ 原则。

假设回归系数的协方差矩阵是 $C$，那如果 $l_0' = (1,x_0, x_0^2)'$，则回归函数的方差即为 $l_0'Cl_0$。

**分段线性回归**

一般在分界线明显的时候才来使用。

取 $C_i(x)$ 作为分段示性回归变量，然后拟合，回归出来的函数是不连续的许多平台，然后再把他们连起来。

<img src="https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202206152153020.png" alt="image-20220615215315912" style="zoom:50%;" />
$$
Y = \beta_0 + \beta_1C_1(X) + \cdots + \beta_kC_k(X) + \epsilon
$$
**基函数**

前两种回归是这种的一个特例，使用基函数的回归即
$$
Y = \beta_0 + \beta_1b_1(X) + \cdots + \beta_kb_k(X) + \epsilon
$$
基函数是确定且已知的。基函数可以是分段的。

**Regression splines**

分段拟合。自由度即拟合的分段函数中未知变量的数目，再减去约束数目。样条中，要求连续、一阶导连续、二阶导连续，所以每个连接点可以减去3个自由度。

![image-20220615222532673](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202206152225765.png)

## Ch 8 决策树

**回归树**

目标：
$$
\min RSS = \min \sum_{j=1}^J\sum_{i\in R_j}(y_i-\hat{y}_{R_j})^2
$$
自顶向下分割，每一步的选择都让 RSS 下降尽可能多。具体建树方式：
$$
\min_{j,s}\sum_{i:x_i\in \{X|X_j<s\}}(y_i-\hat{y}_{R_1})^2+\sum_{i:x_i\in \{X|X_j\geq s\}}(y_i-\hat{y}_{R_2})^2
$$
直到每个区域都没有超过 5 个观测值。

为了避免过拟合，还需要对上述算法生成的大树进行剪枝 pruning。设 $|T|$ 为末端节点分类的个数，则
$$
\min_{\alpha} \sum_{m=1}^{|T|}\sum_{i:x_i\in R_m}(y_i-\hat{y}_{R_m})^2+\alpha|T|
$$
可以使用 k-fold cross-validation 的方法确定 $\alpha$

![image-20220602120326162](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202206021203280.png)

**分类树**

损失函数：
$$
1-\max_k (\hat{p}_{m,k})
$$
$\hat{p}_{m,k}$ 代表在第 $m$ 个区域中，属于第 $k$ 类的个体所占的比例。如果 $\hat{p}_{m,k}=1$，就意味着这一区域中所有的都是 $k$ 类的。

Gini index
$$
G = \sum_{k=1}^K \hat{p}_{m,k}(1-\hat{p}_{m,k})
$$
如果 $\hat{p}_{m,k}$ 接近 0 或 1，那 $G$ 会很小。所以 $G$ 越小越好。

entropy 熵
$$
D = -\sum_{k=1}^K\hat{p}_{m,k}\log \hat{p}_{m,k}
$$
如果 $\hat{p}_{m,k}$ 接近 0 或 1，那 $D$ 会很小。所以 $D$ 越小越好。

不过这些目标函数都过于关注纯净性，而不太关注预测的准确度。所以建树的时候还是会使用 Cross Validation Error。

如果在分类边界非线性的情形下，决策树效果更好；但如果分类的边界是线性的，那线性回归就够。

![image-20220616101001906](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202206161010053.png)

**Bagging - Bootstrap Aggregation**

朴素的决策树可能有很高的方差。

要想减少方差，一个朴素的想法就是用不同的数据训练出来好多模型，然后取这些模型得到的结果的平均值，这样就能减少方差。

对于决策树而言，每棵决策树可以不用剪枝，因为最后的结果会做平均。

**Boosting**

**提升方法（Boosting）**是一种可以用来减小监督学习中偏差的机器学习算法。主要也是学习一系列弱分类器，并将其组合为一个强分类器。Boosting中有代表性的是 **AdaBoost（Adaptive boosting）算法**：刚开始训练时对每一个训练例赋相等的权重，然后用该算法对训练集训练t轮，每次训练后，对训练失败的训练例赋以较大的权重，也就是让学习算法在每次学习以后更注意学错的样本，从而得到多个预测函数。

**Bagging与Boosting**

Bagging和Boosting采用的都是**采样-学习-组合**的方式，但在细节上有一些不同，如

- Bagging中每个训练集互不相关，也就是每个基分类器互不相关，而Boosting中训练集要在上一轮的结果上进行调整，也使得其不能并行计算
- Bagging中预测函数是均匀平等的，但在Boosting中预测函数是加权的

**随机森林**属于 集成学习 中的 Bagging（Bootstrap Aggregation 的简称） 方法

1. 它可以出来很高维度（特征很多）的数据，并且不用降维，无需做特征选择
2. 它可以判断特征的重要程度
3. 可以判断出不同特征之间的相互影响
4. **不容易过拟合**
5. 训练速度比较快，容易做成并行方法
6. 实现起来比较简单
7. 对于不平衡的数据集来说，它可以平衡误差。
8. 如果有很大一部分的特征遗失，仍可以维持准确度。

缺陷：

1. 随机森林已经被证明在某些噪音较大的分类或回归问题上会过拟合。
2. 对于有不同取值的属性的数据，取值划分较多的属性会对随机森林产生更大的影响，所以随机森林在这种数据上产出的属性权值是不可信的

![image-20220616095706659](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202206160957802.png)

**Bayesian Additive Regression Trees** BART



## Ch 9 SVM

**最大间隔超平面**：

![image-20220615224249693](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202206152242786.png)

如果线性不可分，那 $M<0$。因为 $M$ 的含义是支持向量到超平面的距离。

**SVM**：

![image-20220615224610644](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202206152246719.png)

边缘的错误一侧和超平面的错误一侧，是不同的：$\epsilon<0$ 正确分类，$0<\epsilon<1$ 是在边缘的错误一侧，$\epsilon>1$ 是在超平面的错误一侧

C 是通过交叉验证确定的超参数

位于正确分类的点，根本不会影响SVM。所以只有那些在边缘里的，才被叫做支持向量。

**非线性：**

增加特征，加入 $x_i^2$ 这样变成 $2p$ 个特征。

**核函数**

我们想要扩大特征空间，以适应类之间的非线性边界。

实际起作用的只有几个支持向量，待估的 $x$ 是和这些支持向量做内积然后求加权和。

预测函数：
$$
f(x) = \beta_0 + \sum_{\text{support vector }v}\alpha_i<x, v>\xlongequal{\triangle} \beta_0 + \sum_{\text{support vector }v}\alpha_iK(x, v)
$$
线性 Linear Kernel: 
$$
K(x_i, x_j) = \sum_{k=1}^p x_{ik}x_{jk}
$$
多项式 Polynomial Kernel: 
$$
K(x_i, x_j) = (1 + \sum_{k=1}^p x_{ik}x_{jk})^d
$$
径向 Radial Kernel: Radial Basis Function, RBF kernel
$$
K(x_i, x_j) = \exp\left(-\gamma \sum_{k=1}^p (x_{ik} - x_{jk})^2\right)
$$
欧几里得距离越大，这个径向核函数越小

## Ch 10 ANN

多层感知机

实际上就是把一些特征加权求和之后，通过一个激活函数 Threshold Function；然后再到下一层..

Feed-Forward Network 前馈神经网络：从输入开始，一层一层往后算

Feedback Network 反馈神经网络：不同循环之间有依赖，可以有更复杂的函数

训练方法：

一层：对 Logistic 函数求导；使用Batch梯度下降/在线梯度下降

反向传播 Back propagation

学习一个 AND 分类的感知机：

![image-20220615145151035](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202206151451182.png)

但是单层感知机没办法处理亦或。

多层感知机就可以处理亦或了。

![image-20220615150118482](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202206151501586.png)

![image-20220616102208557](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202206161022694.png)

## Ch 12 无监督学习

我们对预测不感兴趣，因为并没有相应的响应变量 Y；我们只是想发现解释变量上有哪些可以分类总结的规律。

更主观，分析没有简单的目标

没有普遍接受的机制来执行交叉验证或在独立数据集上验证结果

### PCA

我们把 $(x_1, \cdots, x_n)$ 映射到第一主成分向量上 $(\phi_1, \cdots, \phi_n)$，得到的投影就是第一主成分得分 $z_{11}, z_{21}, \cdots, z_{n1}$。

### 聚类

特点：小决策产生大后果

k means 层次聚类

![image-20220616083318771](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202206160833821.png)

Hierarchical 聚类 层次聚类：**Agglomerative Clustering** 自底而上，divisive 自顶而下

**Single-linkage**:要比较的距离为元素对之间的最小距离
**Complete-linkage**:要比较的距离为元素对之间的最大距离
**Group <u>average</u>**：要比较的距离为类之间的平均距离：算A组中的观测值与B组中的观测值之间的所有成对不相似性，并记录这些不相似性的平均值

Centroid：质心的距离



Dendrogram：层次聚类的树状图

k-means：

优点：
2，时间复杂度低
缺点：
1，需要对均值给出定义,
2，需要指定要聚类的数目；
3，一些过大的异常值会带来很大影响；
4，算法对初始选值敏感；
5，适合球形聚类

层次聚类：

优点：
1，距离和规则的相似度容易定义，限制少；
2，不需要预先制定聚类数；
3，可以发现类的层次关系；
4，可以聚类成其它形状

缺点：
1，计算复杂度太高；
3，算法很可能聚类成链状

## KNN

KNN算法实现方式
蛮力实现(brute)：计算预测样本到所有训练集样本的距离，然后选择最小的k个距离即可得到K个最邻近点。缺点在于当特征数比较多、样本数比较多的时候，算法的执行效率比较低；
KD树(kd_tree)：KD树算法中，首先是对训练数据进行建模，构建KD树，然后再根据建好的模型来获取邻近样本数据。
除此之外，还有一些从KD_Tree修改后的求解最邻近点的算法，比如:Ball Tree、 BBF Tree、MVP Tree等。
KD Tree构建
    KD树采用从m个样本的n维特征中，分别计算n个特征取值的方差，用方差最大的第k维特征nk作为根节点。对于这个特征，升序排列后选择取值的中位数nkv作为样本的划分点，对于小于该值的样本划分到左子树，对于大于等于该值的样本划分到右子树，对左右子树采用同样的方式找方差最大的特征作为根节点，递归即可产生KD树。

KD Tree查找最近邻
    当我们生成KD树以后，就可以去预测测试集里面的样本目标点了。对于一个目标点，我们首先在KD树里面找到包含目标点的叶子节点。以目标点为圆心，以目标点到叶子节点样本实例的距离为半径，得到一个超球体，最近邻的点一定在这个超球体内部。然后返回叶子节点的父节点，检查另一个子节点包含的超矩形体是否和超球体相交，如果相交就到这个子节点寻找是否有更加近的近邻,有的话 就更新最近邻。如果不相交那就简单了，我们直接返回父节点的父节点，在另一 个子树继续搜索最近邻。当回溯到根节点时，算法结束，此时保存的最近邻节点 就是最终的最近邻。
假设样本集为：{(2,3), (5,4), (9,6), (4,7), (8,1), (7,2)}。构建过程如下：

（1）确定split域，6个数据点在x,y维度上的数据方差分别为39, 28.63。在x轴上方差最大，所以split域值为0（x维的序号为0）

（2）确定分裂节点，根据x维上的值将数据排序，则6个数据点再排序后位于中间的那个数据点为(7,2)，该结点就是分割超平面就是通过(7,2)并垂直于split=0(x)轴的直线x=7

（3）左子空间和右子空间，分割超面x=7将整个空间氛围两部分，x<=7的部分为左子空间，包含3个数据点{(2,3), (5,4), (4,7)}；另一部分为右子空间，包含2个数据点{(9,6), (8,1)}。

（4）分别对左子空间中的数据点和右子空间中的数据点重复上面的步骤构建左子树和右子树直到经过划分的子样本集为空。

![](https://img-blog.csdn.net/20180613230844360?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE1MTExODYx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

我们来查找点(2.1,3.1)，在(7,2)点测试到达(5,4)，在(5,4)点测试到达(2,3)，然后search_path中的结点为<(7,2), (5,4), (2,3)>，从search_path中取出(2,3)作为当前最佳结点nearest, dist为0.141；

然后回溯至(5,4)，以(2.1,3.1)为圆心，以dist=0.141为半径画一个圆，并不和超平面y=4相交，如下图，所以不必跳到结点(5,4)的右子空间去搜索，因为右子空间中不可能有更近样本点了。

![image](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/202206160928843.png)

于是在回溯至(7,2)，同理，以(2.1,3.1)为圆心，以dist=0.141为半径画一个圆并不和超平面x=7相交，所以也不用跳到结点(7,2)的右子空间去搜索。

至此，search_path为空，结束整个搜索，返回nearest(2,3)作为(2.1,3.1)的最近邻点，最近距离为0.141。

再举一个稍微复杂的例子，我们来查找点(2,4.5)，在(7,2)处测试到达(5,4)，在(5,4)处测试到达(4,7)，然后search_path中的结点为<(7,2), (5,4), (4,7)>，从search_path中取出(4,7)作为当前最佳结点nearest, dist为3.202；

然后回溯至(5,4)，以(2,4.5)为圆心，以dist=3.202为半径画一个圆与超平面y=4相交，如下图，所以需要跳到(5,4)的左子空间去搜索。所以要将(2,3)加入到search_path中，现在search_path中的结点为<(7,2), (2, 3)>；另外，(5,4)与(2,4.5)的距离为3.04 < dist = 3.202，所以将(5,4)赋给nearest，并且dist=3.04。

![](http://img1.51cto.com/attachment/201110/13/2531780_1318510004n63e.png)

回溯至(2,3)，(2,3)是叶子节点，直接平判断(2,3)是否离(2,4.5)更近，计算得到距离为1.5，所以nearest更新为(2,3)，dist更新为(1.5)

回溯至(7,2)，同理，以(2,4.5)为圆心，以dist=1.5为半径画一个圆并不和超平面x=7相交, 所以不用跳到结点(7,2)的右子空间去搜索。

至此，search_path为空，结束整个搜索，返回nearest(2,3)作为(2,4.5)的最近邻点，最近距离为1.5。
