# Lecture 10: Branch and Bound Algorithms for Integer Linear Programming

## Introduction to Integer Linear Programming

Integer Linear Programming (ILP) is an optimization problem with the following form:

$$
\begin{align*}
\text{Maximize } & c_1x_1 + c_2x_2 + \ldots + c_nx_n \\
\text{Subject to } & a_{11}x_1 + a_{12}x_2 + \ldots + a_{1n}x_n \leq b_1 \\
& a_{21}x_1 + a_{22}x_2 + \ldots + a_{2n}x_n \leq b_2 \\
& \vdots \\
& a_{m1}x_1 + a_{m2}x_2 + \ldots + a_{mn}x_n \leq b_m \\
& x_1, x_2, \ldots, x_n \text{ are integers}
\end{align*}
$$

The key distinguishing factor from standard Linear Programming is the **integrality constraint** that requires all variables to take integer values.

### Special Cases of ILP

- **Binary ILP**: When variables are restricted to $\{0,1\}$ values
- **Bounded ILP**: When variables are restricted to a range, e.g., $0 \leq x_i \leq 1$

## Computational Complexity

ILP problems are **NP-hard**, making them computationally challenging to solve. Most algorithms for ILP have exponential time complexity in the number of variables and constraints, typically:

$$O(2^{p(m,n)})$$

Where $p(m,n)$ is some polynomial function of $m$ and $n$.

## Algorithms for Solving ILP

Several approaches exist for solving ILP problems:

1. **Branch and Bound** (focus of this lecture)
2. Cutting Plane Methods
3. Branch and Cut (combines the first two)
4. Lift and Project
5. Other specialized algorithms

## Linear Programming Relaxation

A fundamental concept in solving ILP is the **LP relaxation**:

- Remove the integrality constraints from the ILP
- Solve the resulting LP problem
- The solution gives an upper bound (for maximization) on the ILP optimal value

### Geometric Interpretation

- The LP relaxation defines a polyhedron in n-dimensional space
- The ILP feasible solutions are the lattice/integer points within this polyhedron
- The LP optimal solution may be fractional (non-integer)
- If the LP optimal solution happens to be integral, it is also optimal for the ILP

### Example 1: LP Relaxation Gives Integral Solution

Consider:
$$
\begin{align*}
\text{Maximize } & x + y \\
\text{Subject to } & -1 \leq x \leq 1 \\
& -1 \leq y \leq 1 \\
& x, y \text{ are integers}
\end{align*}
$$

In this example:
- The LP relaxation has a feasible region forming a square with vertices at $(-1,-1)$, $(-1,1)$, $(1,-1)$, and $(1,1)$
- The integer points within this region are the lattice points
- The optimal solution for both the LP relaxation and the ILP is $(1,1)$ with objective value 2

### Example 2: LP Relaxation Gives Fractional Solution

Consider:
$$
\begin{align*}
\text{Maximize } & x + y \\
\text{Subject to } & y \geq 0 \\
& y \leq 6x - 6 \\
& y \leq -6x + 12 \\
& x, y \text{ are integers}
\end{align*}
$$

In this example:
- The LP relaxation has a triangular feasible region
- The only feasible integer points are $(1,0)$ and $(2,0)$
- The LP relaxation optimal solution is $(1.5, 3)$ with objective value 4.5
- The ILP optimal solution is $(2,0)$ with objective value 2
- This shows how the LP relaxation can be arbitrarily far from the ILP solution

## Branch and Bound Algorithm

The Branch and Bound algorithm is a systematic approach to solving ILP problems by:
1. Solving LP relaxations
2. Branching on fractional variables
3. Pruning subproblems that cannot yield better solutions

### Algorithm Steps

1. Solve the LP relaxation of the original problem
2. If the solution is integral, we're done
3. If the solution is fractional, select a variable $x_i$ with fractional value $x_i^*$
4. Create two branches:
   - Add constraint $x_i \leq \lfloor x_i^* \rfloor$ (floor branch)
   - Add constraint $x_i \geq \lceil x_i^* \rceil$ (ceiling branch)
5. Recursively solve these subproblems
6. Return the best integral solution found

### Branch and Bound Tree

The algorithm builds a tree where:
- Each node is a linear program
- The root is the original LP relaxation
- Each branching creates two child nodes with additional constraints
- Leaf nodes are either:
  - Infeasible LPs (contradictory constraints)
  - LPs with integral solutions
  - LPs that need further branching

### Example of Branching

If the LP solution has $x_i = 2.127843$:
- Floor branch: Add constraint $x_i \leq 2$
- Ceiling branch: Add constraint $x_i \geq 3$

### Termination and Solution Selection

The algorithm terminates when all leaves have been processed:
- For maximization problems, select the leaf with the highest objective value
- For minimization problems, select the leaf with the lowest objective value

## Optimizations for Branch and Bound

### Pruning

- **Bound Pruning**: If a node's LP relaxation has an objective value worse than the best known integral solution, prune that node
- This significantly reduces the number of nodes explored

### Other Performance Improvements (mentioned but not detailed)

- Variable selection strategies (which fractional variable to branch on)
- Node selection strategies (which leaf to process next)
- Integration with cutting planes

## Implementation Considerations

The lecture mentions that an implementation of the Branch and Bound algorithm is provided in supplementary notes, along with examples.

## Computational Challenges

- Branch and Bound can be exponential time in the worst case
- Various heuristics can improve practical performance
- No provably efficient (polynomial time) algorithm exists for general ILP

## Summary

Branch and Bound is a powerful algorithmic approach for solving Integer Linear Programming problems that:
1. Uses LP relaxations to establish bounds
2. Systematically explores the solution space through branching
3. Prunes branches that cannot yield better solutions
4. Eventually finds the optimal integer solution
