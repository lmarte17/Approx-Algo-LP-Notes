# Lecture 22: Eulerian Tours

## Summary
This lecture introduces the concept of Eulerian tours as a foundation for improving the approximation algorithm for metric TSPs. Unlike the Hamiltonian cycle problem (visiting every vertex exactly once), an Eulerian tour focuses on traversing every edge exactly once. The lecture covers the historical context of Eulerian tours through the Königsberg Bridge problem, presents necessary and sufficient conditions for a graph to have an Eulerian tour, and briefly outlines how these concepts will be used to improve the 2-factor approximation algorithm for metric TSP.

## Eulerian Tours: Definition and Properties

### Definition
- An **Eulerian tour** is a path in an undirected graph that:
  - Traverses every edge exactly once
  - Starts and ends at the same vertex
  - Note: Unlike Hamiltonian cycles, vertices can be visited multiple times

### Example of a Graph with an Eulerian Tour
Consider a graph with vertices 1, 2, 3, 4, 5 and edges:
- A possible Eulerian tour: 1→3→2→5→4→2→1
- This tour traverses each edge exactly once and returns to the starting point

## Historical Context: The Königsberg Bridge Problem

- First studied by mathematician Leonhard Euler in the 18th century
- Often considered the starting point for graph theory in mathematics
- Original problem: Is it possible to walk through the city of Königsberg, crossing each of its seven bridges exactly once, and return to the starting point?
- Euler proved this was impossible by representing the problem as a graph

## Necessary and Sufficient Conditions for Eulerian Tours

### Theorem
A connected undirected graph has an Eulerian tour if and only if every vertex has an even degree (even number of incident edges).

### Proof Intuition
- For any vertex in the middle of the tour (not start/end):
  - Every time you enter the vertex, you must leave it
  - This requires an even number of edges
- For start/end vertex:
  - You leave once at the beginning and enter once at the end
  - Still requires an even number of edges

### Example of a Graph Without an Eulerian Tour
- Consider a graph where vertices B and D each have three incident edges
- The odd number of edges means that after entering and leaving once, you'll be stuck with one edge that can't be properly traversed
- This makes an Eulerian tour impossible

### Converting a Non-Eulerian Graph to an Eulerian Graph
- By adding an edge between the vertices with odd degrees (e.g., connecting B and D)
- After this addition, every vertex has an even degree
- The resulting graph now has an Eulerian tour

## Constructing an Eulerian Tour

### Algorithm Outline
1. Start at any vertex (if all vertices have even degree)
2. Follow edges, marking each as you traverse it
3. When you reach a vertex with no unmarked edges, you're stuck at the start vertex
4. If unmarked edges remain in the graph:
   - Find a vertex in your current tour that has unmarked edges
   - Start a new tour from that vertex
   - When you return to that vertex, splice this new tour into your original tour

### Important Properties
- The algorithm runs in polynomial time (similar to depth-first search)
- If every vertex has an even degree, the algorithm always finds an Eulerian tour
- The algorithm requires some bookkeeping when splicing tours together

## Relationship to Metric TSP Approximation

- Eulerian tours will be used to improve the 2-factor approximation algorithm for metric TSP
- The connection involves:
  - Converting a minimum spanning tree (MST) into a graph where every vertex has even degree
  - Finding an Eulerian tour of this modified graph
  - Short-cutting the Eulerian tour to create a TSP tour

## Key Takeaways

1. **Eulerian tour**: Visit every edge exactly once and return to the starting point
2. **Necessary and sufficient condition**: Every vertex must have an even degree
3. **Contrast with Hamiltonian cycle**: Eulerian tours focus on edges, Hamiltonian cycles focus on vertices
4. **Polynomial-time algorithm**: Eulerian tours can be found efficiently, unlike Hamiltonian cycles which are NP-complete
5. **Application**: Foundation for improving the approximation ratio for metric TSP from 2 to 3/2

In the next lecture, we'll see how Eulerian tours are utilized in Christofides' algorithm to achieve a 3/2-factor approximation for metric TSP.
