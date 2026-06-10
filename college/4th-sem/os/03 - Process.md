
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
