---
title: "Bayesian inference for the Gaussian"
author: ZHU Chen
date: 2020-07-12 11:33:00 +0800
categories: [PRML]
tags: [PRML]
math: true
---
## Known variance \\( \sigma^2 \\) and unknown mean \\( \mu \\)
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