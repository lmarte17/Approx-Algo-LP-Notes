# Lecture 17: Hardness of Approximating TSP

## Introduction to Hardness of Approximation

In previous lectures, we've explored various approximation algorithms:
- Vertex Cover: 2-factor approximation
- Job Shop Scheduling: 3/2-factor approximation
- Max SAT: 7/8-factor approximation

Today we'll prove a fundamental result about the **Traveling Salesperson Problem (TSP)**: not only is finding an optimal solution NP-hard, but even approximating TSP within any constant factor is NP-hard (unless P = NP).

## Core Question

Does there exist an efficient algorithm for TSP that guarantees:

$$\text{Cost}(\text{TSP}_{\text{alg}}) \leq \alpha \cdot \text{Cost}(\text{TSP}_{\text{opt}})$$

Where:
- $\alpha$ is some constant factor (with $\alpha > 1$)
- The algorithm runs in polynomial time

We will prove that unless P = NP, no such algorithm exists for general TSP.

## Proof Strategy: Reduction from Hamiltonian Cycle

We'll use a reduction from the Hamiltonian Cycle problem to show that even approximating TSP is NP-hard.

### Recall: Previous Reduction (Hamiltonian Cycle → TSP)

In our earlier reduction:
- For edges in the original graph: cost = 1
- For edges not in the original graph: cost = 2
- We asked: "Is there a tour of cost ≤ n?"

### New Reduction with Modified Weights

Assume an α-approximation algorithm exists for TSP. We'll modify our reduction:

1. For edges in the original graph: cost = 1
2. For edges not in the original graph: cost = (α-1)n + 2

This creates a critical gap between two scenarios:

### Case Analysis

**Case 1:** If the original graph has a Hamiltonian cycle
- The optimal TSP tour uses only original edges
- Optimal TSP cost = n

**Case 2:** If the original graph has no Hamiltonian cycle
- The optimal TSP tour must use at least one non-original edge
- Tour cost = (n-1) × 1 + ((α-1)n + 2)
- Simplifies to: αn + 1

### Using the Approximation Algorithm to Decide Hamiltonian Cycle

If we run an α-approximation algorithm on our constructed instance:

- If the original graph has a Hamiltonian cycle:
  - Optimal TSP cost = n
  - Approximation algorithm returns tour of cost ≤ αn

- If the original graph has no Hamiltonian cycle:
  - Optimal TSP cost ≥ αn + 1
  - Approximation algorithm returns tour of cost ≥ αn + 1

Therefore, we can determine whether the original graph has a Hamiltonian cycle by checking if the approximation algorithm's tour costs ≤ αn.

## Implications

Since the Hamiltonian Cycle problem is NP-complete, and our reduction shows we can solve it using an α-approximation algorithm for TSP:

- **Theorem:** Unless P = NP, there is no polynomial-time α-approximation algorithm for general TSP for any constant α > 1.

This is a strong inapproximability result, showing that TSP is not just hard to solve exactly, but also hard to approximate within any constant factor.

## Special Cases: Metric and Euclidean TSP

The hardness result above applies only to general TSP. For special cases:

### Metric TSP
- Satisfies the triangle inequality: $c(A,C) \leq c(A,B) + c(B,C)$
- Our reduction doesn't work for metric TSP
- A 3/2-factor approximation exists for metric TSP (which we'll study later)

### Euclidean TSP
- Points are in a 2D plane with straight-line distances
- Even better approximations are possible
- There exists a polynomial-time approximation scheme (PTAS) that can approximate to arbitrarily close to 1

## Conclusion

- General TSP: No constant-factor approximation unless P = NP
- Metric TSP: 3/2-factor approximation is possible
- Euclidean TSP: Can be approximated arbitrarily closely to optimal

Next lecture will explore exact algorithms for solving TSP precisely.