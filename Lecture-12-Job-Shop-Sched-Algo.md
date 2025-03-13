# Lecture 12: Job Scheduling and Algorithm Design

## Introduction to Scheduling Problems

Scheduling problems form a significant class of optimization problems in computer science with wide-ranging applications. This lecture focuses on a specific type: minimizing makespan scheduling.

### Basic Setup
- **Tasks**: $n$ tasks, where task $i$ takes time $T_i$
- **Processors**: $m$ processors ($m \geq 2$)
- **Constraints**: Each task must be assigned to exactly one processor
- **Assignment**: An assignment $A$ maps each task $i$ to a processor $j$

### Key Concept: Load and Makespan

**Load of a Processor**:
- The load of a processor is the sum of times of all tasks assigned to it
- For example, if processor 1 is assigned tasks 1 and 10, its load is $T_1 + T_{10}$

**Makespan**:
- The makespan of an assignment is the maximum load among all processors
- Equivalently, it's the total time needed for all processors to complete their assigned tasks
- Some processors may finish before the makespan, but at least one processor's load will equal the makespan

### Objective

Our goal is to **minimize the makespan** - assign jobs to processors such that the maximum load is as small as possible.

## Complexity of Minimizing Makespan

### NP-Completeness

The problem of minimizing makespan is NP-complete, even when restricted to just two processors ($m = 2$).

**Proof Sketch**:
- The minimizing makespan problem can be reduced from the Partition Problem
- The Partition Problem asks: Given numbers $a_1, a_2, \ldots, a_n$, can they be partitioned into two sets such that each set sums to exactly half of the total?
- For the reduction:
  - Make each number $a_i$ the size of a task $T_i$
  - Ask if there's a makespan less than or equal to half the total sum
  - A "yes" answer to this is equivalent to solving the partition problem
- Since Partition is NP-complete, minimizing makespan is also NP-complete

## Approximation Algorithm: Greedy Scheduler

Since finding the optimal solution is NP-hard, we focus on approximation algorithms.

### Greedy Algorithm Description

1. Initialize an array $J[1..m]$ with all zeros (representing current load on each processor)
2. For each task $i$ from 1 to $n$:
   - Find processor $k$ with minimum current load (minimum value in $J$)
   - Assign task $i$ to processor $k$
   - Update $J[k] = J[k] + T_i$

**Algorithm in Pseudocode**:
```
Initialize Assignment[1..n] = NIL  // No tasks assigned initially
Initialize J[1..m] = 0             // All processors start with zero load

for i = 1 to n:
    k = argmin(J[1..m])            // Find processor with minimum load
    Assignment[i] = k              // Assign task i to processor k
    J[k] = J[k] + T[i]             // Update the load of processor k
```

**Time Complexity**: $O(n \log m)$ when implemented with a priority queue data structure
- The outer loop runs $n$ times
- Each heap/priority queue operation takes $O(\log m)$ time

### Example 1: Greedy Algorithm Achieves Optimal

**Tasks**: T1 = 5, T2 = 3, T3 = 4, T4 = 1, T5 = 1
**Processors**: 2

**Greedy Execution**:
1. Initially, J = [0, 0]
2. Assign T1 (size 5) to Processor 1: J = [5, 0]
3. Assign T2 (size 3) to Processor 2: J = [5, 3]
4. Assign T3 (size 4) to Processor 2: J = [5, 7]
5. Assign T4 (size 1) to Processor 1: J = [6, 7]
6. Assign T5 (size 1) to Processor 1: J = [7, 7]

**Result**: Makespan = 7 (which happens to be optimal in this case)

### Example 2: Greedy Algorithm Performs Poorly

**Tasks**: 
- 10 small tasks of size 1 each (T1 through T10)
- 1 large task of size 10 (T11)
**Processors**: 2

**Optimal Schedule**:
- Assign all 10 small tasks to Processor 1 (load = 10)
- Assign the large task to Processor 2 (load = 10)
- Optimal makespan = 10

**Greedy Execution**:
1. Alternates small tasks between processors (5 to each)
2. When the large task arrives, assigns it to one processor
3. Final loads: [5, 15] or [15, 5]
4. Greedy makespan = 15

This shows greedy can be 1.5× worse than optimal in this example.

## Approximation Factor Analysis

The greedy algorithm does not always produce the optimal solution. Let's analyze how far from optimal it can be in the worst case.

### Proof of 2-Approximation

We'll analyze when and how the makespan is determined in the greedy algorithm:

1. Consider the "makespan-determining step" - the specific moment when a task $T_i$ is assigned to a processor $k$ that ultimately determines the final makespan
2. Let $J_s$ be a snapshot of the processor load array at this point, before adding $T_i$
3. The greedy makespan $M_g = J_s(k) + T_i$

Key observations:
- Processor $k$ had the minimum load when $T_i$ was assigned: $J_s(k) \leq J_s(l)$ for all processors $l$
- The sum of all final processor loads equals the sum of all task times: $\sum_{l=1}^{m} J(l) = \sum_{i=1}^{n} T_i$
- The average load per processor is: $\frac{1}{m} \sum_{i=1}^{n} T_i$
- Since $k$ had minimum load: $J_s(k) \leq \frac{1}{m} \sum_{i=1}^{n} T_i$

For the optimal solution (OPT), we can establish two lower bounds:
1. $OPT \geq \max_i T_i$ (the makespan can't be less than the largest task)
2. $OPT \geq \frac{1}{m} \sum_{i=1}^{n} T_i$ (the makespan can't be less than the average load)

Therefore:
- $J_s(k) \leq OPT$ (from the average load bound)
- $T_i \leq OPT$ (as any task size is a lower bound)

This gives us:
$$M_g = J_s(k) + T_i \leq OPT + OPT = 2 \cdot OPT$$

This proves the greedy algorithm is a 2-approximation.

### Improved Analysis: Sorting Tasks

If we sort the tasks in **descending order** (largest first) before applying the greedy algorithm, we can improve the approximation factor:

#### Improved Approximation Factor: 3/2

When tasks are sorted in descending order ($T_1 \geq T_2 \geq ... \geq T_n$), we can establish:

1. If $n \leq m$ (tasks ≤ processors), both greedy and optimal makespan are the same (trivial case)
2. If $n > m$ (tasks > processors), consider:
   - If makespan is determined in the first $m$ iterations, greedy = optimal
   - If makespan is determined after $m$ iterations (typical case):
     - The "pigeonhole principle" implies that any algorithm (including optimal) must place at least two of the first $m+1$ tasks on the same processor
     - This means $OPT \geq T_1 + T_{m+1}$ or at minimum $OPT \geq 2 \cdot T_{m+1}$
     - Since tasks are sorted, $T_{m+1} \geq T_i$ for all $i > m+1$
     - Therefore, $T_i \leq \frac{OPT}{2}$ for the makespan-determining task $i$

Combining with our earlier bound $J_s(k) \leq OPT$:
$$M_g = J_s(k) + T_i \leq OPT + \frac{OPT}{2} = \frac{3}{2} \cdot OPT$$

This proves that the greedy algorithm with descending-order sorting is a 3/2-approximation.

Further refinements in the analysis can establish an even tighter bound of $(2 - \frac{1}{2m})$, as mentioned in the lecture notes.

## Real-World Applications

### Online Resource Allocation

The greedy scheduling algorithm has important applications in online resource allocation scenarios:

- **Server farm management**: Allocating incoming web requests to available servers
- **Real-time scheduling**: Decisions must be made immediately without knowledge of future tasks

In these online scenarios:
- You can't sort tasks as they arrive in real-time
- The simple "assign to least loaded server" strategy guarantees performance no worse than 2× optimal
- This provides comfort that the greedy approach won't be arbitrarily bad

## Key Insights and Takeaways

1. **Makespan minimization** is a fundamental scheduling problem with many real-world applications
2. Even with just 2 processors, finding the optimal schedule is NP-complete
3. The greedy approach (assign to least loaded processor) is simple, efficient, and provides a 2-approximation
4. Sorting tasks in descending order improves the approximation factor to 3/2
5. These guarantees are valuable in both offline and online scheduling scenarios
6. This analysis demonstrates how simple algorithms can provide provable performance guarantees for NP-hard problems

## Applications

Job scheduling has numerous real-world applications:
- Computing resource allocation
- Cloud computing task assignment
- Manufacturing planning
- Project management
- Web server load balancing
