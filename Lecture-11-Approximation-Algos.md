# Lecture 11: Approximation Algorithms

## Introduction to Approximation Algorithms

Approximation algorithms allow us to solve NP-complete optimization problems in polynomial time by accepting solutions that are within a certain factor of the optimal solution.

### Key Motivation
- Many important NP-complete problems are optimization problems (minimizing or maximizing a value)
- Exact solutions require exponential time algorithms (e.g., $O(2^n)$, $O(3^n)$, etc.)
- Integer Linear Programming (ILP) approaches aren't scalable for large problems
- Approximation algorithms provide practical alternatives with performance guarantees

## NP-Complete Optimization Problems

Examples of NP-complete optimization problems include:

| Problem | Optimization Goal |
|---------|------------------|
| Vertex Cover | Minimize the size of the vertex cover |
| Knapsack | Maximize the total value of selected items within weight limit |
| Traveling Salesperson Problem (TSP) | Minimize the total distance traveled |
| MAX SAT | Maximize the number of satisfied clauses |

## Approximation Guarantees

For an approximation algorithm solving a minimization problem:
- Let OPT = value of the optimal solution
- Let ALG = value of the solution from our approximation algorithm
- The approximation ratio α is defined such that:
  
  $\text{OPT} \leq \text{ALG} \leq \alpha \cdot \text{OPT}$

> Note: For maximization problems, the inequality is reversed:
> $\text{OPT} \geq \text{ALG} \geq \frac{\text{OPT}}{\alpha}$ or $\text{ALG} \geq \frac{\text{OPT}}{\alpha}$

### Types of Approximation Guarantees

1. **Constant Factor Approximation**:
   - α is a constant (e.g., 2, 3, 1.5)
   - Example: "A 2-approximation algorithm" means it produces solutions at most twice the optimal value

2. **Non-Constant Factor Approximation**:
   - α depends on the problem size n
   - Example: O(log n)-approximation - if problem size is $2^{10}$, α ≈ 10; if problem size is $2^{100}$, α ≈ 100

## Characteristics of Approximation Algorithms

- Often surprisingly simple algorithms (e.g., greedy approaches)
- The challenge typically lies in proving the approximation ratio
- Provide practical solutions with theoretical guarantees
- Run in polynomial time (unlike exact algorithms for NP-complete problems)

## Problems to Be Covered

In this module and the following ones, we will explore approximation algorithms for several important problems:

1. Job Shop Scheduling
2. Vertex Cover
3. Knapsack
4. Traveling Salesperson Problem (TSP)
5. MAX SAT

Each of these problems has significant practical applications but is computationally intractable to solve optimally for large instances.

## Summary

- Approximation algorithms provide a middle ground for tackling NP-complete optimization problems
- They run in polynomial time while guaranteeing solutions within some factor of optimal
- The approximation ratio (α) characterizes how close our solution is to optimal
- These techniques are essential for practical implementations of otherwise intractable problems