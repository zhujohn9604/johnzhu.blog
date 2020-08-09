---
title: "Antisymmetic component disapears in the quadratic form"
author: ZHU Chen
date: 2020-07-12 11:33:00 +0800
categories: [PRML]
tags: [PRML]
math: true
---

In the multivariate Gaussian distribution, the term in the exponent has the form
\begin{align}
(\mathbf{x} - \mathbf{\mu})^{T}\Sigma(\mathbf{x} - \mathbf{\mu})
\end{align}
Note that the precision matrix \\(\Sigma\\) typically is a symmetric matrix. More generally, any antisymmetric component would disappear when taking the quadratic form. To prove this, let's first assume that \\(\Sigma\\) is an unsymmetric matrix, which can therefore decompose as the sum of a symmetric matrix and an antisymmetric matrix, namely, \\(\Sigma_{ij} = \Sigma_{ij}^{S} + \Sigma_{ij}^{A}\\), where
\begin{align}
\Sigma_{ij}^{S} = \frac{ \Sigma_{ij} + \Sigma_{ji} }{2}
\end{align}
\begin{align}
\Sigma_{ij}^{A} = \frac{ \Sigma_{ij} - \Sigma_{ji} }{2}
\end{align}
Then, we notice the quadratic form can be written as
\begin{align}
(\mathbf{x} - \mathbf{\mu})^{T}\Sigma(\mathbf{x} - \mathbf{\mu}) = \sum_{i}\sum_{j}(x_i - \mu_i)\Sigma_{ij}(x_j - \mu_j)
\end{align}
When we substitute \\(\Sigma_{ij} = \Sigma_{ij}^{S} + \Sigma_{ij}^{A}\\) into above equation, all terms involving \\(\Sigma_{ij}^{A}\\) would disapear since \\(\Sigma_{ij}^{A} = -\Sigma_{ji}^{A}\\).