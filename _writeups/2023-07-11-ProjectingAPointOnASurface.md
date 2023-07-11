---
title: Projection of Point on a Surface
layout: post
---

---
**NOTE**

Let's fix some notation.

A vector valued function $$f$$ will  be understood as a column matrix. For example, if $$f : \Re^2 \rightarrow \Re^3$$ then

$$
f(u,v) =
\left[\begin{array}{ccc}
f_1(u,v)\\
f_2(u,v)\\
f_3(u,v)
\end{array}\right]
$$

The gradient matrix of $$f$$ will be understood as :

$$
grad(f)(u,v) =
\left[\begin{array}{ccc}
\frac{\partial f_1(u,v)}{\partial u} & \frac{\partial f_1(u,v)}{\partial v}\\
\frac{\partial f_2(u,v)}{\partial u} & \frac{\partial f_2(u,v)}{\partial v}\\
\frac{\partial f_3(u,v)}{\partial u} & \frac{\partial f_3(u,v)}{\partial v}
\end{array}\right]
$$

For a scalar valued function, gradient will be a row vector and will be denoted as $$grad(D)$$.<br>
When grad(D) will be used as vector valued function it will become a column vector and will be denoted as $$D'$$.

---


We look at the problem of finding a projection of a point on a parametrized 2d surface. This setting is common in CAD kernels where the parametrized surface is represented using NURBS or Bezier surface.

There are two ways we can thing about it.
1. Finding the point on the surface closest to the given point.
2. Finding the orthogonal projection of the point on the surface.


It can be proved that the closest point will indeed be an orthogonal projection of the given point. This is a necessary condition, but not sufficient.

Let's see how can we device iterative methods for the projection.

<h3> Finding the closest point on the surface</h3>


Let $$\Gamma \subset \Re^{d+1}$$ be a sufficiently regular surface parametrized by

$$
f : \Xi \leftrightarrow \Gamma
$$

with $$\Xi \subset \Re^{d}$$, such that for each $$x \in \Re^{d+1}$$, there exists a unique projection

$$
x_{\Gamma} := arg min_{y \in \Gamma} || y - x ||  \tag{1}
$$

We are looking for $$x_{\Gamma}$$ in terms of its parameters.


Let's assume the norm to be eculidean. The given point whose projection is to be found be $$p$$.

Given we want to minimize the distance, it is equivalent to finding roots of the derivative of the distance function with the condition that determinant of hessian is greater than 0.

We wish to minimize:

$$
D(u) = \sum_{i=1}^{d+1}{(f_i(u) - p_{i})^2} \tag{2}
$$

where $$u \in \Re^{d}$$.

Let's have the first and second derivative of the distance function handy:

$$
\frac{\partial D}{\partial u_j} = \sum_{i=1}^{d+1} 2 *  \frac{\partial f_i(u)}{\partial u_j} * (f_i(u) - p_{i}) \tag{3}
$$

$$
\frac{\partial D}{\partial u_j u_k} = \sum_{i=1}^{d+1} 2 * \frac{\partial f_i(u)}{\partial u_j} * \frac{\partial f_i(u)}{\partial u_k} +   2 * \frac{\partial f_i(u)}{\partial u_j u_k} * (f_i(u) - p_{i}) \tag{4}
$$


We want to write gradiant and hessian of the distance function as matrix function of gradient and hessian of surface function

Matrix form of equation(3) is equivalent to:

$$
grad(D) = 2*(f-p).grad(f) \tag{5}
$$

Matrix form of equation(4) is equivalent to:

$$
hessian(D) = 2*grad(f)^T.grad(f) + 2 * \sum_{i}^{d+1}(f_i(u) - p_i) * hessian(f_i) \tag{6}
$$

or

$$
hessian(D) = 2*\sum_{i}^{d+1}grad(f_i)^T.grad(f_i) + 2 * \sum_{i}^{d+1}(f_i(u) - p_i) * hessian(f_i) \tag{7}
$$

The hessian of the surface function is not readily available in the result of surface evaluation in CAD kernels. We will take a slightly different approach when actually working with kernels in the next section.

To minimize this objective function with respect to $$u$$, we actually find zeros of the gradient of distance function.
Treating gradient as a vector function, the derivative of gradient transposed is same as hessian.

$$
D'(u) =
\left[\begin{array}{ccc}
\frac{\partial{D(u)}}{\partial u_1}\\
\frac{\partial{D(u)}}{\partial u_2}\\
.\\
.\\
.\\
\frac{\partial{D(u)}}{\partial u_d}\\
\end{array}\right]
$$

$$
D''(u) =
\left[\begin{array}{ccc}
\frac{\partial{D(u)}}{\partial u_1 \partial u_1} & \frac{\partial{D(u)}}{\partial u_1 \partial u_2} & \frac{\partial{D(u)}}{\partial u_1 \partial u_3} & . & .\\
\frac{\partial{D(u)}}{\partial u_1 \partial u_2} & \frac{\partial{D(u)}}{\partial u_2 \partial u_2} & \frac{\partial{D(u)}}{\partial u_2 \partial u_3} & . & .\\
\frac{\partial{D(u)}}{\partial u_1 \partial u_3} & \frac{\partial{D(u)}}{\partial u_2 \partial u_3} & \frac{\partial{D(u)}}{\partial u_3 \partial u_3} & . & .\\
. & . & . & . & .\\
. & . & . & . & .\\
. & . & . & . & .\\
\end{array}\right]
$$

which is same as hessian(D)


The newton iteration for this case becomes

$$
u_{k+1} = u_{k} -  (D''(u_k))^{-1}.(D'(u_k)) \tag{8}
$$


<h3> Evaluating D' and D'' in a kernel </h3>

In a kernel, a typical function to evaluate surface and its derivatives looks like this:

```cpp
eval_srf(Surf* p_surf, double uv_point[2], double coord[3], double deriv1[2][3], double deriv2[3][3], double unit_norm[3]) 
```

The ```deriv1``` looks like

$$
\left[\begin{array}{ccc}
\frac{\partial f_1}{\partial u} & \frac{\partial f_2}{\partial u} & \frac{\partial f_3}{\partial u} \\
 \frac{\partial f_1}{\partial v} & \frac{\partial f_2}{\partial v} & \frac{\partial f_3}{\partial v}
 \end{array}\right]
$$

which is transpose of $$grad(f)$$. We can easily compute grad(D) using equation(5) and hence $$D'$$ which is just a transpose.

$$
grad(D) = 2*(f-p).deriv1(u,v)^T \tag{9}
$$

The```deriv2``` looks like

$$
\left[\begin{array}{ccc}
\frac{\partial f_1}{\partial u \partial u} & \frac{\partial f_2}{\partial u \partial u} & \frac{\partial f_3}{\partial u \partial u} \\
\frac{\partial f_1}{\partial u \partial v} & \frac{\partial f_2}{\partial u \partial v} & \frac{\partial f_3}{\partial u \partial v} \\
\frac{\partial f_1}{\partial v \partial v} & \frac{\partial f_2}{\partial v \partial v} & \frac{\partial f_3}{\partial v \partial v} \\
 \end{array}\right]
$$


If we multiply ```deriv2``` by $$(f - p)$$ we get a 3x1 vector:

$$
V = deriv2.(f-p) = 
\left[ \begin{array}{ccc}
\frac{\partial f_1(u,v)}{\partial u \partial u} * (f_1(u,v) - p_{1}) + \frac{\partial f_2(u,v)}{\partial u \partial u} * (f_2(u,v) - p_{2}) + \frac{\partial f_3(u,v)}{\partial u \partial u} * (f_3(u,v) - p_{3})\\
\frac{\partial f_1(u,v)}{\partial u \partial v} * (f_1(u,v) - p_{1}) + \frac{\partial f_2(u,v)}{\partial u \partial u} * (f_2(u,v) - p_{2}) + \frac{\partial f_3(u,v)}{\partial u \partial u} * (f_3(u,v) - p_{3}) \\
\frac{\partial f_1(u,v)}{\partial v \partial v} * (f_1(u,v) - p_{1}) + \frac{\partial f_2(u,v)}{\partial u \partial u} * (f_2(u,v) - p_{2}) + \frac{\partial f_3(u,v)}{\partial u \partial u} * (f_3(u,v) - p_{3})
\end{array}\right]
$$

Make a matrix which looks like:

$$
H =
\left[ \begin{array}{ccc}
V[0] & V[1] \\
V[1] & V[2]
\end{array}\right]
$$

Using this, the equation (3) can be written as :

$$
D'' = hessian(D) = 2*grad(f)^T.grad(f) + 2 * H \tag{10}
$$


Use equation(9) and (10) and plug it in the newton iteration equation (8).



<h3>Further Comments<\h3>

1. The parameters $$u, v$$ are usually restricted to a finite domain. While iterating, if we reach the boundary of domain for a parameter, we need to stop updating that parameter.
2. We would start with multiple initial guesses. The newton method will coverge at any critical point, not necessarily a minima of the distance function. We need to compare the results from the critical points to conclude our results. 