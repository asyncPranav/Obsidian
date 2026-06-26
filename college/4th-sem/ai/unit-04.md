
---


# UNIT–4 : LOGIC

# Topic 1 : First Order Logic (FOL) ⭐⭐⭐⭐⭐

**(Most Important Topic | 5 Marks + 10 Marks | Frequently Asked in University Exams)**

---

# Definition

**First Order Logic (FOL)**, also called **First Order Predicate Logic (FOPL)**, is a knowledge representation technique used in Artificial Intelligence to represent facts, objects, properties, and relationships between objects.

Unlike propositional logic, which represents only true or false statements, **First Order Logic provides more expressive power by using variables, predicates, functions, and quantifiers.**

### Exam Definition (Write exactly)

> **First Order Logic (FOL)** is a formal knowledge representation language used in Artificial Intelligence to describe objects, their properties, and relationships using predicates, variables, functions, constants, and quantifiers. It allows reasoning about individual objects in a domain and is more expressive than propositional logic.

---

# Why First Order Logic is Needed?

Suppose we know:

- Ram is a student.
    
- Shyam is a student.
    
- Mohan is a student.
    

In **Propositional Logic**, we write:

```
P = Ram is a student
Q = Shyam is a student
R = Mohan is a student
```

Each fact must be written separately.

If there are **100 students**, then we need **100 different propositions**.

This is inefficient.

Instead, FOL writes:

```
Student(x)
```

where **x** can represent any person.

Hence, one statement can represent all students.

---

# Features of First Order Logic

1. Represents objects individually.
    
2. Represents relationships among objects.
    
3. Uses variables.
    
4. Uses quantifiers.
    
5. Supports logical reasoning.
    
6. More expressive than propositional logic.
    
7. Widely used in Expert Systems and Knowledge Representation.
    

---

# 1. Constants

Constants represent **specific objects**.

Examples

```
Ram
India
Delhi
Dog1
Book
```

Example

```
Student(Ram)
```

means

**Ram is a student.**

---

# 2. Variables

Variables represent unknown objects.

Examples

```
x
y
z
a
b
```

Example

```
Student(x)
```

means

x can be any student.

---

# 3. Predicates ⭐⭐⭐⭐

Predicates describe **properties** or **relationships**.

General form

```
Predicate(Object)

OR

Predicate(Object1,Object2)
```

Examples

```
Student(Ram)

Likes(Ram,Mango)

Father(Ram,Rohan)

Tall(Shyam)
```

---

# 4. Functions

Functions return another object.

General form

```
Function(x)
```

Examples

```
FatherOf(Ram)

MotherOf(Shyam)

Capital(India)
```

Example

```
LivesIn(FatherOf(Ram),Delhi)
```

---

# 5. Logical Connectives

Used to combine logical statements.

|Symbol|Meaning|
|---|---|
|∧|AND|
|∨|OR|
|¬|NOT|
|→|IMPLIES|
|↔|IF AND ONLY IF|

Example

```
Student(Ram) ∧ Intelligent(Ram)
```

means

Ram is a student **AND** intelligent.

---

# 6. Quantifiers ⭐⭐⭐⭐⭐

Quantifiers are the most important part of First Order Logic.

There are two types.

---

## (A) Universal Quantifier (∀)

Symbol

```
∀
```

Meaning

"For all"

General form

```
∀x P(x)
```

Meaning

P is true for every x.

Example

```
∀x Student(x) → Human(x)
```

Meaning

**All students are humans.**

Diagram

```
Universe

+----------------------------------+

Student Student Student Student

        ↓

      Human Human Human Human

+----------------------------------+

Applies to ALL objects.
```

---

## (B) Existential Quantifier (∃)

Symbol

```
∃
```

Meaning

"There exists"

General form

```
∃x P(x)
```

Meaning

There exists at least one x satisfying P.

Example

```
∃x Student(x)
```

Meaning

There is at least one student.

Diagram

```
Students

A

B

C  ← satisfies condition

D

Only ONE object is sufficient.
```

---

# Syntax of First Order Logic

General structure

```
Predicate(Term)

Term

↓

Constant
Variable
Function
```

Examples

```
Student(Ram)

Likes(Ram,Mango)

FatherOf(Ram)

∀x Student(x)

∃x Loves(x,Rose)
```

---

# Example of First Order Logic ⭐⭐⭐⭐⭐

Consider the following statements.

1. Every human is mortal.
    
2. Ram is human.
    

Represent them in FOL.

Solution

```
Human(Ram)

∀x Human(x) → Mortal(x)
```

Inference

```
Mortal(Ram)
```

This is one of the most common exam examples.

---

# Another Example

Statement

All birds can fly.

Representation

```
∀x Bird(x) → Fly(x)
```

Statement

Parrot is a bird.

```
Bird(Parrot)
```

Inference

```
Fly(Parrot)
```

---

# Knowledge Representation in FOL

Example Knowledge Base

```
Human(Ram)

Human(Shyam)

Male(Ram)

Female(Sita)

Parent(Ram,Rohan)

Student(Rohan)

∀x Student(x) → Human(x)

∀x Human(x) → Mortal(x)
```

From this knowledge base we infer

```
Human(Rohan)

Mortal(Rohan)

Mortal(Ram)
```

---

# Advantages of First Order Logic ⭐⭐⭐⭐

1. Very expressive language.
    
2. Represents objects and relationships.
    
3. Supports inference.
    
4. Reduces repetition.
    
5. Easy to represent real-world knowledge.
    
6. Used in Expert Systems and AI reasoning.
    
7. Supports variables and quantifiers.
    

---

# Disadvantages of First Order Logic

1. Complex compared to propositional logic.
    
2. Requires more computation.
    
3. Difficult for very large knowledge bases.
    
4. Cannot directly represent uncertainty.
    
5. Needs inference algorithms for reasoning.
    

---

# Applications of First Order Logic ⭐⭐⭐⭐

- Knowledge Representation
    
- Expert Systems
    
- Medical Diagnosis
    
- Robotics
    
- Natural Language Processing
    
- Semantic Web
    
- Question Answering Systems
    
- Automated Reasoning
    

---

# Difference Between Propositional Logic and First Order Logic ⭐⭐⭐⭐⭐

|Propositional Logic|First Order Logic|
|---|---|
|Represents complete statements|Represents objects, properties, and relations|
|Less expressive|More expressive|
|Does not use variables|Uses variables|
|No quantifiers|Uses ∀ and ∃ quantifiers|
|Cannot represent relationships|Can represent relationships|
|Easier to understand|Slightly more complex|
|Example: P = Ram is student|Student(Ram)|

---

# Memory Trick ⭐⭐⭐

Remember the five basic components as:

**CVPFQ**

```
C → Constants

V → Variables

P → Predicates

F → Functions

Q → Quantifiers
```

For Quantifiers:

```
∀  → ALL

∃  → EXISTS
```

---

# UNIT–4 : LOGIC

# Topic 2 : Propositional Logic vs First Order Logic (FOL) ⭐⭐⭐⭐⭐

**(Very Important | Frequently Asked 5 Marks & 10 Marks Question)**

---

# Definition

## Propositional Logic (PL)

**Propositional Logic** is the simplest form of logic in which knowledge is represented using **propositions (statements)** that are either **True or False**. It does not describe the internal structure of a statement.

### Exam Definition

> **Propositional Logic** is a formal logic in which knowledge is represented using propositions (statements) having only two truth values: **True** or **False**.

---

## First Order Logic (FOL)

**First Order Logic (FOL)** is an extension of propositional logic that represents **objects, their properties, and relationships** using predicates, variables, constants, functions, and quantifiers.

### Exam Definition

> **First Order Logic (FOL)** is a knowledge representation language that represents objects, properties, relationships, and supports logical reasoning using predicates, variables, constants, functions, and quantifiers.

---

# Why is FOL Needed?

Consider the following facts:

- Ram is a student.
    
- Shyam is a student.
    
- Mohan is a student.
    

### In Propositional Logic

We write:

```text
P = Ram is a student

Q = Shyam is a student

R = Mohan is a student
```

Every statement must be written separately.

If there are **100 students**, we need **100 propositions**.

---

### In First Order Logic

We write

```text
Student(x)
```

where **x** can be Ram, Shyam, Mohan, or any student.

Only **one expression** represents all students.

Hence, FOL is more powerful.

---

# Basic Difference Diagram ⭐⭐⭐⭐

```text
                 KNOWLEDGE REPRESENTATION

                     ┌───────────────┐
                     │    LOGIC      │
                     └──────┬────────┘
                            │
          ┌─────────────────┴─────────────────┐
          │                                   │
          ▼                                   ▼
  Propositional Logic                 First Order Logic

   Whole Statements                 Objects + Relationships

      P, Q, R                     Student(Ram), Likes(x,y)

   Less Expressive                 Highly Expressive
```

---

# Example Comparison ⭐⭐⭐⭐⭐

### Statement

All students are intelligent.

---

## Propositional Logic

Suppose there are only three students.

```text
P = Ram is intelligent

Q = Shyam is intelligent

R = Mohan is intelligent
```

Need to write

```text
P ∧ Q ∧ R
```

If another student comes, a new proposition must be added.

---

## First Order Logic

Simply write

```text
∀x Student(x) → Intelligent(x)
```

This represents **every student**, even future students.

Hence, FOL is flexible.

---

# Detailed Comparison Table ⭐⭐⭐⭐⭐

|Basis|Propositional Logic|First Order Logic|
|---|---|---|
|Definition|Represents complete statements|Represents objects, properties, and relationships|
|Representation|Uses propositions|Uses predicates|
|Variables|Not used|Used|
|Constants|Not available|Available|
|Functions|Not available|Available|
|Quantifiers|Not available|Uses ∀ and ∃|
|Relationships|Cannot represent relationships|Can represent relationships|
|Expressiveness|Less expressive|More expressive|
|Knowledge Representation|Represents simple facts|Represents complex facts|
|Reusability|Low|High|
|Inference Power|Limited|Powerful|
|Real-world Modelling|Difficult|Easy|
|Memory Requirement|More statements required|Less repetition|
|Complexity|Simple|Slightly complex|
|Applications|Simple logical problems|AI, Expert Systems, Robotics, NLP|

---

# Symbol Comparison

|Feature|PL|FOL|
|---|---|---|
|Proposition|✔|✔|
|Predicate|✘|✔|
|Variable|✘|✔|
|Constant|✘|✔|
|Function|✘|✔|
|Universal Quantifier (∀)|✘|✔|
|Existential Quantifier (∃)|✘|✔|

---

# Real-Life Example ⭐⭐⭐⭐

### Statement

Every teacher teaches students.

---

### Propositional Logic

```text
P = Ram teaches students

Q = Mohan teaches students

R = Shyam teaches students
```

Need separate propositions.

---

### First Order Logic

```text
∀x Teacher(x) → Teaches(x,Students)
```

One expression represents every teacher.

---

# Another Example

### Statement

Ram likes Mango.

---

### Propositional Logic

```text
P = Ram likes Mango
```

Only one statement.

Cannot identify

- Ram
    
- Mango
    
- Relationship
    

---

### First Order Logic

```text
Likes(Ram,Mango)
```

Here,

- Ram → Object
    
- Mango → Object
    
- Likes → Relationship
    

Thus FOL stores meaningful information.

---

# Advantages of Propositional Logic

1. Simple to understand.
    
2. Easy to implement.
    
3. Fast logical operations.
    
4. Suitable for simple problems.
    

---

# Limitations of Propositional Logic ⭐⭐⭐⭐

1. Cannot represent objects individually.
    
2. Cannot represent relationships.
    
3. Cannot use variables.
    
4. Cannot use quantifiers.
    
5. Large knowledge base requires many propositions.
    
6. Less expressive.
    

---

# Advantages of First Order Logic ⭐⭐⭐⭐

1. Represents objects individually.
    
2. Represents relationships.
    
3. Supports variables.
    
4. Uses quantifiers.
    
5. Supports inference.
    
6. Less repetition.
    
7. More expressive.
    
8. Suitable for AI applications.
    

---

# Limitations of First Order Logic

1. More complex than propositional logic.
    
2. Requires more computation.
    
3. Needs inference algorithms.
    
4. Harder to implement.
    

---

# Applications Comparison

### Propositional Logic

- Digital circuits
    
- Boolean reasoning
    
- Simple decision systems
    
- Basic theorem proving
    

---

### First Order Logic

- Knowledge Representation
    
- Expert Systems
    
- Medical Diagnosis
    
- Robotics
    
- Natural Language Processing
    
- Semantic Web
    
- Automated Reasoning
    
- Intelligent Agents
    

---

# Memory Trick ⭐⭐⭐

Remember:

### **PL**

```text
P = Plain Statements
```

Only statements.

---

### **FOL**

```text
F = Facts

O = Objects

L = Links (Relationships)
```

Think of **FOL = Facts + Objects + Links**.

---

# Exam-Oriented Summary

```text
PROPOSITIONAL LOGIC

• Represents statements
• True or False
• No variables
• No quantifiers
• Less expressive
• Simple reasoning

FIRST ORDER LOGIC

• Represents objects
• Represents relationships
• Uses predicates
• Uses variables
• Uses quantifiers
• More expressive
• Powerful inference
```

---

# Frequently Asked University Questions

### 2 Marks

- Define Propositional Logic.
    
- Define First Order Logic.
    
- What is the main difference between PL and FOL?
    

---

### 5 Marks

- Differentiate between Propositional Logic and First Order Logic.
    
- Explain why First Order Logic is more expressive than Propositional Logic.
    

---

### 10 Marks

- Compare Propositional Logic and First Order Logic with suitable examples.
    
- Explain the differences between PL and FOL along with their advantages, limitations, and applications.
    

---

# 5-Mark Answer Format (Exam Ready)

**Q. Differentiate between Propositional Logic and First Order Logic.**

|Propositional Logic|First Order Logic|
|---|---|
|Represents complete statements.|Represents objects, properties, and relationships.|
|Uses propositions (P, Q, R).|Uses predicates such as Student(x).|
|Does not use variables.|Uses variables (x, y, z).|
|Does not use quantifiers.|Uses ∀ (for all) and ∃ (there exists).|
|Less expressive.|More expressive.|
|Cannot represent relationships.|Can represent relationships.|
|Suitable for simple logical problems.|Suitable for complex AI applications.|
|Easier to implement.|More complex but more powerful.|

**Conclusion:**  
**Thus, First Order Logic is an extension of Propositional Logic that provides greater expressive power by representing objects, their properties, and relationships. Therefore, FOL is widely used in Artificial Intelligence for knowledge representation and logical reasoning.**

---

## ⭐ Examiner's Tip

For a **10-mark question**, don't write only the comparison table. Write in this order:

1. Definition of Propositional Logic
    
2. Definition of First Order Logic
    
3. Why FOL is needed (with a simple example)
    
4. One comparison diagram
    
5. Detailed comparison table (8–14 points)
    
6. One real-life example comparing both
    
7. Advantages and limitations of each
    
8. Applications
    
9. A 2–3 line conclusion
    

This structure is complete, easy to remember, and matches the style expected in most B.Tech semester examinations.