
---

# UNIT 3 — DESIGN ENGINEERING

Software Engineering | Exam-oriented notes

These notes cover the complete syllabus without unnecessary extensions. Focus on definitions, important concepts, diagrams, and differences that can be directly used in 5–10 mark answers.

# 1. DESIGN PROCESS AND DESIGN QUALITY

## 1.1 What is Software Design?

Software design is the process of converting software requirements into a blueprint that describes the structure, components, interfaces, and data of a software system.

In simple words, requirements tell us what to develop, while design tells us how to organize and develop it.

### Design Process

The major activities are:

1. Data/Class Design – Defines data structures and database organization.
    
2. Architectural Design – Defines the overall structure of the software.
    
3. Interface Design – Defines communication between users and system components.
    
4. Component-Level Design – Defines the internal logic of individual components.
    
5. Deployment Design – Describes how software components are deployed on hardware.
    

### Diagram

```
       Software Requirements
                |
                ↓
          Data/Class Design
                |
                ↓
       Architectural Design
                |
                ↓
          Interface Design
                |
                ↓
      Component-Level Design
                |
                ↓
          Deployment Design
                |
                ↓
          Software System
```

## 1.2 Design Quality

Design quality refers to how effectively a software design satisfies requirements and supports the development of reliable, efficient, and maintainable software.

### Characteristics of Good Software Design

1. Correctness – Meets all specified requirements.
    
2. Simplicity – Easy to understand and implement.
    
3. Modularity – Divided into smaller independent modules.
    
4. Maintainability – Easy to modify and update.
    
5. Efficiency – Uses resources effectively.
    
6. Reusability – Components can be reused.
    
7. Reliability – Performs correctly under expected conditions.
    
8. Testability – Easy to test and verify.
    

Important: A good design reduces complexity, development cost, errors, and future maintenance effort.

# 2. DESIGN CONCEPTS

Design concepts are fundamental principles used to develop a good software design.

## 2.1 Abstraction

Abstraction means showing only essential information while hiding unnecessary implementation details.

Example: A user uses an ATM to withdraw money without knowing its internal processing.

Types:

- Data abstraction
    
- Procedural abstraction
    
- Control abstraction
    

## 2.2 Modularity

Modularity means dividing a large software system into smaller, manageable modules.

Example: An e-commerce system can be divided into user, product, order, and payment modules.

Advantages:

- Easy maintenance
    
- Easier testing
    
- Reduced complexity
    
- Code reusability
    

## 2.3 Software Architecture

Architecture describes the overall structure of a software system, including its components and their relationships.

Example: Client-server architecture.

## 2.4 Information Hiding

Each module hides its internal implementation and exposes only the necessary information through interfaces.

Example: A payment module provides a payment function without exposing its internal processing logic.

## 2.5 Separation of Concerns

Different responsibilities should be handled by separate modules.

Example:

- Authentication handles login.
    
- Payment handles transactions.
    
- User module handles profiles.
    

This makes software easier to develop and maintain.

## 2.6 Functional Independence

A module should perform a specific task with minimum dependency on other modules.

It is achieved through:

- High Cohesion: Elements inside a module are strongly related.
    
- Low Coupling: Dependency between different modules is minimum.
    

Important for exams

```
Good Design = High Cohesion + Low Coupling
```

## 2.7 Refinement

Refinement is the process of gradually converting a high-level design into more detailed design specifications.

Example:

```
Online Shopping System
          ↓
      Order Module
          ↓
    Place Order Function
          ↓
 Validate Cart → Calculate Price
          ↓
       Save Order
```

## 2.8 Design Patterns

Design patterns are reusable solutions to commonly occurring software design problems.

Example: Singleton, Factory, Observer.

## 2.9 Refactoring

Refactoring means improving the internal structure of existing code without changing its external functionality.

# 3. THE DESIGN MODEL

## Definition

A design model is a representation of the software system that describes how the system will be structured and implemented.

It acts as a bridge between requirements analysis and coding.

### Main Elements of Design Model

|Element|Purpose|
|---|---|
|Data/Class Design|Defines data structures and classes|
|Architectural Design|Defines overall system structure|
|Interface Design|Defines interaction between components and users|
|Component-Level Design|Describes internal logic of components|
|Deployment Design|Describes physical deployment of software|

### Design Model Flow

```
       Requirement Model
               |
               ↓
         Design Model
               |
      ┌────────┼─────────┐
      ↓        ↓         ↓
    Data   Architecture Interface
      ↓        ↓         ↓
      Component-Level Design
               |
               ↓
         Implementation
```

# 4. CREATING AN ARCHITECTURAL DESIGN

## 4.1 Software Architecture

Software architecture is the overall structure of a software system that defines its major components, their responsibilities, relationships, and interactions.

It provides a high-level view of the system before detailed coding begins.

### Example — Three-Tier Architecture

```
       Presentation Layer
        (User Interface)
                |
                ↓
         Business Layer
       (Application Logic)
                |
                ↓
          Data Layer
          (Database)
```

### Importance of Software Architecture

1. Provides a clear structure.
    
2. Helps manage system complexity.
    
3. Supports scalability.
    
4. Improves maintainability.
    
5. Helps developers understand component relationships.
    
6. Supports performance and security decisions.
    

## 4.2 Data Design

Data design is the process of converting information requirements into appropriate data structures, database tables, and relationships.

It determines how data will be stored, organized, and accessed.

### Example — Student Management System

```
STUDENT
----------------
Student_ID
Name
Email
Course_ID
```

```
COURSE
----------------
Course_ID
Course_Name
Duration
```

The `Course_ID` connects students with their respective courses.

### Objectives

- Organize data efficiently.
    
- Reduce unnecessary duplication.
    
- Maintain data integrity.
    
- Improve data accessibility.
    
- Support efficient storage and retrieval.
    

# 5. ARCHITECTURAL STYLES AND PATTERNS

## 5.1 Architectural Style

An architectural style defines a general way of organizing the components and interactions of a software system.

It provides a reusable structural organization for software.

### Major Architectural Styles

### 1. Data-Centered Architecture

A central data repository is shared by different components.

```
       Component A
            |
            ↓
Component B → Database ← Component C
            ↑
       Component D
```

Example: Library management system with a central database.

Use: Systems where multiple components access shared data.

### 2. Data-Flow Architecture

Data moves through a sequence of processing components. Each component transforms the received data and passes it forward.

```
Input
  ↓
Process 1
  ↓
Process 2
  ↓
Process 3
  ↓
Output
```

Example: Compiler processing source code through different stages.

This style is also called pipe-and-filter when processing elements are connected through pipes.

### 3. Call-and-Return Architecture

The system is organized into modules where one module calls another to perform a task.

```
        Main Module
             |
       ┌─────┼─────┐
       ↓     ↓     ↓
    Module A B     C
```

Example: Traditional procedural programs.

### 4. Object-Oriented Architecture

The system is organized around objects that contain data and related operations.

Example: Banking system containing Account, Customer, and Transaction objects.

### 5. Layered Architecture

The system is divided into layers. Each layer performs a particular responsibility and communicates with adjacent layers.

```
       Presentation
             ↓
          Business
             ↓
            Data
             ↓
          Database
```

Example: Three-tier web applications.

## 5.2 Architectural Patterns

An architectural pattern is a reusable solution to a commonly occurring problem in software architecture.

Important patterns:

### 1. Model-View-Controller (MVC)

MVC divides an application into three parts:

- Model: Manages data and business logic.
    
- View: Displays information to users.
    
- Controller: Handles user input and coordinates Model and View.
    

```
         User
          |
          ↓
      Controller
        /    \
       ↓      ↓
     Model → View
              |
              ↓
             User
```

Example: Web applications.

### 2. Client-Server Pattern

The client requests services, and the server processes requests and sends responses.

```
Client 1 ───┐
            |
Client 2 ───┼──→ Server → Database
            |
Client 3 ───┘
```

Example: Browser communicating with a web server.

### Architectural Style vs Architectural Pattern

|Architectural Style|Architectural Pattern|
|---|---|
|Defines general system organization|Provides a reusable architectural solution|
|Describes structural arrangement|Addresses a recurring design problem|
|Example: Layered architecture|Example: MVC|
|Broad organizational approach|More specific solution to a problem|

# 6. ARCHITECTURAL DESIGN

## Definition

Architectural design is the process of identifying the major components of a software system and defining their structure, relationships, and communication.

### Main Activities

1. Analyze system requirements.
    
2. Identify major system components.
    
3. Select an appropriate architectural style.
    
4. Define component responsibilities.
    
5. Define interfaces and communication.
    
6. Evaluate architecture against quality requirements.
    

### Example

For an online shopping application:

```
       E-Commerce System
               |
       ┌───────┼────────┐
       ↓       ↓        ↓
      User   Product   Order
       |       |        |
       └───────┼────────┘
               ↓
            Payment
               |
               ↓
            Database
```

# 7. CONCEPTUAL MODEL OF UML

## 7.1 What is UML?

UML (Unified Modeling Language) is a standard visual modeling language used to represent, specify, visualize, and document software systems.

It helps developers understand system structure and behavior through diagrams.

UML is a modeling language, not a software development process.

## 7.2 Basic Elements of UML

UML has three main building blocks:

### 1. Things

Basic elements used in UML diagrams.

They are classified into:

- Structural Things: Class, interface, component, node.
    
- Behavioral Things: Interaction, state machine.
    
- Grouping Things: Package.
    
- Annotational Things: Notes and comments.
    

### 2. Relationships

Relationships show how UML elements are connected.

Important relationships:

- Association: General connection between classes.
    
- Dependency: One element depends on another.
    
- Generalization: Inheritance relationship.
    
- Realization: A class implements an interface.
    

### 3. Diagrams

Diagrams visually represent different parts of the system.

## 7.3 Classification of UML Diagrams

```
                 UML DIAGRAMS
                      |
          ┌───────────┴───────────┐
          ↓                       ↓
    Structural Diagrams    Behavioral Diagrams
          |                       |
       Class                  Use Case
       Component              Sequence
       Object                 Activity
       Deployment             State Machine
                              Communication
```

Structural diagrams show the static structure of a system, while behavioral diagrams show its behavior and interactions.

# 8. BASIC STRUCTURAL MODELING

## Definition

Structural modeling represents the static structure of a software system by showing its classes, objects, components, and relationships.

It explains what elements exist in the system and how they are connected.

Important elements include:

- Classes
    
- Objects
    
- Attributes
    
- Operations
    
- Interfaces
    
- Relationships
    
- Components
    

### Important Class Relationships

|Relationship|Meaning|
|---|---|
|Association|General connection between classes|
|Inheritance (Generalization)|One class inherits another class|
|Aggregation|Weak whole-part relationship|
|Composition|Strong whole-part relationship|
|Dependency|One class uses or depends on another|
|Realization|A class implements an interface|

### Example

```
        Animal
           △
           |
          Dog
```

Here, Dog inherits the properties and behavior of Animal.

# 9. CLASS DIAGRAM

## Definition

A class diagram is a UML structural diagram that represents classes, their attributes, methods, and relationships.

It describes the static structure of a system.

### Structure of a Class

A class is represented by a rectangle divided into three sections:

```
+----------------------+
|       Student        |
+----------------------+
| - studentId          |
| - name               |
| - email              |
+----------------------+
| + register()         |
| + viewResult()       |
+----------------------+
```

- First section: Class name.
    
- Second section: Attributes.
    
- Third section: Methods/operations.
    

### Visibility Symbols

|Symbol|Meaning|
|---|---|
|`+`|Public|
|`-`|Private|
|`#`|Protected|

### Example — Student and Course

```
+-------------+       +-------------+
|   Student   |       |    Course   |
+-------------+       +-------------+
| studentId   |       | courseId    |
| name        |       | courseName  |
+-------------+       +-------------+
| register()  |       | getDetails()|
+-------------+       +-------------+
       |                    |
       └────── Enrolls ─────┘
```

### Advantages

1. Represents system structure clearly.
    
2. Helps in object-oriented design.
    
3. Shows relationships between classes.
    
4. Helps developers during implementation.
    

# 10. SEQUENCE DIAGRAM

## Definition

A sequence diagram is a UML behavioral diagram that shows interactions between objects in a particular sequence over time.

It focuses on the order in which messages are exchanged.

### Main Components

1. Actor/Object – Participates in interaction.
    
2. Lifeline – Vertical dashed line representing an object's existence over time.
    
3. Activation Bar – Shows the period when an object performs an operation.
    
4. Message – Communication between objects.
    

### Example — User Login

```
User       Login Page      Auth System      Database
 |              |               |               |
 |--Login------>|               |               |
 |              |--Credentials->|               |
 |              |               |--Find User--->|
 |              |               |<--User Data---|
 |              |<--Result------|               |
 |<--Response---|               |               |
```

Important: Time moves from top to bottom in a sequence diagram.

### Uses

- Shows message order.
    
- Explains object interaction.
    
- Helps understand system workflows.
    
- Useful for designing APIs and operations.
    

# 11. COLLABORATION DIAGRAM (COMMUNICATION DIAGRAM)

## Definition

A collaboration diagram is a UML interaction diagram that shows how objects communicate with each other to perform a particular task.

In modern UML terminology, it is called a Communication Diagram.

It focuses on object relationships and message flow rather than emphasizing time.

### Main Components

1. Objects – Participants in communication.
    
2. Links – Connections between objects.
    
3. Messages – Information exchanged between objects.
    
4. Sequence Numbers – Indicate the order of messages.
    

### Example — User Login

```
+------+     1: Login       +------------+
| User | -----------------> | Login Page |
+------+                    +------------+
                                  |
                              2: Verify
                                  |
                                  ↓
                            +------------+
                            | Auth System|
                            +------------+
                                  |
                              3: Query
                                  |
                                  ↓
                            +------------+
                            |  Database  |
                            +------------+
```

### Uses

- Shows relationships among objects.
    
- Represents message communication.
    
- Helps understand system collaboration.
    
- Useful for object-oriented design.
    

### Sequence Diagram vs Collaboration Diagram

|Sequence Diagram|Collaboration Diagram|
|---|---|
|Focuses on time and message order|Focuses on object relationships and communication|
|Uses vertical lifelines|Uses objects connected by links|
|Time flows from top to bottom|Message order is shown by numbering|
|Emphasizes sequence of events|Emphasizes object organization|

Remember: Both diagrams can represent the same interaction from different viewpoints.

# 12. USE CASE DIAGRAM

## Definition

A use case diagram is a UML behavioral diagram that represents the interaction between external users (actors) and the functions provided by a system.

It describes the functional requirements of a system from the user's perspective.

### Main Components

1. Actor – External user or system interacting with the software.
    
2. Use Case – A function or service provided by the system.
    
3. System Boundary – Defines the system scope.
    
4. Association – Connection between actor and use case.
    

### Example — ATM System

```
             ATM SYSTEM
      +--------------------------+
      |                          |
      |   (Check Balance)        |
User ─┼── (Withdraw Cash)        |
      |                          |
      |   (Deposit Money)        |
      |                          |
      |   (Print Statement)      |
      |                          |
      +--------------------------+
```

### Relationships in Use Case Diagrams

1. Association

Shows communication between an actor and a use case.

2. Include

Represents compulsory reuse of another use case.

Example:

```
(Withdraw Cash) --<<include>>--> (Authenticate User)
```

3. Extend

Represents optional or conditional additional behavior.

Example:

```
(Print Receipt) --<<extend>>--> (Withdraw Cash)
```

4. Generalization

Represents inheritance between actors or use cases.

### Advantages

- Easy to understand.
    
- Identifies system functionality.
    
- Helps gather requirements.
    
- Improves communication between developers and users.
    

# 13. COMPONENT DIAGRAM

## Definition

A component diagram is a UML structural diagram that represents the physical or logical components of a software system and the dependencies between them.

A component is an independently identifiable part of a system that performs a specific responsibility.

### Main Components

1. Components – Modules or software units.
    
2. Interfaces – Services provided or required by components.
    
3. Dependencies – Relationships showing that one component depends on another.
    

### Example — E-Commerce System

```
+------------------+
|   User Component |
+------------------+
         |
         ↓
+------------------+
|  Product Component|
+------------------+
         |
         ↓
+------------------+
|   Order Component|
+------------------+
         |
         ↓
+------------------+
| Payment Component|
+------------------+
         |
         ↓
+------------------+
| Database Component|
+------------------+
```

### Advantages

- Shows system organization.
    
- Helps understand module dependencies.
    
- Supports software maintenance.
    
- Helps in system implementation and deployment planning.
    

# 14. IMPORTANT UML DIAGRAM COMPARISON

This comparison is particularly useful for short-answer questions.

|Diagram|Type|Main Purpose|
|---|---|---|
|Class|Structural|Classes, attributes, methods and relationships|
|Sequence|Behavioral|Object interaction in time order|
|Collaboration|Behavioral|Object communication and links|
|Use Case|Behavioral|Actor and system functionality|
|Component|Structural|Software components and dependencies|

### Easy Memory Trick

- Class: What classes exist?
    
- Sequence: What happens first and next?
    
- Collaboration: Which objects communicate?
    
- Use Case: What can the user do?
    
- Component: What modules make up the software?
    

# 15. IMPORTANT DIFFERENCES FOR EXAM

## A. Class Diagram vs Object Diagram

|Class Diagram|Object Diagram|
|---|---|
|Shows classes|Shows objects|
|Represents static design|Represents a snapshot of instances|
|Shows attributes and operations|Shows actual attribute values|
|General structure|Specific example at a particular time|

## B. Sequence vs Use Case Diagram

|Sequence Diagram|Use Case Diagram|
|---|---|
|Shows interaction between objects|Shows interaction between actors and system|
|Focuses on message sequence|Focuses on system functionality|
|Contains lifelines and messages|Contains actors and use cases|
|More detailed interaction view|High-level functional view|

## C. Architectural Design vs Component-Level Design

|Architectural Design|Component-Level Design|
|---|---|
|High-level system design|Detailed internal design|
|Defines major components|Defines individual component logic|
|Focuses on overall structure|Focuses on implementation details|
|Defines component relationships|Defines algorithms and operations|

# 16. LAST-MINUTE REVISION SHEET

Revise before entering the exam hall

```
UNIT 3 — DESIGN ENGINEERING

SOFTWARE DESIGN
Requirements → Design → Implementation

DESIGN PROCESS
1. Data/Class Design
2. Architectural Design
3. Interface Design
4. Component-Level Design
5. Deployment Design

DESIGN QUALITY
Correctness
Simplicity
Modularity
Maintainability
Efficiency
Reusability
Reliability
Testability

DESIGN CONCEPTS
Abstraction
Modularity
Architecture
Information Hiding
Separation of Concerns
High Cohesion
Low Coupling
Refinement
Design Patterns
Refactoring

SOFTWARE ARCHITECTURE
Defines components, responsibilities,
relationships and interactions.

DATA DESIGN
Defines data structures and organization.

ARCHITECTURAL STYLES
1. Data-Centered
2. Data-Flow
3. Call-and-Return
4. Object-Oriented
5. Layered

ARCHITECTURAL PATTERNS
MVC
Client-Server

UML
Unified Modeling Language
Visual modeling of software systems.

CLASS DIAGRAM
Classes + Attributes + Methods + Relationships

SEQUENCE DIAGRAM
Objects + Lifelines + Messages + Time Order

COLLABORATION DIAGRAM
Objects + Links + Numbered Messages

USE CASE DIAGRAM
Actors + Use Cases + System Boundary

COMPONENT DIAGRAM
Components + Interfaces + Dependencies
```

# 17. MOST IMPORTANT QUESTIONS FOR EXAM

Study these in priority order:

|Priority|Question|Important content|
|---|---|---|
|⭐⭐⭐⭐⭐|Explain design concepts and design quality|Abstraction, modularity, cohesion, coupling|
|⭐⭐⭐⭐⭐|Explain software architecture and architectural styles|Five styles with diagrams|
|⭐⭐⭐⭐⭐|Explain UML and its conceptual model|Building blocks and diagram classification|
|⭐⭐⭐⭐⭐|Explain class diagram with example|Class structure and relationships|
|⭐⭐⭐⭐⭐|Explain sequence and collaboration diagrams|Components, diagrams, differences|
|⭐⭐⭐⭐|Explain use case diagram|Actor, use case, include, extend|
|⭐⭐⭐⭐|Explain component diagram|Components, interfaces, dependencies|
|⭐⭐⭐⭐|Explain the software design model|Five design elements|
|⭐⭐⭐|Explain data design and architectural patterns|Data organization, MVC, client-server|

## Exam writing strategy

For a 5-mark answer: Definition + diagram + 3–4 explained points.

For a 10-mark answer: Definition + diagram + components/working + advantages + example.

One important thing: Do not confuse these UML diagrams:

- Class diagram = static structure.
    
- Sequence diagram = time-ordered interaction.
    
- Collaboration diagram = object communication.
    
- Use case diagram = user functionality.
    
- Component diagram = software module organization.
    

These five distinctions are worth memorizing because they make several short-answer and comparison questions easy to attempt.