# Lecture 18: Held-Karp Dynamic Programming Algorithm for TSP

## Introduction to Held-Karp Algorithm

The Held-Karp algorithm is an elegant dynamic programming approach for solving the Traveling Salesperson Problem (TSP) exactly. While still exponential, it significantly improves upon the naive brute force approach.

### Context and Problem Definition
- We focus on the **symmetric TSP**: For any two nodes i and j, cost C(i,j) = C(j,i)
- Self-loops have zero cost: C(i,i) = 0 (though not relevant as we visit each node exactly once)
- Our goal: Find the minimum cost tour that visits each vertex exactly once and returns to the starting vertex

### Algorithm Complexity Comparison
- **Brute force approach**: O(n × n!) - Enumerate all permutations
- **Held-Karp algorithm**: O(n² × 2ⁿ) - Much faster, though still exponential

## Key Insight: Staging Decisions

The core insight of Held-Karp is how it stages the decision-making process for constructing a TSP tour. Instead of building the full tour at once, it:

1. Designates a fixed start/end vertex (typically vertex 1)
2. Finds optimal paths through subsets of vertices
3. Combines these optimal subpaths to form the complete tour

## The Held-Karp Recurrence Relation

### Defining the Subproblem

The algorithm focuses on a specific subproblem:

**minCostPath(S, e)** = Minimum cost path that:
- Starts at vertex 1
- Visits each vertex in set S exactly once
- Ends at vertex e (where e is not in S)

### Relating the TSP to the Subproblem

For a graph with n vertices, the minimum cost TSP tour can be expressed as:

$$\text{minCostTSP} = \min_{e \in \{2,3,...,n\}} \{\text{minCostPath}(\{2,3,...,n\} \setminus \{e\}, e) + C_{e,1}\}$$

Where:
- We try each possible vertex e as the last vertex before returning to 1
- We find the best path through all other vertices ending at e
- We add the cost of returning from e to 1

### Example with n=5
If we have vertices {1,2,3,4,5} with 1 as start/end:

$$\text{minCostTSP} = \min \begin{cases}
\text{minCostPath}(\{2,3,4\}, 5) + C_{5,1} \\
\text{minCostPath}(\{2,3,5\}, 4) + C_{4,1} \\
\text{minCostPath}(\{2,4,5\}, 3) + C_{3,1} \\
\text{minCostPath}(\{3,4,5\}, 2) + C_{2,1} \\
\end{cases}$$

## Recursive Structure for minCostPath

The key recurrence relation for the Held-Karp algorithm:

$$\text{minCostPath}(S, e) = \min_{s \in S} \{\text{minCostPath}(S \setminus \{s\}, s) + C_{s,e}\}$$

This formula captures the optimal substructure:
- If we know the last vertex s in S before reaching e
- We can reduce the problem to finding the best path through S-{s} ending at s
- Then add the cost of going from s to e

### Example of the Recurrence
For minCostPath({2,3,4}, 5):

$$\text{minCostPath}(\{2,3,4\}, 5) = \min \begin{cases}
\text{minCostPath}(\{3,4\}, 2) + C_{2,5} \\
\text{minCostPath}(\{2,4\}, 3) + C_{3,5} \\
\text{minCostPath}(\{2,3\}, 4) + C_{4,5} \\
\end{cases}$$

## Base Cases

1. **Single vertex in S**: 
   $$\text{minCostPath}(\{i\}, e) = C_{1,i} + C_{i,e}$$

2. **Empty set S**:
   $$\text{minCostPath}(\emptyset, e) = C_{1,e}$$

## Memoization Strategy

The Held-Karp algorithm uses memoization to avoid redundant calculations:

1. Create a table indexed by (S, e) pairs where:
   - S is a subset of vertices {2,3,...,n}
   - e is a vertex not in S

2. Fill the table bottom-up:
   - Start with small subsets
   - Use previously computed values for larger subsets

### Table Size Analysis
- Number of possible subsets: 2^(n-1)
- Number of possible end vertices: n
- Total table entries: O(n × 2^(n-1))
- Cost to compute each entry: O(n)
- Overall time complexity: O(n² × 2^n)

## Solution Reconstruction

To reconstruct the optimal TSP tour:

1. For each table entry, store which vertex s gave the minimum value
2. At the top level, identify which endpoint e gave the minimum TSP cost
3. Recursively trace back through the table entries to build the complete tour
4. The tour will be constructed in reverse order (from end to start)

## Algorithm Summary

1. Define the minCostPath subproblem
2. Use the recurrence relation to fill a memoization table
3. Compute the final TSP cost from the table
4. Reconstruct the optimal tour

## Practical Considerations

- While O(n² × 2^n) is still exponential, it's substantially better than the O(n × n!) brute force approach
- In practice, integer linear programming formulations may be faster for some instances
- The algorithm can be extended to handle asymmetric TSP

## Conclusion

The Held-Karp algorithm demonstrates the power of dynamic programming in reducing computational complexity through clever problem decomposition and optimal substructure identification. Despite being developed decades ago, it remains one of the fastest exact algorithms for solving the TSP in terms of worst-case complexity bounds.