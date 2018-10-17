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

![alt text]("Line of Best Fit")

Essentially, that is what regression is.
We are trying to find the line that best fits the data using mathematical techniques.
We normally define $y$ to be dependent variable and $x$ to be the independent variable, and
this is simply a matter of notation. When we say that one variable is dependent and the other is independent, what we are really saying is that there is some function that maps from the independent variable to the dependent variable such that the value of one variable is dependent on the other.

## Simple Linear Regression
In regression's most basic form, *Simple Linear Regression*, we model a linear relationship between $x$ and $y$:
\begin{equation}
y = α + βx
\label{eq1}
\end{equation}
$β$ is the slope or the gradient of the straight line while $α$ is the $y$-intercept.
$α$ and $β$ are known as the *model parameters*, as they are what the model really depends on.

In order to train this model on the data that we have, we have to find the optimal $\hat{α}$ and $\hat{β}$
that provide the "best fit". At this point, we have to rigorously define what is the "best fit". In this section, we will take the "best fit" to be the *ordinary least squares* approach. It can be described as follows.

Given the $i$th data point, ($x_{i}$, $y_{i}$), the prediction of the model will be
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
large, we will know that the line is far away from the data points and is not a good approximation. Therefore the sum of the all the residuals will be a good gauge for how well our model performs. However, it is important to consider that some of these residuals might be negative, which would mean that when we summed them all up, there is a possibility they would cancel each other out. In order to solve this issue, we square the residuals and then sum them up. Geometrically, the sum of the squared residuals can be represented in the cartesian plane:

![alt text]("Sum of Squared Residuals")

Therefore it is clear that order to obtain the optimal model, we will have to minimise the sum of the squared residuals.  Mathematically, the sum of the squared residuals would be a function of the model parameters. The sum of the squared residuals is a type of *Error* or *Loss* function, which is generally a class of functions of the model parameters that tell you how "bad" your model is. Mathematically,
\begin{equation}
S(α, β) = \sum_{i=0}^n ϵ_{i}^2 = \sum_{i=0}^n (y_{i} - α - βx_{i})^2
\label{eq5}
\end{equation}
The optimal model, given by the optimal model parameters $\hat{α}$ and $\hat{β}$, minimises $S$, which is our loss function.
Mathematically,
\begin{equation}
S(\hat{α}, \hat{β}) = \min_{α, β} {S(α, β)} = \min_{α, β} {\sum_{i=0}^n (y_{i} - α - βx_{i})^2}
\label{eq6}
\end{equation}

We can find optimise $S$ by taking the partial derivative of $S$ with respect to $α$ and $β$ respectively, equating the derivatives to $0$ and solving for the optimal model parameters, which come out to be

$$
  \begin{align}
    α &= \frac{\bar{y}\sum_{i=0}^n {x_i^2} - \bar{x}\sum_{i=0}^n {x_iy_i}}{\sum_{i=0}^n {x_i^2} - n(\bar{x})^2} \label{eq7} \\\\
    β &= \frac{\sum_{i=0}^n {x_iy_i} - n\bar{x}\bar{y}}{\sum_{i=0}^n {x_i^2} - n(\bar{x})^2} \label{eq8}
  \end{align}
$$

I will not derive them here, but please feel free to attempt the derivation yourself. A proof of \eqref{eq7} and \eqref{eq8} should be able to be found very easy online as well.

## Gradient Descent

## Non-Linear regression

## Multiple Linear Regression

## Coding a Regression Algorithm from Scratch
