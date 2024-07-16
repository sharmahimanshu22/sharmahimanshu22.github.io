---
title: Discussion on Shewchuk's Predicates - Arbitrary Precision Floating Point Arithmetic
layout: post
---



We will try to  discuss some of the nuances from Shewchuk's paper [^1].



### Definitions

#### Non-Overlapping:

Two floating point valuex x and y are non-overlapping if the least significant nonzero bit of x is more significant than the most significant nonzero bit of y, or vice versa.
Think of this definition when numbers are written down in a binary format and not floating point representation.
In a floating point representation, the significand might be overlapping and still the numbers might be non-overlapping if there exponents are different.



#### Adjacent:

Two floating point x and y are adjacent if x is overlapping with 2y or y is overlapping with 2x. Thus if two numbers are non adjacent, either y >= 2x+1 or x >=  2y+1.


An expansion is non-overlapping/non-adjacent if no two terms are overlapping/adjacent. Usuall they are listed in increasing order.
It is very easy to represent any floating point number in non-overlapping or non-adjacent expansion by using suitable exponent and sign bit.



#### Strongly Non Overlapping Expansion:

No two components are overlapping.
No component is adjacent to 'two' other components
A pair of adjacent components have the property that both components can be expressed with a one-bit significand.


### Comments

(Note: We will refer to some functions here by the same name as they are in the paper. For their definition, look at the paper)


1. Both TWO-SUM and FAST-TWO-SUM return a non-overlapping expansions. FAST-TWO-SUM also returns a non-adjacent expansion if the machine uses round-to-even tiebraking.

2. EXPANSION-SUM preserves Non-Overlapping and Non-Adjacent properties. FAST-EXPANSION-SUM preserves Strongly-Non-Overlapping property - with round-to-even tiebraking. This algo requires strongly-non-overlapping input. It is not useful if inputs are not strongly-non-overlapping.Some thing that the author laments about in FAST-EXPANSION-SUM:
     1. It requires STRONGLY-NON-OVERLAPPING inputs, which is somewhat a complicated definition.
     2. We explicitly need to assume round-to-even tiebreaking condition for the proof. Author asserts that the algorithm works "	just as well" on a machine that uses the round-towards-zero rule, and will even preserve non-overlapping property. However
     this is not pursued because IEEE754 requires round-to-even tie breaking.


3. SPLIT method splits a floating point number into two non-overlapping values.
TWO-PRODUCT takes two p-bit floating point numbers and produce a non-overlapping expansion. If round-to-even tie breaking is used, the expansion produced is non-adjacent.

4. SCALE-EXPANSION takes a non-overlapping expansion and produces a scaled expansion which is also non-overlapping. If input is non-adjacent and round-to-even is used the output is also non-adjacent. 
If input is strongly-non-overlapping and round-to-even tiebreaking is used, output is also strongly-non-overlapping.




Author uses these algorithms to create exact and adaptive versions of these predicates.
From my experience with some github repositories, the adaptive versions from this paper have been replaced by FPG [^2][^3] and only the exact version of prediactes from Shewchuk's paper is used.



In the paper [^3] the error propagation rules used are:

<img src="/images/common/errorpropagation1.png" alt="voronoi" style="border: 2px solid  gray;">

Here, even in multiplication, all error terms are linear in 'ulp/2' ($$\epsilon$$), even for the term associated with x*y.

While in Shewchuk's paper the author takes care of higher order error terms and associates $$\epsilon^2$$ with the x*y term.

Thus FPG generates a much simpler 'single step' filter with associated error bound.

Predicate Construction Kit by Bruno Levy uses FPG by Sylvain Pion followed by exact predicates from Shewchuk's paper.

## References

[^1]: Shewchuk, 1997. Adaptive precision floating-point arithmetic. Discrete & Computational Geometry https://people.eecs.berkeley.edu/~jrs/papers/robustr.pdf

[^2]: Andreas Meyer, Sylvain Pion. FPG: A code generator for fast and certified geometric predicates. Real Numbers and Computers, Jun 2008, Santiago de Compostela, Spain. pp.47-60. inria-00344297

[^3]: Olivier Devillers, Sylvain Pion. Eﬀicient Exact Geometric Predicates for Delaunay Triangulations. RR-4351, INRIA. 2002. inria-00072237

[^4]: Bruno Lévy. Robustness and Eﬀiciency of Geometric Programs The Predicate Construction Kit (PCK). Computer-Aided Design, 2015. hal-01225202