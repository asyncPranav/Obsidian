
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


---

# SEGMENTATION

## Definition

**Segmentation is a memory management technique in which a program is divided into variable-sized logical units called segments. Each segment represents a logical part of a program such as code, data, stack, or functions.**

---

# Need of Segmentation

In paging, memory is divided into fixed-size pages without considering the logical structure of the program.

However, programmers think in terms of:

- Functions
    
- Variables
    
- Data
    
- Stack
    

Segmentation divides memory according to these logical units, making memory management more meaningful and flexible.

---

# Basic Concept of Segmentation

A program is divided into several segments of different sizes.

Example:

```text
Program

Segment 0 → Main Program
Segment 1 → Function A
Segment 2 → Function B
Segment 3 → Stack
```

Each segment can be stored at different locations in physical memory.

---

# Diagram of Segmentation

```text
Process

Segment 0 (Code)

Segment 1 (Data)

Segment 2 (Function)

Segment 3 (Stack)



Physical Memory

-----------------
| Segment 2     |
-----------------

-----------------
| Segment 0     |
-----------------

-----------------
| Segment 3     |
-----------------

-----------------
| Segment 1     |
-----------------
```

**Important Point:** Segments are variable in size and need not be stored continuously.

---

# Segment Table

A Segment Table is maintained by the Operating System to store information about each segment.

Each entry contains:

- Base Address
    
- Limit
    

Example:

|Segment No.|Base Address|Limit|
|---|---|---|
|0|1000|500|
|1|4000|300|
|2|7000|600|
|3|9000|200|

### Meaning

**Base Address:** Starting address of the segment.

**Limit:** Size of the segment.

---

# Address Translation in Segmentation

The logical address consists of:

```text
Logical Address

+----------------+
| Segment Number |
+----------------+
| Offset         |
+----------------+
```

The CPU generates a logical address:

```text
(Segment Number, Offset)
```

Example:

```text
(2,100)
```

Meaning:

- Segment Number = 2
    
- Offset = 100
    

---

## Steps of Address Translation

### Step 1

CPU generates logical address:

```text
(2,100)
```

### Step 2

Search Segment 2 in Segment Table.

Suppose:

```text
Base = 7000
Limit = 600
```

### Step 3

Check:

```text
Offset < Limit
```

```text
100 < 600
```

Valid address.

### Step 4

Calculate Physical Address:

```text
Physical Address = Base + Offset

= 7000 + 100

= 7100
```

---

# Address Translation Diagram

```text
Logical Address
(Segment No., Offset)
          |
          v

     Segment Table
          |
          v

Physical Address

Base + Offset
```

---

# Advantages of Segmentation

1. Matches programmer's logical view of memory.
    
2. Supports protection and security.
    
3. Allows sharing of program segments.
    
4. Better program organization.
    
5. Supports modular programming.
    

---

# Disadvantages of Segmentation

1. Causes external fragmentation.
    
2. Memory management becomes complex.
    
3. May require compaction.
    
4. Extra memory needed for segment tables.
    

---

# Difference Between Paging and Segmentation

|Paging|Segmentation|
|---|---|
|Memory divided into pages|Memory divided into segments|
|Fixed-size blocks|Variable-size blocks|
|Physical division|Logical division|
|Uses Page Table|Uses Segment Table|
|Eliminates external fragmentation|Suffers from external fragmentation|
|Address = Page No + Offset|Address = Segment No + Offset|
|Programmer does not see pages|Programmer sees logical segments|

---

# Conclusion

Segmentation is a memory management technique that divides a program into variable-sized logical units called segments. Each segment has a base address and limit stored in the segment table. It provides logical organization, protection, and sharing but suffers from external fragmentation.

---

# 5 Marks Short Answer

**Segmentation is a memory management technique in which a program is divided into variable-sized logical units called segments. Each segment represents a logical part of the program such as code, data, or stack. A Segment Table stores the base address and limit of each segment. Segmentation supports protection and sharing but may cause external fragmentation.**

---

# Last-Minute Revision (30 Seconds Before Exam)

Remember:

```text
Segmentation

↓

Program divided into

Code
Data
Functions
Stack

↓

Variable Size Segments

↓

Segment Table

(Base + Limit)

↓

Logical Address

(Segment No + Offset)

↓

Physical Address

Base + Offset

↓

External Fragmentation
```

### Memory Trick

**PAGING = Equal Pieces**  
(Fixed-size Pages)

**SEGMENTATION = Meaningful Sections**  
(Code, Data, Stack, Functions)

This single trick helps retain the difference even during the exam.