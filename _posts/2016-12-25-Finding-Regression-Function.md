---
layout: post
title: Finding Regression Function
categories: ['Automated Reasoning']
tags: ['ml', 'regression', 'proofs']
math: true
starred: true
permalink: /proof/finding-regression-function/
---

Let 
\\(X\in R^P\\) denote a real valued random input vector, and \\(Y\in R\\) a real valued random output variable, with joint distribution \\(Pr(X,Y)\\).
We seek a function \\(f(X)\\) for predicting \\(Y\\) given values of the input \\(X\\). This theory requires a _loss function_ \\(L(Y,f(X))\\) for penalizing errors in prediction, and by far the most common and convenient is _squared error loss:_ \\(L(Y,f(X))=(Y-f(X))^2\\). This leads us to a criterion for choosing \\(f\\),

\begin{align\*} 
EPE(f)& = E(Y-f(X))^2\\\\  
& = \int(y-f(x))^2 Pr(dx,dy)
\end{align\*}

the expected (squared) prediction error. By conditioning on \\(X\\), we can write \\(EPE\\) as \\[EPE(f) = E_X E_{Y|X} ((Y-f(X))^2|X)\\]
and we see that it suffices to minimize \\(EPE\\) pointwise:
\begin{equation}
\label{epe}
f(x)=argmin_c E_{Y|X} ([Y-c]^2|X=x)
\end{equation}

For any guess t by writing \\(\mu = E(Y)\\), we have \\(EPE(f)\\) as
\begin{align\*}
E[Y-t]^2 & = E[Y-\mu + \mu-t]^2\\\\  
& = E[Y-\mu]^2 + E[(Y-\mu)(\mu-t)] + E[\mu-t]^2\\\\  
& \\geq E[Y-\mu]^2
\end{align\*}

Applied to conditional distribution of \\(Y\\) given \\(X=x\\), this gives us that \\[ E_{Y\|X}([Y-E(Y\|X=x)]^2\|X=x) \leq E_{Y\|X} ([Y-c]^2\|X=x)\\] for all estimates \\(c\\).

Hence, proved that the solution for equation \eqref{epe} above is \\[f(x)=E(Y|X=x)\\]
the conditional expectation, also known as the _regression_ function.