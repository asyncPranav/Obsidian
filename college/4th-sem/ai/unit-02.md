
---

Here are **UNIT-2 detailed exam-ready notes (topic-wise)**.  
Language is **easy but technical**, and I marked **exam importance** for each.

---

# UNIT–2: SEARCHING

This unit is **VERY IMPORTANT** — usually **one full question** comes from here.

Topics:

1. Searching for solutions
    
2. Uninformed search (BFS, DFS)
    
3. Heuristic search (Hill climbing, A*, AO*)
    
4. Problem reduction
    
5. Game playing & adversarial search
    
6. Minimax algorithm
    
7. Optimal decisions in multiplayer games
    
8. Problems in game playing
    

---

# 1. Searching for Solutions

🔥 Exam Importance: VERY HIGH

## What is Searching?

Searching is the process of **finding a sequence of actions** that leads from **initial state to goal state**.

AI solves problems by **searching through possible states**.

### Example

Initial state → start city  
Goal state → destination  
Search → find best route

---

## Basic Components of Search Problem

### 1. Initial State

Starting point of problem

Example: Start node

---

### 2. Goal State

Target to reach

Example: Destination node

---

### 3. State Space

All possible states reachable

Example:  
All cities in map

---

### 4. Operators / Actions

Moves available

Example:  
Move left, right

---

### 5. Path

Sequence of actions

Example:  
A → B → C → Goal

---

### 6. Solution

Path from initial to goal state

---

# State Space Representation

Usually represented as **tree** or **graph**

Example:

```
        A
      /   \
     B     C
    / \   / \
   D   E F   G
```

Search finds goal node.

---

# Properties of Search Algorithm

Important for exam

Completeness → finds solution or not  
Optimality → best solution or not  
Time complexity → time required  
Space complexity → memory required

---

# Types of Searching

Two main types:

1. Uninformed Search (Blind search)
    
2. Informed Search (Heuristic search)
    

---

# 2. Uninformed Search Strategies

🔥 Exam Importance: VERY VERY HIGH

Uninformed search **does not use extra knowledge**  
It only uses **problem definition**

Types:

- Breadth First Search (BFS)
    
- Depth First Search (DFS)
    

---

# Breadth First Search (BFS)

🔥 Very Important

## Definition

BFS explores **level by level**.

It expands all nodes at depth 1, then depth 2, etc.

Uses **Queue (FIFO)**

---

## BFS Example

```
        A
      /   \
     B     C
    / \   / \
   D   E F   G
```

Order:  
A → B → C → D → E → F → G

---

## BFS Algorithm (Exam Writing)

1. Start with root node
    
2. Insert into queue
    
3. Remove first node
    
4. Check goal
    
5. Expand children
    
6. Add children to queue
    
7. Repeat until goal found
    

---

## Properties of BFS

Complete → Yes  
Optimal → Yes (if cost same)  
Time complexity → O(b^d)  
Space complexity → O(b^d)

b = branching factor  
d = depth

---

## Advantages

Finds shortest path  
Complete  
Simple

---

## Disadvantages

Uses large memory  
Slow for deep tree

---

# Depth First Search (DFS)

🔥 Very Important

## Definition

DFS explores **deepest node first**.

Uses **Stack (LIFO)**

---

## DFS Example

```
        A
      /   \
     B     C
    / \   / \
   D   E F   G
```

Order:  
A → B → D → E → C → F → G

---

## DFS Algorithm

1. Start at root
    
2. Go to deepest node
    
3. Backtrack if needed
    
4. Continue until goal found
    

---

## Properties

Complete → No  
Optimal → No  
Time complexity → O(b^m)  
Space complexity → O(bm)

m = max depth

---

## Advantages

Less memory  
Simple  
Fast for deep solution

---

## Disadvantages

May get stuck  
Not optimal  
Not complete

---

# BFS vs DFS (Very Important Table)

|BFS|DFS|
|---|---|
|Level wise|Depth wise|
|Uses queue|Uses stack|
|Complete|Not complete|
|Optimal|Not optimal|
|High memory|Low memory|
|Slow|Fast|

---

Next topics coming:

- Heuristic search
    
- Hill climbing
    
- A*
    
- AO*
    
- Game playing
    
- Minimax
    

I’ll send **next part now**.



---




# Uninformed (Uniform) Search Strategies

🔥 **Exam Importance: VERY HIGH (Almost guaranteed question)**

In many syllabi, “Uniform search” usually means **Uninformed / Blind Search**.  
It includes methods where AI has **no extra knowledge (heuristics)** about the goal.

So AI only uses:

- Initial state
- Goal test
- Successor function
- Path cost (sometimes)

---

# 1. What is Uninformed Search?

Uninformed search is a type of search in which the algorithm **does not know anything about the goal direction** except the problem definition.

It does NOT use:

- Heuristic values
- Guessing
- Experience

It only explores the search space blindly.

### Exam Definition:

**Uninformed search is a search strategy that explores the state space without any domain knowledge or heuristic information about the goal.**

---

# 2. Characteristics of Uninformed Search

🔥 Important for theory question

- No extra knowledge about goal direction
- Uses only problem definition
- Explores all possible paths
- Can be slow for large problems
- May explore unnecessary nodes
- Guaranteed to find solution in some methods (like BFS)

---

# 3. Types of Uninformed Search (VERY IMPORTANT)

Main methods:

1. Breadth First Search (BFS)
2. Depth First Search (DFS)
3. Uniform Cost Search (UCS)
4. Depth Limited Search (DLS)
5. Iterative Deepening DFS (IDDFS)

(Your syllabus mainly focuses on BFS and DFS, but UCS is often asked extra)

---


# Uninformed Search Strategies (Blind Search)

## 👉 Breadth First Search (BFS) & Depth First Search (DFS)

🔥 **Exam Importance: VERY HIGH (Most expected long question)**

Uninformed search means the algorithm has **no extra knowledge (heuristics)** about the goal. It only uses:

- Initial state
    
- Goal test
    
- Successor function
    

So it explores the state space **blindly**.

---

# 1. Breadth First Search (BFS)

🔥 **VERY IMPORTANT (10 marks question favourite)**

## ✔ Definition

- BFS is a search strategy in which the algorithm explores the **state space level by level**.
    
- It expands all nodes at the current depth before moving to the next depth level.
    

---

## ✔ Key Idea

- “Explore all nearest nodes first, then move deeper”
    
- Works like **spreading in waves**
    

---

## ✔ Data Structure Used

- **Queue (FIFO – First In First Out)**
    

---

## ✔ Working Steps (Algorithm Style)

- Start with the initial node and insert it into the queue
    
- Remove the front node from the queue
    
- Check whether it is the goal state
    
- If yes → stop (solution found)
    
- If not → expand the node (generate all child nodes)
    
- Add all child nodes to the back of the queue
    
- Repeat until goal is found or queue becomes empty
    

---

## ✔ Example Tree

```id="p9q6s3"
        A
      /   \
     B     C
    / \   / \
   D   E F   G
```

### BFS Traversal Order:

- A
    
- B, C
    
- D, E, F, G
    

👉 Final order: **A → B → C → D → E → F → G**

---

## ✔ Properties of BFS

### 1. Completeness

- BFS is **complete**
    
- It will always find a solution if one exists
    

---

### 2. Optimality

- BFS is **optimal** if all step costs are equal
    
- It finds the **shortest path (minimum number of steps)**
    

---

### 3. Time Complexity

- **O(b^d)**  
    Where:
    
- b = branching factor (children per node)
    
- d = depth of solution
    

👉 Expands many nodes → can be slow

---

### 4. Space Complexity

- **O(b^d)**  
    👉 Stores all nodes at a level → very memory expensive
    

---

## ✔ Advantages of BFS

- Guarantees shortest path solution
    
- Always finds solution if it exists
    
- Simple and easy to implement
    

---

## ✔ Disadvantages of BFS

- Very high memory usage
    
- Slow for large search spaces
    
- Not suitable for deep solutions
    

---

## ✔ When to Use BFS

- When solution is **near the root**
    
- When **shortest path is required**
    
- When state space is small
    

---

# 2. Depth First Search (DFS)

🔥 **VERY IMPORTANT (frequently asked)**

---

## ✔ Definition

- DFS is a search strategy where the algorithm explores the **deepest path first before backtracking**.
    
- It goes deep into one branch until it cannot proceed further.
    

---

## ✔ Key Idea

- “Go as deep as possible, then come back”
    
- Works like exploring one full path first
    

---

## ✔ Data Structure Used

- **Stack (LIFO – Last In First Out)**  
    OR
    
- Recursion (function call stack)
    

---

## ✔ Working Steps (Algorithm Style)

- Start with the initial node
    
- Visit the node and mark it
    
- Expand one child node
    
- Go deeper into that child
    
- Continue until no further node exists
    
- If goal not found → backtrack
    
- Explore next unvisited path
    
- Repeat until goal is found
    

---

## ✔ Example Tree

```id="7h3m0q"
        A
      /   \
     B     C
    / \   / \
   D   E F   G
```

### DFS Traversal Order (one possible order):

- A
    
- B
    
- D
    
- E
    
- C
    
- F
    
- G
    

👉 Final order: **A → B → D → E → C → F → G**

_(Order may change depending on implementation)_

---

## ✔ Properties of DFS

### 1. Completeness

- DFS is **not complete**
    
- May get stuck in infinite depth
    

---

### 2. Optimality

- DFS is **not optimal**
    
- It does not guarantee shortest path
    

---

### 3. Time Complexity

- **O(b^m)**  
    Where:
    
- m = maximum depth of search space
    

---

### 4. Space Complexity

- **O(bm)**  
    👉 Very memory efficient compared to BFS
    

---

## ✔ Advantages of DFS

- Requires very little memory
    
- Simple implementation
    
- Can reach deep solutions quickly
    

---

## ✔ Disadvantages of DFS

- May go in wrong direction
    
- Can get stuck in infinite loop
    
- Does not guarantee shortest path
    

---

## ✔ When to Use DFS

- When memory is limited
    
- When solution is deep in the tree
    
- When approximate solution is acceptable
    

---

# 3. BFS vs DFS (VERY IMPORTANT EXAM TABLE)

|Feature|BFS|DFS|
|---|---|---|
|Approach|Level by level|Depth wise|
|Data structure|Queue|Stack|
|Memory usage|High|Low|
|Completeness|Yes|No|
|Optimality|Yes (if equal cost)|No|
|Speed|Slower|Faster (sometimes)|

---



---


# Difference Between Uninformed (Uniform) and Informed Search

🔥 **Exam Importance: VERY HIGH (frequently asked 5–10 marks question)**

This is one of the **most repeated comparison questions** in AI exams.

---

# 1. Uninformed Search (Blind Search)

Uninformed search is a search strategy in which the algorithm **does not have any extra knowledge about the goal**.

It only knows:

- Initial state
    
- Goal test
    
- Possible actions
    

It explores the search space **blindly**.

### Examples:

- BFS (Breadth First Search)
    
- DFS (Depth First Search)
    
- Uniform Cost Search (UCS)
    

---

# 2. Informed Search (Heuristic Search)

Informed search is a strategy in which the algorithm **uses additional knowledge (heuristics)** to guide the search toward the goal.

It tries to choose the **best possible path first**.

### Examples:

- A* Search
    
- Hill Climbing
    
- AO* Search
    

---

# 3. Key Difference (Main Idea)

- Uninformed search = “Search without knowledge”
    
- Informed search = “Search with knowledge (intelligence)”
    

---

# 4. Difference Table (VERY IMPORTANT FOR EXAM)

|Feature|Uninformed Search|Informed Search|
|---|---|---|
|Definition|Searches without extra knowledge|Uses heuristic knowledge to guide search|
|Other name|Blind search|Heuristic search|
|Knowledge used|No domain knowledge|Uses domain knowledge (heuristics)|
|Goal direction|Not guided|Directed toward goal|
|Efficiency|Less efficient|More efficient|
|Speed|Slower|Faster|
|Memory usage|Usually high|Optimized due to guidance|
|Examples|BFS, DFS, UCS|A*, Hill Climbing, AO*|
|Optimal solution|Sometimes (BFS/UCS)|Often optimal (A*)|
|Decision making|Blind exploration|Intelligent decision making|

---

# 5. Simple Explanation (Easy to Remember)

- Uninformed search = **explores all paths without thinking**
    
- Informed search = **uses intelligence to choose best path**
    

---

# 6. Real-Life Example (Very Useful in Exams)

### Finding shortest path in city:

#### Uninformed Search:

- Try every road randomly
    
- Might take long time
    

#### Informed Search:

- Uses GPS + traffic + distance
    
- Directly chooses best route
    

---

# 7. Conclusion (Write in Exam)

Uninformed search explores the state space without any additional information about the goal, while informed search uses heuristics to guide the search process efficiently toward the goal. Therefore, informed search is more efficient and faster compared to uninformed search.

---

# Exam Probability

- Difference question → ⭐⭐⭐⭐⭐ VERY HIGH
    
- BFS/DFS comparison → ⭐⭐⭐⭐⭐
    
- A* link questions → ⭐⭐⭐⭐
    

---

# Informed Search (Heuristic Search)

🔥 **Exam Importance: VERY HIGH (Almost guaranteed long question in Unit-2)**

Informed search is the **smarter version of search algorithms** in AI.

---

# 1. What is Informed Search?

Informed search is a search strategy in which the algorithm uses **extra knowledge about the problem (called heuristic knowledge)** to find the goal faster.

This extra knowledge helps AI to decide:

- which path is better
    
- which node should be expanded first
    

---

## ✔ Exam Definition:

**Informed search is a search strategy that uses heuristic function (h(n)) to estimate the cost from current state to goal state and guides the search efficiently.**

---

# 2. Key Idea of Informed Search

- Instead of exploring blindly
    
- AI uses “intelligence” (heuristics)
    
- It selects the **most promising path first**
    

👉 So search becomes:  
**Goal-directed instead of random exploration**

---

# 3. Heuristic Function (VERY IMPORTANT)

🔥 Must write in exam

Heuristic function is written as:

**h(n)**

It gives:

- Estimated cost from node n to goal
    

---

## ✔ Meaning:

- Low h(n) → closer to goal
    
- High h(n) → far from goal
    

---

## ✔ Example:

If you are in city A and want to reach city D:

- A → D (100 km) → h(n) = 100
    
- B → D (50 km) → h(n) = 50
    

So B is better choice.

---

# 4. Components of Informed Search

Important for theory answer

### 1. Initial State

Starting node

### 2. Goal State

Target node

### 3. Heuristic Function (h(n))

Estimated cost to goal

### 4. Path Cost (g(n))

Actual cost from start to node

### 5. Evaluation Function

Used in A*:

👉 **f(n) = g(n) + h(n)**

---

# 5. How Informed Search Works

Step-by-step idea:

- Start from initial node
    
- Calculate heuristic value for all nodes
    
- Choose node with best (lowest) value
    
- Expand that node
    
- Repeat until goal is found
    

---

# 6. Advantages of Informed Search

🔥 Important points for exam

- Faster than uninformed search
    
- Explores fewer nodes
    
- More efficient
    
- Uses intelligent decision making
    
- Suitable for large problems
    

---

# 7. Disadvantages of Informed Search

- Requires good heuristic function
    
- If heuristic is wrong → wrong result
    
- Designing heuristic is difficult
    
- Can be expensive in complex systems
    

---

# 8. Types of Informed Search Strategies

🔥 VERY IMPORTANT (UNIT-2 core topic)

### 1. Greedy Best First Search

- Uses only h(n)
    
- Chooses node closest to goal
    

---

### 2. A* Search

- Uses both:
    
    - g(n) → cost so far
        
    - h(n) → estimated cost
        

👉 f(n) = g(n) + h(n)

Most important algorithm in AI

---

### 3. Hill Climbing

- Moves step by step toward better solution
    
- Local search method
    

---

### 4. AO* Algorithm

- Used for AND-OR graphs
    
- Solves complex problems by decomposition
    

---

# 9. Example of Informed Search

Suppose AI is solving maze problem:

Without heuristic:

- tries all paths randomly (slow)
    

With heuristic:

- always moves toward exit direction (fast)
    

---

# 10. Informed vs Uninformed (Quick Link)

|Feature|Informed Search|Uninformed Search|
|---|---|---|
|Knowledge|Uses heuristic|No knowledge|
|Speed|Fast|Slow|
|Efficiency|High|Low|
|Intelligence|Yes|No|

---

# 11. Short Note (Exam Ready)

Informed search is a search technique in which heuristic information is used to estimate the cost from current node to goal node. It improves efficiency by expanding only promising nodes. Common informed search methods are Greedy search, A*, Hill climbing and AO*.

---

# 12. Exam Probability

- Informed search definition → ⭐⭐⭐⭐⭐
    
- Heuristic function → ⭐⭐⭐⭐⭐
    
- A* algorithm → ⭐⭐⭐⭐⭐
    
- Comparison questions → ⭐⭐⭐⭐⭐
    
- Types → ⭐⭐⭐⭐
    

---

# Hill Climbing Algorithm (Heuristic Search)

🔥 **Exam Importance: VERY HIGH (Most asked 10 marks question in Unit-2)**

Hill Climbing is one of the **most important heuristic search techniques** in AI.

---

# 1. What is Hill Climbing?

Hill Climbing is a **heuristic search algorithm** used to find the **best solution by continuously improving the current state**.

It works like:  
👉 “Start from a solution and keep improving step by step until no better option is available.”

---

## ✔ Exam Definition:

**Hill Climbing is a local search algorithm in which the next state is chosen based on the best heuristic value among neighboring states, and the process continues until a goal or optimal state is reached.**

---

# 2. Key Idea of Hill Climbing

- Start with an initial solution
    
- Check neighboring states
    
- Move to the best neighbor
    
- Repeat until no better state exists
    

👉 It always tries to go “uphill” toward better solution

---

# 3. Types of Hill Climbing

🔥 Very important for exam

## 1. Simple Hill Climbing

- Checks one neighbor at a time
    
- If better → move
    
- If not → stop or continue checking
    

---

## 2. Steepest Ascent Hill Climbing

- Checks **all neighbors**
    
- Chooses the best among them
    
- More accurate than simple version
    

---

## 3. Stochastic Hill Climbing

- Randomly selects a better neighbor
    
- Does not check all states
    
- Faster but less precise
    

---

# 4. Working of Hill Climbing (Step-by-step)

- Start with initial state
    
- Evaluate current state using heuristic function
    
- Generate all possible neighboring states
    
- Compare their values
    
- Select the best neighbor
    
- Move to that state
    
- Repeat process
    
- Stop when no better neighbor exists
    

---

# 5. Example Idea (Easy Understanding)

Suppose AI is trying to find **highest peak in mountain area**

- Start at random position
    
- Move upward step by step
    
- Keep climbing higher
    
- Stop when no higher path exists
    

👉 That is why it is called “Hill Climbing”

---

# 6. State Space Diagram (Conceptual)

```
        50
       /  \
     40    60
    / \    / \
   30 45  55  70  ← Goal (best state)
```

AI moves:  
40 → 45 → 55 → 70 (best)

---

# 7. Evaluation Function

Hill Climbing uses heuristic:

👉 **h(n)** = value of current state

Goal:

- maximize or minimize h(n)
    

---

# 8. Properties of Hill Climbing

### ✔ Completeness

❌ Not complete  
(may not find solution)

---

### ✔ Optimality

❌ Not optimal  
(may stop at local best)

---

### ✔ Time Complexity

✔ Fast in practice  
Depends on state space

---

### ✔ Space Complexity

✔ Very low  
Only current state stored

---

# 9. Advantages of Hill Climbing

🔥 Important for exam

- Simple algorithm
    
- Requires very less memory
    
- Works well for optimization problems
    
- Fast compared to BFS/DFS
    
- Useful in large search spaces
    

---

# 10. Disadvantages of Hill Climbing

🔥 VERY IMPORTANT (often asked)

### 1. Local Maxima Problem

- Gets stuck in local best solution
    
- Not global best
    

---

### 2. Plateau Problem

- Flat area with same value
    
- No direction to move
    

---

### 3. Ridge Problem

- Difficult path like narrow ridge
    
- Hard to find correct direction
    

---

### 4. No Backtracking

- Once moved, cannot go back
    
- May miss better solution
    

---

# 11. Real Life Example

### Image Recognition / Optimization:

- AI improves model step by step
    
- Stops when no improvement found
    

### Route Optimization:

- AI improves path gradually
    
- Stops when best local route found
    

---

# 12. Hill Climbing Algorithm (Exam Steps)

1. Start with initial state
    
2. Evaluate current state
    
3. Generate neighbors
    
4. Select best neighbor
    
5. If better → move
    
6. Repeat
    
7. Stop when no improvement
    

---

# 13. Hill Climbing vs Other Search

|Feature|Hill Climbing|BFS|
|---|---|---|
|Type|Heuristic|Uninformed|
|Memory|Low|High|
|Optimal|No|Yes|
|Strategy|Local improvement|Level search|

---

# 14. Short Note (Exam Ready)

Hill climbing is a heuristic search technique used for optimization problems. It starts with an initial state and moves to the best neighboring state based on heuristic value. The process continues until no better neighbor exists. However, it may get stuck in local maxima, plateau or ridge problems.

---


# A* Search Algorithm

🔥 **Exam Importance: VERY VERY HIGH (Top scoring + most asked algorithm in Unit–2)**

A* is the **most important informed search algorithm** in AI exams.  
Almost guaranteed question: __“Explain A_ algorithm with example”_*

---

# 1. What is A* Search?

A* is an **informed (heuristic) search algorithm** used to find the **shortest and most optimal path** from a start node to a goal node.

It combines:

- Actual cost so far
    
- Estimated cost to goal
    

---

## ✔ Exam Definition:

__A_ search is a best-first search algorithm that uses both actual cost (g(n)) and heuristic cost (h(n)) to find the optimal solution efficiently._*

---

# 2. Key Idea of A*

A* selects the node that looks **most promising overall**, not just cheapest so far.

It uses:

👉 **f(n) = g(n) + h(n)**

Where:

- g(n) = cost from start to current node
    
- h(n) = estimated cost from current node to goal
    
- f(n) = total estimated cost
    

---

# 3. Meaning of Components

### 1. g(n) – Actual Cost

- Distance traveled from start node to current node
    
- Known cost
    

---

### 2. h(n) – Heuristic Cost

- Estimated cost from current node to goal
    
- Guess based on knowledge
    

---

### 3. f(n) – Total Cost

- Total estimated cost
    
- Used for selection
    

---

# 4. Working Principle of A*

Step-by-step:

- Start from initial node
    
- Calculate f(n) for all nodes
    
- Select node with lowest f(n)
    
- Expand that node
    
- Update costs of neighbors
    
- Repeat until goal is reached
    

---

# 5. Simple Example Idea

Suppose you are traveling:

- g(n): distance already traveled
    
- h(n): estimated distance to destination
    
- f(n): total expected distance
    

AI chooses path with **lowest total cost**

---

# 6. A* Algorithm Steps (Exam Writing Format)

1. Start with initial node
    
2. Add it to OPEN list
    
3. Keep CLOSED list empty
    
4. Repeat until goal found:
    
    - Select node with lowest f(n) from OPEN
        
    - Move it to CLOSED
        
    - Generate child nodes
        
    - Compute g(n), h(n), f(n)
        
    - Add/update nodes in OPEN
        
5. Stop when goal node is selected
    

---

# 7. Data Structures Used

- OPEN list → nodes to be explored
    
- CLOSED list → already explored nodes
    

---

# 8. Example (Small Graph Concept)

```id="k1s8d9"
Start (S)
   |
   A (f=6)
   |
   B (f=4)
   |
Goal (G)
```

A* selects node with lowest f(n) at each step → reaches goal optimally.

---

# 9. Properties of A*

## ✔ Completeness

✔ Yes (if branching factor is finite)

---

## ✔ Optimality

✔ Yes (if heuristic is admissible)

---

## ✔ Time Complexity

- Exponential in worst case
    
- But better than BFS in practice
    

---

## ✔ Space Complexity

- High (stores all generated nodes)
    

---

# 10. Condition for Optimal A*

A* is optimal if heuristic is:

### ✔ Admissible Heuristic

- Never overestimates actual cost
    
- h(n) ≤ true cost
    

---

# 11. Advantages of A*

🔥 Important for exam

- Finds optimal solution
    
- More efficient than BFS
    
- Uses intelligent guidance
    
- Reduces unnecessary search
    
- Widely used in real applications
    

---

# 12. Disadvantages of A*

- Requires good heuristic
    
- High memory usage
    
- Can be slow for large graphs
    
- Complex implementation
    

---

# 13. A* vs Other Algorithms

|Feature|A*|BFS|DFS|
|---|---|---|---|
|Type|Informed|Uninformed|Uninformed|
|Optimal|Yes|Yes|No|
|Uses heuristic|Yes|No|No|
|Efficiency|High|Medium|Low|
|Memory|High|High|Low|

---

# 14. Real Life Applications

- GPS navigation systems
    
- Google Maps shortest path
    
- Robotics path planning
    
- Games (path finding in NPCs)
    
- Network routing
    

---

# 15. Short Note (Exam Ready)

A* is an informed search algorithm that uses both actual cost g(n) and heuristic cost h(n) to compute f(n) = g(n) + h(n). It selects the node with minimum f(n) and expands it. A* is complete and optimal if heuristic is admissible and is widely used in pathfinding problems.

---

# AO* Algorithm (AND–OR Search Algorithm)

🔥 **Exam Importance: VERY HIGH (Often asked 10 marks, especially with A*)**

AO* is a **heuristic search algorithm used for AND–OR graphs**, where the problem is solved by **breaking it into subproblems**.

---

# 1. What is AO* Algorithm?

AO* (And-Or Star) is an **informed search algorithm** used to find the **optimal solution in AND-OR graphs** using heuristic information.

It is an extension of A* but designed for **problem decomposition**.

---

## ✔ Exam Definition:

__AO_ is a heuristic search algorithm used for solving problems represented by AND-OR graphs, where solution requires combining results of multiple subproblems._*

---

# 2. Key Idea of AO*

- Some problems can be divided into subproblems
    
- All subproblems must be solved (AND case)
    
- OR case means choose one best option
    
- AO* finds **minimum cost solution graph**
    

---

# 3. AND-OR Graph Concept (VERY IMPORTANT)

## ✔ OR Node

- Represents **choice**
    
- Only ONE path is needed
    

👉 Example:  
Choose route A OR B

---

## ✔ AND Node

- Represents **decomposition**
    
- ALL branches must be solved
    

👉 Example:  
To complete project:  
Task 1 AND Task 2 AND Task 3

---

# 4. Structure of AND-OR Graph

```id="m7k2q1"
        S
      /   \
     A     B
   (AND)  (OR)
   /  \     \
  C    D     E
```

- OR → choose best path
    
- AND → must solve both children
    

---

# 5. Working Principle of AO*

AO* works like A*, but for AND-OR graphs.

Step-by-step:

- Start from initial node
    
- Assign heuristic values h(n)
    
- Expand most promising node
    
- For OR nodes → select minimum cost child
    
- For AND nodes → sum cost of all children
    
- Update cost of parent nodes
    
- Repeat until solution graph is found
    

---

# 6. Cost Calculation in AO*

## ✔ For OR nodes:

👉 Cost = Minimum cost among children

---

## ✔ For AND nodes:

👉 Cost = Sum of all children costs

---

# 7. Algorithm Steps (Exam Format)

1. Start from initial node
    
2. Assign heuristic values to all nodes
    
3. Mark initial node as unsolved
    
4. Select most promising node
    
5. Expand node
    
6. Compute costs:
    
    - OR → minimum cost child
        
    - AND → sum of all children
        
7. Update parent node cost
    
8. Mark solved nodes
    
9. Repeat until root is solved
    

---

# 8. Example Idea (Simple Understanding)

Suppose goal is:

### To build software system:

- Option 1: Use tool A
    
- Option 2: Use modules B AND C AND D
    

AO* will:

- Compare OR path
    
- Add AND path cost
    
- Choose minimum cost solution
    

---

# 9. Properties of AO* Algorithm

## ✔ Completeness

✔ Yes (if finite graph)

---

## ✔ Optimality

✔ Yes (if heuristic is admissible)

---

## ✔ Time Complexity

- Exponential in worst case
    
- Better than brute force search
    

---

## ✔ Space Complexity

- High (stores graph structure)
    

---

# 10. Advantages of AO*

🔥 Important for exam

- Solves complex decomposition problems
    
- Finds optimal solution graph
    
- Uses heuristic efficiently
    
- Better than brute force AND-OR search
    
- Useful in real AI planning problems
    

---

# 11. Disadvantages of AO*

- Complex algorithm
    
- Difficult to design heuristic
    
- High memory usage
    
- Not suitable for simple problems
    
- Implementation is harder than A*
    

---

# 12. AO* vs A* (VERY IMPORTANT QUESTION)

|Feature|AO*|A*|
|---|---|---|
|Graph type|AND-OR graph|Simple graph|
|Problem type|Decomposition problems|Path finding|
|Cost calculation|AND = sum, OR = min|g(n)+h(n)|
|Usage|Complex AI problems|Shortest path problems|
|Complexity|Higher|Lower|

---

# 13. Applications of AO*

- Planning systems
    
- Game problem decomposition
    
- Expert systems
    
- Decision-making problems
    
- Robotics task planning
    

---

# 14. Short Note (Exam Ready)

AO* is a heuristic search algorithm used for solving AND-OR graphs. In OR nodes, the minimum cost path is selected, while in AND nodes all subproblems must be solved and their costs are added. AO* finds the optimal solution graph using heuristic guidance.

---


# Minimax Algorithm (Game Playing)

🔥 **Exam Importance: VERY VERY HIGH (Most expected 10-mark question in Unit–2)**

Minimax is the **fundamental algorithm for adversarial search (game playing)**. It is always asked along with **Alpha–Beta pruning**.

---

# 1. What is Minimax Algorithm?

Minimax is a decision-making algorithm used in **two-player games** where:

- One player tries to **maximize gain (MAX)**
    
- Other player tries to **minimize gain (MIN)**
    

👉 It assumes both players are **perfectly rational**.

---

## ✔ Exam Definition:

**Minimax is a recursive algorithm used in game playing that selects the optimal move for a player by assuming that the opponent also plays optimally.**

---

# 2. Key Idea of Minimax

- MAX player tries to get **maximum score**
    
- MIN player tries to reduce MAX score
    
- Game is represented as a **tree**
    
- Terminal states have utility values
    

---

# 3. Game Tree Concept

```id="minimax1"
            MAX
          /     \
        MIN     MIN
       /  \     /  \
      3    5   2    9
```

- MAX chooses best value from MIN results
    
- MIN chooses smallest value
    

---

# 4. Working Principle (Step-by-Step)

- Construct full game tree
    
- Assign utility values to leaf nodes
    
- Start from bottom (terminal nodes)
    
- MIN level → choose minimum value
    
- MAX level → choose maximum value
    
- Propagate values upward
    
- Root node gives optimal decision
    

---

# 5. Example (Simple Walkthrough)

### Step 1: Leaf nodes values

- 3, 5, 2, 9
    

### Step 2: MIN nodes

- MIN(3,5) = 3
    
- MIN(2,9) = 2
    

### Step 3: MAX node

- MAX(3,2) = 3
    

👉 Final decision = **3**

---

# 6. Minimax Algorithm (Steps for Exam Writing)

1. Generate game tree
    
2. Assign utility values to terminal nodes
    
3. If node is MAX level:
    
    - Choose maximum value from children
        
4. If node is MIN level:
    
    - Choose minimum value from children
        
5. Repeat recursively until root
    
6. Root value gives optimal move
    

---

# 7. Utility Function

Utility function gives score of a game state:

- Win → +10 (or +1)
    
- Lose → −10 (or −1)
    
- Draw → 0
    

---

# 8. Properties of Minimax

## ✔ Completeness

✔ Yes (if tree is finite)

---

## ✔ Optimality

✔ Yes (if opponent plays optimally)

---

## ✔ Time Complexity

- **O(b^d)**  
    b = branching factor  
    d = depth of tree
    

👉 Expensive for large games

---

## ✔ Space Complexity

- **O(bd)** (DFS based)
    

---

# 9. Advantages of Minimax

🔥 Important for exam

- Finds optimal move
    
- Works for competitive games
    
- Simple concept
    
- Basis of many AI game systems
    
- Ensures best decision against rational opponent
    

---

# 10. Disadvantages of Minimax

- Very slow for large trees
    
- High computation cost
    
- Not practical without optimization
    
- Assumes perfect opponent
    
- Cannot handle uncertainty well
    

---

# 11. Minimax with Depth Limit

In real systems:

- Full tree is not generated
    
- Search is limited to certain depth
    
- Heuristic evaluation is used
    

👉 Called **Depth-Limited Minimax**

---

# 12. Minimax vs Alpha–Beta (VERY IMPORTANT)

|Feature|Minimax|Alpha–Beta|
|---|---|---|
|Nodes|All explored|Pruned nodes|
|Speed|Slow|Faster|
|Result|Optimal|Optimal|
|Efficiency|Low|High|

---

# 13. Applications of Minimax

- Chess AI
    
- Tic-Tac-Toe
    
- Checkers
    
- Connect-4
    
- Any two-player competitive game
    

---

# 14. Short Note (Exam Ready)

Minimax is a recursive algorithm used in two-player games where one player tries to maximize gain and the other tries to minimize it. The algorithm builds a game tree, assigns utility values to terminal nodes, and propagates values upward using MAX and MIN rules. It ensures optimal decision making assuming both players play optimally.

---

# 15. Exam Probability

- Definition → ⭐⭐⭐⭐⭐
    
- Algorithm steps → ⭐⭐⭐⭐⭐
    
- Example tree → ⭐⭐⭐⭐⭐
    
- Properties → ⭐⭐⭐⭐⭐
    
- Comparison → ⭐⭐⭐⭐⭐
    



---


# Alpha–Beta Pruning

🔥 **Exam Importance: VERY HIGH (Almost always asked with Minimax in Unit–2)**

Alpha–Beta pruning is an **optimization technique of Minimax algorithm** used in **game playing (adversarial search)** to reduce the number of nodes evaluated.

---

# 1. What is Alpha–Beta Pruning?

Alpha–Beta pruning is a technique used in **game tree search** that **eliminates branches which cannot influence the final decision**.

👉 It improves the efficiency of Minimax by **skipping unnecessary nodes**.

---

## ✔ Exam Definition:

**Alpha–Beta pruning is an optimization of the Minimax algorithm that reduces the number of nodes evaluated in the search tree by pruning branches that cannot affect the final decision.**

---

# 2. Key Idea of Alpha–Beta Pruning

- Not all nodes need to be explored
    
- If a move is clearly worse, ignore it
    
- Save time and computation
    
- Same result as Minimax, but faster
    

---

# 3. Why Alpha–Beta Pruning is Needed?

Minimax explores:

- Entire game tree
    
- Very large number of nodes
    

Problem:

- Too slow
    
- Not practical for deep games
    

👉 Alpha–Beta solves this by pruning useless branches.

---

# 4. Meaning of Alpha and Beta

## ✔ Alpha (α)

- Best value for **MAX player so far**
    
- Maximum lower bound
    

👉 Represents: “Best option found for MAX”

---

## ✔ Beta (β)

- Best value for **MIN player so far**
    
- Minimum upper bound
    

👉 Represents: “Best option found for MIN”

---

# 5. Core Rule of Pruning

👉 If at any point:

**α ≥ β → Stop exploring that branch (PRUNE)**

This is the most important rule.

---

# 6. Working of Alpha–Beta Pruning (Step-by-Step)

- Start from root node
    
- Initialize:
    
    - α = −∞
        
    - β = +∞
        
- Traverse tree using Minimax order
    
- Update α and β values
    
- At each node check condition:
    
    - If α ≥ β → prune branch
        
- Continue until root decision is made
    

---

# 7. Simple Example Idea

### Game Tree Concept:

```
           MAX
        /        \
      MIN        MIN
     /   \      /   \
    3     5    2     9
```

### Steps:

- Left MIN gives value = 3
    
- Right MIN starts exploring
    
- If 2 already worse than 3 (depending on context)
    
- Remaining branch may be pruned
    

👉 Unnecessary nodes are skipped

---

# 8. Types of Pruning

## ✔ Alpha Cutoff

- Happens at MAX node
    
- When α ≥ β
    
- Stop exploring further MIN branches
    

---

## ✔ Beta Cutoff

- Happens at MIN node
    
- When β ≤ α
    
- Stop exploring further MAX branches
    

---

# 9. Alpha–Beta Algorithm (Exam Steps)

1. Initialize α = −∞, β = +∞
    
2. Start Minimax traversal
    
3. At MAX node:
    
    - Update α = max(α, value)
        
4. At MIN node:
    
    - Update β = min(β, value)
        
5. If α ≥ β → prune remaining branches
    
6. Return best value
    

---

# 10. Properties of Alpha–Beta Pruning

## ✔ Completeness

✔ Same as Minimax (complete if tree finite)

---

## ✔ Optimality

✔ Same optimal result as Minimax

---

## ✔ Time Complexity

- Best case: **O(b^(d/2))**
    
- Worst case: **O(b^d)** (no pruning)
    

👉 Very efficient in best case

---

## ✔ Space Complexity

- Same as Minimax: O(bd)
    

---

# 11. Advantages of Alpha–Beta Pruning

🔥 Important exam points

- Reduces number of nodes evaluated
    
- Faster than Minimax
    
- Gives same optimal result
    
- Improves efficiency significantly
    
- Useful in real-time games
    

---

# 12. Disadvantages

- Depends on move ordering
    
- Worst case same as Minimax
    
- Still exponential in nature
    
- Complex implementation in large trees
    

---

# 13. Alpha–Beta vs Minimax (VERY IMPORTANT)

|Feature|Minimax|Alpha–Beta|
|---|---|---|
|Nodes explored|All|Fewer (pruned)|
|Speed|Slow|Faster|
|Result|Optimal|Optimal|
|Efficiency|Low|High|
|Technique|Basic|Optimized|

---

# 14. Applications

- Chess AI
    
- Tic-Tac-Toe
    
- Checkers
    
- Game playing programs
    
- Decision making in competitive environments
    

---

# 15. Short Note (Exam Ready)

Alpha–Beta pruning is an optimization technique of Minimax algorithm used in game playing. It eliminates branches that do not affect final decision using α and β values. If α ≥ β, the branch is pruned. It reduces computation and improves efficiency while giving the same optimal result as Minimax.

---

# 16. Exam Probability

- Definition → ⭐⭐⭐⭐⭐
    
- Alpha & Beta explanation → ⭐⭐⭐⭐⭐
    
- Algorithm steps → ⭐⭐⭐⭐⭐
    
- Pruning condition → ⭐⭐⭐⭐⭐
    
- Minimax comparison → ⭐⭐⭐⭐⭐
    

---

# Next Topic (VERY IMPORTANT 🔥)

👉 **Minimax Algorithm (core game playing question)**

Say **next** 👍
