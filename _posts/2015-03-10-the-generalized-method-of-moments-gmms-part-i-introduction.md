---
layout: post
title: 'The generalized method of moments (GMMs), Part I: Introduction'
---

Let $$Y_1,\ldots,Y_N$$ be $$(d+1)$$-dimensional observations (collecting the $$X_n\in\mathbb{R}^d$$ covariate within each $$Y_n\in\mathbb{R}$$ response for shorthand) generated from some model with unknown parameters $$\theta\in\Theta$$.

__Goal__: Find the "true" parameters $$\theta^*\in\Theta$$.

__Intuition__: The idea is to find a set of $$k$$ constraints, or "moments", involving the parameters $$\theta$$. What makes GMMs nice is that you need no information per say about how the model depends on $$\theta$$. Certainly they can be used to construct moments (special case: maximum likelihood estimation (MLE)), but one can use, for example, statistical moments (special case: method of moments (MoM)) as the constraints. Analogously, tensor decompositions are used in the case of spectral methods.

More formally, the $$k$$ __moment conditions__ for a vector-valued function $$g(Y,\cdot):\Theta\to\mathbb{R}^k$$ is

$$
m(\theta^*) \equiv \mathbb{E}[g(Y,\theta^*)] = 0_{k\times 1},
$$

where $$0_{k\times 1}$$ is the $$k\times 1$$ zero vector.

As we cannot analytically derive the expectation for arbitrary $$g$$, we use the sample moments instead:

$$
\hat m(\theta) \equiv \frac{1}{N}\sum_{n=1}^N g(Y_n,\theta)
$$

By the Law of Large Numbers, $$\hat{m}(\theta)\to m(\theta)$$, so the problem is thus to find the $$\theta$$ which sets $$\hat m(\theta)$$ to zero.

Cases:

* $$\Theta\supset\mathbb{R}^k$$, i.e., there are more parameters than moment
conditions: The model is not [identifiable](http://en.wikipedia.org/wiki/Identifiability). This is the standard scenario in ordinary least squares (OLS) when there are more covariates than observations and so no unique set of parameters $$\theta$$ exist. Solve this by simply constructing more moments!
* $$\Theta=\mathbb{R}^k$$: There exists a unique solution.
* $$\Theta\subset\mathbb{R}^k$$,
i.e., there are fewer parameters than moment conditions: The parameters are overspecified and the best we can do is to minimize $$m(\theta)$$ instead of solve $$m(\theta)=0$$.

Consider the last scenario: we aim to minimize $$\hat m(\theta)$$ in some way, say $$\|\hat m(\theta)\|$$ for some choice of $$\|\cdot\|$$. We define the __weighted norm__ as

$$
\|\hat m(\theta)\|_W^2 \equiv \hat m(\theta)^T W \hat m(\theta),
$$

where $$W$$ is a positive definite matrix.

The __generalized method of moments__ (GMMs) procedure is to find

$$
\hat\theta = {arg\ min}_{\theta\in\Theta}
\left(\frac{1}{N}\sum_{n=1}^N g(Y_n,\theta)\right)^T W
\left(\frac{1}{N}\sum_{n=1}^N g(Y_n,\theta)\right)
$$

Note that while the motivation is for $$\theta\supset\mathbb{R}^k$$, by the unique solution, this is guaranteed to work for $$\Theta=\mathbb{R}^k$$ too. Hence it is a _generalized_ method of moments.

__Theorem__. Under standard assumptions¹, the estimator $$\hat\theta$$ is [consistent](http://en.wikipedia.org/wiki/Consistent_estimator#Bias_versus_consistency) and [asymptotically normal](http://en.wikipedia.org/wiki/Asymptotic_distribution). Furthermore, if

$$
W \propto
\Omega^{-1}\equiv\mathbb{E}[g(Y_n,\theta^*)g(Y_n,\theta^*)^T]^{-1}
$$

then $$\hat \theta$$ is [asymptotically optimal](http://en.wikipedia.org/wiki/Efficiency_(statistics)), i.e., achieves the Cramér-Rao lower bound.

Note that $$\Omega$$ is the covariance matrix of $$g(Y_n,\theta^*)$$ and $$\Omega^{-1}$$ the precision. Thus the GMM weights the parameters of the estimator $$\hat\theta$$ depending on how much "error" remains in $$g(Y,\cdot)$$ per parameter of $$\theta^*$$ (that is, how far away $$g(Y,\cdot)$$ is from 0).

I haven't seen anyone make this remark before, but the GMM estimator can also be viewed as minimizing a log-normal quantity. Recall that the multivariate normal distribution is proportional to

$$
\exp\Big((Y_n-\mu)^T\Sigma^{-1}(Y_n-\mu)\Big)
$$

Setting $$g(Y_n,\theta)\equiv Y_n-\mu$$, $$W\equiv\Sigma$$, and taking the log, this is exactly the expression for the GMM! By the asymptotic normality, this explains why would want to set $$W\equiv\Sigma$$ in order to achieve statistical efficiency.

¹ The standard assumptions can be found in [1]. In practice they will almost always be satisfied, e.g., compact parameter space, $$g$$ is continuously differentiable in a neighborhood of $$\theta^*$$, output of $$g$$ is never infinite, etc.

## References
[1] Alastair Hall. _Generalized Method of Moments (Advanced Texts in Econometrics)_. Oxford University Press, 2005.
