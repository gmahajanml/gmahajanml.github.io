---
layout: post
title: What is Linear Models and Least Squares?
categories: ['What is']
tags: ['ml', 'regression']
math: true
permalink: /what/linear-models/
---
For the [regression problem]({{site.baseurl}}/what/regression-problem/), the linear model, as its name implies, assumes a linear
relationship. Given a vector of inputs 
\\( X^T = (X_1, X_2,\ldots,X_p)\\), we predict the output 
\\(Y\\) via the model
\\[ \hat{Y} = \hat{\beta_0} + \sum_{j=1}^{p}X_j\hat{\beta_j}\\]

By including the constant variable \\(1\\) in \\(X\\), 
including \\(\hat{\beta_0}\\) in the vector of coefficients 
\\(\hat{\beta}\\), we can write the linear model in vector form as an inner product
\\[\hat{Y} = X^T\hat{\beta}\\]

To fit the linear model to a set of training data, we use the method of least squares. In this approach, we pick the coefficients \\(\beta\\)
to minimize the residual sum of squares
\\[ RSS(\beta) = \sum_{i=1}^{N}(y_i-x_{i}^{T}\beta)^2\\]