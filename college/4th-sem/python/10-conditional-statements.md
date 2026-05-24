
---

# Python Conditional Statements (Detailed Viva Notes)

Conditional statements are used for:

- Decision making
    
- Controlling program flow
    
- Executing code based on conditions
    

This is one of the MOST IMPORTANT topics for:

- Viva
    
- Practical
    
- Logic building
    
- Programs
    

Examiners commonly ask:

- What is conditional statement?
    
- Difference between if and elif
    
- Difference between nested if and ladder if
    
- What is indentation?
    
- Difference between `=` and `==`
    
- Output prediction questions
    

This chapter is VERY important.

---

# 1. What is Conditional Statement?

Conditional statements are:

> Statements used to execute code based on conditions.

---

# Real-Life Analogy

Example:

```text
If it rains → take umbrella
Else → do not take umbrella
```

Decision depends on condition.

Same in programming.

---

# Viva Definition

## Q. What is conditional statement?

> Conditional statements control execution flow based on conditions.

---

# Why Conditional Statements Are Used?

Used for:

- Decision making
    
- Validation
    
- Comparing values
    
- User authentication
    
- Checking conditions
    

---

# Types of Conditional Statements

|Statement|Purpose|
|---|---|
|`if`|Single condition|
|`if-else`|Two conditions|
|`if-elif-else`|Multiple conditions|
|Nested `if`|`if` inside `if`|

---

# 2. if Statement

Executes block only if condition is True.

---

# Syntax

```python
if condition:
    statement
```

---

# Example

```python
age = 18

if age >= 18:
    print("Eligible")
```

Output:

```text
Eligible
```

---

# Important Point

If condition becomes False:

- Block will NOT execute.
    

---

# Example

```python
age = 15

if age >= 18:
    print("Eligible")
```

Output:

```text
No output
```

---

# Flow of if Statement

```text
Condition True → Execute block
Condition False → Skip block
```

---

# Viva Question

## Q. What is if statement?

> if statement executes block only when condition is True.

---

# 3. Indentation (VERY IMPORTANT)

Python uses indentation instead of braces `{}`.

---

# Example

```python
if True:
    print("Hello")
```

Spaces before `print()` are indentation.

---

# Wrong Example

```python
if True:
print("Hello")
```

Error:

```text
IndentationError
```

---

# Important Point

Usually:

- 4 spaces used
    

---

# Viva Question (MOST ASKED)

## Q. What is indentation in Python?

> Indentation means spaces used to define code blocks.

---

# 4. if-else Statement

Used when:

- One block executes for True
    
- Another executes for False
    

---

# Syntax

```python
if condition:
    statements
else:
    statements
```

---

# Example

```python
age = 16

if age >= 18:
    print("Adult")
else:
    print("Minor")
```

Output:

```text
Minor
```

---

# Flow

```text
Condition True → if block
Condition False → else block
```

---

# Viva Question

## Q. What is if-else statement?

> if-else executes one block for True and another for False.

---

# 5. if-elif-else Ladder

Used for multiple conditions.

---

# Syntax

```python
if condition1:
    statements
elif condition2:
    statements
else:
    statements
```

---

# Example

```python
marks = 75

if marks >= 90:
    print("A")
elif marks >= 75:
    print("B")
elif marks >= 50:
    print("C")
else:
    print("Fail")
```

Output:

```text
B
```

---

# Important Rule

Python checks conditions from TOP to BOTTOM.

First True condition executes.

---

# Flow

```text
Check condition1
↓
If False → check condition2
↓
If False → check condition3
↓
Else executes
```

---

# Viva Question

## Q. Why use elif?

> elif is used to check multiple conditions sequentially.

---

# Difference Between if and elif

|if|elif|
|---|---|
|Independent condition|Connected condition|
|Multiple if can execute|Only one block executes|

---

# Example

## Multiple if

```python
x = 10

if x > 5:
    print("A")

if x > 7:
    print("B")
```

Output:

```text
A
B
```

Both execute.

---

# Example with elif

```python
x = 10

if x > 5:
    print("A")
elif x > 7:
    print("B")
```

Output:

```text
A
```

Only first block executes.

---

# Viva Question (IMPORTANT)

## Q. Difference between if and elif?

> Multiple if blocks can execute, but in if-elif ladder only one matching block executes.

---

# 6. Nested if Statement

if statement inside another if.

---

# Syntax

```python
if condition1:
    if condition2:
        statements
```

---

# Example

```python
age = 20
citizen = True

if age >= 18:
    if citizen:
        print("Eligible to vote")
```

Output:

```text
Eligible to vote
```

---

# Viva Question

## Q. What is nested if?

> Nested if means an if statement inside another if statement.

---

# Difference Between Nested if and if-elif Ladder

|Nested if|if-elif|
|---|---|
|if inside if|Multiple conditions|
|Used for dependent conditions|Used for alternatives|

---

# 7. Short-Hand if Statement

Single-line if.

---

# Syntax

```python
if condition: statement
```

---

# Example

```python
x = 10

if x > 5: print("Hello")
```

---

# 8. Short-Hand if-else (Ternary Operator)

---

# Syntax

```python
value_if_true if condition else value_if_false
```

---

# Example

```python
age = 20

result = "Adult" if age >= 18 else "Minor"

print(result)
```

Output:

```text
Adult
```

---

# Viva Question

## Q. What is ternary operator?

> Ternary operator is shorthand form of if-else.

---

# 9. Truthy and Falsy Values (IMPORTANT)

Python treats some values as False automatically.

---

# False Values

```python
False
0
0.0
''
[]
{}
None
```

---

# Example

```python
if 0:
    print("Hello")
else:
    print("False")
```

Output:

```text
False
```

---

# Viva Question

## Q. What are falsy values?

> Values treated as False in conditions are called falsy values.

---

# 10. Logical Operators in Conditions

---

# AND Example

```python
age = 20

if age >= 18 and age <= 60:
    print("Valid")
```

---

# OR Example

```python
x = 5

if x == 5 or x == 10:
    print("Matched")
```

---

# NOT Example

```python
if not False:
    print("Hello")
```

---

# 11. Common Errors

---

# (1) IndentationError

```python
if True:
print("Hello")
```

---

# (2) SyntaxError

```python
if x > 5
    print(x)
```

Missing colon `:`.

---

# (3) Using `=` instead of `==`

Wrong:

```python
if x = 5:
```

Correct:

```python
if x == 5:
```

---

# Viva Question (MOST ASKED)

## Q. Difference between `=` and `==`?

> `=` assigns value while `==` compares values.

---

# 12. Important Programs

# Program 1 — Even/Odd

```python
num = 8

if num % 2 == 0:
    print("Even")
else:
    print("Odd")
```

---

# Program 2 — Largest Number

```python
a = 10
b = 20

if a > b:
    print("A is greater")
else:
    print("B is greater")
```

---

# Program 3 — Grade Calculator

```python
marks = 85

if marks >= 90:
    print("A")
elif marks >= 75:
    print("B")
else:
    print("C")
```

---

# 13. Output Prediction Questions

# Q1

```python
x = 10

if x > 5:
    print("A")
else:
    print("B")
```

Output:

```text
A
```

---

# Q2

```python
x = 5

if x > 10:
    print("A")
elif x > 3:
    print("B")
else:
    print("C")
```

Output:

```text
B
```

---

# Q3

```python
if 0:
    print("Hello")
else:
    print("World")
```

Output:

```text
World
```

---

# Q4

```python
if True:
    if False:
        print("A")
    else:
        print("B")
```

Output:

```text
B
```

---

# 14. Most Important Viva Questions

# Q1. What is conditional statement?

> Conditional statements execute code based on conditions.

---

# Q2. What is indentation?

> Indentation defines code blocks using spaces.

---

# Q3. Difference between if and elif?

VERY IMPORTANT.

---

# Q4. Difference between nested if and if-elif ladder?

VERY IMPORTANT.

---

# Q5. Difference between `=` and `==`?

MOST IMPORTANT.

---

# Q6. What is ternary operator?

> Short-hand form of if-else.

---

# Q7. What are falsy values?

> Values treated as False automatically.

---

# 15. One-Minute Revision Notes

- if → single condition
    
- if-else → two conditions
    
- if-elif-else → multiple conditions
    
- nested if → if inside if
    
- Python uses indentation
    
- `:` required after condition
    
- `==` for comparison
    
- `=` for assignment
    
- elif ladder executes one block only
    

---

# Memory Trick

## “IEN”

|Letter|Statement|
|---|---|
|I|if|
|E|elif|
|N|Nested if|

---

# Examiner Impression Tip

If examiner asks:  
“What is indentation?”

Do NOT say:

> “Spaces before code.”

Say:

> “Indentation is the whitespace used to define block structure in Python.”

This sounds professional and strong in viva.

Next important topics:

- Loops (VERY IMPORTANT)
    
- Functions
    
- Strings in detail
    
- OOPs
    
- Exception Handling
    
- File Handling