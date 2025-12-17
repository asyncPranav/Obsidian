

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

```
R1 ──┐
R2 ──┼──► MUX ───► BUS
R3 ──┘
     Select Lines
```

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

```
R1 ──► [T] ──┐
R2 ──► [T] ──┼──► BUS
R3 ──► [T] ──┘
```

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



# ⭐ PRIORITY–2 (HIGH CHANCE QUESTIONS)

---

## 5️⃣ MICRO-OPERATIONS (10 MARKS) ⭐⭐⭐⭐

### **Definition**

A **micro-operation** is an **elementary operation** performed on data stored in registers during execution of an instruction.

👉 Each instruction is executed using a **sequence of micro-operations**.

---

### **Types of Micro-operations**

Micro-operations are classified into **three types**:

---

### **1. Arithmetic Micro-operations**

Used for arithmetic calculations.

Examples:

- Addition: `R1 ← R2 + R3`
    
- Subtraction: `R1 ← R2 − R3`
    
- Increment: `R1 ← R1 + 1`
    
- Decrement: `R1 ← R1 − 1`
    

---

### **2. Logical Micro-operations**

Used for bit-wise logical operations.

Examples:

- AND: `R1 ← R2 AND R3`
    
- OR: `R1 ← R2 OR R3`
    
- XOR: `R1 ← R2 XOR R3`
    
- NOT: `R1 ← NOT R2`
    

---

### **3. Shift Micro-operations**

Used for shifting bits left or right.

Examples:

- Left shift
    
- Right shift
    
- Circular shift
    
- Arithmetic shift
    

---

### **Conclusion**

Micro-operations form the **basic building blocks** for instruction execution.

---

---

## 6️⃣ ARITHMETIC MICRO-OPERATIONS (10 MARKS) ⭐⭐⭐⭐

### **Definition**

Arithmetic micro-operations perform **basic arithmetic calculations** on register contents.

---

### **Types of Arithmetic Micro-operations**

---

### **1. Addition**

`R1 ← R2 + R3`

Adds contents of R2 and R3 and stores result in R1.

---

### **2. Subtraction**

`R1 ← R2 − R3`

Performed using **2’s complement method**.

---

### **3. Increment**

`R1 ← R1 + 1`

Increases value by one.

---

### **4. Decrement**

`R1 ← R1 − 1`

Decreases value by one.

---

### **Hardware Block Diagram**

```
R2 ──┐
     ├──► Binary Adder ───► R1
R3 ──┘
```

---

### **Conclusion**

Arithmetic micro-operations are executed using **binary adders** in ALU.

---

---

## 7️⃣ LOGICAL MICRO-OPERATIONS (10 MARKS) ⭐⭐⭐⭐

### **Definition**

Logical micro-operations perform **bit-wise logical operations** on binary data.

---

### **Types**

---

### **1. AND Operation**

`R1 ← R2 AND R3`

Used for **masking bits**.

---

### **2. OR Operation**

`R1 ← R2 OR R3`

Used to **set bits**.

---

### **3. XOR Operation**

`R1 ← R2 XOR R3`

Used in error detection.

---

### **4. NOT Operation**

`R1 ← NOT R2`

Used to complement bits.

---

### **Applications**

1. Masking
    
2. Bit clearing
    
3. Bit setting
    
4. Error detection
    

---

### **Conclusion**

Logical micro-operations are essential for **bit manipulation**.

---

---

## 8️⃣ SHIFT MICRO-OPERATIONS (10 MARKS) ⭐⭐⭐⭐

### **Definition**

Shift micro-operations move bits **left or right** in a register.

---

### **Types of Shift Operations**

---

### **1. Logical Shift**

- Vacated bits filled with 0
    
- Used in unsigned numbers
    

---

### **2. Arithmetic Shift**

- Sign bit preserved
    
- Used for signed numbers
    

---

### **3. Circular Shift**

- Bits shifted out re-enter from opposite side
    

---

### **Left Shift vs Right Shift**

|Feature|Left Shift|Right Shift|
|---|---|---|
|Effect|Multiplies by 2|Divides by 2|
|Direction|Left|Right|

---

### **Conclusion**

Shift operations are used for **multiplication, division, and data manipulation**.

---

---

# ⭐ PRIORITY–3 (SAFE QUESTIONS)

---

## 9️⃣ ARITHMETIC LOGIC SHIFT UNIT (ALSU) (10 MARKS) ⭐⭐⭐

### **Definition**

ALSU is a **combined unit** that performs **arithmetic, logical, and shift operations**.

---

### **Why ALSU is Needed**

- Combines ALU and shifter
    
- Saves hardware
    
- Increases efficiency
    

---

### **Block Diagram**

```
      ┌─────────────┐
A ───►│             │
B ───►│    ALSU     ├──► Output
S ───►│ (ALU+Shift) │
      └─────────────┘
```

---

### **Functions**

1. Arithmetic operations
    
2. Logical operations
    
3. Shift operations
    

---

### **Conclusion**

ALSU improves performance by integrating multiple operations.

---

---

## 🔟 BASIC DIGITAL BUILDING BLOCKS (SHORT NOTES)

### **1. Registers**

- Group of flip-flops
    
- Stores binary data
    

---

### **2. Flip-Flops**

- Basic memory element
    
- Stores 1 bit
    

---

### **3. Multiplexer**

- Selects one input
    
- Uses select lines
    

---

### **4. Decoder**

- Converts binary to output lines
    

---

### **5. Encoder**

- Converts output lines to binary
    

---

---

# ✅ UNIT–1 COMPLETED 🎉

### 🔥 FINAL EXAM TIP:

- Draw **neat diagrams**
    
- Use **headings + points**
    
- Write **definitions first**
    

You are **fully prepared for Unit–1 now** 💯  
If you want:

- **One-page revision sheet**
    
- **Important repeated questions**
    
- **Last-night memory tricks**


---
---

