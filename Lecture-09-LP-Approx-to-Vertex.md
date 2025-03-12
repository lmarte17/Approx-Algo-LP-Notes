# Lecture 9: LP Approximations to Vertex Cover

## Introduction and Goals

In this lecture, we explore:
- Linear programming (LP) approximations to Integer Linear Programming (ILP) for the vertex cover problem
- Proving a bound on the integrality gap of vertex cover
- Introduction to approximation algorithms (which will be covered in more detail in the next module)

## Vertex Cover Problem Review

**Definition**: Given an undirected graph $G = (V, E)$, a vertex cover is a subset of vertices such that every edge in the graph is incident on at least one vertex in the subset.

### Example

Consider a graph with 5 vertices:
- A vertex cover could be vertices {2, 3, 4}
- This is valid because every edge in the graph is incident on at least one of these vertices
- For this example, there is no smaller vertex cover (it is optimal)

## Integer Linear Programming (ILP) Formulation

For a graph with vertices $V = \{1, 2, ..., n\}$ and edges $E$:

- **Decision variables**: $w_1, w_2, ..., w_n$ where:
  - $w_i = 1$ if vertex $i$ is in the cover
  - $w_i = 0$ if vertex $i$ is not in the cover

- **Objective function**: Minimize $\sum_{i=1}^n w_i$ (minimize the number of vertices in the cover)

- **Constraints**:
  - For every edge $(i,j) \in E$: $w_i + w_j \geq 1$ (at least one endpoint of each edge must be in the cover)
  - $w_i \in \{0,1\}$ for all $i \in V$ (binary variables)

## LP Relaxation of Vertex Cover

We relax the ILP by allowing fractional values for the decision variables:

- **Decision variables**: $z_1, z_2, ..., z_n$ where:
  - $z_i \in [0,1]$ (can take any value between 0 and 1)

- **Objective function**: Minimize $\sum_{i=1}^n z_i$

- **Constraints**:
  - For every edge $(i,j) \in E$: $z_i + z_j \geq 1$

This is now a linear programming problem that can be solved efficiently using algorithms like the ellipsoidal algorithm, simplex method, or interior point methods.

## Integrality Gap and LP Approximations

In general, the LP relaxation of an ILP can be arbitrarily far from the original ILP optimal solution. For example:
- A problem where the LP optimal solution achieves $y = 1000$
- While the ILP optimal solution achieves $y = 0$
- The gap can be made arbitrarily large

This gap is called the **integrality gap**.

## Rounding Scheme for Vertex Cover

Given the LP optimal solution $z_1^*, z_2^*, ..., z_n^*$, we can round to get an approximation of the ILP solution:

-Define $w_i^{\wedge} = \begin{cases} 
1 & \text{if } z_i^* \geq 0.5 \\
0 & \text{if } z_i^* < 0.5
\end{cases}$

### Claim 1: Rounded solution is a valid vertex cover

**Proof**: 
- For every edge $(i,j) \in E$, we know $z_i^* + z_j^* \geq 1$ (from LP constraints)
- If both $z_i^* < 0.5$ and $z_j^* < 0.5$, then their sum would be $< 1$
- Therefore, at least one of $z_i^*$ or $z_j^*$ must be $\geq 0.5$
- After rounding, at least one of $w_i^{\wedge}$ or $w_j^{\wedge}$ will be 1
- Thus, $w_i^{\wedge} + w_j^{\wedge} \geq 1$ for all edges $(i,j) \in E$
- Therefore, $w_1^{\wedge}, w_2^{\wedge}, ..., w_n^{\wedge}$ is a valid vertex cover

### Claim 2: The rounded solution is at most twice the optimal solution

Let's denote:
- $\sum_{i=1}^n w_i^*$ = size of optimal vertex cover (ILP optimum)
- $\sum_{i=1}^n z_i^*$ = LP optimum
- $\sum_{i=1}^n w_i^{\wedge}$ = size of our rounded solution

**Proof**:
1. Since the ILP optimal solution is contained within the LP feasible region:
   $\sum_{i=1}^n z_i^* \leq \sum_{i=1}^n w_i^*$

2. Each $w_i^{\wedge} \leq 2z_i^*$:
   - If $w_i^{\wedge} = 1$, then $z_i^* \geq 0.5$, so $w_i^{\wedge} \leq 2z_i^*$
   - If $w_i^{\wedge} = 0$, then $0 \leq 2z_i^*$ (since $z_i^* \geq 0$)

3. Therefore:
   $\sum_{i=1}^n w_i^{\wedge} \leq 2\sum_{i=1}^n z_i^* \leq 2\sum_{i=1}^n w_i^*$

4. We can conclude:
   $\sum_{i=1}^n w_i^* \leq \sum_{i=1}^n w_i^{\wedge} \leq 2\sum_{i=1}^n w_i^*$

This means our rounded LP solution gives us a vertex cover that is at most twice the size of the optimal vertex cover.

## Significance

1. **Approximation Algorithm**: 
   - This gives us a 2-approximation algorithm for the vertex cover problem
   - Polynomial time to solve LP + linear time for rounding
   - Much more efficient than solving the ILP (which takes exponential time)

2. **Integrality Gap**:
   - For vertex cover, the integrality gap is bounded by 2
   - This means the optimal ILP solution is at most twice the optimal LP solution

## Conclusion

- This approach demonstrates how LP relaxation and rounding can provide approximation algorithms for NP-hard problems
- For vertex cover, we can achieve a solution that is within a factor of 2 of the optimal solution in polynomial time
- In the next module, we will explore more approximation algorithms, including even better approximation methods for the vertex cover problem