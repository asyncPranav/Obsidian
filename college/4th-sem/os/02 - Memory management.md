
---

# UNIT 2 – PAGING (Most Important Topic) ⭐⭐⭐⭐⭐

**Exam Weightage:** Very High  
**Expected:** 5 Marks, 10 Marks, 15 Marks

---

# Why Was Paging Introduced?

Before paging, memory was allocated continuously (contiguous allocation).

Suppose a process needs 10 MB memory.

OS tries to find one continuous 10 MB block.

Sometimes memory is available but scattered:

```text
2 MB Free
3 MB Free
5 MB Free
```

Total = 10 MB available

But not continuous.

Process cannot be loaded.

This problem is called **External Fragmentation**.

👉 Paging was introduced to solve this problem.

---

# What is Paging?

## Exam Definition

**Paging is a memory management technique in which the logical memory of a process is divided into fixed-size pages and physical memory is divided into fixed-size frames. Pages can be loaded into any available frame in memory.**

---

## Easy Language

Think of a large book.

Instead of keeping the entire book together, we divide it into pages.

Similarly,

Process → divided into Pages

Memory → divided into Frames

And any page can be stored in any frame.

---

# Golden Rule of Paging

Remember this line:

### Pages go into Frames

```text
Process Memory  → Pages
Main Memory     → Frames
```

Examiner loves this statement.

---

# Basic Structure

Suppose:

Process Size = 16 KB

Page Size = 4 KB

Then:

```text
Page 0 = 4 KB
Page 1 = 4 KB
Page 2 = 4 KB
Page 3 = 4 KB
```

Total = 4 Pages

---

Physical Memory:

```text
Frame 0 = 4 KB
Frame 1 = 4 KB
Frame 2 = 4 KB
Frame 3 = 4 KB
Frame 4 = 4 KB
Frame 5 = 4 KB
```

---

# Paging Diagram

This diagram is extremely important.

```text
PROCESS

Page 0
Page 1
Page 2
Page 3

        ↓

PHYSICAL MEMORY

Frame 5 ← Page 0
Frame 2 ← Page 1
Frame 7 ← Page 2
Frame 1 ← Page 3
```

Notice:

Pages are NOT stored sequentially.

This is the beauty of paging.

---

# Page Table

Now a question arises:

How does OS know where pages are stored?

Answer:

Using a **Page Table**.

---

## What is Page Table?

A page table stores the mapping between pages and frames.

---

Example:

|Page Number|Frame Number|
|---|---|
|0|5|
|1|2|
|2|7|
|3|1|

This table is called Page Table.

---

# Memory Trick

Remember:

```text
Page Table =
Directory of pages
```

Just like a school register tells which student sits on which bench.

Page table tells:

```text
Which page is stored in which frame.
```

---

# Address Translation in Paging

🔥 MOST IMPORTANT PART

Almost every university asks this.

---

## Question

CPU generates:

```text
Logical Address
```

But memory understands:

```text
Physical Address
```

How do we convert?

Answer:

Using Page Table.

---

# Logical Address Structure

A logical address consists of:

```text
Page Number + Offset
```

```text
Logical Address

+---------+--------+
| Page No | Offset |
+---------+--------+
```

---

## Meaning

### Page Number

Tells which page we want.

### Offset

Tells exact location inside the page.

---

# Physical Address Structure

```text
Frame Number + Offset
```

```text
Physical Address

+----------+--------+
| Frame No | Offset |
+----------+--------+
```

---

# Address Translation Process

Suppose:

Logical Address:

```text
(Page 2, Offset 100)
```

Page Table:

|Page|Frame|
|---|---|
|0|5|
|1|2|
|2|7|
|3|1|

---

Step 1:

Look up Page 2.

```text
Page 2 → Frame 7
```

Step 2:

Keep offset same.

```text
Offset = 100
```

Step 3:

Physical Address:

```text
(Frame 7, Offset 100)
```

Done.

---

# Address Translation Diagram

```text
Logical Address

(Page 2,100)
        |
        v

   Page Table

Page 2 → Frame 7

        |
        v

Physical Address

(Frame 7,100)
```

This diagram is worth drawing in exam.

---

# Advantages of Paging

## 1. Eliminates External Fragmentation

Biggest advantage.

Memory can be used efficiently.

---

## 2. Better Memory Utilization

Free frames can be used anywhere.

---

## 3. Supports Virtual Memory

Modern operating systems depend on paging.

---

## 4. Easy Allocation

No need for continuous memory.

---

## 5. Efficient Multiprogramming

Many processes can coexist.

---

# Disadvantages of Paging

## 1. Internal Fragmentation

Some space inside page may remain unused.

Example:

Page Size = 4 KB

Need = 10 KB

Allocated = 12 KB

Unused = 2 KB

This unused memory is Internal Fragmentation.

---

## 2. Page Table Overhead

Need extra memory for page tables.

---

## 3. Address Translation Overhead

Extra time needed for page lookup.

---

## 4. Complex Hardware Support

Requires MMU (Memory Management Unit).

---

# Internal vs External Fragmentation

Very Important Short Question

|Internal Fragmentation|External Fragmentation|
|---|---|
|Wastage inside allocated block|Wastage between blocks|
|Occurs in Paging|Occurs in Contiguous Allocation|
|Small unused space|Scattered free spaces|

---

# Real-Life Example

Imagine a hotel.

## Process = Tourist Group

16 tourists arrive.

They are divided into groups of 4.

```text
Page 0 = 4 tourists
Page 1 = 4 tourists
Page 2 = 4 tourists
Page 3 = 4 tourists
```

---

Hotel Rooms:

```text
Frame 1
Frame 2
Frame 3
Frame 4
Frame 5
Frame 6
```

Groups can stay in any room.

No need for consecutive rooms.

This is exactly Paging.

---

# 5 Marks Answer (Quick Revision)

**Paging is a memory management technique in which logical memory is divided into fixed-size pages and physical memory is divided into fixed-size frames. A page can be placed in any available frame. The mapping between pages and frames is maintained using a page table. Paging eliminates external fragmentation and improves memory utilization.**

---

# 10-15 Marks Exam Answer Structure

When asked:

### "Explain Paging with diagram."

Write in this order:

1. Definition
    
2. Why Paging is needed
    
3. Pages and Frames
    
4. Paging Diagram
    
5. Page Table
    
6. Address Translation
    
7. Advantages
    
8. Disadvantages
    
9. Conclusion
    

---

# Super Memory Hack (1 Minute Revision)

Remember the keyword:

### PFTA

**P → Pages**  
**F → Frames**  
**T → Page Table**  
**A → Address Translation**

If you remember **PFTA**, you can reconstruct the entire Paging chapter in the exam.

---

Next topic should be **Segmentation ⭐⭐⭐⭐⭐**, because once you understand Segmentation, the famous **"Paging vs Segmentation"** question becomes very easy and is asked almost every year.