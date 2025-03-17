# Lecture 19: Integer Linear Programming Formulation for TSP

## Introduction
- This lecture focuses on converting Traveling Salesman Problems (TSP) into Integer Linear Programming (ILP) formulations
- We'll primarily work with symmetric TSPs (where cost of edge i→j equals cost of j→i)
- ILP formulations allow us to use commercial solvers like Gurobi, Cplex, or Mosek
  - These are commercial solvers (free for academic use)
  - They implement efficient branch and bound techniques for solving ILPs

## Problem Setup
- TSP with n nodes, labeled 1 through n
- Cost of edge between nodes i and j: $C_{ij}$
- For symmetric TSP: $C_{ij} = C_{ji}$

## First Formulation: Mixed Integer Linear Program (MILP)

### Decision Variables

1. **Tour Variables** ($X_{i→j}$):
   - Binary variables (0 or 1)
   - $X_{i→j} = 1$ if the tour visits node i and immediately after visits node j
   - $X_{i→j} = 0$ otherwise
   - Approximately $n(n-1)$ such variables (one for each ordered pair of nodes)

2. **Timestamp Variables** ($t_i$):
   - Real-valued variables between 1 and n
   - Represents when node i is visited in the tour
   - n such variables (one for each node)

### Constraints

1. **Flow Constraints**: Each vertex must be entered and exited exactly once
   - For each vertex i:
     - $\sum_{j} X_{i→j} = 1$ (exactly one outgoing edge)
     - $\sum_{k} X_{k→i} = 1$ (exactly one incoming edge)

2. **Timestamp Constraints**: To eliminate subtours
   - For each $t_i$: $1 \leq t_i \leq n$
   - If node j is visited immediately after node i, then timestamp of j must be greater than timestamp of i
   - Using the "Big M trick" to formulate this conditional constraint:
     - $t_i + X_{i→j} \leq t_j + n(1-X_{i→j})$ for all i,j where j ≠ 1
   - Special handling for edges entering node 1 (the starting node)

### Objective Function
- Minimize the total cost of the tour: $\min \sum_{i,j} C_{ij} \cdot X_{i→j}$

### Understanding the Big M Trick
The Big M trick converts a conditional constraint into a linear constraint:

If $X_{i→j} = 1$ (tour goes from i to j):
- The constraint becomes: $t_i + 1 \leq t_j$
- This ensures proper ordering of timestamps

If $X_{i→j} = 0$ (tour doesn't go from i to j):
- The constraint becomes: $t_i \leq t_j + n$
- This is always satisfied since $t_i, t_j \in [1,n]$, making it a "no-op" constraint

### Eliminating Subtours

Subtours are isolated cycles that don't visit all vertices. For example, a tour might incorrectly include two separate cycles:
- Cycle 1: 1→2→3→1
- Cycle 2: 4→5→4

The timestamp constraints prevent this:
- If a subtour exists (like 4→5→4), we'd have:
  - $t_4 + 1 \leq t_5$ (from 4→5)
  - $t_5 + 1 \leq t_4$ (from 5→4)
- Combined: $t_4 + 2 \leq t_4$, which simplifies to $2 \leq 0$ (impossible)

### Special Case for Node 1
- The timestamp constraints must exclude edges entering node 1
- This allows the tour to complete a full cycle
- Example: For a tour 1→2→4→5→3→1, the edge 3→1 is exempt from timestamp constraints

## Time Complexity
- Worst-case time: $O(n^3 \cdot 2^{n^2})$
- Theoretical worst-case is worse than Held-Karp algorithm ($O(n^2 \cdot 2^n)$)
- In practice, ILP solvers use many optimizations and may outperform Held-Karp

## Summary
- We can formulate TSP as a Mixed Integer Linear Program
- Two types of variables: binary tour variables and real-valued timestamp variables
- Flow constraints ensure each node is visited exactly once
- Timestamp constraints with the Big M trick prevent subtours
- This formulation works for both symmetric and asymmetric TSPs
- The next lecture will cover an alternative formulation using subtour elimination
