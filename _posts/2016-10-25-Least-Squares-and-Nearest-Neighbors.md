---
layout: post
title: Least Squares and Nearest Neighbors
categories: ['Automated Reasoning']
tags: ['ml', 'regression']
math: true
---
The regression problem [^1] involves modeling how the expected value
of a response \\(y\\) changes in response to changes in an 
explanatory variable \\(x\\) : \\[ E(y|x) = f(x)\\]

Lets develop two simple but powerful prediction methods for \\( f(x)\\): the linear model
fit by least squares and the \\(k\\)-nearest-neighbor prediction rule.
The linear model makes huge assumptions about structure and yields stable
but possibly inaccurate predictions. The method of \\(k\\)-nearest neighbors makes very mild structural assumptions: its predictions are often accurate
but can be unstable.

## Least Squares
The linear model, as its name implies, assumes a linear
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

## Nearest Neighbors
Nearest-neighbor methods [^2] use those observations in 
the training set \\(T\\) closest in input space to \\(x\\)
to form \\(\hat{Y}\\) . Specifically, the \\(k\\)-nearest neighbor fit
for \\(\hat{Y}\\) is defined as follows:
\\[\hat{Y}(x)= \dfrac{1}{k} \sum_{x_i\in N_k(x)} y_i\\]

where \\(N_k(x)\\) is the neighborhood of \\(x\\) defined by the 
\\(k\\) closest points \\(x_i\\) in the training sample.

## _From_ Least Squares _to_ Nearest Neighbors
Consider the two possible scenarios:

**Scenario 1:** The training data in each class were generated from bivariate Gaussian distribution with uncorrelated components and different means.

**Scenario 2:** The training data in each class came from a mixture of 10 low-variance Gaussian distributions, with individual means themselves distributed as Gaussian.

Let's evaluate Linear Model and Nearest Neighbors on both scenarios. We first create training data for \\(Y=1\\), denoted by \\(X_{pos}\\) and 
\\(Y=-1\\), denoted by \\(X_{neg}\\).

{% highlight matlab %}
mu_pos = [2,3];
mu_neg = [0,3];
sigma = [1,1.5;1.5,3];
X_pos = mvnrnd(mu_pos,sigma,100);
X_neg = mvnrnd(mu_neg,sigma,100);
{% endhighlight %}

[GitHub Code](https://github.com/gmahajanml/linear-to-nearest)

<div class="row">
  <div class="col-md-6">
    <div class="thumbnail">
      <a href="{{ site.baseurl }}/images/linear_nearest/scenerio1.png" target="_blank">
        <img src = "{{ site.baseurl }}/images/linear_nearest/scenerio1.png">
        <div class="caption" style="text-align: center;">
          <p>Training Data for Scenario 1</p>
        </div>
      </a>
    </div>
  </div>
  <div class="col-md-6">
    <div class="thumbnail">
      <a href="{{ site.baseurl }}/images/linear_nearest/scenerio2.png" target="_blank">
        <img src = "{{ site.baseurl }}/images/linear_nearest/scenerio2.png">
        <div class="caption" style="text-align: center;">
          <p>Training Data for Scenario 2</p>
        </div>
      </a>
    </div>
  </div>
</div>

## Results for Least Squares
This is how least squares performs on both scenarios.

{% highlight matlab %}
X = [zeros(200,1)+1 [X_neg; X_pos]];
Y = [zeros(100,1)-1;zeros(100,1)+1];
param = inv(transpose(X)*X)*transpose(X)*Y;
{% endhighlight %}

[GitHub Code](https://github.com/gmahajanml/linear-to-nearest)

<div class="row">
  <div class="col-md-6">
    <div class="thumbnail">
      <a href="{{ site.baseurl }}/images/linear_nearest/scenerio1_linear.png" target="_blank">
        <img src = "{{ site.baseurl }}/images/linear_nearest/scenerio1_linear.png">
        <div class="caption" style="text-align: center;">
          <p>Linear Squares on Scenario 1</p>
        </div>
      </a>
    </div>
  </div>
  <div class="col-md-6">
    <div class="thumbnail">
      <a href="{{ site.baseurl }}/images/linear_nearest/scenerio2_linear.png" target="_blank">
        <img src = "{{ site.baseurl }}/images/linear_nearest/scenerio2_linear.png">
        <div class="caption" style="text-align: center;">
          <p>Linear Squares on Scenario 2</p>
        </div>
      </a>
    </div>
  </div>
</div>


## Results for Nearest Neighbors
This is how nearest neighbors performs for k=10.

{% highlight matlab %}
X = [X_neg; X_pos];
Y = [zeros(100,1)-1;zeros(100,1)+1];
M = size(X)(1);
l = linspace(-10, 10, 100);
m = linspace(-10, 10, 100);
[X1,X2] = meshgrid(l, m);
N = length(X1(:));
classes = zeros(size(X1));
for i = 1:N
this = [X1(i) X2(i)];
dists = sum((X - repmat(this,M,1)).^2,2);
[d I] = sort(dists,'ascend');
neighbors = Y(I(1:10));
prediction = sum(neighbors);
if prediction>0
classes(i)=1;
else
classes(i)=-1;
end
end
contour(l,m,classes,[1,1]);
{% endhighlight %}

[GitHub Code](https://github.com/gmahajanml/linear-to-nearest)

<div class="row">
  <div class="col-md-6">
    <div class="thumbnail">
      <a href="{{ site.baseurl }}/images/linear_nearest/scenerio1_nearest.png" target="_blank">
        <img src = "{{ site.baseurl }}/images/linear_nearest/scenerio1_nearest.png">
        <div class="caption" style="text-align: center;">
          <p>Nearest Neighbors on Scenario 1</p>
        </div>
      </a>
    </div>
  </div>
  <div class="col-md-6">
    <div class="thumbnail">
      <a href="{{ site.baseurl }}/images/linear_nearest/scenerio2_nearest.png" target="_blank">
        <img src = "{{ site.baseurl }}/images/linear_nearest/scenerio2_nearest.png">
        <div class="caption" style="text-align: center;">
          <p>Nearest Neighbors on Scenario 2</p>
        </div>
      </a>
    </div>
  </div>
</div>

## Statistical Decision Theory
We can actually develop some theory that provides a framework for developing models such as those discussed informally so far. Let 
\\(X\in R^P\\) denote a real valued random input vector, and \\(Y\in R\\) a real valued random output variable, with joint distribution \\(Pr(X,Y)\\).
We seek a function \\(f(X)\\) for predicting \\(Y\\) given values of the input \\(X\\). This theory requires a _loss function_ \\(L(Y,f(X))\\) for penalizing errors in prediction, and by far the most common and convenient is _squared error loss:_ \\(L(Y,f(X))=(Y-f(X))^2\\). This leads us to a criterion for choosing \\(f\\),

\begin{align\*} 
EPE(f)& = E(Y-f(X))^2\\\\  
& = \int(y-f(x))^2 Pr(dx,dy)
\end{align\*}

the expected (squared) prediction error. By conditioning on \\(X\\), we can write \\(EPE\\) as \\[EPE(f) = E_X E_{Y|X} ((Y-f(X))^2|X)\\]
and we see that it suffices to minimize \\(EPE\\) pointwise:
\\[f(x)=argmin_c E_{Y|X} ((Y-c)^2|X=x)\\]
The solution is \\[f(x)=E(Y|X=x)\\]
the conditional expectation, also known as the _regression_ function.

The nearest-neighbor methods attempt to directly implement this recipe using the training data. At each point \\(x\\), we might ask for the average of all those \\(y_is\\) with input \\(x_i=x\\). Since there is typically at most one observation at any point \\(x\\), we settle for 
\\[\hat{f}(x)=Ave(y_i|x_i \in N_k(x))\\]
where \\("Ave"\\) denotes average, and \\(N_k(x)\\) is the neighborhood
containing the \\(k\\) points in \\(T\\) closest to \\(x\\). Two approximations are happening here:
<ul>
  <li>expectation is approximated by averaging over sample data;</li>
  <li>conditioning at a point is related to conditioning on some region "close" to the target point</li>
</ul>  

How does linear regression fit into this framework? The simplest explanation is that one assumes that the regression function \\(f(x)\\) is approximately linear in its arguments: \\[f(x) \approx x^T\beta\\]
This is a model-based approach- we specify a model for the regression function. Plugging this linear model for \\(f(x)\\) into \\(EPE\\) and differentiating we can solve for \\(\beta\\) theoretically:
\\[\beta = (E(XX^T))^{-1} E(XY)\\]
Note we have not conditioned on \\(X\\); rather we have used our knowledge of the functional relationship to _pool_ over values of \\(X\\). The least squares solution amounts to replacing the expectation b averages overs the training data.

So both \\(k\\)-nearest neighbors and least squares end up approximating
conditional expectations by averages. But they differ dramatically in terms
of model assumptions:
<ul>
  <li>  
    Least squares assumes f(x) is well approximated by a globally linear
    function.  
  </li>
  <li>
    k-nearest neighbors assumes f(x) is well approximated by a locally constant function.
  </li>
</ul>  

## References:
[^1]: Breheny, Patrick. "Introduction to nonparametric regression: Least squares vs. Nearest neighbors". Lecture.

[^2]: Hastie, Trevor, Robert Tibshirani, and J. H. Friedman. The Elements of Statistical Learning: Data Mining, Inference, and Prediction: With 200 Full-color Illustrations. New York: Springer, 2001. Print.