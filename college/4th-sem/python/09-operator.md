
---


# Python Operators (Detailed Viva Notes)

Operators are VERY IMPORTANT for:

- Viva
    
- Practical coding
    
- Logic building
    
- Conditions and loops
    

Almost every Python program uses operators.

Examiners commonly ask:

- What are operators?
    
- Types of operators
    
- Difference between `=` and `==`
    
- Difference between `and` and `&`
    
- Difference between `is` and `==`
    
- Membership operators
    
- Identity operators
    

This is one of the most important viva chapters.

---

# 1. What is Operator?

Operator is:

> A symbol used to perform operation on values or variables.

---

# Real-Life Analogy

```text
2 + 3
```

Here:

- `2` and `3` → operands
    
- `+` → operator
    

Operator performs addition.

---

# Viva Definition

## Q. What is operator?

> Operator is a symbol that performs operations on operands.

---

# Types of Operators in Python

|Operator Type|Purpose|
|---|---|
|Arithmetic|Mathematical operations|
|Assignment|Assign values|
|Comparison|Compare values|
|Logical|Combine conditions|
|Bitwise|Binary operations|
|Membership|Check existence|
|Identity|Check memory identity|

---

# 2. Arithmetic Operators

Used for mathematical calculations.

---

# Arithmetic Operator Table

|Operator|Meaning|Example|
|---|---|---|
|`+`|Addition|`2 + 3`|
|`-`|Subtraction|`5 - 2`|
|`*`|Multiplication|`2 * 3`|
|`/`|Division|`10 / 2`|
|`//`|Floor Division|`10 // 3`|
|`%`|Modulus|`10 % 3`|
|`**`|Exponent|`2 ** 3`|

---

# (1) Addition `+`

```python
print(10 + 5)
```

Output:

```text
15
```

---

# String Concatenation

```python
print("Hello" + "World")
```

Output:

```text
HelloWorld
```

---

# (2) Subtraction `-`

```python
print(10 - 3)
```

Output:

```text
7
```

---

# (3) Multiplication `*`

```python
print(5 * 2)
```

Output:

```text
10
```

---

# String Repetition

```python
print("Hi" * 3)
```

Output:

```text
HiHiHi
```

---

# (4) Division `/`

Returns float result.

---

# Example

```python
print(10 / 3)
```

Output:

```text
3.333333333
```

---

# Important Point

Even:

```python
10 / 2
```

returns:

```text
5.0
```

---

# Viva Question

## Q. What does `/` operator return?

> `/` returns floating-point division result.

---

# (5) Floor Division `//`

Removes decimal part.

---

# Example

```python
print(10 // 3)
```

Output:

```text
3
```

---

# Negative Example

```python
print(-10 // 3)
```

Output:

```text
-4
```

Python moves toward lower integer.

---

# Difference Between `/` and `//`

|`/`|`//`|
|---|---|
|Normal division|Floor division|
|Gives decimal|Removes decimal|

---

# Viva Question

## Q. What is floor division?

> Floor division returns greatest lower integer value after division.

---

# (6) Modulus `%`

Returns remainder.

---

# Example

```python
print(10 % 3)
```

Output:

```text
1
```

---

# Use Cases

- Even/odd
    
- Divisibility
    
- Cyclic operations
    

---

# Example

```python
num = 8

print(num % 2 == 0)
```

Output:

```text
True
```

---

# Viva Question

## Q. What is modulus operator?

> Modulus operator returns remainder after division.

---

# (7) Exponent `**`

Used for power.

---

# Example

```python
print(2 ** 3)
```

Output:

```text
8
```

---

# 3. Assignment Operators

Used to assign values.

---

# Assignment Operator Table

|Operator|Example|Meaning|
|---|---|---|
|`=`|`a = 5`|Assign|
|`+=`|`a += 2`|Add and assign|
|`-=`|`a -= 2`|Subtract and assign|
|`*=`|`a *= 2`|Multiply and assign|
|`/=`|`a /= 2`|Divide and assign|

---

# Example

```python
a = 10

a += 5

print(a)
```

Output:

```text
15
```

---

# Equivalent To

```python
a = a + 5
```

---

# Viva Question

## Q. What is assignment operator?

> Assignment operators assign values to variables.

---

# 4. Comparison Operators

Compare values and return:

- True
    
- False
    

---

# Comparison Operator Table

|Operator|Meaning|
|---|---|
|`==`|Equal|
|`!=`|Not equal|
|`>`|Greater|
|`<`|Smaller|
|`>=`|Greater equal|
|`<=`|Smaller equal|

---

# Example

```python
print(10 > 5)
```

Output:

```text
True
```

---

# Example

```python
print(10 == 10)
```

Output:

```text
True
```

---

# Important Difference `=` vs `==`

|`=`|`==`|
|---|---|
|Assignment|Comparison|
|Assign value|Check equality|

---

# Example

```python
a = 10
```

Assigns value.

---

# Example

```python
print(a == 10)
```

Checks equality.

---

# Viva Question (MOST ASKED)

## Q. Difference between `=` and `==`?

> `=` assigns value while `==` compares values.

---

# 5. Logical Operators

Used to combine conditions.

---

# Logical Operators Table

|Operator|Meaning|
|---|---|
|`and`|Both conditions true|
|`or`|Any one true|
|`not`|Reverses result|

---

# (1) and

Returns True only if BOTH true.

---

# Example

```python
print(10 > 5 and 20 > 10)
```

Output:

```text
True
```

---

# Truth Table

|A|B|A and B|
|---|---|---|
|T|T|T|
|T|F|F|
|F|T|F|
|F|F|F|

---

# (2) or

Returns True if ANY condition true.

---

# Example

```python
print(10 > 5 or 20 < 10)
```

Output:

```text
True
```

---

# (3) not

Reverses result.

---

# Example

```python
print(not True)
```

Output:

```text
False
```

---

# Viva Question

## Q. What is logical operator?

> Logical operators combine multiple conditions.

---

# 6. Bitwise Operators (IMPORTANT FOR VIVA)

Works on binary numbers.

---

# Bitwise Operator Table

|Operator|Meaning|
|---|---|
|`&`|AND|
|`|`|
|`^`|XOR|
|`~`|NOT|
|`<<`|Left shift|
|`>>`|Right shift|

---

# Example of `&`

```python
print(5 & 3)
```

Binary:

```text
5 = 101
3 = 011
---------
    001
```

Output:

```text
1
```

---

# Difference Between `and` and `&`

|`and`|`&`|
|---|---|
|Logical operator|Bitwise operator|
|Works on conditions|Works on bits|

---

# Viva Question (IMPORTANT)

## Q. Difference between `and` and `&`?

> `and` works on logical conditions while `&` works on binary bits.

---

# 7. Membership Operators

Check presence of element.

---

# Operators

|Operator|Meaning|
|---|---|
|`in`|Present|
|`not in`|Absent|

---

# Example

```python
nums = [1, 2, 3]

print(2 in nums)
```

Output:

```text
True
```

---

# Example

```python
print(5 not in nums)
```

Output:

```text
True
```

---

# Viva Question

## Q. What are membership operators?

> Membership operators check whether element exists in sequence.

---

# 8. Identity Operators (VERY IMPORTANT)

Checks memory location.

---

# Operators

|Operator|Meaning|
|---|---|
|`is`|Same object|
|`is not`|Different object|

---

# Example

```python
a = [1, 2]
b = a

print(a is b)
```

Output:

```text
True
```

---

# Example

```python
a = [1, 2]
b = [1, 2]

print(a is b)
```

Output:

```text
False
```

Because memory different.

---

# Difference Between `==` and `is` (MOST IMPORTANT)

|`==`|`is`|
|---|---|
|Checks value equality|Checks memory identity|
|Values same?|Same object?|

---

# Example

```python
a = [1, 2]
b = [1, 2]

print(a == b)
print(a is b)
```

Output:

```text
True
False
```

---

# Viva Question (MOST ASKED)

## Q. Difference between `==` and `is`?

> `==` compares values while `is` compares memory location.

---

# 9. Operator Precedence

Order of execution.

---

# Example

```python
print(2 + 3 * 4)
```

Output:

```text
14
```

Multiplication first.

---

# Common Precedence

```text
()
**
*, /, //, %
+, -
```

---

# 10. Common Errors

---

# TypeError

```python
"2" + 2
```

Cannot add string and integer.

---

# ZeroDivisionError

```python
10 / 0
```

---

# Most Important Viva Questions

# Q1. What is operator?

> Operator performs operations on operands.

---

# Q2. Difference between `=` and `==`?

VERY IMPORTANT.

---

# Q3. Difference between `/` and `//`?

VERY IMPORTANT.

---

# Q4. Difference between `==` and `is`?

VERY IMPORTANT.

---

# Q5. Difference between `and` and `&`?

VERY IMPORTANT.

---

# Q6. What is modulus operator?

> Returns remainder after division.

---

# Output Prediction Questions

# Q1

```python
print(10 // 3)
```

Output:

```text
3
```

---

# Q2

```python
print(2 ** 4)
```

Output:

```text
16
```

---

# Q3

```python
a = [1,2]
b = [1,2]

print(a == b)
print(a is b)
```

Output:

```text
True
False
```

---

# Q4

```python
print(True and False)
```

Output:

```text
False
```

---

# One-Minute Revision Notes

- Arithmetic → calculations
    
- Assignment → assign values
    
- Comparison → True/False
    
- Logical → combine conditions
    
- Bitwise → binary operations
    
- Membership → check existence
    
- Identity → check memory
    

Most important:

- `=` vs `==`
    
- `==` vs `is`
    
- `/` vs `//`
    
- `and` vs `&`
    

---

# Memory Trick

## “AACLBMI”

|Letter|Operator Type|
|---|---|
|A|Arithmetic|
|A|Assignment|
|C|Comparison|
|L|Logical|
|B|Bitwise|
|M|Membership|
|I|Identity|

---

# Examiner Impression Tip

If examiner asks:  
“What is difference between `==` and `is`?”

Do NOT say only:

> “One compares values.”

Say:

> “`==` checks value equality whereas `is` checks whether both variables refer to the same memory object.”

This sounds professional and advanced.

Next best topics:

- Strings in detail
    
- Conditional statements
    
- Loops
    
- Functions
    
- OOPs
    
- Exception Handling
    
- File Handling