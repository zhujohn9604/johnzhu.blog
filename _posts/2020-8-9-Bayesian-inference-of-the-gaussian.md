---
title: "Bayesian inference for the Gaussian"
author: ZHU Chen
date: 2020-08-07 11:33:00 +0800
categories: [PRML]
tags: [PRML]
math: true
---
## Known variance \\( \sigma^2 \\) and unknown mean \\( \mu \\)[^footnote]
Consider a set of \\( N \\) observations \\( \mathbf{X} = \{x_1, \cdots, x_n\} \\) from a Gaussian, where the variance \\(\sigma^2 \\) is known, and we want to infer the mean \\(\mu \\). The likelihood function is therefore can be written as
\begin{align}
p(\mathbf{X}| \mu) = \prod_{n=1}^{N} p(x_n|\mu) = \frac{1}{(2\pi\sigma^2)^{N/2}}\exp{\left(-\frac{1}{2\sigma^2}\sum_{n=1}^{N}(x_n-\mu)^2\right)}
\end{align}
Note the likelihood function is not a probability distribution function over \\(\mu \\), because it is not normalized. We choose the prior \\(p(\mu)\\) given by a Gaussian as well so that it can be a conjugate distribution for the likelihood function. In this case, the posterior will also be a Gaussian. The prior distribution is chosen as

\begin{align}
p(\mu) = \mathcal{N}(\mu|\mu_0, \sigma^2_0)
\end{align}

and the posterior distribution is given by
\begin{align}
p(\mu|\mathbf{X}) \propto p(\mathbf{X}| \mu)p(\mu)
\end{align}
\begin{align}
p(\mu|\mathbf{X}) = \mathcal{N}(\mu|\mu_N, \sigma^2_N)
\end{align}
We can calculate the parameters of the posterior distribution using the technique called `completing the square`. Let's focus on the exponetial terms in the posterior distribution \\(p(\mu|\mathbf{X}) \\), we have

\begin{align}
-\frac{1}{2\sigma^2}\sum_{n=1}^{N}(x_n-\mu)^2 - \frac{1}{2\sigma^2_0}(\mu-\mu_0)^2 = - \frac{1}{2\sigma^2_N}(\mu-\mu_N)^2
\end{align}

The coefficients of \\(\mu^2 \\) and \\(\mu \\) on the `left-hand side` are given as 
\begin{align}
-(\frac{N}{2\sigma^2} + \frac{1}{2\sigma_0^2})\mu^2
\end{align}
and
\begin{align}
-(\frac{N\mu_{ML}}{\sigma^2} + \frac{\mu_0}{\sigma_0^2})\mu
\end{align}
respectively, where
\begin{align}
\mu_{ML} = \frac{1}{N}\sum_{n=1}^{N}x_n
\end{align}
Similarly, the coefficients of \\(\mu^2 \\) and \\(\mu \\) on the `right-hand side` are given as \\( -\frac{1}{2\sigma_{N}^2}\mu^2 \\) and \\(\frac{\mu_N}{\sigma_{N}^2}\mu \\), respectively.
From above, we can be easily get \\(\mu_N\\) and \\(\sigma^2\\) by comparision, and we have
\begin{align}
\mu_N = \frac{N\sigma_0^2}{N\sigma_0^2 + \sigma^2}\mu_{ML} + \frac{\sigma^2}{N\sigma_0^2 + \sigma^2}\mu_0
\end{align}
\begin{align}
\frac{1}{\sigma^2_{N}} = \frac{N}{\sigma^2} + \frac{1}{\sigma^2_0}
\end{align}

### Discussion
One may notice that when the number of observed data points \\(N=0 \\), the mean \\(\mu_N \\) of the posterior distribution will reduce to the prior mean \\(\mu\\). If we have infinite observations (\\(N \rightarrow \infty \\)), then the posterior mean will approach to the maximum likelihood estimator. For the posterior variance \\(\sigma^2_N\\), as we have infinite observations, the precision (inverse of the variance) will go to infinity, and the posterior distribution, therefore, becomes peaked around the maximum likelihood estimator.

## Unknown variance \\( \sigma^2 \\) and known mean \\( \mu \\)
Let's then suppose the mean \\( \mu \\) is known and try to infer variance \\( \sigma^2 \\). Likewise, we first write down the likelihood function in terms of precision \\(\lambda = 1/\sigma^2 \\) as
\begin{align}
p(\mathbf{X}|\lambda) = \prod_{n=1}^{N} N(x_n|\mu, \lambda^{-1}) = \lambda^{N/2}\exp{\left( -\frac{\lambda}{2}\sum_{n=1}^{N}(x_n - \mu)^2 \right)}
\end{align}
In order to select a conjugate prior for the likelihood function, we first observe the components in it. The likelihood function consists of a power of \\(\lambda \\) and the exponential of linear function of \\(\lambda \\), so the gamma distribution might be a proper choice. The prior distribution is therefore given as
\begin{align}
p(\lambda|a_0, b_0) \propto \lambda^{a_{0}-1}\exp{(-b_0\lambda)}
\end{align}
and the posterior distribution is given by
\begin{align}
p(\lambda|\mathbf{X}) = {\rm Gam}(\lambda|a_N, b_N) \propto \lambda^{a_{0} + \frac{N}{2} -1}\exp{\left[ -\left(\frac{1}{2}\sum_{n=1}^{N}(x_n-\mu)^2 + b_0\right) \lambda \right]}
\end{align}
Unlike the Gaussian distribution, we can easily get the parameters of the gamma by observing its function form, which
\begin{align}
a_N = a_{0} + \frac{N}{2}
\end{align}

\begin{align}
b_N = \frac{1}{2}\sum_{n=1}^{N}(x_n-\mu)^2 + b_0 = \frac{N}{2}\sigma^2_{ML} + b_0
\end{align}

where $$\sigma^2_{ML} = \frac{1}{N} \sum_{n=1}^{N} (x_n-\mu)^2$$.

### Discussion
From the expression of \\(a_N\\), we notice that \\(N \\) subsequent observations contribute \\(N/2 \\) to the coefficient \\(a \\). Thus, one can interpret the $$a_0$$ in the prior as $$2a_0$$ `effective prior observations`. Similarly, from the expression of $$b_N$$ we can see that \\(N \\) subsequent observations increase the coefficient \\(b \\) by $$\frac{N}{2}\sigma^2_{ML}$$. Thus, the variance of the `effective prior observation` can be calculated by the total variance \\(2b_0 \\) divided by the number of effective prior observations \\(2a_0\\), which is \\(2b_0 / 2a_0 = b_0/a_0\\).

## Unknown variance \\( \sigma^2 \\) and unknown mean \\( \mu \\)
Finally, let's consider both the mean and the variance are unknown. Similarly, we first write down the likelihood function in terms of mean $$\mu$$ and precision $$\lambda$$ as
\begin{align}
p(\mathbf{X}|\mu, \lambda) = \left(\frac{\lambda}{2\pi}\right)^{N/2}\exp{\left(-\frac{\lambda}{2}\sum_{n=1}^{N}(x_n - \mu)^2\right)}
\end{align}
\begin{align}
\propto \left[\lambda^{1/2}\exp{\left(-\frac{\lambda \mu^2}{2} \right)} \right]^{N}\exp{\left(-\frac{\lambda}{2}\sum_{n=1}^{N}x_n^2 + \lambda\mu\sum_{n=1}^{N}x_n  \right)}
\end{align}
By observing the form of By observing the form of likelihood function, the conjugate prior should take the form as
\begin{align}
p(\mu, \lambda) \propto \left[\lambda^{1/2}\exp{\left(-\frac{\lambda \mu^2}{2} \right)} \right]^{\beta}\exp{\left(-d \lambda + c\lambda\mu  \right)}
\end{align}
\begin{align}
=\exp{\left[-\frac{\beta \lambda}{2}(\mu-c/\beta)^2 \right]}\lambda^{\beta/2}\exp{\left[-\left(d-\frac{c^2}{2\beta} \right)\lambda \right]}
\end{align}
By inspecting into the form of the $$p(\mu, \lambda)$$, we notice that it may consist of two components; one from a Gaussian and the other from a Gamma distribution. Formally, the prior can take the form as
\begin{align}
p(\mu, \lambda) = \mathcal{N}(\mu| c/\beta, (\beta\lambda)^{-1}){\rm Gam}(\lambda| \beta/2+1, d-c^2/(2\beta))
\end{align}
which is called as a `Gaussian-gamma distribution`. One need to notice that these two are not independent to each other, becauase the precision of \\(\mu \\) is a linear function of \\(\lambda \\).

## Extension to the multivariate case
In the case of multivariate Gaussian distribution \\(\mathcal{N}(\mathbf{x}|\mathbf{\mu}, \mathbf{\Lambda^{-1}})\\) for a $$D$$-dimensional $$\mathbf{x}$$. When the mean $$\mathbf{\mu}$$ is known and we want to infer the precision matrix $$\mathbf{\Lambda}$$, the likelihood function is given as
\begin{align}
p(\mathbf{X}|\mathbf{\Lambda}) = \prod_{n=1}^{N} \mathcal{N}(\mathbf{x}|\mathbf{\mu}, \mathbf{\Lambda^{-1}})
\end{align}
\begin{align}
\propto |\Lambda|^{N/2}\exp{\left[-\frac{1}{2}\sum_{n=1}^{N}(\mathbf{x}_n-\mathbf{\mu})^{T}\mathbf{\Lambda}(\mathbf{x}_n-\mathbf{\mu}) \right]}
\end{align}

\begin{align}
= |\Lambda|^{N/2}\exp{\left[-\frac{1}{2}\sum_{n=1}^{N} {\rm Tr} ((\mathbf{x}_n-\mathbf{\mu})^{T}\mathbf{\Lambda}(\mathbf{x}_n-\mathbf{\mu})) \right]}
\end{align}

\begin{align}
= |\Lambda|^{N/2}\exp{\left[-\frac{1}{2}\sum_{n=1}^{N} {\rm Tr} ((\mathbf{x}_n-\mathbf{\mu})(\mathbf{x}_n-\mathbf{\mu})^{T}\mathbf{\Lambda}) \right]}
\end{align}

\begin{align}
= |\Lambda|^{N/2}\exp{\left[-\frac{1}{2} {\rm Tr} (\sum_{n=1}^{N}(\mathbf{x}_n-\mathbf{\mu})(\mathbf{x}_n-\mathbf{\mu})^{T}\mathbf{\Lambda}) \right]}
\end{align}

\begin{align}
= |\Lambda|^{N/2}\exp{\left[-\frac{1}{2} {\rm Tr} (\mathbf{S} \mathbf{\Lambda}) \right]}
\end{align}
where $$\mathbf{S}= \sum_{n=1}^{N}(\mathbf{x}_n-\mathbf{\mu})(\mathbf{x}_n-\mathbf{\mu})^{T}$$.

A conjugate prior therefore comes from the `Wishart` distribution, which is given as
\begin{align}
\mathcal{W}(\mathbf{\Lambda}|\mathbf{W}, \mathcal{v}) \propto |\Lambda|^{(\mathcal{v}-D-1)/2}\exp{\left[-\frac{1}{2} {\rm Tr} (\mathbf{W}^{-1} \mathbf{\Lambda}) \right]}
\end{align}

The posterior distribution is then given as
\begin{align}
p(\mathbf{\Lambda}|\mathbf{X}, \mathbf{W}, \mathcal{v}) \propto p(\mathbf{X}|\mathbf{\Lambda}) \mathcal{W}(\mathbf{\Lambda}|\mathbf{W}, \mathcal{v})
\end{align}

\begin{align}
\propto |\Lambda|^{(\mathcal{v} + N -D-1)/2}\exp{\left[-\frac{1}{2} {\rm Tr} ((\mathbf{S}+\mathbf{W}^{-1}) \mathbf{\Lambda}) \right]}
\end{align}
where $$\mathcal{v}_N = \mathcal{v} + N$$ and $$\mathbf{W}_N = (\mathbf{S}+\mathbf{W}^{-1})^{-1}$$.

#### Reference chapter
[^footnote]: PRML 2.3.6