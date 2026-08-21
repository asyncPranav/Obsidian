
---

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