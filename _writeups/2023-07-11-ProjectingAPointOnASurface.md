---
title: Projection of point on a surface
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
\nabla(f)(u,v) =
\left[\begin{array}{ccc}
\frac{\partial f_1(u,v)}{\partial u} & \frac{\partial f_1(u,v)}{\partial v}\\
\frac{\partial f_2(u,v)}{\partial u} & \frac{\partial f_2(u,v)}{\partial v}\\
\frac{\partial f_3(u,v)}{\partial u} & \frac{\partial f_3(u,v)}{\partial v}
\end{array}\right]
$$

For a scalar valued function, gradient will be a row vector and will be denoted as $$\nabla(D)$$.<br>
When $$\nabla(D)$$ will be used as vector valued function it will become a column vector and will be denoted as $$D'$$.

---


We look at the problem of finding a projection of a point on a parametrized 2d surface. This setting is common in CAD kernels where the parametrized surface is represented using NURBS or Bezier surface.


Let $$\Gamma \subset \Re^{d+1}$$ be a sufficiently regular surface parametrized by

$$
f : \Xi \leftrightarrow \Gamma
$$

with $$\Xi \subset \Re^{d}$$, such that for each $$x \in \Re^{d+1}$$, there exists a unique projection

$$
x_{\Gamma} := arg min_{y \in \Gamma} || y - x ||  \tag{1}
$$

We are looking for $$x_{\Gamma}$$ in terms of its parameters.


Let's assume the norm to be euclidean. The given point whose projection is to be found be $$p$$.


<h2>Finding the closest point</h2>

Given we want to minimize the distance, it is equivalent to finding roots of the derivative of the distance function with the condition that determinant of Hessian is greater than 0.

We wish to minimize the distance function:

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


We want to write gradient and Hessian of the distance function as matrix function of gradient and Hessian of surface function.

Matrix form of equation(3) is equivalent to:

$$
\nabla(D) = 2*(f-p).\nabla(f) \tag{5}
$$

Matrix form of equation(4) is equivalent to:

$$
\nabla^2(D) = 2*\nabla(f)^T.\nabla(f) + 2 * \sum_{i}^{d+1}(f_i(u) - p_i) * \nabla^2(f_i) \tag{6}
$$

or

$$
\nabla^2(D) = 2*\sum_{i}^{d+1}\nabla(f_i)^T.\nabla(f_i) + 2 * \sum_{i}^{d+1}(f_i(u) - p_i) * \nabla^2(f_i) \tag{7}
$$

The Hessian of the surface function is not readily available in the result of surface evaluation in CAD kernels. We will take a slightly different approach when  working with kernels in the next section.

To minimize this objective function with respect to $$u$$, we find zeros of the gradient of distance function.
Treating gradient as a vector function, the derivative of gradient transposed is same as Hessian.

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

which is same as $$\nabla^2(D)$$.


The Newton iteration for this case becomes

$$
u_{k+1} = u_{k} -  (D''(u_k))^{-1}.(D'(u_k)) \tag{8}
$$


<h3> Evaluating D' and D'' in a kernel </h3>

In a kernel, a typical function to evaluate surface and its derivatives looks like this:

```cpp
void eval_srf(Surf* p_surf, double uv_point[2], double coord[3], double deriv1[2][3], double deriv2[3][3], double unit_norm[3]) 
```

The ```deriv1``` looks like

$$
\left[\begin{array}{ccc}
\frac{\partial f_1}{\partial u} & \frac{\partial f_2}{\partial u} & \frac{\partial f_3}{\partial u} \\
 \frac{\partial f_1}{\partial v} & \frac{\partial f_2}{\partial v} & \frac{\partial f_3}{\partial v}
 \end{array}\right]
$$

which is transpose of $$\nabla(f)$$. We can easily compute $$\nabla(D)$$ using equation(5) and hence $$D'$$ which is just a transpose.

$$
\nabla(D) = 2*(f-p).deriv1(u,v)^T \tag{9}
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
D'' = \nabla^2(D) = 2*\nabla(f)^T.\nabla(f) + 2 * H \tag{10}
$$


Use equation(9) and (10) and plug it in the newton iteration equation (8).



<h3>Further Comments</h3>

1. The parameters $$u, v$$ are usually restricted to a finite domain. While iterating, if we reach the boundary of domain for a parameter, we need to stop updating that parameter.
2. We would start with multiple initial guesses. The newton method will coverge at any critical point, not necessarily a minima of the distance function. We need to compare the results from the critical points to conclude our results.

Pseudocode:

```cpp
void findProjection(Surface* s, double point[3], double guessParameters[2], double finalParameters[2], double projectedPoint[3] ) {

  int MAX_ITERATIONS = 100;
  
  int iter = 0;
  while(iter < MAX_ITERATIONS) {
  
    iter = iter+1;
    s->eval_surf(currentParameters, currentPoint, deriv1, deriv2, unit_norm);

    computeDPrime(point, currentPoint, deriv1, DPrime);

    if(isNormZero(DPrime)) {
      finalParameters = currentParameters;
      break;
    } 
    updateParameters(point,currentPoint, deriv1, deriv2, currentParameters);
  }
}
```

```cpp
void updateParameters(double point[3], double currentPoint[3], double deriv1[2][3], double deriv2[3][3], double currentParameters[2]) {

  computeDPrime(point, currentPoint, deriv1, DPrime);
  computeD2Prime(point, currentPoint, deriv1, deriv2, D2Prime);
  computeInverse(D2Prime, D2PrimeInverse);
  computeDeltaU(DPrime, D2PrimeInverse, deltaU);
  
  currentParameters = currentParameters - deltaU;  
}
```

```cpp
void computeDeltaU(double DPrime[2], double D2PrimeInverse[2][2], double deltaU[2]) {
  deltaU[0] = D2PrimeInverse[0][0] * DPrime[0] + D2PrimeInverse[0][1]*DPrime[1];
  deltaU[1] = D2PrimeInverse[1][0] * DPrime[0] + D2PrimeInverse[1][1]*DPrime[1];
}
```

```cpp
void computeDPrime(double point[3], double currentPointOnSurface[3], double deriv1[2][3], double DPrime[2]) {
  DPrime[0] = 0.0;
  DPrime[1] = 0.0;
  
  double fsubtractp[3] {currentPointOnSurface[0] - point[0], currentPointOnSurface[1] - point[1], currentPointOnSurface[2] - point[2]};
  for(int i = 0; i < 3; i++) {
    DPrime[0] += 2 * fsubtractp[i] * deriv1[0][i];
    DPrime[1] += 2 * fsubtractp[i] * deriv1[1][i];
  }
}
```

```cpp
void computeD2Prime(double point[3], double currentPointOnSurface[3], double deriv1[2][3], double deriv2[3][3], double D2Prime[2][2]) {  
  
  for(int i = 0; i < 3; i++) {
    D2Prime[0][0] += 2*deriv1[0][i]*deriv1[0][i];
    D2Prime[0][1] += 2*deriv1[0][i]*deriv1[1][i];
    D2Prime[1][0] += 2*deriv1[1][i]*deriv1[0][i];
    D2Prime[1][1] += 2*deriv1[1][i]*deriv1[1][i];
  }  

  double fsubtractp[3] {currentPointOnSurface[0] - point[0], currentPointOnSurface[1] - point[1], currentPointOnSurface[2] - point[2]};
  double V[3] {0.0, 0.0, 0.0};
  for(int i = 0; i < 3; i++) {
    V[0] += deriv2[0][i]*fsubtractp[i];
    V[1] += deriv2[1][i]*fsubtractp[i];
    V[2] += deriv2[2][i]*fsubtractp[i];
  }

  D2Prime[0][0] += 2*V[0];
  D2Prime[0][1] += 2*V[1];
  D2Prime[1][0] += 2*V[1];
  D2Prime[1][1] += 2*V[2];
  
}
```


<h2>Alternate Approach : Finding the orthogonal projection</h2>

There is another way to think about this problem. It can be proved that the at the nearest point, the vector from point to projection is normal to the tangent plane at the projection. We can thus attempt to find the normal projection of the given point.

This approach takes very similar iteration pattern as Newton's method. In vanilla Newton's method, the desired value of function is always 0 and we make a linear approximation to update the parameter in each step.

<b>Idea</b>

We slightly modify this approach. We are looking for parameter values where the projection of a given point is orthogonal. Given that we don't know the value of the function at such a point, we need to find a different target. Starting with an intial guess on the surface, we make a linear approximation at the guess point. We project our given point on the tangent surface and use that point as a target value to update the parameter values. We repeat this process updating the target in each iteration (as opposed to constant 0 in the vanilla Newton's method) until we reach convergence to a orthogonal point.

<b>Details</b>

Let $$p$$ be the given point. 

At any guess $$ f(u_1, u_2, ... u_d) \rightarrow q \in \Re^{d+1}$$ the tangent plane contains the vectors:

$$
(\frac{\partial f_1(u)}{\partial u_k}, \frac{\partial f_2(u)}{\partial u_k}, . . ,\frac{\partial f_{d+1}(u)}{\partial u_k})
$$

for all $$k$$.


Or if we know the normal vector $$\vec{n}_q$$ at $$q$$ the plane is defined by the point $$q$$ and normal vector. We can easily compute the projection of a point on this plane.

Let the projection of point $$p$$ on this tangent plane be $$r$$. Using linear approximation we have :

$$
r = q + \nabla(f) \Delta(u_k)
$$

From the above equation we can attempt to find $$\Delta(u_k)$$. However $$\nabla(f)$$ is not a square matrix, hence the regular inverse doesn't exist. We will have to resort to generalized inverse. We shall deal with it another day.






