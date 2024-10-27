---
title: "First and Second Fundamental Form"
layout: post
---


Let a 2D manifold be parametrized by $$\Phi$$

$$
\Phi : M \rightarrow N, M \subset \Re^2, N \subset \Re^3
$$


$$\Phi_u$$ and $$\Phi_v$$ are 3d vectors in the 2d tangent space of the manifold:

$$
\Phi_u = d\Phi (\frac{\partial}{\partial u})\\
\Phi_v = d\Phi (\frac{\partial}{\partial v})
$$

where $$d\Phi$$ is the map from tangent space in $$M$$ to tangent space in $$N$$ and  $$\frac{\partial}{\partial u}$$ and $$\frac{\partial}{\partial v}$$ are interpreted as basis of tangent space in the domain $$M$$.

Let $$\gamma(t)$$ be a curve passing through a point $$p$$ on the surface:

$$
\gamma(t) = \Phi(u(t), v(t))
$$

### First Fundamental Form

The inner product of two vectors in the tangent space at a point in $$N$$ given by:

$$ \begin{align}
I(a\Phi_u + b\Phi_v, c\Phi_u + d\Phi_v) = ac\langle\Phi_u, \Phi_u\rangle + (ad+bc)\langle\Phi_u,\Phi_v\rangle + bd\langle\Phi_v,\Phi_v\rangle  \tag{1} 
\end{align}
$$

Denote
$$
E = \langle\Phi_u, \Phi_u\rangle,
F = \langle\Phi_u,\Phi_v\rangle,
G = \langle\Phi_v,\Phi_v\rangle
$$

The matrix

$$
H =
\left[ \begin{array}{ccc}
E & F \\
F & G
\end{array}\right]
$$

is called the first fundamental matrix. It is dependent on $$\Phi$$ which is the parametrization function of our manifold and the point at which we are computing it (which is same as saying it depens on current value of $$u,v$$ parameter or $$t$$ parameter in turn if we are dealing with a curve). However, it can be proved that the inner product given by $$(1)$$ is independent of the local coordinates chosen even though E,F, and G depend on it.



The norm for $$\gamma'(t)$$ will be defined as :

$$
||\gamma'(t)|| = \sqrt{I(\gamma'(t), \gamma'(t))}
$$

The length of a curve on the manifold is given by:

$$
L(\gamma) = \int_{a}^{b} ||\gamma'(t)|| \; dt

$$




First fundametal form is also same as a Riemannian metric induced from the embedding space $$\Re3$$. I believe that there are examples of Riemannian metric which are not induced from $$\Re3$$. And in that sense, first fundamental form need not be induced always from $$\Re3$$ but can be defined for any Riemannian metric.


### Second Fundamental Form

Define **surface normal** at point $$p = \Phi(u,v)$$ as:

$$
\begin{align}
\mathbf{N} = \Phi_u \times \Phi_v
\end{align}
$$

Since $$\mathbf{t} = \frac{\partial \gamma}{\partial s}$$ lies in the tangent plane at $$p$$, it is orthogonal to $$\mathbf{N}$$.

Taking the derivative of the tangent vector : 

$$
\frac{d\mathbf{t}}{ds} = \kappa \mathbf{n}
$$

where $$\mathbf{n}$$ is the normal curvatur vector to the curve.

Then

$$
\kappa \mathbf{n} = \kappa_n \mathbf{N} + \kappa_g \mathbf{N}_g
$$

Think of $$\mathbf{N}_g$$ as the component of $$\kappa \mathbf{n}$$ which is not in the direction $$\mathbf{N}$$. It is actually geodesic curvature vector.


As $$\mathbf{N}$$ and $$\mathbf{t}$$ are orthogonal,

$$
\mathbf{N}.\mathbf{t} = 0
$$

taking a derivative of above expression in arc-length parametrization:

$$

\frac{d\mathbf{t}}{ds}.\mathbf{N} + \mathbf{t}.\frac{d\mathbf{N}}{ds} = 0

$$

thus

$$
\begin{align*}
\kappa_n &= - \mathbf{t}.\frac{d\mathbf{N}}{ds} \\
 &= -\frac{d\gamma}{ds}.\frac{d\mathbf{N}}{ds} \\
 &= -\frac{\frac{d\gamma}{dt}}{\frac{ds}{dt}} . \frac{\frac{d\mathbf{N}}{dt}}{\frac{ds}{dt}} \\
 &= -\frac{( \Phi_{u}\dot{u} + \Phi_{v}\dot{v} ). (\mathbf{N}_u\dot{u} + \mathbf{N}_v\dot{v})} {|\frac{d\gamma}{dt}|^2} \\
 &= -\frac{( \Phi_{u}\dot{u} + \Phi_{v}\dot{v} ). (\mathbf{N}_u\dot{u} + \mathbf{N}_v\dot{v})}	{||\gamma'(t)||^2}
\end{align*}
$$

where last two equalities follow by shifting from arc-length parametrization back to t-parametrization.

The numerator here is what is called Second Fundamental form.

$$
\mathbf L = -\Phi_u.\mathbf{N}_u   \\
\mathbf 2M = - (\Phi_u.\mathbf{N}_v + \Phi_v.\mathbf{N}_u) \\
\mathbf N = -\Phi_v.\mathbf{N}_v
$$

We can also make use of the face that $$\Phi_u.\mathbf{N} = 0$$ and $$\Phi_v.\mathbf{N} = 0$$. Thus

$$
\mathbf L = \Phi_{uu}.\mathbf{N},   \\
\mathbf 2M = \Phi_{uv}.\mathbf{N}  \\
\mathbf N = \Phi_{vv}.\mathbf{N},

$$

Note that the definition of $$\mathbf{L}, \mathbf{M}$$ and $$\mathbf{N}$$ is depenedent only on $$\Phi$$ and not on $$\gamma$$