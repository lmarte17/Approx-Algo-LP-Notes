# Lecture 2: Solving Linear Programming

## Introduction to Solving Linear Programs

When solving linear programs, we provide our LP solver with a formulation that includes:
- An objective function
- A set of constraints
- Decision variables

### Example: Cake Sharing Problem

Consider our previous example involving cutting a cake and sharing it between Alice, Bob, and Charlie while maximizing revenue for charity:

**Objective function:**
$$\text{maximize } 10x_1 + 12x_2 + 15x_3$$

**Subject to constraints:**
- Alice wants as much cake as Bob
- Bob wants at least twice as much cake as Charlie
- Charlie wants at least 1/10 of the cake
- The total amount of cake is 1 (they share one whole cake)
- Bob cannot have more than half the cake

### Optimal Solution

When we solve this LP, the solver returns an optimal solution:
- $x_1^* = 1/2$ (Alice's portion)
- $x_2^* = 1/2$ (Bob's portion)
- $x_3^* = 1/4$ (Charlie's portion)

The objective value achieved by this solution is:
$$10 \cdot \frac{1}{2} + 12 \cdot \frac{1}{2} + 15 \cdot \frac{1}{4} = 5 + 6 + 3.75 = 14.75$$

This is the best possible value - any other solution satisfying all constraints will achieve an objective value less than or equal to 14.75.

## Types of LP Solutions

### 1. Infeasible LPs

An LP solver doesn't always produce an optimal solution. Consider a modification to our problem:

- Alice still wants as much cake as Bob
- Bob wants at least twice as much cake as Charlie
- Charlie now insists on having at least 1/3 of the cake (instead of 1/10)

This new formulation creates an issue: **there is no solution**. The constraints are mutually contradictory:

- Charlie wants at least 1/3 of the cake
- Bob wants at least twice as much as Charlie → Bob needs at least 2/3 of the cake
- Alice wants at least as much cake as Bob → Alice needs at least 2/3 of the cake
- Total: 2/3 + 2/3 + 1/3 = 5/3 cakes (but we only have 1 cake)

When constraints cannot be satisfied simultaneously, the LP is called **infeasible**.

### 2. Unbounded LPs

Let's modify our original problem again:
- Remove the constraint that Bob cannot have more than half the cake
- Remove the constraint that they must share one whole cake (unlimited cake)
- Keep the other constraints:
  - Alice needs at least as much cake as Bob
  - Bob wants at least twice as much cake as Charlie
  - Charlie wants at least 1/10 of the cake

In this formulation, there's no bound on how big $x_1$, $x_2$, and $x_3$ can grow. For example:
- $x_1 = 10^{15}$
- $x_2 = 10^{15}$
- $x_3 = \frac{1}{2} \cdot 10^{15}$

This satisfies all constraints, but why stop there? The values could grow to infinity, making the objective value unbounded.

This type of problem is called an **unbounded LP** - the constraints can be satisfied, but there's no limit to how large the optimal value can become.

## Components of a Linear Programming Problem

1. **Decision Variables**: $x_1$ through $x_n$ (all real-valued, can take fractional values)

2. **Objective Function**: A linear function to maximize (or minimize)
   - If you want to minimize, you can negate everything and maximize the negation

3. **Constraints**: Linear inequalities in the form $a_1x_1 + a_2x_2 + ... + a_nx_n \leq b$
   - Can also accommodate greater than or equal to (≥) or equality (=) constraints

## Possible LP Solver Outcomes

An LP solver can give one of three answers:

1. **Infeasible**: The constraints are mutually contradictory, and no solution exists

2. **Unbounded**: The problem is feasible, but there's no limit to how large the objective value can become (approaches infinity)

3. **Optimal**: The problem has an optimal solution because it's feasible and the objective is bounded
   - The solver returns the optimal values $x_1^*$ through $x_n^*$ that maximize the objective while satisfying all constraints

## Practical Considerations

In real-world logistical problems:
- **Infeasible** solutions indicate that constraints are not formulated correctly
- **Unbounded** solutions suggest we're missing constraints and need to update our formulation
- **Optimal** solutions are what we typically want to see and interpret

## Next Steps

In the next lecture, we'll discuss how linear programming problems relate to practical algorithmic problems, diving deeper into applications and assignments.

---

*Note: The course includes a tutorial that demonstrates how to set up and solve LPs using PuLP in Python, showing examples of each outcome type and how to interpret the results.*