

# Chapter 3 — Basic Syntax in Python (Detailed Viva Notes)

This chapter is EXTREMELY important because every Python program starts from syntax.

If syntax is wrong → program gives error.

In viva, examiners ask many small questions from this chapter because it checks whether your basics are strong or not.

---

# Topics Covered

1. Python Syntax
    
2. print() function
    
3. input() function
    
4. Variables
    
5. Rules of Variables
    
6. Keywords
    
7. Identifiers
    
8. Comments
    
9. Indentation
    
10. Multiple Assignment
    
11. Taking User Input
    
12. Output Formatting
    
13. Escape Characters
    
14. Basic Errors
    

---

# 1. What is Syntax?

Syntax means:

> Rules for writing programs.

Just like English grammar has rules, programming languages also have rules.

---

# Example

Correct:

```python
print("Hello")
```

Wrong:

```python
print"Hello"
```

Wrong syntax gives:

```text
SyntaxError
```

---

# Viva Answer

## Q. What is syntax?

> Syntax is the set of rules used to write valid programs in a programming language.

---

# 2. print() Function

`print()` is used to display output on screen.

---

# Syntax

```python
print(value)
```

---

# Example

```python
print("Hello World")
```

Output:

```text
Hello World
```

---

# Multiple Prints

```python
print("Python")
print("Programming")
```

Output:

```text
Python
Programming
```

---

# Printing Variables

```python
name = "Arpan"
print(name)
```

---

# Printing Multiple Values

```python
a = 10
b = 20
print(a, b)
```

Output:

```text
10 20
```

---

# Separator in print()

By default separator is space.

```python
print(1, 2, 3)
```

Output:

```text
1 2 3
```

---

# Custom Separator

```python
print(1, 2, 3, sep="-")
```

Output:

```text
1-2-3
```

---

# end Parameter

Normally print ends with newline.

```python
print("Hello")
print("World")
```

Output:

```text
Hello
World
```

---

# Changing end

```python
print("Hello", end=" ")
print("World")
```

Output:

```text
Hello World
```

---

# Viva Questions

## Q. What is print() function?

> print() is a built-in function used to display output on screen.

---

## Q. What is default separator in print()?

> Space is the default separator.

---

# 3. input() Function

`input()` is used to take input from user.

---

# Syntax

```python
input("message")
```

---

# Example

```python
name = input("Enter your name: ")
print(name)
```

---

# How it Works

```text
User enters value
        ↓
Stored in variable
        ↓
Can be used later
```

---

# Important Point

`input()` always returns STRING datatype.

---

# Example

```python
age = input("Enter age: ")
print(type(age))
```

Output:

```python
<class 'str'>
```

---

# Converting Input

```python
age = int(input("Enter age: "))
```

---

# Viva Answer

## Q. What does input() return?

> input() returns data in string format by default.

---

# 4. Variables

Variables are containers used to store data.

---

# Real-Life Analogy

Think of variable as a box.

```text
Box Name → Stores Value
```

Example:

```python
age = 20
```

Here:

- `age` → variable name
    
- `20` → value
    

---

# Creating Variables

```python
name = "Arpan"
marks = 95
price = 99.5
```

---

# Dynamic Typing

Python automatically detects datatype.

```python
x = 10
x = "Hello"
```

---

# Variable Naming Rules

## Allowed

```python
name
_age
student1
```

---

## Not Allowed

```python
1name
my name
class
```

---

# Rules

- Cannot start with number
    
- No spaces
    
- No special symbols except `_`
    
- Cannot use keywords
    

---

# Case Sensitive

```python
age ≠ Age
```

Both are different variables.

---

# Viva Questions

## Q. What is variable?

> Variable is a named memory location used to store data.

---

## Q. Is Python case-sensitive?

> Yes, Python is case-sensitive.

---

# 5. Keywords

Keywords are reserved words.

They already have predefined meaning.

---

# Examples

```python
if
else
for
while
class
def
return
```

---

# Checking Keywords

```python
import keyword
print(keyword.kwlist)
```

---

# Viva Answer

## Q. What are keywords?

> Keywords are reserved words that cannot be used as variable names.

---

# 6. Identifiers

Identifiers are names used for:

- Variables
    
- Functions
    
- Classes
    

---

# Example

```python
student_name = "Arpan"
```

`student_name` is identifier.

---

# Rules for Identifiers

✅ Can contain:

- Letters
    
- Digits
    
- Underscore
    

❌ Cannot:

- Start with digit
    
- Contain spaces
    
- Use keywords
    

---

# Good Identifier Examples

```python
name
student_age
totalMarks
```

---

# Bad Identifier Examples

```python
1name
my-name
class
```

---

# Viva Difference

## Keyword vs Identifier

|Keyword|Identifier|
|---|---|
|Reserved word|User-defined name|
|Fixed meaning|Custom meaning|
|Cannot be changed|Can be created|

---

# 7. Comments

Comments are notes for programmers.

Interpreter ignores comments.

---

# Why Comments are Used?

- Improve readability
    
- Explain code
    
- Help debugging
    

---

# Single Line Comment

```python
# This is comment
```

---

# Multi-Line Comment

```python
"""
This is
multi-line comment
"""
```

---

# Viva Answer

## Q. Why comments are important?

> Comments improve code readability and help explain program logic.

---

# 8. Indentation (VERY IMPORTANT)

Python uses indentation instead of braces `{}`.

Indentation means spaces before code.

---

# Correct Example

```python
if True:
    print("Hello")
```

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

# Why Important?

Indentation tells Python:

- Which code belongs to which block
    

---

# Real-Life Analogy

Indentation is like paragraph structure in English.

Without structure → confusion.

---

# Viva Answer

## Q. Why indentation is important in Python?

> Indentation defines blocks of code and improves readability.

---

# 9. Multiple Assignment

Python allows assigning multiple variables together.

---

# Example 1

```python
a, b, c = 10, 20, 30
```

---

# Example 2

```python
x = y = z = 100
```

---

# Swapping Variables

```python
a = 10
b = 20

a, b = b, a
```

---

# Output

```python
a = 20
b = 10
```

---

# Viva Question

## Q. How to swap two variables without temporary variable?

```python
a, b = b, a
```

---

# 10. Output Formatting

---

# Using f-string

```python
name = "Arpan"
age = 20

print(f"My name is {name} and age is {age}")
```

---

# Older Method

```python
print("My name is", name)
```

---

# Viva Answer

## Q. What is f-string?

> f-string is a modern method for formatting strings in Python.

---

# 11. Escape Characters

Escape characters start with `\`.

---

# Common Escape Characters

|Escape|Meaning|
|---|---|
|`\n`|New line|
|`\t`|Tab|
|`\"`|Double quote|
|`\\`|Backslash|

---

# Example

```python
print("Hello\nWorld")
```

Output:

```text
Hello
World
```

---

# 12. Basic Errors in Python

---

# (1) Syntax Error

Wrong syntax.

```python
print("Hello"
```

---

# (2) Name Error

Undefined variable.

```python
print(age)
```

---

# (3) Indentation Error

Wrong spacing.

---

# (4) Type Error

Wrong datatype operation.

```python
"2" + 2
```

---

# Viva Answer

## Q. What is SyntaxError?

> SyntaxError occurs when program syntax rules are violated.

---

# Important Programs

# Program 1 — User Input

```python
name = input("Enter your name: ")
print("Welcome", name)
```

---

# Program 2 — Addition

```python
a = int(input("Enter first number: "))
b = int(input("Enter second number: "))

print("Sum =", a + b)
```

---

# Program 3 — Swap Variables

```python
a = 10
b = 20

a, b = b, a

print(a, b)
```

---

# Most Important Viva Questions

# Q1. Difference between syntax error and logical error?

|Syntax Error|Logical Error|
|---|---|
|Program does not run|Program runs wrong output|
|Grammar mistake|Logic mistake|

---

# Q2. Difference between variable and identifier?

|Variable|Identifier|
|---|---|
|Stores value|Name given|
|Example: 10|Example: age|

---

# Q3. Why Python is case-sensitive?

> Because uppercase and lowercase letters are treated differently.

---

# Q4. What is indentation?

> Indentation is the spaces before code used to define blocks in Python.

---

# Q5. What is the use of comments?

> Comments help explain code and improve readability.

---

# Output Prediction Questions

# Q1

```python
a = 10
b = 20
print(a + b)
```

Output:

```text
30
```

---

# Q2

```python
x = input("Enter:")
print(type(x))
```

Output:

```python
<class 'str'>
```

---

# Q3

```python
print("Hello", end=" ")
print("World")
```

Output:

```text
Hello World
```

---

# Memory Tricks

# Remember Variable Rules

## “NSSK”

|Letter|Meaning|
|---|---|
|N|No number first|
|S|No spaces|
|S|No special symbols|
|K|No keywords|

---

# One-Minute Revision Notes

- Syntax = rules of language
    
- print() displays output
    
- input() takes user input
    
- input() returns string
    
- Variables store data
    
- Keywords are reserved words
    
- Identifiers are user-defined names
    
- Python is case-sensitive
    
- Comments improve readability
    
- Indentation defines code blocks
    

---

# Examiner Impression Tip

When examiner asks:  
“What is indentation?”

Do NOT say only:

> “Spaces before code.”

Say:

> “Indentation is the spaces before statements used to define blocks of code in Python. Python uses indentation instead of braces.”

This sounds much more professional.

Now send:

- “Chapter 4”  
    OR
    
- any topic you want deeper explanation for.