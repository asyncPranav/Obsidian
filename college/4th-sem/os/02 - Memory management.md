
---


# PAGING

## Definition

Paging is a memory management technique in which the logical memory of a process is divided into fixed-size blocks called **pages** and the physical memory is divided into fixed-size blocks called **frames**. The pages of a process can be loaded into any available frame in main memory.

---

## Need of Paging

In contiguous memory allocation, a process requires a continuous block of memory. Due to scattered free memory spaces, memory utilization becomes inefficient and causes **external fragmentation**.

Paging was introduced to:

- Eliminate external fragmentation.
    
- Improve memory utilization.
    
- Allow non-contiguous memory allocation.
    

---

## Basic Concept of Paging

In paging:

- Process memory is divided into **Pages**.
    
- Main memory is divided into **Frames**.
    
- Size of page = Size of frame.
    
- Any page can be placed into any available frame.
    

### Diagram

```text
Process                      Main Memory

Page 0  ------------------>  Frame 3

Page 1  ------------------>  Frame 0

Page 2  ------------------>  Frame 5

Page 3  ------------------>  Frame 2
```

---

## Page Table

A Page Table is maintained by the Operating System to keep track of the mapping between pages and frames.

### Example

|Page Number|Frame Number|
|---|---|
|0|3|
|1|0|
|2|5|
|3|2|

The page table helps the operating system locate pages in physical memory.

---

## Address Translation in Paging

The CPU generates a **Logical Address** which consists of:

```text
Logical Address

+------------+----------+
| Page Number| Offset   |
+------------+----------+
```

The page number is used to search the page table.

The corresponding frame number is obtained and combined with the same offset to generate the physical address.

```text
Physical Address

+-------------+---------+
| Frame Number| Offset  |
+-------------+---------+
```

### Address Translation Diagram

```text
Logical Address
(Page No, Offset)
        |
        v

     Page Table
        |
        v

(Frame No, Offset)

Physical Address
```

---

## Advantages of Paging

1. Eliminates external fragmentation.
    
2. Better memory utilization.
    
3. Supports virtual memory.
    
4. Allows efficient memory allocation.
    
5. Enables multiprogramming.
    

---

## Disadvantages of Paging

1. Causes internal fragmentation.
    
2. Requires additional memory for page tables.
    
3. Increases address translation overhead.
    
4. Requires hardware support such as MMU.
    

---

## Conclusion

Paging is an important memory management technique that divides memory into pages and frames. It improves memory utilization and eliminates external fragmentation by allowing non-contiguous memory allocation.

---

# 5 Marks Short Answer

**Paging is a memory management technique in which logical memory is divided into pages and physical memory is divided into frames. Pages can be loaded into any available frame. A page table is used to map pages to frames. Paging eliminates external fragmentation and improves memory utilization.**

---

# Last-Minute Exam Keywords

Memorize these words:

```text
Paging
↓
Pages
↓
Frames
↓
Page Table
↓
Address Translation
↓
External Fragmentation Removed
```

If you write:

- Definition
    
- Diagram
    
- Page Table
    
- Address Translation
    
- 5 Advantages
    
- 4 Disadvantages
    

you can easily score **8–10 marks out of 10** or **12–14 marks out of 15** on a paging question.

**Professor's exam tip:** In OS exams, a neat diagram of Pages → Frames and Logical Address → Page Table → Physical Address often earns extra marks even if the theory is not perfectly written.