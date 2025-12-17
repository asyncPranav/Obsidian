

---

# ✅ UNIT–1 : COMPUTER ORGANIZATION & ARCHITECTURE

## ⭐ PRIORITY–1 (VERY HIGH CHANCE QUESTIONS)

---

## 1️⃣ VON–NEUMANN ARCHITECTURE (10 MARKS) ⭐⭐⭐⭐⭐

### **Definition**

Von-Neumann Architecture is a computer architecture in which **data and program instructions are stored in the same memory** and are accessed using a **single bus system**.

---

### **Block Diagram (Text Based – Draw in Exam)**

```
            ┌──────────────┐
            │   Input Unit │
            └──────┬───────┘
                   │
        ┌──────────▼──────────┐
        │      Main Memory     │
        │ (Data + Instructions)│
        └──────────┬──────────┘
                   │
        ┌──────────▼──────────┐
        │        CPU           │
        │ ┌──────┐  ┌───────┐ │
        │ │ ALU  │  │  CU   │ │
        │ └──────┘  └───────┘ │
        └──────────┬──────────┘
                   │
            ┌──────▼──────┐
            │ Output Unit │
            └─────────────┘

```

---

### **Main Components**

1. **Input Unit** – Takes input from user
    
2. **Memory Unit** – Stores data and instructions
    
3. **CPU (Central Processing Unit)**
    
    - **ALU** – Performs arithmetic and logical operations
        
    - **Control Unit (CU)** – Controls execution of instructions
        
4. **Output Unit** – Displays result
    

---

### **Stored Program Concept**

- Both **instructions and data are stored in the same memory**
    
- Instructions are executed **sequentially**
    
- CPU fetches:
    
    1. Instruction
        
    2. Data
        
    3. Executes operation
        

📌 **This concept is the foundation of modern computers**

---

### **Advantages**

1. Simple and easy to design
    
2. Less hardware required
    
3. Flexible and general-purpose system
    
4. Widely used in real computers
    

---

### **Limitations (Von-Neumann Bottleneck)**

- Single bus for data & instructions
    
- CPU waits while data/instruction transfer
    
- Reduces system speed
    

📌 This limitation is called **Von-Neumann Bottleneck**

---

### **Conclusion**

Von-Neumann Architecture introduced the **stored program concept** and forms the base of modern computer systems.

---

## ✍️ WRITE THIS EXACTLY FOR FULL 10 MARKS

---

---

## 2️⃣ TYPES OF BUSES (10 MARKS) ⭐⭐⭐⭐⭐

### **Definition**

A **bus** is a group of wires that carries **data, address, and control signals** between different components of a computer.

---

### **Types of Buses**

---

### **1. Data Bus**

- Transfers **actual data**
    
- **Bidirectional**
    
- Width determines speed (8-bit, 16-bit, 32-bit)
    

📌 Example: Sending data from memory to CPU

---

### **2. Address Bus**

- Carries **address of memory location**
    
- **Unidirectional**
    
- Determines memory size
    

📌 Example: CPU sends address to memory

---

### **3. Control Bus**

- Carries **control signals**
    
- Controls read/write operations
    
- Signals like: Read, Write, Interrupt
    

📌 Example: Memory Read signal

---

### **Text Diagram**

```
CPU ── Address Bus ──► Memory
CPU ◄─ Data Bus ──► Memory
CPU ── Control Bus ──► Memory
```

---

### **Comparison (Data vs Address Bus)**

|Feature|Data Bus|Address Bus|
|---|---|---|
|Direction|Bidirectional|Unidirectional|
|Carries|Data|Address|
|Speed Impact|Yes|No|

---

### **Conclusion**

Buses enable communication between CPU, memory, and I/O devices.

---

---

## 3️⃣ BUS ARCHITECTURE

### ➤ Using MULTIPLEXER

### ➤ Using TRI-STATE BUFFER (10 MARKS) ⭐⭐⭐⭐⭐

---

### **Why Bus Architecture is Needed**

- Reduces number of wires
    
- Efficient data transfer
    
- Cost effective design
    

---

## **A) Bus Architecture Using Multiplexer**

### **Working**

- Multiplexer selects **one source at a time**
    
- Selection lines decide active input
    
- Output connected to common bus
    

### **Diagram**

`R1 ──┐ R2 ──┼──► MUX ───► BUS R3 ──┘      Select Lines`

### **Disadvantage**

- Hardware increases with registers
    
- Slower for large systems
    

---

## **B) Bus Architecture Using Tri-State Buffer**

### **Working**

- Uses tri-state buffers (0,1,Z)
    
- Only one buffer enabled at a time
    
- Others remain in high-impedance state
    

### **Diagram**

`R1 ──► [T] ──┐ R2 ──► [T] ──┼──► BUS R3 ──► [T] ──┘`

---

### **Comparison**

|Feature|Multiplexer|Tri-State|
|---|---|---|
|Speed|Slower|Faster|
|Hardware|More|Less|
|Flexibility|Low|High|

---

### **Conclusion**

Tri-state buffer bus is more efficient and widely used.

---

---

## 4️⃣ REGISTER TRANSFER LANGUAGE (RTL) (10 MARKS) ⭐⭐⭐⭐⭐

### **Definition**

Register Transfer Language is a **symbolic notation** used to describe **data transfer and operations between registers**.

---

### **Register Transfer Statement**

`R1 ← R2`

Means: Content of R2 is copied to R1

---

### **Control Function**

Operation executes only if control condition is true.

`P : R1 ← R2`

---

### **Examples**

1. `R1 ← R2 + R3`
    
2. `R4 ← R4 + 1`
    
3. `P : R1 ← R2`
    

---

### **RTL Symbols**

- `←` Transfer
    
- `:` Control condition
    
- `+` Arithmetic operation
    

---

### **Advantages**

1. Simple representation
    
2. Easy to understand
    
3. Used in micro-operation design
    

---

### **Conclusion**

RTL provides a clear method to describe internal operations of CPU.
