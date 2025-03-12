# Approximation Algorithms and Linear Programming
## Lecture 1: What is Linear Programming?

### Introduction to Linear Programming
- **Linear Program (LP)**: A type of mathematical optimization problem
- **Key components**:
  - Maximizing or minimizing an objective function
  - Subject to a set of constraints
- **Distinction**: Different in spirit from dynamic programming

### Objective Function
- If it represents something positive (revenue, profit, happiness) → maximize
- If it represents something negative (cost, pain) → minimize

### Simple Example of a Linear Program

**Objective**:
```
Maximize: x₁ + x₂ - x₃
```

**Decision Variables**:
- x₁, x₂, x₃ are real-valued numbers
- Represent quantities we're making decisions about (e.g., amount of sugar, milk, water)

**Constraints**:
```
Subject to:
x₁ ≤ 1
x₂ ≤ 1
x₃ ≤ 1
x₁, x₂, x₃ ≥ 0
```

**Solution Strategy**:
- To maximize x₁ + x₂ - x₃:
  - Make x₁ as large as possible → x₁ = 1
  - Make x₂ as large as possible → x₂ = 1
  - Make x₃ as small as possible → x₃ = 0
- Optimal objective value: 1 + 1 - 0 = 2

### More Complex Example

**Objective**:
```
Maximize: x₁ - 2x₂ + x₃
```

**Constraints**:
```
Subject to:
x₁ - x₂ + x₃ ≤ 4
x₁ + x₂ - x₃ ≤ 7
x₁ - x₂ ≤ 8
x₁ ≤ 12
x₂ ≤ 14
x₃ ≤ 22
x₁, x₂, x₃ ≥ 0
```

**Notes**:
- All decision variables can take real values
- All constraints must be satisfied simultaneously
- Solution must maximize the objective function among all valid solutions

### Why "Linear" Programming?

1. **Objective function must be linear**:
   - Can include sums, differences, or constant multiples of variables
   - Cannot include nonlinear terms like:
     - Products of variables (e.g., x₁·x₂)
     - Powers of variables (e.g., x₁²)
     - Transcendental functions (e.g., sin(x₁))

2. **Constraints must be linear inequalities**:
   - Linear function ≤ constant
   - Linear function ≥ constant
   - Linear function = constant (can be expressed as two inequalities)

### Why "Programming"?

- Not related to computer programming
- Historical terminology from before computers
- Refers to "program" in the sense of a schedule or plan
- Early applications were in scheduling, planning, and logistics problems

### Formal Definition of Linear Programming

**Standard Form**:
```
Maximize: c₁x₁ + c₂x₂ + ... + cₙxₙ

Subject to:
a₁₁x₁ + a₁₂x₂ + ... + a₁ₙxₙ ≤ b₁
a₂₁x₁ + a₂₂x₂ + ... + a₂ₙxₙ ≤ b₂
...
aₘ₁x₁ + aₘ₂x₂ + ... + aₘₙxₙ ≤ bₘ
```

**Matrix Form**:
```
Maximize: c^T x
Subject to: Ax ≤ b
```
Where:
- c is an n×1 vector of coefficients
- x is an n×1 vector of decision variables
- A is an m×n matrix of constraint coefficients
- b is an m×1 vector of constraint bounds

**Variations that preserve linearity**:
- Minimization instead of maximization (min f(x) = max -f(x))
- Greater-than-or-equal constraints (multiply by -1)
- Equality constraints (express as two inequalities)

**What's not allowed**:
- Strict inequalities (< or >)
- Nonlinear terms in constraints or objective

### Next Steps
- Future lectures will explore more meaningful examples of linear programs
- Applications to algorithmic problems
