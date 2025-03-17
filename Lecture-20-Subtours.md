# Approximation Algorithms - Lecture 20: Subtour Elimination

## Integer Linear Programming Formulation for Symmetric TSP

### Overview
- This lecture focuses on an improved integer linear programming (ILP) formulation for the Traveling Salesperson Problem (TSP)
- Specifically addresses symmetric TSP where cost from i to j equals cost from j to i (cij = cji)
- Uses binary (0-1) variables only, avoiding real-valued timestamp variables from previous formulations

### Key Components

#### Decision Variables
- **xij** = binary variable indicating whether edge (i,j) is used in the tour
- Since this is for symmetric TSP, xij and xji are considered the same variable
- No directionality is encoded in this formulation

#### Degree Constraints
- For every vertex i: ∑(j≠i) xij = 2
- This ensures exactly two edges of the tour are incident on each vertex
- One edge to enter and one edge to leave (though direction is not specified)

#### Objective Function
- Minimize ∑ cij·xij (where i < j, to avoid counting edges twice)
- cij represents the cost of traveling between vertices i and j

#### Initial ILP Formulation
```
Minimize: ∑ cij·xij
Subject to:
  ∑(j≠i) xij = 2   for all vertices i
  xij ∈ {0,1}      for all i,j
```

## The Subtour Problem

### Issue with Basic Formulation
- The degree constraints alone are insufficient
- The solution may contain disconnected components called "subtours"
- Example: In a graph with vertices 1-10, we might get disconnected tours like:
  * 1-2-3-1
  * 4-5-6-4
  * 7-8-9-10-7

### Subtour Definition
- A subtour occurs when:
  * The vertex set can be partitioned into sets S and T = V-S
  * The tour has components that remain entirely within S and/or T
  * No tour edges cross between S and T
  * This creates disconnected components, which is invalid for a TSP tour

### Valid TSP Tour Requirement
- A valid tour must cross between any partition (S and T) at least twice
- The tour should be a single connected cycle visiting all vertices

## Subtour Elimination Constraints

### Formulation
- For every proper non-empty subset S of vertices {1,...,n}:
  * ∑(i∈S, j∉S) xij ≥ 2
- This enforces that any partition of the graph must have at least 2 edges crossing it
- Ensures connectivity of the entire tour

### Theoretical Result
- With all subtour elimination constraints added, the ILP solution gives the minimum cost TSP tour

### Challenge
- Number of required constraints is approximately 2^n - 2
  * All subsets of vertices except empty set and full set
  * Can be reduced by half due to symmetry
- Still an exponential number of constraints

## Incremental Subtour Elimination Approach

### Algorithm
1. Start with only the degree constraints in the ILP
2. Solve the ILP to get an initial solution
3. Check if the solution is a valid TSP tour:
   * If yes, we're done - it's the optimal tour
   * If no, we've found a subtour
4. Add a subtour elimination constraint for the identified subtour
5. Re-solve the ILP with the added constraint
6. Repeat steps 3-5 until a valid tour is found

### Advantages
- Only adds constraints as needed
- Often requires far fewer than the theoretical maximum number of constraints
- Can solve reasonably large TSP instances
- Guarantees optimality when it terminates

### Efficiency
- Still computationally expensive
- But works surprisingly well in practice for reasonably sized problems
- Handles symmetric TSP well

## Summary
- The subtour elimination approach provides a pure 0-1 ILP formulation for TSP
- Avoids timestamp variables from previous formulations
- Requires potentially exponential constraints in the worst case
- The incremental approach adds constraints only as needed
- Next topics will cover metric TSP and approximation algorithms

