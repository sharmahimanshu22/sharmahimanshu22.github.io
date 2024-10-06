---
title: "Simulation of Simplicity"
layout: post
---


Simulation of Simplicity[^1] is one of cornerstones of predicate management in computational geometry. The other two being Shewchuk's expansions and Arithmetic Filters by Andreas Meyer and Sylvain Pion which basically use floating point error propagation to identify 'tight' cases.

The paper starts with the intuition that if our data set has $$n$$ points of $$d$$ dimensions and the predicate takes $$k$$ points as input, we can think of a predicate as a function $$P$$ from a subspace $$\Re^{kd}$$ to $$\{-1,0,1\}$$.

$$
P : \Re^{kd} \rightarrow \{-1,0,1\}
$$

Viewing at the predicate in this space, if we start with specific $$k$$ indices of $$n$$ indices, the set $$f^{-1}(0)$$ is a set of measure $$0$$ in the kd-dimensional space. (The set $$f^{-1}(0)$$ will be union of disjoint manifolds of different dimensions, all less than $$kd$$). We can extend this set in orthogonal direction and obtain zero measure sets in $$nd$$ dimensions. We can do this with other k size subset of $$\{1, ..., n\}$$. The union of all such cases will still be a 0 measure set. Since this set has measure 0, at every point there exists a arbitrary small open ball that contains non-degenerate configuration. This proves that there exists a perturbation to the $$n$$ points that can make the configuration non-degenerate. Exactly what we want.



For a determinant based predicate, they introduce a specific formula for perturbation, where coordinate $$j$$ of point $$i$$ is perturbed by:

$$
\epsilon(i,j) = \epsilon ^ {2 ^ {i.\delta - j}}
$$

In this case the determinant becomes:

$$

\left[ \begin{array}{ccc}
p_{i_{0}1} & p_{i_{0}2} & ... & p_{i_{0}d} & 1 \\
p_{i_{1}1} & p_{i_{1}2} & ... & p_{i_{1}d} & 1\\
. & . & ... & . & .\\
. & . & ... & . & .\\
p_{i_{d}1} & p_{i_{d}2} & ... & p_{i_{d}d} & 1\\
\end{array}\right]

\rightarrow

\left[ \begin{array}{ccc}
p_{i_{0}1} + \epsilon(i_{0},1) & p_{i_{0}2} + \epsilon(i_{0},2) & ... & p_{i_{0}d} + \epsilon(i_{0},d) & 1\\
p_{i_{1}1} + \epsilon(i_{1},1) & p_{i_{1}2} + \epsilon(i_{1},2) & ... & p_{i_{1}d} + \epsilon(i_{1},d) & 1\\
. & . & ... & . & . \\
. & . & ... & . & . \\
p_{i_{d}1} + \epsilon(i_{d},1) & p_{i_{d}2} + \epsilon(i_{d},2) & ... & p_{i_{d}d} + \epsilon(i_{d},d) & 1\\
\end{array}\right]
$$

As long as the rank of the unperturbed determinant is not 0, we can prove that value of perturbed determinant is not 0. In practice, most implementations will assume that the rank of determinant on left is only one less than full rank as most predicates do not make sense when rank is even less.

The form of the perturbation introduced requires the knowledge of index $$i$$ of the points involved. This is not very pleasant.
However, two pleasant things happen with this form of perturbation:

1. We do not actually need to have an exact value of epsilon. The determinant will be a polynomial in epsilon and we just need to know the coeffiecients.
2. The exact value of $$i_d$$ is not very important. We need to place some order on the d+1 inputs which is globally consistent (ie. with all n points). For example, the geogram[^2] library by Bruno Levy provides options for sorting in lexicographic order or by address. Given the inputs to predicates, it sorts the inputs in one of these orders, records whether it is an 'even' or 'odd' permutation of the input and proceeds with rest of calculation.

Thus when we implement the predicates, we need not input the exact indices of the points we provide as input.

With these two blessings, the only thing that's left to compute is various coefficients which take the form of variuos  sub-determinants of the original matrix and keeping a track of the relative indices. With this we get the determinant of the perturbed points.

What's important to realize is that this whole scheme of things introduces a globally consistent perturbation. For example, imagine five collinear in-order points $$p1, p2, p3, p4, p5$$. If we check the orientation of the point $$p5$$ with respect to the line $$p1p2$$ and $$p3p4$$, we should get the same answer for both the queries, either $$+1$$ or $$-1$$. Given that we don't want to provide indices as input to predicate, the second point above plays a crucial role in ensuring this consistency.







## References

[^1]: Edelsbrunner, H., & Mücke, E. P. (1990). Simulation of simplicity: a technique to cope with degenerate cases in geometric algorithms. ACM Transactions on Graphics (tog), 9(1), 66-104.
[^2]: https://github.com/BrunoLevy/geogram
