# Primer for the FEA Analyst
### Types of Meshes:
1. **Eulerian Mesh:** This mesh is based on the spatial coordinates of the nodes. As the body deforms, this mesh remains same. Typically denoted by $$x$$. This extends as the body deforms so that the deformed body is contained within the Eulerian mesh always. 
2. **Lagrangian Mesh:** This mesh is based on material coordinates (i.e. nodes associated with material points on the body). Typically denoted by $$X$$.
Initially, at $$t=0$$, $$x = X $$.
3. **Arbitrary Lagrangian Eulerian Mesh:** The nodes can be programmed to move arbitrarily. The nodes on the boundary are moved so that even in the deformed configuration, the nodes remain on the boundary while the interior nodes are moved to minimize distortion.

### Types of PDEs
| Hyperbolic | Elliptical | Parabolic |
|----------|----------|----------|
| The general equation of a hyperbola is: $$\frac{x^2}{a^2} - \frac{y^2}{b^2} = 1$$ | $$\frac{x^2}{a^2} + \frac{y^2}{b^2} = 1 $$  | $$y = ax^2 + bx + c $$  |
| Wave Equation: $$\frac{\partial^2 u}{\partial t^2} - c^2 \nabla^2 u = 0$$ | Laplace Equation: $$\frac{\partial^2 u}{\partial x^2} + \frac{\partial^2 u}{\partial y^2} = 0$$  Elasticity equation: $$\nabla^2 \sigma  + b = 0$$ |  Diffusion Equation : $$\frac{\partial u}{\partial t} = D \nabla^2 u $$|
|   | Poisson's Equation : $$\nabla^2 u = f(x, y)$$ | |
| Generally time-dependent | Generally time-independent | Exhibit steady-state behavior over time |

## It's Elementary!
1. Low order elements when applied to incompressible materials (i.e. isochoric material with Jacobian $$J = 1$$) tend to lock volumetrically i.e. the displacements are underpredicted.
2. **Patch Tests:** They are used for to check if the element is Convergent, Avoids volumetric locking and Stable.
3. The 4-node isoparametric quadilaterial element (Q4) converges for compressible material but for incompressible or nearly incompresible element it shows volumteric locking.

### Simplex Elements
3 node triangular element in 2D and 4 node tetrahedral element in 3D are called simplex elements.

**What is simplex you ask?**
A simplex is a set of $$n + 1$$ points in $$n$$ dimensions.

Simplex elements exhibit volumetric locking for incompressible materials. 

4 node quadilaterial element and 8 node hexahedral element is more accurate than simplex elements, however they exhibit volumetric locking for full integration (i.e. 2 $$\times$$ 2 in 2D or 2 $$\times$$ 2 $$\times$$ 2 in 3D). 

Volumetric locking can be avoided for these elements by using:
1. reduced integration (i.e. one-point quadrature)  
   * One-point quadrature may suffer from pressure oscillations (called checkerboardind) and other instabilities(e.g. hourglass).  
   * One-point quadrature with hourglass control is very fast.  
3. selective-reduced integration i.e. one-point quadrature on volumetric terms and 2 $$\times$$ 2 quadrature on deviatoric terms.

## Verification and Validation
### Verification
Verification is the process of determining whether the FEA framework (i.e. mathematical implementation) implemented is correct or not. 






