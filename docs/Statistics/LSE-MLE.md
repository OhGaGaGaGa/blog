## 对于普通线性模型，LSE 和 MLE 是一致的

$$
\boldsymbol{Y} = \boldsymbol{X}\boldsymbol{\beta}+\boldsymbol{e}, \boldsymbol{e}\sim N (\boldsymbol{0}, \sigma^2\boldsymbol{I}_n)
$$

最小二乘估计（LSE）中，要求
$$
\min_\boldsymbol{\beta} Q(\boldsymbol{\beta}) = \min_\boldsymbol{\beta}(\boldsymbol{Y} - \boldsymbol{X}\boldsymbol{\beta})'(\boldsymbol{Y} - \boldsymbol{X}\boldsymbol{\beta})
$$
而若使用 MLE，如下。

回顾 MLE 定义

> **定义** 设总体分布族为  $\mathcal{F}=\left\{p(x ; \theta) ; \theta=\left(\theta_{1}, \cdots, \theta_{k}\right) \in \Theta\right\}$  是一个参数分布族,  $p(x ; \theta)$  为密度函数  (probability density function)  或分布列  (probability mass function) . $\widetilde{x}=\left(x_{1}, \cdots, x_{n}\right)$  是由  $\mathcal{F}$  中的一个总体  $p(x ; \theta)$  产生的简单随机样本  $\tilde{X}=\left(X_{1}, \cdots, X_{n}\right)$  的观测值. 这时似然函数为
> $$
> L(\theta ; \widetilde{x})=\prod_{i=1}^{n} p\left(x_{i} ; \theta\right), \quad \theta \in \Theta .
> $$
> 假如存在统计量  $\widehat{\theta}(\tilde{X})=\widehat{\theta}\left(X_{1}, \cdots, X_{n}\right)$ , 使得
> $$
> L(\widehat{\theta}(\widetilde{x}) ; \widetilde{x})=\sup _{\theta \in \Theta} L(\theta ; \widetilde{x}) .
> $$
> 或等价使得
> $$
> l(\widehat{\theta}(\widetilde{x}) ;(\widetilde{x}))=\sup _{\theta \in \Theta} l(\theta ; \widetilde{x}) .
> $$
> 则称  $\widehat{\theta}(\widetilde{X})$  为  $\theta$  的极大似然估计量(Maximum likelihood estimator). 简记为 MLE.

我们可以将线性模型训练集中解释变量视为已知，把效应变量视为 $Y\sim N(\boldsymbol{X}\boldsymbol{\beta}, \sigma^2\boldsymbol{I}_n)$ 总体中的抽样，则可以有密度函数（似然函数）：
$$
\begin{aligned}
L(\boldsymbol{\beta}; \tilde{\boldsymbol{y}}) &= \frac{1}{(\sqrt{2\pi}\sigma)^n}\exp\left(-\frac{1}{2\sigma^2}(\boldsymbol{Y} - \boldsymbol{X}\boldsymbol{\beta})'(\boldsymbol{Y} - \boldsymbol{X}\boldsymbol{\beta})\right)
\end{aligned}
$$
对数似然函数：
$$
\begin{aligned}
l(\boldsymbol{\beta}; \tilde{\boldsymbol{y}}) &= -n\ln{(\sqrt{2\pi}\sigma)}-\frac{1}{2\sigma^2}(\boldsymbol{Y} - \boldsymbol{X}\boldsymbol{\beta})'(\boldsymbol{Y} - \boldsymbol{X}\boldsymbol{\beta})
\end{aligned}
$$
要想使得 $\sup_{\boldsymbol{\beta}}l(\boldsymbol{\beta}; \tilde{\boldsymbol{y}})$，即 $\inf_\boldsymbol{\beta}(\boldsymbol{Y} - \boldsymbol{X}\boldsymbol{\beta})'(\boldsymbol{Y} - \boldsymbol{X}\boldsymbol{\beta})$，与 LSE 一致。
