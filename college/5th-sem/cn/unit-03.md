# UNIT 3 – NETWORK LAYER

**Subject:** Computer Networks  
**Examination:** Mid-Semester  
**Preparation:** Complete and exam-oriented notes

---

# MOST IMPORTANT TOPICS FOR MID-SEM EXAM

Prepare these topics first. They cover the major concepts, diagrams, comparisons and numericals of this unit. These are likely examination areas, not guaranteed questions.

## Priority 1 – Very Important

|Topic|What to Prepare|
|---|---|
|IPv4 vs IPv6|Differences, features, address format and diagrams|
|IPv4 Packet Format|Header diagram and explanation of all fields|
|IPv6 Header Format|Header diagram, fields and characteristics|
|TCP/IP Model|Four layers, functions and diagram|
|Routing|Static vs dynamic, routing protocols and algorithms|
|Congestion Control|Open-loop, closed-loop, Leaky Bucket and Token Bucket|
|IP Addressing|Classes, CIDR, subnet mask and numericals|

## Priority 2 – Important

|Topic|What to Prepare|
|---|---|
|Point-to-Point Networks|Definition, diagram and characteristics|
|Datagram vs Virtual Circuit|Differences and examples|
|Internetworking|Definition, need, components and working|
|Routing Protocols|RIP, OSPF and BGP|
|Distance Vector vs Link State|Working and comparison|
|Flow Control vs Congestion Control|Differences|
|IPv6 Address Types|Unicast, Multicast and Anycast|

## Priority 3 – Additional Preparation

1. Routing metrics and routing tables.
    
2. Routing vs forwarding.
    
3. Encapsulation and decapsulation.
    
4. Public vs private IP addresses.
    
5. NAT and its working.
    
6. IPv4 fragmentation.
    
7. IPv6 address shortening rules.
    
8. IPv4-to-IPv6 transition mechanisms.
    

## Important Diagrams to Practise

1. OSI Model showing Network Layer.
    
2. Point-to-Point Network.
    
3. Datagram and Virtual Circuit Networks.
    
4. Routing diagram.
    
5. Leaky Bucket Algorithm.
    
6. Token Bucket Algorithm.
    
7. TCP/IP Model.
    
8. IPv4 Header Format.
    
9. IPv6 Header Format.
    
10. IPv4-to-IPv6 Dual Stack.
    

**Exam tip:** Practise drawing IPv4 and IPv6 headers carefully. Learn the TCP/IP layers and congestion-control algorithms with their working.

---

# 1. NETWORK LAYER

## 1.1 Definition

The Network Layer is the third layer (Layer 3) of the OSI model. It is responsible for delivering data packets from the source to the destination across different interconnected networks.

It uses logical addressing, routing and packet forwarding to transfer data between networks.

## 1.2 Position in the OSI Model

```
   +--------------------------+
   |  Application Layer       |
   +--------------------------+
   |  Presentation Layer      |
   +--------------------------+
   |  Session Layer           |
   +--------------------------+
   |  Transport Layer         |
   +==========================+
   |  Network Layer           |  <- Layer 3
   +--------------------------+
   |  Data Link Layer         |
   +--------------------------+
   |  Physical Layer          |
   +--------------------------+
```

## 1.3 Functions of Network Layer

|Function|Explanation|
|---|---|
|Logical Addressing|Uses IP addresses to identify source and destination interfaces.|
|Routing|Determines the suitable path for data transmission.|
|Forwarding|Transfers packets to the appropriate next router.|
|Internetworking|Connects different networks for communication.|
|Fragmentation|IPv4 can divide large packets into smaller fragments when required.|
|Congestion Management|Supports mechanisms that help detect and control network congestion.|

**Data Unit:** Packet (IP Datagram).

**Main Device:** Router.

## 1.4 Working of Network Layer

1. Receives data from the Transport Layer.
    
2. Adds an IP header containing source and destination addresses.
    
3. Forms an IP packet.
    
4. Determines the appropriate next hop.
    
5. Forwards the packet toward the destination.
    

## 1.5 Best-Effort Delivery

IP provides connectionless, best-effort delivery.

It means:

- IP attempts to deliver packets to the destination.
    
- It does not guarantee successful delivery.
    
- It does not guarantee packet ordering.
    
- It does not provide retransmission of lost packets by itself.
    

When reliability is required, protocols such as TCP provide additional mechanisms.

---

# 2. POINT-TO-POINT NETWORKS

## 2.1 Definition

A Point-to-Point Network is a communication arrangement in which a direct link connects exactly two devices or network endpoints.

It is commonly used to connect two routers.

## 2.2 Diagram

```
        Network A                 Network B
            |                         |
            v                         v
       +----------+              +----------+
       | Router A |==============| Router B |
       +----------+  Direct Link +----------+
```

## 2.3 Characteristics

1. Connects two endpoints.
    
2. Provides direct communication between connected devices.
    
3. Usually does not require multiple stations to compete for access to the same link.
    
4. Can use wired or wireless communication.
    
5. Commonly used in WAN connections.
    
6. Can provide predictable link capacity depending on the technology.
    

## 2.4 Advantages

1. Simple communication between two endpoints.
    
2. Easier troubleshooting.
    
3. Controlled access to the communication link.
    
4. Predictable performance when a dedicated link is used.
    

## 2.5 Limitations

1. Establishing separate physical links between many devices can be expensive.
    
2. Link failure interrupts communication over that link until an alternative path is used.
    
3. A single point-to-point link cannot connect every device in a large network.
    

## 2.6 Point-to-Point vs Broadcast Network

|Point-to-Point|Broadcast Network|
|---|---|
|Connects two endpoints per link.|Multiple stations share a communication medium.|
|Usually no competition between multiple stations for the same link.|Multiple stations may need to coordinate channel access.|
|Direct communication between endpoints.|A transmission may be visible to multiple stations.|
|Example: Router-to-router link.|Example: Traditional shared Ethernet.|

## 2.7 Types of Packet-Switched Network Services

Two important packet delivery approaches are:

### A. Datagram Network

Definition: In a datagram network, each packet is treated independently and may follow a different route to reach the destination.

Diagram:

```
                 Router B
                /        \
               /          \
Source ---- Router A       Router D ---- Destination
               \          /
                \        /
                 Router C
```

Characteristics:

1. No connection setup is required at the network layer.
    
2. Each packet contains destination-address information.
    
3. Packets may follow different routes.
    
4. Packets may arrive out of order.
    
5. Routers do not require per-connection virtual-circuit state.
    

Example: Internet Protocol (IPv4 and IPv6).

### B. Virtual Circuit Network

Definition: A virtual-circuit network establishes a logical path before transmitting packets.

Diagram:

```
Source -> Router A -> Router B -> Router C -> Destination
          <--------- Logical Path --------->
```

Characteristics:

1. Connection setup is required.
    
2. Routers maintain virtual-circuit information.
    
3. Packets generally follow the same logical path.
    
4. Packets generally arrive in sequence.
    
5. Packets use virtual-circuit identifiers or labels.
    

Examples: ATM, Frame Relay and X.25.

### Datagram vs Virtual Circuit

|Datagram Network|Virtual Circuit Network|
|---|---|
|No connection setup required at network layer.|Connection setup required.|
|Packets routed independently.|Packets follow an established logical path.|
|Different paths are possible.|Generally follows the same logical path.|
|Routers need no per-connection virtual-circuit state.|Routers maintain virtual-circuit state.|
|Packets may arrive out of order.|Packets generally arrive in sequence.|
|Example: IP.|Example: ATM.|

**Remember:** Point-to-point describes a link between two endpoints, whereas datagram and virtual circuit describe packet-delivery approaches.

---

# 3. ROUTING

## 3.1 Definition

Routing is the process of selecting the best available path for transferring data packets from a source to a destination through interconnected networks.

A router performs routing at the Network Layer.

## 3.2 Routing Diagram

```
               Router B
              /        \
             /          \
Source -> Router A       Router D -> Destination
             \          /
              \        /
               Router C
```

## 3.3 Functions of Routing

1. Selects the appropriate path for packet transmission.
    
2. Uses destination IP addresses.
    
3. Maintains routing information.
    
4. Identifies the next hop.
    
5. Finds alternative paths when supported and required.
    

## 3.4 Routing Table

A routing table stores information about available network routes.

Example:

|Destination|Next Hop|
|---|---|
|192.168.1.0/24|Direct|
|10.0.0.0/8|192.168.1.2|
|172.16.0.0/16|192.168.1.3|
|0.0.0.0/0|Default Gateway|

**Next Hop:** The next router or device to which a packet should be forwarded.

`0.0.0.0/0` represents an IPv4 default route.

### Longest Prefix Matching

When multiple routes match a destination IP address, the router generally selects the route with the longest matching prefix.

Example:

|Destination|Next Hop|
|---|---|
|192.168.0.0/16|Router A|
|192.168.1.0/24|Router B|
|192.168.1.128/25|Router C|

For destination `192.168.1.150`, Router C is selected because `/25` is the most specific matching prefix.

## 3.5 Routing Metrics

A routing metric is a value used to evaluate the desirability of a route.

|Metric|Meaning|
|---|---|
|Hop Count|Number of routers traversed.|
|Bandwidth|Data-carrying capacity of a link.|
|Delay|Time required for data to travel.|
|Cost|Numerical value assigned to a route or link.|
|Reliability|Dependability of a network path.|
|Load|Traffic carried by a link.|

The routing protocol determines which metrics are used.

## 3.6 Types of Routing

### A. Static Routing

Definition: Static routing is a method in which routes are manually configured by a network administrator.

Advantages:

1. Simple for small networks.
    
2. No dynamic routing-update traffic.
    
3. Provides manual control over routes.
    
4. Requires fewer routing resources.
    

Disadvantages:

1. Manual configuration is required.
    
2. Difficult to manage in large networks.
    
3. Does not automatically adapt to failures unless alternatives are configured.
    

### B. Dynamic Routing

Definition: Dynamic routing allows routers to automatically learn and update routes using routing protocols.

Advantages:

1. Suitable for large networks.
    
2. Automatically updates routes.
    
3. Can adapt to network changes.
    
4. Can find alternative paths when network topology changes.
    

Disadvantages:

1. More complex to configure.
    
2. Requires CPU and memory.
    
3. Routing updates consume network resources.
    

### Static vs Dynamic Routing

|Static Routing|Dynamic Routing|
|---|---|
|Manual configuration.|Automatic route learning.|
|Suitable for small networks.|Suitable for large networks.|
|Lower resource usage.|Higher resource usage.|
|Does not automatically adapt to failures.|Can adapt to network changes.|
|Simple to configure initially.|More complex.|

## 3.7 Important Routing Protocols

A routing protocol allows routers to exchange routing information.

|Protocol|Full Form|Type|
|---|---|---|
|RIP|Routing Information Protocol|Distance Vector|
|OSPF|Open Shortest Path First|Link State|
|BGP|Border Gateway Protocol|Path Vector|
|IS-IS|Intermediate System to Intermediate System|Link State|

### 1. RIP

- Distance-vector routing protocol.
    
- Uses hop count as its routing metric.
    
- Maximum usable hop count is 15.
    
- A hop count of 16 represents an unreachable destination.
    

### 2. OSPF

- Link-state routing protocol.
    
- Uses network topology information.
    
- Uses Dijkstra's Shortest Path First algorithm.
    
- Commonly used within large IP networks.
    

### 3. BGP

- Path-vector routing protocol.
    
- Used for routing between Autonomous Systems.
    
- Plays an important role in inter-domain Internet routing.
    

## 3.8 Autonomous System (AS)

An Autonomous System is a group of IP networks managed under a common routing administration and coordinated routing policy.

Routing protocols are divided into two categories:

```
              ROUTING PROTOCOLS
                      |
             +--------+--------+
             |                 |
             v                 v
            IGP               EGP
             |                 |
             v                 v
        Within an AS      Between ASes
             |                 |
          RIP/OSPF            BGP
```

- IGP: Interior Gateway Protocol, used within an Autonomous System.
    
- EGP: Exterior Gateway Protocol, used between Autonomous Systems.
    
- BGP is the principal modern inter-domain routing protocol.
    

## 3.9 Distance-Vector Routing

Definition: In distance-vector routing, routers exchange routing information with their neighbouring routers to determine suitable paths.

Working:

1. Each router knows its directly connected networks.
    
2. Routers exchange routing information with neighbours.
    
3. Each router calculates routes using received information.
    
4. Routing tables are updated when routes or their costs change.
    

Example: RIP.

Algorithm family: Bellman-Ford.

## 3.10 Link-State Routing

Definition: In link-state routing, routers build a network topology database using information about the state and cost of network links.

Working:

1. Routers discover neighbouring routers.
    
2. They identify the state and cost of their links.
    
3. Link-state information is distributed through the routing domain.
    
4. Each router builds a topology database.
    
5. Each router calculates shortest paths using Dijkstra's algorithm.
    

Example: OSPF.

### Distance Vector vs Link State

|Distance Vector|Link State|
|---|---|
|Exchanges route information with neighbours.|Distributes link-state information.|
|Has a more limited view of network topology.|Builds a topology database.|
|Uses Bellman-Ford algorithm family.|Uses Dijkstra's algorithm.|
|Example: RIP.|Example: OSPF.|

## 3.11 Routing vs Forwarding

|Routing|Forwarding|
|---|---|
|Selects the path.|Transfers packets to the next hop.|
|Uses routing information and algorithms.|Uses a forwarding table.|
|Determines where the packet should go.|Performs the actual local packet transfer.|

## 3.12 Routing Loop

A routing loop occurs when packets repeatedly travel between routers instead of reaching their destination.

Effects:

1. Wastes bandwidth.
    
2. Increases network congestion.
    
3. Causes packet delay and loss.
    

Routing protocols use mechanisms to reduce routing loops.

---

# 4. CONGESTION CONTROL

## 4.1 Definition

Congestion occurs when too many packets enter a network and the network cannot handle them efficiently because of limited bandwidth, buffer space or processing capacity.

In simple words, when network traffic exceeds network capacity, congestion occurs.

## 4.2 Diagram

```
 Computer A ----\
 Computer B -----+----> +----------------+
 Computer C ----/       | Router         |
                        | Full Buffer    |
                        +----------------+
                                |
                                v
                         Limited Capacity
                              Link
```

## 4.3 Causes of Congestion

1. Excessive network traffic.
    
2. Limited bandwidth.
    
3. Insufficient router buffer space.
    
4. Slow processing of packets.
    
5. Too many retransmissions.
    
6. Sudden traffic bursts.
    

## 4.4 Effects of Congestion

1. Packet loss.
    
2. Increased delay.
    
3. Reduced throughput.
    
4. Increased retransmissions.
    
5. Poor network performance.
    

## 4.5 Flow Control vs Congestion Control

|Flow Control|Congestion Control|
|---|---|
|Protects the receiver from a fast sender.|Protects the network from excessive traffic.|
|Focuses on sender-receiver capacity.|Focuses on network capacity.|
|Controls the rate at which the receiver can accept data.|Controls excessive traffic entering the network.|
|Example: TCP receiver window.|Example: TCP congestion window and ECN.|

## 4.6 Types of Congestion Control

There are two major approaches:

```
           CONGESTION CONTROL
                    |
          +---------+---------+
          |                   |
          v                   v
      Open-Loop            Closed-Loop
      Prevention           Feedback
```

### A. Open-Loop Congestion Control

Definition: Open-loop congestion control attempts to prevent congestion before it occurs without depending on feedback about the current congestion state.

Techniques:

1. Retransmission policy.
    
2. Window policy.
    
3. Acknowledgement policy.
    
4. Packet-discard policy.
    
5. Admission policy.
    

### B. Closed-Loop Congestion Control

Definition: Closed-loop congestion control detects existing congestion and uses feedback to reduce it.

Techniques:

1. Choke packets.
    
2. Backpressure.
    
3. Explicit Congestion Notification (ECN).
    
4. Load shedding.
    

### Open-Loop vs Closed-Loop

|Open-Loop|Closed-Loop|
|---|---|
|Prevents congestion.|Detects and controls congestion.|
|No current congestion feedback required.|Uses feedback.|
|Preventive approach.|Feedback-based approach.|
|Uses predefined policies.|Adjusts based on network conditions.|

## 4.7 Traffic Shaping

Traffic shaping is a technique used to control the rate at which packets enter a network.

It smooths sudden traffic bursts and helps reduce congestion.

Two important algorithms are:

1. Leaky Bucket.
    
2. Token Bucket.
    

### A. Leaky Bucket Algorithm

Definition: The Leaky Bucket Algorithm controls the outgoing rate of packets by sending them at a relatively fixed rate.

Diagram:

```
       Bursty Input
             |
             v
      +--------------+
      |    Bucket    |
      |    / Queue   |
      +--------------+
             |
             v
       Fixed-Rate Output
             |
             v
           Network
```

Working:

1. Incoming packets enter the bucket or buffer.
    
2. Packets leave at a fixed configured rate.
    
3. Packets wait in the queue if the output is busy.
    
4. If the buffer becomes full, additional packets are discarded.
    
5. It smooths bursty network traffic.
    

Advantages:

- Controls output rate.
    
- Smooths traffic bursts.
    
- Helps prevent sudden traffic overload.
    

Limitation: Packets may be discarded when the buffer becomes full.

### B. Token Bucket Algorithm

Definition: The Token Bucket Algorithm controls the average transmission rate while allowing controlled bursts of data.

Diagram:

```
      Token Generator
             |
             v
       +-------------+
       |Token Bucket |
       +-------------+
             |
             v
      Packet Transmission
             |
             v
           Network
```

Working:

1. Tokens are generated at a fixed rate.
    
2. Tokens are stored in the bucket.
    
3. A packet requires sufficient tokens for transmission.
    
4. Accumulated tokens allow bursts of packets.
    
5. If insufficient tokens are available, packets wait or are handled according to the configured policy.
    

Advantages:

- Controls average traffic rate.
    
- Allows controlled bursts.
    
- Suitable for variable network traffic.
    

### Leaky Bucket vs Token Bucket

|Leaky Bucket|Token Bucket|
|---|---|
|Fixed-rate output.|Allows controlled bursts.|
|Smooths traffic.|Controls average traffic rate.|
|Excess packets may be dropped if the buffer is full.|Packets may wait or be dropped depending on policy.|
|Does not preserve accumulated permission to transmit bursts.|Accumulated tokens permit bursts.|

## 4.8 Choke Packet

A choke packet is a special control packet sent by a congested router to the source, asking it to reduce its transmission rate.

Working:

1. Router detects congestion.
    
2. It sends a choke packet to the source.
    
3. Source receives the notification.
    
4. Source reduces its sending rate.
    

## 4.9 Backpressure

Backpressure is a congestion-control technique in which a congested router signals upstream routers to reduce or stop sending traffic.

```
Source -> Router A -> Router B -> Router C
                     |
                     v
                  Congested
                     |
                     v
             Signal upstream routers
```

## 4.10 Load Shedding

Load shedding is a technique in which a router deliberately discards selected packets when it cannot handle all incoming traffic.

It helps reduce overload and preserve network operation.

## 4.11 Explicit Congestion Notification (ECN)

ECN is a mechanism that allows network devices to indicate congestion by marking packets instead of necessarily dropping them.

Working:

1. A router detects congestion.
    
2. It marks an ECN-capable packet.
    
3. Congestion information is communicated to the sender.
    
4. The sender reduces its transmission rate.
    

## 4.12 Admission Control

Admission control decides whether a new traffic flow or connection should be allowed into a network.

If sufficient network resources are unavailable, the new request may be rejected to prevent congestion.

---

# 5. INTERNETWORKING

## 5.1 Definition

Internetworking is the process of connecting two or more independent networks so that devices on different networks can communicate with each other.

It is mainly achieved using routers and common network protocols such as IP.

## 5.2 Diagram

```
       Network A                 Network B
    +-------------+           +-------------+
    | Computers   |           | Computers   |
    +-------------+           +-------------+
           |                         |
           v                         v
       +--------+               +--------+
       | Router |===============| Router |
       +--------+    Internet   +--------+
```

## 5.3 Need for Internetworking

1. Connects different networks.
    
2. Allows communication between distant computers.
    
3. Enables resource sharing.
    
4. Supports global communication.
    
5. Allows different network technologies to work together.
    

## 5.4 Components of Internetworking

1. Hosts: Computers, servers and other end devices.
    
2. Routers: Forward packets between networks.
    
3. Communication Links: Carry packets between devices.
    
4. Network Protocols: Define addressing, packet formats and communication.
    
5. Gateways: Provide translation between different protocols or systems when required.
    

## 5.5 Working of Internetworking

Suppose a computer in Network A wants to communicate with a server in Network B.

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
 Internet / Other Networks
    |
    v
 Router B
    |
    v
 Server B
```

Steps:

1. Computer A checks the destination IP address.
    
2. If the destination is outside its local subnet, it sends the packet to its default gateway.
    
3. Router A checks the destination address.
    
4. Router A selects the next hop using its routing table.
    
5. Intermediate routers forward the packet toward Network B.
    
6. Router B forwards the packet into the destination network.
    
7. Server B receives the packet.
    

At every router, the incoming link-layer frame is processed and a new frame is created for the next link. The IP packet is forwarded toward the destination.

## 5.6 Challenges in Internetworking

1. Different network technologies.
    
2. Different MTU sizes.
    
3. Addressing and routing requirements.
    
4. Network congestion.
    
5. Security between interconnected networks.
    

---

# 6. TCP/IP MODEL

## 6.1 Definition

TCP/IP is a set of communication protocols used for data transmission over the Internet.

TCP/IP stands for Transmission Control Protocol / Internet Protocol.

It is commonly represented using four layers.

## 6.2 TCP/IP Model Diagram

```
+------------------------------+
|  4. Application Layer        |
+------------------------------+
|  3. Transport Layer          |
+------------------------------+
|  2. Internet Layer           |
+------------------------------+
|  1. Network Access / Link    |
+------------------------------+
```

## 6.3 Functions of TCP/IP Layers

### 1. Application Layer

- Provides services to user applications.
    
- Handles application-level communication.
    
- Protocols: HTTP, HTTPS, FTP, SMTP, DNS and SSH.
    

### 2. Transport Layer

- Provides process-to-process communication.
    
- Performs segmentation and reassembly.
    
- Uses port numbers.
    
- TCP provides reliable, ordered data delivery.
    
- UDP provides connectionless communication without TCP-style reliability.
    

### 3. Internet Layer

- Provides logical addressing.
    
- Performs packet routing and forwarding.
    
- Protocols: IPv4, IPv6 and ICMP.
    

### 4. Network Access / Link Layer

- Handles transmission over the local network.
    
- Performs framing and link-level communication.
    
- Examples: Ethernet, Wi-Fi and PPP.
    

## 6.4 TCP/IP vs OSI Model

|OSI Model|TCP/IP Model|
|---|---|
|Has 7 layers.|Commonly represented using 4 layers.|
|Reference model.|Practical protocol suite and commonly taught model.|
|Application, Presentation and Session are separate.|Their functions are generally combined into Application.|
|Network Layer.|Internet Layer.|
|Data Link and Physical are separate.|Combined into Network Access/Link.|

## 6.5 TCP vs IP

|TCP|IP|
|---|---|
|Transport Layer protocol.|Network/Internet Layer protocol.|
|Provides reliable, ordered byte-stream delivery.|Provides best-effort packet delivery.|
|Uses port numbers.|Uses IP addresses.|
|Handles retransmission and flow control.|Handles addressing and routing.|

## 6.6 TCP vs UDP

|TCP|UDP|
|---|---|
|Connection-oriented.|Connectionless.|
|Reliable data delivery.|Does not provide TCP-style reliability.|
|Maintains data order.|Does not guarantee data order.|
|Uses acknowledgements and retransmissions.|No TCP-style acknowledgements and retransmissions.|
|More protocol overhead.|Less protocol overhead.|

## 6.7 Encapsulation in TCP/IP

Encapsulation means adding protocol information at each layer as data moves down the network stack.

```
       Application Data
              |
              v
          TCP / UDP
              |
              v
      Segment / Datagram
              |
              v
          IP Header
              |
              v
           IP Packet
              |
              v
         Link Header
              |
              v
            Frame
              |
              v
       Physical Transmission
```

At the receiver, the process is reversed. It is called decapsulation.

### Protocol Data Units (PDU)

|Layer|Data Unit|
|---|---|
|Application|Data / Message|
|Transport|TCP Segment / UDP Datagram|
|Internet|IP Packet|
|Network Access|Frame|
|Physical transmission|Bits / Signals|

---

# 7. IP PACKET

## 7.1 Definition

An IP packet is a unit of data transmitted using the Internet Protocol. It contains a header and a payload.

The IP header contains control information, including source and destination IP addresses.

Diagram:

```
       IP PACKET
+----------------------+
|      IP Header       |
+----------------------+
|       Payload        |
|     (Actual Data)    |
+----------------------+
```

There are two main versions:

1. IPv4 packet.
    
2. IPv6 packet.
    

---

# 8. IPv4 PACKET FORMAT

## 8.1 Definition

IPv4 is Internet Protocol Version 4. It uses 32-bit addresses to identify source and destination interfaces.

An IPv4 header has a minimum size of 20 bytes and a maximum size of 60 bytes.

## 8.2 IPv4 Header Diagram

```
       0                 15                31
       +---------+---------+-----+----+------+
       | Version |   IHL   |DSCP |ECN | Total|
       | 4 bits  | 4 bits  |6 bit|2bit| Length|
       +---------+---------+-----+----+------+
       | Identification  |Flags| Fragment    |
       |     16 bits     |3 bit| Offset      |
       +-----------------+-----+--------------+
       | TTL  | Protocol | Header Checksum   |
       | 8 bit| 8 bits   | 16 bits           |
       +-------------------------------------+
       |       Source IP Address             |
       |             32 bits                 |
       +-------------------------------------+
       |     Destination IP Address          |
       |             32 bits                 |
       +-------------------------------------+
       |       Options and Padding            |
       |          (If present)                |
       +-------------------------------------+
       |             Payload                  |
       +-------------------------------------+
```

## 8.3 Important IPv4 Header Fields

|Field|Size|Function|
|---|---|---|
|Version|4 bits|Identifies the IP version. IPv4 = 4.|
|IHL|4 bits|Specifies the header length.|
|DSCP|6 bits|Supports differentiated traffic treatment.|
|ECN|2 bits|Supports explicit congestion notification.|
|Total Length|16 bits|Specifies total packet size in bytes.|
|Identification|16 bits|Identifies fragments of the original packet.|
|Flags|3 bits|Controls fragmentation.|
|Fragment Offset|13 bits|Specifies fragment position.|
|TTL|8 bits|Limits packet lifetime.|
|Protocol|8 bits|Identifies the upper-layer protocol.|
|Header Checksum|16 bits|Detects errors in the IPv4 header.|
|Source Address|32 bits|IP address of sender.|
|Destination Address|32 bits|IP address of receiver.|
|Options and Padding|Variable|Provides optional functions and alignment.|

## 8.4 Important Fields Explained

**Version:** Specifies the IP version. IPv4 uses value 4.

**IHL:** Specifies the length of the IPv4 header in 32-bit words.

**Total Length:** Specifies the total size of the entire IPv4 packet, including header and payload.

**Identification:** Identifies fragments belonging to the same original packet.

**Flags:** Controls fragmentation. Important bits include:

- DF: Don't Fragment.
    
- MF: More Fragments.
    

**Fragment Offset:** Identifies the position of a fragment in the original packet. It is measured in units of 8 bytes.

**TTL (Time To Live):** A router reduces the TTL value while forwarding the packet. When it reaches zero, the router discards the packet. It prevents endless circulation.

**Protocol:** Identifies the upper-layer protocol.

Common values:

|Protocol|Number|
|---|---|
|ICMP|1|
|TCP|6|
|UDP|17|

**Header Checksum:** Detects errors in the IPv4 header only, not in the payload.

**Source Address:** Identifies the sender's IP address.

**Destination Address:** Identifies the receiver's IP address.

## 8.5 IPv4 Fragmentation

IPv4 fragmentation divides a large IP packet into smaller fragments when necessary to accommodate a link's MTU.

Diagram:

```
       Original Packet
              |
              v
        Fragmentation
              |
         +----+----+
         |         |
         v         v
     Fragment 1  Fragment 2
         |         |
         +----+----+
              |
              v
       Destination Reassembles
```

Important fields:

1. Identification: Identifies fragments of the original packet.
    
2. DF: Indicates that the packet must not be fragmented.
    
3. MF: Indicates that more fragments follow.
    
4. Fragment Offset: Indicates the fragment's position.
    

**Important:** Reassembly takes place at the destination.

## 8.6 IPv4 Packet Characteristics

- Address size = 32 bits.
    
- Header size = 20–60 bytes.
    
- Maximum total packet length = 65,535 bytes.
    
- Supports fragmentation.
    
- Has a header checksum.
    
- Provides connectionless, best-effort delivery.
    

---

# 9. IP ADDRESSING

## 9.1 Definition

An IP address is a logical address assigned to a network interface to identify it and enable communication over an IP network.

There are two main versions:

1. IPv4.
    
2. IPv6.
    

## 9.2 IPv4 Address

IPv4 uses a 32-bit address divided into four octets.

Each octet contains 8 bits and has a decimal value from 0 to 255.

Example:

```
192.168.1.10

192      .168      .1        .10
8 bits   8 bits    8 bits    8 bits

Total = 32 bits
```

### Binary Representation

```
192       168       1         10
11000000.10101000.00000001.00001010
```

## 9.3 Classful IPv4 Addressing

Historically, IPv4 addresses were divided into five classes.

|Class|First Octet Range|Default Mask|Purpose|
|---|---|---|---|
|A|1–126|255.0.0.0|Large networks|
|B|128–191|255.255.0.0|Medium networks|
|C|192–223|255.255.255.0|Small networks|
|D|224–239|Not applicable|Multicast|
|E|240–255|Not applicable|Reserved/experimental|

Important points:

- 127 is reserved for loopback.
    
- Class A, B and C have traditional network and host portions.
    
- Modern networks use CIDR instead of classful addressing.
    

## 9.4 Network ID and Host ID

An IP address has two logical parts:

**Network ID:** Identifies the network.

**Host ID:** Identifies an address within that network.

Example:

```
IP Address: 192.168.1.10/24

Network ID = 192.168.1
Host ID    = 10
```

The prefix length determines the number of network bits.

## 9.5 CIDR (Classless Inter-Domain Routing)

CIDR is a method of assigning IP addresses using variable-length network prefixes rather than fixed address classes.

Example:

```
192.168.1.10/24
```

`/24` means the first 24 bits are the network portion.

Advantages:

1. Allows flexible subnet sizes.
    
2. Reduces IP address wastage.
    
3. Supports route aggregation.
    
4. Is used in modern IP addressing and routing.
    

## 9.6 Subnet Mask

A subnet mask is used to identify the network portion and host portion of an IPv4 address.

Example:

```
IP Address : 192.168.1.10/24
Subnet Mask: 255.255.255.0
```

Here:

- Network bits = 24.
    
- Host bits = 8.
    
- Network Address = 192.168.1.0.
    
- Broadcast Address = 192.168.1.255.
    

## 9.7 Network Address and Broadcast Address

**Network Address:** Identifies the subnet itself. The host portion contains all zeros.

**Broadcast Address:** Identifies all hosts on a conventional IPv4 subnet. The host portion contains all ones.

## 9.8 Private IPv4 Address Ranges

Private IP addresses are commonly used in homes, colleges and offices.

|Range|CIDR|
|---|---|
|10.0.0.0 – 10.255.255.255|10.0.0.0/8|
|172.16.0.0 – 172.31.255.255|172.16.0.0/12|
|192.168.0.0 – 192.168.255.255|192.168.0.0/16|

Private IP addresses are not directly routed across the public Internet.

## 9.9 Public vs Private IP

|Public IP|Private IP|
|---|---|
|Used for globally routed communication.|Used within private networks.|
|Globally coordinated.|Can be reused in separate private networks.|
|Public reachability depends on routing and security configuration.|Generally uses NAT or a suitable gateway for public IPv4 access.|

## 9.10 NAT (Network Address Translation)

NAT is a technique that translates IP addresses, commonly allowing devices using private IPv4 addresses to communicate through a public IPv4 address.

Diagram:

```
 Computer 1: 192.168.1.10
 Computer 2: 192.168.1.11
 Computer 3: 192.168.1.12
              |
              v
          NAT Router
              |
              v
        Public IP Address
              |
              v
            Internet
```

NAT is commonly used in homes, colleges and offices to share public IPv4 connectivity.

---

# 10. IPV6 (INTERNET PROTOCOL VERSION 6)

## 10.1 Definition

IPv6 is the newer version of the Internet Protocol, designed mainly to overcome the shortage of IPv4 addresses and improve the IP packet header design.

IPv4 = 32 bits.

IPv6 = 128 bits.

## 10.2 Need for IPv6

1. Shortage of IPv4 addresses.
    
2. Increasing number of Internet-connected devices.
    
3. Need for a larger address space.
    
4. Improved base-header design.
    
5. Better support for modern network communication.
    

## 10.3 Characteristics of IPv6

1. Uses 128-bit addresses.
    
2. Provides a very large address space.
    
3. Uses hexadecimal notation.
    
4. Has a fixed 40-byte base header.
    
5. Does not use broadcast addresses.
    
6. Supports multicast and anycast.
    
7. Does not have a base-header checksum.
    
8. Routers do not fragment IPv6 packets.
    
9. Uses extension headers for optional information.
    
10. Supports automatic address configuration.
    

## 10.4 IPv6 Address Format

An IPv6 address is written as eight groups of hexadecimal digits separated by colons.

Example:

```
2001:0DB8:0000:0000:0000:0000:0000:0001
```

Short form:

```
2001:DB8::1
```

### IPv6 Address Shortening Rules

Rule 1: Leading zeros within a group can be removed.

Example:

```
0db8 -> db8
00ff -> ff
0001 -> 1
```

Rule 2: One consecutive sequence of zero groups can be replaced with `::`.

Example:

```
2001:0db8:0000:0000:0000:0000:0000:0001
```

Short form:

```
2001:db8::1
```

Important: `::` can be used only once in a single IPv6 address.

## 10.5 Types of IPv6 Addresses

### 1. Unicast

Identifies a single network interface. Data is delivered to that interface.

### 2. Multicast

Identifies a group of interfaces. Data is delivered to group members.

### 3. Anycast

Identifies multiple interfaces, but data is delivered to one suitable member, usually according to routing.

**Important:** IPv6 does not support broadcast.

## 10.6 Important IPv6 Address Ranges

|Address / Prefix|Purpose|
|---|---|
|2000::/3|Global Unicast|
|fe80::/10|Link-Local|
|fc00::/7|Unique Local|
|ff00::/8|Multicast|
|::1|Loopback|
|::|Unspecified Address|

## 10.7 IPv6 Header Format

IPv6 has a fixed 40-byte base header.

Diagram:

```
+----------+---------------+----------------+
| Version  | Traffic Class | Flow Label     |
| 4 bits   | 8 bits        | 20 bits        |
+----------+---------------+----------------+
| Payload Length | Next Header | Hop Limit   |
| 16 bits        | 8 bits      | 8 bits      |
+-------------------------------------------+
|                                           |
|             Source Address                |
|                128 bits                   |
|                                           |
+-------------------------------------------+
|                                           |
|          Destination Address              |
|                128 bits                   |
|                                           |
+-------------------------------------------+
|              Payload                      |
+-------------------------------------------+
```

The payload may contain extension headers followed by upper-layer data.

## 10.8 IPv6 Header Fields

|Field|Size|Function|
|---|---|---|
|Version|4 bits|Identifies IPv6. Value = 6.|
|Traffic Class|8 bits|Indicates traffic priority/handling.|
|Flow Label|20 bits|Identifies packets belonging to a flow.|
|Payload Length|16 bits|Specifies the length after the base header.|
|Next Header|8 bits|Identifies the next extension header or upper-layer protocol.|
|Hop Limit|8 bits|Limits the number of router hops.|
|Source Address|128 bits|Identifies the sender.|
|Destination Address|128 bits|Identifies the receiver.|

### Important IPv6 Header Facts

- Fixed base header size = 40 bytes.
    
- No IHL field because header size is fixed.
    
- No base-header checksum.
    
- Hop Limit is used instead of TTL.
    
- Optional information is carried through extension headers.
    
- Routers do not fragment IPv6 packets.
    
- The source can perform fragmentation using a Fragment extension header.
    

## 10.9 IPv6 Extension Headers

IPv6 uses extension headers to carry optional information outside the fixed base header.

Examples:

1. Fragment Header.
    
2. Routing Header.
    
3. Hop-by-Hop Options Header.
    
4. Destination Options Header.
    

The Next Header field identifies the next extension header or upper-layer protocol.

## 10.10 IPv4 vs IPv6

This is one of the most important questions for examination.

|Feature|IPv4|IPv6|
|---|---|---|
|Address Size|32 bits|128 bits|
|Address Format|Decimal|Hexadecimal|
|Example|192.168.1.1|2001:db8::1|
|Header Size|20–60 bytes|Fixed 40 bytes|
|Address Space|Smaller|Very large|
|Checksum|Present|Absent|
|Broadcast|Supported|Not supported|
|Fragmentation|Routers may fragment unless prohibited|Routers do not fragment|
|Address Resolution|ARP commonly used|Neighbor Discovery|
|Configuration|Manual/DHCP etc.|SLAAC/DHCPv6 etc.|
|Header Design|Variable length|Fixed base header|
|Hop Control|TTL|Hop Limit|

## 10.11 Advantages of IPv6

1. Large address space.
    
2. Simplified fixed-length base header.
    
3. Better support for extension headers.
    
4. No broadcast addressing.
    
5. Supports modern network requirements.
    
6. Supports automatic address configuration.
    

## 10.12 Limitations of IPv6

1. IPv4 and IPv6 are not directly interoperable without suitable mechanisms.
    
2. Network devices and software may require upgrades.
    
3. Migration can require additional planning.
    
4. IPv6 does not automatically make a network secure.
    

## 10.13 IPv4-to-IPv6 Transition Mechanisms

Three important methods are:

### 1. Dual Stack

A device supports both IPv4 and IPv6.

```
              Device
                 |
           +-----+-----+
           |           |
           v           v
          IPv4        IPv6
           |           |
           +-----+-----+
                 |
               Network
```

### 2. Tunneling

One IP protocol's packets are carried inside packets of another protocol to cross a network that does not natively support the carried protocol.

### 3. Translation

Converts communication between IPv4 and IPv6 networks where necessary.

Example: NAT64.

---

# 11. IMPORTANT FORMULAS AND NUMERICALS

These formulas are important for IP addressing and subnetting questions.

## 11.1 IPv4 Address Size

IPv4 uses 32 bits.

Number of possible bit patterns:

2^32 = 4,294,967,296

## 11.2 IPv6 Address Size

IPv6 uses 128 bits.

Number of possible bit patterns:

2^128 ≈ 3.4 × 10^38

## 11.3 Number of Host Bits

h = 32 - Prefix Length

## 11.4 Total Number of Addresses

Total addresses = 2^h

## 11.5 Usable IPv4 Host Addresses

For conventional IPv4 subnets:

Usable hosts = 2^h - 2

The formula excludes the network and broadcast addresses. Special prefixes such as /31 have different rules.

## 11.6 Example: /24 Subnet

Given:

IP Address = 192.168.1.10/24

Step 1: Find host bits.

h = 32 - 24 = 8

Step 2: Find total addresses.

2^8 = 256

Step 3: Find usable hosts.

2^8 - 2 = 254

Answer: 254 usable host addresses.

## 11.7 Example: /26 Subnet

Given:

IP Address = 192.168.1.70/26

Step 1: Subnet mask.

255.255.255.192

Step 2: Host bits.

h = 32 - 26 = 6

Step 3: Total addresses.

2^6 = 64

Step 4: Usable hosts.

2^6 - 2 = 62

Step 5: Block size.

256 - 192 = 64

Subnet ranges:

0–63  
64–127  
128–191  
192–255

Since 70 belongs to 64–127:

|Detail|Answer|
|---|---|
|Network Address|192.168.1.64|
|First Usable Host|192.168.1.65|
|Last Usable Host|192.168.1.126|
|Broadcast Address|192.168.1.127|
|Total Addresses|64|
|Usable Hosts|62|

---

# 12. IMPORTANT EXAM QUESTIONS

## A. Long-Answer Questions (5–10 Marks)

1. Define the Network Layer and explain its major functions with a diagram.
    
2. Explain point-to-point networks with a suitable diagram.
    
3. Differentiate datagram networks and virtual-circuit networks.
    
4. Define routing. Explain static and dynamic routing.
    
5. Explain distance-vector and link-state routing.
    
6. Explain RIP, OSPF and BGP routing protocols.
    
7. Define congestion. Explain its causes and effects.
    
8. Explain open-loop and closed-loop congestion control.
    
9. Explain Leaky Bucket and Token Bucket algorithms with diagrams.
    
10. Define internetworking and explain its need and working.
    
11. Explain the TCP/IP model with a diagram and functions of each layer.
    
12. Draw and explain the IPv4 packet format.
    
13. Explain IPv4 addressing and classful addressing.
    
14. Explain CIDR and subnetting with an example.
    
15. Draw and explain the IPv6 header format.
    
16. Explain IPv6 addressing and its address types.
    
17. Differentiate IPv4 and IPv6.
    
18. Explain IPv4-to-IPv6 transition mechanisms.
    

## B. Short-Answer Questions (2–3 Marks)

1. What is a Network Layer?
    
2. Define point-to-point network.
    
3. What is a datagram network?
    
4. What is a virtual-circuit network?
    
5. Define routing and forwarding.
    
6. What is a routing table?
    
7. Define routing metrics.
    
8. What is an Autonomous System?
    
9. What is congestion?
    
10. Differentiate flow control and congestion control.
    
11. What is a choke packet?
    
12. Define backpressure.
    
13. What is load shedding?
    
14. What is ECN?
    
15. Define internetworking.
    
16. What is encapsulation?
    
17. What is an IP packet?
    
18. What is CIDR?
    
19. What is a subnet mask?
    
20. What is NAT?
    
21. What is TTL?
    
22. What is fragmentation?
    
23. Define IPv6 anycast.
    
24. What is an IPv6 extension header?
    
25. What are the IPv4 and IPv6 address lengths?
    

## C. Numerical Questions

Practise:

1. Finding the number of host bits.
    
2. Finding total and usable addresses.
    
3. Calculating subnet masks.
    
4. Finding network and broadcast addresses.
    
5. Identifying IPv4 address classes.
    
6. Calculating the size of IPv4 and IPv6 address spaces.
    
7. Comparing route costs using link weights.
    

---

# 13. LAST-MINUTE REVISION SHEET

## Network Layer

- OSI Layer 3.
    
- Data unit: Packet.
    
- Main device: Router.
    
- Functions: Addressing, routing and forwarding.
    

## Point-to-Point

- Connects two endpoints.
    
- Commonly used between routers.
    
- Datagram: Independent packets.
    
- Virtual Circuit: Established logical path.
    

## Routing

- Static: Manual configuration.
    
- Dynamic: Automatic route learning.
    
- RIP: Distance Vector.
    
- OSPF: Link State.
    
- BGP: Path Vector.
    
- Dijkstra: Shortest-path algorithm.
    
- Bellman-Ford: Distance-vector algorithm family.
    

## Congestion Control

- Congestion: Traffic exceeds network capacity.
    
- Flow Control: Protects the receiver.
    
- Congestion Control: Protects the network.
    
- Open Loop: Prevention.
    
- Closed Loop: Feedback.
    
- Leaky Bucket: Fixed-rate output.
    
- Token Bucket: Controlled bursts.
    
- Choke Packet: Requests source to reduce traffic.
    
- Backpressure: Signals upstream routers.
    
- Load Shedding: Discards selected packets.
    
- ECN: Congestion marking.
    

## Internetworking

- Connects independent networks.
    
- Routers forward packets between networks.
    
- Uses IP addressing and routing.
    

## TCP/IP Model

1. Application.
    
2. Transport.
    
3. Internet.
    
4. Network Access/Link.
    

## IPv4

- Address = 32 bits.
    
- Header = 20–60 bytes.
    
- TTL limits packet lifetime.
    
- Header checksum present.
    
- Fragmentation supported.
    

## IPv4 Classes

- A: 1–126.
    
- B: 128–191.
    
- C: 192–223.
    
- D: 224–239.
    
- E: 240–255.
    

## IPv6

- Address = 128 bits.
    
- Base header = 40 bytes.
    
- Header checksum absent.
    
- No broadcast.
    
- Routers do not fragment.
    
- Uses hexadecimal notation.
    
- Supports extension headers.
    

---

# 14. FINAL STUDY STRATEGY

Recommended order for preparing this unit:

1. IPv4 and IPv6 – Learn the header diagrams and differences.
    
2. IP Addressing – Study classes, CIDR, subnet masks and practise numericals.
    
3. TCP/IP Model – Learn the four layers and encapsulation.
    
4. Routing – Understand routing types, protocols and algorithms.
    
5. Congestion Control – Learn causes, effects, open/closed loop and both bucket algorithms.
    
6. Point-to-Point Networks – Prepare datagram and virtual circuit.
    
7. Internetworking – Learn definition, components and working.
    

**Final examination advice:** For every long-answer question, try to write the definition first, followed by a labelled diagram, working or explanation, and important characteristics. For comparison questions, use tables. For algorithms, practise the diagrams and write the working in steps.

Do not waste time memorising every sentence. Understand the concepts and practise reproducing the diagrams, definitions, comparison tables and important formulas.
