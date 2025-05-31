---
layout: post
title:  "Regression"
date:   2018-10-17 00:01:00 +0800
category: post
tags: [algorithms]
---

* TOC
{:toc}
Regression is one of the most basic and fundamental Machine Learning techniques,
 which lays the foundation for many of the more complicated techniques and algorithms
that are used in practise. In this article, we will look at the mathematical formulation
of regression models, how to train them, as well as write some code from scratch
in python to make and train our very own regression algorithm.

## What is Regression?
Regression, very simply, is a statistical model that allows us to model the relationship
between two variables. One of these variables is known as the *independent variable*
and the other one is known as the *dependent variable*. You might remember drawing
a "best-fit" line from high school physics or mathematics:

![alt text](https://www.mathsisfun.com/data/images/scatter-ice-cream1a.svg "Line of Best Fit")

Essentially, that is what regression is.
We are trying to find the line that best fits the data using mathematical techniques.
We normally define $y$ to be dependent variable and $x$ to be the independent variable, and
this is simply a matter of notation. When we say that one variable is dependent and the other is
independent, what we are really saying is that there is some function that maps from the independent
variable to the dependent variable such that the value of one variable is dependent on the other.

## Simple Linear Regression
In regression's most basic form, *Simple Linear Regression*, we model a linear relationship between $x$
and $y$:
\begin{equation}
y = α + βx
\label{eq1}
\end{equation}
$β$ is the slope or the gradient of the straight line while $α$ is the $y$-intercept.
$α$ and $β$ are known as the *model parameters*, as they are what the model really depends on.

In order to train this model on the data that we have, we have to find the optimal $\hat{α}$ and $\hat{β}$
that provide the "best fit". At this point, we have to rigorously define what is the "best fit". In this
section, we will take the "best fit" to be the *ordinary least squares* approach. It can be described as
follows.

Given the $i$th data point, $(x_{i}, y_{i})$, the prediction of the model will be
\begin{equation}
\hat{y_{i}} = α + βx_{i}
\label{eq2}
\end{equation}
However, the true value of $y_{i}$ is some
\begin{equation}
y_{i} = α + βx_{i} + ϵ_{i}
\label{eq3}
\end{equation}
Where $ϵ_{i}$ is known as the *residual term*, which is the difference between the observed value
and the estimated value. Rearranging,
\begin{equation}
ϵ_{i} = y_{i} - α - βx_{i}
\label{eq4}
\end{equation}
Geometrically, $ϵ_{i}$ is the vertical distance from the data point to the line. If the residual is
large, we will know that the line is far away from the data points and is not a good approximation.
Therefore the sum of the all the residuals will be a good gauge for how well our model performs. However,
it is important to consider that some of these residuals might be negative, which would mean that when we
sum them all up, there is a possibility they would cancel each other out. In order to solve this issue, we
square the residuals and then sum them up. Geometrically, the sum of the squared residuals can be
represented in the cartesian plane:

![alt text]({{site.url}}/assets/images/2018-10-17-regression/squared_residuals.png "Sum of Squared Residuals")

Therefore it is clear that order to obtain the optimal model, we will have to minimise the sum of the
squared residuals.  Mathematically, the sum of the squared residuals would be a function of the model
parameters. The sum of the squared residuals is a type of *Loss* or *Cost* function, which is generally a
class of functions of the model parameters that tell you how "bad" your model is. Mathematically,
\begin{equation}
S(α, β) = \sum_{i=0}^n ϵ_{i}^2 = \sum_{i=0}^n (y_{i} - α - βx_{i})^2
\label{eq5}
\end{equation}
The optimal model, given by the optimal model parameters $\hat{α}$ and $\hat{β}$, minimises $S$, which is
our loss function.
Mathematically,
\begin{equation}
S(\hat{α}, \hat{β}) = \min_{α, β} {S(α, β)} = \min_{α, β} {\sum_{i=0}^n (y_{i} - α - βx_{i})^2}
\label{eq6}
\end{equation}

We can find optimise $S$ by taking the partial derivative of $S$ with respect to $α$ and $β$ respectively,
equating the derivatives to $0$ and solving for the optimal model parameters, which come out to be

$$
  \begin{align}
    α &= \frac{\bar{y}\sum_{i=0}^n {x_i^2} - \bar{x}\sum_{i=0}^n {x_iy_i}}{\sum_{i=0}^n {x_i^2} - n(\bar{x})^2} \label{eq7} \\\\
    β &= \frac{\sum_{i=0}^n {x_iy_i} - n\bar{x}\bar{y}}{\sum_{i=0}^n {x_i^2} - n(\bar{x})^2} \label{eq8}
  \end{align}
$$

I will not derive them here, but please feel free to attempt the derivation yourself. A proof of
\eqref{eq7} and \eqref{eq8} should be able to be found very easy online as well.

## Multiple Linear Regression
Up till this point, we considered simple linear regression, which is really just trying to model a
univariate function. However, most times in practise, we have a multivariate function where the
dependent variable is a linear unction of multiple independent variables. Therefore for some observation
$(x_{i0}, x_{i1},…,x_{ip}, y_i)$ out of $n$ observations,

$$
\begin{equation}
y_i = β_{0}x_{i0} + β_{1}x_{i1} + … + β_{p}x_{ip} + ϵ_i
\label{eq9}
\end{equation}
$$

Where we define $x_{i0}=1$ so that $β_0$ can serve as a constant term, and $ϵ_i$ is the error term.

Before we go on, we have arrived at the point where all the fancy subscripts can start become hard to keep
track of. We can solve this problem by greatly simplifying our equations by writing them in terms of
matrices and vectors. Let us define:

$$
\begin{equation}
\mathbf{y}=
\begin{pmatrix}
  y_1 \\\\
  y_2 \\\\
  ⋮ \\\\
  y_n
\end{pmatrix}
\end{equation}
\label{eq10}
$$

$$
\begin{equation}
\mathbf{X} =
\begin{pmatrix}
  1 & x_{11} & … & x_{1p} \\\\
  1 & x_{21} & … & x_{2p} \\\\
  ⋮ & ⋮ & ⋱ & ⋮ \\\\
  1 & x_{n1} & ⋯ & x_{np}
\end{pmatrix}
\end{equation}
\label{eq11}
$$

$$
\begin{equation}
\mathbf{β}=
\begin{pmatrix}
  β_0 \\\\
  β_1 \\\\
  ⋮ \\\\
  β_p
\end{pmatrix}
\end{equation}
\label{eq12}
$$

$$
\begin{equation}
\mathbf{ϵ}=
\begin{pmatrix}
  ϵ_1 \\\\
  ϵ_2 \\\\
  ⋮ \\\\
  ϵ_n
\end{pmatrix}
\end{equation}
\label{eq13}
$$

Now the relationship can be simplified to:

$$
\begin{equation}
\mathbf{y} = \mathbf{X}\mathbf{β} + \mathbf{ϵ}
\label{eq14}
\end{equation}
$$

Our model will predict some:

$$
\begin{equation}
\mathbf{\hat{y}} = \mathbf{X}\mathbf{β}
\label{eq15}
\end{equation}
$$

The process to solve for the optimal model parameters $\mathbf{β}$ is still the same. First, we must
formulate a cost function. We will now use a very common cost function, the *Mean Squared Error*, which is
essentially the same thing as what we did earlier, just that now we take the mean of the total sum of the
squared residuals:

$$
\begin{equation}
  \begin{split}
    S(\mathbf{β}) &= \frac{1}{n}‖\mathbf{ϵ}‖^2 \\\\
                  &= \frac{1}{n}‖\mathbf{y}-\mathbf{\hat{y}}‖^2 \\\\
                  &= \frac{1}{n}‖\mathbf{y}-\mathbf{X}\mathbf{β}‖^2
  \end{split}
\label{eq16}
\end{equation}
$$

The optimal model parameters are:$\DeclareMathOperator*{\argmin}{arg\,min}$

$$
  \begin{equation}
  \label{eq17}
    \mathbf{\hat{β}} = \argmin_{\mathbf{β}} {S(\mathbf{β})}  
  \end{equation}
$$

In order to minimise the cost function, we take its derivative with respect to $\mathbf{β}$ and equate it
to 0.

$$
  \begin{gathered}
    ∇S(\mathbf{\hat{β}}) = 0 \\\\
    \frac{∂}{∂\mathbf{β}}(\frac{1}{n}‖\mathbf{y}-\mathbf{X}\mathbf{β}‖^2)\Bigg|_{\mathbf{β}=\mathbf{\hat{β}}} = 0 \\\\
    \frac{1}{n}\frac{∂}{∂\mathbf{β}}(\mathbf{y}-\mathbf{X}\mathbf{β})^\mathrm{T}(\mathbf{y}-\mathbf{X}\mathbf{β})\Bigg|_{\mathbf{β}=\mathbf{\hat{β}}} = 0 \\\\
    \frac{1}{n}\frac{∂}{∂\mathbf{β}}(\mathbf{y}^\mathrm{T}\mathbf{y} - \mathbf{β}^\mathrm{T}\mathbf{X}^\mathrm{T}\mathbf{y} - \mathbf{y}^\mathrm{T}\mathbf{X}\mathbf{β} + \mathbf{β}^\mathrm{T}\mathbf{X}^\mathrm{T}\mathbf{X}\mathbf{β})\Bigg|_{\mathbf{β}=\mathbf{\hat{β}}} = 0 \\\\
    (\mathbf{β}^\mathrm{T}\mathbf{X}^\mathrm{T}\mathbf{y})^\mathrm{T} = \mathbf{y}^\mathrm{T}\mathbf{X}\mathbf{β}\,\text{has the dimensions 1x1, so it is equal to its own transpose and}\,\mathbf{β}^\mathrm{T}\mathbf{X}^\mathrm{T}\mathbf{y} = \mathbf{y}^\mathrm{T}\mathbf{X}\mathbf{β} \\\\⁠
    \frac{1}{n}\frac{∂}{∂\mathbf{β}}(\mathbf{y}^\mathrm{T}\mathbf{y} - 2\mathbf{β}^\mathrm{T}\mathbf{X}^\mathrm{T}\mathbf{y} + \mathbf{β}^\mathrm{T}\mathbf{X}^\mathrm{T}\mathbf{X}\mathbf{β})\Bigg|_{\mathbf{β}=\mathbf{\hat{β}}} = 0 \\\\
    \frac{2}{n}(-\mathbf{X}^\mathrm{T}\mathbf{y} + (\mathbf{X}^\mathrm{T}\mathbf{X})\mathbf{\hat{β}}) = 0 \\\\
  \end{gathered} \\\\
$$

And finally, we have:

$$
  \begin{equation}
  \label{eq18}
    \mathbf{\hat{β}} = (\mathbf{X}^\mathrm{T}\mathbf{X})^{-1}\mathbf{X}^\mathrm{T}\mathbf{y}
  \end{equation}
$$

Equation \eqref{eq18} is exactly what we worked so hard for. This vector contains the optimal model
parameters, which means we have found the optimal model that fits our data.

## Gradient Descent
Up till this point, we have only considered *analytical solutions*. Looking back at equation \eqref{eq18}, if
we have a large $\mathbf{X}$ (which happens *very* often), the computation of the inverse
$(\mathbf{X}^\mathrm{T}\mathbf{X})^{-1}$ will take a substantial amount of time. Furthermore, keeping such
a large matrix in memory to run computations on can be very memory inefficient.

We can solve this issue by introducing numerical methods for optimisation. The one we will talk about here
is gradient descent (more accurately *batch* gradient descent), as it is the most standard and common
iterative algorithm used for optimisation in machine learning. Most other optimisation algorithms used in
machine leanining can be derived by modifying standard gradient descent in one way or another.

Gradient descent is a rather intuitive algorithm, and really isn't very hard to understand at all. Let's
say that you are trying to climb down a hill, but blindfolded. You want to get to the bottom of the hill
in the shortest way possible. Since you are blindfolded, you can only feel the steepness of the terrain in
the near vicinity of you with your foot. Intuitively, what you would do is to try to find the direction
where the descent is the steepest, and take a step in that direction. Then you would try to find the
direction of steepest descent again, and take another step in that direction. You would repeat this until
the terrain became nearly flat, at which point you would know that you have reached the bottom of the
hill.

Gradient descent works exactly this way! Let $F(\mathbf{x})$ be a multi-variable function that is defined
and differentiable in the neighbourhood of a point $\mathbf{a}$. Then in order to find the shortest path
to the minimum point, we would need to move along the direction of steepest descent. We know that the
direction of steepest ascent is given by the gradient of the function, $∇F(\mathbf{a})$, therefore the
direction of steepest descent must be given by the *negative* gradient, $-∇F(\mathbf{a})$. We then need to
update $\mathbf{a}$ such that we "take a step" from $\mathbf{a}$ in the direction of the negative
gradient. We can write an update rule as follows:

$$
  \begin{equation}
  \label{eq19}
    \mathbf{a} \gets \mathbf{a} - \gamma∇F(\mathbf{a})
  \end{equation}
$$

Where $\gamma\in\mathbb{R}^{+}$ is known as the step parameter. This parameter controls the "step size",
which is how much you move $\mathbf{a}$ in the direction of the negative gradient. Intuitively, a larger
step parameter would mean that we move towards the minimum faster and in less iterations, however it would
be much easier to overshoot it. A smaller step parameter would mean that convergence to the minimum is
slower, but the probability that we would hit it is higher. Many solutions exist for this problem, amongst
which probably the most popular is introducing an "adaptive" step parameter. This parameter starts off
big, and as the gradient gets less steep (which is indicative of approaching minima), the parameter is
decreased so that the algorithm does not overshoot. I will not cover the specifics here nor will I
implement it.

Instead of analyti d cally solving for the optimal model parameters, we can now use gradient descent with our
cost function! Let us modify \eqref{eq16} to be the "one half" mean squared error. This is purely so that
when we differentiate our cost function later, we will get rid of that pesky $2$ in the numerator.

$$
  \begin{equation}
  \label{eq20}
    S(\mathbf{β}) = \frac{1}{2n}‖\mathbf{y}-\mathbf{X}\mathbf{β}‖^2
  \end{equation}
$$

We now differentiate:

$$
  \begin{equation}
  \label{eq21}
    ∇S = \frac{1}{n}(-\mathbf{X}^\mathrm{T}\mathbf{y} + \mathbf{X}^\mathrm{T}\mathbf{X}\mathbf{β})
  \end{equation}
$$

And finally, we combine equations \eqref{eq19} and \eqref{eq21} into a single update rule:

$$
  \begin{equation}
  \label{eq22}
    \begin{split}
      \mathbf{β_{q+1}} &= \mathbf{β_q} - γ∇S(\mathbf{β_q}) \\\\
                       &= \mathbf{β_q} - \frac{γ}{n}(-\mathbf{X}^\mathrm{T}\mathbf{y} + \mathbf{X}^\mathrm{T}\mathbf{X}\mathbf{β_q}) \\\\
                       &= \mathbf{β_q} - \frac{γ}{n}\mathbf{X}^\mathrm{T}(\mathbf{X}\mathbf{β_q} - \mathbf{y})
    \end{split}
  \end{equation}
$$

How do we know when to stop? Some variations of this algorithm stop when the gradient is within a certain
range that is close to zero that can be arbitrarily defined, such as $-0.001≤∇S≤0.001$. Other variations
just run the algorithm for a large predetermined number of iterations, known in machine learning terms as
*epochs* (this is the method I will be implemeting later). Other variations include storing the previous
epochs' cost in a variable, computing the cost in the current epoch, and subtracting the two to see if the
difference is smaller than a certain arbitrary threshold, upon which the algorithm can be halted.

## Coding a Regression Algorithm from Scratch
