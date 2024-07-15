---
title: Robust Predicates
layout: post
---



### Computational Geometry requires robust predicates


A.

As is well known, computational geometry algorithms require that we are able to compute predicates in a 'robust' manner. Whether the 3d point lies above or below a plane should be computed either correctly without any floating point errors or at least 'consistently' in the sense that the result does not mess up the larger geometry. For example if we have three collinear points a, b, and c in that order, and we compute orientation of a new point 'd' with respect to this line, the answer should be same irrespective we use 'ab' , 'bc', or 'ac' to do the computation.

1. Arithmetic Filter [^1]
2. Adaptive Precision Floating-Point Arithmetic and Fast Robust Geometric Predicates [^2]
3. Interval Arithmetic [^3]
4. Multi Precision Numerics[^4]

are the popular techniques used in this field.

B.

A second nuance in Computational Geometry Algorithm is dealing with degenerate cases. It can quickly become daunting to deal with each special case in an algorithm. A technique - Simulation of Simplicity - introduced in [^5] lays out a method to deal with this probelm.

C.

Another nuance in geometric algorithms involve the intermediate geometric objects generated through the algorithm. Suppose an algorithm reqiures a new point 'p' to be computed and stored as an intermediate value. This intermediate value will be further used in other predicates. A question arises of how these intermediate values should be stored and what do we need to take care of using these values.




## References

[^1]: Meyer, Pion, 2008. FPG: A code generator. In: Real Numbers and Computers. pp. 47–60. https://inria.hal.science/hal-01225202v1/document
[^2]: Shewchuk, 1997. Adaptive precision floating-point arithmetic. Discrete & Computational Geometry https://people.eecs.berkeley.edu/~jrs/papers/robustr.pdf
[^3]: Hervé Brönnimann, Christoph Burnikel, Sylvain Pion. Interval Arithmetic Yields Eﬀicient Dynamic Filters for Computational Geometry. Discrete Applied Mathematics, 2001, 109, pp.25-47. inria- 00344281 https://inria.hal.science/inria-00344281/document
[^4]: MPFR, boost multiprecision libraries
[^5]: Herbert Edelsbrunner and Ernst Peter Mücke. 1990. Simulation of simplicity: a technique to cope with degenerate cases in geometric algorithms. ACM Trans. Graph. 9, 1 (Jan. 1990), 66–104. https://doi.org/10.1145/77635.77639

