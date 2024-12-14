---
Name: Leo Lupo
Topic: Multidimensional Methods for Root Finding
Title: Challenges and Methods in Multidimensional Root Finding
---

# Table of Contents
- [Introduction](#introduction)
- [Hisorical Significance](#historical-significance)
- [Strengths and Challenges of Euler's Method](#strengths-and-challenges)
- [Broyden's Method](#broydens-method)
- [Edge Cases](#edge-cases)
- [Conclusion](#conclusion)
- [Pseudocode](#pseudocode)
- [References](#references)


# Multidimensional Methods for Root Finding

## Introduction

This article explores multidimensional root-finding methods, providing an overview of how a few key approaches work, their unique characteristics, and the factors that make solving these problems more complex than their one-dimensional counterparts.

Many Graphics used in this Project were made in GIFsmos, here is a link to the project files used.

## Historical Significance

During the late 17th century, astronomers faced a pressing challenge: predicting the motion of planets accurately. Johannes Kepler had formulated his famous laws of planetary motion, including the law that planets move in elliptical orbits with the Sun at one focus. However, determining the exact position of a planet at a given time required solving a key equation known as **Kepler's Equation**:

$$
M = E - esin(E)
$$

Where:

- **M** is the mean anomaly (a measure of time within the orbital period).
- **E** is the eccentric anomaly (related to the position of the planet along its orbit).
- **e** is the orbital eccentricity (a measure of how elliptical the orbit is).

Kepler's Equation is transcendental, meaning it involves a mix of algebraic and non-algebraic functions (in this case, trigonometric), making it impossible to solve exactly using standard algebraic methods. This means that iterative numerical techniques are required to approximate the solution for E. This posed a major problem for astronomers who needed precise planetary positions for navigation, calendar design, and understanding celestial mechanics.

![Visualization of Kepler's laws showing an elliptical orbit with the Sun at one focus, along with the geometric relationships for M, E, and e. The Sun is represented as a stationary dot at one focus of the ellipse. The moving planet represents the relationship between its mean anomaly (M), eccentric anomaly (E), and the orbital eccentricity (e).](images/kepler2.gif)

In this visualization, The purple dot represents the planet, the Sun is the big orange one at the focus of the elipse, and the eccentric anomaly (E), is the angle between the semi-marjor axis (the green line) and the line connecting the center of the ellipse to the planet's projection onto the circumscribed circle.

By introducing an iterative approach that could refine approximate solutions, Newton offered a "method" to solving equations like Kepler's. Some may even call it Newton's Method perhaps.

### Newton's Method

Newton's Method is a 1 dimensional root approximating tequenique. That is, it let's you approximate for the value of x where f(x) = 0.

1. Find the tangent line of the function you are trying to approximate at any random point on the function.
2. Simply find the root of that tangent line.
3. Plug it into this function:

$$
x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}
$$

The formula predicts the root $x_{n+1}$ by subtracting the last root predicted, in this case the x-intercept of the tangent line, by the value of the function at that point over the derivative of the function at that point.

4. Plug that new value back into the function and repeat until satisfied.

We can approximate the solution to Kepler's Equation using this method:

Start with the formula from earlier:

$$
M = E - esin(E)
$$

and rearrange it so that we are solving for the root:

$$
0 = E - esin(E) - M
$$

Now, if we make that the function $f(x_n)$ and plug it into Newton's method:

$$
E_{n+1} = E_n - \frac{E_n - e \sin(E_n) - M}{1 - e \cos(E_n)}
$$

#### Let's say:
- Mean anomaly \( M = 0.75 \) radians
- Orbital eccentricity \( e = 0.3 \)
- Initial guess \( E_0 = 0.75 \) radians

#### Iteration Steps:

1. **Iteration 1:**

$$
E_1 = E_0 - \frac{E_0 - e \sin(E_0) - M}{1 - e \cos(E_0)}
$$

   Substitute values:

   $$
   E_1 = 0.75 - \frac{0.75 - 0.3 \sin(0.75) - 0.75}{1 - 0.3 \cos(0.75)}
   $$

   Simplify:

   $$
   E_1 \approx 0.784
   $$

2. **Iteration 2:**

$$
   E_2 = E_1 - \frac{E_1 - e \sin(E_1) - M}{1 - e \cos(E_1)}
   $$

   Substitute values:

   $$
   E_2 = 0.784 - \frac{0.784 - 0.3 \sin(0.784) - 0.75}{1 - 0.3 \cos(0.784)}
   $$

   Simplify:

   $$
   E_2 \approx 0.785
   $$

Below is a graphical representation running Euler's Method on Kepler's Equations applied at different starting points:

![Newton's Method Visualization](images/newton2.gif)

Where the black line is Kepler's Equation, $f(E) = E - esin(E) - M$, the red line is the tangent at the point tested.
The black vertical dotted line represents the actual root of Kepler's function, the green vertical line is E1, the blue is is E2, and the red one if you can see it is E3.

After running Euler's Method 3 on Kepler's Equations 3 times, yeilding E3, it very consistantly is very close to the actual root.

### Enhanced Techniques from Modern Applications

In modern times, more sophisticated algorithms, including a second-order Newton-Raphson approach, have been developed to increase precision and efficiency. For example, the NASA Technical Note elaborates on methods used for satellite orbit determination, where an enhanced version of Newton's method incorporates second-order terms to achieve cubic convergence:

![Equation](https://latex.codecogs.com/gif.latex?E_{n%2B1}%20=%20E_n%20-%20\frac{f(E_n)}{f'(E_n)}%20-%20\frac{1}{2}%20\cdot%20\frac{f''(E_n)%20\cdot%20\left(\frac{f(E_n)}{f'(E_n)}\right)^2}{f'(E_n)})

Where:

- **f(E)** is the Kepler equation reformulated as f(E) = E - e \* sin(E) - M
- **f'(E)** and **f''(E)** are the first and second derivatives of f(E)

This second-order correction accelerates convergence and is particularly valuable in high-precision applications such as satellite navigation and space exploration. It demonstrates how Newton’s foundational ideas have evolved to meet the demands of modern science.

To break this down: while the standard Newton-Raphson method adjusts estimates based on the first derivative, the second-order approach incorporates information from the curvature of the function (via the second derivative). This added insight helps the method converge even faster, particularly when very high accuracy is required.

**[Image Placeholder: Graphical representation comparing first-order and second-order Newton-Raphson convergence.]**

This iterative process quickly converged to the true value of E, especially for small eccentricities. The success of this approach in solving Kepler's Equation highlights the importance of root-finding methods and their transformative impact on scientific computation. This historical foundation will guide the exploration of how such techniques extend into multidimensional contexts, addressing the added complexities and opportunities they present.

## Strengths and Challenges

Newton's Method is highly effective in certain types of multidimensional problems, particularly when the equations and conditions align well. These are the scenarios where it excels:

#### **1. Well-Conditioned Jacobians**
- Newton's Method performs best when the Jacobian matrix is stable and invertible (not singular or poorly conditioned). A well-behaved Jacobian ensures smooth progression toward the root.
- **Example:** Systems of equations with minimal interaction between variables, such as weakly coupled systems in mechanics or circuits.

#### **2. Smooth, Differentiable Functions**
- The method thrives on functions that are continuously differentiable and exhibit locally quadratic behavior near the root.
- **Example:** Optimization problems in physics or engineering where gradients are predictable and regular, such as energy minimization in a mechanical system.

#### **3. Close Initial Guesses**
- When the starting point is near the actual root, Newton's Method converges quadratically, meaning the number of correct digits doubles with each iteration.
- **Example:** Solving design optimization problems where a good initial guess is available from prior analysis or simulation.

### Failures of Newton's Method

While powerful, Newton's Method has limitations that can prevent it from working effectively in some cases:

#### **1. Poor Initial Guesses**
- The method is highly sensitive to the starting point. A guess far from the root can lead to divergence or convergence to the wrong solution.
- **Example:** Functions with multiple roots or saddle points can mislead the method, causing oscillations or divergence.

#### **2. Singular or Ill-Conditioned Jacobians**
- If the Jacobian matrix is singular or nearly singular, the method fails because the matrix cannot be reliably inverted.
- **Example:** Systems with strongly interdependent variables or nearly parallel constraints can cause numerical instability.

### Introducing Broyden's Method

Broyden’s Method addresses some of these limitations by approximating the Jacobian matrix rather than recalculating and inverting it at each iteration. This makes it more robust and computationally efficient in challenging scenarios.

## Broydens Method
#### **How Broyden's Method Works**

1. **Jacobian Approximation**
   - Broyden's Method begins with an initial solution guess \( \mathbf{x}_0 \) and an approximate Jacobian matrix \( \mathbf{B}_0 \), often initialized as the identity matrix:

     $$
     \mathbf{B}_0 = \mathbf{I}
     $$

   - The method avoids recalculating the Jacobian matrix by updating the approximation iteratively using information from previous steps.

2. **Iterative Updates**
   - At each iteration \( n \):
     - Solve the linear system:

       $$
       \mathbf{B}_n \Delta \mathbf{x}_n = -\mathbf{F}(\mathbf{x}_n)
       $$

       to compute the update step \( \Delta \mathbf{x}_n \).
     - Update the solution:

       $$
       \mathbf{x}_{n+1} = \mathbf{x}_n + \Delta \mathbf{x}_n
       $$

     - Evaluate the function at the new solution:

       $$
       \Delta \mathbf{F}_n = \mathbf{F}(\mathbf{x}_{n+1}) - \mathbf{F}(\mathbf{x}_n)
       $$

     - Adjust the Jacobian approximation using the rank-one update formula:

       $$
       \mathbf{B}_{n+1} = \mathbf{B}_n + \frac{(\Delta \mathbf{F}_n - \mathbf{B}_n \Delta \mathbf{x}_n) \Delta \mathbf{x}_n^T}{\| \Delta \mathbf{x}_n \|^2}
       $$

3. **Convergence**
   - Repeat the iterative process until convergence criteria are met, such as:

     $$
     \| \mathbf{F}(\mathbf{x}_n) \| < \text{tolerance} \quad \text{or} \quad \| \Delta \mathbf{x}_n \| < \text{tolerance}.
     $$

#### **Key Advantages of Broyden's Method**

1. **Efficient Jacobian Updates**
   - By approximating the Jacobian instead of recalculating it, Broyden’s Method avoids the computational cost of explicit Jacobian evaluation and inversion.

2. **Robustness in Singular Scenarios**
   - Broyden’s Method can handle cases where the Jacobian is singular or ill-conditioned, enabling the iterative process to continue even when Newton’s Method would fail.

3. **Scalability**
   - It scales better for high-dimensional problems where recalculating and inverting a full Jacobian matrix would be computationally prohibitive. Limited-memory variants further enhance its utility in large systems.

#### **Best Use Cases**

- **Large Nonlinear Systems:** Effective for solving high-dimensional problems where explicit Jacobian computations are too costly.
- **Poorly Conditioned Problems:** Performs well in cases where the Jacobian matrix is nearly singular or ill-conditioned.
- **Optimization Problems:** Useful for scenarios with many variables and nonlinear constraints, such as physical simulations or machine learning applications.

#### **Edge Cases in Broyden's Method**

1. **Sparse Jacobian Matrices**
   - When the Jacobian matrix is sparse (mostly zeros), directly updating and storing the full approximation \( \mathbf{B}_n \) can be inefficient.
   - **Solution:** Use sparse matrix storage formats and operations to reduce memory and computational costs.

2. **Banded Jacobian Matrices**
   - For banded Jacobians, where nonzero elements are clustered near the diagonal, standard updates waste resources on zero elements.
   - **Solution:** Adapt the update step to preserve and operate only on the banded structure of the Jacobian.

3. **Slow Convergence in Ill-Conditioned Systems**
   - In systems with poor initial guesses or nearly singular Jacobians, the approximation \( \mathbf{B}_n \) may degrade, leading to slow progress.
   - **Solution:** Introduce damping factors to stabilize updates or restart with a fresh approximation when necessary.

4. **High-Dimensional Problems**
   - In systems with a large number of variables, maintaining and updating \( \mathbf{B}_n \) becomes computationally expensive.
   - **Solution:** Use limited-memory variants, such as L-BFGS, to approximate only a portion of the Jacobian, focusing on the most critical directions.

5. **Highly Nonlinear Systems**
   - When the system of equations is highly nonlinear, the rank-one update may fail to capture the behavior accurately, causing divergence.
   - **Solution:** Combine Broyden’s Method with line search techniques to control step sizes and ensure stability.

6. **Discontinuous Functions**
   - For functions with discontinuities, Broyden’s Method may struggle as the Jacobian approximation fails to account for abrupt changes.
   - **Solution:** Switch to global methods or hybrid approaches that combine Broyden’s Method with robustness-focused strategies like trust-region methods.


## Pseudocode
```text
Code used to generate GIF of Newton's Method used in 3D
1. Define functions:
   - Create function `f(x, y)` for the first equation.
   - Create function `g(x, y)` for the second equation.

2. Define Jacobian Matrix:
   - Write a function to calculate the Jacobian matrix `J(x, y)` using partial derivatives of `f` and `g`.

3. Implement Newton's Method:
   - Input: Initial guess `(x0, y0)`, maximum iterations, and tolerance.
   - Loop through iterations:
     - Evaluate the Jacobian matrix at the current guess.
     - Compute the function values `f(x, y)` and `g(x, y)`.
     - Solve for the update step using `J^-1 * F`.
     - Update the guess `(x, y)`.
     - Check for convergence by comparing the step size to the tolerance.
     - Save the current point to the list of iterations.
   - Output: List of points representing iterative guesses.

4. Generate a grid for visualization:
   - Create a meshgrid of `x` and `y` values.
   - Compute `z` values for `f(x, y)` and `g(x, y)` to represent surfaces.

5. Plot in 3D:
   - Initialize a 3D plot.
   - Add the surfaces `z1 = f(x, y)` and `z2 = g(x, y)`.
   - Add iterative points as markers projected onto the `x-y` plane.

6. Create an animation:
   - Loop through frames:
     - Gradually reveal iterative points based on frame count.
     - Rotate the 3D view slightly for each frame.
     - Update the plot to show progress.
   - Save the animation as a GIF.

7. Download the GIF
```

## References

1.<a id="ref1"></a> Press, W. H., Teukolsky, S. A., Vetterling, W. T., & Flannery, B. P. (2007). Chapter 9: Root finding and nonlinear sets of equations. In *Numerical recipes: The art of scientific computing* (3rd ed.). New York: Cambridge University Press. ISBN 978-0-521-88068-8.
