# Lecture 15: Introduction to the Traveling Salesman Problem (TSP)

## Overview
This lecture introduces the Traveling Salesman Problem (TSP), a fundamental problem in combinatorial optimization with significant practical applications. We'll explore the problem definition, its variants, real-world applications, and a preview of the algorithms we'll study to solve it.

## Problem Definition

### Basic TSP Formulation
- **Input**: A complete graph $G = (V, E)$ with $n$ vertices
  - Complete graph means there's an edge between every pair of vertices
  - Each edge $(i, j)$ has an associated cost/weight $w_{ij}$
  
- **Goal**: Find the optimal tour (also called TSP tour) that:
  1. Starts from any vertex
  2. Visits all vertices exactly once (except the starting vertex)
  3. Returns to the starting vertex
  4. Minimizes the total cost of the tour

### Example
Consider a 5-vertex complete graph with the following edge costs:
- Blue edges: cost 1
- Pink edges: cost 2

**Tour 1**: 1 → 2 → 4 → 5 → 3 → 1
- Cost: 1 + 1 + 1 + 2 + 1 = 6

**Tour 2**: 1 → 3 → 2 → 4 → 5 → 1
- Cost: 1 + 1 + 1 + 1 + 1 = 5 (optimal)

## Variants of TSP

### Symmetric TSP
- In a symmetric TSP, the cost of traveling from vertex $i$ to $j$ is the same as traveling from $j$ to $i$
- Mathematically: $w_{ij} = w_{ji}$ for all pairs $(i,j)$
- Represented as an undirected complete graph

### Asymmetric TSP
- In an asymmetric TSP, the cost of traveling from vertex $i$ to $j$ may differ from traveling from $j$ to $i$
- Mathematically: $w_{ij}$ may not equal $w_{ji}$
- Better represented as a directed complete graph
- Two directed edges between each pair of vertices with potentially different costs

### Mathematical Representation
TSP can be represented using a cost matrix $C$ where:
- $C$ is an $n \times n$ matrix
- $c_{ij}$ represents the cost of traveling from vertex $i$ to vertex $j$
- For symmetric TSP: $c_{ij} = c_{ji}$ (the matrix is symmetric)
- For asymmetric TSP: $c_{ij}$ may not equal $c_{ji}$

## Real-World Applications

### Logistics and Delivery
- Package delivery services (FedEx, UPS, Amazon)
- Optimizing routes for delivery vehicles
- Addresses in a city represented as vertices
- Travel distances or times between addresses as edge costs
- One-way streets can create asymmetric costs
- Starting point is typically a depot, where the delivery vehicle begins and ends its route

### Manufacturing
- Printed circuit board (PCB) manufacturing
- Soldering iron needs to visit multiple points on a circuit board
- Points that need soldering are vertices
- Distances between points are edge costs
- Optimizing the movement path of the soldering arm
- Even small improvements (1 second per board) can lead to significant efficiency gains (1000+ more boards per day)

### Other Applications
- Robotics and planning
- Vehicle routing problems (extension to multiple vehicles)
- Various logistics and transportation applications

## Computational Complexity

TSP is an NP-complete problem, which means:
- Unless P = NP, we do not know efficient exact solutions
- Exact algorithms will be inefficient for large instances
- We need to explore approximation algorithms and heuristics

## Course Roadmap for TSP

1. **NP-completeness proof**
   - Demonstrate why TSP is computationally hard

2. **Exact Algorithms**
   - Brute force approach
   - Dynamic programming solutions
   - Integer Linear Programming formulations

3. **Approximation Algorithms**
   - Christofides' algorithm (3/2-approximation for symmetric TSP)
   - Theoretical guarantees on solution quality

4. **Heuristics**
   - Practical approaches for solving TSP
   - Brief overview of common heuristics

## Key Points to Remember

- TSP is a fundamental optimization problem with numerous applications
- The goal is to find a minimum-cost tour visiting all vertices exactly once
- Symmetric TSP has equal costs in both directions; asymmetric TSP doesn't
- TSP is NP-complete, making exact solutions computationally expensive
- For practical applications, approximation algorithms and heuristics are essential
- In the next lecture, we'll examine the computational complexity of TSP in detail