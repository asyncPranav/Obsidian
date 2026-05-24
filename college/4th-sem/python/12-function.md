
---

# Python Functions (Detailed Viva Notes)

Functions are one of the MOST IMPORTANT topics in Python viva and practical.

Almost every examiner asks questions from functions because they are the foundation of:

- Modular programming
    
- Code reusability
    
- Real projects
    
- Backend development
    

Common viva questions:

- What is function?
    
- Difference between function and method
    
- Types of functions
    
- Parameters vs arguments
    
- Return vs print
    
- Recursion
    
- Lambda function
    
- Local vs global variable
    

This chapter is EXTREMELY IMPORTANT.

---

# 1. What is Function?

Function is:

> A block of reusable code designed to perform a specific task.

---

# Real-Life Analogy

Think of a calculator button.

Example:

```text
Addition button
```

Whenever pressed:

- Same task performed repeatedly.
    

Similarly:

- Functions perform reusable tasks.
    

---

# Viva Definition

## Q. What is function?

> Function is reusable block of code that performs specific task.

---

# Why Functions Are Used?

Functions help:

- Reduce code repetition
    
- Improve readability
    
- Make debugging easier
    
- Divide program into modules
    

---

# Example Without Function

```python
a = 10
b = 20
print(a + b)

x = 5
y = 7
print(x + y)
```

Repeated logic.

---

# Example With Function

```python
def add(a, b):
    print(a + b)

add(10, 20)
add(5, 7)
```

Reusable.

---

# 2. Types of Functions

|Type|Meaning|
|---|---|
|Built-in functions|Already provided by Python|
|User-defined functions|Created by programmer|

---

# Built-in Function Examples

```python
print()
len()
type()
sum()
max()
```

---

# User-Defined Function

Created using `def`.

---

# Syntax

```python
def function_name():
    statements
```

---

# Example

```python
def greet():
    print("Hello")

greet()
```

Output:

```text
Hello
```

---

# Important Terms

|Term|Meaning|
|---|---|
|Function definition|Creating function|
|Function call|Executing function|

---

# Viva Question

## Q. Difference between function definition and function call?

> Function definition creates function while function call executes it.

---

# 3. Function with Parameters

Parameters receive values.

---

# Syntax

```python
def function_name(parameter):
    statements
```

---

# Example

```python
def greet(name):
    print("Hello", name)

greet("Arpan")
```

Output:

```text
Hello Arpan
```

---

# 4. Parameters vs Arguments (MOST ASKED)

|Parameter|Argument|
|---|---|
|Variable in function definition|Actual value passed|

---

# Example

```python
def add(a, b):
    print(a + b)

add(10, 20)
```

Here:

- `a, b` → parameters
    
- `10, 20` → arguments
    

---

# Viva Question (MOST IMPORTANT)

## Q. Difference between parameter and argument?

> Parameters are variables in function definition while arguments are actual values passed during function call.

---

# 5. Types of Arguments

---

# (1) Positional Arguments

Matched by position.

---

# Example

```python
def student(name, age):
    print(name, age)

student("Arpan", 20)
```

---

# Wrong Order

```python
student(20, "Arpan")
```

Incorrect meaning.

---

# (2) Keyword Arguments

Arguments passed using parameter names.

---

# Example

```python
student(age=20, name="Arpan")
```

Order does not matter.

---

# (3) Default Arguments

Default value assigned.

---

# Example

```python
def greet(name="Guest"):
    print(name)

greet()
```

Output:

```text
Guest
```

---

# (4) Variable Length Arguments

Using `*args`.

---

# Example

```python
def total(*nums):
    print(sum(nums))

total(1, 2, 3)
```

Output:

```text
6
```

---

# Viva Question

## Q. What is *args?

> *args allows function to accept variable number of arguments.

---

# 6. Return Statement (VERY IMPORTANT)

return sends value back.

---

# Example

```python
def add(a, b):
    return a + b

x = add(2, 3)

print(x)
```

Output:

```text
5
```

---

# Difference Between print() and return()

|print()|return|
|---|---|
|Displays output|Sends value back|
|Cannot reuse|Reusable result|

---

# Example Using print()

```python
def add(a, b):
    print(a + b)

x = add(2, 3)

print(x)
```

Output:

```text
5
None
```

---

# Why None?

Because function returned nothing.

---

# Viva Question (MOST ASKED)

## Q. Difference between print and return?

> print displays value while return sends value back to caller.

---

# 7. Function Returning Multiple Values

Python returns tuple automatically.

---

# Example

```python
def calc(a, b):
    return a+b, a-b

x = calc(10, 5)

print(x)
```

Output:

```text
(15, 5)
```

---

# Unpacking

```python
s, d = calc(10, 5)
```

---

# 8. Scope of Variables (VERY IMPORTANT)

---

# Local Variable

Created inside function.

---

# Example

```python
def test():
    x = 10
    print(x)

test()
```

---

# Important Point

Cannot access outside function.

---

# Global Variable

Created outside function.

---

# Example

```python
x = 100

def test():
    print(x)

test()
```

Output:

```text
100
```

---

# Modifying Global Variable

Using `global`.

---

# Example

```python
x = 10

def test():
    global x
    x = 20

test()

print(x)
```

Output:

```text
20
```

---

# Viva Question

## Q. Difference between local and global variable?

|Local|Global|
|---|---|
|Inside function|Outside function|
|Limited scope|Accessible everywhere|

---

# 9. Recursive Function (VERY IMPORTANT)

Function calling itself.

---

# Example

```python
def show():
    print("Hello")
    show()
```

Infinite recursion.

---

# Factorial Example

```python
def fact(n):
    if n == 1:
        return 1
    return n * fact(n-1)

print(fact(5))
```

Output:

```text
120
```

---

# Important Term

Base condition required.

Otherwise:

- Infinite recursion.
    

---

# Viva Question

## Q. What is recursion?

> Recursion is process where function calls itself.

---

# 10. Lambda Function (IMPORTANT)

Anonymous single-line function.

---

# Syntax

```python
lambda arguments : expression
```

---

# Example

```python
square = lambda x: x*x

print(square(5))
```

Output:

```text
25
```

---

# Equivalent Normal Function

```python
def square(x):
    return x*x
```

---

# Viva Question

## Q. What is lambda function?

> Lambda is anonymous function written in single line.

---

# 11. Function Inside Function

---

# Example

```python
def outer():
    def inner():
        print("Inner")
    
    inner()

outer()
```

---

# 12. Docstring

Used for function documentation.

---

# Example

```python
def add(a, b):
    """Returns sum"""
    return a+b
```

---

# Accessing Docstring

```python
print(add.__doc__)
```

---

# Viva Question

## Q. What is docstring?

> Docstring is string used to describe function purpose.

---

# 13. Important Built-in Functions

|Function|Work|
|---|---|
|len()|Length|
|max()|Maximum|
|min()|Minimum|
|sum()|Sum|
|type()|Datatype|

---

# 14. Common Programs

---

# Program 1 — Addition Function

```python
def add(a, b):
    return a+b

print(add(10, 20))
```

---

# Program 2 — Even/Odd Function

```python
def check(n):
    if n % 2 == 0:
        return "Even"
    return "Odd"

print(check(8))
```

---

# Program 3 — Factorial Using Recursion

```python
def fact(n):
    if n == 1:
        return 1
    return n * fact(n-1)

print(fact(5))
```

---

# 15. Common Errors

---

# NameError

Calling undefined function.

```python
hello()
```

---

# TypeError

Wrong number of arguments.

```python
def add(a, b):
    return a+b

add(5)
```

---

# RecursionError

Infinite recursion.

```python
def test():
    test()

test()
```

---

# 16. Output Prediction Questions

# Q1

```python
def add(a, b):
    return a+b

print(add(2,3))
```

Output:

```text
5
```

---

# Q2

```python
def test():
    print("Hello")

x = test()

print(x)
```

Output:

```text
Hello
None
```

---

# Q3

```python
x = 10

def show():
    x = 20
    print(x)

show()
print(x)
```

Output:

```text
20
10
```

---

# 17. Most Important Viva Questions

# Q1. What is function?

> Function is reusable block of code performing specific task.

---

# Q2. Difference between function and method?

|Function|Method|
|---|---|
|Independent|Associated with object/class|

Example:

```python
len()
```

Function

```python
list.append()
```

Method

---

# Q3. Difference between parameter and argument?

MOST IMPORTANT.

---

# Q4. Difference between print and return?

MOST IMPORTANT.

---

# Q5. What is recursion?

MOST IMPORTANT.

---

# Q6. What is lambda function?

> Anonymous single-line function.

---

# Q7. Difference between local and global variable?

VERY IMPORTANT.

---

# Q8. What is *args?

> Accepts variable number of arguments.

---

# 18. One-Minute Revision Notes

- Function → reusable code block
    
- def → creates function
    
- Parameters → variables
    
- Arguments → actual values
    
- return → sends value back
    
- print → displays output
    
- Local variable → inside function
    
- Global variable → outside function
    
- Recursion → function calling itself
    
- Lambda → anonymous function
    

---

# Memory Trick

## “PARG RL”

|Letter|Meaning|
|---|---|
|P|Parameters|
|A|Arguments|
|R|Return|
|G|Global|
|R|Recursion|
|L|Lambda|

---

# Examiner Impression Tip

If examiner asks:  
“What is recursion?”

Do NOT say only:

> “Function calling itself.”

Say:

> “Recursion is a technique where function solves problem by calling itself repeatedly until base condition is reached.”

This sounds advanced and professional.

Next most important topics:

- Strings in detail
    
- OOPs basics
    
- Exception Handling
    
- File Handling
    
- Modules & Packages