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
| Wave Equation: $$\frac{\partial^2 u}{\partial t^2} - c^2 \nabla^2 u = 0$$ | Laplace Equation: $$\frac{\partial^2 u}{\partial x^2} + \frac{\partial^2 u}{\partial y^2} = 0$$  Elasticity equation: $$\nabla \sigma  + b = 0$$ |  Diffusion Equation : $$\frac{\partial u}{\partial t} = D \nabla^2 u $$|
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

## Verification and Validation (wip)
### Verification
Verification is the process of determining whether the FEA framework (i.e. mathematical implementation) implemented is correct or not. 

## Weak Forms and Strong Forms
The strong form for elliptic elasticity PDE at steady-state is :  
$$\nabla \sigma + b =0$$  
This statement means that the **divergence** of $$\sigma$$ is equal to $$-b$$ at equilibrium. This refers to the fact that for the body to be in equilibrium the sum of forces has to be equal to zero i.e. force/stress moving into one end must come out of the other end.

**NO FORCE IS CREATED INSIDE THE BODY!**
## Strong form
If we take a PDE, then its solution will be of form $$u$$, which may be an $$n$$-dimensional vector. This vector exists in some $$n$$-dimensional space called Sobelov space.

We approximate this solution $$u$$ using a solution $$u_h$$. We do this using a polynomial $$p$$. Generally, an error exists between this approximate solution $$u_h$$ and the ideal solution $$u$$ because:
* The domain may be complex and is discretized using element of size $$h$$. Due to this a curve is no longer a curve but a collection of discrete segments. This casues a loss of data/information.
* The size of the polynomial and the type of polynomial may not be enough to capture the complex behavior of domains.
* The choice of the polynomial may be influenced not only by what can represent the physics and capture the deformation well, but also computational factors.

**Due to these, the approximate solution $$u_h$$ may not satisfy the PDE exactly**

We ideally want to solve the PDE ($$\nabla \sigma + b =0$$) to get $$u$$. However, as mentioned before, the best we can do is to get an approximate solution $$u_h$$ that satisfies the PDE *weakly* i.e. Strong form cannot be satisfied for all the points of the domain. We want something that can be satisfied in an average sense.This is where weak form comes into picture  

$$\int_{\Omega} (\nabla \sigma + b) f_t d\Omega = 0$$ to get $$u_h$$.

The error is then defined as :

$$||u-u_h||$$

## Normalise Norms!
The error is distance between two vectors $$u$$ and $$u_h$$. But these two vectors may have different dimensions, so in which space do we take the norm. Mathematically we take the norm in *Hilbert space*. The norms are defined as:

$$||u -u_h||_{H_m}$$
 
As a rule of thumb, $$m=0$$ if we want to measure the norm of $$C^0$$ parameters e.g. displacement. The value of $$m=1$$ for $$C^1$$ parameters i.e. strain.
### Quantifying error

The error can be quantified:
$$||u-u_h|| <= Ch^\alpha{u}_{Hr}$$
where
$$\alpha = min(k+1 -m, r-m)$$

Let us see what these complicated terms mean. The elements like 3 node triangular element, 4 node quad element, 4 node tetrahedral element and 8 node hexahedral element can completely represent a polynomial function of order 1 i.e. $$k=1$$. That means the value of alpha for these elements would be:
$$\alpha = min(2 - 0, r-0)$$

The value of $$r$$ for smooth solutions i.e. no discontinuites or cracks is $$\infty$$. Then the value of $$\alpha$$ for smooth solution fields is 2. The rate of convergence of these elements for $$H_0$$ or $$L_2$$ norm is 2. The rate of convergence in $$H_1$$ i.e. for  strain, is then 1. 

# Explicit vs Implicit Analysis
## Explicit Analysis

Explicit analysis is used to simulate events that last over a very short period of time e.g. car crash, drop test, impact, ballistics etc. Explicit analysis has the following charateristics:
* Explicit analysis does not check for convergance in each time step
* Explicit analysis does not solve for equilibrium at each step.
* Explicit analysis does not perform matrix inversion.
* Explicit analysis is conditionally stable. The stability condition is called Courant Freidrichs Lewy (CFL) condition.

### CFL condition
CFL condition states that the explicit analysis will be stable if:  
  
$$\Delta t <= t_{crit}$$

The term $$t_{crit}$$ is defined as :

$$t_{crit} = f \frac{L}{c}$$

where, the parameters $$f$$ is a scaling factor with value less than or equal to 1. The term $$L$$ is the charateristic length of the smallest element in the mesh and $$c$$ is the speed of stress wave. The stress wave propogating inside a medium can be compression waves or shear waves. It has been found that the compression waves travel faster than shear waves and its speed is equal to the speed of the sound. Hence $$c$$ is usually taken as the speed of sound inside the medium. 

**Characteristic Length**

Characteristic length of an element is defined as the shown below. 

![Alt Text](https://github.com/bhttchr6/FEAprimer/blob/main/CharacteristicLength.png)

### Hourglass, Shear Locking and Volumetric Locking
Hourglass: There are two ways of integrating using gauss-quadrature formula. Let us say that we have a 2D QUAD element. We can use the full $$2\times2$$ quadrature or we can use the reduced integration method that basically uses only a single integration point at the center of the element. If we use the full $$2\times2$$ quadrature formula then the ramk of the stiffness matrix is: Total number of degrees of freedom - the number of rigid body motions i.e. 8 - 3 (2 translations and 1 rotation) = 5. If instead we use only 1 integration point then we not only have the 3 rigid body motions (i.e. the motions that do not produce any strain energy) but also 2 other types of motions that does not produce any strain energy so the rank decreases to 3. Thus under reduced integration the stiffness matrix becomes rank deficient. These zero-energy modes are called hourglass modes. 
Hourglass can be prevented by using second-order reduced integration elements.


Hourglass-stiffness:

Volumetric Locking: If we use full integration for say hexahedral elements then the displacements computed maybe less than what it should be i.e. elements may show stiffning behavior. This is because the sides of the elements have to remain 90 degrees. 

### Bonded layers 
[Bonded layers and stress analysis](https://wp.optics.arizona.edu/optomech/wp-content/uploads/sites/53/2016/10/chen-1979.pdf)

# Columns and Beams (Teaching PPT)
[Theory of beams and columns](https://github.com/bhttchr6/FEAprimer/blob/main/Teaching_PPT.pdf)

Question: What are simplex elements?

Answer: For 2D, the dim = 2 then simplex element in 2D is collection of dim + 1 = 3 points i.e. a triangle. For 3D, simplex element has dim+1 nodes, so 4 nodes i.e a tetrahedral element. 

Q> What are issues with simplex elements?

Answer> For plane strain problem and for incompressible and nearly incompressible materials, simplex elements tend to display volumetric locking. Simplex elements also exhibit shear locking ( or stiff behavior in bending).

Q> What is difference between volumetric locking and shear locking?

answer> Volumetric locking does not converge but stiff behavior(shear locking) converges but the accuracy is very low for coarse meshes. Thus even though stiff behavior is not as bad a volumetric locking, it hampers accurate simulation on coarse meshes.

Q> Why use 4 node Quad element or 8 node hexahedral element when simplex elements exist?

answer> 4 node QUAD and 8 node hexahedral elements provide better accuracy over simplex elements.

Q> What are the disadvantages associated with QUAD and HEX elements?

answer> For full integration i.e. $$2\times2$$ for 2D and $$2\times2\times2$$ on 3D, these elements exhibit volumetric locking for incompressible or nearly0incompressible materials. The also show shear locking (stiff behavior) in bending.

Q> How can you avoid these disadvantages?

answer> We can use uniorm reduced integration or selective reduced integration to avoid these behavior.

Q> what is the difference between uniformm reduced integration and selective reduced integration (SRI)?

answer> Uniform reduced integration using one-pointt quadrature for integration of both volumetric ad deviatoric terms while SRI uses one-point on volumetric terms and full integration on deviatoric terms.

Q> Then why not use it everywhere?

answer> They suffer from the problem of pressure-fluctuation (checkerboaring) and spatial instability in the form of hourglass, spurious modes, zero-energy modes, chickewiring etc. 

Q> How to avoid these instabilities?

answer> Hourglass stabilization with reduced integration can work for these elements.

Q> Why do we use higher order elements?

answer> Higher order elements for linear elastic problems can provide higher accuracy as compared to low-order elements.

Q> Then why not use higher order elements for nonlinear problems as well?

answer> Higher order elements are not well suited for dynamics and large-deformation with Lagrangian meshes. 
* It is difficult to generate good diagonal mass matrices for these elements.
* Wave solutions for hyperbolic equations tend to be noisy for higher-order elements due to the presence of optical modes.
* For large-deformation these elements fail more often and their accuracy deteriorates more rapidly than low-order elements because the Jacobian can become negative easily at quadrature points.

Q> For elastic-plastic materials does higher-order elements work better than lower-order elements?

answer> Elastic-plastic material model is continuous in displacement but is not differentiable everywhere because of the existance of a sharp kink in the stress-strain plot. Thus the strain is $$C^0$$. If strain is $$C^0$$ then displacement is $$C^1$$.  

Q> Is the wavespeed in elastic-plastic material higher or lower than elastic material?

answer> wavespeed is proportional to stiffness and inversely proportional to density of the material. Since for the same density of material, a plastic material will have lower stiffness as compared to elastic material, hence the wavespeed is lower for plastic mateial.

Q> If the wavespeed for elasto-plastic material is lower than elastic material and since the critical time step is inversely proportional to wavespeed does that mean we can use higher time step size for plastic material?

answer> No. This is because a elasto-plastic material can unload at any moment, sometimes due to numerical noise. During the unloading step, the path followed is elastic which has higher wavespeed and hence if timestep is higher than the critical time step is, the solution will blow up. 

Q> what is mass scaling?

answer> The mass of very small elements (i.e. the elements that control the time-step size) is increased to increase the overall step size.

Q> is mass scaling the only approach?

answer> no subcycling is also available. In sub-cycling the time-step size for only the stiff elements/small elements are reduced.

Q> Can mass scaling be used for every situation?

answer> No, mass scaling can only be used when high-frequency effects are not important. e.g. sheet-metal forming. Sheet-metal forming process is essentially a static process. 






