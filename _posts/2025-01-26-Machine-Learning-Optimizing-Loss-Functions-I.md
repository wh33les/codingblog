---
layout: post
title: Machine learning&#58; Optimizing loss functions I.
date: 2025-01-26 #13:00:00 -06:00
---
I recently finished a Kaggle competition where I had to predict housing prices, given 79 features.  I tested several models, including linear and polynomial regression, some tree-based algorithms, and most interestingly, a neural network using PyTorch.  I learned a lot about neural networks in the process, and I wanted to touch on one aspect in these next two posts, loss functions.

A **neural network** can be thought of as a composition of affine functions with "activation functions" between each one, that make the overall composition non-linear.  In this way one can approximate any continuous function arbitrarily closely with the right neural network.  This is an example of a [universal approximation theorem.](https://en.wikipedia.org/wiki/Universal_approximation_theorem)  The $i$th affine function $A_i:\mathbb R^{m_i}\to \mathbb R^{n_i}$ has the form $A_i(\vec x) = W_i\vec x +\vec b_i$, where $W_i$ is a matrix, and the **parameters** $W_i$, $\vec b_i$ for all $i$ can be tuned to give the best approximation by **training** the neural network.  Training a neural network consists of step-by-step modifying the parameters to minimize a **loss function**, a function that measures the error of the predictions made on the training data.  

## Example: Linear regression

Linear regression is an example of a neural network with one affine function $f_{\vec\beta}:X\vec\beta$, where 

$$X = \begin{pmatrix}1 & x_{11} & \cdots & x_{1m} \\
1 & x_{12} & \cdots & x_{2m} \\
\vdots & \vdots & \cdots & \vdots \\
1 & x_{1n} & \cdots & x_{nm}
\end{pmatrix}$$

denotes $m$ features and $n$ observations, $\vec\beta = \left(\beta_0,\beta_1,\dots,\beta_m\right)$, and there is no activation function.  The loss function is the mean square error, 
$$\text{MSE}\left(\vec\beta\right) = \frac{1}{n}\|\vec y - X\vec\beta\|^2.$$
There are two ways to minimize this function.  

The most direct way is the linear algebra approach, which I will go over briefly because it's so concise.  Minimizing the distance between $\vec y$ and $X\vec\beta$ is the same as finding the length of the projection of the vector $\vec y$ to the hyperplane spanned by the columns of $X$.  The coefficients $\hat{\vec\beta}=(\hat\beta_0,\cdots,\hat\beta_m)$ of the projection written as a linear combination of the $\vec x_i$s give the parameters that optimize the linear approximation.  We have the formula
$$\vec y = X\hat{\vec\beta} + \vec y_{\perp}$$,
where $\vec y_{\perp}$ is perpendicular to $X\hat{\vec\beta}$.  This means it is killed by the transpose of $X$, so we have

$$
X^T\vec y = X^T(X\hat{\vec\beta} + \vec y_{\perp})\quad\text{implies}\quad
X^T\vec y = X^TX\hat{\vec\beta} 
\quad\text{implies}\quad
\hat{\vec\beta} = \left(X^TX\right)^{-1}X^T\vec y.
$$ 

This is also known as the **normal equation** (and note, it doesn't work if the features are not linearly independent -- in that case one has to do feature selection to remove redundancies).

The other way uses multivariate calculus, and while it's more cumbersome than the linear algebra method, it's also the method that's generalizable to neural networks.  For multivariable functions, optima occur where the gradient is zero.  The loss function in this case happens to be [convex,](https://en.wikipedia.org/wiki/Convex_function) which guarantees anywhere the gradient is zero is a global minumum.  Thus we just set the gradient equal to zero and solve for $\hat\beta$.

$$
\nabla \text{MSE}(\vec\beta) = \lim_{\vec h\to \vec 0}\frac{\text{MSE}\left(\vec\beta+\vec h\right)-\text{MSE}(\vec\beta)}{\vec h} 
\\
= \lim_{\vec h\to \vec 0}\frac{\|\vec y-X\hat{\vec\beta}\|^2 - 2\left(\vec y-X\hat{\vec\beta}\right)\cdot(X\vec h) + \|X\vec h\|^2 - \|\vec y-X\hat{\vec\beta}\|^2}{\vec h} 
\\
= \lim_{\vec h\to \vec 0}\frac{\|\vec y-X\hat{\vec\beta}\|^2 - 2X^T\left(\vec y - X\hat{\vec\beta}\right)\cdot\vec h + \|X\vec h\|^2 - \|\vec y-X\hat{\vec\beta}\|^2}{\vec h}
\\
= - 2X^T(\vec y - X\hat{\vec\beta})
$$

Now set to zero and solve:

$$
-2X^T\left(\vec y-X\hat{\vec\beta}\right) = 0 

\quad\text{implies}\quad \hat{\vec\beta} = \left(X^TX\right)^{-1}X^T\vec y,
$$

which verifies the linear algebra technique.

So how does this generalize to neural networks?  Neural networks can use different loss functions, but most commonly MSE is used for regression problems and cross-entropy is used for classification.  Difficulty arises because these functions may not be convex -- even MSE is no longer convex once activation functions make the model non-linear.  In the next post I will describe the different methods of minimizing the loss functions used in neural networks.  Stay tuned!