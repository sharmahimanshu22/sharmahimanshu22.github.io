---
title: "Comments on Poisson Surface Reconstruction and Adaptive Multigrid Finite Element Method"
layout: post
---


We discuss some aspects of the paper : Poisson Surface Reconstruction by Michael Kazhdan, Matthew Bolitho and Hugues Hoppe [^1]



The paper lays out the scheme for surface reconstruction from point cloud data (normal at each point is also given. I believe point cloud with normals is called Hermitian Data).

The paper lays out a method using "implicit function framework". Here the problem of surface reconstruction is formulated as solving a differential equation and obtaining a appropriate iso-surface of the solution.

The authors formulate the problem as a Poission equation:

$$
\nabla.\nabla \widetilde{\chi}  = \nabla \vec{V}
$$

where $$\chi$$ is the smoothed indicator function for inside of a mesh and $$\vec{V}$$ is the vector field obtained by 'interpolating' the normals from the hermitian data. The smoothing of indicator function and interpolation of normals is done by using a smoothing function. We would like this smoothing function to have a kind of gaussian shape around the surface/point cloud. Thus they build an adaptive oct-tree using the point cloud and define a local b-spline $$F_o$$ for each of the node of oct tree. Using this framework the pde on the oct tree $$O$$ (of size $$\vert O \vert$$) is :

$$
\min_{x\in\Re^{\vert O \vert}} ||Lx-v||^2
$$

where:

$$
L_{oo'} = \langle \frac{\partial^2 F_o}{\partial x^2}, F_{o'} \rangle + \langle \frac{\partial^2 F_o}{\partial y^2}, F_{o'} \rangle + \langle \frac{\partial^2 F_o}{\partial z^2}, F_{o'} \rangle   \\

v = \langle \nabla.\vec{V}, F_o \rangle
$$

There is an extensive theory about b-spline functions which is used to compute these inner products.

This oct-tree is now used as a layered mesh with layers closer to root being coarse mesh and layers at the leaf being the fine mesh. In the language of numerical partial differential equations this is called a multi-grid.

The paper just mentions that they use Gauss-Siedel method to solve the laplace equation. Future papers which extend this work by same authors [^2] [^3] give out more details about this technique. In this approach they numerically solve the pde at each level of the oct tree moving from coarser to finer levels.
In any introductory tutorial to multigrid methods, the method starts by solving equation on finer grid and then proceeds to coarser grid. The one outlined in [^3] seems to be a modification of the the standard multigrid technique.





### References
[^1]: Poisson Surface Reconstruction https://hhoppe.com/poissonrecon.pdf

[^2]: Screened Poisson Surface Reconstruction https://www.cs.jhu.edu/~misha/MyPapers/ToG13.pdf

[^3]: An Adaptive Multigrid Solver for Applications in Computer Graphics: https://hhoppe.com/adaptivemultigrid.pdf



