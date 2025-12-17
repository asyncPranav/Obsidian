

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


# ✅ UNIT–2 : BASIC COMPUTER ORGANIZATION

## 🟢 PRIORITY–1 (EXTREMELY IMPORTANT)

---

## 1️⃣ ADDRESSING MODES (10 MARKS) ⭐⭐⭐⭐⭐

🔥 **MOST IMPORTANT TOPIC OF UNIT–2**

---

### **Definition**

An **addressing mode** is a method used to **specify how the operand (data) of an instruction is accessed**.

---

### **Purpose of Addressing Modes**

Addressing modes are needed to:

1. Access data efficiently
    
2. Reduce instruction size
    
3. Provide flexibility in programming
    
4. Support different data structures
    

---

### **Types of Addressing Modes**

---

### **1. Immediate Addressing Mode**

- Operand is **part of instruction**
    
- No memory access required
    

📌 Example:

```
ADD R1, #5
```

→ Adds value 5 to R1

✔ Fast execution  
❌ Limited data size

---

### **2. Direct Addressing Mode**

- Address field contains **actual memory address**
    
- Operand is fetched from memory
    

📌 Example:

```
ADD R1, 200
```

→ Operand is at memory location 200

✔ Simple  
❌ Limited address space

---

### **3. Indirect Addressing Mode**

- Address field points to another address
    
- Requires **two memory accesses**
    

📌 Example:

```
ADD R1, @200
```

→ Address of operand is stored at location 200

✔ Large address space  
❌ Slower execution

---

### **4. Register Addressing Mode**

- Operand stored in **register**
    
- No memory access
    

📌 Example:

```
ADD R1, R2
```

✔ Very fast  
❌ Limited registers

---

### **5. Register Indirect Addressing Mode**

- Register holds **memory address**
    
- Operand accessed from memory
    

📌 Example:

```
ADD R1, (R2)
```

✔ Flexible  
❌ Extra memory access

---

### **6. Indexed / Relative Addressing Mode (Basic Idea)**

- Effective address = Base address + Index
    
- Used in arrays and loops
    

📌 Example:

```
ADD R1, 100(R2)
```

---

### **Conclusion**

Addressing modes improve **program efficiency and flexibility**.

---

### ✍️ **WRITE THIS FOR FULL 10 MARKS**

---

## 🔸 DIFFERENCE: IMMEDIATE vs DIRECT ADDRESSING (5–6 MARKS SAFE)

|Feature|Immediate|Direct|
|---|---|---|
|Operand|Inside instruction|In memory|
|Memory Access|No|Yes|
|Speed|Faster|Slower|
|Example|ADD R1,#5|ADD R1,200|

---

---

## 2️⃣ INSTRUCTION CYCLE (10 MARKS) ⭐⭐⭐⭐⭐

🔥 **NEAR GUARANTEED QUESTION**

---

### **Definition**

The **instruction cycle** is the complete sequence of steps performed by the CPU to **fetch, decode, and execute an instruction**.

---

### **Phases of Instruction Cycle**

---

### **1. Fetch Cycle**

- Instruction fetched from memory
    
- Stored in Instruction Register (IR)
    

📌 Micro-operation:

```
IR ← Memory[PC]
PC ← PC + 1
```

---

### **2. Decode Cycle**

- Instruction decoded
    
- Opcode analyzed
    
- Addressing mode identified
    

---

### **3. Execute Cycle**

- Actual operation performed
    
- ALU executes instruction
    
- Result stored
    

---

### **Flow Diagram (Text – Draw in Exam)**

```
 ┌────────┐
 │ Fetch  │
 └───┬────┘
     │
 ┌───▼────┐
 │ Decode │
 └───┬────┘
     │
 ┌───▼────┐
 │ Execute│
 └────────┘
```

---

### **Conclusion**

Instruction cycle ensures **systematic execution** of all instructions.

---

---

## 3️⃣ INSTRUCTION CODES (10 MARKS) ⭐⭐⭐⭐⭐

---

### **What is an Instruction?**

An instruction is a **binary command** that tells the computer **what operation to perform**.

---

### **What is Instruction Code?**

Instruction code is the **binary representation of an instruction**, stored in memory.

---

### **Fields of an Instruction Code**

---

### **1. Opcode (Operation Code)**

- Specifies operation to be performed
    
- Example: ADD, SUB, LOAD
    

---

### **2. Operand / Address Field**

- Specifies data or address of data
    
- Can be register or memory address
    

---

### **General Instruction Format**

```
┌────────┬─────────────┐
│ Opcode │ Address     │
└────────┴─────────────┘
```

---

### **Example**

```
ADD R1, R2
```

- Opcode: ADD
    
- Operand: R1, R2
    

---

### **Conclusion**

Instruction codes define **how CPU understands and executes commands**.



# ✅ UNIT–2 : PRIORITY–2 (VERY IMPORTANT)

---

## 4️⃣ GENERAL COMPUTER REGISTERS

### WITH COMMON BUS SYSTEM (10 MARKS) ⭐⭐⭐⭐

📌 **Classic & scoring COA question**

---

### **What are Registers?**

Registers are **high-speed storage locations** inside the CPU used to **store data, instructions, and addresses temporarily**.

---

### **Types of Registers (Brief)**

1. **General Purpose Registers (R1, R2, …)** – Store data
    
2. **Accumulator (AC)** – Stores intermediate results
    
3. **Program Counter (PC)** – Holds address of next instruction
    
4. **Instruction Register (IR)** – Holds current instruction
    
5. **Memory Address Register (MAR)** – Holds memory address
    
6. **Memory Data Register (MDR)** – Holds data from memory
    

---

### **Common Bus System – Concept**

A **common bus system** is a single set of lines used to **transfer data between registers**.

📌 Purpose:

- Reduces number of wires
    
- Simplifies hardware
    
- Efficient data transfer
    

---

### **Block Diagram (Text Based – Draw in Exam)**

```
R1 ──┐
R2 ──┼──► MUX / Tri-State ───► BUS ───► ALU
R3 ──┘

```

---

### **Role of Multiplexer**

- Selects **one register at a time**
    
- Output connected to common bus
    
- Controlled by select lines
    

---

### **Role of Tri-State Buffer**

- Allows multiple registers
    
- Only one enabled at a time
    
- Others remain in high-impedance state
    

---

### **Conclusion**

General register organization with common bus system **reduces hardware and increases efficiency**.

---

## ✍️ WRITE THIS FOR FULL 10 MARKS

---

---

## 5️⃣ INPUT–OUTPUT CONFIGURATION (10 MARKS) ⭐⭐⭐⭐

📌 **Very common theory question**

---

### **What is Input–Output Configuration?**

Input–Output configuration refers to the **method used to connect I/O devices with CPU and memory**.

---

### **Need for I/O Interface**

- CPU and I/O devices operate at different speeds
    
- Data formats are different
    
- I/O interface acts as a **bridge**
    

---

### **Role of I/O Interface**

1. Controls data transfer
    
2. Provides status information
    
3. Handles control signals
    
4. Synchronizes CPU and I/O devices
    

---

### **CPU–Memory–I/O Interaction**

        `┌────────┐         │  CPU   │         └───┬────┘             │      ┌──────▼──────┐      │ I/O Interface│      └───┬──────┬──┘          │      │      Input     Output      Device    Device`

---

### **Types of I/O Operations**

- Programmed I/O
    
- Interrupt driven I/O
    
- DMA (Direct Memory Access)
    

---

### **Conclusion**

I/O configuration ensures **smooth communication** between CPU and external devices.

---

---

## 6️⃣ INTERRUPT CYCLE (10 MARKS) ⭐⭐⭐⭐

📌 **Often asked with instruction cycle**

---

### **What is an Interrupt?**

An interrupt is a **signal that temporarily stops the CPU** to attend an **urgent task**.

---

### **Why Interrupt is Needed**

1. Efficient CPU utilization
    
2. Faster response to I/O devices
    
3. Avoids continuous polling
    

---

### **Interrupt Cycle – Steps**

1. CPU completes current instruction
    
2. Saves current status (PC, registers)
    
3. Transfers control to Interrupt Service Routine (ISR)
    
4. Executes ISR
    
5. Restores previous state
    
6. Returns to normal execution
    

---

### **Flow (Text Diagram)**

`Instruction Execution         │    Interrupt Signal         │  Save CPU State         │  Execute ISR         │  Restore State         │  Continue Program`

---

### **Difference: Instruction Cycle vs Interrupt Cycle**

|Feature|Instruction Cycle|Interrupt Cycle|
|---|---|---|
|Purpose|Execute instruction|Handle interrupt|
|Occurrence|Always|When interrupt occurs|
|Control|Program based|Hardware based|

---

### **Conclusion**

Interrupt cycle improves **system efficiency and responsiveness**.