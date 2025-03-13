# Lecture 13: Approximation Algorithms for MAXSAT Problems

## Introduction to MAX-3-SAT

MAX-3-SAT is a variant of the Boolean Satisfiability problem where we aim to maximize the number of satisfied clauses rather than requiring all clauses to be satisfied.

### Problem Definition

- We have $n$ boolean variables: $x_1, x_2, \ldots, x_n$
- Each variable can take on either `true` or `false` values
- We have $m$ clauses, where each clause contains exactly 3 literals
- A literal can be either a variable or its negation
- Example clauses:
  - $C_1: x_1 \lor \overline{x_2} \lor \overline{x_3}$
  - $C_2: \overline{x_4} \lor x_3 \lor x_2$
  - $C_3: \overline{x_4} \lor \overline{x_2} \lor x_3$

### Objective

The goal in MAX-3-SAT is to find a truth assignment that satisfies the maximum number of clauses.

### Complexity
- MAX-3-SAT is NP-complete
- Can be reduced to an integer linear program (ILP)

## 7/8-Approximation Algorithm for MAX-3-SAT

We'll develop an approximation algorithm that guarantees a solution within 7/8 of the optimal solution.

### Approximation Guarantee

For a MAX-3-SAT problem with optimal solution $OPT$, our algorithm will find a solution that satisfies at least $\frac{7}{8} \cdot OPT$ clauses.

Example: If the optimal solution satisfies 80 clauses, our approximation algorithm will satisfy at least 70 clauses.

### Random Assignment Analysis

First, let's analyze how a random truth assignment performs:

1. Suppose we assign each variable randomly with probability 1/2 for true and 1/2 for false
2. For any clause with 3 distinct literals, what is the probability it's satisfied?
   - A clause is NOT satisfied only when all its literals evaluate to false
   - For a 3-literal clause, this probability is $\frac{1}{8}$
   - Therefore, the probability a clause IS satisfied is $1 - \frac{1}{8} = \frac{7}{8}$

3. Using indicator variables and linearity of expectation:
   - Expected number of satisfied clauses = $\sum_{j=1}^{m} P(\text{clause }j\text{ is satisfied}) = \frac{7}{8} \cdot m$

So a random assignment satisfies 7/8 of the clauses on average. However, this is just an expected value - a specific random assignment might perform worse.

### Derandomization: The Method of Conditional Expectations

We can convert this probabilistic approach into a deterministic algorithm using the method of conditional expectations:

#### Key Insight:
If the average performance is 7/8, then there must exist at least one assignment that performs at least as well as the average.

#### Algorithm:
1. Start with the original formula $\phi$
2. For each variable $x_i$ (i = 1 to n):
   - Consider setting $x_i = \text{true}$ and compute expected number of satisfied clauses
   - Consider setting $x_i = \text{false}$ and compute expected number of satisfied clauses
   - Choose the assignment that gives the higher expectation
   - Fix this value for $x_i$ and continue with the next variable

#### Proof of Correctness:
- Let $E[\phi]$ = expected number of clauses satisfied in original formula = $\frac{7}{8} \cdot m$
- When deciding on variable $x_1$:
  - Let $\phi_1$ = formula with $x_1 = \text{true}$
  - Let $\phi_2$ = formula with $x_1 = \text{false}$
  - Then $E[\phi] = \frac{1}{2}E[\phi_1] + \frac{1}{2}E[\phi_2]$
  - Since $E[\phi] = \frac{7}{8} \cdot m$, at least one of $E[\phi_1]$ or $E[\phi_2]$ must be $\geq \frac{7}{8} \cdot m$
  - We select the option with the higher expectation

- This process continues for all variables, maintaining the invariant that the expected number of satisfied clauses is always $\geq \frac{7}{8} \cdot m$
- At the end, we have a fully determined assignment with no randomness, which satisfies at least $\frac{7}{8} \cdot m$ clauses

## Example Application

Consider a MAX-3-SAT problem with 4 variables ($x_1, x_2, x_3, x_4$) and 5 clauses:
- $C_1: x_1 \lor \overline{x_2} \lor x_3$
- $C_2: \overline{x_1} \lor x_2 \lor x_3$
- $C_3: x_1 \lor \overline{x_3} \lor x_4$
- $C_4: \overline{x_1} \lor x_3 \lor \overline{x_4}$
- $C_5: \overline{x_2} \lor \overline{x_3} \lor \overline{x_4}$

### Step 1: Consider $x_1$
- If $x_1 = \text{true}$:
  - $C_1$ is satisfied (expectation = 1)
  - $C_2$ becomes $x_2 \lor x_3$ (expectation = 3/4)
  - $C_3$ is satisfied (expectation = 1)
  - $C_4$ becomes $x_3 \lor \overline{x_4}$ (expectation = 3/4)
  - $C_5$ remains $\overline{x_2} \lor \overline{x_3} \lor \overline{x_4}$ (expectation = 7/8)
  - Total expectation: $1 + 3/4 + 1 + 3/4 + 7/8 = 4.375$

- If $x_1 = \text{false}$:
  - $C_1$ becomes $\overline{x_2} \lor x_3$ (expectation = 3/4)
  - $C_2$ is satisfied (expectation = 1)
  - $C_3$ becomes $\overline{x_3} \lor x_4$ (expectation = 3/4)
  - $C_4$ is satisfied (expectation = 1)
  - $C_5$ remains $\overline{x_2} \lor \overline{x_3} \lor \overline{x_4}$ (expectation = 7/8)
  - Total expectation: $3/4 + 1 + 3/4 + 1 + 7/8 = 4.375$

Since both assignments have equal expectations, we can choose either. Let's select $x_1 = \text{true}$.

### Step 2: Consider $x_3$ (with $x_1 = \text{true}$)
- If $x_3 = \text{false}$:
  - $C_1$ is satisfied (expectation = 1)
  - $C_2$ becomes $x_2$ (expectation = 1/2)
  - $C_3$ is satisfied (expectation = 1)
  - $C_4$ becomes $\overline{x_4}$ (expectation = 1/2)
  - $C_5$ becomes $\overline{x_2} \lor \overline{x_4}$ (expectation = 3/4)
  - Total expectation: $1 + 1/2 + 1 + 1/2 + 3/4 = 3.75$

- If $x_3 = \text{true}$:
  - $C_1$ is satisfied (expectation = 1)
  - $C_2$ is satisfied (expectation = 1)
  - $C_3$ is satisfied (expectation = 1)
  - $C_4$ is satisfied (expectation = 1)
  - $C_5$ becomes $\overline{x_2} \lor \overline{x_4}$ (expectation = 3/4)
  - Total expectation: $1 + 1 + 1 + 1 + 3/4 = 4.75$

Since setting $x_3 = \text{true}$ gives a higher expectation, we choose $x_3 = \text{true}$.

### Step 3: Consider $x_2$ (with $x_1 = \text{true}$, $x_3 = \text{true}$)
- If $x_2 = \text{false}$, all five clauses are satisfied without considering $x_4$
- If $x_2 = \text{true}$, fewer clauses would be satisfied

Therefore, we set $x_2 = \text{false}$.

### Final Assignment:
- $x_1 = \text{true}$
- $x_2 = \text{false}$
- $x_3 = \text{true}$
- $x_4$ can be either true or false (doesn't affect the solution)

This assignment satisfies all 5 clauses, which is optimal for this example.

## Analogy: Finding Above-Average Height

An analogy to understand this algorithm:
- Imagine a classroom where the average height is 5'8"
- To find someone taller than average, we can:
  1. Divide the class into two halves
  2. Calculate the average height of each half
  3. Select the half with the higher average (e.g., 5'10" vs. 5'6")
  4. Divide that half further
  5. Repeat until we find individuals above the original average

Similarly, our algorithm divides the space of truth assignments and repeatedly selects the better half until we find a specific assignment that performs at least as well as the expected 7/8 approximation.

## Summary

- MAX-3-SAT is an NP-complete problem where we aim to maximize the number of satisfied clauses
- A random assignment satisfies 7/8 of the clauses in expectation
- Using the method of conditional expectations, we can derandomize this into a deterministic algorithm
- This gives us a 7/8-approximation algorithm that runs in polynomial time
- The algorithm makes greedy choices for each variable, always maintaining the invariant that the expected performance is at least 7/8 of optimal