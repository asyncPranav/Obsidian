
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


