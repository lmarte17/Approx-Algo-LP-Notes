# Lecture 13: Approximation Algorithms for Vertex Covers

## Introduction to Vertex Cover Problem

- **Definition**: Given a graph, a vertex cover is a set of nodes such that every edge in the graph is incident on at least one of the chosen nodes.
- The goal is to find the **smallest possible vertex cover**.
- This problem is NP-hard, making it computationally expensive to find the optimal solution for large graphs.

## Approximation Algorithm Approach

Instead of seeking the optimal solution, we aim to develop efficient algorithms that produce solutions with a guaranteed approximation ratio:

- For vertex cover, we want to find an algorithm that produces a vertex cover that is at most $\alpha$ times larger than the optimal vertex cover.
- Formally: $|VC_{algorithm}| \leq \alpha \cdot |VC_{optimal}|$
- A **factor-2 approximation** means that our algorithm produces a vertex cover that is at most twice the size of the optimal.

### Practical Implications

- For large graphs (e.g., 1,000 nodes, 40,000 edges), finding the optimal solution is computationally infeasible.
- If our approximation algorithm finds a vertex cover of size 100, we can guarantee that the optimal solution is at least 50 (given a factor-2 approximation).
- Our algorithm runs very quickly compared to an exact algorithm that might take an extremely long time.

## Greedy Algorithm 1: Maximum Degree Vertex

### Algorithm Description

1. Start with an empty vertex cover
2. While the graph has edges:
   - Choose the vertex with the highest degree (most incident edges)
   - Add it to the vertex cover
   - Remove the vertex and all its incident edges from the graph
3. Return the vertex cover

### Algorithm Implementation

```
let G_0 = original graph G
i = 1
while G_{i-1} has edges:
    V_i = vertex with maximum degree in G_{i-1}
    d_i = degree of V_i
    Add V_i to vertex cover
    G_i = G_{i-1} with V_i and its incident edges removed
    i = i + 1
return vertex cover
```

### Performance Analysis

- This algorithm provides an $O(\log n)$ approximation, where $n$ is the number of vertices.
- For a graph with 100 vertices, the approximation factor is between 6 and 7.
- For a graph with 1,000 vertices, the approximation factor is between 9 and 10.
- The approximation quality degrades as the graph size increases.

### Counter-Example Construction

A class of graphs can be constructed to show that this algorithm performs poorly:

1. Create vertices $a_1, a_2, ..., a_n$ (the optimal vertex cover would include just these)
2. Create a hierarchy of "b" vertices:
   - $b_n$ connected to all $n$ of the "a" vertices
   - $b_{n-1}$ connected to $n-1$ of the "a" vertices
   - For each $i$ from $n$ down to 1, create vertices $b_i^1, b_i^2, ..., b_i^{n/i}$ each connected to $i$ different "a" vertices
3. When the greedy algorithm runs with a tie-breaking preference for "b" vertices:
   - It will choose approximately $n \log n$ vertices
   - While the optimal solution requires only $n$ vertices

## Greedy Algorithm 2: Maximal Matching Approach

### Algorithm Description

1. Start with an empty vertex cover
2. While the graph has edges:
   - Choose any edge in the graph
   - Add both endpoints of the edge to the vertex cover
   - Remove both vertices and all their incident edges
3. Return the vertex cover

### Algorithm Implementation

```
while graph has edges:
    Choose an edge e = (v_1, v_2)
    Add v_1 and v_2 to vertex cover
    Remove v_1, v_2, and all edges incident on them
return vertex cover
```

### Performance Analysis

- This algorithm provides a **factor-2 approximation** guarantee.
- Proof outline:
  - For every edge selected, the algorithm adds 2 vertices to the cover
  - Any valid vertex cover (including the optimal) must include at least 1 vertex from each edge
  - Therefore, for every 2 vertices we add, the optimal solution must add at least 1
  - This gives us the 2-approximation guarantee

- This matching-based algorithm, counter-intuitively, performs better than the maximum degree greedy algorithm.
- On the same counter-example construction, this algorithm would find a vertex cover of size $2n$, compared to the $n \log n$ vertices chosen by the maximum degree algorithm.

## Conclusion

- The vertex cover problem is NP-hard, making approximation algorithms necessary for large graphs.
- The maximal matching approach provides a factor-2 approximation and is very efficient.
- Interestingly, this simple approach outperforms the seemingly more sophisticated maximum degree greedy algorithm.
- Finding an approximation algorithm with a factor better than 2 remains an important open problem.
