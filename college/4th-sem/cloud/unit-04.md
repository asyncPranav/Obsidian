
---

## Unit 4 Priority Order (Study in this order)

### ⭐⭐⭐⭐⭐ Most Important

1. Concepts of Virtualization
2. Levels of Virtualization
3. Server Virtualization
4. Storage Virtualization
5. Networking Virtualization

### ⭐⭐⭐⭐ Important

6. Application Virtualization
7. Desktop Virtualization

### ⭐⭐⭐ Medium

8. Fundamental Concepts of Compute
9. Benefits, Advantages and Disadvantages

### Likely 10-Mark Questions from Unit 4

- Explain Virtualization with diagram.
- Explain Levels of Virtualization.
- Explain Server Virtualization.
- Explain Storage Virtualization.
- Explain Networking Virtualization.
- Differentiate Cloud Computing and Virtualization.
- Advantages and Disadvantages of Virtualization.


---


# ⭐⭐⭐ EXAM IMPORTANCE: HIGH

# Unit 4 – Fundamental Concepts of Compute

---

# 1. Definition

**Compute** refers to the processing power of a computer system that is used to execute instructions, perform calculations, run applications, and process data.

In cloud computing, compute resources mainly include:

- CPU (Processor)
    
- Memory (RAM)
    
- Storage
    
- Operating System
    
- Network Resources
    

These resources work together to perform computing tasks.

---

# 2. Introduction

Before understanding virtualization and cloud computing, it is important to understand the concept of **compute**.

Every application that we use, such as Google Chrome, WhatsApp, YouTube, or an online banking system, requires computing resources to function.

Traditionally, organizations purchased physical servers and hardware to obtain computing power. However, cloud computing provides these compute resources on demand through the internet.

Thus, compute is the foundation on which cloud computing and virtualization are built.

---

# 3. Basic Components of Compute

A computer performs tasks using several important components.

## (A) CPU (Central Processing Unit)

CPU is called the **brain of the computer**.

Functions:

- Executes instructions
    
- Performs calculations
    
- Processes data
    
- Controls system operations
    

The faster the CPU, the better the system performance.

---

## (B) Memory (RAM)

RAM is temporary memory used while programs are running.

Functions:

- Stores active data
    
- Stores running applications
    
- Provides quick access to information
    

More RAM allows more applications to run simultaneously.

---

## (C) Storage

Storage is used for permanent data retention.

Examples:

- Hard Disk Drive (HDD)
    
- Solid State Drive (SSD)
    

Functions:

- Stores operating systems
    
- Stores files and databases
    
- Stores application software
    

---

## (D) Operating System

The Operating System acts as an interface between hardware and users.

Examples:

- Windows
    
- Linux
    
- macOS
    

Functions:

- Resource management
    
- Memory management
    
- Process management
    
- Device management
    

---

## (E) Network Resources

Networking enables communication between systems.

Functions:

- Data transfer
    
- Internet connectivity
    
- Resource sharing
    

Examples:

- LAN
    
- WAN
    
- Internet
    

---

# 4. Compute Architecture

### Basic Compute Structure

```text
        USER
          │
          ▼
  ┌─────────────────┐
  │  APPLICATIONS   │
  └─────────────────┘
          │
          ▼
  ┌─────────────────┐
  │ OPERATING SYSTEM│
  └─────────────────┘
          │
          ▼
 ┌─────┬─────┬─────┐
 │ CPU │ RAM │Disk │
 └─────┴─────┴─────┘
          │
          ▼
      NETWORK
```

### Explanation

- User interacts with applications.
    
- Applications run on the operating system.
    
- Operating system uses CPU, RAM, and storage.
    
- Network connects the system with other devices.
    

---

# 5. Compute Resources in Cloud Computing

In cloud computing, users do not purchase physical hardware.

Instead, cloud providers offer compute resources as services.

Examples:

- Virtual Machines
    
- Cloud Servers
    
- Containers
    
- Serverless Computing
    

Users can increase or decrease resources according to their needs.

---

### Cloud Compute Model

```text
           USER
             │
             ▼
        INTERNET
             │
             ▼
   ┌───────────────────┐
   │   CLOUD PROVIDER  │
   └───────────────────┘
             │
   ┌─────────┼─────────┐
   ▼         ▼         ▼
 CPU       RAM      STORAGE
             │
             ▼
      APPLICATIONS
```

---

# 6. Characteristics of Compute Resources

## 1. Processing Capability

Ability to execute instructions and perform calculations.

---

## 2. Scalability

Resources can be increased or decreased according to demand.

Example:

- Increase CPU during high traffic.
    
- Reduce CPU during low traffic.
    

---

## 3. Availability

Resources remain available whenever users need them.

---

## 4. Flexibility

Different types of resources can be allocated based on workload.

---

## 5. Resource Sharing

Multiple users can share computing resources.

---

# 7. Importance of Compute in Cloud Computing

Compute resources are essential because they:

- Run applications
    
- Process user requests
    
- Store and manage data
    
- Support virtualization
    
- Enable cloud services
    

Without compute resources, cloud services cannot function.

---

# 8. Traditional Compute vs Cloud Compute

|Feature|Traditional Compute|Cloud Compute|
|---|---|---|
|Hardware|Purchased by organization|Provided by cloud provider|
|Cost|High|Pay-as-you-use|
|Scalability|Limited|Very high|
|Maintenance|User responsibility|Provider responsibility|
|Resource Allocation|Fixed|Dynamic|

---

# 9. Advantages of Cloud Compute

### 1. Cost Efficient

No need to purchase expensive hardware.

### 2. Scalability

Resources can be increased instantly.

### 3. High Performance

Modern cloud servers provide powerful processing.

### 4. Global Availability

Resources can be accessed from anywhere.

### 5. Flexibility

Resources can be customized according to workload.

---

# 10. Disadvantages

### 1. Internet Dependency

Cloud resources require internet access.

### 2. Security Concerns

Data is stored on external servers.

### 3. Vendor Dependency

Users depend on cloud providers.

### 4. Service Downtime

Provider failures may affect services.

---

# 11. Exam Definition (Most Important)

> Compute refers to the processing resources of a computer system, including CPU, memory, storage, operating systems, and networking components that are used to execute applications and process data. In cloud computing, compute resources are provided on demand through cloud infrastructure.

---

# 12. 5-Mark Answer (Exam Ready)

**Fundamental concepts of compute** refer to the basic computing resources required for processing data and running applications. These resources include CPU, memory, storage, operating systems, and networking components. CPU performs calculations, RAM stores temporary data, storage keeps permanent data, and the operating system manages hardware resources. In cloud computing, compute resources are provided on demand through virtual machines and cloud servers. Compute forms the foundation of virtualization and cloud computing by enabling application execution, data processing, and resource management.

---

# ⭐ Most Expected Exam Questions

### 2 Marks

- What is compute?
    
- What are compute resources?
    

### 5 Marks

- Explain fundamental concepts of compute.
    
- Explain components of compute resources.
    

### 10 Marks

- Explain compute architecture with diagram.
    
- Explain compute resources used in cloud computing.
    

---

# 🚀 Last Minute Revision

- Compute = Processing power of a computer.
    
- Main components = CPU + RAM + Storage + OS + Network.
    
- Compute is the foundation of cloud computing.
    
- Cloud provides compute resources on demand.
    
- Virtualization uses compute resources to create virtual machines.
    

----


# ⭐⭐⭐⭐⭐ EXAM IMPORTANCE: VERY HIGH (MOST IMPORTANT TOPIC OF UNIT 4)

**Concepts of Virtualization** is one of the most frequently asked topics in Cloud Computing exams. Questions of **5 marks, 10 marks, and even long-answer questions** are commonly asked from this topic.

---

# Unit 4 – Concepts of Virtualization

---

# 1. Definition

> **Virtualization** is a technology that creates a virtual (software-based) version of computing resources such as servers, operating systems, storage devices, networks, or applications using a software layer called a **hypervisor**.

In simple words:

> Virtualization allows a single physical computer to behave like multiple independent computers.

---

# 2. Introduction

Traditionally, one physical server could run only one operating system and a limited number of applications.

This created several problems:

- Hardware resources remained underutilized.
    
- Organizations had to purchase many servers.
    
- Infrastructure cost increased.
    
- Maintenance became difficult.
    

To overcome these problems, virtualization was introduced.

Virtualization enables multiple virtual machines (VMs) to run on a single physical machine, allowing better utilization of hardware resources.

Today, virtualization is the foundation of modern cloud computing because cloud providers create thousands of virtual machines on physical servers and provide them to users as cloud services.

---

# 3. What is a Virtual Machine (VM)?

A **Virtual Machine (VM)** is a software-based computer that behaves like a real computer.

Each VM has:

- Virtual CPU
    
- Virtual RAM
    
- Virtual Storage
    
- Operating System
    
- Applications
    

A VM works independently even though multiple VMs share the same physical hardware.

---

# 4. Basic Concept of Virtualization

### Without Virtualization

```text
+----------------------+
|    Application       |
+----------------------+
|   Operating System   |
+----------------------+
|   Physical Server    |
+----------------------+
```

Problems:

- One OS per server
    
- Resource wastage
    
- High cost
    

---

### With Virtualization

```text
+---------+  +---------+  +---------+
|  VM 1   |  |  VM 2   |  |  VM 3   |
| OS + App|  | OS + App|  | OS + App|
+---------+  +---------+  +---------+
        \        |        /
         \       |       /
      +-------------------+
      |    Hypervisor     |
      +-------------------+
      | Physical Hardware |
      +-------------------+
```

One physical server runs multiple virtual machines.

---

# 5. Need for Virtualization

Virtualization was introduced because traditional systems suffered from:

### 1. Low Hardware Utilization

Most servers used only 10–20% of their capacity.

### 2. High Infrastructure Cost

Organizations needed separate servers for different applications.

### 3. Increased Power Consumption

More servers required more electricity.

### 4. Complex Management

Managing hundreds of physical servers was difficult.

### 5. Limited Scalability

Expanding infrastructure required purchasing new hardware.

Virtualization solved these issues by sharing hardware resources efficiently.

---

# 6. How Virtualization Works

The key component behind virtualization is the **Hypervisor**.

A hypervisor sits between hardware and virtual machines.

Its functions are:

- Creates virtual machines
    
- Allocates CPU and RAM
    
- Manages storage
    
- Controls VM communication
    
- Ensures isolation between VMs
    

---

## Working Diagram

```text
           User Applications
                   │
      ┌────────────┼────────────┐
      ▼            ▼            ▼
   VM-1         VM-2         VM-3
 (Windows)     (Linux)     (Ubuntu)
      │            │            │
      └────────────┼────────────┘
                   ▼
            HYPERVISOR
                   ▼
      Physical Server Hardware
      (CPU, RAM, Disk, Network)
```

---

# 7. Components of Virtualization

## (A) Physical Hardware

Actual server resources:

- CPU
    
- RAM
    
- Disk
    
- Network
    

---

## (B) Hypervisor

Software responsible for creating and managing VMs.

Examples:

- VMware ESXi
    
- Microsoft Hyper-V
    
- KVM
    
- Xen
    

---

## (C) Virtual Machines

Software-based computers running independently.

---

## (D) Guest Operating System

Operating system installed inside VM.

Examples:

- Windows
    
- Linux
    
- Ubuntu
    

---

# 8. Characteristics of Virtualization

### 1. Resource Sharing

Multiple VMs share the same hardware.

---

### 2. Isolation

Failure of one VM does not affect others.

---

### 3. Flexibility

Different operating systems can run simultaneously.

---

### 4. Scalability

New virtual machines can be created quickly.

---

### 5. Hardware Independence

VMs are independent of physical hardware.

---

# 9. Types of Resources that can be Virtualized

Virtualization is not limited to servers only.

### 1. Server Virtualization

Creates multiple virtual servers.

### 2. Storage Virtualization

Combines multiple storage devices into one logical storage unit.

### 3. Network Virtualization

Creates virtual networks.

### 4. Desktop Virtualization

Provides virtual desktop environments.

### 5. Application Virtualization

Runs applications independently of OS.

---

# 10. Role of Virtualization in Cloud Computing

Virtualization is the technology that makes cloud computing possible.

Without virtualization:

- Cloud providers would need a separate physical server for every user.
    

With virtualization:

- One physical server can serve many users.
    
- Resources can be allocated dynamically.
    
- Infrastructure becomes highly scalable.
    

---

## Virtualization in Cloud

```text
            CLOUD USERS
      ┌──────┬──────┬──────┐
      ▼      ▼      ▼      ▼
     VM1    VM2    VM3    VM4
      │      │      │      │
      └──────┼──────┼──────┘
             ▼
        Hypervisor
             ▼
      Physical Server
```

This is exactly how AWS, Azure, and Google Cloud provide services.

---

# 11. Advantages of Virtualization

### 1. Better Resource Utilization

Hardware is used efficiently.

### 2. Cost Reduction

Fewer physical servers are needed.

### 3. Easy Scalability

VMs can be added quickly.

### 4. Improved Flexibility

Different OS can run on same hardware.

### 5. Faster Deployment

New systems can be created in minutes.

### 6. Disaster Recovery

VM backups can be restored easily.

---

# 12. Disadvantages of Virtualization

### 1. Performance Overhead

Virtual layer may slightly reduce performance.

### 2. Single Point of Failure

If physical server fails, multiple VMs may stop.

### 3. Security Concerns

Hypervisor attacks can affect VMs.

### 4. Initial Setup Complexity

Requires skilled administrators.

---

# 13. Virtualization vs Traditional Computing

|Feature|Traditional Computing|Virtualization|
|---|---|---|
|Servers|One OS per server|Multiple OS per server|
|Hardware Usage|Low|High|
|Cost|High|Lower|
|Scalability|Difficult|Easy|
|Resource Utilization|Poor|Efficient|

---

# 14. Exam Definition (Most Important)

> Virtualization is a technology that creates virtual versions of computing resources such as servers, storage, networks, and operating systems by using a hypervisor, allowing multiple virtual machines to run on a single physical machine.

---

# 15. 5-Mark Answer (Exam Ready)

Virtualization is a technology that enables the creation of virtual versions of computing resources such as servers, operating systems, storage, and networks. It allows multiple virtual machines to run on a single physical server using a software layer called a hypervisor. Virtualization improves resource utilization, reduces infrastructure cost, provides scalability, and simplifies management. It forms the foundation of cloud computing because cloud providers use virtualization to deliver computing resources efficiently to multiple users.

---

# ⭐ Most Expected Exam Questions

### 2 Marks

- Define virtualization.
    
- What is a virtual machine?
    
- What is a hypervisor?
    

### 5 Marks

- Explain the concept of virtualization.
    
- Explain the need for virtualization.
    
- Advantages of virtualization.
    

### 10 Marks

- Explain virtualization with neat diagram.
    
- Explain working of virtualization.
    
- Explain role of virtualization in cloud computing.
    

---

# 🚀 Last Minute Revision

- Virtualization = Creating virtual computers from one physical computer.
    
- Main component = Hypervisor.
    
- Virtual machine (VM) = Software-based computer.
    
- One server → Multiple VMs.
    
- Benefits = Cost saving, scalability, resource utilization.
    
- Foundation of Cloud Computing.
    
- **Keywords for exam:** Hypervisor, VM, Resource Sharing, Isolation, Scalability, Cloud Computing.
    

### Diagram to Always Draw in Exam

```text
 VM1      VM2      VM3
  │        │        │
  └────────┼────────┘
           ▼
      Hypervisor
           ▼
   Physical Hardware
```

This single diagram can fetch extra marks in almost every virtualization question.



---

# ⭐⭐⭐ VERY IMPORTANT TOPIC (HIGH EXAM WEIGHTAGE)

# Unit 4 – Levels of Virtualization

### Exam Importance: ⭐⭐⭐⭐⭐ (Very High)

This topic is frequently asked in **5 marks and 10 marks questions** because it explains the different ways virtualization can be implemented in a computer system.

---

# 1. Definition

**Levels of Virtualization** refer to the different layers of a computer system where virtualization can be applied to create virtual resources, environments, or services.

In simple words:

> Virtualization can be done at different levels such as hardware level, operating system level, server level, storage level, network level, and application level.

Each level provides a different type of virtual environment and serves different purposes in cloud computing.

---

# 2. Why Different Levels of Virtualization are Needed?

Different users have different requirements:

- Some need virtual machines.
    
- Some need virtual storage.
    
- Some need virtual networks.
    
- Some need only virtual applications.
    

Therefore virtualization is implemented at multiple levels.

### Benefits

- Better resource utilization
    
- Cost reduction
    
- Flexibility
    
- Scalability
    
- Easy management
    

---

# 3. Levels of Virtualization

## Structured Diagram

```text
                LEVELS OF VIRTUALIZATION
                          │
 ┌────────────────────────┼────────────────────────┐
 │                        │                        │
Hardware              Operating               Application
Virtualization      System Virtualization    Virtualization
 │                        │                        │
Server                Containers           Virtual Apps
Virtualization
 │
Storage Virtualization
 │
Network Virtualization
 │
Desktop Virtualization
```

---

# 4. Hardware Virtualization

### Definition

Hardware virtualization is the process of creating virtual versions of physical computers using a software layer called a hypervisor.

### Working

A hypervisor sits between hardware and operating systems.

It allows multiple operating systems to run simultaneously on a single physical machine.

### Diagram

```text
      Virtual Machine 1
      (Guest OS)

      Virtual Machine 2
      (Guest OS)

      Virtual Machine 3
      (Guest OS)
             │
        Hypervisor
             │
      Physical Hardware
```

### Examples

- VMware ESXi
    
- Microsoft Hyper-V
    
- KVM
    

### Advantages

- Better hardware utilization
    
- Multiple operating systems
    
- Reduced infrastructure cost
    

---

# 5. Operating System Level Virtualization

### Definition

In OS-level virtualization, multiple isolated environments share the same operating system kernel.

These isolated environments are called containers.

### Working

Instead of virtualizing hardware, the operating system creates separate execution environments.

### Diagram

```text
    Container 1
    Container 2
    Container 3
         │
 Shared Operating System
         │
     Hardware
```

### Examples

- Docker
    
- LXC
    
- Kubernetes Pods
    

### Advantages

- Lightweight
    
- Fast startup
    
- Less resource consumption
    

### Limitation

All containers must use the same OS kernel.

---

# 6. Server Virtualization

### Definition

Server virtualization divides a single physical server into multiple virtual servers.

Each virtual server behaves like an independent server.

### Diagram

```text
       Physical Server
              │
        Hypervisor
              │
 ┌────────┬────────┬────────┐
 │Virtual │Virtual │Virtual │
 │Server1 │Server2 │Server3 │
 └────────┴────────┴────────┘
```

### Uses

- Cloud data centers
    
- Enterprise environments
    
- Hosting services
    

---

# 7. Storage Virtualization

### Definition

Storage virtualization combines multiple physical storage devices into a single logical storage unit.

To users, it appears as one large storage system.

### Diagram

```text
      Physical Disk 1
      Physical Disk 2
      Physical Disk 3
             │
    Storage Virtualization
             │
      Virtual Storage Pool
```

### Advantages

- Easier storage management
    
- Better backup
    
- Improved scalability
    

### Example

- SAN (Storage Area Network)
    
- NAS systems
    

---

# 8. Network Virtualization

### Definition

Network virtualization creates virtual networks independent of physical network hardware.

Multiple virtual networks can run on the same physical infrastructure.

### Diagram

```text
       Physical Network
              │
    Network Virtualization
              │
 ┌────────┬────────┬────────┐
 │VLAN 1  │VLAN 2  │VLAN 3  │
 └────────┴────────┴────────┘
```

### Advantages

- Better security
    
- Network isolation
    
- Flexible management
    

### Examples

- VLAN
    
- SDN (Software Defined Networking)
    

---

# 9. Desktop Virtualization

### Definition

Desktop virtualization separates the desktop environment from the physical device.

The desktop runs on a remote server while users access it through the network.

### Diagram

```text
      User Devices
     /      |      \
    /       |       \
 Laptop   Mobile   PC
      \      |      /
       Virtual Desktop
            Server
```

### Advantages

- Centralized management
    
- Remote access
    
- Improved security
    

### Example

- Virtual Desktop Infrastructure (VDI)
    

---

# 10. Application Virtualization

### Definition

Application virtualization allows applications to run independently from the underlying operating system.

Applications are delivered in a virtual environment.

### Diagram

```text
      Virtual Application
              │
    Virtualization Layer
              │
      Operating System
              │
          Hardware
```

### Examples

- Microsoft App-V
    
- VMware ThinApp
    

### Advantages

- Easy deployment
    
- No installation conflicts
    
- Better portability
    

---

# 11. Comparison of Different Levels

|Level|What is Virtualized?|Example|
|---|---|---|
|Hardware|Entire hardware platform|VMware|
|OS Level|Operating system environment|Docker|
|Server|Physical server|Hyper-V|
|Storage|Storage devices|SAN|
|Network|Network resources|VLAN|
|Desktop|User desktops|VDI|
|Application|Software applications|App-V|

---

# 12. Relationship Between Levels

### Easy Diagram

```text
Application Virtualization
           ↑
Desktop Virtualization
           ↑
OS Virtualization
           ↑
Server Virtualization
           ↑
Hardware Virtualization
           ↑
Physical Hardware
```

Each upper layer depends on the lower layer.

---

# 13. Advantages of Multiple Virtualization Levels

### 1. Better Resource Utilization

Resources are used efficiently.

### 2. Scalability

Resources can be increased easily.

### 3. Reduced Cost

Less hardware is required.

### 4. Flexibility

Different virtualization methods can be chosen according to requirements.

### 5. Improved Management

Resources become easier to control and monitor.

---

# 14. Exam Definition (Write This)

> Levels of virtualization refer to the various layers of a computing system where virtualization can be implemented, such as hardware, operating system, server, storage, network, desktop, and application levels, to improve resource utilization, scalability, and flexibility.

---

# 15. 5-Mark Answer Structure

1. Definition
    
2. Diagram of levels
    
3. Explain Hardware Virtualization
    
4. Explain OS Virtualization
    
5. Explain Storage and Network Virtualization
    
6. Conclusion
    

---

# 16. 10-Mark Answer Structure

1. Definition
    
2. Main diagram of levels
    
3. Hardware Virtualization
    
4. OS Virtualization
    
5. Server Virtualization
    
6. Storage Virtualization
    
7. Network Virtualization
    
8. Desktop Virtualization
    
9. Application Virtualization
    
10. Advantages
    

---

# 🔥 Last-Minute Revision

- **Hardware Virtualization** → Multiple VMs on one machine
    
- **OS Virtualization** → Containers (Docker)
    
- **Server Virtualization** → Multiple virtual servers
    
- **Storage Virtualization** → Multiple disks become one pool
    
- **Network Virtualization** → Virtual networks (VLAN)
    
- **Desktop Virtualization** → Remote desktops
    
- **Application Virtualization** → Virtual software
    

### One-Line Revision

> Levels of virtualization describe the different layers of a computer system where virtual resources can be created, including hardware, operating system, server, storage, network, desktop, and application levels.