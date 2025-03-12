# Lecture 6: Integer Linear Programs

## Introduction to Integer Linear Programming

An Integer Linear Program (ILP) is similar to a Linear Program with one critical difference: the decision variables must be integers.

### Basic Definition

- Like a Linear Program, an ILP involves:
  - Maximizing or minimizing a linear objective function
  - Subject to linear constraints (inequalities)
- **Key Difference**: Decision variables must be integers (elements of set $\mathbb{Z}$)

### Example Format:
$$
\begin{align}
\text{Maximize } & 2x_1 + 3x_2 + 3.5x_3 \\
\text{Subject to } & \text{linear constraints} \\
& x_1, x_2, x_3 \in \mathbb{Z}
\end{align}
$$

### Contrast with Linear Programming
- In standard LP: Variables can be fractional (e.g., $x_1 = 2.5$, $x_2 = 0.5$)
- In ILP: Variables must be integers (e.g., $-3, -2, -1, 0, 1, 2, 3, ...$)
- The integer constraint comes from the German word for integers, represented by $\mathbb{Z}$

## The Knapsack Problem as an ILP

### Problem Description
- We have $n$ items: $I_1, I_2, ..., I_n$
- Each item $i$ has:
  - Value: $V_i$
  - Weight: $W_i$
- Knapsack has weight capacity $W$
- Goal: Select items to maximize total value while keeping total weight ≤ $W$

### Notes on Problem Complexity
- This is a combinatorial optimization problem
- With $n$ items, there are $2^n$ possible combinations
- Knapsack is NP-hard (though it has a pseudo-polynomial time dynamic programming solution)

### ILP Formulation

**Decision Variables:**
- $x_i = 0$ if item $I_i$ is not picked
- $x_i = 1$ if item $I_i$ is picked
- These are called indicator or binary variables

**Objective Function:**
$$\text{Maximize } V_1x_1 + V_2x_2 + ... + V_nx_n$$

**Constraints:**
$$W_1x_1 + W_2x_2 + ... + W_nx_n \leq W$$
$$0 \leq x_i \leq 1 \text{ for all } i = 1, 2, ..., n$$
$$x_1, x_2, ..., x_n \in \mathbb{Z}$$

Since the variables are restricted to be integers between 0 and 1, they can only take values 0 or 1.

### Special Case: Binary ILP
- Also called 0-1 ILP
- All variables can only take values 0 or 1
- Knapsack is a classic example of a 0-1 ILP

## Comparing LP and ILP: Geometric Intuition

### Example 1: Same Optimal Solution

$$
\begin{align}
\text{Maximize } & 2x_1 + 3x_2 \\
\text{Subject to } & x_1 + x_2 \leq 5 \\
& x_1, x_2 \geq 0
\end{align}
$$

**Geometric Interpretation:**
- LP feasible region: Entire triangle with vertices at (0,0), (5,0), and (0,5)
- ILP feasible region: Just the integer points (lattice points) within this triangle
- In this case, optimal solution for both LP and ILP: $(0,5)$

### Example 2: Different Optimal Solutions

$$
\begin{align}
\text{Maximize } & x_1 + x_2 \\
\text{Subject to } & 4x_1 + 3x_2 \leq 8 \\
& x_1, x_2 \geq 0
\end{align}
$$

**Geometric Interpretation:**
- LP feasible region: Triangle bounded by the constraints
- LP optimal solution: $(0, \frac{8}{3})$ with value $\frac{8}{3}$ = 2.666...
- ILP feasible region: Only the integer points
- ILP optimal solution: Either $(0,2)$, $(1,1)$, or $(2,0)$ with value $2$

## LP Relaxation and Integrality Gap

### LP Relaxation
LP relaxation of an ILP is obtained by removing the integrality constraints.

**General Form:**
$$
\begin{align}
\text{Maximize } & \sum_{i} c_i x_i \\
\text{Subject to } & \sum_{j} a_{ij} x_j \leq b_i \text{ for } i = 1, 2, ..., m\\
& x_i \in \mathbb{Z} \text{ for all } i
\end{align}
$$

Removing the integrality constraints gives the LP relaxation.

### Key Properties

1. For maximization problems:
   - Optimal solution of LP ≥ Optimal solution of ILP
   - This is because every ILP solution is also an LP solution, but not vice versa

2. If the optimal solution of the LP is integral:
   - Optimal solution of LP = Optimal solution of ILP
   - This happens when the LP's optimal solution already satisfies the integrality constraints

### Integrality Gap

The integrality gap measures how far the LP optimal solution can be from the ILP optimal solution.

**Definition (for maximization):**
$$\text{Integrality Gap} = \frac{\text{Optimal LP Solution}}{\text{Optimal ILP Solution}}$$

For minimization problems, the ratio is inverted.

**Properties:**
- Always ≥ 1 (by definition)
- Example: If LP optimum = 2.5 and ILP optimum = 2, then gap = 1.25

**Unbounded Gap:**
- In general, there is no universal bound on the integrality gap
- An example was constructed with:
  - A triangular feasible region with one vertex at $(0.5, M)$ where $M$ is arbitrarily large
  - Objective: Maximize $x_2$
  - LP optimum: $M$ (can be made arbitrarily large)
  - ILP optimum: 0
  - This makes the integrality gap unbounded (effectively infinite)

## Conclusion

- While integrality gaps can be arbitrarily large in general, certain classes of problems have bounded integrality gaps
- These bounded-gap problems will be important when studying approximation algorithms
- For those problems, LP relaxations can provide meaningful bounds on ILP solutions
