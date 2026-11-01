
---

# UNIT 3 – NETWORK LAYER

Computer Networks · Mid-Sem Exam Notes · Concise & Complete

These notes cover every topic in your syllabus with important definitions, diagrams, comparisons and examples. The focus is on what you need to understand and reproduce in the examination.

## 1. Network Layer

Definition: The Network Layer is the third layer of the OSI model. It is responsible for delivering data packets from source to destination across different interconnected networks.

Main functions:

1. Logical addressing: Provides IP addresses to identify source and destination devices.
    
2. Routing: Determines the best path for data transmission.
    
3. Forwarding: Transfers packets to the appropriate next router.
    
4. Internetworking: Connects different networks.
    
5. Fragmentation: IPv4 can divide packets into smaller fragments when required.
    

Diagram:

```
        OSI MODEL
   ┌─────────────────┐
   │ Application     │
   ├─────────────────┤
   │ Presentation    │
   ├─────────────────┤
   │ Session         │
   ├─────────────────┤
   │ Transport       │
   ├═════════════════┤
   │ Network Layer   │  ← Layer 3
   ├─────────────────┤
   │ Data Link       │
   ├─────────────────┤
   │ Physical        │
   └─────────────────┘
```

Data unit: Packet (IP datagram).

## 2. Point-to-Point Networks

Definition: A point-to-point network is a communication arrangement in which a direct link connects two devices or network nodes.

It is commonly used to connect two routers.

Diagram:

```
 Network A                 Network B
    |                         |
    v                         v
+---------+    Direct     +---------+
| Router A|===============| Router B|
+---------+     Link      +---------+
```

Characteristics:

- Connects two endpoints.
    
- Provides direct communication between connected devices.
    
- Does not require multiple stations to compete for the same link.
    
- Can use wired or wireless communication.
    
- Commonly used in WAN connections.
    

### Types of Network Service

Two important packet-network approaches are:

A. Datagram Network

In a datagram network, each packet is treated independently and may follow a different route.

```
             R2
           /    \
Source → R1      R4 → Destination
           \    /
             R3
```

- No connection setup is required at the network layer.
    
- Each packet contains destination address information.
    
- Packets may take different routes.
    
- Packets may arrive out of order.
    

Example: Internet Protocol (IP).

B. Virtual Circuit Network

A virtual-circuit network establishes a logical path before transmitting packets.

```
A → R1 → R2 → R3 → B
    <--- Logical Path --->
```

- Connection setup is required.
    
- All packets generally follow the same logical path.
    
- Routers maintain virtual-circuit information.
    
- Packets generally arrive in sequence.
    

Examples: ATM, Frame Relay and X.25.

### Datagram vs Virtual Circuit

|Datagram Network|Virtual Circuit|
|---|---|
|No connection setup|Connection setup required|
|Packets routed independently|Packets follow an established logical path|
|Different paths possible|Generally same path|
|Packets may arrive out of order|Packets generally arrive in order|
|Example: IP|Example: ATM|

## 3. Routing

Definition: Routing is the process of selecting the best available path for transferring data packets from source to destination through interconnected networks.

A router performs routing at the Network Layer.

### Diagram

```
              Router B
             /        \
Source → Router A      Router D → Destination
             \        /
              Router C
```

### Functions of Routing

1. Selects the appropriate path.
    
2. Uses destination IP addresses.
    
3. Maintains routing information.
    
4. Forwards packets toward the destination.
    
5. Finds alternative routes when supported and required.
    

### Routing Table

A routing table stores information about available routes.

|Destination|Next Hop|
|---|---|
|192.168.1.0/24|Direct|
|10.0.0.0/8|192.168.1.2|
|0.0.0.0/0|Default gateway|

Next Hop: The next router or device to which a packet should be forwarded.

### Types of Routing

A. Static Routing

Routes are manually configured by a network administrator.

Advantages:

- Simple for small networks.
    
- No routing update traffic.
    
- Provides manual control.
    

Disadvantages:

- Manual configuration is required.
    
- Difficult to manage in large networks.
    
- Does not automatically adapt to failures unless alternatives are configured.
    

B. Dynamic Routing

Routers automatically learn and update routes using routing protocols.

Advantages:

- Suitable for large networks.
    
- Automatically updates routes.
    
- Can find alternative paths when network topology changes.
    

Disadvantages:

- More complex.
    
- Requires CPU and memory.
    
- Routing updates consume network resources.
    

### Static vs Dynamic Routing

|Static Routing|Dynamic Routing|
|---|---|
|Manual configuration|Automatic route learning|
|Suitable for small networks|Suitable for large networks|
|Less resource usage|More resource usage|
|Does not automatically adapt|Can adapt to network changes|
|Simple|More complex|

### Important Routing Protocols

|Protocol|Full Form|Type / Function|
|---|---|---|
|RIP|Routing Information Protocol|Distance Vector|
|OSPF|Open Shortest Path First|Link State|
|BGP|Border Gateway Protocol|Path Vector|

Distance Vector Routing: Routers exchange routing information with their neighbouring routers. RIP is an example.

Link State Routing: Routers build a network topology database and calculate shortest paths using Dijkstra's algorithm. OSPF is an example.

Path Vector Routing: Maintains information about the path through Autonomous Systems. BGP is an example.

Important routing algorithms:

- Dijkstra's algorithm – Shortest path, used by OSPF.
    
- Bellman-Ford algorithm – Basis of distance-vector routing.
    

### Routing vs Forwarding

|Routing|Forwarding|
|---|---|
|Selects the path|Sends packet to next hop|
|Uses routing information|Uses forwarding table|
|Determines where packet should go|Performs actual packet transfer|

## 4. Congestion Control

### Definition

Congestion occurs when too many packets enter a network and the network cannot handle them efficiently due to limited bandwidth, buffer space or processing capacity.

In simple words, when network traffic exceeds network capacity, congestion occurs.

### Diagram

```
Computer A ──┐
Computer B ──┼──> +-------------+
Computer C ──┘    |   Router    |
                  | Full Buffer |
                  +-------------+
                         |
                         v
                    Slow Network
```

### Causes of Congestion

1. Excessive network traffic.
    
2. Limited bandwidth.
    
3. Insufficient router buffer space.
    
4. Slow processing of packets.
    
5. Too many retransmissions.
    

### Effects of Congestion

- Packet loss.
    
- Increased delay.
    
- Reduced throughput.
    
- Network performance degradation.
    
- Increased retransmissions.
    

### Types of Congestion Control

There are two main approaches:

A. Open-Loop Congestion Control

It attempts to prevent congestion before it occurs, without relying on feedback about the current congestion state.

Techniques:

- Retransmission policy.
    
- Window policy.
    
- Acknowledgement policy.
    
- Packet-discard policy.
    
- Admission policy.
    

B. Closed-Loop Congestion Control

It detects existing congestion and uses feedback to control network traffic.

Techniques:

- Choke packets.
    
- Backpressure.
    
- Explicit Congestion Notification (ECN).
    
- Load shedding.
    

### Open-Loop vs Closed-Loop

|Open-Loop|Closed-Loop|
|---|---|
|Prevents congestion|Detects and controls congestion|
|No congestion feedback required|Uses feedback|
|Preventive approach|Feedback-based approach|
|Uses predefined policies|Adjusts based on network conditions|

### Important Congestion Control Techniques

1. Leaky Bucket Algorithm

It controls the rate of outgoing packets by sending them at a fixed rate.

```
 Bursty Input
      |
      v
 +-----------+
 |  Bucket   |
 |  / Queue  |
 +-----------+
      |
      v
 Fixed-rate Output
      |
    Network
```

- Packets enter a buffer.
    
- Packets leave at a controlled rate.
    
- If the buffer becomes full, additional packets are discarded.
    
- It helps smooth bursty traffic.
    

2. Token Bucket Algorithm

It allows controlled bursts of data while controlling the average transmission rate.

```
 Token Generator
       |
       v
 +------------+
 |Token Bucket|
 +------------+
       |
       v
 Packet Transmission
       |
       v
     Network
```

- Tokens are generated at a fixed rate.
    
- Tokens are stored in the bucket.
    
- A packet needs sufficient tokens for transmission.
    
- Accumulated tokens allow bursts of packets.
    

Leaky Bucket vs Token Bucket

|Leaky Bucket|Token Bucket|
|---|---|
|Fixed-rate output|Allows controlled bursts|
|Smooths traffic|Controls average traffic rate|
|Excess packets may be dropped|Packets wait or are dropped if tokens are insufficient, depending on implementation|

3. Choke Packet: A congested router sends a special control packet to the source asking it to reduce its transmission rate.

4. Load Shedding: A router deliberately discards selected packets when it cannot handle all incoming traffic.

5. Backpressure: A congested router signals upstream routers to reduce or stop sending traffic.

Important difference:

- Flow control protects the receiver from a fast sender.
    
- Congestion control protects the network from excessive traffic.
    

## 5. Internetworking

Definition: Internetworking is the process of connecting two or more independent networks so that devices on different networks can communicate with each other.

It is mainly achieved using routers and common network protocols such as IP.

### Diagram

```
       Network A             Network B
     +-----------+         +-----------+
     | Computer  |         | Computer  |
     +-----------+         +-----------+
           |                     |
           v                     v
        Router A ============== Router B
                  Internet
```

### Need for Internetworking

1. Connects different networks.
    
2. Allows communication between distant computers.
    
3. Enables resource sharing.
    
4. Supports global communication.
    
5. Allows different network technologies to work together.
    

### Components

- Routers
    
- Hosts/computers
    
- Communication links
    
- Network protocols
    
- Gateways, where translation is required
    

## 6. TCP/IP Model

Definition: TCP/IP is a set of communication protocols used for data transmission over the Internet.

TCP/IP stands for Transmission Control Protocol / Internet Protocol.

It is commonly represented using four layers.

### TCP/IP Model Diagram

```
┌─────────────────────────────┐
│  4. Application Layer       │
├─────────────────────────────┤
│  3. Transport Layer         │
├─────────────────────────────┤
│  2. Internet Layer          │
├─────────────────────────────┤
│  1. Network Access / Link   │
└─────────────────────────────┘
```

### Functions of TCP/IP Layers

1. Application Layer

- Provides services to user applications.
    
- Handles application-level communication.
    
- Protocols: HTTP, HTTPS, FTP, SMTP, DNS.
    

2. Transport Layer

- Provides process-to-process communication.
    
- Performs segmentation and reassembly.
    
- Uses port numbers.
    
- TCP provides reliable delivery; UDP provides connectionless delivery without TCP-style reliability.
    

3. Internet Layer

- Provides logical addressing.
    
- Performs packet routing and forwarding.
    
- Protocols: IPv4, IPv6, ICMP.
    

4. Network Access/Link Layer

- Handles transmission over the local network.
    
- Performs framing and link-level communication.
    
- Examples: Ethernet and Wi-Fi.
    

### TCP/IP vs OSI Model

|OSI Model|TCP/IP Model|
|---|---|
|7 layers|4 layers commonly|
|Reference model|Practical protocol suite/model|
|Application, Presentation and Session are separate|Combined into Application|
|Network Layer|Internet Layer|
|Data Link and Physical separate|Combined into Network Access/Link|

### TCP and IP Difference

|TCP|IP|
|---|---|
|Transport Layer protocol|Internet/Network Layer protocol|
|Provides reliable, ordered data delivery|Provides best-effort packet delivery|
|Uses port numbers|Uses IP addresses|
|Handles retransmission and flow control|Handles addressing and routing|

## 7. IP Packet (IPv4)

Definition: An IP packet is a unit of data transmitted using the Internet Protocol. It contains a header and a payload.

```
       IPv4 PACKET
┌───────────────────────┐
│     IP Header         │
├───────────────────────┤
│       Payload         │
│    (Actual Data)      │
└───────────────────────┘
```

### IPv4 Header Format

The IPv4 header has a minimum size of 20 bytes and a maximum size of 60 bytes.

```
  0                 15                31
  +---------+-------+------------------+
  | Version |  IHL  | DSCP |ECN| Length|
  +---------+-------+------------------+
  | Identification  |Flags|Frag Offset |
  +-----------------+-----+------------+
  | TTL     |Protocol | Header Checksum|
  +------------------------------------+
  |          Source IP Address         |
  +------------------------------------+
  |       Destination IP Address       |
  +------------------------------------+
  |       Options (if present)         |
  +------------------------------------+
  |             Payload                |
  +------------------------------------+
```

### Important Header Fields

|Field|Function|
|---|---|
|Version|Specifies IP version (IPv4 = 4).|
|IHL|Specifies header length.|
|Total Length|Total size of the packet in bytes.|
|Identification|Identifies fragments of an original packet.|
|Flags|Controls fragmentation.|
|Fragment Offset|Identifies fragment position.|
|TTL|Limits packet lifetime and prevents endless looping.|
|Protocol|Identifies the upper-layer protocol, such as TCP or UDP.|
|Header Checksum|Detects errors in the IPv4 header.|
|Source Address|IP address of sender.|
|Destination Address|IP address of receiver.|

Remember: IPv4 uses 32-bit addresses and supports fragmentation by routers unless prohibited by the packet's fragmentation settings.

## 8. IP Addressing

### Definition

An IP address is a logical address assigned to a network interface to identify it and enable communication over an IP network.

There are two main versions:

1. IPv4
    
2. IPv6
    

### IPv4 Address

IPv4 uses a 32-bit address, divided into four octets. Each octet contains 8 bits.

Example:

```
192.168.1.10

192      .168      .1        .10
8 bits   8 bits    8 bits    8 bits

Total = 32 bits
```

### Classful IPv4 Addressing

Historically, IPv4 addresses were divided into five classes.

|Class|First Octet Range|Default Mask|Purpose|
|---|---|---|---|
|A|1–126|255.0.0.0|Large networks|
|B|128–191|255.255.0.0|Medium networks|
|C|192–223|255.255.255.0|Small networks|
|D|224–239|Not applicable|Multicast|
|E|240–255|Not applicable|Experimental/reserved|

Note: 127 is reserved for loopback. Modern networks use CIDR rather than classful addressing.

### Network ID and Host ID

An IP address has two logical parts:

- Network ID: Identifies the network.
    
- Host ID: Identifies an interface within that network.
    

Example:

```
IP Address: 192.168.1.10/24

Network ID = 192.168.1
Host ID    = 10
```

### CIDR (Classless Inter-Domain Routing)

CIDR represents an IP address with a prefix length.

Example:

```
192.168.1.10/24
```

`/24` means the first 24 bits are the network portion.

### Private IPv4 Address Ranges

|Range|CIDR|
|---|---|
|10.0.0.0 – 10.255.255.255|10.0.0.0/8|
|172.16.0.0 – 172.31.255.255|172.16.0.0/12|
|192.168.0.0 – 192.168.255.255|192.168.0.0/16|

Private IP addresses are commonly used in homes, colleges and offices.

## 9. IPv6 (Internet Protocol Version 6)

### Definition

IPv6 is the newer version of the Internet Protocol, designed mainly to overcome the shortage of IPv4 addresses and improve the IP packet header design.

IPv4 = 32 bits

IPv6 = 128 bits

### Need for IPv6

1. Shortage of IPv4 addresses.
    
2. Increasing number of Internet-connected devices.
    
3. Need for a larger address space.
    
4. Improved base-header design.
    
5. Better support for modern network communication.
    

### Characteristics of IPv6

- Uses 128-bit addresses.
    
- Provides a very large address space.
    
- Uses hexadecimal notation.
    
- Has a fixed 40-byte base header.
    
- Does not use broadcast addresses.
    
- Supports multicast and anycast.
    
- Does not have a base-header checksum.
    
- Routers do not fragment IPv6 packets.
    

### IPv6 Address Format

An IPv6 address is written as eight groups of hexadecimal digits.

Example:

```
2001:0DB8:0000:0000:0000:0000:0000:0001
```

Short form:

```
2001:DB8::1
```

Leading zeros in groups can be removed, and one consecutive sequence of zero groups can be replaced with `::`.

### Types of IPv6 Addresses

1. Unicast: Identifies a single network interface.

2. Multicast: Identifies a group of interfaces. Data is delivered to group members.

3. Anycast: Identifies multiple interfaces, but data is delivered to one suitable member, usually according to routing.

Important: IPv6 does not support broadcast.

### IPv6 Header Format

IPv6 has a fixed 40-byte base header.

```
+----------+---------------+----------------+
| Version  | Traffic Class | Flow Label     |
| 4 bits   | 8 bits        | 20 bits        |
+----------+---------------+----------------+
| Payload Length | Next Header | Hop Limit   |
| 16 bits        | 8 bits      | 8 bits      |
+-------------------------------------------+
|             Source Address                 |
|                128 bits                    |
+-------------------------------------------+
|           Destination Address              |
|                128 bits                    |
+-------------------------------------------+
|             Payload                        |
+-------------------------------------------+
```

### IPv6 Header Fields

|Field|Function|
|---|---|
|Version|Identifies IPv6 (value 6).|
|Traffic Class|Indicates traffic priority/handling.|
|Flow Label|Identifies packets belonging to a flow.|
|Payload Length|Specifies the length after the base header.|
|Next Header|Identifies the next extension header or upper-layer protocol.|
|Hop Limit|Limits the number of router hops.|
|Source Address|Identifies the sender.|
|Destination Address|Identifies the receiver.|

### IPv4 vs IPv6

Very important exam comparison

|Feature|IPv4|IPv6|
|---|---|---|
|Address size|32 bits|128 bits|
|Address format|Decimal|Hexadecimal|
|Example|192.168.1.1|2001:db8::1|
|Header size|20–60 bytes|Fixed 40 bytes|
|Address space|Smaller|Very large|
|Checksum|Present|Absent|
|Broadcast|Supported|Not supported|
|Fragmentation|Routers may fragment|Only source can fragment|
|Address resolution|ARP commonly used|Neighbor Discovery|
|Configuration|Manual/DHCP etc.|SLAAC/DHCPv6 etc.|

# 10. Important Formulas for Numericals

1. IPv4 address size

\[ 2^{32}=4,294,967,296 \]

2. IPv6 address size

\[ 2^{128}\approx3.4\times10^{38} \]

3. Number of host bits

\[ h=32-\text{Prefix Length} \]

4. Usable IPv4 host addresses in a conventional subnet

\[ \text{Usable Hosts}=2^h-2 \]

Example: `/24`

\[ h=32-24=8 \]

\[ 2^8-2=\boxed{254} \]

# 11. Most Important Questions for Mid-Sem

Prepare these questions first. They cover the major examinable concepts of your syllabus.

|Priority|Question|What to Prepare|
|---|---|---|
|1|Explain IPv4 and IPv6. Differentiate them.|Header diagrams + comparison|
|2|Draw and explain IPv4 packet format.|Header fields|
|3|Explain TCP/IP model.|Four layers + functions|
|4|Explain routing and its types.|Static, dynamic, RIP, OSPF, BGP|
|5|Explain congestion control.|Causes, effects, open/closed loop, Leaky/Token Bucket|
|6|Explain IP addressing.|Classes, Network ID, Host ID, CIDR|
|7|Explain point-to-point networks.|Datagram and virtual circuit|
|8|Define internetworking and explain its need.|Diagram, components and working|

## Last-minute revision: Learn these facts

Network Layer

- OSI Layer 3
    
- Data unit: Packet
    
- Main device: Router
    

Routing

- RIP → Distance Vector
    
- OSPF → Link State
    
- BGP → Path Vector
    

Congestion Control

- Leaky Bucket → Fixed-rate output
    
- Token Bucket → Controlled bursts
    
- Open Loop → Prevention
    
- Closed Loop → Feedback
    

TCP/IP

- Application
    
- Transport
    
- Internet
    
- Network Access/Link
    

IP Addressing

- IPv4 → 32 bits
    
- IPv6 → 128 bits
    
- CIDR → Variable-length prefix
    

IP Headers

- IPv4 → 20–60 bytes
    
- IPv6 → 40 bytes
    
- IPv4 has header checksum; IPv6 does not.
    

My recommendation for tonight: First study IPv4/IPv6 and their diagrams, then TCP/IP, routing, congestion control, IP addressing, and finally point-to-point networks. Practise drawing the IPv4 and IPv6 headers yourself. Those diagrams and comparison tables will help you write structured answers without memorising unnecessarily long paragraphs.