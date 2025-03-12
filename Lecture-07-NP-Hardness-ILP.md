# Lecture 7: NP Hardness of Integer Linear Programming (ILP)

## Introduction

This lecture focuses on proving that Integer Linear Programming (ILP) is NP-complete. We will revisit the concept of NP-completeness from previous coursework and demonstrate why ILP is fundamentally more difficult than standard Linear Programming.

## Integer Linear Programming: Definition

An Integer Linear Program can be formulated as:

**Maximize:** $c_1x_1 + c_2x_2 + ... + c_nx_n$

**Subject to constraints:**
- $a_{11}x_1 + a_{12}x_2 + ... + a_{1n}x_n \leq b_1$
- $a_{21}x_1 + a_{22}x_2 + ... + a_{2n}x_n \leq b_2$
- ...
- $a_{m1}x_1 + a_{m2}x_2 + ... + a_{mn}x_n \leq b_m$

**With the critical constraint:** $x_1, x_2, ..., x_n \in \mathbb{Z}$ (all variables must be integers)

## Decision Version of ILP

To prove NP-completeness, we consider the decision version of the problem:

**Question:** Is there a solution that satisfies all constraints such that the objective function value is at least $k$?

This transforms the optimization problem into a yes/no problem, which is appropriate for NP-completeness analysis.

## Binary ILP: A Restricted Version

We'll prove NP-completeness for an even more restricted version of ILP:

**Binary ILP (0-1 ILP):** All variables are restricted to be either 0 or 1.

This can be formulated by adding constraints:
- $0 \leq x_i \leq 1$ for all $i$
- $x_i \in \mathbb{Z}$ for all $i$

Which effectively means $x_i \in \{0,1\}$.

## Proving NP-Completeness

### Step 1: Proving Binary ILP is in NP

A problem is in NP if:
- Given a certificate (a potential solution)
- We can verify in polynomial time whether it's correct

For Binary ILP, the certificate is simply the values of $x_1, x_2, ..., x_n$.
Given these values, we can efficiently check:
1. Whether all constraints are satisfied
2. Whether the objective function value is at least $k$
3. Whether all variables are 0 or 1

Since all these checks can be done in polynomial time, Binary ILP is in NP.

### Step 2: Proving NP-Hardness via Reduction

We'll reduce 3-SAT, a known NP-complete problem, to Binary ILP.

#### 3-SAT Problem:
- Given propositions $p_1, p_2, ..., p_n$ that can be true or false
- And $m$ clauses, each containing exactly 3 literals (a proposition or its negation)
- Determine if there exists a truth assignment that satisfies all clauses

#### Example of 3-SAT:
- Clause 1: $p_1 \lor p_3 \lor \neg p_7$
- Clause 2: $p_3 \lor p_8 \lor \neg p_4$
- Clause 3: $p_5 \lor \neg p_9 \lor \neg p_4$
- ...and so on

#### Reduction Process:

1. **Create decision variables:**
   - For each proposition $p_i$, create a binary variable $x_i$
   - $x_i = 0$ represents $p_i$ is false
   - $x_i = 1$ represents $p_i$ is true

2. **Create clause indicator variables:**
   - For each clause $c_j$, create a binary variable $t_j$
   - $t_j = 0$ means clause $c_j$ is not satisfied
   - $t_j = 1$ means clause $c_j$ is satisfied

3. **Set up the objective function:**
   - Maximize $t_1 + t_2 + ... + t_m$ (the number of satisfied clauses)

4. **Translate each clause into a constraint:**
   - For clause $c_j$, create a constraint that ensures $t_j$ can only be 1 if at least one literal in the clause is true
   
   Examples:
   - For clause 1 ($p_1 \lor p_3 \lor \neg p_7$): $x_1 + x_3 + (1-x_7) \geq t_1$
   - For clause 2 ($p_3 \lor p_8 \lor \neg p_4$): $x_3 + x_8 + (1-x_4) \geq t_2$
   - For clause 3 ($p_5 \lor \neg p_9 \lor \neg p_4$): $x_5 + (1-x_9) + (1-x_4) \geq t_3$

5. **Set the threshold value:**
   - Ask if there is a solution with objective value $\geq m$
   - Since $m$ is the total number of clauses, this is equivalent to asking if all clauses can be satisfied

#### Key Insight:
- For a clause like $p_1 \lor p_3 \lor \neg p_7$, the constraint $x_1 + x_3 + (1-x_7) \geq t_1$ ensures:
  - If all literals are false, the left side evaluates to 0, forcing $t_1 = 0$
  - If any literal is true, the left side is at least 1, allowing $t_1 = 1$
  - Since we're maximizing the sum of all $t_j$, there's an incentive to set as many $t_j$ to 1 as possible

#### Completeness of the Reduction:
- Any satisfying assignment for the 3-SAT problem can be directly mapped to a solution for the Binary ILP with objective value $m$
- Conversely, any solution to the Binary ILP with objective value $m$ can be mapped back to a satisfying assignment for the 3-SAT problem

Therefore, Binary ILP is NP-complete, and by extension, general ILP is NP-hard.

## Implications for Solving ILP

Unlike Linear Programming, which can be solved in polynomial time using:
- Simplex algorithm (fast in practice but exponential in worst case)
- Interior point methods
- Ellipsoidal method (Khachiyan's algorithm)

Integer Linear Programming:
- Has no known polynomial-time algorithm
- If such an algorithm were found, it would prove P = NP
- Current algorithms rely on techniques like branch and bound, and cutting planes
- These algorithms are exponential in the worst case

## Conclusion

- ILP is NP-hard, making it fundamentally more difficult than standard LP
- No known polynomial-time algorithms exist for solving ILP
- Existing algorithms have exponential worst-case running times
- In future lectures, we'll explore specific algorithmic problems that can be formulated as ILPs