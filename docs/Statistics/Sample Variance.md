## 样本方差

假设 $X_1, X_2, \cdots, X_n$ 是 i.i.d. 的。

由无偏性，定义样本方差为
$$
S^2 = \frac{1}{n-1} \sum_{i=1}^n (X_i - \overline{X})^2
$$
**Lemma.** 上述定义与下式等价
$$
S^2 = \frac{1}{n(n-1)}\sum _{i\neq j} \frac{1}{2}(X_i - X_j)^2
$$
**证明**. 
$$
\begin{aligned}
\sum _{i\neq j} \frac{1}{2}(X_i - X_j)^2 &= \sum _{i,j} \frac{1}{2}(X_i - X_j)^2 - \sum _{i=j} \frac{1}{2}(X_i - X_j)^2\\
&= \sum_{i,j}\frac{1}{2}(X_i - X_j)^2\\
&= \frac{n}{2}\sum_{i=1}^nX_i^2 + \frac{n}{2}\sum_{j=1}^nX_j^2 - \sum_{i=1}^n\sum_{j=1}^nX_iX_j\\
&= n\sum_{i=1}^nX_i^2 - (\sum_{i=1}^nX_i)^2
\end{aligned}
$$

## 样本方差的方差

下面讨论样本方差的方差计算。

由于  $\mathbb{E}\left[\dfrac{1}{2}\left(X_{i}-X_{j}\right)^{2}\right]=\mathbb{E}\left[\dfrac{1}{2}\left\{\left(X_{i}-\mu\right)-\left(X_{j}-\mu\right)\right\}^{2}\right]=\sigma^{2}$ , 从而有
$$
\operatorname{Var}\left(S^{2}\right)=\frac{1}{n^{2}(n-1)^{2}} \sum_{i \neq j}\sum_{k \neq l} \mathbb{E}\left[\left[\frac{1}{2}\left(X_{i}-X_{j}\right)^{2}-\sigma^{2}\right]\left[\frac{1}{2}\left(X_{k}-X_{l}\right)^{2}-\sigma^{2}\right]\right]
$$
记  $A_{i j}=\left(X_{i}-X_{j}\right)^{2} / 2-\sigma^{2}, A_{k l}=\left(X_{k}-X_{l}\right)^{2} / 2-\sigma^{2}$  。需考虑 $\{i, j\} $ 和  $\{k, l\} $ 相交的 情况。
- 情形1.  $\{i, j\}$  和  $\{k, l\}$  无交集, 可知此时 $ \mathbb{E}\left(A_{i j} A_{k l}\right)=0  $。
- 情形2.  $|\{i, j\} \cap\{k, l\}|=1$ , 即存在一个元素相同。共计有  $4 n(n-1)(n-2)$  这样 的项。现计算
$$
B\xlongequal{\triangle}\mathbb{E}\left[\left\{\frac{1}{2}\left(X_{1}-X_{2}\right)^{2}-\sigma^{2}\right\}\left\{\frac{1}{2}\left(X_{1}-X_{3}\right)^{2}-\sigma^{2}\right\}\right]
$$
对此可首先计算  $\mathbb{E}\left[\dfrac{1}{2}\left(X_{1}-X_{2}\right)^{2}-\sigma^{2} \mid X_{1}\right]$  。容易得出
$$
\mathbb{E}\left[\frac{1}{2}\left(x-X_{2}\right)^{2}-\sigma^{2}\right]=\frac{1}{2}\left[(x-\mu)^{2}-\sigma^{2}\right]
$$
因而
$$
B=\frac{1}{4} \mathbb{E}\left[\left\{(X-\mu)^{2}-\sigma^{2}\right\}^{2}\right]=\frac{1}{4}\left(\mu_{4}-\sigma^{4}\right) .
$$
这里 $ \mu_{4}=\mathbb{E}(X-\mu)^{4} $.
- 情形3. $ |\{i, j\} \cap\{k, l\}|=2 $, 即存在两个元素相同。共计有  $2 n(n-1) $ 这样的项。 现计算
$$
C\xlongequal{\triangle} \mathbb{E}\left[\left(\frac{1}{2}\left(X_{1}-X_{2}\right)^{2}-\sigma^{2}\right)^{2}\right]
$$
可求得
$$
C=\frac{1}{4} \mathbb{E}\left[\left(X_{1}-\mu\right)-\left(X_{2}-\mu\right)\right]^{4}-\sigma^{4}=\frac{1}{2}\left(\mu_{4}+\sigma^{4}\right) .
$$
将上述结果整理之后可得
$$
\begin{aligned}
\operatorname{Var}\left(S^{2}\right)&=\frac{1}{n^{2}(n-1)^{2}}\left[4 n(n-1)(n-2) \frac{1}{4}\left(\mu_{4}-\sigma^{4}\right)+2 n(n-1) \frac{1}{2}\left(\mu_{4}+\sigma^{4}\right)\right] \\
&=\frac{\mu_{4}}{n}-\frac{n-3}{n(n-1)} \sigma^{4}\\
&=\frac{\mu_{4}-\sigma^{4}}{n}+\frac{2 \sigma^{4}}{n(n-1)} .
\end{aligned}
$$

类似地，可以使用[二次型]("Quadratic Form")的方法。

## 样本均值和样本方差的协方差

即计算 $ \operatorname{Cov}\left(\bar{X}, S^{2}\right) $ 。
$$
\begin{array}{l}
\operatorname{Cov}\left(\bar{X}, S^{2}\right)=\operatorname{Cov}\left(\frac{1}{n} \sum_{i=1}^{n} X_{i}, \frac{1}{2 n(n-1)} \sum_{j \neq l}\left(X_{j}-X_{l}\right)^{2}\right) \\
=\frac{1}{2 n^{2}(n-1)} \sum_{i=1}^{n} \sum_{j \neq l} \operatorname{Cov}\left(X_{i},\left(X_{j}-X_{l}\right)^{2}\right) .
\end{array}
$$
下面对 $ D_{i j l}\xlongequal{\triangle}\operatorname{Cov}\left(X_{i},\left(X_{j}-X_{l}\right)^{2}\right)$  进行讨论。若 $ i \neq j \neq l$ , 则  $D_{i j l}=0 $ 。若 $ i=j \neq l$  或者 $ i=l \neq j$ , 则有
$$
\begin{array}{l}
D_{i j l}=\operatorname{Cov}\left(X_{j},\left(X_{j}-X_{l}\right)^{2}\right) \\
=\mathbb{E}\left[\left(X_{j}-\mu\right)\left\{\left(X_{j}-\mu\right)-\left(X_{l}-\mu\right)\right\}^{2}\right] \\
=\mathbb{E}\left[\left(X_{j}-\mu\right)^{3}\right]
\end{array}
$$
从而可得
$$
\operatorname{Cov}\left(\bar{X}, S^{2}\right)=\frac{1}{2 n^{2}(n-1)} 2 n(n-1) \mathbb{E}\left[\left(X_{j}-\mu\right)^{3}\right]=\frac{\mathbb{E}\left[\left(X_{j}-\mu\right)^{3}\right]}{n} .
$$
对于正态分布而言, 样本均值和样本方差是独立的。而从上述结果可知对于一般的分布只要偏度为 0 , 则样本均值和样本方差就是不相关的。

参考：https://mp.weixin.qq.com/s/PGgsX9Nh8-1ePBfmgO9h2g