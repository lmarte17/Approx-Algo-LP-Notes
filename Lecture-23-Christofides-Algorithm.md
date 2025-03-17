# Lecture 23: Christofides' Algorithm

## Summary
This lecture presents Christofides' algorithm, an elegant approximation algorithm for the metric Traveling Salesman Problem (TSP) that achieves a 3/2-factor approximation. The algorithm improves upon the previously discussed 2-factor approximation by incorporating minimum-weight perfect matching of odd-degree vertices. The lecture explains the algorithm's steps in detail and provides a proof of the 3/2 approximation ratio.

## Review of Previous Approach

### MST-Based 2-Factor Approximation
- Starting with a metric TSP instance (complete graph with weights satisfying triangle inequality)
- Computing a minimum spanning tree (MST)
- Performing a tree traversal with shortcutting to create a tour
- This approach yields a tour with weight ≤ 2 times the optimal solution

### Limitations
- The simple tree traversal approach cannot achieve better than a 2-factor approximation
- We need a more sophisticated approach to improve this bound

## Christofides' Algorithm

### Key Insight
The core insight is to create a graph where all vertices have even degree, which allows for an Eulerian tour. This is done by:
1. Finding the MST
2. Identifying vertices with odd degree in the MST
3. Adding additional edges to make all vertices have even degree

### Algorithm Steps:
1. Compute the minimum spanning tree (MST) of the metric TSP instance
2. Identify all vertices with odd degree in the MST
   - Let O be the set of odd-degree vertices in the MST
3. Compute a minimum-weight perfect matching M on the subgraph induced by O
   - Note: There is always an even number of odd-degree vertices in any graph (theorem)
   - The matching can be found using Edmonds' Blossom algorithm
4. Add the edges from the matching M to the MST
   - If an edge appears in both MST and M, treat it as two separate edges
5. The resulting graph has every vertex with even degree
6. Find an Eulerian tour E of this graph (always possible with even degrees)
7. Create a TSP tour by shortcutting the Eulerian tour
   - Skip repeated vertices, taking direct edges instead

### Example
- MST with odd-degree vertices (e.g., vertices 1, 2, 4, 5, 7, 10, 12, 13)
- Compute minimum-weight matching on these vertices: (1-2), (4-13), (7-12), (5-10)
- Add these matching edges to the MST
- Resulting graph has all vertices with even degree
- Compute Eulerian tour of the combined graph
- Shortcut the Eulerian tour to get final TSP tour

## Proof of 3/2-Factor Approximation

### Key Components of the Proof:
Let:
- T = final TSP tour produced by Christofides' algorithm
- E = Eulerian tour before shortcutting
- MST = minimum spanning tree of the TSP instance
- M = minimum-weight perfect matching on odd-degree vertices
- OPT = optimal TSP tour

### Step 1: Weight Relationship Between Tour and Eulerian Tour
- w(T) ≤ w(E)
- This follows from the triangle inequality: whenever we shortcut, the direct edge weight is less than the sum of weights along the path being replaced

### Step 2: Weight of Eulerian Tour
- w(E) = w(MST) + w(M)
- The Eulerian tour traverses each edge in the combined graph exactly once

### Step 3: Weight of MST
- w(MST) ≤ w(OPT)
- Removing any edge from OPT creates a spanning tree, and MST is minimum among all spanning trees

### Step 4: Weight of Matching (Key Insight)
- w(M) ≤ (1/2) · w(OPT)

Proof of Step 4:
- Consider the optimal tour restricted to odd-degree vertices O
- This forms a cycle visiting each vertex in O exactly once
- The edges of this cycle can be divided into two disjoint matchings:
  * M₁: edges at even positions in the cycle
  * M₂: edges at odd positions in the cycle
- Due to triangle inequality: w(OPT) ≥ w(M₁) + w(M₂)
- Since M is the minimum-weight matching: w(M) ≤ min(w(M₁), w(M₂))
- Therefore: w(M) ≤ (1/2) · (w(M₁) + w(M₂)) ≤ (1/2) · w(OPT)

### Final Analysis
Combining all inequalities:
- w(T) ≤ w(E) = w(MST) + w(M) ≤ w(OPT) + (1/2) · w(OPT) = (3/2) · w(OPT)

Therefore, Christofides' algorithm achieves a 3/2-factor approximation for metric TSP.

## Historical Significance and Recent Developments

- Christofides' algorithm was developed by Nicos Christofides, an electrical engineer
- The algorithm remained the best known approximation for metric TSP for nearly 50 years
- In 2022, a slight improvement to the algorithm was discovered, achieving a factor of (3/2 - ε) for a very small ε (approximately 10⁻¹²)
- This marks the first improvement to the approximation ratio in decades

## Practical Considerations

- The algorithm requires finding a minimum-weight perfect matching
- This can be done in polynomial time using Edmonds' Blossom algorithm
- The overall algorithm runs in polynomial time
- The elegance of the algorithm lies in its simplicity and the clever use of graph-theoretic properties
