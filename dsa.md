
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

```sh
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

`INSERT(root, key) 1. If root == NULL        create new node and return 2. If key < root.data        INSERT(root.left, key) 3. Else if key > root.data        INSERT(root.right, key)`

---

### **Explanation**

- Compare key with root
    
- Move left or right based on comparison
    
- Insert when NULL position is found
    

---

### **Example: Insert 25**

Initial BST:

      `30      /  \    20   40`

After inserting 25:

      `30      /  \    20   40      \      25`

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

`Before:          After:    30              30   /  \            /  \ 20   40          NULL 40`

---

## **Case 2: Deleting a Node with One Child**

### **Condition**

- Node has **only one child**
    

### **Action**

- Replace node with its child
    

### **Example**

Delete **30**:

`Before:          After:    30              40      \               \      40               50        \         50`

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

`Before:         50        /  \      30    70            /          60  Inorder Successor of 50 = 60`

`After:         60        /  \      30    70`

---

### **Algorithm: Delete in BST**

`DELETE(root, key) 1. If root == NULL        return root 2. If key < root.data        root.left = DELETE(root.left, key) 3. Else if key > root.data        root.right = DELETE(root.right, key) 4. Else        If root has no child             delete node        Else if root has one child             replace with child        Else             find inorder successor             copy value             delete successor 5. Return root`

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