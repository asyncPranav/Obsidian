``
---


# UNIT – 2: MEDIUM ACCESS SUBLAYER & DATA LINK LAYER

## Complete Exam-Oriented Notes

### Syllabus Covered

1. Medium Access (MAC) Sublayer
2. Channel Allocation
3. LAN Protocols – ALOHA
4. Overview of IEEE Standards
5. FDDI
6. Data Link Layer – Elementary Data Link Protocols
7. Sliding Window Protocols
8. Error Handling

---

# 1. MEDIUM ACCESS SUBLAYER

## 1.1 What is the Medium Access Control (MAC) Sublayer?

The **Medium Access Control (MAC) sublayer** is the lower part of the **Data Link Layer**.

The Data Link Layer is commonly divided into:

```
           DATA LINK LAYER
                  │
          ┌───────┴───────┐
          │               │
          ↓               ↓
       LLC Sublayer     MAC Sublayer
                          │
                          ↓
                    Physical Layer
```

### MAC sublayer is responsible for:

- Deciding **which station can use a shared communication channel**.
- Controlling access to a common medium.
- Handling MAC addressing in LAN technologies.
- Coordinating transmission when multiple devices share the same medium.

### Example

Suppose four computers share one channel:

```
 A ─┐
 B ─┤
 C ─┼──── Shared Channel
 D ─┘
```

If A, B and C transmit simultaneously, their signals may interfere.

The MAC sublayer determines **who can transmit and when**.

### Exam definition

> **The MAC sublayer is the lower sublayer of the Data Link Layer responsible for controlling access to a shared transmission medium and coordinating data transmission among competing stations.**

---

# 2. CHANNEL ALLOCATION

## 2.1 Meaning 

**Channel allocation** is the process of determining **how a communication channel is shared among multiple stations**.

Consider:

```
A ─┐
B ─┤
C ─┼──── Common Channel
D ─┘
```

The problem is:

> Who should use the channel, when should they use it, and how should simultaneous transmissions be handled?

Channel allocation methods are broadly classified as:

```
                 CHANNEL ALLOCATION
                        │
             ┌──────────┴──────────┐
             ↓                     ↓
       Static Allocation      Dynamic Allocation
             │                     │
     ┌───────┴───────┐       ┌─────┴─────────┐
     ↓               ↓       ↓               ↓
    FDMA            TDMA   Random Access   Controlled Access
```

---

# 3. STATIC CHANNEL ALLOCATION

In **static allocation**, the channel is divided among stations **in advance**.

The allocation does not change dynamically according to current traffic.

The two classical methods are:

1. **FDMA**
2. **TDMA**

---

## 3.1 FDMA — Frequency Division Multiple Access

In **FDMA**, the available bandwidth is divided into different **frequency bands**, and each station gets a separate frequency band.

```
Frequency
   ↑
   │ ┌──────────────┐
 f4│ │ Station D    │
   │ ├──────────────┤
 f3│ │ Station C    │
   │ ├──────────────┤
 f2│ │ Station B    │
   │ ├──────────────┤
 f1│ │ Station A    │
   │ └──────────────┘
   └────────────────────→ Time
```

Each station can transmit continuously using its allocated frequency.

### Advantages

- Simple concept.
- Stations can transmit simultaneously.
- No collision between stations using separate frequency bands.

### Disadvantages

- Bandwidth is wasted when a station has no data.
- Fixed allocation is inefficient for bursty computer traffic.
- Guard bands may be required between channels.

---

# 4. TDMA — TIME DIVISION MULTIPLE ACCESS

In **TDMA**, all stations use the same frequency/channel, but each gets a **different time slot**.

```
Time →
┌──────┬──────┬──────┬──────┬──────┐
│  A   │  B   │  C   │  D   │  A   │
└──────┴──────┴──────┴──────┴──────┘
```

Station A transmits during its slot, B during its slot, and so on.

### Advantages

- No collision during assigned slots.
- Simple scheduling.
- All stations can use the same frequency.

### Disadvantages

- Time slots can be wasted when a station has nothing to send.
- Requires synchronization.
- Fixed allocation is not ideal for uneven traffic.

---

# 5. DYNAMIC CHANNEL ALLOCATION

In **dynamic allocation**, channel access depends on the **current traffic and requests from stations**.

Unlike static allocation, resources are not permanently assigned.

It can be classified into:

### A. Random Access

Stations compete for the channel.

Examples:

- ALOHA
- Slotted ALOHA
- CSMA
- CSMA/CD
- CSMA/CA

### B. Controlled Access

Stations coordinate their access.

Examples:

- Polling
- Token passing

For this syllabus, **ALOHA** is the key random-access protocol.

---

# 6. ALOHA PROTOCOL

## 6.1 Introduction

**ALOHA** is a random-access MAC protocol in which a station **transmits whenever it has data to send**, without first checking whether the channel is free.

It was developed for packet radio networks and became an important basis for later random-access protocols.

### Basic idea

```
Station A ─┐
Station B ─┤
Station C ─┼──── Shared Channel
Station D ─┘

Transmit whenever data is ready
          ↓
      Collision?
       /      \
     Yes       No
      ↓         ↓
 Retransmit    Success
    later
```

The two classical forms are:

1. **Pure ALOHA**
2. **Slotted ALOHA**

---

# 7. PURE ALOHA

## 7.1 Working

In Pure ALOHA:

> A station transmits **immediately whenever it has a frame**.

There is no requirement that transmission begin at a particular time.

### Example

```
Time →
A:        [------Frame------]

B:              [------Frame------]
                     ↑
                  Collision
```

If two frames overlap in time, they collide.

After collision, the stations wait for a **random backoff period** and retransmit.

---

## 7.2 Vulnerable Period

For Pure ALOHA, a frame is vulnerable to collision for a period of:

> **2T**

where **T = frame transmission time**.

```
             Vulnerable Period
        <--------- 2T --------->
             |             |
             ↓             ↓
          -T |      0      | +T
             |             |
             └── Frame ────┘
```

A collision can occur if another frame starts up to **T before or T after** the beginning of the reference frame.

---

## 7.3 Throughput of Pure ALOHA

The throughput is:

\[ S = Ge^{-2G} \]

where:

- \(S\) = throughput
- \(G\) = offered load

Maximum throughput occurs at:

\[ G = 0.5 \]

and:

\[ S_{max} = \frac{1}{2e} \]

Therefore:

\[ S_{max} \approx 0.184 \]

### Maximum efficiency

> **Pure ALOHA maximum efficiency ≈ 18.4%**

This is a very important numerical fact.

---

# 8. SLOTTED ALOHA

## 8.1 Working

Slotted ALOHA improves Pure ALOHA by dividing time into **equal slots**, where one slot is equal to the frame transmission time.

A station can begin transmission **only at the beginning of a time slot**.

```
Time →
┌────┬────┬────┬────┬────┐
│ S1 │ S2 │ S3 │ S4 │ S5 │
└────┴────┴────┴────┴────┘
```

If two stations transmit in the same slot:

```
Slot 3
┌───────────────────┐
│ A + B → Collision │
└───────────────────┘
```

---

## 8.2 Vulnerable Period

For Slotted ALOHA:

> **Vulnerable period = T**

because transmissions can only start at slot boundaries.

---

## 8.3 Throughput

The throughput is:

\[ S = Ge^{-G} \]

Maximum occurs at:

\[ G=1 \]

Thus:

\[ S_{max}=\frac{1}{e} \]\[ S_{max}\approx0.368 \]

### Maximum efficiency

> **Slotted ALOHA maximum efficiency ≈ 36.8%**

---

# 9. PURE ALOHA vs SLOTTED ALOHA

|Feature|Pure ALOHA|Slotted ALOHA|
|---|---|---|
|Transmission|Anytime|Only at slot boundaries|
|Synchronization|Not required|Required|
|Vulnerable period|2T|T|
|Maximum throughput|18.4%|36.8%|
|Collision probability|Higher|Lower|
|Efficiency|Lower|Higher|

### Memory trick

> **Pure ALOHA → 18.4%**

> **Slotted ALOHA → 36.8%**

> Slotted ALOHA is approximately **twice as efficient** as Pure ALOHA.

---

# 10. IEEE STANDARDS OVERVIEW

The **IEEE 802 family** contains standards for LANs and MANs.

```
                  IEEE 802
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
    Wired LAN     Wireless LAN   Other LAN/MAN
        │              │
      802.3          802.11
     Ethernet         Wi-Fi
```

## Important IEEE Standards

|Standard|Technology|Main Use|
|---|---|---|
|**802.1**|LAN/MAN bridging and management|Bridging, VLANs, network management|
|**802.2**|LLC|Logical Link Control|
|**802.3**|Ethernet|Wired LAN|
|**802.4**|Token Bus|Historical LAN standard|
|**802.5**|Token Ring|Historical LAN standard|
|**802.11**|WLAN|Wi-Fi|
|**802.15**|WPAN|Bluetooth and similar personal-area technologies|
|**802.16**|Broadband Wireless Access|Metropolitan/wireless broadband|
|**802.17**|Resilient Packet Ring|Ring-based metropolitan networking|

### Important distinction

> **FDDI is not an IEEE 802 standard.**

FDDI stands for **Fiber Distributed Data Interface** and was standardized primarily through **ANSI** standards work. It is commonly taught alongside IEEE LAN standards because it is another important LAN technology.

This distinction is useful in an exam because the syllabus groups them together but they are not technically the same standard family.

---

# 11. FDDI

## 11.1 Full Form

**FDDI = Fiber Distributed Data Interface**

It is a **100 Mbps fiber-optic LAN technology** based on a **token-passing ring architecture**.

It was designed for high-speed communication, particularly for backbone and campus networking.

---

# 12. FDDI Architecture

FDDI commonly uses **two counter-rotating rings**:

```
             PRIMARY RING
      ┌────────────────────────┐
      │                        ↓
   [A] → [B] → [C] → [D] → [E]
      ↑                        │
      └────────────────────────┘

             SECONDARY RING
      ←────────────────────────
```

The two rings provide **fault tolerance**.

### Normal operation

The primary ring carries normal traffic.

### Failure condition

If a link or station fails, the network can use the secondary ring to create a reconfigured path.

```
Normal:

A → B → C → D → E

Failure:

A → B   X   C → D → E
      \________________/
       Secondary ring
```

This is called **ring wrapping** or **self-healing ring operation**.

---

# 13. FDDI Characteristics

### 1. Fiber optic medium

Provides high-speed communication and long-distance capability.

### 2. Token passing

A token circulates around the ring.

Only the station holding the token can transmit.

```
A → B → C → D → A
        TOKEN →
```

### 3. Dual ring

Provides redundancy and fault tolerance.

### 4. 100 Mbps

The classical FDDI data rate is **100 Mbps**.

### 5. Large network span

FDDI was designed to support substantial campus/backbone distances.

### 6. Fault tolerance

The dual-ring architecture allows recovery from certain link/station failures.

---

# 14. FDDI Operation

Suppose station A wants to transmit:

```
A → B → C → D
```

The token moves around the ring.

When A receives the token:

```
TOKEN → A
```

A captures the token and transmits data.

After transmission, the token is released so another station can transmit.

### Basic process

```
Token arrives
      ↓
Station captures token
      ↓
Transmit data
      ↓
Release token
      ↓
Next station gets opportunity
```

---

# 15. FDDI Advantages and Limitations

### Advantages

- High speed for its era.
- Fiber provides long-distance communication.
- Token passing provides controlled access.
- Dual ring improves reliability.
- Suitable for campus and backbone environments.

### Limitations

- More expensive than ordinary Ethernet.
- More complex installation and maintenance.
- Modern Ethernet technologies largely replaced it in most deployments.

---

# 16. DATA LINK LAYER

The **Data Link Layer** is Layer 2 of the OSI model.

Its major responsibilities include:

- Framing
- Physical addressing
- Error detection/control
- Flow control
- Medium access control

For this unit, we focus especially on:

```
Data Link Layer
       │
       ├── Elementary Data Link Protocols
       ├── Sliding Window Protocols
       └── Error Handling
```

---

# 17. ELEMENTARY DATA LINK PROTOCOLS

Elementary data-link protocols illustrate the fundamental mechanisms used to transfer frames between two directly connected stations.

They are often presented using increasing levels of complexity:

1. **Unrestricted simplex protocol**
2. **Simplex stop-and-wait protocol**
3. **Simplex protocol for a noisy channel**

---

# 18. UNRESTRICTED SIMPLEX PROTOCOL

## Definition

An **unrestricted simplex protocol** assumes:

- Data flows only in one direction.
- The communication channel is error-free.
- The receiver can process frames immediately.
- The sender never needs to retransmit.

```
Sender
   |
   | Frame 1
   ↓
Receiver
   |
   | Frame 2
   ↓
Receiver
```

### Working

The sender continuously sends frames.

```
Sender                      Receiver

 Frame 1 ───────────────────→
 Frame 2 ───────────────────→
 Frame 3 ───────────────────→
 Frame 4 ───────────────────→
```

No acknowledgements are required.

### Limitation

This is an **idealized model**.

In real networks:

- The receiver may be slower.
- Frames may be lost.
- Frames may be damaged.

---

# 19. SIMPLEX STOP-AND-WAIT PROTOCOL

To handle a receiver that cannot process frames as quickly as the sender, **flow control** is introduced.

The sender sends **one frame and waits before sending the next**.

```
Sender                         Receiver

 Frame 1 ─────────────────────→
          wait
          wait
 Frame 2 ─────────────────────→
          wait
 Frame 3 ─────────────────────→
```

In the classic idealized "simplex stop-and-wait" protocol, the receiver's ability to process each frame provides the required pacing.

### Main idea

> **Send one frame → wait → send next frame**

### Limitation

It may waste channel capacity because the sender remains idle while waiting.

---

# 20. SIMPLEX PROTOCOL FOR A NOISY CHANNEL

Now assume the channel can introduce errors or lose frames.

The sender needs:

- **Acknowledgements (ACKs)**
- **Retransmission**
- **Sequence numbers**
- **Timers**

### Working

```
Sender                         Receiver

 Frame 0 ─────────────────────→
          ←──────────────── ACK 0

 Frame 1 ─────────────────────→
          ←──────────────── ACK 1

 Frame 0 ─────────────────────→
          ←──────────────── ACK 0
```

If a frame is lost:

```
Sender                         Receiver

 Frame 1 ───────X

          [Timer expires]

 Frame 1 ─────────────────────→
          ←──────────────── ACK 1
```

### Why sequence numbers are needed

Suppose the ACK is lost and the sender retransmits a frame.

Without a sequence number, the receiver may treat the duplicate as a new frame.

```
Original Frame 0
        ↓
       lost ACK
        ↓
Sender retransmits Frame 0
        ↓
Receiver needs to identify it as duplicate
```

Thus, sequence numbers distinguish new frames from retransmissions.

---

# 21. STOP-AND-WAIT ARQ

The noisy-channel protocol is an example of **Stop-and-Wait ARQ**.

**ARQ = Automatic Repeat reQuest**

### Basic operation

```
Send frame
    ↓
Start timer
    ↓
ACK received?
   /     \
 Yes      No
 ↓         ↓
Next      Timeout
frame       ↓
          Retransmit
```

### Main components

- Sequence number
- ACK
- Timer
- Retransmission

### Advantages

- Simple.
- Reliable.
- Easy to implement.

### Disadvantage

- Low efficiency on long-delay links because only one frame can be outstanding.

---

# 22. SLIDING WINDOW PROTOCOLS

## 22.1 Definition

A **sliding window protocol** allows the sender to transmit **multiple frames before receiving acknowledgements**, up to a permitted window size.

This improves channel utilization.

### Stop-and-Wait

```
Send F1
 ↓
Wait ACK
 ↓
Send F2
 ↓
Wait ACK
```

### Sliding Window

```
F1 → F2 → F3 → F4 → F5
          ACKs coming back
```

The sender does not have to stop after every frame.

---

# 23. Sliding Window Concept

Suppose the window size is 4:

```
Sequence numbers:

0   1   2   3   4   5   6   7
┌───────────────┐
│ 0 1 2 3       │
└───────────────┘
    Sender Window
```

The sender can send frames:

```
0, 1, 2, 3
```

When ACK for frame 0 arrives:

```
1   2   3   4   5   6   7
┌───────────────┐
│ 1 2 3 4       │
└───────────────┘
```

The window has **slid forward**.

---

# 24. Why Sliding Window is Needed

Stop-and-wait wastes time:

```
Transmission
     ↓
     ↓
   Wait
     ↓
Transmission
     ↓
   Wait
```

Sliding window keeps multiple frames in transit:

```
F1 → F2 → F3 → F4
      ↑
   ACKs return
```

Therefore:

> **Sliding window improves link utilization and throughput, especially on high-delay links.**

---

# 25. Types of Sliding Window Protocols

Important versions are:

1. **One-Bit Sliding Window**
2. **Go-Back-N ARQ**
3. **Selective Repeat ARQ**

---

# 26. ONE-BIT SLIDING WINDOW PROTOCOL

A **one-bit sliding window protocol** uses a **1-bit sequence number**, so sequence numbers alternate between:

```
0, 1, 0, 1, ...
```

The effective window size is one frame at a time, making it closely related to Stop-and-Wait ARQ while illustrating the sliding-window concept.

### Operation

```
Frame 0 ─────────────────→
        ←──────── ACK 0

Frame 1 ─────────────────→
        ←──────── ACK 1

Frame 0 ─────────────────→
```

### Main purpose

It allows the receiver to distinguish between:

- A new frame
- A retransmission of the previous frame

### Important point

> One-bit sequence numbering is sufficient when only one unacknowledged frame can be outstanding.

---

# 27. GO-BACK-N ARQ

## Definition

**Go-Back-N ARQ** allows multiple outstanding frames.

If a frame is lost or damaged, the sender **retransmits that erroneous frame and all subsequent frames that were sent after it**.

### Example

Suppose:

```
F0 → F1 → F2 → F3 → F4
```

F2 is lost:

```
F0 → F1 → X → F3 → F4
```

Receiver detects the missing F2.

The sender goes back to F2 and retransmits:

```
F2 → F3 → F4
```

Hence the name:

> **Go-Back-N**

### Diagram

```
Sender                         Receiver

 F0 ─────────────────────────→
 F1 ─────────────────────────→
 F2 ─────────X
 F3 ─────────────────────────→
 F4 ─────────────────────────→

             Missing F2 detected
                     ↓

 F2 ─────────────────────────→
 F3 ─────────────────────────→
 F4 ─────────────────────────→
```

### Advantages

- Simpler than Selective Repeat.
- Receiver implementation is relatively simple.
- Good when errors are infrequent.

### Disadvantage

Correct frames after the damaged/lost frame may need retransmission.

---

# 28. SELECTIVE REPEAT ARQ

## Definition

In **Selective Repeat ARQ**, only the **lost or damaged frames are retransmitted**.

### Example

```
F0 → F1 → F2 → F3 → F4
          ↑
       F2 lost
```

Instead of retransmitting F2, F3 and F4:

```
Only F2 is retransmitted
```

```
F2 ─────────────────────────→
```

### Diagram

```
Sender                         Receiver

 F0 ─────────────────────────→
 F1 ─────────────────────────→
 F2 ─────────X
 F3 ─────────────────────────→
 F4 ─────────────────────────→

 Receiver buffers F3 and F4

 F2 ─────────────────────────→

 F0 F1 F2 F3 F4
      Reassembled
```

### Advantages

- Saves bandwidth.
- Only erroneous/lost frames are retransmitted.
- Efficient on noisy links.

### Disadvantages

- More complex.
- Receiver needs buffer space.
- Requires more sophisticated sequence-number management.

---

# 29. GO-BACK-N vs SELECTIVE REPEAT

|Feature|Go-Back-N|Selective Repeat|
|---|---|---|
|Error handling|Retransmits error frame and subsequent frames|Retransmits only erroneous/lost frames|
|Receiver buffering|Less|More|
|Complexity|Lower|Higher|
|Efficiency with errors|Lower|Higher|
|Out-of-order frames|Usually discarded|Can be buffered|
|Bandwidth usage during errors|Higher|Lower|

### Memory trick

> **GBN → Go back and repeat many**

> **SR → Select only the damaged/lost one**

---

# 30. ERROR HANDLING

## 30.1 What is an Error?

An error occurs when the data received at the destination **differs from the data sent by the source**.

Example:

```
Sent:      10110010
Received:  10100010
               ↑
             Error
```

Errors occur because of:

- Noise
- Interference
- Signal attenuation
- Hardware problems
- Transmission problems

---

# 31. Types of Errors

## A. Single-Bit Error

Only one bit changes.

```
Sent:      10110101
Received:  10100101
               ↑
             Error
```

## B. Burst Error

Two or more bits in a data unit are affected.

```
Sent:      110101100101
Received:  110010110101
              └───┘
             Burst error
```

### Important point

In practical communication systems, **burst errors are common and more important** than isolated single-bit errors.

---

# 32. Error Detection and Error Correction

Error handling has two broad approaches:

```
                  ERROR HANDLING
                       │
             ┌─────────┴─────────┐
             ↓                   ↓
       Error Detection      Error Correction
             │                   │
       Detect error        Find/correct error
             ↓                   ↓
       Retransmission       Correct at receiver
```

### Error Detection

The receiver determines whether the received data contains an error.

Examples:

- Parity
- Checksum
- CRC

### Error Correction

The receiver identifies and corrects the erroneous bit(s) without requiring retransmission in suitable schemes.

Example:

- Hamming code

---

# 33. PARITY CHECK

Parity is one of the simplest error-detection techniques.

An additional bit called a **parity bit** is added to the data.

There are two types:

- Even parity
- Odd parity

---

## 33.1 Even Parity

The parity bit is chosen so the total number of 1s becomes **even**.

Example:

Data:

```
1011001
```

Number of 1s = 4, already even.

Therefore:

```
Data + parity = 10110010
                 ↑
             parity bit
```

If the number of 1s is odd, the parity bit becomes 1 to make the total even.

### Limitation

Parity can reliably detect many single-bit errors, but it cannot detect **all multiple-bit errors**, especially errors that change an even number of bits.

---

# 34. CHECKSUM

In a **checksum**, data is divided into fixed-size words, and the words are added using binary arithmetic.

A complement of the sum is transmitted as the checksum.

### Basic process

```
Data Words
   ↓
Binary addition
   ↓
Sum
   ↓
Complement
   ↓
Checksum
```

At the receiver:

```
Received data + checksum
          ↓
       Recalculate
          ↓
      Check result
```

If the calculated result indicates a mismatch, an error is detected.

### Common use

Checksums are widely used in communication and networking protocols because they are simple to implement.

---

# 35. CRC — CYCLIC REDUNDANCY CHECK

**CRC** is a powerful error-detection method widely used in data communication.

It is based on **polynomial division over binary values**.

### Basic idea

The sender divides the data by a predefined **generator polynomial**.

The remainder becomes the CRC.

```
Data + zeros
     ↓
Binary polynomial division
     ↓
Remainder
     ↓
CRC
```

The CRC is appended to the frame.

### Receiver

The receiver performs the same division.

```
Received frame
      ↓
Divide by generator
      ↓
Remainder = 0 ?
   /          \
 Yes           No
 ↓              ↓
No detected     Error
error
```

---

# 36. CRC Example Structure

Suppose:

```
Data = 110101
Generator = 1011
```

The sender:

1. Appends zeros to the data.
2. Performs modulo-2 division.
3. Gets the remainder.
4. Appends the remainder to the original data.

```
             Data
          110101000
                ÷
             1011
                ↓
          Remainder
                ↓
        Transmitted Frame
```

### Why CRC is important

CRC is very effective at detecting:

- Single-bit errors
- Many multiple-bit errors
- Burst errors

### Exam point

> **CRC is generally more powerful for burst-error detection than simple parity checking.**

---

# 37. HAMMING CODE

**Hamming code** is an error-control technique that can **detect and correct certain bit errors**.

For the basic Hamming code, additional **parity bits** are inserted at positions:

```
1, 2, 4, 8, 16, ...
```

### Example structure

```
Position:
1  2  3  4  5  6  7
P1 P2 D1 P4 D2 D3 D4
```

where:

- P = parity bit
- D = data bit

The parity bits are chosen to cover specific groups of positions.

At the receiver, parity checks produce a **syndrome** indicating the position of an erroneous bit.

### Basic Hamming principle

```
Received Code
      ↓
Parity checks
      ↓
Syndrome
      ↓
Error position
      ↓
Correct bit
```

For the basic Hamming code, **single-bit errors can be corrected**.

---

# 38. ARQ — AUTOMATIC REPEAT REQUEST

ARQ uses feedback from the receiver to achieve reliable transmission.

The main mechanisms are:

- ACK
- NACK
- Timeout
- Retransmission

### ACK

**Acknowledgement**

Indicates successful receipt.

```
Frame →→→→→ Receiver
        ← ACK
```

### NACK

**Negative Acknowledgement**

Indicates that the frame was incorrect or missing.

```
Frame →→→→→ Receiver
        ← NACK
```

The sender then retransmits.

---

# 39. ARQ Working

```
             Send Frame
                  ↓
             Start Timer
                  ↓
             Wait for ACK
                  ↓
           ┌──────┴──────┐
           ↓             ↓
        ACK received   Timeout/NACK
           ↓             ↓
       Send next       Retransmit
        frame            frame
```

ARQ is used by protocols such as:

- Stop-and-Wait ARQ
- Go-Back-N ARQ
- Selective Repeat ARQ

---

# 40. FLOW CONTROL AND ERROR CONTROL

These are different concepts.

## Flow Control

Prevents a **fast sender from overwhelming a slow receiver**.

Example:

```
Fast Sender ─────────→ Slow Receiver
                  ↓
             Buffer overflow
```

Sliding-window mechanisms provide flow control.

## Error Control

Deals with:

- Lost frames
- Damaged frames
- Duplicate frames
- Retransmission

```
Error Control
     ↓
Detection
     ↓
ACK / NACK / Timeout
     ↓
Retransmission or correction
```

### Important distinction

> **Flow control = speed management**

> **Error control = reliability management**

---

# 41. COMPLETE UNIT FLOW

```
                 UNIT – 2
                    │
        ┌───────────┴────────────┐
        ↓                        ↓
      MAC                        DLL
        │                        │
   Channel Allocation      Data Link Protocols
        │                        │
   ┌────┴────┐             ┌─────┴─────────────┐
   ↓         ↓             ↓                   ↓
 Static    Dynamic      Elementary          Sliding
   │         │          Protocols            Window
   │         │             │                   │
 FDMA/TDMA  ALOHA     Stop-and-Wait       ┌────┼────┐
   │         │             │              ↓    ↓    ↓
   │     ┌───┴───┐         │             1-bit GBN   SR
   │     ↓       ↓         │
   │   Pure    Slotted     │
   │   ALOHA    ALOHA      │
   │                       │
   └──── IEEE + FDDI ──────┘
                             
                         Error Handling
                              │
                  ┌───────────┴────────────┐
                  ↓                        ↓
              Detection               Correction
                  │                        │
            ┌─────┼─────┐                Hamming
            ↓     ↓     ↓
          Parity Checksum CRC
```

---

# 42. IMPORTANT DIFFERENCES FOR EXAMS

## Pure ALOHA vs Slotted ALOHA

|Pure ALOHA|Slotted ALOHA|
|---|---|
|Send anytime|Send only at slot boundary|
|No synchronization|Synchronization required|
|Vulnerable period = 2T|Vulnerable period = T|
|Max efficiency = 18.4%|Max efficiency = 36.8%|

---

## FDMA vs TDMA

|FDMA|TDMA|
|---|---|
|Divides frequency|Divides time|
|Different frequency bands|Different time slots|
|Simultaneous transmission possible|Different stations transmit in different slots|
|Wastes unused frequency allocation|Wastes unused time slots|

---

## Stop-and-Wait vs Sliding Window

|Stop-and-Wait|Sliding Window|
|---|---|
|One outstanding frame|Multiple outstanding frames|
|More waiting|Better link utilization|
|Simple|More complex|
|Lower throughput on long-delay links|Higher throughput|

---

## Go-Back-N vs Selective Repeat

|Go-Back-N|Selective Repeat|
|---|---|
|Retransmits error frame and later frames|Retransmits only error/lost frame|
|Simpler|More complex|
|Less buffering|More buffering|
|Less efficient under errors|More efficient under errors|

---

## Error Detection vs Error Correction

|Error Detection|Error Correction|
|---|---|
|Detects whether an error occurred|Determines/corrects certain errors|
|Often followed by retransmission|Can correct without retransmission|
|Parity, checksum, CRC|Hamming code|

---

# 43. IMPORTANT FORMULAS

### Pure ALOHA

\[ S = Ge^{-2G} \]

Maximum:

\[ G=0.5 \]\[ S_{max}=\frac{1}{2e}\approx18.4\% \]

---

### Slotted ALOHA

\[ S=Ge^{-G} \]

Maximum:

\[ G=1 \]\[ S_{max}=\frac{1}{e}\approx36.8\% \]

### Remember

```
Pure ALOHA      → 2T → 18.4%
Slotted ALOHA   → T  → 36.8%
```

---

# 44. VERY IMPORTANT TERMS

|Term|Meaning|
|---|---|
|**MAC**|Controls access to shared transmission medium|
|**Channel Allocation**|Decides how multiple stations share a channel|
|**FDMA**|Frequency-based allocation|
|**TDMA**|Time-based allocation|
|**ALOHA**|Random-access protocol|
|**FDDI**|Fiber Distributed Data Interface|
|**Frame**|Data-link layer transmission unit|
|**ACK**|Positive acknowledgement|
|**NACK**|Negative acknowledgement|
|**ARQ**|Automatic Repeat reQuest|
|**Sliding Window**|Allows multiple outstanding frames|
|**Go-Back-N**|Retransmits from erroneous frame onward|
|**Selective Repeat**|Retransmits only erroneous/lost frames|
|**Parity**|Simple error detection|
|**Checksum**|Arithmetic-based error detection|
|**CRC**|Polynomial-based error detection|
|**Hamming Code**|Error detection/correction using parity positions|

---

# 45. EXAM-FOCUSED QUESTIONS FROM UNIT 2

### Long-answer questions

1. Explain the **MAC sublayer** and the need for channel allocation.
2. Explain **static and dynamic channel allocation**.
3. Explain **Pure ALOHA and Slotted ALOHA** with their working, vulnerable periods and throughput.
4. Compare **Pure ALOHA and Slotted ALOHA**.
5. Explain the important **IEEE 802 standards**.
6. Explain the architecture and working of **FDDI**.
7. Explain **elementary data-link protocols**.
8. Explain **Stop-and-Wait ARQ**.
9. Explain **Sliding Window Protocols**.
10. Explain **Go-Back-N and Selective Repeat ARQ**.
11. Explain **error detection and error correction techniques**.
12. Explain **Parity, Checksum, CRC and Hamming Code**.

### Numericals you should prepare

- Pure ALOHA throughput
- Slotted ALOHA throughput
- Finding maximum throughput
- Basic CRC division
- Basic Hamming-code parity positions
- Sliding-window sequence-number/window-size questions

---

# 46. ONE-PAGE LAST-MINUTE REVISION

```
MAC SUBLAYER
    ↓
Controls access to shared medium

CHANNEL ALLOCATION
    │
    ├── Static
    │     ├── FDMA → Frequency
    │     └── TDMA → Time
    │
    └── Dynamic
          └── Random/Controlled access
                ↓
              ALOHA

ALOHA
    │
    ├── Pure ALOHA
    │      ├── Anytime transmission
    │      ├── Vulnerable period = 2T
    │      └── Max = 18.4%
    │
    └── Slotted ALOHA
           ├── Slot-boundary transmission
           ├── Vulnerable period = T
           └── Max = 36.8%

IEEE
    ├── 802.3 → Ethernet
    ├── 802.11 → Wi-Fi
    ├── 802.15 → WPAN
    ├── 802.16 → Broadband Wireless
    └── 802.5 → Token Ring

FDDI
    ├── Fiber Distributed Data Interface
    ├── 100 Mbps
    ├── Token passing
    └── Dual counter-rotating rings

DATA LINK PROTOCOLS
    │
    ├── Unrestricted Simplex
    ├── Simplex Stop-and-Wait
    └── Noisy Channel / Stop-and-Wait ARQ

SLIDING WINDOW
    │
    ├── One-Bit
    ├── Go-Back-N
    └── Selective Repeat

ERROR HANDLING
    │
    ├── Detection
    │     ├── Parity
    │     ├── Checksum
    │     └── CRC
    │
    └── Correction
          └── Hamming Code
```

## Final exam priority

For this unit, the topics that deserve the **most preparation** are:

**ALOHA numericals and comparison → Sliding Window → Go-Back-N vs Selective Repeat → CRC → Hamming Code → Channel Allocation → FDDI → Elementary Data Link Protocols → IEEE standards.**