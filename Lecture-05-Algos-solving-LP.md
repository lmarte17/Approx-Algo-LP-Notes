# Lecture 5: Algorithms for Solving Linear Programs

## Historical Overview

### Early Development
- **Kantorovich (USSR)**: 
  - One of the first to formulate linear programming
  - Famous Russian economist who won the Nobel Prize (likely in the 1960s)
  - His early work couldn't be published immediately

- **George Dantzig (US)**:
  - Developed linear programming during World War II
  - Published one of the first papers on linear programming in the late 1940s
  - Invented the Simplex Algorithm, one of the most popular LP solving methods

### Algorithmic Significance
- Simplex Algorithm ranks among the top computer algorithms in usage (ranked #2 or #3)
- For comparison, the Fast Fourier Transform is another algorithm on this list

## Major Algorithms for Solving LPs

### 1. Simplex Algorithm (Dantzig, 1940s)
- **Theoretical complexity**: Exponential time in the worst case
  - Can take $O(m^n)$ time or worse (for LPs with m variables and n constraints)
- **Practical performance**: Works in polynomial time for most practical problems
- Still widely used in practice despite theoretical inefficiency

### 2. Ellipsoidal Method (Khachiyan, 1979)
- First theoretically polynomial-time algorithm for LP
- Based on geometric shapes called ellipsoids (deformed spheres)
- Elegant theoretical approach but less practical due to:
  - Numerical issues
  - Often outperformed by Simplex in practice

### 3. Interior Point Methods (1960s, refined in 1980s)
- Karmarkar (1980s) first observed their efficiency for LPs
- Further refined by Nesterov, Nemirovski, and others
- Elegant algorithms with strong theoretical foundations
- Complex analysis but relatively simple implementation

## Understanding the Simplex Algorithm

### Geometric Intuition
- Linear programs can be viewed geometrically:
  - Feasible region is a convex polyhedron (polygon in 2D)
  - Constraints form faces (or edges in 2D) of the polyhedron
  - Corners are called vertices

### Key Simplex Insight: Vertex Optimality
- **Fundamental theorem**: If an optimal solution exists, it can be found at a vertex
- Even with multiple optimal solutions, at least one will be at a vertex
- This allows Simplex to focus only on vertices rather than all points in the polyhedron

### How Simplex Works
1. **Initialization**: Find any vertex of the polyhedron
2. **Iteration**: From current vertex, examine neighboring vertices
3. **Movement**: Move to a neighboring vertex if objective function increases
4. **Termination**: Stop when no neighboring vertex gives better objective value

### Example Walkthrough

Consider the LP:
- Maximize $X_1 - 2X_2$ subject to constraints forming a polygon

Steps:
1. Start at initial vertex $V_0$ (objective value = -4)
2. Move to adjacent vertex $V_1$ at (6/5, 6/5) (objective value = -1.2)
3. Move to vertex at (2, 0) (objective value = 4)
4. Stop since moving to any adjacent vertex would decrease the objective

### Vertices and Adjacency
- In 2D, a vertex is the intersection of two faces (constraints) such that:
  - The point satisfies all other constraints (remains in the polygon)
- Two vertices are adjacent if they share a common face
- Simplex moves along adjacent vertices, increasing the objective at each step

## Other Algorithmic Approaches

### Interior Point Methods
- Start from a point inside the polygon
- Move toward the objective while staying inside the feasible region
- Different philosophical approach compared to Simplex

### Ellipsoidal Methods
- Cover the polyhedron with an ellipsoid
- Iteratively shrink the ellipsoid while still covering the polyhedron
- Theoretically elegant but less practical than Simplex

## Conclusion
The choice of algorithm depends on the specific problem structure:
- Simplex: Widely used due to practical efficiency
- Interior Point: Good for large, sparse problems
- Ellipsoidal: Historically important but rarely used in practice

*Note: The instructor has additional materials with visualizations using matplotlib in Python that provide deeper insight into how these algorithms work, particularly the Simplex algorithm.*