
----

# **🔴 UNIT–3 (Binary Trees & BST)**

# **1️⃣ Binary Search Tree (BST)** ⭐⭐⭐⭐⭐

_(10 Marks Answer)_

---

## **Definition of Binary Search Tree (BST)**

A **Binary Search Tree (BST)** is a special type of **binary tree** in which:

- Each node has **at most two children**
    
- The tree follows a specific **ordering property**
    

### **BST Rule (Core Definition)**

For every node **N**:

- All values in the **left subtree** of N are **less than N**
    
- All values in the **right subtree** of N are **greater than N**
    
- Both left and right subtrees are **also BSTs**
    

---

## **Properties of Binary Search Tree**

1. Each node has **maximum two children**
    
2. **Left subtree contains smaller values**
    
3. **Right subtree contains larger values**
    
4. **No duplicate elements** (in standard BST)
    
5. **Inorder traversal of BST gives sorted order**
    
6. Searching, insertion, and deletion take **O(h)** time  
    where **h = height of tree**
    

---

## **Example of a BST**
```
        50
       /  \
     30    70
    / \    / \
  20  40  60  80
```

---

## **Searching in a Binary Search Tree**

### **Idea**

- Compare the key with current node
    
- Move left if key is smaller
    
- Move right if key is greater
    

---

### **Algorithm: Search in BST**

```java
SEARCH(root, key)
1. If root == NULL or root.data == key
       return root
2. If key < root.data
       return SEARCH(root.left, key)
3. Else
       return SEARCH(root.right, key)
```

---

### **Explanation**

- Start from root
    
- If key is equal → element found
    
- If key is smaller → go to left subtree
    
- If key is larger → go to right subtree
    
- Continue until key is found or tree ends
    

---

### **Time Complexity**

- Best Case: O(log n)
    
- Worst Case (skewed tree): O(n)
    

---

## **Insertion in a Binary Search Tree**

### **Idea**

- Insert the new element at the **correct position**
    
- BST property must remain unchanged
    

---

### **Algorithm: Insert in BST**

```java
INSERT(root, key)
1. If root == NULL
       create new node and return
2. If key < root.data
       INSERT(root.left, key)
3. Else if key > root.data
       INSERT(root.right, key)
```

---

### **Explanation**

- Compare key with root
    
- Move left or right based on comparison
    
- Insert when NULL position is found
    

---

### **Example: Insert 25**

Initial BST:

```
      30
     /  \
   20   40
```

After inserting 25:

```
      30
     /  \
   20   40
     \
     25
```

---

## **Deletion in a Binary Search Tree**

Deletion is the **most important and tricky part** of BST.  
There are **THREE CASES**.

---

## **Case 1: Deleting a Leaf Node**

### **Condition**

- Node has **no children**
    

### **Action**

- Simply remove the node
    

### **Example**

Delete **20**:

```
Before:          After:
   30              30
  /  \            /  \
20   40          NULL 40
```

---

## **Case 2: Deleting a Node with One Child**

### **Condition**

- Node has **only one child**
    

### **Action**

- Replace node with its child
    

### **Example**

Delete **30**:

```
Before:          After:
   30              40
     \               \
     40               50
       \
        50

```

---

## **Case 3: Deleting a Node with Two Children** ⭐⭐⭐⭐⭐

### **Condition**

- Node has **both left and right children**
    

### **Action (Using Inorder Successor)**

Steps:

1. Find **inorder successor** (smallest value in right subtree)
    
2. Copy successor value to node
    
3. Delete successor node
    

---

### **Inorder Successor**

- Smallest element in the **right subtree**
    
- Found by going **leftmost** in right subtree
    

---

### **Example**

Delete **50**:

```
Before:
        50
       /  \
     30    70
           /
         60

Inorder Successor of 50 = 60

After:
        60
       /  \
     30    70

```


---

### **Algorithm: Delete in BST**

```java
DELETE(root, key)
1. If root == NULL
       return root
2. If key < root.data
       root.left = DELETE(root.left, key)
3. Else if key > root.data
       root.right = DELETE(root.right, key)
4. Else
       If root has no child
            delete node
       Else if root has one child
            replace with child
       Else
            find inorder successor
            copy value
            delete successor
5. Return root
```

---

## **Time Complexity of BST Operations**

|Operation|Time Complexity|
|---|---|
|Search|O(h)|
|Insert|O(h)|
|Delete|O(h)|

_(h = height of tree)_

---

## **Advantages of BST**

1. Faster searching than arrays
    
2. Maintains sorted data
    
3. Efficient insertion and deletion
    

---

## **Disadvantages of BST**

1. Can become **skewed**
    
2. Worst case time complexity is **O(n)**
    
3. Needs balancing (AVL, Red-Black Trees)

---



# **2️⃣ Tree Traversals — DFS & BFS** ⭐⭐⭐⭐⭐

_(10 Marks Answer)_

---

## **Introduction to Tree Traversal**

**Tree traversal** is the process of **visiting each node of a tree exactly once** in a specific order.

Tree traversals are mainly classified into:

1. **Depth First Search (DFS)**
    
2. **Breadth First Search (BFS)**
    

---

## **1️⃣ Depth First Search (DFS)**

### **Definition**

**Depth First Search (DFS)** is a tree traversal technique in which:

- We explore a node **as deep as possible** before backtracking.
    

DFS is mainly implemented using **recursion** or **stack**.

---

### **Types of DFS Traversals**

DFS has **three types**:

1. **Inorder Traversal**
    
2. **Preorder Traversal**
    
3. **Postorder Traversal**
    

---

### **Example Tree (for all DFS traversals)**

```
        A
       / \
      B   C
     / \   \
    D   E   F
```

---

## **1️⃣ Inorder Traversal (Left → Root → Right)**

### **Definition**

In **Inorder traversal**, nodes are visited in the following order:

1. Left subtree
    
2. Root node
    
3. Right subtree
    

---

### **Algorithm: Inorder Traversal**

```java
INORDER(root)
1. If root ≠ NULL
2.    INORDER(root.left)
3.    Visit root
4.    INORDER(root.right)
```

---

### **Inorder Traversal Output**

```
D  B  E  A  C  F
```

---

### **Special Property**

- Inorder traversal of a **BST gives sorted order**
    

---

## **2️⃣ Preorder Traversal (Root → Left → Right)**

### **Definition**

In **Preorder traversal**, nodes are visited in:

1. Root
    
2. Left subtree
    
3. Right subtree
    

---

### **Algorithm: Preorder Traversal**

```java
PREORDER(root)
1. If root ≠ NULL
2.    Visit root
3.    PREORDER(root.left)
4.    PREORDER(root.right)
```

---

### **Preorder Traversal Output**

`A  B  D  E  C  F`

---

### **Application**

- Used to **create a copy of tree**
    
- Used in **expression tree evaluation**
    

---

## **3️⃣ Postorder Traversal (Left → Right → Root)**

### **Definition**

In **Postorder traversal**, nodes are visited in:

1. Left subtree
    
2. Right subtree
    
3. Root node
    

---

### **Algorithm: Postorder Traversal**

```java
POSTORDER(root)
1. If root ≠ NULL
2.    POSTORDER(root.left)
3.    POSTORDER(root.right)
4.    Visit root
```

---

### **Postorder Traversal Output**

`D  E  B  F  C  A`

---

### **Application**

- Used for **deleting a tree**
    
- Used in **postfix expression evaluation**
    

---

## **Time Complexity of DFS Traversals**

- **O(n)** for all DFS traversals
    
- n = number of nodes
    

---

## **2️⃣ Breadth First Search (BFS)**

### **Definition**

**Breadth First Search (BFS)** is a traversal technique where:

- Nodes are visited **level by level**
    
- Also called **Level Order Traversal**
    
- Implemented using a **queue**
    

---

### **Algorithm: Level Order Traversal**

```java
LEVELORDER(root)
1. If root == NULL return
2. Create empty queue Q
3. Enqueue root into Q
4. While Q is not empty
5.    temp = Dequeue Q
6.    Visit temp
7.    If temp.left ≠ NULL
         Enqueue temp.left
8.    If temp.right ≠ NULL
         Enqueue temp.right
```

---

### **Level Order Traversal Output**

`A  B  C  D  E  F`

---

## **Difference Between DFS and BFS** ⭐⭐⭐⭐⭐

|Basis|DFS|BFS|
|---|---|---|
|Full Form|Depth First Search|Breadth First Search|
|Traversal Style|Goes deep first|Goes level by level|
|Data Structure Used|Stack / Recursion|Queue|
|Memory Usage|Less|More|
|Applications|Expression trees|Shortest path|
|Examples|Inorder, Preorder, Postorder|Level Order|

---

## **Advantages of Tree Traversals**

1. Easy access to all nodes
    
2. Helps in searching and sorting
    
3. Useful in expression evaluation


---


# **3️⃣ Trees & Binary Trees (Basics)** ⭐⭐⭐⭐

_(10 Marks Answer)_

---

## **Definition of Tree**

A **tree** is a **non-linear data structure** that consists of:

- A finite set of **nodes**
    
- Connected by **edges**
    
- With one special node called the **root**
    

A tree represents **hierarchical relationships** between elements.

---

### **Formal Definition**

A tree is a collection of nodes such that:

- One node is designated as the **root**
    
- Remaining nodes are divided into **disjoint subtrees**
    
- Each subtree is itself a tree
    

---

## **Definition of Binary Tree**

A **binary tree** is a special type of tree in which:

- Each node has **at most two children**
    
- These children are called:
    
    - **Left child**
        
    - **Right child**
        

---

### **Formal Definition**

A binary tree is a finite set of nodes which is either:

- Empty, or
    
- Consists of a root node and two disjoint binary trees called the **left subtree** and **right subtree**
    

---

## **Example of Binary Tree**

```
        A
       / \
      B   C
     / \
    D   E
```

---

## **Properties of Binary Tree**

1. Maximum number of children per node = **2**
    
2. Children are ordered as **left and right**
    
3. Maximum nodes at level _i_ = **2ⁱ**
    
4. Maximum nodes in height _h_ = **2ʰ − 1**
    
5. Minimum height for _n_ nodes = **⌈log₂(n+1)⌉**
    
6. A binary tree can be **empty**
    

---

## **Difference Between Tree and Binary Tree** ⭐⭐⭐⭐

|Basis|Tree|Binary Tree|
|---|---|---|
|Max children|Any number|At most 2|
|Order of children|Not ordered|Ordered (Left & Right)|
|Subtrees|Any number|At most 2|
|Applications|File system|Searching, expressions|
|Example|General hierarchy|BST, Heap|

---

## **Tree Terminology** ⭐⭐⭐⭐⭐

### **1️⃣ Degree of a Node**

- Number of children of a node
    
- Degree of tree = maximum degree of any node
    

---

### **2️⃣ Root Node**

- The topmost node of a tree
    
- Has **no parent**
    

---

### **3️⃣ Leaf Node**

- Node with **no children**
    
- Also called **external node**
    

---

### **4️⃣ Parent Node**

- A node which has one or more children
    

---

### **5️⃣ Child Node**

- Node directly connected below a parent
    

---

### **6️⃣ Level of a Node**

- Level of root = **0**
    
- Level of any node = level of parent + 1
    

---

### **7️⃣ Height of a Tree**

- Number of edges on the **longest path** from root to leaf
    
- Height = max level in the tree
    

---

### **8️⃣ Depth of a Node**

- Number of edges from root to that node
    

---

### **9️⃣ Sibling Nodes**

- Nodes having the **same parent**
    

---

### **10️⃣ Subtree**

- A tree formed by a node and its descendants


---



# **4️⃣ Properties, Types & Representation of Binary Trees** ⭐⭐⭐⭐

_(10 Marks Answer — Java Reference)_

---

## **Introduction**

A **binary tree** is a non-linear hierarchical data structure in which:

- Each node has **at most two children**
    
- These children are called **left child** and **right child**
    

Binary trees are widely used in **searching, sorting, heaps, expression trees, and BSTs**.

---

## **Properties of Binary Trees**

1. Maximum number of nodes at level _i_ = **2ⁱ**
    
2. Maximum number of nodes in a binary tree of height _h_ = **2ʰ − 1**
    
3. Minimum number of nodes in a binary tree of height _h_ = **h + 1**
    
4. Minimum height of a binary tree with _n_ nodes = **⌈log₂(n + 1)⌉**
    
5. Each node has **at most two children**
    
6. A binary tree can be **empty**
    

---

## **Types of Binary Trees** ⭐⭐⭐⭐⭐

---

## **1️⃣ Full Binary Tree**

### **Definition**

A **full binary tree** is a binary tree in which:

- Every node has **either 0 or 2 children**
    

---

### **Example**
```
        A
       / \
      B   C
     / \
    D   E
```

---

### **Key Property**

- No node has **exactly one child**
    

---

## **2️⃣ Complete Binary Tree**

### **Definition**

A **complete binary tree** is a binary tree in which:

- All levels are completely filled
    
- Except possibly the **last level**
    
- Last level nodes are filled **from left to right**
    

---

### **Example**

```
        A
       / \
      B   C
     / \  /
    D  E F
```

---

### **Application**

- Used in **Heap data structure**
    

---

## **3️⃣ Perfect Binary Tree**

### **Definition**

A **perfect binary tree** is a binary tree in which:

- All internal nodes have **two children**
    
- All leaf nodes are at the **same level**
    

---

### **Example**

```
        A
       / \
      B   C
     / \ / \
    D  E F  G
```

---

### **Property**

- Total number of nodes = **2ʰ − 1**
    

---

## **4️⃣ Skewed Binary Tree**

### **Definition**

A **skewed binary tree** is a binary tree in which:

- Every node has **only one child**
    

---

### **Types**

1. **Left-skewed binary tree**
    
2. **Right-skewed binary tree**
    

---

### **Example (Right Skewed)**

```
A
 \
  B
   \
    C
     \
      D
```

---

### **Disadvantage**

- Time complexity becomes **O(n)** (like a linked list)
    

---

## **Representation of Binary Trees in Memory** ⭐⭐⭐⭐⭐

Binary trees are represented in memory in **two ways**:

---

## **1️⃣ Array Representation**

### **Concept**

- Binary tree nodes are stored in an **array**
    
- Best suited for **complete binary trees**
    

---

### **Index Formula**

If a node is at index **i**:

- Left child → **2i**
    
- Right child → **2i + 1**
    
- Parent → **⌊i / 2⌋**
    

---

### **Example**

```
        A
       / \
      B   C
     / \
    D   E
```

Array representation:

```
Index:  1  2  3  4  5
Value:  A  B  C  D  E
```

---

### **Java Representation (Array)**

```
char[] tree = {' ', 'A', 'B', 'C', 'D', 'E'};
```

---

### **Advantages**

1. Simple implementation
    
2. No pointer overhead
    
3. Fast access
    

---

### **Disadvantages**

1. Wastage of memory for sparse trees
    
2. Not suitable for skewed trees
    

---

## **2️⃣ Linked Representation**

### **Concept**

- Each node contains:
    
    - Data
        
    - Reference to left child
        
    - Reference to right child
        

---

### **Java Node Structure**

`class Node {     int data;     Node left, right;      Node(int data) {         this.data = data;         left = right = null;     } }`

---

### **Example Structure**

      `[A]      /   \    [B]   [C]    / \  [D] [E]`

---

### **Advantages**

1. Efficient memory usage
    
2. Dynamic structure
    
3. Suitable for all types of trees
    

---

### **Disadvantages**

1. Extra memory for pointers
    
2. Slightly complex than array representation
    

---

## **Comparison of Representations**

|Basis|Array Representation|Linked Representation|
|---|---|---|
|Memory usage|Efficient for complete trees|Efficient for sparse trees|
|Flexibility|Less|More|
|Pointer required|No|Yes|
|Best used for|Heaps|BST, Expression Trees|