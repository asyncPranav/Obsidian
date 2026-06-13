
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
    

