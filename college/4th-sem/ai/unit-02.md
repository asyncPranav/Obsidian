
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

If you want next:  
👉 **Heuristic Search (Hill Climbing, A*, AO*) – VERY IMPORTANT UNIT 2 TOPIC**

Just say **next** 👍