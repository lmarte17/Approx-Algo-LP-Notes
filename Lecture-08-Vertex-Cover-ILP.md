# Lecture 8: Vertex Cover as an ILP

## Introduction to Vertex Cover

A **vertex cover** is a fundamental problem in graph theory with important applications in algorithm design and optimization.

### Definition

Given an undirected graph $G = (V, E)$:
- A vertex cover is a subset of vertices $S \subseteq V$
- For every edge $(u, v) \in E$, at least one of the endpoints ($u$ or $v$) must be in $S$
- The goal is to find the smallest possible subset $S$ that covers all edges

## Example Graph

Consider an undirected graph with 6 vertices (labeled 1-6) connected by edges.

### First Attempt at a Cover

If we select vertices $\{3, 4, 5\}$:
- Edge $(1,4)$ is covered by vertex 4
- Edge $(2,4)$ is covered by vertex 4
- Edge $(4,5)$ is covered by vertices 4 and 5
- Edge $(5,6)$ is covered by vertex 5
- Edge $(4,6)$ is covered by vertex 4
- Edge $(3,6)$ is covered by vertex 3

This is a valid vertex cover because every edge has at least one endpoint in our selected set $\{3, 4, 5\}$.

### Optimal Cover

We can do better than the 3-vertex solution. The graph can actually be covered using just 2 vertices:

If we select vertices $\{4, 6\}$:
- All edges connected to vertex 4 are covered by vertex 4
- The remaining edges $(3,6)$ and $(5,6)$ are covered by vertex 6

Therefore, $\{4, 6\}$ is a smaller (optimal) vertex cover for this graph.

## Formulating Vertex Cover as an Integer Linear Program (ILP)

### Decision Variables

For each vertex $v_i$ in the graph, we define a binary variable $x_i$:
- $x_i = 0$ if vertex $v_i$ is NOT in the cover
- $x_i = 1$ if vertex $v_i$ IS in the cover

### Objective Function

We want to minimize the total number of vertices in our cover:

$$\text{Minimize } \sum_{i=1}^{n} x_i$$

Where $n$ is the number of vertices in the graph.

### Constraints

For each edge $(u, v) \in E$, we need at least one endpoint to be in the cover:

$$x_u + x_v \geq 1$$

For example, in our graph:
- For edge $(1,4)$: $x_1 + x_4 \geq 1$
- For edge $(2,4)$: $x_2 + x_4 \geq 1$
- For edge $(5,6)$: $x_5 + x_6 \geq 1$
- And so on for every edge

### Complete ILP Formulation

For a graph $G = (V, E)$:

$$\text{Minimize } \sum_{v \in V} x_v$$

Subject to:
$$x_u + x_v \geq 1 \quad \forall (u, v) \in E$$
$$x_v \in \{0, 1\} \quad \forall v \in V$$

## Relaxing the Integrality Constraint

An interesting property of the vertex cover ILP formulation is what happens when we relax the integrality constraint:
- Instead of $x_v \in \{0, 1\}$, we allow $x_v \in [0, 1]$
- This converts the ILP into a standard Linear Program (LP)
- The integrality gap between the ILP and LP solutions is bounded
- This property will be explored further in the next module on approximation algorithms for vertex cover

## Conclusion

The Vertex Cover problem is an important NP-hard problem that can be formulated as an Integer Linear Program. This formulation gives us:
- A clear mathematical representation of the problem
- A pathway to exact solutions (for small instances)
- The foundation for approximation algorithms (for larger instances)

In the next module, we will explore approximation algorithms for the vertex cover problem based on this ILP formulation.