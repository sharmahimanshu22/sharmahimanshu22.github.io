---
title: "First Fundamental Form"
layout: post
---


Let a 2D manifold be parametrized by $$\Phi$$

$$
\Phi : M \rightarrow N, M \subset \Re^2, N \subset \Re^3
$$


$$\Phi_u$$ and $$\Phi_v$$ are 3d vectors in the 2d tangent space of the manifold. The definition for the partial derivative is :

$$
\Phi_u = d\Phi (\frac{\partial}{\partial u})\\
\Phi_v = d\Phi (\frac{\partial}{\partial v})
$$

where $$d\Phi$$ is the map from tangent space in $$M$$ to tangent space in $$N$$ and  $$\frac{\partial}{\partial u}$$ and $$\frac{\partial}{\partial v}$$ are interpreted as basis of tangent space in the domain $$M$$.

The dot product of two vectors in the tangent space at a point in $$N$$ is given by:

$$ \begin{align}
I(a\Phi_u + b\Phi_v, c\Phi_u + d\Phi_v) = ac\langle\Phi_u, \Phi_u\rangle + (ad+bc)\langle\Phi_u,\Phi_v\rangle + bd\langle\Phi_v,\Phi_v\rangle
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

is called the first fundamental matrix. Note that it is dependent on $$\Phi$$ which is the parametrization function of our manifold and the point at which we are computing it.