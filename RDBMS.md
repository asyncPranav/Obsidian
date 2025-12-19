
----

# 🔥 UNIT–1 : MOST IMPORTANT TOPICS (EXAM-ORIENTED)

## 🟥 TOP PRIORITY (VERY HIGH CHANCE – ALMOST GUARANTEED)


# **Database & DBMS**

## **1. Database – Definition**

A **database** is an organized collection of **related data** stored electronically in a computer system.  
It is designed to store, retrieve, and manage data efficiently.

**Example:**  
Student database containing Roll No, Name, Course, Marks.

---

## **2. DBMS – Definition**

A **Database Management System (DBMS)** is a **software system** that allows users to **define, create, store, retrieve, update, and control access** to a database.

**Examples:** MySQL, Oracle, SQL Server, PostgreSQL.

---

## **3. Database System**

A **database system** consists of:

- Database
    
- DBMS software
    
- Application programs
    
- Users
    

---

# **4. Characteristics of Database Approach**

The database approach has the following important characteristics:

---

## **1️⃣ Self-Describing Nature of Database System**

A DBMS contains not only the data but also the **description of the data**, known as **metadata**.

- Metadata includes:
    
    - Data type
        
    - Size
        
    - Structure
        
    - Constraints
        

This information is stored in a **system catalog (data dictionary)**.

👉 Hence, the database is **self-describing**.

---

## **2️⃣ Program–Data Independence**

In DBMS, **application programs are independent of data storage structure**.

- Changes in data structure **do not require changes** in application programs.
    
- Achieved through:
    
    - Logical data independence
        
    - Physical data independence
        

👉 This makes the system **flexible and easy to maintain**.

---

## **3️⃣ Data Abstraction**

DBMS hides complex internal details from users and provides **different levels of abstraction**.

### Levels of Data Abstraction:

1. **Physical Level** – How data is stored internally
    
2. **Logical Level** – What data is stored and relationships
    
3. **View Level** – User-specific view of data
    

👉 Data abstraction simplifies database usage.

---

## **4️⃣ Multiple Views of Data**

A DBMS allows different users to have **different views** of the same database.

- Each user sees only the data relevant to them.
    
- Improves **security and usability**.
    

**Example:**

- Student sees marks
    
- Admin sees all student records
    

---

## **5️⃣ Data Sharing and Multi-User Access**

A DBMS allows **multiple users to access the database simultaneously**.

- Supports:
    
    - Concurrency control
        
    - Transaction management
        
- Prevents data inconsistency.
    

👉 Enables **efficient data sharing** in organizations.

---


# **File System vs Database System (DBMS)**

## **Definition (Intro – 1 mark)**

A **file system** stores data in separate files managed by the operating system, whereas a **Database Management System (DBMS)** stores data in an integrated database and provides controlled access to it.

---

## **Difference between File System and DBMS**

|**Basis of Comparison**|**File System**|**Database System (DBMS)**|
|---|---|---|
|**Data Redundancy**|High data redundancy due to duplicate files|Minimal redundancy due to centralized database|
|**Data Consistency**|Data inconsistency may occur|Data consistency is maintained|
|**Data Sharing**|Data sharing is difficult|Data sharing is easy among multiple users|
|**Security**|Less security, handled by OS|High security using authorization and access control|
|**Data Integrity**|Integrity constraints are difficult to enforce|Integrity constraints are easily enforced|
|**Backup & Recovery**|Manual and difficult|Automatic backup and recovery mechanisms|
|**Data Independence**|No data independence|Logical and physical data independence supported|
|**Concurrency Control**|Not supported|Supported using transaction management|
|**Query Processing**|No query language support|SQL support for efficient querying|
|**Maintenance Cost**|High maintenance cost|Lower maintenance cost in long term|

---

## **Why DBMS is Better than File System? (Conclusion – 2 marks)**

A DBMS is better than a file system because it **reduces redundancy**, **maintains consistency**, **ensures data security and integrity**, supports **multi-user access**, provides **backup and recovery**, and offers **data independence**, making data management more efficient and reliable.