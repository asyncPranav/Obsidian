---
title: "Unit 4 — Testing Strategies"
subject: "Software Engineering"
tags:
  - software-engineering
  - unit-4
  - testing-strategies
  - exam-notes
---

# UNIT 4 — TESTING STRATEGIES

**Subject:** Software Engineering  
**Purpose:** University examination notes

> [!important] Exam preparation priority
> The topics below are prioritised according to syllabus coverage and their suitability for examination questions. This is a study guide, not a prediction that any question is guaranteed to appear.

## 1. Important Topics According to Examination Perspective

### Priority 1 — Very Important

1. Strategic approach to software testing and testing levels.
2. Black-box testing and its techniques.
3. White-box testing, basis path testing and cyclomatic complexity.
4. Validation testing and acceptance testing.
5. System testing and its types.
6. Debugging techniques and debugging process.
7. Software quality metrics.
8. Function Point (FP) metrics.
9. Cyclomatic complexity and Halstead metrics.
10. Testing metrics and Defect Removal Efficiency (DRE).

### Priority 2 — Important

1. Unit testing and integration testing.
2. Top-down and bottom-up integration testing.
3. Regression testing and smoke testing.
4. Metrics for architectural and component-level design.
5. CK metrics for object-oriented design.
6. Metrics for software maintenance.
7. Mean Time to Change (MTTC) and Backlog Management Index (BMI).

### Priority 3 — Additional

1. Characteristics of a good testing strategy.
2. Test coverage metrics.
3. Fan-in and fan-out.
4. Usability and integrity metrics.
5. Debugging versus testing.

### Important Diagrams, Comparisons and Numericals

| Category | What to practise |
|---|---|
| Diagrams | Testing strategy, testing levels, integration testing, control flow graph, debugging process |
| Comparisons | Black-box vs white-box, verification vs validation, top-down vs bottom-up, testing vs debugging |
| Formulas | FP, cyclomatic complexity, Halstead, DRE, MTTC, BMI, defect density |
| Numericals | Function points, cyclomatic complexity, Halstead metrics, DRE, BMI |

---

# PART A — SOFTWARE TESTING

## 2. Strategic Approach to Software Testing

### 2.1 Definition

Software testing is the process of executing and evaluating software to identify defects and verify that it satisfies specified requirements.

The main purpose of testing is to detect errors and improve software quality.

### 2.2 Strategic Approach

A software testing strategy is a systematic approach that defines how testing will be performed throughout the software development process.

It follows these important principles:

1. Testing starts at the component level and moves towards the complete system.
2. Different testing techniques are used at different stages.
3. Testing is performed by developers and, where appropriate, an independent testing team.
4. Testing and debugging are different activities.
5. Test cases should be designed to detect errors.
6. Testing should verify both functional and non-functional requirements.

### 2.3 Levels of Software Testing

Testing is generally performed at four major levels:

```text
       Unit Testing
            |
            v
     Integration Testing
            |
            v
     Validation Testing
            |
            v
       System Testing
            |
            v
       Final Software
```

1. **Unit Testing:** Individual components or modules are tested separately.
2. **Integration Testing:** Multiple modules are combined and tested to identify interface and interaction errors.
3. **Validation Testing:** Checks whether the developed software satisfies customer requirements.
4. **System Testing:** The complete integrated software system is tested in its intended environment.

These stages are explained in detail below.

## 3. Test Strategies for Conventional Software

### 3.1 Unit Testing

**Definition:** Unit testing is the process of testing individual software components independently to verify that each component works correctly.

It is generally performed by developers.

#### Important Areas of Unit Testing

1. **Module Interface:** Checks whether data is correctly passed between modules.
2. **Local Data Structures:** Checks whether data is stored and accessed correctly.
3. **Boundary Conditions:** Tests values at the limits of input ranges.
4. **Independent Paths:** Checks different execution paths inside a module.
5. **Error Handling:** Checks whether errors are handled properly.

#### Example

Suppose a function calculates the sum of two numbers.

```javascript
function add(a, b) {
    return a + b;
}
```

Test cases:

| Input | Expected Output |
|---|---:|
| 2, 3 | 5 |
| 0, 5 | 5 |
| -2, 3 | 1 |
| -2, -3 | -5 |

Each test checks the correctness of the function.

#### Drivers and Stubs

Sometimes a module is tested before all related modules are available.

- **Driver:** A temporary program that calls the module being tested.
- **Stub:** A temporary replacement for a lower-level module called by the module being tested.

### 3.2 Integration Testing

**Definition:** Integration testing is the process of combining individually tested modules and testing their interactions to identify interface and communication errors.

Its main purpose is to check whether different modules work correctly together.

#### Integration Testing Strategies

There are two major approaches.

##### A. Top-Down Integration Testing

Testing starts with the main control module and moves towards lower-level modules. Missing lower-level modules are replaced with temporary **stubs**.

```text
             Main Module
                  |
          +-------+-------+
          |               |
          v               v
       Module A         Module B
          |               |
       +--+--+           Stub
       |     |
       v     v
    Module C Module D
```

**Steps:**

1. Start with the main control module.
2. Add lower-level modules gradually.
3. Use stubs for unavailable modules.
4. Test the integrated modules.
5. Replace stubs with actual modules.
6. Repeat until all modules are integrated.

**Advantages:**

- Important control logic is tested early.
- A working structure is available early.
- High-level design errors can be detected early.

**Disadvantages:**

- Requires development of stubs.
- Lower-level functionality may be tested late.
- Testing can be difficult when many stubs are needed.

##### B. Bottom-Up Integration Testing

Testing starts with lower-level modules and moves towards higher-level modules. **Drivers** are used to call lower-level modules.

```text
       Module A       Module B
           \           /
            \         /
             \       /
          Intermediate
               |
               v
          Main Module
```

**Steps:**

1. Identify lower-level modules.
2. Combine related modules into clusters.
3. Use drivers to test each cluster.
4. Test the clusters.
5. Combine them with higher-level modules.
6. Continue until the complete system is integrated.

**Advantages:**

- Lower-level functions are tested early.
- No stubs are required.
- Useful for testing reusable utility modules.

**Disadvantages:**

- Drivers are required.
- Main control logic is tested late.
- A complete working system may not be available initially.

#### Other Important Integration Testing Techniques

**1. Regression Testing**

Regression testing means re-executing previously performed tests after software changes to ensure that existing functionality has not been broken.

*Example:* After changing the login functionality, previously working registration and password-reset features are tested again.

**2. Smoke Testing**

Smoke testing is a preliminary test performed on a new software build to check whether its major functions work and whether the build is stable enough for further testing.

*Example:* Checking whether login, dashboard loading and basic navigation work after a new build.

#### Top-Down vs Bottom-Up Testing

| Top-Down | Bottom-Up |
|---|---|
| Starts with higher-level modules. | Starts with lower-level modules. |
| Uses stubs. | Uses drivers. |
| Tests main control logic early. | Tests utility modules early. |
| Lower-level modules are tested later. | Higher-level modules are tested later. |
| Main structure is available early. | Complete system is available later. |

## 4. Black-Box Testing

### Definition

Black-box testing is a software testing technique in which the internal structure and source code of the software are not examined. Testing is performed by providing inputs and checking the outputs against expected results.

It focuses on the external functionality of software.

### Diagram

```text
         Test Input
              |
              v
      +----------------+
      |    Software    |
      |  (Black Box)   |
      +----------------+
              |
              v
         Actual Output
              |
              v
      Compare with Expected
            Output
```

### Important Techniques of Black-Box Testing

#### 1. Equivalence Partitioning

Input data is divided into groups called **equivalence classes**. One or more representative values are selected from each class.

*Example:* Suppose an age field accepts values from 18 to 60.

| Equivalence Class | Example |
|---|---:|
| Below 18 — Invalid | 15 |
| 18–60 — Valid | 30 |
| Above 60 — Invalid | 65 |

Instead of testing every possible value, representative values are tested.

#### 2. Boundary Value Analysis

Testing is performed at the boundaries of input ranges because errors commonly occur at boundary values.

For an age range of 18–60, important test values are:

```text
17, 18, 19, 59, 60, 61
```

These values test just below, at, and just above the limits.

#### 3. Decision Table Testing

A decision table represents different combinations of conditions and their corresponding actions.

*Example: Login system.*

| Valid Username | Valid Password | Result |
|---|---|---|
| Yes | Yes | Login successful |
| Yes | No | Login failed |
| No | Yes | Login failed |
| No | No | Login failed |

It is useful when system behaviour depends on multiple conditions.

### Advantages

- Does not require programming knowledge.
- Tests software from the user's perspective.
- Useful for functional testing.
- Helps identify missing functions and incorrect outputs.

### Disadvantages

- Internal code paths are not examined.
- Complete input coverage is difficult.
- Some hidden logical errors may remain undetected.

## 5. White-Box Testing

### Definition

White-box testing is a software testing technique in which the internal code, logic, control flow and structure of a program are examined to design test cases.

It is also called **structural testing** or **glass-box testing**.

### Diagram

```text
        Source Code
             |
             v
      Examine Internal
           Logic
             |
             v
      Design Test Cases
             |
             v
      Execute Test Cases
             |
             v
       Check Code Paths
```

### Important Techniques

#### 1. Basis Path Testing

Basis path testing is a white-box technique that identifies a set of linearly independent execution paths through a program.

The number of independent paths is determined using **Cyclomatic Complexity**.

#### Cyclomatic Complexity Formula

$$
V(G) = E - N + 2P
$$

Where:

- $E$ = Number of edges in the control flow graph.
- $N$ = Number of nodes.
- $P$ = Number of connected components (normally 1 for a single program flow graph).

For a single connected graph:

$$
V(G) = E - N + 2
$$

Other equivalent formulas:

$$
V(G) = \text{Number of regions}
$$

$$
V(G) = \text{Number of predicate nodes} + 1
$$

**Meaning:** Cyclomatic complexity gives the number of linearly independent paths and the minimum number of basis-path test cases.

#### Solved Numerical

Suppose a control flow graph contains:

- Edges ($E$) = 8
- Nodes ($N$) = 7
- Connected components ($P$) = 1

Using:

$$
V(G) = E - N + 2P
$$

$$
V(G) = 8 - 7 + 2(1)
$$

$$
\boxed{V(G) = 3}
$$

**Answer:** Three linearly independent paths should be tested for basis path coverage.

#### Example Control Flow Graph

```text
             Start
               |
               v
               A
               |
               v
          Decision 1
           /     \
         Yes      No
          |        |
          v        |
          B        |
           \      /
            v    v
          Decision 2
            /   \
          Yes    No
           |      |
           v      |
           C      |
            \    /
             v  v
              End
```

There are two predicate nodes, so:

$$
V(G) = 2 + 1 = 3
$$

#### 2. Condition Testing

Checks logical conditions in a program to ensure they produce correct results.

*Example:*

```javascript
if (age >= 18 && age <= 60)
```

Test the individual conditions with true and false values.

#### 3. Data Flow Testing

Checks how variables are defined, used and modified throughout program execution.

It helps identify:

- Variables used before initialization.
- Variables that are never used.
- Incorrect variable updates.

#### 4. Loop Testing

Checks the behaviour of loops.

Important cases:

- Zero iterations.
- One iteration.
- Two iterations.
- Typical number of iterations.
- Maximum allowed iterations.
- Beyond the allowed limit, where applicable.

### Advantages

- Examines internal logic.
- Helps detect logical errors.
- Measures code and branch coverage.
- Useful for identifying unreachable code.

### Disadvantages

- Requires programming knowledge.
- Can be time-consuming for large systems.
- Complete path testing may be impractical because the number of paths can be very large.

## 6. Black-Box vs White-Box Testing

| Black-Box Testing | White-Box Testing |
|---|---|
| Tests external functionality. | Tests internal structure and logic. |
| Source code is not required. | Source code is generally required. |
| Focuses on inputs and outputs. | Focuses on execution paths and conditions. |
| Based on requirements/specifications. | Based on program structure. |
| Techniques: equivalence partitioning, boundary analysis. | Techniques: basis path, condition, data flow. |
| Usually suitable for functional testing. | Useful for structural testing. |

> **Remember:** Black-box = what the software does. White-box = how the software works internally.

## 7. Validation Testing

### Definition

Validation testing is the process of checking whether the developed software satisfies customer requirements and is suitable for its intended use.

It answers:

> Are we building the right product?

Validation testing is generally performed after integration testing.

### Objectives

1. Verify that all specified requirements are satisfied.
2. Confirm that software performs expected functions.
3. Check whether the system meets user expectations.
4. Identify missing or incorrect functionality.

### Acceptance Testing

Acceptance testing is performed to determine whether the software is acceptable to the customer or end user.

Two important types are:

#### 1. Alpha Testing

- Performed at the developer's site.
- Conducted in a controlled environment.
- Developers may observe users and identify issues.

#### 2. Beta Testing

- Performed at the customer's or user's site.
- Software is used in a real or realistic environment.
- Users provide feedback and report defects.

### Alpha vs Beta Testing

| Alpha Testing | Beta Testing |
|---|---|
| Conducted at developer's site. | Conducted at user's/customer's site. |
| Controlled environment. | Real-world or user environment. |
| Developers may observe testing. | Actual users test the software. |
| Generally performed before beta testing. | Generally performed after alpha testing. |

## 8. System Testing

### Definition

System testing is the process of testing the complete integrated software system to verify that it satisfies specified system requirements and works correctly in its intended environment.

It focuses on the overall system rather than individual modules.

### Types of System Testing

#### 1. Recovery Testing

Checks whether the system can recover from failures such as hardware crashes, software errors or network failures.

*Example:* Checking whether a banking system restores transaction data after a server crash.

#### 2. Security Testing

Checks whether the system protects information and prevents unauthorized access.

Examples:

- Testing login authentication.
- Checking access permissions.
- Testing protection against unauthorized requests.

#### 3. Stress Testing

Checks how the system behaves under extreme conditions or workload beyond normal operating limits.

*Example:* Testing a website with extremely high simultaneous user requests.

#### 4. Performance Testing

Checks the system's speed, response time, throughput and resource utilization under specified workloads.

*Example:* Measuring how long a website takes to respond when 1,000 users access it simultaneously.

### Stress Testing vs Performance Testing

| Performance Testing | Stress Testing |
|---|---|
| Measures system performance under specified workloads. | Tests behaviour under extreme or abnormal workloads. |
| Focuses on response time and throughput. | Focuses on stability and failure behaviour. |
| Checks whether performance requirements are met. | Checks system limits and recovery behaviour. |

## 9. The Art of Debugging

### Definition

Debugging is the process of identifying, analysing, locating and correcting the cause of errors or defects in software.

Testing reveals failures, whereas debugging finds and fixes their causes.

### Debugging Process

```text
      Software Failure
             |
             v
      Reproduce the Bug
             |
             v
      Locate the Cause
             |
             v
       Analyse the Error
             |
             v
        Correct the Code
             |
             v
      Retest the Software
             |
             v
      Regression Testing
```

### Important Debugging Techniques

#### 1. Brute Force Method

The developer uses print statements, logs, memory dumps or debugging tools to locate the error.

- **Advantage:** Simple and easy to apply.
- **Disadvantage:** May generate large amounts of unnecessary information and be inefficient.

#### 2. Backtracking

The developer starts from the point where the error is observed and traces program execution backward to identify its cause.

- **Advantage:** Useful for relatively small programs.
- **Disadvantage:** Difficult for complex programs with many possible execution paths.

#### 3. Cause Elimination

Possible causes of an error are identified and systematically eliminated through testing.

It uses:

- **Induction:** Drawing possible causes from observed evidence.
- **Deduction:** Testing and eliminating possible explanations.

- **Advantage:** A systematic approach to identifying the root cause.

### Testing vs Debugging

| Testing | Debugging |
|---|---|
| Identifies software failures. | Identifies the cause of failures. |
| Detects defects. | Locates and corrects defects. |
| Uses test cases. | Uses debugging tools and reasoning. |
| Can be performed by testers. | Usually performed by developers. |
| Does not necessarily fix errors. | Includes error correction. |

---

# PART B — PRODUCT METRICS

## 10. Software Quality

### 10.1 Definition

Software quality is the degree to which a software product satisfies specified requirements, user expectations and established quality standards.

A good-quality software product should be:

- Correct.
- Reliable.
- Efficient.
- Secure.
- Maintainable.
- Easy to use.

### 10.2 Software Quality Metrics

Software quality metrics are quantitative measures used to evaluate and assess the quality of software.

Important quality metrics:

#### 1. Correctness

Correctness measures how accurately software performs its required functions.

It can be measured using the number of defects per thousand lines of code.

$$
\text{Defect Density} = \frac{\text{Total Defects}}{\text{KLOC}}
$$

Where KLOC means thousand lines of code.

*Example:* If software contains 20 defects and has 10,000 lines of code:

$$
\text{Defect Density} = \frac{20}{10} = 2
$$

**Answer:** 2 defects per KLOC.

A lower defect density generally indicates better correctness.

#### 2. Maintainability

Maintainability measures how easily software can be modified, corrected or improved.

One measure is **Mean Time to Change (MTTC)**. Lower MTTC generally indicates better maintainability.

#### 3. Integrity

Integrity measures the ability of software to protect itself and its data against unauthorized access or attacks.

A commonly used integrity metric is:

$$
\text{Integrity} = \sum_{i=1}^{n} \left[1 - T_i(1-S_i)\right]
$$

Where:

- $T_i$ = Probability of a particular type of attack.
- $S_i$ = Probability that the security mechanism will repel that attack.
- $n$ = Number of attack types considered.

A higher integrity value indicates stronger protection under this model.

#### 4. Usability

Usability measures how easily users can learn and operate software.

It can be evaluated through:

- Time required to learn the software.
- Time required to become efficient.
- Improvement in user productivity.
- User satisfaction and feedback.

## 11. Metrics for Analysis Model

### Definition

Analysis model metrics are measurements used to estimate the size, functionality, completeness and quality of software requirements and analysis models.

An important technique is **Function Point Analysis**.

### 11.1 Function Point (FP) Metrics

Function Point is a software measurement technique used to estimate software size based on the functionality provided to users.

It is independent of the programming language used.

#### Five Components of Function Point Analysis

| Component | Meaning | Example |
|---|---|---|
| EI — External Input | Data entering the system. | Registration form. |
| EO — External Output | Processed data produced by the system. | Monthly report. |
| EQ — External Inquiry | Input requiring an immediate response without significant processing. | Search request. |
| ILF — Internal Logical File | Data maintained within the application. | User database. |
| EIF — External Interface File | Data used by the application but maintained outside it. | External database file. |

#### Standard Complexity Weights

| Component | Simple | Average | Complex |
|---|---:|---:|---:|
| External Input (EI) | 3 | 4 | 6 |
| External Output (EO) | 4 | 5 | 7 |
| External Inquiry (EQ) | 3 | 4 | 6 |
| Internal Logical File (ILF) | 7 | 10 | 15 |
| External Interface File (EIF) | 5 | 7 | 10 |

#### Function Point Formula

First, calculate Unadjusted Function Points:

$$
UFP = \sum (\text{Count} \times \text{Weight})
$$

Then calculate the Value Adjustment Factor:

$$
VAF = 0.65 + 0.01\sum F_i
$$

Where $F_i$ is the rating of each of the 14 general system characteristics. Each characteristic is rated from 0 to 5.

Finally:

$$
\boxed{FP = UFP \times VAF}
$$

#### Solved Numerical Example

Suppose a software system has the following components:

| Component | Count | Complexity | Weight | Total |
|---|---:|---|---:|---:|
| EI | 4 | Average | 4 | 16 |
| EO | 3 | Average | 5 | 15 |
| EQ | 2 | Simple | 3 | 6 |
| ILF | 2 | Average | 10 | 20 |
| EIF | 1 | Simple | 5 | 5 |
| **Total** | | | | **62** |

Therefore:

$$
UFP = 62
$$

Suppose the total of the 14 adjustment factors is 35.

$$
VAF = 0.65 + 0.01(35)
$$

$$
VAF = 0.65 + 0.35 = 1
$$

Now:

$$
FP = 62 \times 1
$$

$$
\boxed{FP = 62}
$$

The estimated functional size of the software is 62 function points.

#### Advantages of Function Point Analysis

1. Independent of programming language.
2. Helps estimate project size.
3. Useful for cost and effort estimation.
4. Can be calculated from requirements and analysis information.

#### Other Analysis Model Measures

Analysis models can also be evaluated using:

- Completeness of requirements.
- Consistency of requirements.
- Number of ambiguous requirements.
- Requirements traceability.
- **Bang metric:** Estimates functional size using functional primitives identified in the analysis model.

## 12. Metrics for Design Model

### Definition

Design metrics are measurements used to evaluate the complexity, structure, quality and maintainability of software design.

They help identify poor modularity and excessive dependencies.

### 12.1 Architectural Design Metrics

These metrics measure the complexity of modules within software architecture.

**Fan-in:** Number of modules that directly call or depend on a particular module.

**Fan-out:** Number of modules directly called or used by a particular module.

Higher fan-out generally indicates greater dependency and structural complexity.

#### Three Important Formulas

**1. Structural Complexity**

$$
S(i) = [\text{fan-out}(i)]^2
$$

**2. Data Complexity**

$$
D(i) = \frac{v(i)}{\text{fan-out}(i)+1}
$$

**3. System Complexity**

$$
\boxed{C(i) = S(i) + D(i)}
$$

Where:

- $v(i)$ = Number of input and output variables associated with module $i$.
- $S(i)$ = Structural complexity.
- $D(i)$ = Data complexity.
- $C(i)$ = Total system complexity of the module.

#### Solved Example

Suppose:

- Fan-out = 3.
- Number of input/output variables = 8.

Structural complexity:

$$
S(i) = 3^2 = 9
$$

Data complexity:

$$
D(i) = \frac{8}{3+1} = 2
$$

System complexity:

$$
C(i) = 9 + 2
$$

$$
\boxed{C(i) = 11}
$$

### 12.2 Component-Level Design Metrics

These metrics evaluate the internal quality of individual software components.

Important measures:

1. **Cohesion:** Measures how closely related the responsibilities within a module are.
2. **Coupling:** Measures dependency between modules.
3. **Complexity:** Measures the complexity of component logic.
4. **Fan-in and Fan-out:** Measure module relationships.

A good component design should have **high cohesion and low coupling**.

### 12.3 Object-Oriented Design Metrics (CK Metrics)

The CK metric suite is used to measure the quality and complexity of object-oriented design.

| Metric | Full Form / Meaning |
|---|---|
| WMC | Weighted Methods per Class |
| DIT | Depth of Inheritance Tree |
| NOC | Number of Children |
| CBO | Coupling Between Object Classes |
| RFC | Response for a Class |
| LCOM | Lack of Cohesion in Methods |

**Brief explanation:**

- **WMC:** Measures the total complexity of methods in a class.
- **DIT:** Measures the depth of a class in the inheritance hierarchy.
- **NOC:** Counts the immediate subclasses of a class.
- **CBO:** Measures the number of other classes connected to or dependent on a class.
- **RFC:** Measures the number of methods that may execute in response to a message received by an object.
- **LCOM:** Measures the lack of cohesion among methods of a class.

These metrics help identify classes that may be difficult to understand, test or maintain.

## 13. Metrics for Source Code

### Definition

Source code metrics are quantitative measurements used to evaluate the size, complexity and quality of program code.

Important metrics include:

1. Lines of Code (LOC).
2. Cyclomatic Complexity.
3. Halstead Software Metrics.

### 13.1 Lines of Code (LOC)

LOC measures the size of a program by counting its source code lines.

KLOC means Thousand Lines of Code.

$$
KLOC = \frac{LOC}{1000}
$$

*Example:* If a program has 5,000 lines:

$$
KLOC = \frac{5000}{1000} = 5
$$

LOC is useful for estimating software size, but it is not a direct measure of software quality.

### 13.2 Cyclomatic Complexity

Cyclomatic complexity measures the number of linearly independent paths through the program's control flow graph.

$$
\boxed{V(G) = E - N + 2P}
$$

It helps determine the minimum number of basis-path test cases.

The detailed explanation and solved numerical are given in Section 5.

### 13.3 Halstead Software Metrics

Halstead metrics measure software complexity using the number of operators and operands in source code.

Four basic quantities are used:

| Symbol | Meaning |
|---|---|
| $n_1$ | Number of distinct operators. |
| $n_2$ | Number of distinct operands. |
| $N_1$ | Total occurrences of operators. |
| $N_2$ | Total occurrences of operands. |

#### Important Formulas

**1. Program Vocabulary**

$$
n = n_1 + n_2
$$

**2. Program Length**

$$
N = N_1 + N_2
$$

**3. Program Volume**

$$
\boxed{V = N\log_2 n}
$$

**4. Program Difficulty**

$$
\boxed{D = \frac{n_1}{2}\times\frac{N_2}{n_2}}
$$

**5. Programming Effort**

$$
\boxed{E = D\times V}
$$

#### Solved Numerical Example

Suppose:

- $n_1 = 4$
- $n_2 = 5$
- $N_1 = 12$
- $N_2 = 15$

**Step 1: Program Vocabulary**

$$
n = 4 + 5 = 9
$$

**Step 2: Program Length**

$$
N = 12 + 15 = 27
$$

**Step 3: Volume**

$$
V = 27\log_2 9
$$

$$
V = 27\times 3.17
$$

$$
\boxed{V\approx 85.59}
$$

**Step 4: Difficulty**

$$
D = \frac{4}{2}\times\frac{15}{5}
$$

$$
D = 2\times 3
$$

$$
\boxed{D = 6}
$$

**Step 5: Effort**

$$
E = D\times V
$$

$$
E = 6\times 85.59
$$

$$
\boxed{E\approx 513.54}
$$

Therefore, program volume is approximately 85.59 and programming effort is approximately 513.54 Halstead effort units.

> [!note] Halstead metrics
> Halstead measures are empirical estimates, not direct measurements of actual development time.

## 14. Metrics for Testing

### Definition

Testing metrics are quantitative measurements used to evaluate testing effectiveness, test coverage and the ability of testing activities to identify defects.

### 14.1 Test Coverage

Test coverage measures the extent to which software code, requirements or conditions are tested.

Important types:

**1. Statement Coverage**

$$
\text{Statement Coverage} = \frac{\text{Executed Statements}}{\text{Total Statements}}\times 100
$$

**2. Branch Coverage**

$$
\text{Branch Coverage} = \frac{\text{Executed Branches}}{\text{Total Branches}}\times 100
$$

**3. Requirements Coverage**

$$
\text{Requirements Coverage} = \frac{\text{Requirements Tested}}{\text{Total Requirements}}\times 100
$$

*Example:* If 45 out of 50 statements are executed:

$$
\text{Coverage} = \frac{45}{50}\times 100
$$

$$
\boxed{90\%}
$$

### 14.2 Defect Removal Efficiency (DRE)

DRE measures the effectiveness of the software development and testing process in removing defects before software delivery.

Formula:

$$
\boxed{DRE = \frac{E}{E+D}\times 100}
$$

Where:

- $E$ = Number of defects found before delivery.
- $D$ = Number of defects found after delivery.

#### Solved Numerical

Suppose:

- Defects found before delivery = 90.
- Defects found after delivery = 10.

$$
DRE = \frac{90}{90+10}\times 100
$$

$$
DRE = \frac{90}{100}\times 100
$$

$$
\boxed{DRE = 90\%}
$$

**Interpretation:** The process removed 90% of the total defects considered before delivery.

Higher DRE generally indicates more effective defect removal.

## 15. Metrics for Maintenance

### Definition

Maintenance metrics are measurements used to evaluate the effort, efficiency, speed and quality of software maintenance activities.

They help organizations understand how effectively software is being corrected, modified and updated after delivery.

### 15.1 Mean Time to Change (MTTC)

MTTC measures the average time required to implement a software change.

It includes:

- Analysing the change request.
- Designing the modification.
- Implementing the change.
- Testing the modification.
- Releasing the change.

Formula:

$$
\boxed{MTTC = \frac{\text{Total Time for All Changes}}{\text{Number of Changes}}}
$$

*Example:* Suppose 5 software changes require a total of 40 hours.

$$
MTTC = \frac{40}{5}
$$

$$
\boxed{MTTC = 8\text{ hours}}
$$

### 15.2 Backlog Management Index (BMI)

BMI measures how effectively an organization handles incoming maintenance requests.

Formula:

$$
\boxed{BMI = \frac{\text{Requests Closed}}{\text{Requests Received}}\times 100}
$$

*Example:* Suppose:

- Requests received = 50.
- Requests closed = 60.

$$
BMI = \frac{60}{50}\times 100
$$

$$
\boxed{BMI = 120\%}
$$

**Interpretation:**

| BMI | Meaning |
|---|---|
| Above 100% | Backlog is generally decreasing. |
| Equal to 100% | Backlog is generally stable. |
| Below 100% | Backlog is generally increasing. |

This interpretation assumes the same measurement period and consistently counted requests.

### 15.3 Other Maintenance Metrics

**1. Fix Backlog**  
Number of unresolved maintenance requests waiting to be addressed.

**2. Fix Response Time**  
Time taken to begin analysing a maintenance request after it is received.

**3. Percentage of Overdue Fixes**  
Measures how many fixes are completed later than their promised deadlines.

$$
\text{Overdue Fixes} = \frac{\text{Number of Overdue Fixes}}{\text{Total Fixes}}\times 100
$$

**4. Average Age of Backlog**  
Measures the average age of pending maintenance requests.

$$
\text{Average Age} = \frac{\text{Total Age of Pending Requests}}{\text{Number of Pending Requests}}
$$

### Importance of Maintenance Metrics

1. Helps measure maintenance productivity.
2. Improves response time.
3. Helps control maintenance costs.
4. Identifies unresolved problems.
5. Improves software maintainability.

## 16. Important Formula Sheet

These are the formulas to practise before the examination.

| Metric | Formula |
|---|---|
| Defect Density | Defects / KLOC |
| KLOC | LOC / 1000 |
| UFP | Sum of (Count × Weight) |
| VAF | $0.65 + 0.01\sum F_i$ |
| Function Points | $UFP\times VAF$ |
| Cyclomatic Complexity | $V(G)=E-N+2P$ |
| Halstead Vocabulary | $n=n_1+n_2$ |
| Halstead Length | $N=N_1+N_2$ |
| Halstead Volume | $V=N\log_2 n$ |
| Halstead Difficulty | $D=(n_1/2)(N_2/n_2)$ |
| Halstead Effort | $E=D\times V$ |
| Statement Coverage | Executed Statements / Total Statements × 100 |
| Branch Coverage | Executed Branches / Total Branches × 100 |
| DRE | $E/(E+D)\times 100$ |
| MTTC | Total Change Time / Number of Changes |
| BMI | Requests Closed / Requests Received × 100 |
| Structural Complexity | $S(i)=\text{fan-out}(i)^2$ |
| Data Complexity | $D(i)=v(i)/(\text{fan-out}(i)+1)$ |
| System Complexity | $C(i)=S(i)+D(i)$ |

---

# 17. Important Differences for Examination

## A. Verification vs Validation

| Verification | Validation |
|---|---|
| Checks whether the product is being developed correctly. | Checks whether the correct product is being developed. |
| Focuses on specifications and design conformance. | Focuses on customer needs. |
| Often involves reviews and inspections. | Involves execution and evaluation of software. |
| Example: Reviewing design documents. | Example: Testing the developed application. |

> **Remember:** Verification = Are we building the product right?  
> Validation = Are we building the right product?

## B. Unit vs Integration vs Validation vs System Testing

| Unit | Integration | Validation | System |
|---|---|---|---|
| Individual module. | Combined modules. | Requirements satisfaction. | Complete system. |
| Tests components. | Tests interactions. | Tests customer needs. | Tests overall behaviour. |
| Early testing level. | After unit testing. | After integration. | After validation in the typical sequence. |
| Usually performed by developers. | Usually performed by developers/test team. | Testing team/customer involvement. | Often involves an independent testing team. |

---

# 18. Important Long-Answer Questions (5–10 Marks)

Prepare these questions thoroughly.

| Priority | Question |
|---|---|
| Very High | Explain the strategic approach to software testing with a diagram. |
| Very High | Explain black-box testing and its techniques with examples. |
| Very High | Explain white-box testing and basis path testing with cyclomatic complexity. |
| Very High | Explain integration testing and its top-down and bottom-up approaches. |
| Very High | Explain validation testing and acceptance testing. |
| Very High | Explain system testing and its different types. |
| Very High | Explain debugging and its techniques. |
| Very High | Explain software quality metrics with formulas. |
| Very High | Explain Function Point Analysis with a solved example. |
| Very High | Explain Halstead software metrics with formulas. |
| High | Explain metrics for the software design model. |
| High | Explain CK metrics used in object-oriented design. |
| High | Explain metrics for testing, including DRE and test coverage. |
| High | Explain software maintenance metrics, MTTC and BMI. |

## 19. Important Short-Answer Questions (2–3 Marks)

1. Define software testing.
2. What is unit testing?
3. Define integration testing.
4. What are drivers and stubs?
5. What is regression testing?
6. What is smoke testing?
7. Define black-box testing.
8. Define white-box testing.
9. What is equivalence partitioning?
10. Define boundary value analysis.
11. What is basis path testing?
12. Define cyclomatic complexity.
13. What is validation testing?
14. Differentiate alpha and beta testing.
15. What is system testing?
16. Define stress testing and recovery testing.
17. What is debugging?
18. Name three debugging techniques.
19. Define function point.
20. What is Halstead volume?
21. Define defect density.
22. What is DRE?
23. What are fan-in and fan-out?
24. Define MTTC.
25. What is Backlog Management Index?
26. Expand WMC, DIT, NOC, CBO, RFC and LCOM.

## 20. Important Numericals to Practise

These are the major calculation-based topics in this unit.

| Numerical | Important formula |
|---|---|
| Function Point Analysis | $FP=UFP\times VAF$ |
| Cyclomatic Complexity | $V(G)=E-N+2P$ |
| Halstead Metrics | Volume, Difficulty, Effort |
| Defect Density | Defects / KLOC |
| Test Coverage | Executed / Total × 100 |
| Defect Removal Efficiency | $E/(E+D)\times 100$ |
| MTTC | Total time / Number of changes |
| BMI | Closed requests / Received requests × 100 |
| Architectural Complexity | $S(i)$, $D(i)$, $C(i)$ |

**Numerical preparation tip:** Practise at least one complete example each of Function Point Analysis, Cyclomatic Complexity and Halstead metrics. Also revise the simpler DRE, BMI and coverage calculations.

## 21. Important Diagrams to Practise

These are the diagrams worth drawing in your answer sheet.

1. **Software Testing Levels** — Unit → Integration → Validation → System.
2. **Integration Testing** — Top-down and bottom-up diagrams, showing stubs and drivers.
3. **Control Flow Graph** — Predicate nodes, edges and independent paths.
4. **Black-Box Testing** — Input → Software → Output.
5. **Debugging Process** — Reproduce → Locate → Fix → Retest.

---

# 22. Last-Minute Revision Sheet

Revise this section before entering the exam hall.

## A. Testing Strategy

```text
Software Testing
       |
       v
Unit Testing
       |
       v
Integration Testing
       |
       v
Validation Testing
       |
       v
System Testing
```

## B. Testing Types

```text
UNIT
Individual modules

INTEGRATION
Combined modules

VALIDATION
Customer requirements

SYSTEM
Complete integrated system
```

## C. Black-Box Testing

- Internal code is not examined.
- Focuses on inputs and outputs.
- Equivalence partitioning.
- Boundary value analysis.
- Decision table testing.

## D. White-Box Testing

- Examines internal code and logic.
- Basis path testing.
- Condition testing.
- Data flow testing.
- Loop testing.

## E. Integration Testing

```text
Top-Down = Stubs
Bottom-Up = Drivers
Regression = Retest after changes
Smoke = Check build stability
```

## F. Validation and System Testing

```text
Validation
   |
   +-- Acceptance Testing
          |
          +-- Alpha
          +-- Beta

System Testing
   |
   +-- Recovery
   +-- Security
   +-- Stress
   +-- Performance
```

## G. Debugging

Three techniques:

1. Brute Force.
2. Backtracking.
3. Cause Elimination.

## H. Software Quality Metrics

| Metric | Meaning |
|---|---|
| Correctness | Absence of defects. |
| Maintainability | Ease of modification. |
| Integrity | Protection against attacks. |
| Usability | Ease of use. |

## I. Important Full Forms

| Abbreviation | Full Form |
|---|---|
| LOC | Lines of Code |
| KLOC | Thousand Lines of Code |
| FP | Function Point |
| UFP | Unadjusted Function Points |
| VAF | Value Adjustment Factor |
| CFG | Control Flow Graph |
| DRE | Defect Removal Efficiency |
| MTTC | Mean Time to Change |
| BMI | Backlog Management Index |
| EI | External Input |
| EO | External Output |
| EQ | External Inquiry |
| ILF | Internal Logical File |
| EIF | External Interface File |
| WMC | Weighted Methods per Class |
| DIT | Depth of Inheritance Tree |
| NOC | Number of Children |
| CBO | Coupling Between Object Classes |
| RFC | Response for a Class |
| LCOM | Lack of Cohesion in Methods |

## Final Exam Preparation Strategy

If your remaining time is limited, follow this order:

1. **First:** Black-box, white-box and integration testing.
2. **Second:** Validation testing, system testing and debugging.
3. **Third:** Software quality and Function Point Analysis.
4. **Fourth:** Cyclomatic complexity and Halstead numericals.
5. **Fifth:** Testing metrics, design metrics and maintenance metrics.

**Exam tip:** In numerical questions, always write the formula first, substitute the given values, show the calculation and underline the final answer. In theory questions, use definitions, labelled diagrams and numbered headings.

These notes cover the topics listed in Unit 4. Focus particularly on understanding the testing levels and practising the metric formulas rather than memorising every paragraph.
