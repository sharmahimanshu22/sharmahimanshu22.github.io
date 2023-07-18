---
title: "Understanding Curvature"
layout: post
---

As of now this article proceeds in a very non-intuitive fashion. Shall make it better later.

Given a 2D manifold parametrized by $$\Phi$$ the dot product of two elements in the tangent space.

Note that $$\Phi_u$$ is a 3d vector in the 2d tangent space of the manifold represented by $$\Phi : M \rightarrow N$$, $$M \subset \Re^2, N \subset \Re^3$$. The partial derivative

$$
\Phi_u = d\Phi (\frac{\partial}{\partial u})\\
\Phi_v = d\Phi (\frac{\partial}{\partial v})
$$

where $$\frac{\partial}{\partial u}$$ and $$\frac{\partial}{\partial v}$$ are treated as basis of tangent space in the domain of $$\Phi$$

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

is called the first fundamental matrix. Not that it is dependent on $$\Phi$$ which in our case is the parametrization function of our manifold.