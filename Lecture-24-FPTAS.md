# Lecture 24: Fully Polynomial Time Approximation Schemes (FPTAS)

## Summary
This lecture introduces the concept of Fully Polynomial Time Approximation Schemes (FPTAS), a special class of approximation algorithms that can achieve arbitrarily close approximations to the optimal solution. The lecture focuses on developing an FPTAS for the Knapsack problem by cleverly scaling down the item values and applying dynamic programming techniques. This approach allows for a trade-off between solution quality and running time that can be precisely controlled.

## Introduction to FPTAS

### Definition and Properties
- An FPTAS provides a (1+ε)-approximation for minimization problems or a (1-ε)-approximation for maximization problems
- The running time is polynomial in both the input size and 1/ε
- For any ε > 0, the algorithm guarantees:
  * For minimization: Solution ≤ (1+ε)·OPT
  * For maximization: Solution ≥ (1-ε)·OPT
- The smaller ε is chosen, the closer to optimal the solution becomes, but at the cost of increased running time

### Significance
- FPTAS algorithms are rare and valuable
- They allow users to precisely control the trade-off between solution quality and running time
- They often build upon pseudo-polynomial time algorithms (especially dynamic programming)

## The Knapsack Problem Recap

### Problem Definition
- Given:
  * n items: a₁, a₂, ..., aₙ
  * Each item aᵢ has a weight wᵢ and a value vᵢ
  * A weight budget W
- Goal: Find a subset S of items such that:
  * Total weight ≤ W
  * Total value is maximized

### Standard Dynamic Programming Approach
- Define maxValue(i,w) as the maximum value achievable using the first i items with weight limit w
- Base case: maxValue(0,w) = 0 if w ≥ 0, else -∞
- Recurrence relation:
  * maxValue(i+1,w) = max(maxValue(i,w), vᵢ₊₁ + maxValue(i,w-wᵢ₊₁))
- Running time: O(nW) - pseudo-polynomial (depends on the value of W, not its bit length)

## Alternative Dynamic Programming Formulation

### Dual Approach
- Define W(i,v) as the minimum weight needed to achieve exactly value v using only the first i items
- Base cases:
  * W(0,0) = 0
  * W(0,v) = +∞ for v > 0
  * W(1,v₁) = w₁
  * W(1,0) = 0
  * W(1,v) = +∞ for v ≠ 0 and v ≠ v₁
- Recurrence relation:
  * W(i+1,v) = min(W(i,v), wᵢ₊₁ + W(i,v-vᵢ₊₁))
- Original knapsack solution:
  * Find max v such that W(n,v) ≤ W

### Analysis of the Dual Approach
- Running time: O(n·V_max), where V_max is the maximum possible total value
- Still pseudo-polynomial as V_max can be large relative to the input size
- However, this formulation allows for value scaling to create an FPTAS

## FPTAS for Knapsack

### Value Scaling Approach
1. Choose a scaling factor k (to be determined)
2. For each item, compute a scaled value:
   * v'ᵢ = ⌊vᵢ/k⌋ (floor of vᵢ/k)
3. Solve the knapsack problem using the dual DP approach with the scaled values v'ᵢ
4. Return the solution found (which is guaranteed to respect the weight constraint)

### Analysis of the Scaled Solution
- Let S' be the set of items returned by the algorithm
- Let O be the optimal set of items for the original problem
- For any item i:
  * vᵢ/k ≥ v'ᵢ ≥ vᵢ/k - 1
- For any set S of items:
  * v(S)/k ≥ v'(S) ≥ v(S)/k - |S|

### Approximation Guarantee
1. Since S' is optimal for the scaled values:
   * v'(S') ≥ v'(O)
2. From our inequality:
   * v'(S') ≥ v(O)/k - |O|
3. Also:
   * v(S') ≥ k·v'(S')
4. Combining:
   * v(S') ≥ v(O) - k·|O| ≥ v(O) - k·n

### Setting the Scaling Factor
- Choose k = ε·v_max/n, where v_max is the maximum item value
- This gives:
  * v(S') ≥ v(O) - ε·v_max
  * Since v_max ≤ v(O), we get:
  * v(S') ≥ (1-ε)·v(O)
- Therefore, the algorithm achieves a (1-ε)-approximation

### Running Time Analysis
- The DP approach takes O(n²·v'_max) time
- With scaling, v'_max = v_max/k = v_max/(ε·v_max/n) = n/ε
- Therefore, the running time is O(n³/ε)
- This is polynomial in both n and 1/ε, qualifying as an FPTAS

## Practical Implementation

### Algorithm Steps
1. Compute v_max (maximum item value)
2. Set k = ε·v_max/n
3. Compute scaled values v'ᵢ = ⌊vᵢ/k⌋ for each item
4. Use the dual DP approach to find the optimal solution for scaled values
5. Return this solution as the approximate solution to the original problem

### Trade-offs
- Smaller ε gives better approximation but increases running time
- For example:
  * ε = 0.01 gives a solution within 99% of optimal
  * ε = 0.001 gives a solution within 99.9% of optimal
  * The running time increases by a factor of 10 when ε decreases by a factor of 10

## Conclusion

- The FPTAS for Knapsack demonstrates a powerful technique: scaling values to achieve arbitrary precision
- This technique can sometimes be applied to other problems with pseudo-polynomial time algorithms
- FPTAS provides a formal framework for trading off solution quality and running time
- It's one of the strongest positive results in approximation algorithms, showing that for some NP-hard problems, we can get arbitrarily close to optimal solutions in polynomial time
