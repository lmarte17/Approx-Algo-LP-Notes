# Lecture 16: NP-hardness of TSP

## Introduction
This lecture focuses on proving that the Traveling Salesperson Problem (TSP) is NP-complete. We'll examine the decision version of TSP and provide a formal proof of its NP-completeness through reduction from the Hamiltonian Cycle problem.

## The Decision Version of TSP

The decision version of TSP can be formulated as:

**Input:** 
- A weighted graph $G = (V, E)$ with weights on edges
- A weight limit $W$

**Question:** 
- Is there a TSP tour with total cost less than or equal to $W$?

## Proving NP-Completeness

To prove that TSP is NP-complete, we need to establish two key properties:

1. TSP is in NP
2. TSP is NP-hard (by reducing a known NP-complete problem to TSP)

### Part 1: TSP is in NP

A problem is in NP if we can verify a proposed solution in polynomial time.

For TSP, given a proposed tour (e.g., a sequence of vertices like $1 \rightarrow 3 \rightarrow 7 \rightarrow 2 \rightarrow 4 \rightarrow 8 \rightarrow 15 \rightarrow 24 \rightarrow \ldots \rightarrow 1$), we can:

1. Verify that it is a valid TSP tour:
   - Check that it visits every vertex exactly once
   - Confirm it returns to the starting vertex
   - This can be done in linear time $O(n)$

2. Verify that the total cost is less than or equal to $W$:
   - Calculate the sum of edge weights in the tour
   - Compare this sum to $W$
   - This can also be done in linear time $O(n)$

Since we can verify a proposed solution in polynomial time, TSP is in NP.

### Part 2: TSP is NP-hard

To prove TSP is NP-hard, we will reduce a known NP-complete problem to TSP. Specifically, we'll reduce the Hamiltonian Cycle problem to TSP.

#### Hamiltonian Cycle Problem

**Input:** An undirected graph $G = (V, E)$

**Question:** Does $G$ contain a Hamiltonian cycle?

> Note: A Hamiltonian cycle is a cycle that visits every vertex exactly once and returns to the starting vertex.

The Hamiltonian Cycle problem is known to be NP-complete (proven by Richard Karp in 1974 through reduction from 3-CNF SAT).

Key differences between Hamiltonian Cycle and TSP:
1. In Hamiltonian Cycle, the graph is not necessarily complete (some edges may be missing)
2. There are no costs/weights associated with edges in Hamiltonian Cycle

#### Reduction from Hamiltonian Cycle to TSP

The reduction works as follows:

1. Take the input graph $G = (V, E)$ from the Hamiltonian Cycle problem
2. Create a complete graph $G' = (V, E')$ where:
   - For each edge $e \in E$ (original edges in $G$), assign a weight of 1
   - For each edge $e \notin E$ (missing edges in $G$), assign a weight of 2
3. Set the weight limit $W = n$ (where $n$ is the number of vertices)

**Claim:** $G$ has a Hamiltonian cycle if and only if $G'$ has a TSP tour of cost less than or equal to $n$.

**Proof:**

→ (Forward direction) If $G$ has a Hamiltonian cycle, then the same cycle in $G'$ uses only edges of weight 1, resulting in a tour of cost exactly $n$.

← (Backward direction) If $G'$ has a TSP tour of cost less than or equal to $n$, then this tour must use only edges of weight 1 (since any tour using even one edge of weight 2 would have a total cost of at least $n+1$). Therefore, all edges in this tour must exist in the original graph $G$, forming a Hamiltonian cycle.

Therefore, by solving the TSP problem on $G'$ with weight limit $W = n$, we can determine whether $G$ has a Hamiltonian cycle:
- If there's a TSP tour of cost ≤ $n$, then $G$ has a Hamiltonian cycle
- If every TSP tour has cost ≥ $n+1$, then $G$ does not have a Hamiltonian cycle

## Implications

The reduction proves that TSP is NP-complete. Furthermore, this reduction reveals that TSP is actually strongly NP-complete, meaning it remains hard even when the numeric parameters in the problem (edge weights) are bounded by a polynomial in the input size.

Additionally, this hardness result suggests that TSP is not only difficult to solve exactly but also difficult to approximate efficiently, which will be discussed in future lectures.

## Summary

- TSP is NP-complete, as proven by:
  - Showing TSP is in NP (solutions can be verified in polynomial time)
  - Reducing the known NP-complete Hamiltonian Cycle problem to TSP
- The reduction assigns weights of 1 to existing edges and weights of 2 to missing edges
- TSP is strongly NP-complete, indicating its inherent computational difficulty
- The NP-completeness of TSP has significant implications for approximation algorithms, which will be explored further