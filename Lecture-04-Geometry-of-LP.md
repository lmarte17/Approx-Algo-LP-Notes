# Lecture 4: Geometry of Linear Programming

## Introduction to the Geometric View of LP

Linear Programming problems can be better understood by examining their geometric interpretation. This lecture explores how LP constraints and objectives manifest geometrically, providing intuition about the nature of solutions.

## Geometric Representation of a Simple LP

### Example Problem

Consider the following linear programming problem:

**Maximize:** $x_1 - 2x_2$

**Subject to:**
- $x_1 \geq 0$
- $x_2 \geq 0$
- $3x_1 + 2x_2 \leq 6$
- $2x_1 + 3x_2 \leq 6$

### Visualizing Constraints in 2D

When we have only two decision variables ($x_1$ and $x_2$), we can visualize the problem in a 2D coordinate system:

1. **Non-negativity constraints:**
   - $x_1 \geq 0$ represents everything to the right of the $x_2$ axis
   - $x_2 \geq 0$ represents everything above the $x_1$ axis
   - Together, these constraints define the first quadrant of the coordinate system

2. **First inequality constraint:**
   - $3x_1 + 2x_2 \leq 6$ forms a straight line connecting points $(2,0)$ and $(0,3)$
   - The feasible region includes all points below and to the left of this line

3. **Second inequality constraint:**
   - $2x_1 + 3x_2 \leq 6$ forms a straight line connecting points $(3,0)$ and $(0,2)$
   - The feasible region includes all points below and to the right of this line

### The Feasible Region

The **feasible region** is the set of all points that satisfy all constraints simultaneously. In our example, this forms a quadrilateral (4-sided polygon) with vertices at:
- $(0,0)$
- $(0,2)$
- $(\frac{6}{5}, \frac{6}{5})$ - calculated by solving the system of equations $3x_1 + 2x_2 = 6$ and $2x_1 + 3x_2 = 6$
- $(2,0)$

This polygon represents all possible solutions that satisfy our constraints.

## Visualizing the Objective Function

The objective function $x_1 - 2x_2$ can be represented as a family of parallel lines:

- Each line represents points with the same objective value (level sets)
- For example:
  - The line where $x_1 - 2x_2 = 0$ passes through $(0,0)$ and $(2,1)$
  - The line where $x_1 - 2x_2 = -\frac{6}{5}$ passes through $(\frac{6}{5}, \frac{6}{5})$
  - The line where $x_1 - 2x_2 = -4$ passes through another part of the feasible region
  - The line where $x_1 - 2x_2 = 1$ represents a higher objective value
  - The line where $x_1 - 2x_2 = 2$ touches the feasible region at exactly one point: $(2,0)$

### Finding the Optimal Solution

When maximizing, we "sweep" these parallel lines in the direction of increasing objective value until the line barely touches the feasible region at a single point.

In our example:
- The point $(2,0)$ is where the objective function achieves its maximum value of 2
- If we tried to move to a higher objective value (such as 2.5), the corresponding line would no longer intersect the feasible region

Therefore, the optimal solution to this LP is $x_1^* = 2, x_2^* = 0$.

## Generalizing to Higher Dimensions

### Convex Polyhedra

When we extend this geometric view to higher dimensions:

- With $n$ decision variables, the feasible region exists in $n$-dimensional space
- Each constraint forms a **hyperplane** instead of a line
- Each inequality constraint defines a **half-space** (everything on one side of the hyperplane)
- The feasible region is the intersection of all these half-spaces
- This intersection forms a geometric shape called a **convex polyhedron**

### Definition of Convexity

A polyhedron is **convex** if for any two points $A$ and $B$ inside the polyhedron, the entire straight line connecting $A$ and $B$ is also inside the polyhedron.

Examples:
- A square, rectangle, or triangle is convex
- A shape with "dents" or "notches" is non-convex

**Important property:** The feasible region of any linear program is always a convex polyhedron (when non-empty).

### Objective Function in Higher Dimensions

In higher dimensions:
- The level sets of the objective function form hyperplanes
- We still "sweep" these hyperplanes in the direction of increasing objective value
- The optimal solution occurs at the point where the hyperplane last touches the feasible region

## Unbounded Linear Programs

### Example of an Unbounded LP

Consider the problem:

**Maximize:** $x_1 + x_2$

**Subject to:**
- $x_1 \geq 3$
- $x_2 \geq 5$

Geometrically:
- The constraint $x_1 \geq 3$ forms a vertical line at $x_1 = 3$ with the feasible region to the right
- The constraint $x_2 \geq 5$ forms a horizontal line at $x_2 = 5$ with the feasible region above
- The resulting feasible region is unbounded, extending infinitely to the northeast

In this case, the objective function $x_1 + x_2$ can increase without bound, as the level sets can be pushed infinitely far while still intersecting the feasible region.

## Conclusion

Understanding the geometry of linear programming provides insight into:
- How constraints shape the solution space
- Why optimal solutions typically occur at vertices of the feasible region
- The nature of unbounded problems
- The foundation for algorithms like the Simplex method (to be covered in the next lecture)

The geometric interpretation is crucial for developing intuition about linear programs and their solutions, even when working with higher-dimensional problems that cannot be visualized directly.
