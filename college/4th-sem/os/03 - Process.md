
---

# PROCESS STATES (Process Life Cycle) ⭐⭐⭐⭐⭐

**Exam Probability: Very High**

Questions Asked:

- Explain Process States with diagram.
    
- Process Life Cycle.
    
- State Transition Diagram.
    

---

# What is a Process?

## Definition

**A process is a program in execution.**

When a program is loaded into memory and starts running, it becomes a process.

### Example

```text
Chrome Running      → Process
VS Code Running     → Process
Music Player Running→ Process
```

---

# What are Process States?

During its execution, a process moves through different states.

These states describe the current condition of the process.

---

# Main Process States

There are 5 basic process states:

```text
1. New
2. Ready
3. Running
4. Waiting (Blocked)
5. Terminated
```

---

# Process State Transition Diagram

⭐ Draw this diagram in exam

```text
                Admitted
                   |
                   v
              +---------+
              |  NEW    |
              +---------+
                   |
                   v
              +---------+
              | READY   |
              +---------+
                   |
        Scheduler  |
        Dispatch   v
              +---------+
              | RUNNING |
              +---------+
              /    |    \
             /     |     \
            /      |      \
           v       v       v
     +---------+   |   +---------+
     | WAITING |   |   |TERMINATE|
     +---------+   |   +---------+
           |       |
           | I/O   |
           | Done  |
           v       |
        +---------+
        | READY   |
        +---------+
```

---

# Explanation of Each State

## 1. New State

A process is in the **New State** when it is being created.

### Example

You double-click Chrome.

Chrome process is created.

```text
Program Created
      ↓
NEW State
```

---

## 2. Ready State

The process is loaded into memory and is ready to execute.

It is waiting for CPU allocation.

### Important Line

**Ready means waiting for CPU.**

### Example

```text
Process Ready
CPU Busy
↓
Waiting in Ready Queue
```

---

## 3. Running State

The CPU is currently executing the process.

### Example

```text
Chrome is using CPU

↓

RUNNING State
```

Only one process can be in Running state on a single CPU at a particular instant.

---

## 4. Waiting (Blocked) State

The process is waiting for an event or I/O operation.

### Example

```text
Process requests file

↓

Waiting for Disk

↓

WAITING State
```

### Important Line

**Waiting means waiting for an event or I/O.**

---

## 5. Terminated State

The process has completed its execution.

### Example

```text
Calculator Closed

↓

TERMINATED State
```

The operating system removes the process from memory.

---

# State Transitions

These transitions are frequently asked.

### New → Ready

Process is admitted into memory.

---

### Ready → Running

CPU is assigned by scheduler.

---

### Running → Waiting

Process requests I/O.

Example:

```text
Reading File
Printing Document
Keyboard Input
```

---

### Waiting → Ready

I/O operation completes.

---

### Running → Ready

CPU time expires.

Occurs in time-sharing systems.

---

### Running → Terminated

Process finishes execution.

---

# Easy Memory Trick

Remember:

## NRRWT

```text
N → New
R → Ready
R → Running
W → Waiting
T → Terminated
```

Or simply:

```text
Create
Wait CPU
Use CPU
Wait I/O
Finish
```

This sequence helps recall the entire process life cycle.

---

# Advantages of Process States

1. Better CPU utilization.
    
2. Efficient process management.
    
3. Supports multitasking.
    
4. Improves system performance.
    
5. Helps scheduling decisions.
    

---

# 5 Marks Answer

## Explain Process States

A process is a program in execution. During execution, a process passes through different states called process states. The main states are New, Ready, Running, Waiting, and Terminated. A process is created in the New state, waits for CPU in the Ready state, executes in the Running state, waits for I/O in the Waiting state, and moves to the Terminated state after completion.

---

# 10–15 Marks Answer Structure

When asked:

### "Explain Process States with diagram"

Write:

1. Definition of Process
    
2. Definition of Process States
    
3. State Transition Diagram
    
4. Explanation of New State
    
5. Explanation of Ready State
    
6. Explanation of Running State
    
7. Explanation of Waiting State
    
8. Explanation of Terminated State
    
9. State Transitions
    
10. Conclusion
    

---

# One-Minute Revision Before Exam

```text
Process = Program in Execution

States:

NEW
 ↓
READY
 ↓
RUNNING
 ↓
WAITING
 ↓
READY
 ↓
RUNNING
 ↓
TERMINATED

Ready = Waiting for CPU

Waiting = Waiting for I/O

Terminated = Finished
```

---

# INTERPROCESS COMMUNICATION (IPC) ⭐⭐⭐⭐⭐

**Exam Probability: Very High**

Frequently Asked Questions:

- What is IPC?
    
- Explain Interprocess Communication.
    
- Methods of IPC.
    
- Shared Memory vs Message Passing.
    
- Why IPC is needed?
    

---

# What is IPC?

## Definition

**Interprocess Communication (IPC) is a mechanism that allows processes to communicate and exchange data with each other.**

---

# Why IPC is Needed?

In a multiprogramming system, many processes run simultaneously.

Sometimes processes need to:

- Exchange information
    
- Share data
    
- Coordinate their activities
    
- Access common resources
    

For this purpose, IPC is required.

---

## Example

Suppose:

```text
Process A → Downloads a file

Process B → Opens the file
```

Process B needs information from Process A.

This communication happens through IPC.

---

# Need of IPC

IPC is used for:

### 1. Information Sharing

Processes exchange data.

### 2. Resource Sharing

Processes share common resources.

### 3. Computation Speedup

Tasks are divided among processes.

### 4. Modularity

Large programs can be divided into smaller processes.

### 5. Convenience

Multiple processes work together efficiently.

---

# Methods of IPC

There are two main methods of IPC.

```text
IPC

↓

1. Shared Memory

2. Message Passing
```

---

# IPC Diagram

```text
            IPC

             |
    -------------------
    |                 |
    v                 v

Shared Memory    Message Passing
```

Draw this simple diagram in exam.

---

# 1. Shared Memory

## Definition

**Shared Memory is an IPC method in which two or more processes communicate by sharing a common memory area.**

---

# Working of Shared Memory

The operating system creates a memory region.

Multiple processes can access that region.

```text
Process A
      |
      |
      v

+----------------+
| Shared Memory  |
+----------------+

      ^
      |
      |

Process B
```

---

## Example

Suppose:

```text
Process A
```

stores:

```text
Student Marks
```

in shared memory.

Process B directly reads those marks.

No message transfer is required.

---

# Advantages of Shared Memory

1. Very fast communication.
    
2. Suitable for large amounts of data.
    
3. Less communication overhead.
    
4. Efficient resource utilization.
    

---

# Disadvantages of Shared Memory

1. Synchronization is required.
    
2. Risk of data inconsistency.
    
3. More complex implementation.
    
4. Security issues may occur.
    

---

# 2. Message Passing

## Definition

**Message Passing is an IPC method in which processes communicate by sending and receiving messages.**

---

# Working of Message Passing

Instead of sharing memory, processes exchange messages.

```text
Process A

     |
     | Message
     v

Process B
```

---

## Example

```text
Process A
```

sends:

```text
"File Download Complete"
```

to

```text
Process B
```

using a message.

---

# Message Passing Diagram

```text
Process A
    |
 SEND
    |
    v
Message
    |
 RECEIVE
    |
    v
Process B
```

---

# Advantages of Message Passing

1. Simple to implement.
    
2. No shared memory needed.
    
3. Better protection and security.
    
4. Easy synchronization.
    

---

# Disadvantages of Message Passing

1. Slower than shared memory.
    
2. Communication overhead.
    
3. Not efficient for large data transfer.
    

---

# Shared Memory vs Message Passing

⭐⭐⭐⭐⭐ Most Important Comparison

|Shared Memory|Message Passing|
|---|---|
|Processes share common memory|Processes exchange messages|
|Faster|Slower|
|Suitable for large data|Suitable for small data|
|Synchronization required|Synchronization simpler|
|Complex implementation|Easy implementation|
|Less overhead|More communication overhead|

---

# Advantages of IPC

1. Enables communication between processes.
    
2. Supports data sharing.
    
3. Improves resource utilization.
    
4. Helps synchronization.
    
5. Increases system efficiency.
    

---

# 5 Marks Answer

## What is IPC?

**Interprocess Communication (IPC) is a mechanism that allows processes to communicate and exchange data with each other. IPC is used for information sharing, resource sharing, and synchronization. The two main methods of IPC are Shared Memory and Message Passing.**

---

# 10–15 Marks Answer Structure

When asked:

### "Explain IPC and its methods"

Write:

1. Definition of IPC
    
2. Need of IPC
    
3. IPC Diagram
    
4. Shared Memory
    
    - Definition
        
    - Diagram
        
    - Advantages
        
    - Disadvantages
        
5. Message Passing
    
    - Definition
        
    - Diagram
        
    - Advantages
        
    - Disadvantages
        
6. Shared Memory vs Message Passing
    
7. Conclusion
    

---

# Exam Conclusion

**Interprocess Communication (IPC) is an important operating system mechanism that enables processes to exchange information and coordinate their activities. The two major IPC methods are Shared Memory and Message Passing. IPC improves communication, synchronization, and efficient resource utilization in a multiprogramming environment.**

---

# One-Minute Revision Before Exam

```text
IPC

↓

Communication Between Processes

↓

Methods

1. Shared Memory
2. Message Passing

↓

Shared Memory
Fast
Large Data

↓

Message Passing
Simple
Secure

↓

Purpose

Data Sharing
Resource Sharing
Synchronization
```

### Memory Trick

Remember:

**IPC = "Processes Talking"**

And the two ways they talk:

```text
Talk through Common Room
= Shared Memory

Talk through Letters
= Message Passing
```

This single analogy is often enough to recall the entire IPC chapter during the exam.


---


# SCHEDULING CRITERIA ⭐⭐⭐⭐⭐

**Exam Probability: Very High**

Frequently Asked Questions:

- What is Scheduling Criteria?
    
- Explain Scheduling Criteria.
    
- What are the criteria used to evaluate CPU Scheduling?
    
- Write short notes on CPU Utilization, Throughput, Turnaround Time, Waiting Time, and Response Time.
    

---

# Introduction

CPU Scheduling is used to decide which process gets the CPU next.

Different scheduling algorithms are evaluated using certain performance measures called **Scheduling Criteria**.

---

# Definition

**Scheduling Criteria are the standards used to evaluate the performance and efficiency of CPU scheduling algorithms.**

A good scheduling algorithm should maximize CPU usage and minimize process waiting time.

---

# Main Scheduling Criteria

There are five important scheduling criteria:

```text
1. CPU Utilization
2. Throughput
3. Turnaround Time
4. Waiting Time
5. Response Time
```

---

# Diagram

Draw this simple diagram in exam.

```text
        Scheduling Criteria

                 |
------------------------------------------------
|          |           |          |            |
CPU     Throughput  Turnaround  Waiting   Response
Utilization            Time      Time      Time
```

---

# 1. CPU Utilization

## Definition

**CPU Utilization is the percentage of time during which the CPU remains busy executing processes.**

---

## Formula

CPU\ Utilization=\frac{CPU\ Busy\ Time}{Total\ Time}\times100

---

## Goal

CPU Utilization should be **maximum**.

---

## Example

Suppose:

```text
CPU Busy = 90 seconds

Total Time = 100 seconds
```

Then:

```text
CPU Utilization = 90%
```

---

## Exam Point

**Higher CPU Utilization means better system performance.**

---

# 2. Throughput

## Definition

**Throughput is the number of processes completed per unit time.**

---

## Formula

Throughput=\frac{Number\ of\ Completed\ Processes}{Unit\ Time}

---

## Example

If:

```text
20 Processes
```

are completed in:

```text
10 Seconds
```

Then:

```text
Throughput = 2 Processes/Second
```

---

## Goal

Throughput should be **maximum**.

---

## Exam Point

**More completed processes = Higher Throughput.**

---

# 3. Turnaround Time (TAT)

⭐⭐⭐⭐⭐ Most Important

---

## Definition

**Turnaround Time is the total time taken by a process from its arrival to its completion.**

---

## Formula

Turnaround\ Time=Completion\ Time-Arrival\ Time

---

## Example

Suppose:

```text
Arrival Time = 2 ms

Completion Time = 12 ms
```

Then:

```text
TAT = 12 - 2

= 10 ms
```

---

## Goal

Turnaround Time should be **minimum**.

---

## Easy Memory Trick

```text
Entered System
       ↓
Completed

Total Time

= Turnaround Time
```

---

# 4. Waiting Time (WT)

⭐⭐⭐⭐⭐ Most Asked Numerical Concept

---

## Definition

**Waiting Time is the total time a process spends waiting in the ready queue before getting CPU.**

---

## Formula

Waiting\ Time=Turnaround\ Time-Burst\ Time

---

## Example

Suppose:

```text
TAT = 20 ms

Burst Time = 8 ms
```

Then:

```text
WT = 20 - 8

= 12 ms
```

---

## Goal

Waiting Time should be **minimum**.

---

## Easy Memory Trick

```text
Process Waiting

for CPU

↓

Waiting Time
```

---

# 5. Response Time (RT)

⭐⭐⭐⭐⭐ Very Important

---

## Definition

**Response Time is the time between the submission of a process and its first response from the CPU.**

---

## Formula

Response\ Time=First\ CPU\ Allocation\ Time-Arrival\ Time

---

## Example

Suppose:

```text
Arrival Time = 0 ms

First CPU Allocation = 4 ms
```

Then:

```text
RT = 4 ms
```

---

## Goal

Response Time should be **minimum**.

---

## Example

When you click a browser icon:

```text
Click Chrome

↓

Chrome Starts Opening

↓

Response Time
```

---

# Summary Table

⭐⭐⭐⭐⭐ Most Important Revision Table

|Criteria|Meaning|Goal|
|---|---|---|
|CPU Utilization|CPU busy time|Maximum|
|Throughput|Processes completed per unit time|Maximum|
|Turnaround Time|Arrival to Completion|Minimum|
|Waiting Time|Time spent in Ready Queue|Minimum|
|Response Time|Arrival to First Response|Minimum|

---

# 5 Marks Answer

## Explain Scheduling Criteria

**Scheduling Criteria are the measures used to evaluate CPU scheduling algorithms. The main scheduling criteria are CPU Utilization, Throughput, Turnaround Time, Waiting Time, and Response Time. CPU Utilization and Throughput should be maximized, whereas Turnaround Time, Waiting Time, and Response Time should be minimized for better system performance.**

---

# Exam Conclusion

**Scheduling Criteria help in measuring the efficiency of CPU scheduling algorithms. A good scheduling algorithm maximizes CPU Utilization and Throughput while minimizing Turnaround Time, Waiting Time, and Response Time.**

---

# One-Minute Revision Before Exam

```text
Scheduling Criteria

1. CPU Utilization → Maximum

2. Throughput → Maximum

3. Turnaround Time → Minimum

4. Waiting Time → Minimum

5. Response Time → Minimum
```

# Super Memory Trick

Remember:

### "UTTWR"

```text
U → Utilization

T → Throughput

T → Turnaround Time

W → Waiting Time

R → Response Time
```

And remember:

```text
First Two → MAXIMUM

Last Three → MINIMUM
```

This single trick is enough to reproduce the entire answer in the exam.