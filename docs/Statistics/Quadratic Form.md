##  二次型

假设 $ \boldsymbol{X}=\left(X_{1}, \cdots, X_{n}\right)^{\prime} $ 为$  n \times 1 $ 随机向量, $ \boldsymbol{A}=\left(a_{i j}\right)$  为 $ n \times n $ 对称矩阵, 则称随机变量
$$
\boldsymbol{X}^{\prime} \boldsymbol{A} \boldsymbol{X}=\sum_{i=1}^{n} \sum_{j=1}^{n} a_{i j} X_{i} X_{j}
$$
为 $ \boldsymbol{X}  $ 的二次型, 称 $ \boldsymbol{A} $ 为此二次型的二次型矩阵.

## 期望

**定理1**：设 $\mathrm{E}(\boldsymbol{X})=\boldsymbol{\mu}, \operatorname{Cov}(\boldsymbol{X})=\boldsymbol{\Sigma}$，则
$$
\mathrm{E}\left(\boldsymbol{X}^{\prime} \boldsymbol{A} \boldsymbol{X}\right)=\boldsymbol{\mu}^{\prime} \boldsymbol{A} \boldsymbol{\mu}+\operatorname{tr}(\boldsymbol{A} \boldsymbol{\Sigma})
$$

**证明**：因为
$$
\begin{aligned}
\boldsymbol{X}^{\prime} \boldsymbol{A} \boldsymbol{X}=&(\boldsymbol{X}-\boldsymbol{\mu}+\boldsymbol{\mu})^{\prime} \boldsymbol{A}(\boldsymbol{X}-\boldsymbol{\mu}+\boldsymbol{\mu}) \\
=&(\boldsymbol{X}-\boldsymbol{\mu})^{\prime} \boldsymbol{A}(\boldsymbol{X}-\boldsymbol{\mu})+\boldsymbol{\mu}^{\prime} \boldsymbol{A}(\boldsymbol{X}-\boldsymbol{\mu}) \\
&+(\boldsymbol{X}-\boldsymbol{\mu})^{\prime} \boldsymbol{A} \boldsymbol{\mu}+\boldsymbol{\mu}^{\prime} \boldsymbol{A} \boldsymbol{\mu} .
\end{aligned}
$$
上式第二项与第三项的数学期望为零. 因此，只需证明
$$
\mathrm{E}\left[(\boldsymbol{X}-\boldsymbol{\mu})^{\prime} \boldsymbol{A}(\boldsymbol{X}-\boldsymbol{\mu})\right]=\operatorname{tr}(\boldsymbol{A} \boldsymbol{\Sigma})
$$
利用迹的性质 $\operatorname{tr}(\boldsymbol{A B})=\operatorname{tr}(\boldsymbol{B} \boldsymbol{A})$  以及求迹和求期望可交换次序 , 可知
$$
\begin{aligned}
\mathrm{E}\left[(\boldsymbol{X}-\boldsymbol{\mu})^{\prime} \boldsymbol{A}(\boldsymbol{X}-\boldsymbol{\mu})\right] &=\mathrm{E}\left[\operatorname{tr}\left((\boldsymbol{X}-\boldsymbol{\mu})^{\prime} \boldsymbol{A}(\boldsymbol{X}-\boldsymbol{\mu})\right)\right] \\
&=\mathrm{E}\left\{\operatorname{tr}\left[\boldsymbol{A}(\boldsymbol{X}-\boldsymbol{\mu})(\boldsymbol{X}-\boldsymbol{\mu})^{\prime}\right]\right\} \\
&=\operatorname{tr}\left\{\boldsymbol{A E}\left[(\boldsymbol{X}-\boldsymbol{\mu})(\boldsymbol{X}-\boldsymbol{\mu})^{\prime}\right]\right\} \\
&=\operatorname{tr}(\boldsymbol{A} \boldsymbol{\Sigma})
\end{aligned}
$$
## 方差

**定理2**：设随机变量  $X_{i}, i=1, \cdots, n$  相互独立, 且  $\mathrm{E}\left(X_{i}\right)=\mu_{i}$ . 假设  $X_{i}-\mu_{i}, i=1, \cdots, n$  是同分布的且
$$
\operatorname{Var}\left(X_{i}\right)=\sigma^{2}, m_{r}=E\left(X_{i}-\mu_{i}\right)^{r}, r=3,4
$$
$ \boldsymbol{A}=\left(a_{i j}\right)$  为  $n \times n$  对称矩阵. 记
$$
\boldsymbol{X}=\left(X_{1}, \cdots, X_{n}\right)^{\prime}, \quad \boldsymbol{\mu}=\left(\mu_{1}, \cdots, \mu_{n}\right)^{\prime}
$$

则
$$
\operatorname{Var}\left(\boldsymbol{X}^{\prime} \boldsymbol{A} \boldsymbol{X}\right)=\left(m_{4}-3 \sigma^{4}\right) \boldsymbol{a}^{\prime} \boldsymbol{a}+2 \sigma^{4} \operatorname{tr}\left(\boldsymbol{A}^{2}\right)+4 \sigma^{2} \boldsymbol{\mu}^{\prime} \boldsymbol{A}^{2} \boldsymbol{\mu}+4 m_{3} \boldsymbol{\mu}^{\prime} \boldsymbol{A} \boldsymbol{a} 
$$

其中  $\boldsymbol{a}=\left(a_{11}, \cdots, a_{n n}\right)^{\prime}$  ，即  $\boldsymbol{A}$  的对角元组成的列向量。

**证明**：首先注意到
$$
\operatorname{Var}\left(\boldsymbol{X}^{\prime} \boldsymbol{A} \boldsymbol{X}\right)=\mathrm{E}\left(\boldsymbol{X}^{\prime} \boldsymbol{A} \boldsymbol{X}\right)^{2}-\left[\mathrm{E}\left(\boldsymbol{X}^{\prime} \boldsymbol{A} \boldsymbol{X}\right)\right]^{2},
$$

由定理1以及 $\mathrm{E}(\boldsymbol{X})=\boldsymbol{\mu}$ 和 $\operatorname{Cov}(\boldsymbol{X})=\sigma^{2} \boldsymbol{I}_{n}$  可推得

$$
\mathrm{E}\left(\boldsymbol{X}^{\prime} \boldsymbol{A} \boldsymbol{X}\right)=\boldsymbol{\mu}^{\prime} \boldsymbol{A} \boldsymbol{\mu}+\sigma^{2} \operatorname{tr}(\boldsymbol{A})
$$

所以我们的问题主要是计算第一项（平方的期望）. 将  $\boldsymbol{X}^{\prime} \boldsymbol{A} \boldsymbol{X}$  改写为

$$
\boldsymbol{X}^{\prime} \boldsymbol{A} \boldsymbol{X}=(\boldsymbol{X}-\boldsymbol{\mu})^{\prime} \boldsymbol{A}(\boldsymbol{X}-\boldsymbol{\mu})+2 \boldsymbol{\mu}^{\prime} \boldsymbol{A}(\boldsymbol{X}-\boldsymbol{\mu})+\boldsymbol{\mu}^{\prime} \boldsymbol{A} \boldsymbol{\mu}
$$

将其平方得到

$$
\begin{aligned}
\left(\boldsymbol{X}^{\prime} \boldsymbol{A} \boldsymbol{X}\right)^{2}=& {\left[(\boldsymbol{X}-\boldsymbol{\mu})^{\prime} \boldsymbol{A}(\boldsymbol{X}-\boldsymbol{\mu})\right]^{2}+4\left[\boldsymbol{\mu}^{\prime} \boldsymbol{A}(\boldsymbol{X}-\boldsymbol{\mu})\right]^{2}+\left(\boldsymbol{\mu}^{\prime} \boldsymbol{A} \boldsymbol{\mu}\right)^{2} } \\
&+2 \boldsymbol{\mu}^{\prime} \boldsymbol{A} \boldsymbol{\mu}\left[(\boldsymbol{X}-\boldsymbol{\mu})^{\prime} \boldsymbol{A}(\boldsymbol{X}-\boldsymbol{\mu})+2 \boldsymbol{\mu}^{\prime} \boldsymbol{A}(\boldsymbol{X}-\boldsymbol{\mu})\right] \\
&+4 \boldsymbol{\mu}^{\prime} \boldsymbol{A}(\boldsymbol{X}-\boldsymbol{\mu})(\boldsymbol{X}-\boldsymbol{\mu})^{\prime} \boldsymbol{A}(\boldsymbol{X}-\boldsymbol{\mu}) .
\end{aligned}
$$

令 $ \boldsymbol{Z}=\boldsymbol{X}-\boldsymbol{\mu}$ , 则  $\mathrm{E}(\boldsymbol{Z})=\mathbf{0}$ . 再次利用定理1得
$$
\begin{aligned}
\mathrm{E}\left(\boldsymbol{X}^{\prime} \boldsymbol{A} \boldsymbol{X}\right)^{2}=\mathrm{E}\left(\boldsymbol{Z}^{\prime} \boldsymbol{A} \boldsymbol{Z}\right)^{2}+4 \mathrm{E}\left(\boldsymbol{\mu}^{\prime} \boldsymbol{A} \boldsymbol{Z}\right)^{2}+\left(\boldsymbol{\mu}^{\prime} \boldsymbol{A} \boldsymbol{\mu}\right)^{2} \\
+2 \boldsymbol{\mu}^{\prime} \boldsymbol{A} \boldsymbol{\mu}\left(\sigma^{2} \operatorname{tr}(\boldsymbol{A})\right)+4 \mathrm{E}\left(\boldsymbol{\mu}^{\prime} \boldsymbol{A} \boldsymbol{Z} \boldsymbol{Z}^{\prime} \boldsymbol{A} \boldsymbol{Z}\right) 
\end{aligned}
$$
下面逐个计算上式所含的每个均值. 由
$$
\left(\boldsymbol{Z}^{\prime} \boldsymbol{A} \boldsymbol{Z}\right)^{2}=\sum_{i} \sum_{j} \sum_{k} \sum_{l} a_{i j} a_{k l} Z_{i} Z_{j} Z_{k} Z_{l}
$$
及 $ Z_{i} $ 的独立性可得
$$
\mathrm{E}\left(Z_{i} Z_{j} Z_{k} Z_{l}\right)=\left\{\begin{array}{ll}
m_{4}, & \text { 若 } i=j=k=l, \\
\sigma^{4}, & \text { 若 } i=j \neq k=l ; i=k \neq j=l ; i=l \neq j=k, \\
0, & \text { 其它, }
\end{array}\right.
$$
便有下列结果:
$$
\mathrm{E}\left(\boldsymbol{Z}^{\prime} \boldsymbol{A} \boldsymbol{Z}\right)^{2} = m_{4}\left(\sum_{i = 1}^{n} a_{i i}^{2}\right)+\sigma^{4}\left(\sum_{i \neq k} a_{i i} a_{k k}+\sum_{i \neq j} a_{i j}^{2}+\sum_{i \neq j} a_{i j} a_{j i}\right) \\ = m_{4} \boldsymbol{a}^{\prime} \boldsymbol{a}+\sigma^{4}\left\{[\operatorname{tr}(\boldsymbol{A})]^{2}-\boldsymbol{a}^{\prime} \boldsymbol{a}+2\left[\operatorname{tr}\left(\boldsymbol{A}^{2}\right)-\boldsymbol{a}^{\prime} \boldsymbol{a}\right]\right\} \\ = \left(m_{4}-3 \sigma^{4}\right) \boldsymbol{a}^{\prime} \boldsymbol{a}+\sigma^{4}\left\{[\operatorname{tr}(\boldsymbol{A})]^{2}+2 \operatorname{tr}\left(\boldsymbol{A}^{2}\right)\right\}, \quad(2.2 .5) \\
$$
而
$$
\begin{aligned}
\mathrm{E}\left(\boldsymbol{\mu}^{\prime} \boldsymbol{A} \boldsymbol{Z}\right)^{2} = \mathrm{E}\left(\boldsymbol{Z}^{\prime} \boldsymbol{A} \boldsymbol{\mu} \boldsymbol{\mu}^{\prime} \boldsymbol{A} \boldsymbol{Z}\right) \\ = \sigma^{2} \cdot \operatorname{tr}\left(\boldsymbol{A} \boldsymbol{\mu} \boldsymbol{\mu}^{\prime} \boldsymbol{A}\right) \\ = \sigma^{2} \cdot \boldsymbol{\mu}^{\prime} \boldsymbol{A}^{2} \boldsymbol{\mu} .
\end{aligned}
$$
最后, 若记  $b=A \mu$ , 则
$$
\mathrm{E}\left(\boldsymbol{\mu}^{\prime} \boldsymbol{A} \boldsymbol{Z} \boldsymbol{Z}^{\prime} \boldsymbol{A} \boldsymbol{Z}\right)=\sum_{i} \sum_{j} \sum_{k} b_{i} a_{j k} \mathrm{E}\left(Z_{i} Z_{j} Z_{k}\right)
$$
因为
$$
\mathrm{E}\left(Z_{i} Z_{j} Z_{k}\right)=\left\{\begin{array}{ll}
m_{3}, & \text { 若 } i=j=k, \\
0, & \text { 其它, }
\end{array}\right.
$$
所以
$$
\mathrm{E}\left(\boldsymbol{\mu}^{\prime} \boldsymbol{A} \boldsymbol{Z} \boldsymbol{Z}^{\prime} \boldsymbol{A} \boldsymbol{Z}\right)=m_{3} \sum_{i} b_{i} a_{i i}=m_{3} \boldsymbol{b}^{\prime} \boldsymbol{a}=m_{3} \boldsymbol{\mu}^{\prime} \boldsymbol{A} \boldsymbol{a}
$$
代回，便可得到需要证明的结果.



整理自王松桂老师《线性模型引论》和庞天晓老师课件，使用 https://www.latexlive.com/home 进行 Latex OCR。
