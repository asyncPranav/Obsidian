
----


# UNIT 3: NETWORK LAYER

## Complete Detailed Notes — Computer Networks

**Syllabus covered**

1. Network Layer and Point-to-Point Networks
2. Routing
3. Congestion Control
4. Internetworking
5. TCP/IP Model
6. IP Packet
7. IP Addressing
8. IPv6

These notes follow the syllabus topic by topic, with diagrams, important comparisons, examples, and numerical concepts relevant to university examinations.

---

# 1. NETWORK LAYER

## 1.1 Definition

The **Network Layer is Layer 3 of the OSI model**. It is responsible for delivering packets from a source device to a destination across one or more interconnected networks.

It primarily uses **logical addressing, routing, and packet forwarding** to accomplish this task.

### Position in the OSI Model

```
┌────────────────────────────┐
│  Application Layer         │
├────────────────────────────┤
│  Presentation Layer        │
├────────────────────────────┤
│  Session Layer             │
├────────────────────────────┤
│  Transport Layer           │
├════════════════════════════┤
│  NETWORK LAYER             │ ← Layer 3
├────────────────────────────┤
│  Data Link Layer           │
├────────────────────────────┤
│  Physical Layer            │
└────────────────────────────┘
```

## 1.2 Functions of the Network Layer

|Function|Explanation|
|---|---|
|Logical addressing|Uses IP addresses to identify source and destination devices.|
|Routing|Determines suitable paths through interconnected networks.|
|Packet forwarding|Sends packets to the appropriate next hop.|
|Internetworking|Enables communication across different interconnected networks.|
|Fragmentation|In IPv4, packets may be divided to accommodate link MTU restrictions.|
|Congestion management|Supports mechanisms that help detect, prevent, or control network congestion.|

### Data unit

The Network Layer's data unit is called a **packet** or, in IP terminology, an **IP datagram**.

### Basic working

```
Source Application
        |
        v
   Transport Layer
        |
        | Segment
        v
   Network Layer
        |
        | Adds IP header
        v
    IP Packet
        |
        v
   Data Link Layer
        |
        | Frame
        v
   Physical Network
```

At the destination, the packet moves upward through the receiving device's networking layers.

**Important:** IP provides best-effort delivery. It does not, by itself, guarantee that packets will arrive, arrive in order, or arrive exactly once.

---

# 2. POINT-TO-POINT NETWORKS

## 2.1 Definition

A **point-to-point network link** is a communication link that connects exactly two endpoints.

The endpoints may be two computers, two routers, or a computer and a network device.

### Diagram

```
       Point-to-Point Link

    Router A ------------- Router B
                 Link
```

The link may use copper cable, fiber-optic cable, a wireless connection, or another suitable transmission medium.

Point-to-point links are frequently used to connect routers in larger networks.

## 2.2 Working

Suppose two routers are directly connected:

```
Network A                         Network B
    |                                 |
    v                                 v
+---------+     Direct Link      +---------+
| Router A| ==================== | Router B|
+---------+                      +---------+
```

1. Router A receives a packet that must reach Network B.
2. Router A determines that Router B is the appropriate next hop.
3. Router A sends the packet through the point-to-point link.
4. Router B receives the packet and processes its destination address.
5. Router B forwards the packet toward its destination.

A packet travelling across a large network may pass through several point-to-point links.

## 2.3 Characteristics

- Connects two endpoints.
- Does not require multiple stations to compete for the same link.
- Can provide predictable link capacity, depending on the technology and service.
- Makes it relatively easy to identify the two endpoints.
- Can be used between routers to build larger networks.

## 2.4 Advantages

**1. Simple communication:** Only two endpoints share the link.

**2. Predictable performance:** A dedicated link can provide predictable capacity and delay characteristics.

**3. Easier troubleshooting:** The two endpoints and their connecting link can be tested independently.

**4. Controlled access:** Multiple stations do not compete to transmit on the same point-to-point link.

## 2.5 Limitations

- Establishing a separate physical link for every pair of devices may be expensive.
- A link failure interrupts communication over that link until an alternative route or replacement link is used.
- A point-to-point link alone does not provide communication between every device in a large network.

### Point-to-point vs broadcast network

|Feature|Point-to-Point|Broadcast/Shared-Medium Network|
|---|---|---|
|Connection|Two endpoints per link|Multiple stations share a medium|
|Channel access competition|Usually absent between endpoints|May require medium-access coordination|
|Example|Router-to-router leased line|Traditional shared Ethernet|
|Data delivery|Sent to the other endpoint|Depending on the technology, transmissions may be visible to multiple stations|

### Important distinction

**Point-to-point describes the connection between two endpoints.** Routing determines how packets travel across multiple links to reach a destination.

In many textbooks, point-to-point connectivity is also discussed alongside two network-layer service models: datagram networks and virtual-circuit networks.

## 2.6 Datagram and Virtual-Circuit Networks

These models describe two different approaches to packet delivery.

### A. Datagram Network

In a datagram network, each packet is handled independently using its destination address.

```
              +---- Router B ----+
             /                    \
Source ---- Router A               Router D ---- Destination
             \                    /
              +---- Router C ----+
```

Different packets belonging to the same communication may take different routes.

**Characteristics:**

- No network-layer connection setup is required before sending each datagram.
- Routers use destination-address information to forward packets.
- Packets may take different paths.
- Packets can arrive out of order.

**Example:** IPv4 and IPv6 use datagram-based packet delivery.

### B. Virtual-Circuit Network

In a virtual-circuit network, a logical path is established before the data packets are transmitted.

```
Source → Router A → Router B → Router C → Destination
        <-------- Logical Path --------->
```

Packets associated with the virtual circuit generally follow the established logical path.

**Characteristics:**

- Requires connection or virtual-circuit setup.
- Intermediate devices maintain relevant connection state.
- Packets use virtual-circuit identifiers or labels.
- Packets generally follow the same logical path.

**Examples:** X.25, Frame Relay, and ATM are associated with virtual-circuit networking.

### Datagram vs virtual circuit

|Feature|Datagram Network|Virtual-Circuit Network|
|---|---|---|
|Setup|Not required at the network layer|Required|
|Routing|Each packet is forwarded independently|Packets follow an established logical circuit|
|Router state|No per-connection virtual-circuit state is required|Virtual-circuit state is maintained|
|Path|Packets may take different paths|Usually follows one established logical path|
|Example|IP networks|ATM and Frame Relay|

**Exam point:** A virtual circuit is a logical packet-switched path; it is not necessarily a physically dedicated circuit.

---

# 3. ROUTING

## 3.1 Definition

**Routing is the process of determining a suitable path through a network for delivering packets from a source to a destination.**

Routers use destination addresses and routing information to make routing decisions.

### Example

```
                 +------ Router B ------+
                /                        \
Source -- Router A                      Router D -- Destination
                \                        /
                 +------ Router C ------+
```

Router A must select a suitable next hop to move the packet toward its destination.

## 3.2 Routing and Forwarding

These terms describe related but distinct operations.

|Routing|Forwarding|
|---|---|
|Determines suitable paths|Transfers packets to the selected next hop|
|Uses routing information and algorithms|Uses the forwarding table|
|Can involve information from the wider network|Is performed locally by a router|

## 3.3 Routing Table

A **routing table** stores information used by a router to choose where packets should go.

A simplified example:

|Destination Network|Next Hop|Interface|
|---|---|---|
|192.168.1.0/24|Directly connected|Ethernet 0|
|10.0.0.0/8|192.168.1.2|Ethernet 1|
|172.16.0.0/16|192.168.1.3|Ethernet 2|
|0.0.0.0/0|192.168.1.1|Ethernet 1|

Here, `0.0.0.0/0` represents an IPv4 default route.

When multiple routes match a destination, routers generally use **longest-prefix matching** to select the most specific matching route. Other criteria, such as administrative preference and route metrics, can also affect route selection.

## 3.4 Routing Metrics

A routing metric is a value used to evaluate the desirability of a route.

|Metric|Meaning|
|---|---|
|Hop count|Number of routers traversed|
|Bandwidth|Link transmission capacity|
|Delay|Time required for packets to travel|
|Cost|Numerical value assigned to a link or route|
|Reliability|Dependability of the path|
|Load|Traffic carried by a link|

The metric used depends on the routing protocol. A route with fewer hops is not necessarily the fastest route.

## 3.5 Types of Routing

### A. Static Routing

Static routes are configured manually by a network administrator.

**Advantages:**

- Simple for small networks.
- No dynamic routing-update traffic.
- Provides explicit control over routes.

**Disadvantages:**

- Requires manual configuration.
- Becomes difficult to maintain at scale.
- Does not automatically adapt to link failures unless alternate routes and suitable mechanisms are configured.

### B. Dynamic Routing

Dynamic routing allows routers to learn and update routes through routing protocols.

**Advantages:**

- Suitable for larger networks.
- Can adapt to topology changes.
- Reduces the need to configure every route manually.

**Disadvantages:**

- Uses CPU and memory.
- Requires additional configuration.
- Routing updates and convergence can consume resources.

### Static vs dynamic routing

|Feature|Static|Dynamic|
|---|---|---|
|Route configuration|Manual|Learned using protocols|
|Response to topology changes|Usually requires manual action or preconfigured mechanisms|Can update automatically|
|Resource requirements|Generally lower|Generally higher|
|Suitability|Small or stable networks|Larger or changing networks|

## 3.6 Routing Protocols

A routing protocol allows routers to exchange routing information.

|Protocol|Type|Main Characteristics|
|---|---|---|
|**RIP**|Distance vector|Uses hop count; 15 is the maximum usable hop count, while 16 indicates an unreachable destination.|
|**OSPF**|Link state|Uses link-state information and Dijkstra's shortest-path-first algorithm.|
|**IS-IS**|Link state|Calculates routes using link-state information.|
|**BGP**|Path vector|Exchanges routing information between Autonomous Systems and uses path attributes for route selection.|

### Routing protocol classification

```
              ROUTING PROTOCOLS
                     |
          +----------+----------+
          |                     |
          v                     v
         IGP                   EGP
          |                     |
          v                     v
   Within one AS           Between ASes
          |                     |
     +----+----+                |
     v         v                v
    RIP       OSPF             BGP
```

An **Autonomous System (AS)** is a group of IP networks under a common routing administration that presents a coordinated routing policy to other systems.

- **IGP:** Interior Gateway Protocol, used within an AS.
- **EGP:** Exterior Gateway Protocol, used between ASes. BGP is the principal modern interdomain routing protocol.

## 3.7 Distance-Vector Routing

In distance-vector routing, routers learn routes by exchanging routing information with their neighbours.

Each router maintains information conceptually represented as:

```
Destination → Distance/Metric → Next Hop
```

### Working

1. Each router knows its directly connected networks.
2. Routers exchange routing information with neighbours.
3. Each router calculates routes using received information and its routing algorithm.
4. Routing information is updated when routes or metrics change.

**Example protocol:** RIP.

Distance-vector routing is associated with the Bellman–Ford algorithm family.

## 3.8 Link-State Routing

In link-state routing, routers build a topology database from link-state information distributed through the routing domain.

```
        Router A
         /    \
        /      \
   Router B----Router C
        \      /
         \    /
        Router D
```

### Working

1. Routers discover their neighbours.
2. They identify the state and cost of their links.
3. Link-state information is distributed through the routing domain.
4. Each router builds a topology database.
5. Each router calculates the shortest paths from itself to destinations.

**Example protocol:** OSPF.

Link-state routing uses Dijkstra's shortest-path-first algorithm.

### Distance vector vs link state

|Feature|Distance Vector|Link State|
|---|---|---|
|Main information|Route distances learned from neighbours|Network topology and link costs|
|Knowledge of topology|More limited|Broader view of the routing domain|
|Example|RIP|OSPF|
|Algorithm|Bellman–Ford family|Dijkstra SPF|

## 3.9 Shortest-Path Calculation

Routing algorithms may select paths using the sum of link costs.

Consider the following network:

```
          2           3
     A -------- B -------- D
      \                    /
       \4                1/
        \                /
         ------- C ------
               2
```

Possible routes from A to D:

**Route 1: A → B → D**

\[ \text{Cost}=2+3=5 \]

**Route 2: A → C → D**

\[ \text{Cost}=4+2=6 \]

If these are the only possible paths and the route metric is additive link cost, the first route is preferred because its cost is lower.

## 3.10 Routing Loops

A routing loop occurs when packets circulate repeatedly between routers rather than reaching their destination.

```
        +------ R2 ------+
        |                |
        v                |
       R1                R3
        ^                |
        |                |
        +----------------+
```

**Effects:**

- Wastes bandwidth.
- Increases congestion.
- Delays or prevents delivery.

Routing protocols use mechanisms such as split horizon, route poisoning, or loop-free topology calculations to reduce routing-loop problems, depending on the protocol.

---

# 4. CONGESTION CONTROL

## 4.1 Definition

**Network congestion occurs when the amount of traffic entering a network exceeds its available forwarding or buffering capacity.**

When too many packets compete for limited resources, queues grow and packets may be delayed or dropped.

### Diagram

```
Multiple Sources
   |    |    |
   v    v    v
+----------------+
| Router Queue   |  ← Buffer becomes full
+----------------+
        |
        v
   Limited-Capacity
       Link
```

## 4.2 Causes of Congestion

1. Too many packets enter the network simultaneously.
2. A link has insufficient capacity for the offered traffic.
3. Routers have limited buffer space.
4. Traffic is concentrated on a particular path.
5. Retransmissions increase traffic when packets are lost.
6. Some traffic bursts exceed the rate at which packets can be forwarded.

## 4.3 Effects of Congestion

- Increased delay.
- Packet loss.
- Reduced throughput.
- Excessive retransmissions.
- Poor application performance.
- In severe cases, congestion can reduce the useful data delivered despite substantial traffic.

## 4.4 Flow Control vs Congestion Control

These are not the same.

|Flow Control|Congestion Control|
|---|---|
|Prevents a sender from overwhelming its receiver|Prevents excessive traffic from overwhelming network resources|
|Focuses on sender–receiver capacity|Focuses on the network's available capacity|
|Receiver capacity is central|Router queues, links, and aggregate traffic are central|
|Example: receiver-advertised TCP window|Example: TCP congestion window and network congestion signalling|

**Remember:** Flow control protects the receiver; congestion control protects the network from excessive traffic.

## 4.5 Approaches to Congestion Control

The major approaches are:

```
             CONGESTION CONTROL
                     |
            +--------+--------+
            |                 |
            v                 v
       Open-Loop           Closed-Loop
       Prevention          Feedback
```

### A. Open-Loop Congestion Control

Open-loop methods try to prevent congestion **before it develops**, without relying on feedback about the current congestion state.

Examples include:

- Retransmission policies.
- Window policies.
- Acknowledgement policies.
- Packet-discard policies.
- Admission policies.

These policies are intended to reduce unnecessary traffic or avoid admitting more traffic than the network can handle.

### B. Closed-Loop Congestion Control

Closed-loop methods use information about network conditions to detect and respond to congestion.

The general process is:

```
Network congestion
        |
        v
Detect congestion
        |
        v
Send feedback
        |
        v
Reduce traffic or adjust routes
        |
        v
Congestion decreases
```

Examples include:

- Choke packets.
- Explicit Congestion Notification (ECN).
- Backpressure.
- Feedback-based transmission-rate adjustment.

## 4.6 Traffic Shaping

**Traffic shaping** controls the rate at which traffic enters a network. It smooths traffic bursts and helps reduce congestion.

Two classical mechanisms are:

1. Leaky Bucket
2. Token Bucket

### A. Leaky Bucket Algorithm

The leaky bucket algorithm allows packets to be transmitted at a controlled rate from a buffer.

```
Bursty Input
     |
     v
+------------+
| Bucket/    |
| Queue      |
+------------+
     |
     | Controlled output rate
     v
   Network
```

### Working

1. Incoming packets enter the bucket or queue.
2. Packets leave at a configured rate.
3. Incoming traffic may temporarily accumulate in the queue.
4. If the buffer becomes full, additional packets may be discarded.

### Advantages

- Smooths bursts.
- Controls the output rate.
- Helps limit sudden traffic surges.

### Limitation

Traffic above the available buffer capacity may be discarded even if the network is idle shortly afterward.

### B. Token Bucket Algorithm

The token bucket algorithm allows controlled bursts while limiting the long-term transmission rate.

Tokens are generated at a configured rate and stored up to a maximum capacity.

```
Token Generator
       |
       v
+---------------+
| Token Bucket  |
+---------------+
       |
       v
Packets + Tokens
       |
       v
     Network
```

### Working

1. Tokens are generated at a fixed rate.
2. Tokens accumulate up to the bucket limit.
3. A packet requires sufficient tokens before it can be transmitted under the policy.
4. If enough tokens have accumulated, a burst can be transmitted.
5. If insufficient tokens are available, packets wait or are handled according to the configured policy.

### Advantages

- Limits the long-term traffic rate.
- Permits bursts when tokens have accumulated.
- Is suitable for traffic that fluctuates over time.

### Leaky bucket vs token bucket

|Feature|Leaky Bucket|Token Bucket|
|---|---|---|
|Main purpose|Smooths traffic output|Controls average rate while allowing bursts|
|Burst handling|Output is paced at a controlled rate|Accumulated tokens permit bursts|
|Excess traffic|May be queued or dropped|May wait, be shaped, or be dropped according to policy|
|Output behaviour|Generally smoother|May be bursty within the configured limits|

## 4.7 Backpressure

Backpressure is a congestion-control technique in which a congested node signals upstream nodes to reduce or stop sending traffic.

```
Source → Router A → Router B → Router C
                     |
                     | Congested
                     v
             Signal upstream nodes
```

The signal can propagate upstream until the traffic entering the congested area is reduced.

This technique is associated with networks that provide suitable feedback mechanisms.

## 4.8 Choke Packets

A **choke packet** is a control packet sent by a congested router to notify a source that it should reduce its transmission rate.

```
Source --------------------> Router
  ^                           |
  |                           | Congestion detected
  +------ Choke Packet -------+
```

On receiving the notification, the source reduces its sending rate according to the protocol's policy.

## 4.9 Load Shedding

**Load shedding** means deliberately discarding some packets when a router or network cannot process all incoming traffic.

A router may discard:

- Lower-priority packets.
- Packets from selected traffic classes.
- Packets according to a configured discard policy.

Load shedding sacrifices some traffic to preserve the operation of the network and reduce overload.

## 4.10 Explicit Congestion Notification (ECN)

ECN allows capable network devices to signal congestion without necessarily discarding the packet to communicate that signal.

A congestion-aware router marks a packet, and the receiving endpoint or transport protocol can inform the sender. The sender can then reduce its sending rate.

ECN requires compatible support and appropriate configuration throughout the relevant communication path.

## 4.11 Admission Control

Admission control determines whether a new flow or connection should be allowed to use the network.

If the network cannot provide the required resources or service guarantees, the request may be rejected.

This approach is especially relevant to networks that reserve resources or provide quality-of-service guarantees.

### Congestion-control techniques: summary

|Technique|Main Idea|
|---|---|
|Traffic shaping|Regulates the rate at which traffic enters the network|
|Backpressure|Signals upstream nodes to reduce transmission|
|Choke packet|Notifies a source to reduce its sending rate|
|Load shedding|Discards selected traffic during overload|
|ECN|Explicitly signals congestion through packet marking|
|Admission control|Limits new traffic when sufficient resources are unavailable|

---

# 5. INTERNETWORKING

## 5.1 Definition

**Internetworking is the process of connecting two or more independent networks so that devices on different networks can communicate.**

Networks may use different link technologies and have different physical architectures.

The Network Layer makes communication possible across these heterogeneous networks by using common addressing and packet-forwarding mechanisms.

### Diagram

```
       Network A                  Network B
    +-------------+            +-------------+
    | Computers   |            | Computers   |
    +-------------+            +-------------+
           |                          |
           v                          v
       +--------+                +--------+
       | Router |================| Router |
       +--------+    Inter-      +--------+
                     network
```

## 5.2 Need for Internetworking

Internetworking enables:

- Communication between different LANs.
- Connections between organizational networks.
- Communication across geographically separated locations.
- Access to servers and services on remote networks.
- Communication between networks using different link technologies.

## 5.3 Components of Internetworking

### 1. Hosts

Computers, servers, smartphones, and other end devices that send or receive data.

### 2. Routers

Forward IP packets between networks.

### 3. Communication links

Carry packets between devices and networks.

### 4. Protocols

Define addressing, packet formats, routing, and communication behaviour.

### 5. Gateways and translation mechanisms

Some networks or applications require gateways to translate between different protocols, addressing schemes, or representations.

## 5.4 How Internetworking Works

Suppose a computer in one LAN communicates with a server on another network.

```
Computer A
    |
    v
 Local Switch
    |
    v
 Router A
    |
    v
 Network / Internet
    |
    v
 Router B
    |
    v
 Server B
```

The process is:

1. Computer A determines that the destination IP address is outside its local IP subnet.
2. It sends the packet to its configured default gateway.
3. Router A examines the destination address and selects the next hop.
4. Intermediate routers forward the packet toward the destination network.
5. Router B forwards it into the destination LAN.
6. Server B receives the packet.

At each router, the link-layer frame is processed and a new frame is created for the next link. The IP packet is forwarded toward the destination, subject to any required network-layer processing.

## 5.5 Challenges in Internetworking

|Challenge|Explanation|
|---|---|
|Heterogeneous technologies|Networks may use Ethernet, Wi-Fi, fiber, or other link technologies.|
|Addressing|Devices need logical addresses that identify source and destination IP endpoints.|
|Routing|Routers need paths through multiple interconnected networks.|
|Different MTUs|A packet may be larger than the maximum transmission unit supported by a link.|
|Congestion|Traffic may exceed the capacity of links or routers.|
|Security|Traffic crosses network boundaries and may require filtering and protection.|

---

# 6. TCP/IP MODEL

## 6.1 Definition

**TCP/IP is the protocol suite that forms the foundation of communication over the Internet.**

TCP/IP is also commonly taught as a four-layer networking model.

TCP/IP stands for **Transmission Control Protocol / Internet Protocol**.

## 6.2 Layers of the TCP/IP Model

```
┌───────────────────────────────┐
│  4. Application Layer         │
├───────────────────────────────┤
│  3. Transport Layer           │
├───────────────────────────────┤
│  2. Internet Layer            │
├───────────────────────────────┤
│  1. Network Access / Link     │
└───────────────────────────────┘
```

## 6.3 Functions of Each Layer

|Layer|Main Responsibilities|Examples|
|---|---|---|
|Application|Provides application-level communication protocols|HTTP, HTTPS, DNS, SMTP, SSH|
|Transport|Provides process-to-process communication|TCP, UDP|
|Internet|Provides logical addressing and packet delivery across networks|IPv4, IPv6, ICMP|
|Network Access/Link|Handles local-link transmission and framing|Ethernet, Wi-Fi, PPP|

### 1. Application Layer

Provides protocols for applications to exchange information.

Examples:

- HTTP/HTTPS for web communication.
- DNS for translating domain names into IP addresses.
- SMTP for sending email.
- SSH for secure remote access.

### 2. Transport Layer

Provides communication between applications running on different devices.

**TCP:**

- Connection-oriented.
- Provides reliable, ordered byte-stream delivery.
- Uses acknowledgements, retransmissions, flow control, and congestion control.

**UDP:**

- Connectionless.
- Does not provide TCP-style reliability or ordered delivery.
- Offers a simpler transport service with lower protocol overhead.

### 3. Internet Layer

Provides logical addressing and delivery of packets across interconnected networks.

Important protocols include IPv4, IPv6, and ICMP.

The Internet Layer does not guarantee delivery or ordering.

### 4. Network Access/Link Layer

Handles transmission over the local link, including link-layer framing and access to the transmission medium.

Examples include Ethernet, Wi-Fi, and PPP.

## 6.4 Encapsulation in TCP/IP

Encapsulation occurs when each layer adds protocol information to the data it receives.

```
Application Data
       |
       v
Transport Layer
       |
       v
TCP Segment / UDP Datagram
       |
       v
Internet Layer
       |
       v
IP Packet
       |
       v
Link Layer
       |
       v
Frame
       |
       v
Physical Transmission
```

### Protocol data units

|Layer|Data Unit|
|---|---|
|Application|Data / message|
|Transport|TCP segment or UDP datagram|
|Internet|IP packet/datagram|
|Link|Frame|
|Physical transmission|Bits/signals|

At the receiving end, decapsulation occurs as data moves upward through the layers.

## 6.5 OSI vs TCP/IP

|Feature|OSI|TCP/IP|
|---|---|---|
|Number of layers|7|Commonly 4|
|Nature|Reference model|Protocol suite and commonly taught model|
|Session layer|Separate|Functions are generally included in the Application layer|
|Presentation layer|Separate|Functions are generally included in the Application layer|
|Physical and Data Link|Separate|Combined in the Network Access/Link layer|
|Usage|Conceptual and educational reference|Foundation of modern Internet communication|

---

# 7. IP PACKET

## 7.1 Definition

An **IP packet** is the data unit used by the Internet Protocol to carry data from a source IP address toward a destination IP address.

An IP packet consists of:

1. An IP header containing control and addressing information.
2. A payload carrying the data from the upper layer.

The IPv4 and IPv6 packet headers have different formats.

---

# 8. IPv4 PACKET FORMAT

## 8.1 IPv4 Header

An IPv4 header is normally **20 bytes** long without options. Its maximum size is **60 bytes**.

### Diagram

```
                 IPv4 HEADER
  0                   15                  31
  +---------+---------+---------------------+
  | Version |   IHL   | DSCP | ECN | Total  |
  |   4     |    4    |  6   |  2  | Length |
  +---------+---------+---------------------+
  | Identification    |Flags| Fragment Offset|
  +-------------------+-----+----------------+
  | TTL       | Protocol     | Header Checksum|
  +------------------------------------------+
  |              Source IP Address           |
  +------------------------------------------+
  |           Destination IP Address          |
  +------------------------------------------+
  |     Options and Padding (if present)      |
  +------------------------------------------+
  |                  Payload                  |
  +------------------------------------------+
```

_Diagram fields are arranged for examination purposes; their sizes are specified in the descriptions below._

## 8.2 IPv4 Header Fields

|Field|Size|Function|
|---|---|---|
|Version|4 bits|Identifies the IP version; IPv4 uses value 4.|
|IHL|4 bits|Specifies the header length in 32-bit words.|
|DSCP|6 bits|Supports differentiated traffic treatment.|
|ECN|2 bits|Supports explicit congestion notification.|
|Total Length|16 bits|Specifies the total size of the IP packet in bytes, including header and data.|
|Identification|16 bits|Identifies fragments belonging to the same original datagram.|
|Flags|3 bits|Includes fragmentation-related controls, such as DF and MF.|
|Fragment Offset|13 bits|Identifies a fragment's position in the original datagram in 8-byte units.|
|TTL|8 bits|Limits packet lifetime by being reduced as the packet traverses routers.|
|Protocol|8 bits|Identifies the next-layer payload protocol.|
|Header Checksum|16 bits|Detects corruption in the IPv4 header.|
|Source Address|32 bits|Identifies the source IPv4 address.|
|Destination Address|32 bits|Identifies the destination IPv4 address.|
|Options and Padding|Variable|Provides optional functions and aligns the header when required.|

### Important fields explained

**TTL (Time to Live):** A router reduces the TTL value as it forwards a packet. When the TTL expires, the router discards the packet. This prevents a packet from circulating indefinitely.

**Protocol:** Identifies the upper-layer protocol carried by the packet. Common values include TCP (6), UDP (17), and ICMP (1).

**Fragmentation:** If IPv4 fragmentation is performed, fragments carry information that allows the original datagram to be reconstructed at the destination. The DF flag indicates that fragmentation is prohibited for that packet; the MF flag indicates that more fragments follow.

**Header checksum:** Covers only the IPv4 header, not the payload. It must be updated when header fields affecting the checksum change, including TTL during normal forwarding.

## 8.3 IPv4 Packet Characteristics

- Uses 32-bit source and destination addresses.
- Provides connectionless, best-effort delivery.
- Allows variable-length headers.
- Supports fragmentation and reassembly.
- Includes a header checksum.
- Has a maximum total packet length of 65,535 bytes, although practical link MTUs are commonly much smaller.

---

# 9. IP ADDRESSING

## 9.1 Definition

An **IP address is a logical address assigned to a network interface for communication using the Internet Protocol**.

It identifies an interface within an IP network and allows packets to be routed toward their destination.

Two main IP versions are used:

1. IPv4
2. IPv6

## 9.2 IPv4 Address

An IPv4 address contains **32 bits**, divided into four 8-bit groups called octets.

Example:

```
192.168.1.10
```

Each octet is represented in decimal, with a value from 0 to 255.

### Binary representation

```
192       168       1         10
11000000.10101000.00000001.00001010
```

## 9.3 Network and Host Parts

An IP address is interpreted using a network prefix.

- **Network portion:** Identifies the IP network or subnet.
- **Host portion:** Identifies an address within that subnet.

The prefix length determines how many bits belong to the network portion.

### Example

```
IP Address: 192.168.1.10/24

Network bits: 24
Host bits:     8
```

The `/24` prefix means that the first 24 bits form the network prefix.

The corresponding subnet mask is:

```
255.255.255.0
```

For this conventional IPv4 subnet:

- Network address: `192.168.1.0`
- Example host address: `192.168.1.10`
- Broadcast address: `192.168.1.255`

A `/24` subnet has 256 total addresses. Under conventional IPv4 subnetting, 254 are available for ordinary host assignments because the network and broadcast addresses are reserved. Special cases such as `/31` point-to-point subnets follow different rules.

---

## 9.4 Classful IPv4 Addressing

Classful addressing is the historical method of dividing IPv4 addresses into fixed classes.

Although modern networks use CIDR, classful addressing remains relevant to many examinations.

|Class|First Octet Range|Default Mask|Traditional Use|
|---|---|---|---|
|A|1–126|255.0.0.0 (`/8`)|Large networks|
|B|128–191|255.255.0.0 (`/16`)|Medium-sized networks|
|C|192–223|255.255.255.0 (`/24`)|Smaller networks|
|D|224–239|Not applicable|Multicast|
|E|240–255|Not applicable|Reserved/experimental range|

**Important notes:**

- `127.0.0.0/8` is reserved for loopback.
- `0.0.0.0` has special meanings, including an unspecified source address in relevant contexts.
- Classes A, B, and C are historical classifications; modern routing primarily uses classless addressing.

## 9.5 Classless Inter-Domain Routing (CIDR)

**CIDR** allows IP networks to use variable-length prefixes instead of fixed class-based network sizes.

It represents an address with a slash followed by the prefix length.

Examples:

```
192.168.1.0/24
10.20.0.0/16
172.20.10.0/24
```

The number after the slash indicates the number of network-prefix bits.

### Why CIDR is useful

- Supports flexible subnet sizes.
- Reduces address wastage.
- Allows aggregation of multiple routes into larger prefixes.
- Is the standard approach used in modern IP addressing and routing.

### Host-count formula

For an ordinary IPv4 subnet with \(h\) host bits, the conventional number of usable host addresses is:

\[ \boxed{2^h-2} \]

For example, a `/24` prefix leaves:

\[ h=32-24=8 \]

Thus:

\[ 2^8-2=254 \]

This formula assumes conventional subnetting and does not apply unchanged to all special-purpose prefixes.

## 9.6 Private IPv4 Addresses

Private IPv4 addresses are used within private networks and are not directly routed across the public Internet.

|Private Range|CIDR|
|---|---|
|`10.0.0.0` – `10.255.255.255`|`10.0.0.0/8`|
|`172.16.0.0` – `172.31.255.255`|`172.16.0.0/12`|
|`192.168.0.0` – `192.168.255.255`|`192.168.0.0/16`|

A network commonly uses private addressing internally and may use Network Address Translation (NAT) to enable communication with the public IPv4 Internet.

### Public vs private IP addresses

|Public IP|Private IP|
|---|---|
|Used for globally routed communication|Used within private networks|
|Must be globally coordinated to avoid address conflicts|Can be reused in separate private networks|
|Public reachability depends on routing, filtering, and configuration|Usually requires routing/NAT or a suitable gateway to reach the public Internet|

---

# 10. IPV6

## 10.1 Definition

**IPv6 (Internet Protocol version 6) is the successor to IPv4, designed primarily to provide a much larger address space and improve the IP protocol's base-header design.**

IPv6 uses **128-bit addresses**, compared with IPv4's 32-bit addresses.

## 10.2 Why Was IPv6 Introduced?

IPv4 offers approximately 4.3 billion possible addresses in its 32-bit address space. The rapid growth of the Internet made this space insufficient for unrestricted global allocation.

IPv6 was introduced to provide a much larger address space and support the continued growth of Internet-connected devices.

Other design improvements include:

- A simplified fixed-length base header.
- Support for extension headers.
- Removal of the IPv4 header checksum.
- A redesigned fragmentation mechanism.

## 10.3 Characteristics of IPv6

|Feature|Description|
|---|---|
|Address size|128 bits|
|Address notation|Eight groups of hexadecimal digits, separated by colons|
|Base header size|Fixed at 40 bytes|
|Address space|Vastly larger than IPv4|
|Broadcast|No broadcast addressing; multicast serves group-delivery purposes|
|Header checksum|No IPv6 base-header checksum|
|Fragmentation|Routers do not fragment IPv6 packets; the source may use a Fragment extension header|
|Configuration|Supports manual configuration, SLAAC, DHCPv6, and other mechanisms|
|Address resolution|Uses Neighbor Discovery through ICMPv6 rather than ARP|

## 10.4 IPv6 Address Format

An IPv6 address consists of **128 bits**, written as eight groups of four hexadecimal digits.

Example:

```
2001:0db8:0000:0000:02aa:00ff:fe28:9c5a
```

Each group represents 16 bits.

### Shortening an IPv6 Address

IPv6 allows two main abbreviation rules:

**Rule 1: Omit leading zeros within a group.**

For example:

```
0db8 → db8
00ff → ff
```

**Rule 2: Replace one consecutive run of all-zero groups with `::`.**

For example:

```
2001:0db8:0000:0000:0000:0000:0000:0001
```

can be written as:

```
2001:db8::1
```

**Important:** The `::` notation can be used only once in a single address because it represents an unknown number of omitted zero groups.

## 10.5 IPv6 Address Types

### A. Unicast

Identifies a single interface. A packet sent to a unicast address is intended for that interface.

Common categories include:

- Global unicast
- Link-local unicast
- Unique local addresses

### B. Multicast

Identifies a group of interfaces. Packets sent to a multicast address are delivered to members of the group according to the network's multicast mechanisms.

### C. Anycast

The same address is assigned to multiple interfaces. A packet sent to that address is delivered to one suitable member of the group, according to routing and the network's forwarding behaviour.

**IPv6 does not use broadcast addresses.** Multicast is used for many purposes that would otherwise require broadcasting.

## 10.6 Important IPv6 Address Ranges

|Prefix/Address|Purpose|
|---|---|
|`2000::/3`|Global unicast address space|
|`fe80::/10`|Link-local addresses|
|`fc00::/7`|Unique local address space|
|`ff00::/8`|Multicast addresses|
|`::1`|Loopback address|
|`::`|Unspecified address|

Link-local addresses are used on the local link and are not globally routable.

## 10.7 IPv6 Packet Header

IPv6 has a **fixed 40-byte base header**.

### Diagram

```
                    IPv6 BASE HEADER

  +---------+----------------+----------------------+
  | Version | Traffic Class  | Flow Label           |
  |  4 bits |    8 bits      | 20 bits              |
  +---------+----------------+----------------------+
  | Payload Length | Next Header | Hop Limit         |
  |   16 bits      |   8 bits    | 8 bits            |
  +----------------------------------------------- -+
  |                                                 |
  |               Source Address                    |
  |                   128 bits                      |
  |                                                 |
  +-------------------------------------------------+
  |                                                 |
  |            Destination Address                  |
  |                   128 bits                      |
  |                                                 |
  +-------------------------------------------------+
  |              Extension Headers                  |
  |                  (Optional)                     |
  +-------------------------------------------------+
  |                    Payload                      |
  +-------------------------------------------------+
```

## 10.8 IPv6 Header Fields

|Field|Size|Function|
|---|---|---|
|Version|4 bits|Identifies IPv6; value is 6.|
|Traffic Class|8 bits|Supports traffic classification and differentiated handling.|
|Flow Label|20 bits|Identifies packets belonging to a flow for specialised handling where supported.|
|Payload Length|16 bits|Specifies the size of the data after the 40-byte base header, including extension headers.|
|Next Header|8 bits|Identifies an extension header or the upper-layer protocol.|
|Hop Limit|8 bits|Limits how many router hops the packet may traverse.|
|Source Address|128 bits|Identifies the source interface's IPv6 address.|
|Destination Address|128 bits|Identifies the destination IPv6 address.|

### Important differences from IPv4

IPv6:

- Does not have the IPv4 IHL field because its base header has a fixed size.
- Does not have a base-header checksum.
- Uses Hop Limit instead of TTL.
- Uses extension headers for optional information.
- Does not allow routers to fragment packets.
- Allows the sending host to perform fragmentation using a Fragment extension header when appropriate.

## 10.9 IPv4 vs IPv6

This is a very important examination question.

|Feature|IPv4|IPv6|
|---|---|---|
|Address size|32 bits|128 bits|
|Example|`192.168.1.10`|`2001:db8::1`|
|Address notation|Decimal octets|Hexadecimal groups|
|Base header size|20–60 bytes|40 bytes|
|Header checksum|Present|Absent|
|Broadcast|Supported|Not supported|
|Multicast|Supported|Supported|
|Fragmentation|Routers may fragment unless prohibited|Routers do not fragment|
|Address resolution|ARP is commonly used on Ethernet|Neighbor Discovery using ICMPv6|
|Configuration|Manual, DHCP, and other methods|Manual, SLAAC, DHCPv6, and other methods|
|Address space|Smaller|Vastly larger|

## 10.10 IPv6 Advantages

1. **Large address space:** Provides many more addresses than IPv4.
2. **Simplified base header:** Uses a fixed-size base header, reducing certain aspects of header processing.
3. **Improved support for extension headers:** Optional information can be carried separately from the base header.
4. **No broadcast:** Uses multicast and other address types instead.
5. **Modern network support:** Supports current networking requirements, including large-scale addressing and autoconfiguration.

### Limitations and challenges

- IPv4 and IPv6 are not directly interoperable without dual-stack operation or suitable transition mechanisms.
- Network administrators may need to update software, hardware, monitoring, and security rules.
- IPv6 does not automatically make a network secure.
- IPv4 and IPv6 often need to coexist during migration.

## 10.11 IPv4-to-IPv6 Transition

Three important transition approaches are:

### 1. Dual Stack

A device or network operates both IPv4 and IPv6.

```
            Device
               |
          +----+----+
          |         |
          v         v
        IPv4       IPv6
          |         |
          +----+----+
               |
             Network
```

### 2. Tunneling

Packets of one protocol are carried inside packets of another protocol across a network that does not natively support the carried protocol.

### 3. Translation

A translation mechanism converts between IPv4 and IPv6 communication where necessary. For example, NAT64 supports communication between IPv6 clients and IPv4 servers in suitable deployments.

---

# 11. IMPORTANT COMPARISONS FOR EXAMS

These are the key comparisons from Unit 3.

## A. TCP vs IP

|TCP|IP|
|---|---|
|Transport-layer protocol|Network-layer protocol|
|Provides reliable, ordered byte-stream delivery|Provides best-effort packet delivery|
|Uses port numbers|Uses IP addresses|
|Performs retransmission and congestion-control functions|Routes and forwards packets toward destinations|

## B. Routing vs Congestion Control

|Routing|Congestion Control|
|---|---|
|Selects paths toward destinations|Manages excessive traffic relative to network capacity|
|Uses route information and metrics|Uses traffic control, feedback, and resource-management mechanisms|
|Primarily concerns path selection|Primarily concerns network load and available resources|

## C. IPv4 vs IPv6

Focus particularly on address size, header length, broadcast support, fragmentation, header checksum, and address notation.

The full comparison is provided in Section 10.9.

---

# 12. IMPORTANT FORMULAS AND NUMERICAL CONCEPTS

## 12.1 IPv4 Address Length

\[ \boxed{\text{IPv4 address length}=32\text{ bits}} \]

## 12.2 IPv6 Address Length

\[ \boxed{\text{IPv6 address length}=128\text{ bits}} \]

## 12.3 Number of Possible Addresses

For an address space of \(n\) bits:

\[ \boxed{\text{Number of possible bit patterns}=2^n} \]

IPv4:

\[ 2^{32}=4,294,967,296 \]

IPv6:

\[ 2^{128}\approx3.4\times10^{38} \]

These are the theoretical sizes of the address spaces; not every address is available for ordinary global unicast assignment.

## 12.4 Number of Host Bits

For ordinary IPv4 subnetting:

\[ \boxed{h=32-\text{prefix length}} \]

## 12.5 Number of Usable IPv4 Host Addresses

For a conventional IPv4 subnet:

\[ \boxed{\text{Usable hosts}=2^h-2} \]

Example: `/26`

\[ h=32-26=6 \]\[ 2^6-2=62 \]

Therefore, a conventional `/26` subnet provides **62 usable host addresses**.

---

# 13. EXAM-IMPORTANT QUESTIONS — UNIT 3

## Long-answer questions

1. Define the Network Layer and explain its major functions.
2. Explain point-to-point networks with a suitable diagram.
3. Differentiate datagram networks and virtual-circuit networks.
4. Explain routing and the functions of a routing table.
5. Differentiate static routing and dynamic routing.
6. Explain distance-vector and link-state routing.
7. Explain RIP, OSPF, and BGP.
8. Define congestion and discuss its causes and effects.
9. Differentiate flow control and congestion control.
10. Explain open-loop and closed-loop congestion-control approaches.
11. Explain the leaky bucket and token bucket algorithms.
12. Define internetworking and explain its working.
13. Explain the TCP/IP model and the functions of its layers.
14. Draw and explain the IPv4 packet format.
15. Explain IPv4 addressing and classful address ranges.
16. Explain CIDR and subnet masks with examples.
17. Explain IPv6 addressing and the IPv6 header format.
18. Differentiate IPv4 and IPv6.
19. Explain the transition mechanisms used for IPv4 and IPv6 coexistence.

## Numerical and short-problem preparation

Prepare these areas particularly well:

- Finding an IPv4 network address from an IP address and prefix.
- Finding the number of host bits and usable host addresses.
- Identifying IPv4 address classes.
- Calculating the total size of IPv4 and IPv6 address spaces.
- Comparing the costs of candidate routes.
- Understanding IPv4 packet length and fragmentation fields.
- Tracing how packets are forwarded across multiple networks.

---

# 14. LAST-MINUTE REVISION SHEET

```
UNIT 3 — NETWORK LAYER

1. NETWORK LAYER
   Layer 3
   Logical addressing
   Routing
   Packet forwarding
   Internetworking

2. POINT-TO-POINT NETWORKS
   Two endpoints per link
   Used to connect routers
   Datagram network → Independent packets
   Virtual circuit → Established logical path

3. ROUTING
   Static → Manually configured
   Dynamic → Routes learned using protocols

   RIP   → Distance vector
   OSPF  → Link state + Dijkstra
   BGP   → Path vector + inter-AS routing

4. CONGESTION CONTROL
   Congestion → Traffic exceeds network capacity
   Flow control → Protects receiver
   Congestion control → Protects network resources

   Leaky bucket → Smooths output
   Token bucket → Allows controlled bursts
   Choke packet → Signals source to reduce rate
   Load shedding → Discards selected traffic
   ECN → Signals congestion through packet marking

5. INTERNETWORKING
   Connects independent networks
   Routers forward packets between networks

6. TCP/IP MODEL
   Application
   Transport
   Internet
   Network Access / Link

7. IPv4 PACKET
   Address length = 32 bits
   Header = 20–60 bytes
   Header checksum = Present
   TTL limits packet lifetime
   Fragmentation supported

8. IPv4 ADDRESSING
   Class A → 1–126
   Class B → 128–191
   Class C → 192–223
   Class D → 224–239
   Class E → 240–255

   CIDR → Variable-length network prefixes

9. IPv6
   Address length = 128 bits
   Base header = 40 bytes
   Header checksum = Absent
   No broadcast
   Routers do not fragment
   Uses extension headers
```

## What to prioritise for the examination

Study these topics thoroughly first: **IPv4 packet format, IPv4 addressing and CIDR, IPv6 header and addressing, TCP/IP model, routing protocols, and congestion-control techniques.** These provide the core definitions, diagrams, comparisons, and numerical concepts needed to answer most types of questions from Unit 3.