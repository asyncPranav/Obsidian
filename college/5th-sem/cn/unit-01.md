
# Computer Network 

A **computer network** is a collection of two or more computers and other devices that are connected to each other so they can communicate, share data, and share resources such as files, printers, software, and internet access.

**Simple definition:**

> A computer network is a group of interconnected computers and devices that exchange information and share resources through communication links.

**Example:**  
A school computer lab where all computers are connected to a central server and can access the internet and shared files is a computer network.


----


### Goals of Computer Networks

The main goals of a computer network are:

1. **Resource Sharing** – Share hardware, software, files, printers, and internet connections.
    
2. **Data Sharing** – Allow users to quickly exchange data and information.
    
3. **Communication** – Enable communication through email, messaging, video calls, etc.
    
4. **Reliability** – Provide backup and alternative ways to access data or services if one system fails.
    
5. **Cost Reduction** – Reduce costs by sharing resources such as printers, storage, and internet connections.
    
6. **Scalability** – Make it easy to add new computers and devices to the network.
    
7. **Centralized Management** – Allow administrators to manage users, devices, security, and data from a central location.
    
8. **Remote Access** – Allow users to access resources and services from different locations.
    
9. **Security** – Protect data and network resources from unauthorized access.


---

### Applications of Computer Networks

1. **Communication** – Email, instant messaging, voice calls, and video conferencing.
    
2. **Resource Sharing** – Sharing printers, storage, software, and internet connections.
    
3. **File Sharing** – Transferring and accessing files between computers.
    
4. **Internet & Web Services** – Browsing websites, online search, and accessing cloud services.
    
5. **Online Banking** – Internet banking, online payments, and ATM networks.
    
6. **E-Commerce** – Online shopping, ordering, and digital payments.
    
7. **Education** – Online classes, virtual labs, digital libraries, and e-learning platforms.
    
8. **Entertainment** – Online gaming, video streaming, music streaming, and social media.
    
9. **Business & Organizations** – Sharing databases, applications, and communication between offices.
    
10. **Cloud Computing** – Accessing applications, storage, and computing resources over the internet.
    
11. **Healthcare** – Sharing medical records, telemedicine, and communication between hospitals.
    
12. **Remote Access** – Accessing computers, servers, and organizational resources from anywhere.

---


## Data Communication

**Definition:**  
Data communication is the process of **exchanging data between two or more devices through a transmission medium using a set of rules called protocols.**

### Components of Data Communication

There are **5 basic components**:

1. **Sender (Source)** – The device that sends the data.
    
    - Example: Computer, mobile phone
        
2. **Receiver (Destination)** – The device that receives the data.
    
    - Example: Computer, mobile phone
        
3. **Message** – The actual information being transmitted.
    
    - Example: Text, image, audio, video, file
        
4. **Transmission Medium (Channel)** – The path through which data travels.
    
    - Example: Ethernet cable, fiber-optic cable, Wi-Fi
        
5. **Protocol** – A set of rules that controls how data is communicated between devices.
    
    - Example: TCP/IP, HTTP, FTP
        

### Text Diagram

```text
              DATA COMMUNICATION

   ┌──────────┐                         ┌──────────┐
   │  Sender  │                         │ Receiver │
   │ (Source) │                         │(Dest.)   │
   └────┬─────┘                         └────▲─────┘
        │                                    │
        │          Message / Data            │
        ▼                                    │
   ┌──────────┐    ───────────────>     ┌──────────┐
   │ Encoding │                         │ Decoding │
   └────┬─────┘                         └────▲─────┘
        │                                    │
        ▼                                    │
   ┌─────────────────────────────────────────────┐
   │       Transmission Medium / Channel         │
   │     (Cable / Fiber / Wireless / Wi-Fi)      │
   └─────────────────────────────────────────────┘
                         │
                         ▼
                    Data Travels

             ┌──────────────────┐
             │     Protocol     │
             │   (Rules of      │
             │  Communication)  │
             └──────────────────┘
```

### Easy Flow to Remember

**Sender → Message → Transmission Medium → Receiver**

And **Protocol** defines the rules used throughout the communication.

**Example:** When you send a WhatsApp message, your phone is the **sender**, the text is the **message**, Wi-Fi/mobile network is the **transmission medium**, your friend's phone is the **receiver**, and communication protocols control how the data is transmitted.


---


## Transmission Mode

**Definition:**  
Transmission mode refers to the **direction in which data can flow between two connected devices**.

There are **3 types of transmission modes**:

### 1. Simplex Mode

Data flows in **only one direction**.

```text
Sender ───────────────► Receiver
```

- Receiver cannot send data back.
    
- Example: **Keyboard → Computer**, TV broadcasting.
    
- **Easy to remember:** One-way communication.
    

---

### 2. Half-Duplex Mode

Data can flow in **both directions, but not at the same time**.

```text
Device A ─────────────► Device B
         ◄─────────────
```

- Both devices can send and receive.
    
- Only one device transmits at a time.
    
- Example: **Walkie-talkie**.
    
- **Easy to remember:** Two-way, but one at a time.
    

---

### 3. Full-Duplex Mode

Data can flow in **both directions simultaneously**.

```text
Device A ◄════════════► Device B
          Both ways
          at the same time
```

- Both devices can send and receive at the same time.
    
- Example: **Telephone/video call**.
    
- **Easy to remember:** Two-way simultaneously.
    

### Quick Comparison

|Mode|Direction|Simultaneous?|Example|
|---|---|---|---|
|**Simplex**|One-way|❌|TV broadcast|
|**Half-Duplex**|Both ways|❌|Walkie-talkie|
|**Full-Duplex**|Both ways|✅|Telephone call|

**Shortcut:**  
**Simplex = One-way** → **Half-duplex = Two-way, one at a time** → **Full-duplex = Two-way, simultaneously**.


---

## Network Criteria

A computer network is generally evaluated using **three main criteria**:

### 1. Performance

Performance refers to **how efficiently and quickly a network transfers data**.

It depends on:

- **Bandwidth** – Amount of data that can be transmitted per second.
    
- **Throughput** – Actual amount of data successfully transferred.
    
- **Delay/Latency** – Time taken for data to travel from sender to receiver.
    

### 2. Reliability

Reliability refers to the **ability of a network to work correctly and continuously**.

It depends on:

- Failure frequency
    
- Recovery time after failure
    
- Availability of the network
    

### 3. Security

Security refers to **protecting network data and resources from unauthorized access or attacks**.

It includes:

- Data confidentiality
    
- Data integrity
    
- Authentication
    
- Protection against unauthorized access
    

### In short:

```text
Network Criteria
      │
      ├── Performance
      ├── Reliability
      └── Security
```

---

# Types of Connection

A connection describes **how devices are connected to each other in a network**.

There are mainly **two types**:

## 1. Point-to-Point Connection

A **dedicated connection** exists between two devices.

```text
Device A ═══════════ Device B
          Dedicated
          connection
```

- Only two devices are involved.
    
- The entire communication link is used by these two devices.
    
- Example: Direct connection between two computers.
    

**Advantages:** Fast and secure because the connection is dedicated.

---

## 2. Multipoint Connection

A **single communication link is shared by multiple devices**.

```text
             Device A
                │
                │
Device B ───────┼─────── Device C
                │
                │
             Device D
```

- Multiple devices share the same connection.
    
- Also called a **multidrop connection**.
    
- Example: Multiple devices connected through a shared communication medium.
    

### Quick Comparison

|Feature|Point-to-Point|Multipoint|
|---|---|---|
|Devices|2|More than 2|
|Connection|Dedicated|Shared|
|Communication link|Used by two devices|Shared by multiple devices|
|Example|Direct computer-to-computer link|Shared network medium|

**Remember:**  
**Point-to-Point = One link, two devices**  
**Multipoint = One link, multiple devices**


---

# Network Topology

### Definition

**Network topology** refers to the **physical or logical arrangement of devices (nodes) and connections (links) in a computer network.**

In simple words, it tells us **how computers and other network devices are connected to each other.**

## Types of Network Topology

The main types are:

1. Bus Topology
    
2. Star Topology
    
3. Ring Topology
    
4. Mesh Topology
    
5. Tree Topology
    
6. Hybrid Topology
    

---

## 1. Bus Topology

All devices are connected to a **single main cable called the backbone**.

### Diagram

```text
        Main Backbone Cable
══════════════════════════════════════
    │          │          │          │
    │          │          │          │
  [PC1]      [PC2]      [PC3]      [PC4]

                         │
                    Terminator
```

**Advantages:**

- Simple and inexpensive
    
- Requires less cable
    

**Disadvantages:**

- Failure of backbone can affect the entire network
    
- Difficult to troubleshoot
    
- Performance decreases when many devices are connected
    

**Example:** Older Ethernet networks.

---

# 2. Star Topology

All devices are connected to a **central device**, such as a switch or hub.

### Diagram

```text
                 [PC1]
                   │
                   │
[PC2] ───────── [ SWITCH ] ───────── [PC3]
                   │
                   │
                 [PC4]
                   │
                 [PC5]
```

**Advantages:**

- Easy to install and manage
    
- Failure of one cable affects only one device
    
- Easy to add or remove devices
    

**Disadvantages:**

- Failure of the central device affects the entire network
    
- Requires more cable than bus topology
    

**Example:** Modern LANs commonly use star topology.

---

# 3. Ring Topology

Each device is connected to **two other devices**, forming a closed ring.

### Diagram

```text
              [PC1]
             /     \
           /         \
       [PC2]         [PC4]
          \           /
           \         /
             [PC3]
```

Data travels around the ring from one device to another.

**Advantages:**

- Organized data transmission
    
- Less chance of data collision in some ring implementations
    

**Disadvantages:**

- Failure of a link/device can disrupt the network
    
- Adding or removing devices can be difficult
    

---

# 4. Mesh Topology

In mesh topology, devices are connected to **multiple or all other devices**.

There are two types:

### A. Full Mesh

Every device is directly connected to **every other device**.

```text
             [PC1]
            /  |  \
           /   |   \
        [PC2]--|--[PC3]
          \    |    /
           \   |   /
             [PC4]
```

**Advantages:**

- Very reliable
    
- Multiple paths are available
    
- Failure of one connection does not necessarily stop communication
    

**Disadvantages:**

- Very expensive
    
- Requires a large number of connections
    
- Difficult to install and maintain
    

### B. Partial Mesh

Only some devices have connections to multiple other devices.

```text
       [PC1]────────[PC2]
         │  \        │
         │   \       │
         │    \      │
       [PC3]────────[PC4]
```

---

# 5. Tree Topology

Tree topology combines characteristics of **star and bus topologies** and has a **hierarchical structure**.

### Diagram

```text
                    [Core Switch]
                    /           \
                   /             \
            [Switch 1]         [Switch 2]
             /    \             /    \
            /      \           /      \
         [PC1]    [PC2]      [PC3]    [PC4]
```

**Advantages:**

- Easy to expand
    
- Suitable for large networks
    
- Hierarchical management
    

**Disadvantages:**

- Failure of a higher-level device can affect its branch
    
- Requires more cable and configuration
    

**Example:** Large organizational networks.

---

# 6. Hybrid Topology

Hybrid topology is a **combination of two or more different topologies**.

### Diagram

```text
             STAR                         STAR

          [PC1]                          [PC4]
             \                             /
              \                           /
            [Switch 1] ═════════════ [Switch 2]
              /                           \
             /                             \
          [PC2]                           [PC5]
                                            \
                                            [PC6]
```

Here, different network sections use star topology and are connected together.

**Advantages:**

- Flexible
    
- Easily scalable
    
- Can be designed according to requirements
    

**Disadvantages:**

- More expensive
    
- Complex to design and manage
    

---

# Quick Revision Table

|Topology|Basic Structure|Main Feature|
|---|---|---|
|**Bus**|Single backbone|One main cable|
|**Star**|Central device|All devices connect to center|
|**Ring**|Circular|Each device connects to two others|
|**Mesh**|Multiple connections|Multiple paths|
|**Tree**|Hierarchical|Combination of star/bus structure|
|**Hybrid**|Combination|Two or more topologies combined|

### 🧠 Easy Way to Remember

```text
BUS    → ─────────────  (One backbone)

STAR   →      ●        (Central device)
             /|\
            / | \

RING   →    ○──○
            │  │
            ○──○

MESH   →    ○──○
           /│\/│
          ○─┼─○
           \│/

TREE   →       ●
              / \
             ●   ●
            / \ / \

HYBRID → Combination of different topologies
```

**Most important for exams:** Know the **definition, diagram, advantages, disadvantages, and examples** of each topology.




----

# Types of Computer Networks — Exam Perspective

A **computer network** can be classified into different types mainly on the basis of **geographical area/coverage**, size, purpose, and ownership.

For exams, the **most important classification is based on geographical coverage**:

> **PAN → LAN → MAN → WAN**

---

## 1. PAN — Personal Area Network

**PAN (Personal Area Network)** is a network used to connect devices located around a **single person**, usually within a very small area.

### Coverage

- Approximately **1–10 meters**
    
- Usually centered around an individual.
    

### Examples

- Connecting a smartphone with Bluetooth headphones.
    
- Connecting a phone with a smartwatch.
    
- Connecting a laptop with a wireless mouse.
    

### Technologies

- Bluetooth
    
- USB
    
- Infrared
    
- Wi-Fi (for some personal connections)
    

### Advantages

- Easy to set up.
    
- Low cost.
    
- Requires very little infrastructure.
    
- Suitable for personal devices.
    

### Disadvantages

- Very short range.
    
- Generally supports fewer devices.
    
- Lower capacity compared with larger networks.
    

### Example diagram

```text
          Smartwatch
              |
              | Bluetooth
              |
       +-------------+
       | Smartphone  |
       +-------------+
          /       \
         /         \
   Earbuds        Laptop
```

### Exam definition

> **PAN is a small network used to connect personal electronic devices within a short range around an individual.**

---

# 2. LAN — Local Area Network

**LAN (Local Area Network)** is a network that connects computers and devices within a **small geographical area**, such as a room, home, office, school, or building.

### Coverage

Usually within:

- Room
    
- Home
    
- Office
    
- School
    
- Building
    
- Small campus
    

### Examples

- Computer lab network.
    
- Office network.
    
- Home Wi-Fi network.
    
- Network connecting computers in a school.
    

### Technologies

- Ethernet
    
- Wi-Fi
    

### Characteristics

- High data transfer speed.
    
- Usually privately owned.
    
- Relatively inexpensive.
    
- Covers a limited geographical area.
    
- Easy to manage.
    

### Advantages

1. **High speed** — Data can be transferred quickly.
    
2. **Resource sharing** — Printers, files, applications, and Internet connections can be shared.
    
3. **Low cost** — Less expensive than large-scale networks.
    
4. **Easy management** — Network can generally be controlled by one organization.
    
5. **Better security** — Easier to control access.
    

### Disadvantages

- Limited geographical coverage.
    
- Initial installation requires networking equipment.
    
- Failure of important network devices can affect many users.
    

### Example diagram

```text
             +-------------+
             |   Switch    |
             +-------------+
              /     |     \
             /      |      \
        PC-1       PC-2    PC-3
                              |
                           Printer
```

### Exam definition

> **LAN is a computer network that connects devices within a limited geographical area such as a home, office, school, or building.**

---

# 3. MAN — Metropolitan Area Network

**MAN (Metropolitan Area Network)** is a network that covers a **larger geographical area than a LAN but smaller than a WAN**, generally covering a city or metropolitan area.

### Coverage

It can cover:

- A city
    
- Large campus
    
- Multiple buildings
    
- Metropolitan region
    

### Example

Suppose a university has several campuses located in different parts of a city and connects them using a high-speed network.

```text
      Campus A
          |
          |
          |
    +-----------+
    |   MAN     |
    |  Network  |
    +-----------+
       /     \
      /       \
Campus B     Campus C
```

### Characteristics

- Larger than LAN.
    
- Smaller than WAN.
    
- Often connects multiple LANs.
    
- Can be operated by a private organization or service provider.
    
- Usually uses high-speed communication links.
    

### Advantages

1. Covers a large geographical area.
    
2. Connects multiple LANs.
    
3. Allows sharing of resources between different locations.
    
4. Provides high-speed communication within a metropolitan region.
    

### Disadvantages

- More expensive than LAN.
    
- More complex to manage.
    
- Security management is more difficult.
    
- Infrastructure installation can be costly.
    

### Exam definition

> **MAN is a network that connects multiple LANs across a city or metropolitan area.**

---

# 4. WAN — Wide Area Network

**WAN (Wide Area Network)** is a network that covers a **very large geographical area**, such as countries, continents, or the entire world.

It connects multiple LANs and MANs over long distances.

### Coverage

- Country
    
- Multiple countries
    
- Continents
    
- Worldwide
    

### Example

The **Internet** is the most well-known example of a WAN.

```text
       LAN
        |
        |
    +-------+
    | Router|
    +-------+
        |
        |
   =====WAN=====
    /          \
   /            \
 LAN            LAN
Office A       Office B
```

### Technologies

WAN may use:

- Fiber-optic cables
    
- Microwave links
    
- Satellite communication
    
- Leased lines
    
- Cellular networks
    

### Characteristics

- Very large geographical coverage.
    
- Connects LANs and MANs.
    
- Usually involves telecommunications/service providers.
    
- More expensive and complex than LAN.
    
- Data transmission may involve multiple networks and routers.
    

### Advantages

1. Covers very large areas.
    
2. Connects geographically distant offices.
    
3. Enables global communication.
    
4. Allows organizations to share resources across locations.
    

### Disadvantages

1. Expensive to establish and maintain.
    
2. More complex to manage.
    
3. Generally has higher latency than LAN.
    
4. Security is more challenging.
    
5. Dependence on communication service providers may exist.
    

### Exam definition

> **WAN is a computer network that connects computers and smaller networks over a large geographical area such as a country, continent, or the entire world.**

---

# 5. CAN — Campus Area Network

**CAN (Campus Area Network)** connects multiple LANs within a **university, college, corporate campus, or similar organization**.

It is generally larger than a LAN but smaller than a MAN.

### Example

```text
       College Campus
   _______________________

   [Engineering Building]
            |
            |
       +---------+
       | Campus  |
       | Network |
       +---------+
        /       \
       /         \
 [Library]    [Admin Block]
```

### Characteristics

- Covers a campus or group of nearby buildings.
    
- Usually privately owned.
    
- Connects multiple LANs.
    
- Common in universities and large organizations.
    

### Exam definition

> **CAN is a network that connects multiple LANs within a limited campus or organizational area.**

---

# 6. HAN — Home Area Network

**HAN (Home Area Network)** is a network used to connect devices within a **home**.

### Examples

- Smartphone
    
- Laptop
    
- Smart TV
    
- Printer
    
- Smart speaker
    
- IoT devices
    

```text
             Internet
                |
             Router
          /     |      \
         /      |       \
     Laptop   Smart TV  Phone
                       |
                  Smart Watch
```

### Characteristics

- Small geographical area.
    
- Usually privately owned.
    
- Uses Wi-Fi, Ethernet, Bluetooth, etc.
    
- Allows sharing of Internet and devices.
    

### Exam definition

> **HAN is a network that connects digital devices within a home for communication and resource sharing.**

---

# 7. WLAN — Wireless Local Area Network

**WLAN (Wireless Local Area Network)** is essentially a LAN in which devices communicate primarily through **wireless communication**, commonly Wi-Fi.

### Example

```text
             Internet
                 |
              Router
                 |
        )))) Wi-Fi ((((
        /       |       \
      Laptop   Phone    Tablet
```

### Advantages

- No need for extensive cables.
    
- Easy to install.
    
- Devices can move within the coverage area.
    
- Easy to expand.
    

### Disadvantages

- Security risks if poorly configured.
    
- Interference can affect performance.
    
- Generally less predictable than wired Ethernet.
    
- Range is limited.
    

---

# Comparison Table — Very Important for Exams

|Type|Full Form|Approx. Coverage|Example|
|---|---|---|---|
|**PAN**|Personal Area Network|Very small/personal range|Phone + smartwatch|
|**LAN**|Local Area Network|Room/building|Computer lab|
|**CAN**|Campus Area Network|Campus/organization|University campus|
|**MAN**|Metropolitan Area Network|City|City-wide network|
|**WAN**|Wide Area Network|Country/continent/world|Internet|
|**HAN**|Home Area Network|Home|Home Wi-Fi|
|**WLAN**|Wireless LAN|Local wireless area|Wi-Fi network|

### Easy order to remember

```text
PAN
 ↓
LAN
 ↓
CAN
 ↓
MAN
 ↓
WAN

Small -------------------------> Large
```

---

# Most Important Difference: LAN vs MAN vs WAN

|Feature|LAN|MAN|WAN|
|---|---|---|---|
|Coverage|Small|City/metropolitan|Very large|
|Connects|Devices|Multiple LANs|LANs/MANs|
|Speed|Generally high|High|Varies|
|Cost|Low|Medium/High|High|
|Complexity|Low|Medium|High|
|Example|Office|City network|Internet|
|Ownership|Usually private|Private/provider|Often multiple providers|

---

## ⭐ Exam Trick

If the question is:

**"Explain types of networks based on geographical area."**

Write these **four first**:

1. **PAN — Personal Area Network**
    
2. **LAN — Local Area Network**
    
3. **MAN — Metropolitan Area Network**
    
4. **WAN — Wide Area Network**
    

Then, if your syllabus includes them, mention **CAN, HAN and WLAN** as additional classifications.

### One-line revision

> **PAN = Person → LAN = Building → MAN = City → WAN = Country/World**

This is the simplest way to remember the hierarchy for an exam.


---


# Network Models — Detailed Exam Notes

## 1. What is a Network Model?

A **network model** is a conceptual framework that describes **how communication between two or more devices takes place over a computer network**.

It divides the complicated networking process into **different layers**, where each layer performs a specific set of functions.

### Why do we need network models?

Network communication involves many activities:

```text
Application
    ↓
Data formatting
    ↓
Reliable delivery
    ↓
Addressing
    ↓
Routing
    ↓
Transmission
    ↓
Physical medium
```

Instead of handling everything together, network models divide these responsibilities into layers.

### Main Network Models

There are two important network models:

1. **OSI Model**
    
2. **TCP/IP Model**
    

---

# 2. OSI Model

**OSI** stands for **Open Systems Interconnection**.

It was developed by the **International Organization for Standardization (ISO)**.

The OSI model consists of **7 layers**.

```text
┌──────────────────────────┐
│  7. Application          │
├──────────────────────────┤
│  6. Presentation         │
├──────────────────────────┤
│  5. Session              │
├──────────────────────────┤
│  4. Transport            │
├──────────────────────────┤
│  3. Network              │
├──────────────────────────┤
│  2. Data Link            │
├──────────────────────────┤
│  1. Physical             │
└──────────────────────────┘
```

### Easy way to remember

From **top to bottom**:

> **A P S T N D P**

**All People Seem To Need Data Processing**

From **bottom to top**:

> **P D N T S P A**

---

# 3. Seven Layers of OSI Model

---

## Layer 1 — Physical Layer

The **Physical Layer** is the lowest layer of the OSI model.

It is responsible for transmitting **raw bits (0s and 1s)** through the physical communication medium.

### Main functions

- Transmission of bits.
    
- Defines physical characteristics of the network.
    
- Defines cables, connectors and interfaces.
    
- Defines voltage/signal levels.
    
- Defines data transmission rate.
    
- Defines physical topology.
    

### Examples

- Ethernet cables
    
- Fiber-optic cables
    
- Radio signals
    
- Hubs
    
- Repeaters
    

### Data unit

**Bits**

```text
Sender
  |
  |  10110101
  ↓
Physical Medium
  |
  ↓
Receiver
```

### Exam definition

> **The Physical Layer is responsible for transmitting raw bits over a physical communication medium.**

---

# Layer 2 — Data Link Layer

The **Data Link Layer** provides **node-to-node delivery** of data.

It takes raw bits from the Physical Layer and organizes them into **frames**.

### Main functions

1. **Framing**
    
    - Converts data into frames.
        
2. **MAC addressing**
    
    - Uses MAC addresses to identify devices on a local network.
        
3. **Error detection**
    
    - Detects errors that occur during transmission.
        
4. **Flow control**
    
    - Controls the rate of data transmission between directly connected devices.
        
5. **Access control**
    
    - Determines which device can use a shared transmission medium.
        

### Devices

- Switch
    
- Bridge
    

### Protocol/examples

- Ethernet
    
- PPP
    
- HDLC
    

### Data unit

**Frame**

```text
Network Layer
      ↓
  ┌───────────┐
  │   Frame   │
  └───────────┘
      ↓
Physical Layer
```

### Exam definition

> **The Data Link Layer provides reliable node-to-node delivery and organizes data into frames.**

---

# Layer 3 — Network Layer

The **Network Layer** is responsible for **source-to-destination delivery across different networks**.

Its most important functions are **logical addressing and routing**.

### Main functions

### 1. Logical addressing

It uses logical addresses such as **IP addresses**.

Example:

```text
192.168.1.10
```

### 2. Routing

Determines the best path for data to travel from source to destination.

```text
Computer A
    |
 Router 1
   /   \
  /     \
R2       R3
  \     /
   \   /
  Router 4
      |
Computer B
```

### 3. Packet forwarding

Forwards packets toward their destination.

### Device

**Router**

### Protocols

- IP
    
- ICMP
    
- IPsec (network-layer security)
    

### Data unit

**Packet**

### Exam definition

> **The Network Layer is responsible for logical addressing, routing, and delivery of packets from source to destination across interconnected networks.**

---

# Layer 4 — Transport Layer

The **Transport Layer** provides **end-to-end communication** between applications running on different devices.

It is one of the most important layers.

### Main functions

1. **Segmentation**
    
    - Breaks large data into smaller units.
        
2. **Reassembly**
    
    - Reconstructs the original data at the destination.
        
3. **Flow control**
    
    - Prevents the sender from overwhelming the receiver.
        
4. **Error control**
    
    - Detects and handles transmission problems.
        
5. **Reliable delivery**
    
    - Ensures data reaches the destination correctly when the protocol provides reliability.
        
6. **Port addressing**
    
    - Uses port numbers to identify applications.
        

Example:

```text
IP Address + Port Number

192.168.1.5 : 443
```

### Important protocols

- **TCP** — reliable, connection-oriented
    
- **UDP** — connectionless, faster but does not provide TCP-style reliability
    

### Data unit

- **Segment** — commonly associated with TCP
    
- **Datagram** — commonly associated with UDP
    

### Exam definition

> **The Transport Layer provides end-to-end delivery, segmentation, flow control, error control, and process-to-process communication.**

---

# Layer 5 — Session Layer

The **Session Layer** establishes, manages, and terminates communication sessions between applications.

### Main functions

- Establishes a session.
    
- Maintains a session.
    
- Terminates a session.
    
- Provides synchronization.
    
- Helps manage communication between applications.
    

### Example

Suppose two systems are communicating:

```text
System A                  System B

   |                         |
   |---- Session Start ----->|
   |                         |
   |<---- Data Exchange ---->|
   |                         |
   |----- Session End ------>|
```

### Exam definition

> **The Session Layer establishes, manages, synchronizes, and terminates communication sessions between applications.**

---

# Layer 6 — Presentation Layer

The **Presentation Layer** is concerned with the **format and representation of data**.

It ensures that data sent by one system can be understood by another system.

### Main functions

### 1. Translation

Converts data between different formats.

Example:

```text
Sender format
      ↓
Translation
      ↓
Receiver format
```

### 2. Encryption/Decryption

Used to transform data into a protected form and back.

### 3. Compression/Decompression

Reduces the size of data before transmission.

### Examples

- Data encoding
    
- Character encoding
    
- Encryption
    
- Compression
    
- JPEG
    
- MPEG
    
- ASCII/Unicode
    

### Exam definition

> **The Presentation Layer handles data translation, encryption, decryption, compression, and decompression.**

---

# Layer 7 — Application Layer

The **Application Layer** is the highest layer of the OSI model.

It provides network services directly to **user applications**.

### Important protocols

- HTTP / HTTPS → Web
    
- FTP → File transfer
    
- SMTP → Email sending
    
- DNS → Domain name resolution
    
- SSH → Secure remote access
    

### Examples

When you open a website:

```text
Browser
   ↓
HTTP/HTTPS
   ↓
Application Layer
```

### Important point

The Application Layer is **not the application itself**.

For example:

```text
Chrome
  ↓
Uses HTTP/HTTPS
  ↓
Application Layer
```

### Exam definition

> **The Application Layer provides network services and interfaces that allow user applications to communicate over a network.**

---

# 4. Complete OSI Layer Table

|Layer|Name|Main Function|Data Unit|Examples/Devices|
|---|---|---|---|---|
|**7**|Application|Network services to applications|Data|HTTP, FTP, SMTP, DNS|
|**6**|Presentation|Translation, encryption, compression|Data|JPEG, encoding, encryption|
|**5**|Session|Establish/manage/terminate sessions|Data|Session management|
|**4**|Transport|End-to-end delivery, reliability, ports|Segment/Datagram|TCP, UDP|
|**3**|Network|Routing and logical addressing|Packet|IP, Router|
|**2**|Data Link|Framing, MAC, error detection|Frame|Ethernet, Switch|
|**1**|Physical|Transmission of raw bits|Bits|Cable, Hub, Repeater|

---

# 5. How Data Travels Through the OSI Model

Suppose **Computer A sends a message to Computer B**.

### At the sender

```text
Application
     ↓
Presentation
     ↓
Session
     ↓
Transport       → Segment
     ↓
Network         → Packet
     ↓
Data Link       → Frame
     ↓
Physical        → Bits
```

At the receiver, the process is reversed:

```text
Bits
 ↓
Frame
 ↓
Packet
 ↓
Segment
 ↓
Session
 ↓
Presentation
 ↓
Application
```

This process is called:

### Encapsulation

At the sender, each lower layer adds its own control information.

```text
Application Data
      ↓
   Segment
      ↓
    Packet
      ↓
    Frame
      ↓
     Bits
```

At the receiver, headers are removed layer by layer.

This is called:

### Decapsulation

```text
Bits
 ↓
Frame
 ↓
Packet
 ↓
Segment
 ↓
Data
```

---

# 6. TCP/IP Model

The **TCP/IP model** is the practical networking model used as the foundation of the modern Internet.

TCP/IP stands for:

> **Transmission Control Protocol / Internet Protocol**

Unlike OSI's seven layers, the commonly taught TCP/IP model has **4 layers**.

```text
┌──────────────────────────┐
│  Application             │
├──────────────────────────┤
│  Transport               │
├──────────────────────────┤
│  Internet                │
├──────────────────────────┤
│  Network Access/Link     │
└──────────────────────────┘
```

---

# 7. Layers of TCP/IP Model

## 1. Application Layer

Provides services to applications.

It combines the functions of:

- OSI Application
    
- OSI Presentation
    
- OSI Session
    

### Protocols

- HTTP/HTTPS
    
- FTP
    
- SMTP
    
- DNS
    
- SSH
    

---

## 2. Transport Layer

Provides communication between applications/processes.

### Protocols

- TCP
    
- UDP
    

### Functions

- Segmentation
    
- Reliability
    
- Flow control
    
- Error control
    
- Port addressing
    

---

## 3. Internet Layer

Responsible for logical addressing and routing.

### Main protocol

**IP**

Other examples:

- ICMP
    
- IPsec
    

### Device

**Router**

### Data unit

**Packet**

---

## 4. Network Access / Link Layer

Responsible for communication over the local network and physical medium.

It combines the functions of:

- OSI Data Link Layer
    
- OSI Physical Layer
    

### Examples

- Ethernet
    
- Wi-Fi
    
- PPP
    

---

# 8. OSI vs TCP/IP Model

This is a **very important exam question**.

|OSI Model|TCP/IP Model|
|---|---|
|7 layers|4 layers commonly|
|Developed by ISO|Developed around DARPA/Internet protocols|
|Mainly a reference/conceptual model|Practical protocol suite/model|
|Session is separate|Session functions are part of Application|
|Presentation is separate|Presentation functions are part of Application|
|Physical is separate|Combined into Network Access/Link|
|Protocol-independent reference model|Built around TCP/IP protocols|
|More detailed layer separation|More practical and less granular|

### Layer mapping

```text
OSI MODEL                    TCP/IP MODEL

Application ───────┐
Presentation ──────┼──────→ Application
Session ───────────┘

Transport ─────────────────→ Transport

Network ───────────────────→ Internet

Data Link ────────┐
Physical ─────────┴───────→ Network Access / Link
```

---

# 9. OSI vs TCP/IP — Short Answer

If asked **"Differentiate between OSI and TCP/IP"**, write:

> The OSI model is a seven-layer reference model developed by ISO, whereas the TCP/IP model is a practical networking model/protocol suite used for Internet communication. OSI has separate Application, Presentation, Session, Transport, Network, Data Link, and Physical layers, while TCP/IP commonly has Application, Transport, Internet, and Network Access/Link layers.

---

# ⭐ 10. Most Important Points for Exam

Remember this table:

|OSI Layer|Keyword|
|---|---|
|**7. Application**|Services|
|**6. Presentation**|Translation|
|**5. Session**|Session|
|**4. Transport**|End-to-end|
|**3. Network**|Routing|
|**2. Data Link**|Frame/MAC|
|**1. Physical**|Bits|

### One-line memory trick

> **Application → Presentation → Session → Transport → Network → Data Link → Physical**

**A P S T N D P**

> **All People Seem To Need Data Processing**

---

# Layered Architecture in Computer Networks

## 1. Definition

**Layered architecture** is a networking design approach in which the communication process is divided into **multiple layers**, and each layer performs a **specific set of functions**.

Each layer:

- Provides services to the layer above it.
    
- Uses services provided by the layer below it.
    
- Communicates logically with the corresponding layer on the other computer.
    

### Simple idea

Instead of making one huge system handle everything:

```text
Sending data
 ├── Application
 ├── Formatting
 ├── Reliable delivery
 ├── Routing
 ├── Framing
 └── Physical transmission
```

we divide it into layers:

```text
┌──────────────────────┐
│      Layer 3         │
├──────────────────────┤
│      Layer 2         │
├──────────────────────┤
│      Layer 1         │
└──────────────────────┘
```

---

# 2. Why is Layered Architecture Needed?

Network communication is very complicated. Layering makes it easier to **design, understand, develop, maintain, and troubleshoot** a network.

### Major reasons:

### 1. Reduces complexity

A large networking problem is divided into smaller problems.

```text
Complex Network Communication
             ↓
     ┌───────┼───────┐
     ↓       ↓       ↓
   Layer   Layer   Layer
```

Each layer focuses on its own task.

---

### 2. Modularity

Each layer works as a separate module.

If one layer needs modification, other layers generally don't need to be redesigned completely.

**Example:**

You can change from Ethernet to Wi-Fi at the lower networking layers without changing how an application such as a web browser works.

---

### 3. Standardization

Layering provides standard interfaces and responsibilities.

Different manufacturers can create networking hardware/software that can work together by following common standards.

---

### 4. Easier troubleshooting

If communication fails, engineers can identify which layer is responsible.

For example:

```text
Website not loading
       ↓
Check Application
       ↓
Check Transport
       ↓
Check Network
       ↓
Check Data Link
       ↓
Check Physical
```

---

### 5. Easier development

Developers can concentrate on one particular layer instead of understanding the entire network implementation.

---

### 6. Interoperability

Different systems and devices can communicate even when they are produced by different manufacturers.

---

# 3. Basic Structure of Layered Architecture

Consider two computers communicating:

```text
        COMPUTER A                         COMPUTER B

     Application Layer  <-------------->  Application Layer
            ↑                                  ↑
            │                                  │
      Transport Layer    <-------------->  Transport Layer
            ↑                                  ↑
            │                                  │
       Network Layer     <-------------->  Network Layer
            ↑                                  ↑
            │                                  │
       Data Link Layer   <-------------->  Data Link Layer
            ↑                                  ↑
            │                                  │
       Physical Layer    <-------------->  Physical Layer
            │
            └────── Physical Medium ───────────┘
```

### Important concept

The layers on the **same level** communicate logically with each other.

For example:

```text
Computer A                         Computer B

Transport Layer  ←──────────────→  Transport Layer
```

But physically, data actually moves **down through the layers on the sender**, travels through the network, and then moves **up through the layers on the receiver**.

---

# 4. How Layered Communication Works

Suppose you send:

> **HELLO**

from Computer A to Computer B.

### Sender side

```text
Application
    ↓
     HELLO
    ↓
Transport
    ↓
  [Header + HELLO]
    ↓
Network
    ↓
[Header + Header + HELLO]
    ↓
Data Link
    ↓
[Header + Packet + Trailer]
    ↓
Physical
    ↓
101010101010...
```

The data is gradually prepared for transmission.

At the receiver:

```text
101010101010...
       ↓
Physical
       ↓
Data Link
       ↓
Network
       ↓
Transport
       ↓
Application
       ↓
     HELLO
```

This is called:

### Encapsulation

At the sender, each layer adds its own control information.

```text
Data
 ↓
Segment
 ↓
Packet
 ↓
Frame
 ↓
Bits
```

At the receiver, this information is removed.

This is called:

### Decapsulation

```text
Bits
 ↓
Frame
 ↓
Packet
 ↓
Segment
 ↓
Data
```

---

# 5. Protocols in Layered Architecture

Each layer generally uses specific **protocols**.

For example, in the TCP/IP model:

```text
Application
    │
    ├── HTTP
    ├── DNS
    ├── FTP
    └── SMTP
    ↓
Transport
    │
    ├── TCP
    └── UDP
    ↓
Internet
    │
    └── IP
    ↓
Network Access / Link
    │
    ├── Ethernet
    └── Wi-Fi
```

A **protocol** is a set of rules that determines how communication takes place.

---

# 6. Important Principles of Layered Architecture

### 1. Each layer has a specific function

For example:

- Transport → end-to-end communication
    
- Network → routing
    
- Data Link → framing
    
- Physical → bit transmission
    

---

### 2. Each layer provides services to the layer above

```text
       Application
            ↑
     uses services of
            │
       Transport
            ↑
     uses services of
            │
        Network
```

---

### 3. Each layer uses services of the layer below

For example:

```text
Application
     ↓ uses
Transport
     ↓ uses
Network
     ↓ uses
Data Link
     ↓ uses
Physical
```

---

### 4. Changes should be localized

A change in one layer should ideally have minimal impact on other layers.

For example, changing the physical medium from copper cable to fiber should not require rewriting an application such as a web browser.

---

# 7. Advantages of Layered Architecture

### 1. Simplicity

Complex networking tasks are divided into manageable layers.

### 2. Modularity

Each layer can be designed and maintained separately.

### 3. Standardization

Provides common standards for communication.

### 4. Easy troubleshooting

Problems can be isolated to particular layers.

### 5. Flexibility

Changes in one layer can often be made without changing the entire system.

### 6. Interoperability

Devices and software from different manufacturers can communicate.

### 7. Easier learning

Students and engineers can understand networking one layer at a time.

---

# 8. Disadvantages of Layered Architecture

Layering also has some disadvantages.

### 1. Overhead

Each layer may add headers or trailers.

```text
Data
 ↓
Header + Data
 ↓
Header + Header + Data
 ↓
Header + Header + Header + Data + Trailer
```

This increases the amount of transmitted information.

### 2. Duplicate functionality

Sometimes different layers may perform related functions, leading to redundancy.

### 3. Reduced efficiency

Strict separation between layers can sometimes make communication less efficient.

### 4. Difficulty in defining layers

It can be difficult to decide exactly which function should belong to which layer.

---

# 9. Layered Architecture vs Network Model

These terms are related but not exactly the same.

|Layered Architecture|Network Model|
|---|---|
|General design approach|Specific conceptual model|
|Divides communication into layers|Defines the particular layers|
|Explains how layers interact|Describes functions/protocols associated with layers|
|Used as a design principle|Examples: OSI, TCP/IP|

### Example

**Layered architecture** is the concept.

**OSI and TCP/IP** are models that implement the idea of layering.

---

# 10. OSI as a Layered Architecture

The OSI model uses **7 layers**:

```text
7 ─ Application
6 ─ Presentation
5 ─ Session
4 ─ Transport
3 ─ Network
2 ─ Data Link
1 ─ Physical
```

Each layer has a specific responsibility and provides services to the layer above it.

---

# ⭐ Exam Answer — Short Version

If the question is **"What is layered architecture?"**, write:

> **Layered architecture is a network design approach in which the communication process is divided into a number of independent layers. Each layer performs a specific function, provides services to the layer above it, and uses services of the layer below it. This approach reduces complexity, provides modularity and standardization, makes troubleshooting easier, and allows different systems to communicate efficiently. OSI and TCP/IP are examples of network models based on layered architecture.**

### Remember:

```text
Layered Architecture
        ↓
Divide complex communication
        ↓
      Layers
        ↓
Each layer has a specific function
        ↓
Provides service ↑
Uses service    ↓
        ↓
Simpler + Modular + Standard + Easier to troubleshoot
```


---


# Switching in Computer Networks — Detailed Exam Notes

## 1. What is Switching?

**Switching** is the technique used in computer networks to **forward data from a source device to a destination device through one or more intermediate network devices**.

When the source and destination are not directly connected, the data must travel through intermediate devices. The process of selecting and using a path for transferring data is called **switching**.

### Simple example

```text
Computer A
    |
    |
  Switch
    |
    |
Computer B
```

In a larger network:

```text
A ─── S1 ─── S2 ─── S3 ─── B
      ↑       ↑       ↑
   Switching devices
```

---

# 2. Why is Switching Needed?

Imagine 100 computers communicating with each other.

If every computer had a direct connection to every other computer:

```text
A ───── B
|\     /|
| \   / |
|  \ /  |
|  / \  |
| /   \ |
|/     \|
C ───── D
```

The number of connections would become very large.

Switching solves this problem by allowing devices to **share communication paths and forward data when required**.

### Main purposes

- Efficient use of network resources
    
- Connecting multiple devices
    
- Forwarding data toward its destination
    
- Reducing the need for dedicated physical connections
    
- Supporting communication between distant devices
    

---

# 3. Types of Switching

The three major types traditionally studied in computer networks are:

```text
                    Switching
                       |
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
   Circuit          Message        Packet
   Switching        Switching      Switching
                                      |
                               ┌──────┴──────┐
                               ↓             ↓
                           Datagram      Virtual
                           Network        Circuit
```

### Main types:

1. **Circuit Switching**
    
2. **Message Switching**
    
3. **Packet Switching**
    

---

# 4. Circuit Switching

## Definition

**Circuit switching** is a switching technique in which a **dedicated communication path (circuit) is established between the sender and receiver before data transmission begins**.

The same path is generally used throughout the communication session.

### Simple diagram

```text
Sender A                                      Receiver B

   |                                               |
   |                                               |
   +---- Switch 1 ---- Switch 2 ---- Switch 3 ----+
                    Dedicated Path
```

Once the circuit is established:

```text
A → S1 → S2 → S3 → B
```

Data follows this established path.

---

# 5. Three Phases of Circuit Switching

Circuit switching generally involves **three phases**.

```text
┌─────────────────┐
│ 1. Setup Phase  │
└────────┬────────┘
         ↓
┌─────────────────┐
│ 2. Data Transfer│
└────────┬────────┘
         ↓
┌─────────────────┐
│ 3. Teardown     │
└─────────────────┘
```

## Phase 1 — Circuit Establishment

A dedicated path is established between the source and destination.

```text
A → S1 → S2 → S3 → B
      Dedicated Circuit
```

If a suitable path cannot be established, communication cannot begin.

---

## Phase 2 — Data Transfer

Once the circuit is established, data is transmitted through the same path.

```text
A
 ↓
S1
 ↓
S2
 ↓
S3
 ↓
B
```

The path remains reserved for that communication.

---

## Phase 3 — Circuit Teardown

After communication is completed, the dedicated circuit is released.

The network resources become available for other communications.

---

# 6. Example of Circuit Switching

Traditional **telephone networks** are the classic example.

When you make a traditional circuit-switched telephone call:

```text
Caller
  ↓
Telephone Exchange
  ↓
Exchange
  ↓
Receiver
```

A communication circuit is established before the conversation takes place.

> Modern telephone systems increasingly use packet-based technologies, so traditional circuit-switched telephony is mainly a textbook/classical example.

---

# 7. Advantages of Circuit Switching

### 1. Dedicated path

A dedicated path is available throughout the session.

### 2. Predictable performance

Once established, transmission has relatively predictable characteristics.

### 3. Constant bandwidth

The allocated resources remain available during the connection.

### 4. Suitable for continuous communication

It was well suited to traditional voice communication.

### 5. No store-and-forward delay for each packet

After setup, data can flow continuously through the established circuit.

---

# 8. Disadvantages of Circuit Switching

### 1. Wastage of resources

If no data is being sent, the reserved resources may remain unused.

```text
Reserved circuit:

A ─── S1 ─── S2 ─── B

       No data
         ↓
Resources still reserved
```

### 2. Setup time

Communication cannot begin until the circuit is established.

### 3. Failure of circuit

If the established path fails, communication may be interrupted.

### 4. Expensive

Dedicated resources can make circuit switching inefficient for bursty computer data.

### 5. Poor resource utilization

The channel cannot be efficiently shared among many bursty communications.

---

# 9. Message Switching

## Definition

**Message switching** is a technique in which the **entire message is treated as one unit and stored at an intermediate device before being forwarded to the next device**.

There is **no dedicated path** between source and destination.

It is called **store-and-forward switching**.

### Diagram

```text
A
|
↓
S1
|   Store entire message
↓
S2
|   Store entire message
↓
S3
|   Store entire message
↓
B
```

The message travels from one node to another.

---

# 10. How Message Switching Works

Suppose A wants to send a message to B.

```text
A → S1 → S2 → S3 → B
```

### Step 1

A sends the complete message to S1.

### Step 2

S1 stores the complete message.

### Step 3

S1 forwards it to S2.

### Step 4

S2 stores the complete message and forwards it to S3.

### Step 5

S3 forwards it to B.

This is:

> **Store → Forward → Store → Forward**

---

# 11. Example of Message Switching

Historically, **telegraph networks** are commonly used as examples.

Message switching was also associated with early store-and-forward communication systems.

---

# 12. Advantages of Message Switching

### 1. No dedicated path

Resources are not reserved for one communication.

### 2. Better resource utilization

Network links can be shared among different messages.

### 3. Routing flexibility

A message can potentially be routed through different intermediate nodes.

### 4. Priority can be supported

Messages can be given different priorities in some systems.

---

# 13. Disadvantages of Message Switching

### 1. Large storage requirement

The intermediate device must store the **entire message**.

### 2. High delay

A message may have to wait at every intermediate node.

### 3. Not suitable for real-time communication

Large delays make it unsuitable for applications requiring immediate communication.

### 4. Large buffer requirement

Intermediate devices need sufficient storage capacity.

---

# 14. Packet Switching

## Definition

**Packet switching** is a switching technique in which a message is **divided into smaller units called packets**, and these packets are transmitted through the network.

It is the fundamental switching technique used by modern computer networks and the Internet.

### Diagram

```text
Original Message
       |
       ↓
 ┌────┬────┬────┬────┐
 │ P1 │ P2 │ P3 │ P4 │
 └────┴────┴────┴────┘
       |
       ↓
     Network
       |
       ↓
 ┌────┬────┬────┬────┐
 │ P1 │ P2 │ P3 │ P4 │
 └────┴────┴────┴────┘
       |
       ↓
Original Message
```

---

# 15. How Packet Switching Works

Suppose the sender has a large message:

```text
MESSAGE
```

It is divided into packets:

```text
P1   P2   P3   P4
```

Each packet contains information such as:

- Source address
    
- Destination address
    
- Sequence/identification information
    
- Data
    
- Control information
    

Packets are forwarded through the network.

At the destination, packets are reassembled to recover the original data.

---

# 16. Types of Packet Switching

There are two major types:

1. **Datagram Packet Switching**
    
2. **Virtual Circuit Packet Switching**
    

---

# 17. Datagram Packet Switching

In **datagram packet switching**, each packet is treated **independently**.

Packets from the same message may take **different routes**.

### Example

```text
             ┌── S2 ──┐
A ── S1 ─────┤         ├── S5 ── B
             └── S3 ──┘
```

Suppose:

```text
P1 → S2 → S5 → B
P2 → S3 → S4 → B
P3 → S2 → S4 → B
```

The packets may arrive:

```text
P2 → P1 → P3
```

instead of:

```text
P1 → P2 → P3
```

The destination uses sequence information to reconstruct the data correctly when required.

### Characteristics

- No dedicated path.
    
- Each packet is routed independently.
    
- Packets can take different routes.
    
- Packets may arrive out of order.
    
- Packets may be delayed or lost.
    

### Example

**IP networks / Internet Protocol** use datagram-style packet forwarding.

---

# 18. Virtual Circuit Packet Switching

In **virtual circuit packet switching**, a logical path is established before packets are transmitted.

However, unlike circuit switching, the physical resources are not necessarily dedicated exclusively to one communication.

### Diagram

```text
A ─── S1 ─── S2 ─── S3 ─── B
      <---- Virtual Circuit ---->
```

Once the virtual circuit is established:

```text
P1 → S1 → S2 → S3 → B
P2 → S1 → S2 → S3 → B
P3 → S1 → S2 → S3 → B
```

Packets generally follow the same logical path.

### Characteristics

- Logical path is established.
    
- Packets generally follow the same route.
    
- Packets usually arrive in order.
    
- Network resources are shared.
    
- Requires setup and teardown.
    

### Examples

- Frame Relay
    
- ATM
    
- X.25
    

---

# 19. Circuit Switching vs Packet Switching

|Feature|Circuit Switching|Packet Switching|
|---|---|---|
|Path|Dedicated|Shared|
|Setup|Required|Depends on type|
|Data|Continuous stream|Packets|
|Resource usage|Reserved|Shared|
|Efficiency|Lower for bursty data|Generally higher|
|Delay|Setup delay, then predictable|Variable|
|Failure|Circuit failure can disrupt session|Packets may be rerouted depending on network|
|Suitable for|Traditional voice|Computer/data networks|
|Example|Traditional telephone network|Internet|

---

# 20. Message Switching vs Packet Switching

|Feature|Message Switching|Packet Switching|
|---|---|---|
|Data unit|Complete message|Packet|
|Division|Message is not divided|Message is divided|
|Storage|Entire message stored|Packets stored/forwarded|
|Delay|High|Generally lower|
|Buffer requirement|Large|Smaller|
|Real-time communication|Not suitable|Better suited|
|Network usage|Store-and-forward|Packet-based forwarding|

---

# 21. Three Switching Techniques — Important Table

|Feature|Circuit|Message|Packet|
|---|---|---|---|
|Dedicated path|Yes|No|No|
|Data unit|Continuous stream|Entire message|Packet|
|Setup|Yes|No|Depends on type|
|Store-and-forward|No, after setup|Yes|Yes|
|Resource utilization|Lower|Better|High|
|Delay|Setup + transmission|High|Generally lower|
|Suitable for|Traditional voice|Non-real-time messages|Internet/data|
|Example|Traditional telephone|Telegraph|Internet|

---

# 22. Circuit vs Message vs Packet — Diagram

```text
                 SWITCHING
                     |
       ┌─────────────┼─────────────┐
       ↓             ↓             ↓
   CIRCUIT         MESSAGE       PACKET
   SWITCHING       SWITCHING     SWITCHING
       |             |             |
 Dedicated        Entire         Data divided
   path           message        into packets
       |             |             |
       ↓             ↓             ↓
   Setup          Store &       Store &
   required       Forward       Forward
                                   |
                         ┌─────────┴─────────┐
                         ↓                   ↓
                     Datagram          Virtual Circuit
                     Independent        Logical path
                      packets            established
```

---

# 23. Switching and Multiplexing — Difference

These two terms are sometimes confused.

### Switching

Determines **where data should be forwarded**.

### Multiplexing

Allows **multiple signals/data streams to share one communication channel**.

Simple idea:

```text
Multiple Sources
      ↓
  Multiplexer
      ↓
 Single Channel
      ↓
  Demultiplexer
      ↓
Multiple Destinations
```

---

# ⭐ 24. Important Exam Definitions

### Switching

> **Switching is the technique of forwarding data from a source to a destination through intermediate network devices by selecting an appropriate communication path.**

### Circuit Switching

> **Circuit switching establishes a dedicated communication path between the sender and receiver before data transmission.**

### Message Switching

> **Message switching transmits the entire message using a store-and-forward technique without establishing a dedicated path.**

### Packet Switching

> **Packet switching divides a message into smaller packets and forwards them through a shared network toward the destination.**

---

# ⭐ 25. Exam Memory Trick

Remember:

### Circuit = **Dedicated Path**

```text
A ───────────── B
   Fixed path
```

### Message = **Whole Message**

```text
MESSAGE
   ↓
Store → Forward → Store → Forward
```

### Packet = **Small Pieces**

```text
MESSAGE
 ↓
P1 P2 P3 P4
 ↓
Network
 ↓
P1 P2 P3 P4
```

And for packet switching:

> **Datagram = Every packet thinks independently**

> **Virtual Circuit = Packets follow an established logical path**

---

# Routing in Computer Networks — Detailed Exam Notes

## 1. What is Routing?

**Routing** is the process of **selecting the best path for data packets to travel from a source network/device to a destination network/device**.

Routing mainly happens at the **Network Layer (Layer 3) of the OSI model**.

### Simple example

```text
Source
  |
  ↓
Router A
  |
  ├──────── Router B ────────┐
  |                          |
  └──────── Router C ────────┤
                             ↓
                         Destination
```

The router decides whether the packet should go through **Router B or Router C** based on available routing information.

### Exam definition

> **Routing is the process of determining and selecting an appropriate path for forwarding data packets from a source to a destination across one or more interconnected networks.**

---

# 2. Why is Routing Needed?

In a small network, devices may communicate directly.

```text
A ───────── B
```

But in a large network:

```text
A ─ Router 1 ─ Router 2 ─ Router 3 ─ B
```

The source cannot know everything about the entire network.

Routers help determine:

- Where the destination is.
    
- Which next router should receive the packet.
    
- Which available path should be used.
    
- How packets should be forwarded.
    

---

# 3. Routing vs Forwarding

These two terms are very important and are often confused.

### Routing

**Routing = deciding the path.**

It determines how packets should reach their destination.

### Forwarding

**Forwarding = sending the packet to the next hop.**

Example:

```text
Routing:
"To reach B, use Router 2."

Forwarding:
"Send this packet to Router 2."
```

### Simple comparison

|Routing|Forwarding|
|---|---|
|Determines the path|Moves packet to next hop|
|Network-wide process|Local router operation|
|Builds/uses routing information|Uses forwarding table|
|"Where should it go?"|"Send it there now."|

---

# 4. Router

A **router** is a networking device that connects different networks and forwards packets between them based on destination addressing and routing information.

### Basic diagram

```text
Network A
   |
   |
 Router
   |
   |
Network B
```

A router has interfaces connected to different networks.

```text
          Router
       ┌──────────┐
LAN A ─┤          ├─ LAN B
       │          │
WAN  ──┤          ├─ LAN C
       └──────────┘
```

---

# 5. Routing Table

A router uses a **routing table** to determine where packets should be forwarded.

A simplified routing table might look like:

```text
Destination       Next Hop       Interface
------------------------------------------------
192.168.1.0/24    Direct         eth0
10.0.0.0/8        192.168.1.2    eth1
172.16.0.0/16     192.168.1.3    eth2
Default           192.168.1.1    eth1
```

### Important fields

**Destination Network**

- Network the router wants to reach.
    

**Next Hop**

- The next router/device to which the packet should be sent.
    

**Interface**

- Router interface through which the packet should leave.
    

**Metric**

- A value used to compare possible routes.
    

---

# 6. How Routing Works

Suppose:

```text
Computer A
192.168.1.10

        |
        ↓

Router R1
        |
        ↓

Router R2
        |
        ↓

Computer B
192.168.3.10
```

Computer A sends a packet to `192.168.3.10`.

### Step 1 — Packet creation

The source creates a packet containing the destination IP address.

```text
Source IP:      192.168.1.10
Destination IP: 192.168.3.10
```

### Step 2 — Packet reaches router

Router R1 receives the packet.

### Step 3 — Router checks destination

R1 looks at:

```text
Destination = 192.168.3.10
```

### Step 4 — Routing table lookup

R1 checks its routing table to determine the best matching route.

### Step 5 — Forwarding

R1 forwards the packet to the appropriate next hop.

```text
A → R1 → R2 → B
```

### Step 6 — Destination receives packet

The packet eventually reaches Computer B.

---

# 7. Routing Metrics

A **routing metric** is a value used to determine how desirable one route is compared with another.

Common metrics include:

### 1. Hop Count

Number of routers that a packet must pass through.

```text
A → R1 → R2 → B

Hop count = 2
```

Lower hop count is generally preferred by protocols that use hop count as their metric.

---

### 2. Bandwidth

The capacity of a network link.

A route with higher available bandwidth may be preferred depending on the routing protocol.

---

### 3. Delay

Time required for data to travel through the network.

Lower delay is generally preferable.

---

### 4. Cost

A numerical value assigned to a link or route.

```text
Route A = Cost 10
Route B = Cost 25

Preferred → Route A
```

---

### 5. Reliability

Measures how dependable a communication link is.

A more reliable path may be preferred.

---

### 6. Load

Represents how heavily a network link is being used.

A less congested route may be preferred in systems that consider load.

---

# 8. Types of Routing

Routing can be classified in several ways.

The most important classification for exams is:

```text
                    ROUTING
                       |
          ┌────────────┴────────────┐
          ↓                         ↓
      Static                     Dynamic
      Routing                    Routing
                                   |
                       ┌───────────┼───────────┐
                       ↓           ↓           ↓
                     RIP          OSPF         BGP
```

Another important classification is:

```text
                    ROUTING
                       |
             ┌─────────┴─────────┐
             ↓                   ↓
          Interior             Exterior
          Routing               Routing
             |                     |
        Within AS             Between ASes
```

---

# 9. Static Routing

## Definition

**Static routing** is a routing method in which routes are **manually configured by a network administrator**.

The router does not automatically learn routes from other routers.

### Example

Administrator configures:

```text
Network: 192.168.3.0/24
Next Hop: 192.168.1.2
```

### Diagram

```text
A ─── R1 ─── R2 ─── B
      ↑
      |
Manually configured route
```

---

## Advantages of Static Routing

### 1. Simple for small networks

Easy to understand and configure when there are few routes.

### 2. No routing protocol overhead

Routers do not need to exchange routing updates.

### 3. More predictable

The administrator explicitly determines the route.

### 4. Can improve security

No routing protocol updates need to be exchanged with neighboring routers.

---

## Disadvantages

### 1. Manual configuration

Every route must be configured manually.

### 2. Poor scalability

Difficult to manage in large networks.

### 3. Does not automatically adapt

If a link fails, the administrator may need to modify the route.

### 4. Configuration errors

Incorrect configuration can cause routing problems.

---

# 10. Dynamic Routing

## Definition

**Dynamic routing** is a routing method in which routers **automatically learn and update routes using routing protocols**.

```text
        R1
       /  \
      /    \
     R2────R3
      \    /
       \  /
        R4
```

Routers exchange routing information and calculate routes.

---

## Advantages

### 1. Automatic updates

Routes can be updated when network conditions change.

### 2. Suitable for large networks

Can handle many routes.

### 3. Fault tolerance

Can potentially find alternative paths when a link fails.

### 4. Less manual work

Administrators don't have to manually configure every route.

---

## Disadvantages

- More complex.
    
- Uses CPU and memory.
    
- Routing protocols generate network traffic.
    
- Configuration and troubleshooting can be more difficult.
    

---

# 11. Static vs Dynamic Routing

|Feature|Static Routing|Dynamic Routing|
|---|---|---|
|Configuration|Manual|Automatic/learned|
|Updates|Manual|Automatic|
|Complexity|Low for small networks|Higher|
|Scalability|Poor|Good|
|Failure adaptation|Usually manual|Can adapt automatically|
|Resource usage|Low|Higher|
|Suitable for|Small/simple networks|Medium/large networks|

---

# 12. Routing Protocols

A **routing protocol** is a set of rules used by routers to **exchange routing information and determine routes**.

Important routing protocols include:

### RIP

**Routing Information Protocol**

- Distance-vector protocol.
    
- Uses **hop count** as its primary metric.
    
- Maximum usable hop count is **15**; 16 represents unreachable.
    
- Simple but limited for large networks.
    

---

### OSPF

**Open Shortest Path First**

- Link-state routing protocol.
    
- Uses a cost metric.
    
- Uses **Dijkstra's shortest-path algorithm**.
    
- Commonly used inside large enterprise networks.
    

---

### BGP

**Border Gateway Protocol**

- Used to exchange routing information **between Autonomous Systems**.
    
- It is the primary inter-domain routing protocol of the Internet.
    
- Path-vector protocol.
    

---

# 13. Classification of Routing Protocols

## A. Interior Gateway Protocols — IGP

Used for routing **within a single Autonomous System (AS)**.

Examples:

- RIP
    
- OSPF
    
- IS-IS
    
- EIGRP
    

```text
       Autonomous System
   ┌─────────────────────┐
   │ R1 ─ R2 ─ R3 ─ R4   │
   │                     │
   └─────────────────────┘
```

---

## B. Exterior Gateway Protocol — EGP

Used for routing **between Autonomous Systems**.

The main modern example is:

> **BGP**

```text
       AS 100                 AS 200
   ┌───────────┐          ┌───────────┐
   │ R1 ─ R2   │──────────│ R3 ─ R4   │
   └───────────┘   BGP    └───────────┘
```

---

# 14. Distance Vector Routing

In **distance-vector routing**, a router learns routes by exchanging information with neighboring routers.

Each router generally maintains information such as:

```text
Destination → Distance → Next Hop
```

Example:

```text
Destination    Distance    Next Hop
------------------------------------
Network A      2           R2
Network B      4           R3
```

### Main idea

> **"I can reach destination X at distance Y through neighbor Z."**

### Example

```text
A ─── R1 ─── R2 ─── R3 ─── B

R1 learns:
B → 3 hops via R2
```

### Example protocol

**RIP**

---

# 15. Link-State Routing

In **link-state routing**, routers build a more complete view of the network topology by exchanging information about their links.

Then each router calculates the shortest/best paths.

### Basic idea

```text
        R1
       /  \
      /    \
     R2────R3
      \    /
       \  /
        R4
```

Routers learn the topology and calculate paths.

### Algorithm

**Dijkstra's Shortest Path First (SPF)** algorithm is commonly associated with link-state routing.

### Example protocol

**OSPF**

---

# 16. Distance Vector vs Link State

|Feature|Distance Vector|Link State|
|---|---|---|
|Knowledge|Information from neighbors|Broader topology information|
|Algorithm|Bellman-Ford family|Dijkstra SPF|
|Updates|Typically exchanged with neighbors|Link-state information is flooded within an area/domain|
|Convergence|Generally slower|Generally faster|
|Complexity|Simpler|More complex|
|Example|RIP|OSPF|

---

# 17. Path Vector Routing

**Path-vector routing** maintains information about the path or sequence of Autonomous Systems toward a destination.

The best-known example is:

> **BGP**

Example:

```text
AS 100 → AS 200 → AS 300 → Destination
```

BGP can use the AS path and other routing attributes to select routes.

### Exam definition

> **Path-vector routing is a routing technique in which routing information includes the path through Autonomous Systems to reach a destination.**

---

# 18. Default Routing

**Default routing** is used when a router does not have a more specific route for a destination.

A default route is commonly represented as:

```text
0.0.0.0/0
```

Example:

```text
LAN
 |
Router
 |
Default Route
 |
Internet
```

### Example

If the router doesn't know a specific route:

```text
Destination = Unknown
       ↓
Use Default Route
       ↓
Internet Gateway
```

### Exam definition

> **Default routing provides a route to use when no more specific route to the destination exists in the routing table.**

---

# 19. Unicast, Broadcast, Multicast and Anycast Routing

Routing can also be discussed based on how destinations are addressed.

## 1. Unicast

**One sender → One receiver**

```text
A ─────────→ B
```

Example:

- Normal web request to a particular server.
    

---

## 2. Broadcast

**One sender → All devices in a broadcast domain**

```text
          B
         ↑
         |
A ───────┼────── C
         |
         ↓
         D
```

---

## 3. Multicast

**One sender → Selected group of receivers**

```text
          B
         ↑
         |
A ───────┼────── C
         
         D
      (not selected)
```

---

## 4. Anycast

**One sender → One suitable/nearest member of a group**

```text
             Server B
            /
Client ────┼──── Server C
            \
             Server D

      → One suitable server selected
```

Anycast is widely used in distributed Internet services.

---

# 20. Routing Algorithms

A **routing algorithm** is a procedure used to determine the best route between source and destination.

Important concepts include:

### 1. Shortest Path Algorithm

Finds a path with the lowest cost according to the chosen metric.

Example:

```text
A ──2── B ──3── D
 \             /
  \4           /1
   \           /
      C ─────
```

Possible paths:

```text
A → B → D = 2 + 3 = 5
```

The algorithm compares possible routes according to the routing metric.

---

### 2. Dijkstra's Algorithm

Used in **link-state routing**.

It calculates shortest paths from one source to all reachable destinations.

Used conceptually by protocols such as:

> **OSPF**

---

### 3. Bellman-Ford Algorithm

Used as the basis for **distance-vector routing**.

It calculates routes based on information exchanged with neighboring routers.

Associated with:

> **RIP**

---

# 21. Routing Process — Complete Diagram

```text
             SOURCE
                |
                ↓
        +---------------+
        |    Router     |
        +---------------+
                |
                ↓
       Check Destination IP
                |
                ↓
        Search Routing Table
                |
                ↓
        Select Best Route
                |
                ↓
           Next Hop
                |
                ↓
        +---------------+
        | Next Router   |
        +---------------+
                |
                ↓
             Repeat
                |
                ↓
          DESTINATION
```

---

# 22. Routing Loops

A **routing loop** occurs when packets continuously circulate between routers instead of reaching their destination.

Example:

```text
       ┌──── R2 ────┐
       ↓            |
      R1             R3
       ↑            |
       └────────────┘

Packet:
R1 → R2 → R3 → R1 → R2 → ...
```

### Problems caused

- Wastes bandwidth.
    
- Increases network congestion.
    
- Packets may never reach the destination.
    
- Can cause excessive resource consumption.
    

Routing protocols use mechanisms to prevent or limit loops.

---

# 23. Routing Metrics vs Routing Protocols

Don't confuse these.

### Routing metric

A **value used to evaluate a route**.

Examples:

- Hop count
    
- Cost
    
- Delay
    
- Bandwidth
    

### Routing protocol

A **set of rules used to exchange routing information and calculate/select routes**.

Examples:

- RIP
    
- OSPF
    
- BGP
    

```text
Routing Protocol
       ↓
Collects/uses routing information
       ↓
Calculates/selects route
       ↓
Uses routing metric/attributes
```

---

# 24. Important Routing Terms

|Term|Meaning|
|---|---|
|**Router**|Device that forwards packets between networks|
|**Route**|Path toward a destination|
|**Routing Table**|Information used to determine where packets should go|
|**Next Hop**|Next router/device on the path|
|**Metric**|Value used to compare routes|
|**Routing Protocol**|Rules for exchanging routing information|
|**Static Routing**|Manually configured routes|
|**Dynamic Routing**|Routes learned/updated automatically|
|**Default Route**|Route used when no more specific route matches|
|**Hop**|A step from one router to the next|
|**Autonomous System**|Network/domain under a common routing administration|

---

# ⭐ 25. Static, Dynamic, Distance Vector, Link State — Don't Confuse Them

These classifications describe **different aspects**.

```text
                    ROUTING
                       |
          ┌────────────┴────────────┐
          ↓                         ↓
       Static                    Dynamic
       Manual                    Automatic
                                  |
                    ┌─────────────┼─────────────┐
                    ↓             ↓             ↓
              Distance Vector  Link State   Path Vector
                    ↓             ↓             ↓
                   RIP           OSPF          BGP
```

This distinction is **very important for exams**.

---

# ⭐ 26. Most Important Exam Comparison

## Distance Vector vs Link State vs Path Vector

|Feature|Distance Vector|Link State|Path Vector|
|---|---|---|---|
|Basic idea|Distance via neighbors|Network topology|Path through ASes|
|Information|Distance + direction|Link-state/topology information|AS path + attributes|
|Example|RIP|OSPF|BGP|
|Algorithm/approach|Bellman-Ford family|Dijkstra SPF|Path-vector|
|Typical use|Small/simple networks|Internal networks|Inter-AS Internet routing|

---

# ⭐ 27. One-Page Revision

```text
ROUTING
   │
   ├── Purpose
   │     └── Select best path + forward packets
   │
   ├── Main Device
   │     └── Router
   │
   ├── Routing Table
   │     ├── Destination
   │     ├── Next Hop
   │     ├── Interface
   │     └── Metric
   │
   ├── Configuration
   │     ├── Static
   │     └── Dynamic
   │
   ├── Dynamic Routing Approaches
   │     ├── Distance Vector → RIP
   │     ├── Link State → OSPF
   │     └── Path Vector → BGP
   │
   ├── Scope
   │     ├── IGP → Within AS
   │     └── BGP → Between ASes
   │
   └── Routing Types by Delivery
         ├── Unicast
         ├── Broadcast
         ├── Multicast
         └── Anycast
```


---


# Network Devices — Detailed Exam Notes

## 1. What are Network Devices?

**Network devices** are hardware components used to **connect computers and other devices, transmit data, control network traffic, and enable communication between different networks**.

Examples include:

- Repeater
    
- Hub
    
- Bridge
    
- Switch
    
- Router
    
- Gateway
    
- Modem
    
- Wireless Access Point
    
- NIC
    

---

# 2. Classification of Network Devices

A useful exam-oriented classification is:

```text
                    NETWORK DEVICES
                          │
        ┌─────────────────┴─────────────────┐
        ↓                                   ↓
  Connecting/Forwarding               Access/Communication
        │                                   │
   ┌────┼────┬────┬────┐              ┌────┼────┐
   ↓    ↓    ↓    ↓    ↓              ↓    ↓    ↓
Repeater Hub Bridge Switch Router    NIC  Modem  AP
                                     
                         ↓
                      Gateway
```

---

# 3. Repeater

## Definition

A **repeater** is a network device that **receives a weak or degraded signal, regenerates/reshapes it, and retransmits it** to extend the communication distance.

It operates at the **Physical Layer (Layer 1)** of the OSI model.

### Diagram

```text
Computer A
    |
    | Weak signal
    ↓
+-----------+
| Repeater  |
+-----------+
    |
    | Regenerated signal
    ↓
Computer B
```

### Why is it needed?

Signals can become weaker as they travel over a communication medium. This phenomenon is called **attenuation**.

A repeater helps extend the transmission distance by regenerating the signal.

### Functions

- Receives signal.
    
- Regenerates signal.
    
- Retransmits signal.
    
- Extends network distance.
    

### Advantages

- Simple device.
    
- Extends transmission distance.
    
- Helps overcome signal attenuation.
    
- Relatively inexpensive.
    

### Disadvantages

- Does not filter traffic.
    
- Does not understand destination addresses.
    
- Does not reduce network traffic.
    
- Operates only at the Physical Layer.
    

### Exam definition

> **A repeater is a Layer 1 device that regenerates and retransmits signals to extend the transmission distance of a network.**

---

# 4. Hub

## Definition

A **hub** is a basic networking device used to connect multiple devices in a LAN. When it receives a signal/frame from one port, it **repeats the signal out through its other ports**.

It operates at the **Physical Layer (Layer 1)**.

### Diagram

```text
             PC1
              |
              |
         +----------+
PC2 -----|   HUB    |----- PC3
         +----------+
              |
             PC4
```

If PC1 sends data to PC3:

```text
PC1
 ↓
HUB
 ↓ ↓ ↓
PC2 PC3 PC4
```

The hub does not know which port leads to PC3, so it repeats the signal to the other ports.

### Types of Hub

#### 1. Active Hub

- Receives and regenerates signals.
    
- Requires power.
    

#### 2. Passive Hub

- Mainly acts as a connection point.
    
- Does not regenerate signals.
    

#### 3. Intelligent Hub

- Provides additional monitoring/management features compared with basic hubs.
    

### Advantages

- Simple.
    
- Easy to install.
    
- Relatively inexpensive.
    
- Can connect multiple devices.
    

### Disadvantages

- Sends traffic to multiple ports.
    
- Creates one shared collision domain.
    
- Less efficient than switches.
    
- Offers little traffic control.
    
- Generally operates in half-duplex Ethernet environments.
    

### Exam definition

> **A hub is a Layer 1 networking device that connects multiple devices and repeats incoming signals to multiple ports.**

---

# 5. Bridge

## Definition

A **bridge** is a networking device that connects **two or more LAN segments** and forwards or filters frames using **MAC addresses**.

It operates at the **Data Link Layer (Layer 2)**.

### Diagram

```text
 LAN 1                         LAN 2

PC1 ── PC2 ──┐             ┌── PC4 ── PC5
             │             │
             └── Bridge ───┘
```

### How does a bridge work?

A bridge examines the **MAC address** of an Ethernet frame and decides whether the frame should be:

- Forwarded
    
- Filtered
    

### Functions

- Connects LAN segments.
    
- Uses MAC addresses.
    
- Filters frames.
    
- Reduces unnecessary traffic between segments.
    
- Separates collision domains.
    

### Advantages

- Better than a hub.
    
- Filters unnecessary traffic.
    
- Reduces collisions.
    
- Connects different LAN segments.
    

### Disadvantages

- More expensive/complex than a hub.
    
- Limited scalability compared with modern switches.
    
- Primarily a Layer 2 device.
    

### Exam definition

> **A bridge is a Layer 2 device that connects LAN segments and forwards or filters frames based on MAC addresses.**

---

# 6. Switch

## Definition

A **network switch** is a multi-port Layer 2 device that connects devices in a LAN and **forwards Ethernet frames to the appropriate port based on MAC addresses**.

Modern switches may also perform **Layer 3 routing** if they are Layer 3 switches.

### Diagram

```text
             PC1
              |
              |
         +----------+
PC2 -----|  SWITCH  |----- PC3
         +----------+
              |
             PC4
```

Suppose PC1 wants to send data to PC3:

```text
PC1
 ↓
Switch
 ↓
PC3
```

Unlike a basic hub, the switch normally sends the frame only through the port associated with the destination MAC address.

---

## How does a switch learn MAC addresses?

A switch maintains a **MAC address table**.

Example:

|MAC Address|Port|
|---|---|
|AA:AA:AA:AA:AA:AA|Port 1|
|BB:BB:BB:BB:BB:BB|Port 2|
|CC:CC:CC:CC:CC:CC|Port 3|

When a frame arrives, the switch can:

1. Learn the **source MAC address** and associate it with the incoming port.
    
2. Look up the **destination MAC address**.
    
3. Forward the frame to the appropriate port if known.
    
4. If the destination is unknown, flood the frame within the relevant VLAN/broadcast domain.
    

### Types of Switches

#### 1. Unmanaged Switch

- Plug-and-play.
    
- Little/no configuration.
    
- Suitable for simple networks.
    

#### 2. Managed Switch

- Can be configured and monitored.
    
- Supports features such as VLANs, STP, and management interfaces.
    

#### 3. Layer 3 Switch

- Performs switching and can also perform IP routing.
    

### Advantages

- Efficient forwarding.
    
- Reduces unnecessary traffic compared with hubs.
    
- Each switch port is generally its own collision domain.
    
- Supports full-duplex communication.
    
- Can provide VLANs and other management features.
    

### Disadvantages

- More expensive than a basic hub.
    
- Configuration can be complex.
    
- Broadcasts are still propagated within a VLAN unless controlled by routing or other mechanisms.
    

### Exam definition

> **A switch is a Layer 2 device that connects devices in a LAN and forwards frames using MAC addresses.**

---

# 7. Router

## Definition

A **router** is a networking device that **connects different IP networks and forwards packets between them using IP addresses and routing information**.

It primarily operates at the **Network Layer (Layer 3)**.

### Diagram

```text
     LAN A
       |
       |
   +--------+
   | Router |
   +--------+
       |
       |
     LAN B
```

A router can connect:

```text
Home Network ── Router ── Internet
```

### Main functions

#### 1. Routing

Determines the appropriate path for packets.

#### 2. Packet forwarding

Forwards packets toward their destination.

#### 3. Inter-network communication

Connects different networks.

#### 4. Logical addressing

Uses IP addresses.

#### 5. Broadcast domain separation

Routers separate broadcast domains.

### Routing table

A router maintains routing information such as:

```text
Destination       Next Hop
--------------------------------
192.168.1.0/24    Direct
10.0.0.0/8        192.168.1.1
0.0.0.0/0         ISP Gateway
```

### Advantages

- Connects different networks.
    
- Determines routes.
    
- Controls packet forwarding.
    
- Separates broadcast domains.
    
- Can provide security-related functions such as ACLs.
    

### Disadvantages

- More complex than switches/hubs.
    
- Usually more expensive.
    
- Packet processing can introduce latency.
    

### Exam definition

> **A router is a Layer 3 device that connects different networks and forwards packets based on logical IP addresses and routing information.**

---

# 8. Gateway

## Definition

A **gateway** is a device or software component that acts as an **entry/exit point between networks and can perform protocol or architectural translation when required**.

The term "gateway" is broad and is used in different contexts.

### Diagram

```text
Network A
    |
    |
+----------+
| Gateway  |
+----------+
    |
    |
Network B
```

### Example

A network may use a gateway to communicate with an external network that uses a different protocol or architecture.

### Functions

- Connects different networks.
    
- Can translate between protocols/formats.
    
- Acts as an entry/exit point.
    
- Can perform protocol conversion.
    

### Important exam point

You may see textbooks say:

> **Gateway operates at all layers of the OSI model.**

This is an oversimplification. A gateway is **not limited to one specific OSI layer**; its exact function depends on what kind of gateway is being discussed.

### Exam definition

> **A gateway is a device or software component that connects dissimilar networks and, when necessary, performs protocol or format translation between them.**

---

# 9. Modem

## Definition

**Modem** stands for:

> **MO**dulator + **DE**Modulator

A modem converts digital data into signals suitable for a particular transmission medium and converts received signals back into digital data.

### Basic concept

```text
Digital Data
     ↓
  Modulation
     ↓
Suitable Signal
     ↓
Communication Medium
     ↓
  Demodulation
     ↓
Digital Data
```

### Diagram

```text
Computer
   |
Digital Data
   ↓
+--------+
| Modem  |
+--------+
   |
Communication Line
   |
+--------+
| Modem  |
+--------+
   |
   ↓
Computer/Network
```

### Functions

- Modulation.
    
- Demodulation.
    
- Provides connectivity over certain communication services.
    

### Types

Depending on technology:

- DSL modem
    
- Cable modem
    
- Cellular modem
    
- Dial-up modem
    

### Advantages

- Enables digital devices to communicate over compatible communication services.
    
- Converts signals between required forms.
    

### Disadvantages

- Performance depends on the underlying access technology.
    
- Some traditional modem technologies have limited speed.
    

### Exam definition

> **A modem is a device that performs modulation and demodulation to enable data communication over a suitable transmission medium.**

---

# 10. Wireless Access Point (WAP/AP)

## Definition

A **Wireless Access Point (AP)** is a networking device that provides **wireless network access to Wi-Fi devices** and connects them to a wired LAN or other network infrastructure.

### Diagram

```text
             Laptop
                )))
               )))
              )))
        +---------------+
        | Wireless AP   |
        +---------------+
                |
             Ethernet
                |
             Switch
                |
             Network
```

### Functions

- Provides Wi-Fi connectivity.
    
- Connects wireless clients to a wired network.
    
- Manages wireless communication within its coverage area.
    
- Can provide wireless security/authentication features.
    

### Examples of connected devices

- Smartphones
    
- Laptops
    
- Tablets
    
- IoT devices
    

### Exam definition

> **A wireless access point is a device that allows wireless devices to connect to a network, typically using Wi-Fi.**

---

# 11. NIC — Network Interface Card

## Definition

A **Network Interface Card (NIC)** is a hardware component that provides a device with a **network interface for communicating over a network**.

NICs can be:

- Wired
    
- Wireless
    

### Diagram

```text
Computer
   |
   ↓
+---------+
|   NIC   |
+---------+
   |
   ↓
Network
```

### Main functions

- Provides network connectivity.
    
- Sends and receives network data.
    
- Uses a MAC address for Layer 2 communication.
    
- Converts data between the computer's internal representation and network signaling.
    

### Types

#### Ethernet NIC

Uses wired Ethernet.

```text
PC → Ethernet NIC → Ethernet Cable → Switch
```

#### Wireless NIC

Uses Wi-Fi.

```text
Laptop → Wireless NIC ))) Access Point
```

### Exam definition

> **A NIC is a hardware interface that enables a computer or other device to connect and communicate with a network.**

---

# 12. Firewall

## Definition

A **firewall** is a security system that **monitors and controls network traffic according to defined security rules**.

It can be implemented as hardware, software, or a combination.

### Diagram

```text
Internet
   |
   ↓
+----------+
| Firewall |
+----------+
   |
   ↓
Internal Network
```

### Functions

- Filters traffic.
    
- Blocks unauthorized connections.
    
- Allows permitted traffic.
    
- Helps protect internal networks.
    
- Can enforce security policies.
    

### Types

- Packet-filtering firewall
    
- Stateful firewall
    
- Application/proxy firewall
    
- Next-generation firewall (NGFW)
    

### Exam definition

> **A firewall is a hardware or software security system that monitors and controls incoming and outgoing network traffic according to predefined security rules.**

---

# 13. Network Devices According to OSI Layer

This is **very important for exams**.

```text
OSI Layer                    Devices

Layer 7 ─ Application
Layer 6 ─ Presentation
Layer 5 ─ Session
Layer 4 ─ Transport

Layer 3 ─ Network          → Router

Layer 2 ─ Data Link        → Bridge
                             Switch
                             NIC*

Layer 1 ─ Physical         → Repeater
                             Hub
                             Cable
                             Modem*
                             NIC*
```

**Important:** Some devices span multiple layers depending on their implementation. For exam purposes, the common associations are:

|Device|Common OSI Layer|
|---|---|
|Repeater|Layer 1 — Physical|
|Hub|Layer 1 — Physical|
|Bridge|Layer 2 — Data Link|
|Switch|Layer 2 — Data Link|
|Router|Layer 3 — Network|
|Gateway|Depends on function|
|Modem|Depends on technology; traditionally associated with lower layers|
|Access Point|Commonly Layer 2|
|NIC|Primarily Layer 1/2 functions|
|Firewall|Depends on type; can operate at multiple layers|

---

# 14. Hub vs Switch vs Router

This is one of the **most important exam comparisons**.

|Feature|Hub|Switch|Router|
|---|---|---|---|
|Main OSI layer|Layer 1|Layer 2|Layer 3|
|Address used|None|MAC|IP|
|Connects|Devices in LAN|Devices in LAN|Different networks|
|Forwarding|Repeats to ports|Based on MAC|Based on IP/routing table|
|Collision domains|One shared collision domain|Separate per port|Separate|
|Broadcast domains|One|Usually one per VLAN|Separates broadcast domains|
|Intelligence|Very low|Higher|Higher|
|Example|Old Ethernet hub|LAN switch|Home/enterprise router|

---

# 15. Repeater vs Hub vs Bridge vs Switch vs Router

|Device|Main Purpose|Address|Layer|
|---|---|---|---|
|**Repeater**|Regenerate signal|None|L1|
|**Hub**|Repeat signal to multiple ports|None|L1|
|**Bridge**|Connect/filter LAN segments|MAC|L2|
|**Switch**|Efficiently forward LAN frames|MAC|L2|
|**Router**|Connect networks and route packets|IP|L3|

### Easy memory

```text
Repeater → Regenerate
Hub      → Repeat
Bridge   → Connect LAN segments
Switch   → MAC-based forwarding
Router   → IP-based routing
```

---

# 16. Bridge vs Switch

A switch is essentially a **multi-port Layer 2 forwarding device** and can be viewed as an evolution of the traditional bridge concept.

|Bridge|Switch|
|---|---|
|Usually fewer ports|Usually many ports|
|Older technology/concept|Modern LAN technology|
|Layer 2|Primarily Layer 2|
|Uses MAC addresses|Uses MAC addresses|
|Connects LAN segments|Connects many LAN devices|

---

# 17. Access Point vs Router

These are also commonly confused.

### Access Point

Primarily provides **wireless connectivity** to a network.

```text
Phone )))
Laptop ))) → AP → Wired Network
```

### Router

Primarily connects **different IP networks** and performs routing.

```text
LAN → Router → Internet
```

A typical home "Wi-Fi router" often combines:

```text
Router
   +
Switch
   +
Wireless Access Point
   +
Other functions
```

---

# 18. Complete Network Devices Diagram

```text
                         INTERNET
                            |
                            |
                         ROUTER
                            |
                       +----+----+
                       |         |
                    SWITCH       AP
                    /  |  \       )))
                   /   |   \      )))
                 PC1  PC2  PC3   Laptop
                  |
                 NIC
                  |
               Network

          Older/other devices:

        Repeater → Extends signal
        Hub      → Repeats to ports
        Bridge   → Connects LAN segments
        Modem    → Modulates/Demodulates
        Gateway  → Connects/Translates networks
        Firewall → Filters traffic
```

---

# ⭐ 19. One-Line Definitions for Revision

|Device|One-line definition|
|---|---|
|**Repeater**|Regenerates and retransmits signals to extend network distance.|
|**Hub**|Repeats incoming signals to multiple ports.|
|**Bridge**|Connects LAN segments and filters frames using MAC addresses.|
|**Switch**|Forwards LAN frames to appropriate ports using MAC addresses.|
|**Router**|Connects different networks and forwards packets using IP/routing information.|
|**Gateway**|Connects networks and may translate between protocols or architectures.|
|**Modem**|Performs modulation and demodulation for communication over a suitable medium.|
|**Access Point**|Provides wireless network connectivity to Wi-Fi devices.|
|**NIC**|Provides a device with an interface for network communication.|
|**Firewall**|Monitors and controls network traffic according to security rules.|

---

# 🎯 Most Important Devices for Exams

If you have limited time, study these **first**:

### Must know

1. **Repeater**
    
2. **Hub**
    
3. **Bridge**
    
4. **Switch**
    
5. **Router**
    
6. **Gateway**
    
7. **Modem**
    
8. **Wireless Access Point**
    
9. **NIC**
    
10. **Firewall**
    

And especially memorize:

> **Repeater/Hub → Layer 1 → Signal**  
> **Bridge/Switch → Layer 2 → MAC/Frame**  
> **Router → Layer 3 → IP/Packet**  
> **Gateway → Protocol/Network Translation**  
> **Firewall → Security/Traffic Filtering**

These layer associations, definitions, functions, diagrams, and comparisons are the **highest-value material for theory questions**.