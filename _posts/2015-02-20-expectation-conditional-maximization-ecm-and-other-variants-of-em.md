---
layout: post
title: Expectation Conditional Maximization (ECM) and other variants of EM
---

Here we shall introduce the Expectation Conditional Maximization algorithm (ECM) by Meng and Rubin (1993) by motivating it from a typical example. I'll also add some thoughts about other natural considerations at the end.

Let $$n$$ observations $$y_1,\ldots,y_n\sim\mathcal{N}(X_i\beta, \Sigma)$$ be sampled i.i.d. where $$X_i$$'s are given features for each $$i$$. Let both the linear coefficients $$\beta$$ and the covariance $$\Sigma$$ be unknown, and say for each $$y_i$$ only some values are observed, i.e., there exists missing data. Partition the observations $$Y=(Y_{obs},Y_{mis})$$, where $$Y_{obs}$$ represents all those observed and $$Y_{mis}$$ the missing data.

Now consider the problem of calculating the maximum likelihood estimate (MLE) of $$\theta=(\beta,\Sigma)$$. This has no closed form expression, so let's consider the EM algorithm (Dempster et al., 1977) instead:

1. Initialize $$\theta^{(0)}=(\beta^{(0)},\Sigma^{(0)})$$.
2. While $$\theta$$ has not converged, i.e., $$\|\theta^{(t+1)}-\theta^{(t)}\|/\|\theta^{(t)}\|\le\epsilon$$:
    1. E-step: Calculate expectation of the sufficient statistics, conditional on observed data and current parameter values:

    <!--Q(\theta\mid\theta^{(t)}) &= \mathbb{E}_{Y_{mis}}[\ell(\theta; Y)\mid Y_{obs}, \theta=\theta^{(t)}]\\-->
    $$
    \mathbb{E}[Y_i\mid Y_{obs}, \beta^{(t)}, \Sigma^{(t)}],\qquad
    \mathbb{E}[Y_iY_i^T\mid Y_{obs}, \beta^{(t)}, \Sigma^{(t)}]
    $$

    &#x200B;
    2. M-step: Substitute the above into expressions for the sufficient statistics

    $$
    \theta^{(t+1)} = \mathrm{arg\ max}_\theta Q(\theta\mid\theta^{(t)})
    $$

ECM is a natural consideration for EM, which replaces the maximization step over one's parameters of interest by conditioning on a subset of these parameters

PxEM is a way of incorporating additional information into one's estimate by modifying the current parameters based on some set of constraints you know must be true.

Intuitively, I see the advantage of this variant in the same way the [Stein's estimator]() dominates the MLE for the trivial example of three observations sampled i.i.d. from a normal $$y_i\sim \mathcal{N}(\mu_i, 1)$$, where $$\mu_i$$'s are unknown for $$i=1,2,3$$. It is easy to see that the MLE is $$\mu_i^{MLE} = y_i$$. However, this estimator is inadmissable; it is dominated by the estimate ... While this seems paradoxical, what is really happening is the underlying truth that all observations are sampled from a normal distribution with the same variance. By taking advantage of the fact that all observations share this attribute, you can obtain a better estimate by introducing dependencies.

## References
* Arthur Dempster, Nan Laird, and Donald Rubin. Maximum likelihood from incomplete data via the EM algorithm. *Journal of the Royal Statistical Society*, Series B 39 (1): 1–38, 1977.
* Xiao-Li Meng and Donald Rubin. Maximum likelihood estimation via the ECM algorithm: a general framework. *Biometrika*, 80 (2): 267–278, 1993.
