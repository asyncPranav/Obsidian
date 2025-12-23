
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

`INORDER(root) 1. If root ≠ NULL 2.    INORDER(root.left) 3.    Visit root 4.    INORDER(root.right)`

---

### **Inorder Traversal Output**

`D  B  E  A  C  F`

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

`PREORDER(root) 1. If root ≠ NULL 2.    Visit root 3.    PREORDER(root.left) 4.    PREORDER(root.right)`

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

`POSTORDER(root) 1. If root ≠ NULL 2.    POSTORDER(root.left) 3.    POSTORDER(root.right) 4.    Visit root`

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

`LEVELORDER(root) 1. If root == NULL return 2. Create empty queue Q 3. Enqueue root into Q 4. While Q is not empty 5.    temp = Dequeue Q 6.    Visit temp 7.    If temp.left ≠ NULL          Enqueue temp.left 8.    If temp.right ≠ NULL          Enqueue temp.right`

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