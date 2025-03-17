# Lecture 21: Metric TSP Approximation Algorithms

## Summary
This lecture focuses on metric Traveling Salesman Problem (TSP) instances and how they can be approximated, unlike general TSP which cannot be approximated to any constant factor unless P=NP. We introduce the concept of metric TSP, discuss the properties that make a TSP instance metric, and present a 2-factor approximation algorithm based on minimum spanning trees (MST). The lecture introduces Christofides' algorithm, which achieves a 3/2-factor approximation for metric TSP instances.

## Metric TSP Properties

### Definition
A TSP is called a **metric TSP** when the cost function satisfies these properties:
- **Non-negative costs**: All costs must be strictly positive between different nodes (costs are 0 only for self-loops)
- **Symmetry**: Cost from node i to j equals cost from j to i
- **Triangle inequality**: For any three nodes i, j, k, the direct cost from i to k must be less than or equal to the cost of going from i to j plus the cost from j to k

Mathematically, the triangle inequality states:
For any i, j, k: $C_{ik} \leq C_{ij} + C_{jk}$

### Examples of Metric TSP
- **Euclidean TSP**: When locations are points on a plane and distances are measured as straight-line distances
- Many real-world TSP instances are naturally metric (e.g., road networks)

### Important Note
- Metric TSP remains NP-complete 
- What changes is the ability to approximate it to a constant factor

## Short-cutting Property

The triangle inequality extends to longer paths:
- For any path 1→2→3→4→...→n, the direct edge from 1→n costs less than or equal to the sum of all edges along the path
- Formally: $C_{1n} \leq C_{12} + C_{23} + C_{34} + ... + C_{n-1,n}$
- This property is proven by applying triangle inequality repeatedly and using induction
- This "short-cutting" property is crucial for approximation algorithms

## 2-Factor Approximation Algorithm

### Algorithm Steps:
1. Start with the complete graph of the metric TSP instance
2. Compute a minimum spanning tree (MST) of the graph
3. Traverse the MST (doing a tree walk)
4. Create a TSP tour by short-cutting the traversal

### Tree Traversal and Short-cutting Procedure:
1. Choose a root node (arbitrarily)
2. Perform a depth-first traversal of the MST
3. When backtracking, use "short-cutting" to avoid revisiting nodes:
   - Skip nodes already in the tour
   - Add direct edges (shortcuts) from the last vertex in the tour to the next unvisited vertex

### Analysis of the Algorithm:
Let:
- C = cost of the produced tour
- T = cost of the minimum spanning tree 
- OPT = cost of the optimal TSP tour

We can prove that:
1. $T \leq C$ (The MST cost is less than the tour cost)
2. $C \leq 2T$ (The tour cost is at most twice the MST cost)
   - This is because each edge in the MST is traversed at most twice in the tree walk
   - Each shortcut edge costs less than the corresponding path in the tree (by triangle inequality)
3. $T \leq OPT$ (The MST cost is less than the optimal tour cost)
   - Removing any edge from the optimal tour creates a spanning tree
   - The minimum spanning tree must cost less than any other spanning tree

Therefore: $OPT \leq C \leq 2·OPT$

This proves the algorithm is a 2-factor approximation.

## Introduction to Christofides' Algorithm

Christofides' algorithm improves upon the 2-factor approximation to achieve a 3/2-factor approximation. The full details of this algorithm will be covered in subsequent lectures, but the key insight is that it modifies the MST-based approach to achieve a better approximation ratio.

### Key Components of Christofides' Algorithm:
- Starts with a minimum spanning tree
- Identifies odd-degree vertices
- Computes a minimum-weight perfect matching of odd-degree vertices
- Combines the MST and matching to create an Eulerian graph
- Derives a TSP tour through shortcutting

This elegant algorithm achieves a 3/2-factor approximation, which remains the best known approximation ratio for metric TSP for several decades.
