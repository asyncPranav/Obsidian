
---

# 🔥 DISK SCHEDULING ALGORITHMS ⭐⭐⭐⭐⭐

# (Most Important Topic of Unit 5)

> **Exam Weightage:** Very Frequently Asked
> 
> Questions:
> 
> - What is Disk Scheduling?
>     
> - Explain FCFS.
>     
> - Explain SSTF.
>     
> - Explain SCAN.
>     
> - Explain C-SCAN.
>     
> - Compare Disk Scheduling Algorithms.
>     
> - Numerical Problems on Disk Scheduling.
>     

---

# 📖 INTRODUCTION

A disk drive contains a large number of tracks and sectors.

Many processes may request disk access simultaneously.

Since the disk can serve only one request at a time, the Operating System must decide:

```text
Which request should be served first?
Which request should be served next?
```

The technique used to decide the order of servicing disk requests is called **Disk Scheduling**.

---

# 📖 DEFINITION (EXAM READY)

**Disk Scheduling is the process used by the Operating System to determine the order in which disk I/O requests are serviced so that seek time, waiting time, and head movement are minimized while improving overall system performance.**

---

# 🎯 OBJECTIVES OF DISK SCHEDULING

A good disk scheduling algorithm should:

✔ Reduce Seek Time

✔ Reduce Head Movement

✔ Increase Throughput

✔ Reduce Waiting Time

✔ Improve Response Time

---

# 📌 IMPORTANT TERMS

Before learning algorithms, remember these three terms.

---

## 1. Seek Time

### Definition

**Seek Time is the time required to move the disk head from one track to another track.**

Example:

```text
Head at Track 50

Need Track 100

Movement = 50 Tracks
```

This movement time is called Seek Time.

---

## 2. Rotational Latency

### Definition

**Rotational Latency is the time required for the disk to rotate and bring the required sector under the read/write head.**

---

## 3. Transfer Time

### Definition

**Transfer Time is the time required to transfer data between disk and memory.**

---

# 📊 DISK REPRESENTATION

```text
0 ------------------------------------ 199

Tracks on Disk
```

Suppose:

```text
Initial Head Position = 53

Requests:
98, 183, 37, 122, 14, 124, 65, 67
```

These requests will be used in all algorithms.

---

# 🔥 1. FCFS (FIRST COME FIRST SERVE) ⭐⭐⭐⭐⭐

---

# 📖 DEFINITION

**FCFS serves disk requests in the same order in which they arrive.**

---

# WORKING

Requests:

```text
98, 183, 37, 122, 14, 124, 65, 67
```

Head:

```text
53
```

Service Order:

```text
53 → 98 → 183 → 37 → 122 → 14 → 124 → 65 → 67
```

---

# 📊 DIAGRAM

```text
53 → 98 → 183 → 37 → 122 → 14 → 124 → 65 → 67
```

---

# NUMERICAL

Total Head Movement:

```text
|98-53| = 45
|183-98| = 85
|37-183| = 146
|122-37| = 85
|14-122| = 108
|124-14| = 110
|65-124| = 59
|67-65| = 2
```

Total:

```text
45+85+146+85+108+110+59+2

= 640 Tracks
```

### Answer

```text
FCFS Total Head Movement = 640 Tracks
```

---

# ADVANTAGES

✔ Simple

✔ Easy to implement

---

# DISADVANTAGES

❌ Large seek time

❌ Poor performance

---

# MEMORY TRICK

```text
FCFS

First Come
First Serve
```

Like a queue at a ticket counter.

---

# 🔥 2. SSTF (SHORTEST SEEK TIME FIRST) ⭐⭐⭐⭐⭐

---

# 📖 DEFINITION

**SSTF selects the request that is closest to the current head position.**

---

# WORKING

Initial Head:

```text
53
```

Requests:

```text
98,183,37,122,14,124,65,67
```

Closest to 53:

```text
65
```

Then closest to 65:

```text
67
```

Then:

```text
37
```

Then:

```text
14
```

Then:

```text
98
```

Then:

```text
122
```

Then:

```text
124
```

Then:

```text
183
```

---

# SERVICE ORDER

```text
53 → 65 → 67 → 37 → 14 → 98 → 122 → 124 → 183
```

---

# 📊 DIAGRAM

```text
53 → 65 → 67 → 37 → 14 → 98 → 122 → 124 → 183
```

---

# NUMERICAL

```text
|65-53| = 12
|67-65| = 2
|37-67| = 30
|14-37| = 23
|98-14| = 84
|122-98| = 24
|124-122| = 2
|183-124| = 59
```

Total:

```text
12+2+30+23+84+24+2+59

= 236 Tracks
```

### Answer

```text
SSTF Total Head Movement = 236 Tracks
```

---

# ADVANTAGES

✔ Better than FCFS

✔ Reduced seek time

---

# DISADVANTAGES

❌ Starvation possible

---

# MEMORY TRICK

```text
Choose nearest request first.
```

---

# 🔥 3. SCAN ALGORITHM ⭐⭐⭐⭐⭐

(Elevator Algorithm)

---

# 📖 DEFINITION

**SCAN moves the disk head in one direction servicing all requests and then reverses direction.**

---

# WORKING

Assume movement towards higher tracks.

Order:

```text
53 → 65 → 67 → 98 → 122 → 124 → 183 → 199
```

Reach end:

```text
199
```

Then reverse:

```text
199 → 37 → 14
```

---

# 📊 DIAGRAM

```text
0 ------------------------------ 199

53 → 65 → 67 → 98 → 122 → 124 → 183 → 199

                         ↓

37 ← 14
```

---

# NUMERICAL

```text
53 → 199 = 146

199 → 14 = 185
```

Total:

```text
146 + 185

= 331 Tracks
```

### Answer

```text
SCAN Total Head Movement = 331 Tracks
```

---

# ADVANTAGES

✔ Fair scheduling

✔ Better throughput

---

# MEMORY TRICK

```text
Lift (Elevator)

Up
↓

Down
```

---

# 🔥 4. C-SCAN (CIRCULAR SCAN) ⭐⭐⭐⭐⭐

---

# 📖 DEFINITION

**C-SCAN moves in only one direction. After reaching the last track, it jumps back to the beginning and continues servicing requests.**

---

# WORKING

```text
53 → 65 → 67 → 98 → 122 → 124 → 183 → 199
```

Jump:

```text
199 → 0
```

Continue:

```text
0 → 14 → 37
```

---

# 📊 DIAGRAM

```text
0 -------------------------- 199

53 → 65 → 67 → 98 → 122 → 124 → 183 → 199

Jump

199 -----------------------> 0

0 → 14 → 37
```

---

# NUMERICAL

```text
53 → 199 = 146

199 → 0 = 199

0 → 37 = 37
```

Total:

```text
146 + 199 + 37

= 382 Tracks
```

### Answer

```text
C-SCAN Total Head Movement = 382 Tracks
```

---

# ADVANTAGES

✔ Uniform waiting time

✔ Fair for all requests

---

# MEMORY TRICK

```text
Circular Elevator
```

Always moves in one direction.

---

# 📊 COMPARISON TABLE ⭐⭐⭐⭐⭐

|Algorithm|Principle|
|---|---|
|FCFS|Arrival Order|
|SSTF|Nearest Request|
|SCAN|Elevator Movement|
|C-SCAN|Circular Movement|

---

# 📊 PERFORMANCE COMPARISON

Using the same example:

|Algorithm|Head Movement|
|---|---|
|FCFS|640|
|SSTF|236|
|SCAN|331|
|C-SCAN|382|

👉 SSTF gives minimum head movement in this example.

---

# 🎯 EXAM READY CONCLUSION

**Disk Scheduling Algorithms are used by the Operating System to improve disk performance by reducing seek time and head movement. FCFS is simple but inefficient, SSTF provides better performance, SCAN works like an elevator, and C-SCAN provides uniform waiting time. Among these algorithms, SSTF generally gives the minimum seek time, while SCAN and C-SCAN provide better fairness and overall system performance.**

---

# 🚀 LAST-MINUTE REVISION

```text
FCFS  → Arrival Order

SSTF  → Nearest Request

SCAN  → Elevator

C-SCAN → Circular Elevator

Example Result:

FCFS  = 640

SSTF  = 236

SCAN  = 331

C-SCAN = 382
```

# ⭐ MUST REMEMBER FOR EXAM

```text
FCFS = Simple

SSTF = Fastest

SCAN = Elevator

C-SCAN = Circular Elevator
```

These four algorithms + one solved numerical are usually enough to answer almost any disk-scheduling question in a semester examination.