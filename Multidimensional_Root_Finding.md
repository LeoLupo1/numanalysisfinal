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

Many of the GIFS used in this Project were made in GIFsmos, here is a link to the project files used.

## Historical Significance

During the late 17th century, astronomers faced a pressing challenge: predicting the motion of planets accurately. Johannes Kepler had formulated his famous laws of planetary motion, including the law that planets move in elliptical orbits with the Sun at one focus. However, determining the exact position of a planet at a given time required solving a key equation known as **Kepler's Equation**:

$$
M = E - esin(E)
$$

Where:

- **M** is the mean anomaly (a measure of time within the orbital period).
- **E** is the eccentric anomaly (related to the position of the planet along its orbit).
- **e** is the orbital eccentricity (a measure of how elliptical the orbit is).

Kepler's Equation is transcendental, meaning it involves a mix of algebraic and non-algebraic functions (in this case, trigonometric), making it impossible to solve exactly using standard algebraic methods. This means that iterative numerical techniques are required to approximate the solution for E. This posed a major problem for astronomers who needed precise planetary positions for navigation, calendar design, and understanding celestial mechanics.[1]

![Visualization of Kepler's laws showing an elliptical orbit with the Sun at one focus, along with the geometric relationships for M, E, and e. The Sun is represented as a stationary dot at one focus of the ellipse. The moving planet represents the relationship between its mean anomaly (M), eccentric anomaly (E), and the orbital eccentricity (e).](images/kepler2.gif)

In this visualization, The purple dot represents the planet, the Sun is the big orange one at the focus of the elipse, and the eccentric anomaly (E), is the angle between the semi-marjor axis (the green line) and the line connecting the center of the ellipse to the planet's projection onto the circumscribed circle.

By introducing an iterative approach that could refine approximate solutions, Newton offered a "method" to solving equations like Kepler's. Some may even call it Newton's Method perhaps.

### Newton's Method

Newton's Method, also sometimes called the Newton-Raphson Method, is a 1 dimensional root approximating tequenique. That is, it let's you approximate for the value of x where f(x) = 0.

1. Find the tangent line of the function you are trying to approximate at any random point on the function.
2. Simply find the root of that tangent line.
3. Plug it into this function:

$$
x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}
$$

The formula predicts the root $x_{n+1}$ by subtracting the last root predicted, in this case the x-intercept of the tangent line, by the value of the function at that point over the derivative of the function at that point.

4. Plug that new value back into the function and repeat until satisfied.

### Approximating the solution to Kepler's Equation using Newton's Method:

Start with Kepler's Equation from earlier:

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
- Initial guess \( E_0 = 0.75 \) radiansf

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

After running Euler's Method on Kepler's Equations 3 times, yeilding E3, it very consistantly is very close to the actual root.


### Newton’s Method in Two Dimensions

In one dimension, Newton's Method adjusts guesses for a root based on the slope of the function at a given point. In two (or more) dimensions, we generalize this idea to a system of equations. Instead of dealing with a single derivative, we now work with a matrix of partial derivatives, the Jacobian matrix, which captures how each function in the system depends on each variable.

This approach enables us to iteratively approximate the solution to a system of nonlinear equations:

#### **Example: Circle and Parabola**

![Equation](https://latex.codecogs.com/gif.latex?f_1(x%2C%20y)%20%3D%20x%5E2%20%2B%20y%5E2%20-%201%2C%20f_2(x%2C%20y)%20%3D%20x%20-%20y%5E2)

These equations represent a circle of radius 1 and a parabola, and our goal is to find their intersection points in the \(x\)-\(y\) plane.

#### **The 2D Newton's Method Formula**

Newton's Method in two dimensions updates the current guess:

![Equation](https://latex.codecogs.com/gif.latex?%5Cmathbf%7Bx%7D_%7Bn%2B1%7D%20%3D%20%5Cmathbf%7Bx%7D_n%20-%20%5Cmathbf%7BJ%7D(%5Cmathbf%7Bx%7D_n)^{-1}%20%5Cmathbf%7BF%7D(%5Cmathbf%7Bx%7D_n))

where:

- ![Equation](https://latex.codecogs.com/gif.latex?%5Cmathbf%7BF%7D(%5Cmathbf%7Bx%7D)%20%3D%20%5Bf_1(x%2C%20y)%2C%20f_2(x%2C%20y)%5D%5ET),
- ![Equation](https://latex.codecogs.com/gif.latex?%5Cmathbf%7BJ%7D(%5Cmathbf%7Bx%7D)%20%3D%20%5Cbegin%7Bbmatrix%7D%20%5Cfrac%7B%5Cpartial%20f_1%7D%7B%5Cpartial%20x%7D%20%26%20%5Cfrac%7B%5Cpartial%20f_1%7D%7B%5Cpartial%20y%7D%20%5C%5C%20%5Cfrac%7B%5Cpartial%20f_2%7D%7B%5Cpartial%20x%7D%20%26%20%5Cfrac%7B%5Cpartial%20f_2%7D%7B%5Cpartial%20y%7D%20%5Cend%7Bbmatrix%7D)

Jacobian of the circle and parabola:

![Equation](https://latex.codecogs.com/gif.latex?%5Cmathbf%7BJ%7D(x%2C%20y)%20%3D%20%5Cbegin%7Bbmatrix%7D%202x%20%26%202y%20%5C%5C%201%20%26%20-2y%20%5Cend%7Bbmatrix%7D)

### Step-by-Step Iterations of 2D Newton's Method

We solve the system of equations:

$$
\begin{aligned}
f_1(x, y) &= x^2 + y^2 - 1, \\
f_2(x, y) &= x - y^2.
\end{aligned}
$$

Using the 2D Newton's Method formula:

$$
\mathbf{x}_{n+1} = \mathbf{x}_n - \mathbf{J}(\mathbf{x}_n)^{-1} \mathbf{F}(\mathbf{x}_n),
$$

where:

$$
\mathbf{F}(\mathbf{x}) = \begin{bmatrix} f_1(x, y) \\ f_2(x, y) \end{bmatrix}, \quad
\mathbf{J}(x, y) = \begin{bmatrix}
\frac{\partial f_1}{\partial x} & \frac{\partial f_1}{\partial y} \\
\frac{\partial f_2}{\partial x} & \frac{\partial f_2}{\partial y}
\end{bmatrix} = \begin{bmatrix}
2x & 2y \\
1 & -2y
\end{bmatrix}.
$$

We start with the initial guess $\mathbf{x}_0 = \begin{bmatrix} 0.2 \\ 0.2 \end{bmatrix}$ and compute the next values iteratively.


#### **Iteration 0: Initial Guess**
$$
\mathbf{x}_0 = \begin{bmatrix} 0.2 \\ 0.2 \end{bmatrix}
$$
$$
\mathbf{F}(\mathbf{x}_0) = \begin{bmatrix}
0.2^2 + 0.2^2 - 1 \\
0.2 - 0.2^2
\end{bmatrix} = \begin{bmatrix}
-0.96 \\
0.16
\end{bmatrix}
$$
$$
\mathbf{J}(\mathbf{x}_0) = \begin{bmatrix}
2(0.2) & 2(0.2) \\
1 & -2(0.2)
\end{bmatrix} = \begin{bmatrix}
0.4 & 0.4 \\
1 & -0.4
\end{bmatrix}
$$

Solve:

$$
\mathbf{x}_1 = \mathbf{x}_0 - \mathbf{J}(\mathbf{x}_0)^{-1} \mathbf{F}(\mathbf{x}_0)
$$

Result:

$$
\mathbf{x}_1 = \begin{bmatrix} 0.7428571428571429 \\ 1.9571428571428569 \end{bmatrix}
$$

---

#### **Iteration 1**
$$
\mathbf{x}_1 = \begin{bmatrix} 0.7428571428571429 \\ 1.9571428571428569 \end{bmatrix}
$$
$$
\mathbf{F}(\mathbf{x}_1) = \begin{bmatrix}
0.742857^2 + 1.957143^2 - 1 \\
0.742857 - 1.957143^2
\end{bmatrix} = \begin{bmatrix}
3.573469388462849 \\
-2.086530611537151
\end{bmatrix}
$$
$$
\mathbf{J}(\mathbf{x}_1) = \begin{bmatrix}
2(0.742857) & 2(1.957143) \\
1 & -2(1.957143)
\end{bmatrix} = \begin{bmatrix}
1.485714 & 3.914286 \\
1 & -3.914286
\end{bmatrix}
$$

Solve:

$$
\mathbf{x}_2 = \mathbf{x}_1 - \mathbf{J}(\mathbf{x}_1)^{-1} \mathbf{F}(\mathbf{x}_1)
$$

Result:

$$
\mathbf{x}_2 = \begin{bmatrix} 0.6243021346469627 \\ 1.1380646746491196 \end{bmatrix}
$$

---

#### **Iteration 2**
$$
\mathbf{x}_2 = \begin{bmatrix} 0.6243021346469627 \\ 1.1380646746491196 \end{bmatrix}
$$
$$
\mathbf{F}(\mathbf{x}_2) = \begin{bmatrix}
0.624302^2 + 1.138065^2 - 1 \\
0.624302 - 1.138065^2
\end{bmatrix} = \begin{bmatrix}
0.476682405639648 \\
-0.672045149621038
\end{bmatrix}
$$
$$
\mathbf{J}(\mathbf{x}_2) = \begin{bmatrix}
2(0.624302) & 2(1.138065) \\
1 & -2(1.138065)
\end{bmatrix} = \begin{bmatrix}
1.248604 & 2.276129 \\
1 & -2.276129
\end{bmatrix}
$$

Solve:

$$
\mathbf{x}_3 = \mathbf{x}_2 - \mathbf{J}(\mathbf{x}_2)^{-1} \mathbf{F}(\mathbf{x}_2)
$$

Result:

$$
\mathbf{x}_3 = \begin{bmatrix} 0.6180514616567657 \\ 0.84056851423266 \end{bmatrix}
$$

---

#### **Final Results**
After 7 iterations, the solution converges to:

$$
\mathbf{x}_7 = \begin{bmatrix} 0.6180339887498948 \\ 0.7861513777598889 \end{bmatrix}
$$

![3D Newton's Method Visualization](images/newtons3.gif)

Based on the results and the visualization above, there are some interesting things we can learn about Newton's method for Multivariable root finding.
Firstly, notice that the first point entered, (0.2,0.2), was closer to the actual root solution than the first iteration guess, (0.742,1.957)

## Strengths and Challenges

Some of the scenarios where Newton's Method excels:

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

### Alternative Methods to Address Failures of Newton's Method

Broyden’s Method addresses some of these limitations by approximating the Jacobian matrix rather than recalculating and inverting it at each iteration. This makes it more robust and computationally efficient in challenging scenarios.

## Broydens Method
#### **How Broyden's Method Works**

1. **Jacobian Approximation**
   - Broyden's Method begins with an initial solution guess \( \mathbf{x}_0 \) and an approximate Jacobian matrix \( \mathbf{B}_0 \), often initialized as the identity matrix:

     ![Equation](https://latex.codecogs.com/gif.latex?%5Cmathbf%7BB%7D_0%20%3D%20%5Cmathbf%7BI%7D)

   - The method avoids recalculating the Jacobian matrix by updating the approximation iteratively using information from previous steps.

2. **Iterative Updates**
   - At each iteration \( n \):
     - Solve the linear system:

       ![Equation](https://latex.codecogs.com/gif.latex?%5Cmathbf%7BB%7D_n%20%5CDelta%20%5Cmathbf%7Bx%7D_n%20%3D%20-%5Cmathbf%7BF%7D(%5Cmathbf%7Bx%7D_n))

       to compute the update step \( \Delta \mathbf{x}_n \).
     - Update the solution:

       ![Equation](https://latex.codecogs.com/gif.latex?%5Cmathbf%7Bx%7D_%7Bn%2B1%7D%20%3D%20%5Cmathbf%7Bx%7D_n%20%2B%20%5CDelta%20%5Cmathbf%7Bx%7D_n)

     - Evaluate the function at the new solution:

       ![Equation](https://latex.codecogs.com/gif.latex?%5CDelta%20%5Cmathbf%7BF%7D_n%20%3D%20%5Cmathbf%7BF%7D(%5Cmathbf%7Bx%7D_%7Bn%2B1%7D)%20-%20%5Cmathbf%7BF%7D(%5Cmathbf%7Bx%7D_n))

     - Adjust the Jacobian approximation using the rank-one update formula:

       ![Equation](https://latex.codecogs.com/gif.latex?%5Cmathbf%7BB%7D_%7Bn%2B1%7D%20%3D%20%5Cmathbf%7BB%7D_n%20%2B%20%5Cfrac%7B(%5CDelta%20%5Cmathbf%7BF%7D_n%20-%20%5Cmathbf%7BB%7D_n%20%5CDelta%20%5Cmathbf%7Bx%7D_n)%20%5CDelta%20%5Cmathbf%7Bx%7D_n%5ET%7D%7B%5C%7C%20%5CDelta%20%5Cmathbf%7Bx%7D_n%20%5C%7C%5E2%7D)

3. **Convergence**
   - Repeat the iterative process until convergence criteria are met, such as:

     ![Equation](https://latex.codecogs.com/gif.latex?%5C%7C%20%5Cmathbf%7BF%7D(%5Cmathbf%7Bx%7D_n)%20%5C%7C%20%3C%20%5Ctext%7Btolerance%7D%20%5Cquad%20%5Ctext%7Bor%7D%20%5Cquad%20%5C%7C%20%5CDelta%20%5Cmathbf%7Bx%7D_n%20%5C%7C%20%3C%20%5Ctext%7Btolerance%7D)

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
1.<a id="ref1"></a> Daekin, R. E. (2017, December). Solutions of Kepler’s equation. http://www.mygeodesy.id.au/documents/Solutions%20of%20Keplers%20Equation.pdf

2.<a id="ref2"></a> Press, W. H., Teukolsky, S. A., Vetterling, W. T., & Flannery, B. P. (2007). Chapter 9: Root finding and nonlinear sets of equations. In *Numerical recipes: The art of scientific computing* (3rd ed.). New York: Cambridge University Press. ISBN 978-0-521-88068-8.

3.<a id="ref3"></a> Kohout, J. M., & Layton, L. (n.d.). Optimized solution of Kepler’s equation. https://ntrs.nasa.gov/api/citations/19720016564/downloads/19720016564.pdf 

4.<a id="ref4"></a>3.4 Broyden’s method. Washington University in St. Louis Engineering. (n.d.). https://classes.engineering.wustl.edu/2010/spring/ese415/ch3-4.pdf
