## Ch 1

平稳序列的定义、会证平稳序列

常见的平稳序列：线性和、调和平稳序列（决定性的）

自协方差函数的性质：对称性、非负定（什么时候正定：趋于0，..）、有界性

Cauchy-Schwarz 不等式

协方差矩阵的一些性质

白噪声（谱密度是常数）

正交、不相关（零均值 正交等价于不相关）

平稳序列的和、乘积（独立才还是平稳序列，从E的角度）

### 平稳序列

控制收敛定理、单调收敛定理

绝对可和：L1收敛，a.s.收敛

平方可和：L2收敛

了解一下严平稳序列的一系列内容

### 谱函数

谱分布函数和自协方差函数是一一对应的

平稳序列一定有谱分布函数

线性平稳序列（和式）的谱密度，常常把 $e^{i\lambda}$ 记成 $z$ 计算方便

相互正交+常数：谱分布函数 谱密度函数 都是线性的



## Ch 2

齐次常系数线性差分方程

会叙述什么是AR(p)模型：白噪声、最小相位条件、差分方程

平稳解：A(z)的Taylor展开，也可以用Wold系数

谱密度函数

自协方差函数和谱密度函数的关系（记得 $\gamma_{-k} = \gamma_k$）

要想计算 AR(p) 的协方差函数，用Yule-Walker方程，经常用推论；自回归系数怎么算

偏相关系数，p后截尾的

不用记Levinson递推

考试第一题，直接用解方程更加方便

AR(p) 充要条件：偏相关系数 $a_{n,n}$ p截尾



## Ch 3

要会证：MA(q) 的充要条件：自协方差函数是 q后截尾的

定义：白噪声、不在单位圆内-保证MA(q)的唯一性、滑动平均方程

可逆的：在单位圆外

倒过来的基本思想：把谱密度函数倒过来，然后再凑/MA(1) 可以设系数然后待定系数解方程

![image-20211229104259390](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2021/12/202112291043571.png)

2 可以不用递推？

3 有什么简单的方法？

4 $X_t = \epsilon_t + b\epsilon_{t-1}$，有 $(1 + b^2)\sigma^2 = 1, b\sigma^2 = \rho$，有解，$\Delta\geq 0$。

或 $E(X_t - yX_{t-1})^2\geq 0, \forall y$。

5  $(1 + b^2)\sigma^2 = 1, b\sigma^2 = \rho$

eg

$$
\gamma_0 = 1.2\\
\gamma_k = 0.9\gamma_{k-1} - 0.1\gamma_{k-2},k\geq 1
$$

求证AR(2)，并给出AR(2)模型

解：

$$
令 \varepsilon_t = X_t - 0.9X_{t-1} + 0.2X_{t-2}，证它是白噪声
$$

法2：算出 $\{\gamma_k\}$，再推谱密度函数

## ARMA(p,q)

叙述：没有公共根、单位圆外、不在单位圆内，差分方程

通解、平稳解

一般算到 $\psi(2)$ 就行，如果直接要求算系数那就直接级数展开/递推

延拓的YW方程，算自回归系数，要证矩阵是正定的

考试的时候可以用引理2.2，来证正定

要会证 $\Gamma_{m,q}$ 可逆

如果已经有了AR部分，MA怎么证？定理2.4



谱密度函数

可逆的ARMA模型

eg 小测第一题

法1

$$
\psi(z) = \frac{1-2bz}{1-bz} = 2-\frac{1}{1-bz} = 1-bz-b^2z^2\cdots\\
\Rightarrow \psi_0 = 1, \psi_j = -b^j,j\geq 1\\
\Rightarrow \gamma_j
$$

法2

两边同乘 $X_t,X_{t-1}$

$$
\begin{cases}
\gamma_0 - b\gamma_1 = 1 + 2b^2\\
\gamma_1 - b\gamma_0 = -2b\\
\gamma_k - b\gamma_{k-1} = 0, k\geq 2
\end{cases}
$$

用YW方程

法3

直接算 $\gamma_0 = EX_t^2,\gamma_1 = EX_tX_{t-1}$。

## Ch 4

### 均值的估计

要会证明是相合估计，$\bar{X}$ 的方差

小测第二题

$$
(1-a)\sum_{t=1}^N X_t-aX_0 + aX_N = \sum_{t=1}^N \varepsilon_t
$$

再同除 $\sqrt{N}$。

如果右边再变成 $\sum_{t=1}^N (\varepsilon_t + b\varepsilon_t)$，就把右边也像左边一样拆拆

定理 CLT 不用知道的

### 自协方差函数

自协方差函数的估计公式要会写

样本自协方差矩阵是正定的

要会证相合性，即渐进无偏性

要知道MA(1)和白噪声的两个情况 ，定理不用看；主要弄在卡方0的检验?

### 白噪声的 $\chi^2$ 检验

### 样本自相关置信区间检验法



## Ch 5

### 预测的概念和性质

叙述概念

要记住 $\boldsymbol{X} = (X_1, \cdots, X_n)'$ 还是反着的，这会导致 $a'X$ 有区别。

最佳线性预测的许多性质

Hilbert空间不提，不用管投影算子

最佳预测

### 决定性平稳序列、非决定性、纯非决定性

$$
\begin{aligned}
\sigma_1^2 &= \lim_{m\to \infty} \mathrm{E}(X_{n+k}-L(X_{n+k}|\boldsymbol{X}_{n,m}))\\
&= \mathrm{E}(X_{n+k}-L(X_{n+k}|H_n))\\
&= \mathrm{E}(X_{n+k}-L(X_{n+k}|X_k,k\leq n))\\
\end{aligned}
$$

单调递增

### AR MA ARMA

时间序列的递推预报公式：证明的思路要弄清楚：定理3.1

构造新息序列，样本新息序列是正交的，等价于用它来预测

要会证新息预测、递推公式

比较简单的系数 $\theta_{11}$ 等，要会推导



例子：习题4 P183 4.4

![image-20211229115400745](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2021/12/202112291154813.png)

$$
\begin{aligned}
\lim_{k\to \infty}\lim_{n\to \infty} E(L(X_{n+k}|X_{n}, X_{n-1}, \cdots, X_1)) 
&= \lim_{k\to \infty}\lim_{n\to \infty} E(L(X_{k}|X_{0}, X_{-1}, \cdots, X_{1-n}))\\
&= \lim_{k\to \infty} E(L(X_{k}|H_0))\\
&= \lim_{k\to \infty} E(L(X_{k}|\epsilon_0, \cdots))\\
&= \lim_{k\to \infty} \sum_{j=k}^\infty \psi_jE\epsilon_{k-j}^2\\
&= \lim_{k\to \infty} \sum_{j=k}^\infty \psi_j\sigma^2\\
&= 0
\end{aligned}
$$

第一步，虽然线性预测是不一样的，但是数学期望的平方是一样的:如果仔细说明就按照定理2.1的写法插入一些

(2) 同理 $\psi_0E\epsilon_1^2 = \sigma^2$ 

## Ch 6

叙述 AR 的YW估计、最小二乘估计、MLE

用后面MA(q)那里的形式叙述定阶 主要是AIC BIC定阶

要知道逆相关函数怎么定义，简单叙述ARp 谱密度函数直接的关系，逆相关函数和自相关函数的关系

逆相关函数的叙述过程看的懂就好



考的比较多的：MA新息估计

AIC BIC 定阶



ARMA

矩法估计：延拓YW方程、MA(q)需要用到自协方差估计？

自回归逼近法

MLE

递推公式不用背

ARMA就算了

