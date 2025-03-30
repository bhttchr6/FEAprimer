# Primer for the FEA Analyst
### Types of Meshes:
1. Eulerian Mesh: This mesh is based on the spatial coordinates of the nodes. As the body deforms, this mesh remains same. Typically denoted by $$x$$. This extends as the body deforms so that the deformed body is contained within the Eulerian mesh always. 
2. Lagrangian Mesh: This mesh is based on material coordinates (i.e. nodes associated with material points on the body). Typically denoted by $$X$$.
Initially, at $$t=0$$, $$x = X $$.
3. Arbitrary Lagrangian Eulerian Mesh: The nodes can be programmed to move arbitrarily. The nodes on the boundary are moved so that even in the deformed configuration, the nodes remain on the boundary while the interior nodes are moved to minimize distortion.

### Types of PDEs
| Hyperbolic | Elliptical | Parabolic |
|----------|----------|----------|
| The general equation of a hyperbola is: $$\frac{x^2}{a^2} - \frac{y^2}{b^2} = 1$$ | $$\frac{x^2}{a^2} + \frac{y^2}{b^2} = 1 $$  | $$y = ax^2 + bx + c $$  |
| Wave Equation: $$\frac{\partial^2 u}{\partial t^2} - c^2 \nabla^2 u = 0$$ | Laplace Equation: $$\frac{\partial^2 u}{\partial x^2} + \frac{\partial^2 u}{\partial y^2} = 0$$ |  Diffusion Equation : $$\frac{\partial u}{\partial t} = D \nabla^2 u $$|
|   | Poisson's Equation : $$\nabla^2 u = f(x, y)$$ | |
| Generally time-dependent | Generally time-independent | Exhibit steady-state behavior over time |
