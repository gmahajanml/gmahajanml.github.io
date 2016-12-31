---
layout: post
title: Minimum Mean Square Error Estimator
categories: ['Personal']
tags: ['ml', 'regression', 'proofs']
math: true
---

In the last [blog post]({{site.baseurl}}/Least-Squares-and-Nearest-Neighbors/), I mentioned that the the expected (squared) prediction error can be represented as
\begin{align\*} 
EPE(f)& = E[Y-f(X)]^2\\\\  
& = \int[y-f(x)]^2 Pr(dx,dy)
\end{align\*}
By conditioning on \\(X\\), we can write \\(EPE\\) as \\[EPE(f) = E_X E_{Y|X} ([Y-f(X)]^2|X)\\]
and we see that it suffices to minimize \\(EPE\\) pointwise:
\begin{equation}
\label{epe}
f(x)=argmin_c E_{Y|X} ([Y-c]^2|X=x)
\end{equation}

In this post, I will prove how the solution to equation \eqref{epe} above is \\[f(x)=E(Y|X=x)\\]
the conditional expectation, also known as the _regression_ function.

For any guess t by writing \\(\mu = E(Y)\\), we have \\(EPE(f)\\) as
\begin{align\*}
E[Y-t]^2 & = E[Y-\mu + \mu-t]^2\\\\  
& = E[Y-\mu]^2 + E[(Y-\mu)(\mu-t)] + E[\mu-t]^2\\\\  
& \\geq E[Y-\mu]^2
\end{align\*}

Applied to conditional distribution of \\(Y\\) given \\(X=x\\), this gives us that \\[ E_{Y\|X}([Y-E(Y\|X=x)]^2\|X=x) \leq E_{Y\|X} ([Y-c]^2\|X=x)\\] for all estimates \\(c\\). Hence, proved.