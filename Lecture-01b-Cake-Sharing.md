# Approximation Algorithms and Linear Programming
## Lecture 1b: Cake Sharing Problem

### Introduction to LP Modeling: The Cake Sharing Problem

The cake sharing problem serves as an introduction to modeling real-world scenarios using linear programming (LP).

### Problem Statement

**Scenario**: Three players (Alice, Bob, and Charlie) need to share one cake

**Constraints**:
1. Alice wants at least as much cake as Bob
2. Bob wants at least twice as much cake as Charlie
3. Charlie wants at least one-tenth of the cake
4. Bob's doctor ordered him to have no more than half the cake

**Charity Payments**:
- Alice: willing to pay $10 for the entire cake (or proportionally)
- Bob: willing to pay $12 for the entire cake (or proportionally)
- Charlie: willing to pay $15 for the entire cake (or proportionally)

**Objective**: Divide the cake to maximize the total amount paid to charity while respecting all constraints

### LP Formulation

#### Decision Variables
- xₐ = fraction of cake that goes to Alice
- x𝙱 = fraction of cake that goes to Bob
- x𝙲 = fraction of cake that goes to Charlie

#### Objective Function
Maximize: 10xₐ + 12x𝙱 + 15x𝙲

#### Constraints
1. xₐ ≥ x𝙱 (Alice gets at least as much as Bob)
2. x𝙱 ≥ 2x𝙲 (Bob gets at least twice as much as Charlie)
3. x𝙲 ≥ 0.1 (Charlie gets at least 1/10 of the cake)
4. x𝙱 ≤ 0.5 (Bob's doctor's orders)
5. xₐ + x𝙱 + x𝙲 = 1 (The entire cake is distributed)
6. xₐ, x𝙱, x𝙲 ≥ 0 (Non-negativity constraints)
7. xₐ, x𝙱, x𝙲 ≤ 1 (Upper bound constraints - redundant given constraint #5)

### Solution Approach

**Trial and Error Method**:
- Set Charlie's portion first: x𝙲
- Bob gets twice as much: x𝙱 = 2x𝙲
- Alice gets the same as Bob: xₐ = x𝙱 = 2x𝙲

Using constraint #5:
- x𝙲 + 2x𝙲 + 2x𝙲 = 1
- 5x𝙲 = 1
- x𝙲 = 1/5

Therefore:
- x𝙲 = 1/5 (Charlie gets 1/5 of the cake)
- x𝙱 = 2/5 (Bob gets 2/5 of the cake)
- xₐ = 2/5 (Alice gets 2/5 of the cake)

**Verification**:
- xₐ ≥ x𝙱? Yes, 2/5 = 2/5 ✓
- x𝙱 ≥ 2x𝙲? Yes, 2/5 ≥ 2(1/5) ✓
- x𝙲 ≥ 0.1? Yes, 1/5 > 1/10 ✓
- x𝙱 ≤ 0.5? Yes, 2/5 < 1/2 ✓
- xₐ + x𝙱 + x𝙲 = 1? Yes, 2/5 + 2/5 + 1/5 = 1 ✓
- All values ≥ 0? Yes ✓

**Objective Value**:
- Total charity amount = 10(2/5) + 12(2/5) + 15(1/5)
- = 4 + 4.8 + 3
- = $11.80

### Optimality Analysis

The solution is optimal, which can be shown through informal analysis:

1. **If we increase Bob's share by ε₁**:
   - Alice's share must increase by at least ε₂
   - Charlie's share decreases by (ε₁ + ε₂)
   - Change in objective: 12ε₁ + 10ε₂ - 15(ε₁ + ε₂)
   - This is negative, so increasing Bob's share isn't beneficial

2. **If we decrease Bob's share by ε**:
   - Charlie's share must decrease by at least ε/2 (due to constraint #2)
   - Alice's share increases by 3ε/2
   - Change in objective: 10(3ε/2) - 12ε - 15(ε/2)
   - = 15ε - 12ε - 7.5ε = -4.5ε < 0
   - This is also negative, so decreasing Bob's share isn't beneficial

Since neither increasing nor decreasing Bob's share improves the objective value, the current solution is optimal.

### Conclusion

- The cake sharing problem demonstrates how to model a real-world situation as a linear program
- We identified decision variables, formulated an objective function, and established constraints
- A trial solution was verified to be feasible and optimal
- Advanced topics like duality (not covered in this course) would provide additional methods for proving optimality

In future lectures, algorithms and software packages for solving LPs will be discussed.
