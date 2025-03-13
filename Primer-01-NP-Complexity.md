# Computational Complexity Primer: P, NP, and NP-Completeness

## Introduction to Computational Complexity

Computational complexity theory classifies computational problems according to their inherent difficulty. It provides a framework for understanding which problems can be solved efficiently and which ones are likely intractable regardless of algorithmic improvements.

## Decision Problems

Complexity theory typically focuses on **decision problems** - problems with a yes/no answer.

**Examples of decision problems:**
- Is number $n$ prime?
- Does graph $G$ have a Hamiltonian cycle?
- Can tasks $T_1, T_2, ..., T_n$ be scheduled on $m$ processors with makespan $\leq k$?

Any optimization problem can be converted to a decision problem:
- Instead of "Find the minimum makespan"
- Ask "Is there a schedule with makespan $\leq k$?"

## The Class P

**P** (Polynomial Time) is the class of decision problems that can be solved by a deterministic algorithm in polynomial time.

**Formal definition:** A problem is in P if there exists an algorithm that can solve it in $O(n^k)$ time, where $n$ is the input size and $k$ is a constant.

**Examples of problems in P:**
- Determining if a number is prime
- Finding the shortest path in a graph
- Linear programming
- 2-SAT (Boolean satisfiability with 2 literals per clause)

Problems in P are generally considered **efficiently solvable**.

## The Class NP

**NP** (Nondeterministic Polynomial Time) is the class of decision problems for which:
- A "yes" answer can be **verified** in polynomial time given a certificate/proof
- It does not necessarily mean we can **find** a solution in polynomial time

**Formal definition:** A problem is in NP if, given a potential solution (certificate), we can verify whether it is correct in polynomial time.

**Examples of problems in NP:**
- All problems in P (trivially, since we can solve them in polynomial time)
- Boolean satisfiability (SAT)
- Hamiltonian cycle problem
- Traveling Salesman decision problem
- Integer Linear Programming
- Minimizing makespan in scheduling

## The Relationship Between P and NP

- P is a subset of NP (every problem solvable in polynomial time can have its solution verified in polynomial time)
- The famous P vs. NP question asks: Is P = NP?
- Most computer scientists believe P ≠ NP, but this remains an open question
- If P = NP, then every problem whose solution can be verified quickly could also be solved quickly

![P vs NP Diagram](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a0/P_np_np-complete_np-hard.svg/800px-P_np_np-complete_np-hard.svg.png)

## NP-Completeness

**NP-Complete** problems are the "hardest" problems in NP.

**Formal definition:** A problem X is NP-Complete if:
1. X is in NP
2. Every problem in NP can be reduced to X in polynomial time (polynomial-time reducibility)

If any NP-Complete problem could be solved in polynomial time, then P = NP.

### Key Properties of NP-Complete Problems:
- If we find a polynomial-time algorithm for one NP-Complete problem, we can solve all NP problems in polynomial time
- If we prove that one NP-Complete problem cannot be solved in polynomial time, then P ≠ NP

### Important NP-Complete Problems:
- Boolean Satisfiability (SAT)
- 3-SAT (SAT with exactly 3 literals per clause)
- Traveling Salesman Problem (decision version)
- Vertex Cover
- Graph Coloring
- Subset Sum
- Partition Problem
- Knapsack (decision version)
- Job Shop Scheduling (minimizing makespan)

## Reductions

Reductions are a key tool for proving NP-Completeness.

**Definition:** A reduction from problem A to problem B is an algorithm that transforms any instance of A into an equivalent instance of B.

**Properties of reductions:**
- If problem A reduces to problem B, and B is in P, then A is in P
- If A reduces to B, and A is NP-Complete, then B is NP-Hard
- If A reduces to B, A is NP-Complete, and B is in NP, then B is NP-Complete

### Example Reduction: SAT to 3-SAT
- SAT: Given Boolean variables and clauses, is there an assignment that satisfies all clauses?
- 3-SAT: Same as SAT, but each clause has exactly 3 literals
- Reduction: Transform clauses with more or fewer than 3 literals into equivalent collections of 3-literal clauses

## NP-Hardness

**NP-Hard** problems are at least as hard as the hardest problems in NP.

**Formal definition:** A problem is NP-Hard if every problem in NP can be reduced to it in polynomial time.

**Properties:**
- NP-Hard problems may or may not be in NP
- NP-Complete = NP ∩ NP-Hard
- Some NP-Hard problems cannot be expressed as decision problems

**Examples of NP-Hard problems:**
- All NP-Complete problems
- Traveling Salesman Problem (optimization version)
- Halting Problem (not in NP, undecidable)

## Proving NP-Completeness

To prove a problem X is NP-Complete:
1. Prove X is in NP by showing a polynomial-time verification algorithm
2. Choose a known NP-Complete problem Y (e.g., SAT, 3-SAT, Vertex Cover)
3. Construct a polynomial-time reduction from Y to X

The first NP-Complete problem (SAT) was proven through the Cook-Levin theorem, which showed that every problem in NP can be reduced to SAT.

### Example: Proving Minimizing Makespan is NP-Complete
1. Verification: Given a schedule, we can check its makespan in polynomial time
2. Reduction from Partition Problem:
   - Given numbers $a_1, a_2, ..., a_n$, can they be partitioned into two sets with equal sums?
   - Convert to scheduling: Create n tasks with times $T_i = a_i$ and 2 processors
   - Ask: Is there a schedule with makespan $\leq \frac{1}{2}\sum a_i$?
   - Solution to partition exists if and only if such a schedule exists

## Approximation Algorithms

Since NP-Complete problems are unlikely to have polynomial-time exact algorithms, we often use approximation algorithms.

**Definition:** An $\alpha$-approximation algorithm is a polynomial-time algorithm that produces a solution within a factor of $\alpha$ of the optimal solution.

### Example: Greedy Scheduler for Minimizing Makespan
- Greedy: Assign each job to the processor with the minimum current load
- Proven to be a 2-approximation algorithm
- In practice, often performs much better than the worst-case guarantee

## Coping with NP-Completeness

When facing an NP-Complete problem, several approaches are possible:
1. **Approximation algorithms**: Settle for near-optimal solutions
2. **Parameterized algorithms**: Efficient algorithms when certain parameters are small
3. **Heuristics**: Methods that work well in practice but may not have theoretical guarantees
4. **Special cases**: Identify restricted versions of the problem that are in P
5. **Exponential algorithms**: Design algorithms that, while exponential, are significantly faster than brute force

## Complexity Classes Beyond NP

**PSPACE**: Problems solvable using polynomial space (regardless of time)
- Contains both NP and co-NP

**co-NP**: Problems whose "no" instances can be verified in polynomial time
- SAT is in NP, while determining if a formula is unsatisfiable is in co-NP

**EXPTIME**: Problems solvable in exponential time
- Contains PSPACE

**Polynomial Hierarchy (PH)**: A hierarchy of complexity classes that extends beyond NP

## Practical Implications

Understanding complexity theory helps practitioners:
1. Recognize when they're facing fundamentally hard problems
2. Avoid spending resources searching for efficient exact algorithms when they likely don't exist
3. Choose appropriate approaches (approximation, heuristics, etc.) for hard problems
4. Identify special cases that might be tractable

---

## Key Takeaways

1. **P**: Problems solvable in polynomial time
2. **NP**: Problems verifiable in polynomial time
3. **NP-Complete**: The "hardest" problems in NP
4. **NP-Hard**: Problems at least as hard as NP-Complete problems
5. **P vs. NP**: The central open question in complexity theory
6. For NP-Complete problems, we typically use approximation algorithms or other approaches rather than seeking exact polynomial-time solutions
