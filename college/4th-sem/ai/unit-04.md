
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

# University Exam Questions

### 2 Marks

- Define First Order Logic.
    
- What are predicates?
    
- What are quantifiers?
    
- Define universal quantifier.
    
- Define existential quantifier.
    

---

### 5 Marks

- Explain the components of First Order Logic.
    
- Explain universal and existential quantifiers with examples.
    
- Differentiate between propositional logic and First Order Logic.
    

---

### 10 Marks

- Explain First Order Logic with suitable examples.
    
- Describe the syntax, components, advantages, disadvantages, and applications of First Order Logic.
    
- Explain First Order Logic and knowledge representation using examples.
    

---

# One-Page Revision Sheet (Last 5-Minute Revision)

```
FIRST ORDER LOGIC (FOL)

Definition:
Represents objects, properties, and relationships using predicates,
variables, constants, functions, and quantifiers.

Components:
• Constants
• Variables
• Predicates
• Functions
• Quantifiers
• Logical Connectives

Quantifiers:
∀ = For All
∃ = There Exists

Example:
∀x Human(x) → Mortal(x)

Human(Ram)

Therefore,

Mortal(Ram)

Advantages:
✔ More expressive
✔ Supports inference
✔ Represents relationships
✔ Less repetition

Applications:
Knowledge Representation
Expert Systems
Robotics
Medical Diagnosis
NLP
Semantic Web

Difference:
PL → Statements only
FOL → Objects + Relationships + Quantifiers
```

---

## ⭐ Examiner's Tip (Very Important)

For a **10-mark answer**, follow this sequence to maximize marks:

1. Definition
    
2. Need for FOL
    
3. Components (with labeled diagram)
    
4. Explain each component with examples
    
5. Quantifiers (∀ and ∃) with diagrams and examples
    
6. Knowledge representation example
    
7. Advantages
    
8. Disadvantages
    
9. Applications
    
10. Difference between Propositional Logic and FOL
    
11. Conclude with: **"Thus, First Order Logic is a powerful and expressive knowledge representation technique used in Artificial Intelligence to model real-world facts and perform logical reasoning efficiently."**
    

This structure is comprehensive, easy for an examiner to follow, and appropriate for a B.Tech semester examination.