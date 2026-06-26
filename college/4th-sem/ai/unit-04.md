
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


---


# UNIT–4 : LOGIC

# Topic 3 : Unification ⭐⭐⭐⭐⭐

**(Most Important Topic | Frequently Asked in 5 Marks & 10 Marks Questions)**

> **Exam Weightage:** Very High  
> Unification is the **foundation of First Order Logic inference, Resolution, Forward Chaining, and Backward Chaining.** Understand this topic well because it is used repeatedly in later topics.

---

# Definition

**Unification** is the process of making two logical expressions identical by finding suitable substitutions for their variables.

It is one of the most important techniques used in **First Order Logic (FOL)** to perform logical inference.

### Exam Definition (Write Exactly)

> **Unification** is the process of finding a substitution for variables so that two predicate expressions become identical. The substitution obtained is called a **Unifier**.

---

# Why is Unification Needed?

Suppose our Knowledge Base contains:

```text
Human(Ram)
```

Now we want to prove

```text
Human(x)
```

Here,

```text
x = Ram
```

After substitution,

```text
Human(Ram)
```

Both expressions become identical.

This matching process is called **Unification**.

---

# Need of Unification

Unification is required because:

- Knowledge Base contains variables.
    
- Queries may contain constants.
    
- AI systems must match facts with rules.
    
- Logical inference requires matching predicates.
    

Without unification, AI cannot derive new facts.

---

# Basic Idea of Unification

```text
          Expression 1

          Human(x)

              │
      Substitute x = Ram
              │
              ▼

        Human(Ram)

              ▲

      Human(Ram)

          Expression 2

Both expressions become identical.
```

---

# Important Terms ⭐⭐⭐⭐

## 1. Substitution

Replacing a variable with a constant or another variable.

Example

```text
Student(x)

x = Ram

↓

Student(Ram)
```

---

## 2. Unifier

The substitution that makes two expressions identical.

Example

```text
Likes(x,Mango)

Likes(Ram,Mango)

Unifier = {x/Ram}
```

---

## 3. Most General Unifier (MGU) ⭐⭐⭐⭐⭐

The **Most General Unifier (MGU)** is the simplest substitution that makes two expressions identical without adding unnecessary restrictions.

### Exam Definition

> **Most General Unifier (MGU)** is the simplest substitution that unifies two expressions while keeping them as general as possible.

Example

```text
P(x)

P(Ram)

MGU = {x/Ram}
```

---

# Components of Unification

```text
             Unification

                  │

      ┌───────────┼────────────┐

      │           │            │

 Variables    Constants    Predicates

                  │

            Substitution

                  │

             Matching

                  │

             Inference
```

---

# Rules of Unification ⭐⭐⭐⭐⭐

To unify two expressions, follow these rules.

---

## Rule 1

If both constants are same

Example

```text
Student(Ram)

Student(Ram)
```

Result

✔ Successfully Unified

---

## Rule 2

If constants are different

Example

```text
Student(Ram)

Student(Shyam)
```

Result

✘ Cannot be unified.

---

## Rule 3

Variable can match a constant.

Example

```text
Student(x)

Student(Ram)
```

Substitution

```text
{x/Ram}
```

Unified.

---

## Rule 4

Variable can match another variable.

Example

```text
Student(x)

Student(y)
```

Substitution

```text
{x/y}
```

Unified.

---

## Rule 5

Predicate names must be same.

Example

```text
Student(Ram)

Teacher(Ram)
```

Cannot unify.

Reason

Predicate names differ.

---

## Rule 6

Number of arguments must be same.

Example

```text
Likes(Ram,Mango)

Likes(Ram)
```

Cannot unify.

---

# Unification Algorithm ⭐⭐⭐⭐⭐

### Step 1

Compare predicate names.

If different

→ Fail.

---

### Step 2

Compare number of arguments.

If different

→ Fail.

---

### Step 3

Compare arguments one by one.

- Same constants → Continue
    
- Variable → Substitute
    
- Different constants → Fail
    

---

### Step 4

Store substitutions.

---

### Step 5

Apply substitutions.

---

### Step 6

If expressions become identical

→ Success.

Otherwise

→ Failure.

---

# Flow Diagram of Unification Algorithm

```text
          Start

             │

 Compare Predicate Name

             │

      Same ?──────No

       │            │

      Yes         Failure

       │

Compare Number of Arguments

       │

 Same ?──────No

      │         │

     Yes      Failure

      │

Compare Arguments

      │

Variable ?

      │

 Substitute

      │

Apply Substitution

      │

Expressions Match ?

      │

Yes ─────► Success

No ──────► Failure
```

---

# Solved Examples ⭐⭐⭐⭐⭐

## Example 1

Unify

```text
Student(x)

Student(Ram)
```

Solution

Predicate same ✔

Arguments

```text
x = Ram
```

Result

```text
{x/Ram}
```

Successfully Unified.

---

## Example 2

Unify

```text
Likes(x,Mango)

Likes(Ram,Mango)
```

Solution

Predicate same ✔

Argument 1

```text
x = Ram
```

Argument 2

Same constant.

Result

```text
{x/Ram}
```

Unified.

---

## Example 3

Unify

```text
Parent(x,y)

Parent(Ram,Rohan)
```

Solution

Substitutions

```text
x = Ram

y = Rohan
```

Result

```text
{x/Ram, y/Rohan}
```

Unified.

---

## Example 4

Unify

```text
Teacher(Ram)

Student(Ram)
```

Predicate names differ.

Result

Not Unified.

---

## Example 5

Unify

```text
Father(x,Rohan)

Father(Ram,y)
```

Solution

First argument

```text
x = Ram
```

Second argument

```text
y = Rohan
```

Result

```text
{x/Ram, y/Rohan}
```

Unified.

---

## Example 6

Unify

```text
Likes(Ram,Mango)

Likes(Shyam,Mango)
```

First arguments differ.

Ram ≠ Shyam

Result

Cannot unify.

---

# Real-Life Example ⭐⭐⭐⭐

Knowledge Base

```text
Teacher(Ram)

∀x Teacher(x) → Employee(x)
```

Query

```text
Employee(Ram)
```

Unification

```text
Teacher(x)

Teacher(Ram)
```

Substitution

```text
{x/Ram}
```

Inference

```text
Employee(Ram)
```

---

# Advantages of Unification ⭐⭐⭐⭐

1. Enables logical inference.
    
2. Matches facts with rules.
    
3. Reduces duplicate rules.
    
4. Used in Resolution.
    
5. Used in Forward Chaining.
    
6. Used in Backward Chaining.
    
7. Supports AI reasoning.
    

---

# Limitations of Unification

1. Works only when predicate names match.
    
2. Cannot unify different constants.
    
3. Complex for very large knowledge bases.
    
4. Recursive structures increase complexity.
    

---

# Applications of Unification ⭐⭐⭐⭐

- First Order Logic
    
- Automated Theorem Proving
    
- Resolution Method
    
- Expert Systems
    
- Natural Language Processing
    
- Logic Programming (e.g., Prolog)
    
- Question Answering Systems
    
- AI Reasoning Systems
    

---

# Difference Between Matching and Unification ⭐⭐⭐

|Matching|Unification|
|---|---|
|One-way substitution|Two-way substitution|
|Less flexible|More flexible|
|Used for simple matching|Used for logical inference|
|Simpler|More powerful|

---

# Memory Trick ⭐⭐⭐

Remember:

```text
UNIFY

U → Understand predicates

N → Names should match

I → Identify variables

F → Find substitutions

Y → Yield identical expressions
```

Also remember the **3 Golden Rules**:

```text
Same Predicate ✔

Same Number of Arguments ✔

Variables can be substituted ✔
```

If any one of these fails, **unification fails**.

---



---

# UNIT–4: LOGIC

# Topic: Forward Chaining ⭐⭐⭐⭐⭐

## Definition

**Forward Chaining** is a **data-driven inference technique** in Artificial Intelligence that starts with the **known facts** in the knowledge base and repeatedly applies **IF–THEN rules** to derive new facts until the desired goal is reached or no more rules can be applied.

---

## Working of Forward Chaining

Forward Chaining begins with the facts available in the knowledge base. The inference engine searches for rules whose **IF (condition)** part matches the known facts. When a matching rule is found, it is fired and its **THEN (conclusion)** part is added as a new fact to the knowledge base. This process continues until the goal is achieved or no applicable rule remains.

### Diagram

```text
Known Facts
     │
     ▼
Match IF Condition
     │
     ▼
Apply Rule
     │
     ▼
Generate New Fact
     │
     ▼
Goal Reached?
```

---

## Algorithm

1. Start with the known facts in the Knowledge Base.
    
2. Compare the facts with the IF part of all rules.
    
3. Select the rule whose condition is satisfied.
    
4. Fire the rule and generate a new fact.
    
5. Add the new fact to the Knowledge Base.
    
6. Repeat the process until:
    
    - the goal is reached, or
        
    - no more rules can be applied.
        

---

## Example

### Knowledge Base

```
Student(Ram)
```

### Rules

```
R1: Student(x) → Human(x)

R2: Human(x) → Mortal(x)
```

### Goal

```
Mortal(Ram)
```

### Solution

```
Student(Ram)
      │
      ▼
Human(Ram)
      │
      ▼
Mortal(Ram)
```

Thus, **Ram is Mortal**.

---

## Characteristics

- It is a **data-driven** approach.
    
- Starts from **known facts**.
    
- Uses **IF–THEN production rules**.
    
- Generates new facts until the goal is reached.
    
- Suitable when all facts are already available.
    

---

## Advantages

- Simple and easy to implement.
    
- Automatically derives new knowledge.
    
- Suitable for expert systems and decision support systems.
    
- Can derive multiple conclusions from the same facts.
    
- Efficient when many facts are available.
    

---

## Disadvantages

- May generate many unnecessary facts.
    
- Can be slow for large knowledge bases.
    
- Requires repeated rule matching.
    
- Not efficient when only one specific goal is required.
    

---

## Applications

- Expert Systems
    
- Medical Diagnosis
    
- Fault Detection
    
- Robotics
    
- Decision Support Systems
    
- Weather Prediction
    

---

## Exam Difference (Forward vs Backward Chaining)

|Forward Chaining|Backward Chaining|
|---|---|
|Data-driven|Goal-driven|
|Starts with known facts|Starts with the goal|
|Moves towards conclusion|Moves backwards to facts|
|Suitable when many facts are available|Suitable when a specific goal is to be proved|

---

## 30-Second Revision

```
Forward Chaining

• Data-driven inference technique
• Starts from known facts
• Uses IF–THEN rules
• Generates new facts
• Stops when goal is reached

Flow:
Facts → Rule Matching → New Fact → Goal
```

---

# UNIT–4: LOGIC

# Topic: Backward Chaining ⭐⭐⭐⭐⭐

**(Very Important | Frequently Asked 5 & 10 Marks Question)**

---

# Definition

**Backward Chaining** is a **goal-driven inference technique** in Artificial Intelligence that starts with the **goal (query)** and works backward by searching for rules and facts that can prove the goal. The process continues until the goal is proved or no supporting facts/rules are found.

> **Exam Definition:**  
> **Backward Chaining** is a goal-driven inference method that starts with the desired goal and works backward through inference rules to determine whether the goal can be satisfied using the available facts.

---

# Working of Backward Chaining

Backward Chaining begins with the goal instead of the known facts. The inference engine searches for a rule whose conclusion matches the goal. It then checks whether the conditions (premises) of that rule are true. If they are not known, they become **sub-goals**. This process continues recursively until the required facts are found in the knowledge base or the goal cannot be proved.

### Diagram

```text
           Goal
             │
             ▼
      Find Matching Rule
             │
             ▼
      Check Rule Conditions
             │
     ┌───────┴────────┐
     │                │
 Facts Found      Create Sub-goals
     │                │
     └───────┬────────┘
             ▼
        Goal Proved
```

---

# Algorithm

1. Start with the required goal.
    
2. Search for a rule whose conclusion matches the goal.
    
3. Check whether the rule's conditions are true.
    
4. If conditions are not known, treat them as sub-goals.
    
5. Continue searching until facts are found.
    
6. If all sub-goals are satisfied, the goal is proved; otherwise, it fails.
    

---

# Example

### Knowledge Base

```text
Student(Ram)
```

### Rules

```text
R1: Student(x) → Human(x)

R2: Human(x) → Mortal(x)
```

### Goal

```text
Mortal(Ram)
```

### Solution

To prove **Mortal(Ram)**:

- Look for a rule that concludes `Mortal(x)`.
    
- Rule R2 says: `Human(x) → Mortal(x)`.
    
- Therefore, prove `Human(Ram)`.
    
- Rule R1 says: `Student(x) → Human(x)`.
    
- Check if `Student(Ram)` exists in the Knowledge Base.
    
- Since it exists, `Human(Ram)` is proved.
    
- Hence, **Mortal(Ram)** is also proved.
    

### Inference Diagram

```text
Mortal(Ram)
      ▲
      │
Human(Ram)
      ▲
      │
Student(Ram)
```

---

# Characteristics

- It is a **goal-driven** approach.
    
- Starts from the desired goal.
    
- Searches backward to known facts.
    
- Creates sub-goals when necessary.
    
- Efficient when the goal is specific.
    

---

# Advantages

- Efficient for solving a specific query.
    
- Does not generate unnecessary facts.
    
- Faster when the number of goals is small.
    
- Requires less memory than Forward Chaining.
    
- Widely used in expert systems and diagnosis systems.
    

---

# Disadvantages

- Not suitable when many goals need to be found.
    
- Performance decreases with a large number of rules.
    
- Recursive search may become complex.
    
- May fail if required facts are missing.
    

---

# Applications

- Expert Systems
    
- Medical Diagnosis
    
- Theorem Proving
    
- Fault Diagnosis
    
- Question Answering Systems
    
- Logic Programming (Prolog)
    

---

# Difference Between Forward and Backward Chaining ⭐⭐⭐⭐⭐

|Forward Chaining|Backward Chaining|
|---|---|
|Data-driven approach|Goal-driven approach|
|Starts with known facts|Starts with the goal|
|Moves from facts to conclusion|Moves from goal to facts|
|Generates many new facts|Generates only required facts|
|Suitable when many facts are available|Suitable when a specific goal is to be proved|
|May consume more memory|Requires less memory|

---

# 1-Minute Revision

```text
Backward Chaining

• Goal-driven inference technique
• Starts with the goal
• Searches rules backward
• Creates sub-goals
• Stops when the goal is proved

Flow:
Goal → Rule Matching → Sub-goals → Facts → Goal Proved
```

---
  

**Memory Trick:**

- **Forward Chaining = Facts → Goal (Data-driven)**
    
- **Backward Chaining = Goal → Facts (Goal-driven)**


---


# UNIT–4: LOGIC

# Topic: Resolution ⭐⭐⭐⭐⭐

**(Most Important Topic | Frequently Asked 5 & 10 Marks Question)**

> **Exam Weightage:** ⭐⭐⭐⭐⭐  
> Resolution is the **most important inference rule** in First Order Logic and is widely used in **Automated Theorem Proving, Expert Systems, and AI reasoning**.

---

# Definition

**Resolution** is an inference technique used in Artificial Intelligence to prove whether a statement is true or false. It works by converting all statements into **Clause Form (Conjunctive Normal Form - CNF)** and deriving a contradiction. If a contradiction (empty clause) is obtained, the goal is proved.

### Exam Definition

> **Resolution** is a rule of inference that proves a conclusion by resolving two clauses containing complementary literals. If an empty clause (□) is obtained, the statement is proved.

---

# Basic Idea

Resolution follows the **Proof by Contradiction** method.

Instead of proving the goal directly:

1. Negate the goal.
    
2. Add it to the Knowledge Base.
    
3. Apply the Resolution Rule repeatedly.
    
4. If an **empty clause (□)** is obtained, the original goal is true.
    

### Diagram

```text
Knowledge Base
      │
      ▼
Negate Goal
      │
      ▼
Convert to CNF
      │
      ▼
Apply Resolution
      │
      ▼
Empty Clause (□)?
      │
 ┌────┴────┐
 │         │
Yes       No
 │         │
Goal     Cannot
Proved   Prove
```

---

# Resolution Rule

If two clauses contain complementary literals (P and ¬P), they can be resolved.

General Rule:

```text
(P ∨ Q)

(¬P ∨ R)

────────────

(Q ∨ R)
```

Here, **P** and **¬P** cancel each other, producing the new clause **Q ∨ R**.

---

# Steps of Resolution Algorithm

1. Convert all statements into **Conjunctive Normal Form (CNF)**.
    
2. Negate the goal and add it to the Knowledge Base.
    
3. Select two clauses containing complementary literals.
    
4. Resolve them to produce a new clause.
    
5. Repeat until:
    
    - an **empty clause (□)** is obtained (goal proved), or
        
    - no further resolution is possible.
        

---

# Example 1 (Very Important)

### Knowledge Base

```text
1. Human(Ram)

2. Human(x) → Mortal(x)
```

### Goal

```text
Mortal(Ram)
```

### Step 1: Convert to CNF

Rule:

```text
¬Human(x) ∨ Mortal(x)
```

Negated Goal:

```text
¬Mortal(Ram)
```

Knowledge Base becomes:

```text
Human(Ram)

¬Human(x) ∨ Mortal(x)

¬Mortal(Ram)
```

### Step 2: Apply Resolution

Resolve:

```text
Mortal(Ram)

¬Mortal(Ram)
```

Result:

```text
Human(Ram)
```

Resolve again:

```text
Human(Ram)

¬Human(Ram)
```

Result:

```text
□
```

Since the **empty clause (□)** is obtained, the goal **Mortal(Ram)** is proved.

---

# Example 2

### Clauses

```text
P ∨ Q

¬P
```

Applying Resolution:

```text
P ∨ Q

¬P

────────

Q
```

Thus, the new clause is **Q**.

---

# Characteristics

- Uses **proof by contradiction**.
    
- Works on **CNF clauses**.
    
- Uses complementary literals.
    
- Produces new clauses until proof is obtained.
    
- Fundamental inference method in AI.
    

---

# Advantages

- Simple and systematic inference technique.
    
- Sound and complete for First Order Logic.
    
- Widely used in Automated Theorem Proving.
    
- Efficient for logical reasoning.
    
- Forms the basis of many AI systems.
    

---

# Disadvantages

- All statements must be converted to CNF.
    
- Resolution may generate many intermediate clauses.
    
- Computationally expensive for large knowledge bases.
    
- Difficult to understand for beginners.
    

---

# Applications

- Automated Theorem Proving
    
- Expert Systems
    
- Logic Programming (Prolog)
    
- Knowledge Representation
    
- Question Answering Systems
    
- Artificial Intelligence Reasoning
    

---

# Difference: Resolution vs Forward & Backward Chaining

|Resolution|Forward Chaining|Backward Chaining|
|---|---|---|
|Uses proof by contradiction|Data-driven|Goal-driven|
|Works on CNF clauses|Uses IF–THEN rules|Uses IF–THEN rules|
|Based on complementary literals|Starts with facts|Starts with goal|
|Produces empty clause (□)|Produces new facts|Produces sub-goals|



---

# 1-Minute Revision

```text
Resolution

• Inference rule in AI
• Uses proof by contradiction
• Convert statements to CNF
• Negate the goal
• Apply resolution rule
• Empty clause (□) means goal is proved

Flow:
Knowledge Base
      ↓
Negate Goal
      ↓
Convert to CNF
      ↓
Apply Resolution
      ↓
Empty Clause (□)
      ↓
Goal Proved
```

---
