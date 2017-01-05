---
layout: post
title: From Least Squares to Nearest Neighbors
categories: ['Automated Reasoning']
tags: ['ml', 'regression']
math: true
starred: true
code: https://github.com/gmahajanml/linear-to-nearest
---
Let's evaluate two simple but powerful prediction methods: the [linear model
fit by least squares]({{site.baseurl}}/what/linear-models/) and the \\(k\\)-[nearest-neighbor]({{site.baseurl}}/what/nearest-neighbors-mentods/) prediction rule on a simple [regression problem]({{site.baseurl}}/what/regression-problem/). We will see how the linear model makes huge assumptions about structure and yields stable but possibly inaccurate predictions. The method of \\(k\\)-nearest neighbors, however, makes very mild structural assumptions: its predictions are often accurate but can be unstable.

## Training Data
Consider the two possible scenarios:

**Scenario 1:** The training data in each class were generated from bivariate Gaussian distribution with uncorrelated components and different means.

**Scenario 2:** The training data in each class came from a mixture of 10 low-variance Gaussian distributions, with individual means themselves distributed as Gaussian.

page_break

We first create training data for \\(Y=1\\), denoted by \\(X_{pos}\\) and 
\\(Y=-1\\), denoted by \\(X_{neg}\\).

{% highlight matlab %}
mu_pos = [2,3];
mu_neg = [0,3];
sigma = [1,1.5;1.5,3];
X_pos = mvnrnd(mu_pos,sigma,100);
X_neg = mvnrnd(mu_neg,sigma,100);
{% endhighlight %}

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

page_break

## Results for Least Squares
This is how least squares performs on both scenarios.

{% highlight matlab %}
X = [zeros(200,1)+1 [X_neg; X_pos]];
Y = [zeros(100,1)-1;zeros(100,1)+1];
param = inv(transpose(X)*X)*transpose(X)*Y;
{% endhighlight %}

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

page_break

## Results for Nearest Neighbors
This is how nearest neighbors performs for k=10.

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

page_break

## Conclusion
We can actually develop some theory that provides a framework for developing models such as those discussed informally so far. As discussed in another [post]({{site.baseurl}}/proof/finding-regression-function/), for squared error loss function, the regression function is \\[f(x)=E(Y|X=x)\\]

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
    function. The linear decision boundary from least squares is very smooth, and apparently
stable to fit. More suitable for scenario 1.
  </li>
  <li>
    k-nearest neighbors assumes f(x) is well approximated by a locally constant function. Thus, it is wiggly and unstable. It however does not
rely on any stringent assumptions about the underlying data, and can adapt
to any situation. More suitable for scenario 2.
  </li>
</ul>