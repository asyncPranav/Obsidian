
---

# Primitive Data Types in Python (Detailed Viva Notes)

Before learning advanced collections like List, Tuple, Set, and Dictionary, you must fully understand basic/primitive data types because every Python program uses them.

---

# What is a Data Type?

A data type tells Python:

- What kind of value is stored
    
- What operations can be performed
    

---

# Real-Life Analogy

Imagine containers:

|Container|Stores|
|---|---|
|Water bottle|Water|
|Pencil box|Pens|
|Wallet|Money|

Similarly:

|Data Type|Stores|
|---|---|
|int|Integer numbers|
|float|Decimal numbers|
|str|Text|
|bool|True/False|

---

# Main Primitive Data Types

|Data Type|Meaning|Example|
|---|---|---|
|int|Integer|10|
|float|Decimal number|10.5|
|complex|Complex number|2+3j|
|str|String/Text|"Hello"|
|bool|Boolean|True|

---

# 1. Integer Data Type (`int`)

Integer means whole number.

---

# Examples

```python
a = 10
b = -50
c = 0
```

All are integers.

---

# Features of int

- No decimal point
    
- Can be positive
    
- Can be negative
    
- Can be zero
    
- Unlimited size in Python
    

---

# Checking Type

```python
a = 10
print(type(a))
```

Output:

```python
<class 'int'>
```

---

# Mathematical Operations

```python
a = 10
b = 5

print(a + b)
print(a - b)
print(a * b)
```

---

# Viva Questions

## Q. What is int datatype?

> int datatype stores whole numbers without decimal values.

---

## Q. Can integers be negative?

> Yes, integers can be positive, negative, or zero.

---

# 2. Float Data Type (`float`)

Float means decimal numbers.

---

# Examples

```python
a = 10.5
b = -4.7
c = 0.0
```

---

# Why Called Float?

Because decimal point can “float”.

Example:

```text
10.5
105.0
0.105
```

Decimal position changes.

---

# Checking Type

```python
x = 10.5
print(type(x))
```

Output:

```python
<class 'float'>
```

---

# Float Operations

```python
a = 5.5
b = 2.0

print(a + b)
print(a * b)
```

---

# Important Point

Even:

```python
5.0
```

is float, not int.

---

# Viva Questions

## Q. What is float datatype?

> float datatype stores decimal point numbers.

---

## Q. Difference between int and float?

|int|float|
|---|---|
|Whole numbers|Decimal numbers|
|Example: 10|Example: 10.5|

---

# 3. Complex Data Type (`complex`)

Python supports complex numbers.

Format:

```text
real + imaginary
```

---

# Example

```python
x = 2 + 3j
```

Here:

- 2 → real part
    
- 3j → imaginary part
    

---

# Checking Type

```python
x = 2 + 3j
print(type(x))
```

Output:

```python
<class 'complex'>
```

---

# Accessing Parts

```python
x = 2 + 3j

print(x.real)
print(x.imag)
```

---

# Output

```text
2.0
3.0
```

---

# Viva Questions

## Q. What is complex datatype?

> complex datatype stores complex numbers containing real and imaginary parts.

---

## Q. What does j represent?

> `j` represents imaginary part in Python.

---

# 4. String Data Type (`str`)

String means text or collection of characters.

---

# Examples

```python
name = "Arpan"
city = 'Lucknow'
```

---

# Strings Can Use

- Single quotes
    
- Double quotes
    
- Triple quotes
    

---

# Example

```python
a = 'Python'
b = "Python"
c = """Python"""
```

All are valid strings.

---

# Checking Type

```python
name = "Arpan"
print(type(name))
```

Output:

```python
<class 'str'>
```

---

# String Indexing

Every character has position.

```text
P  Y  T  H  O  N
0  1  2  3  4  5
```

---

# Example

```python
word = "Python"

print(word[0])
print(word[3])
```

Output:

```text
P
h
```

---

# Negative Indexing

```text
P  Y  T  H  O  N
-6 -5 -4 -3 -2 -1
```

---

# String Slicing

Syntax:

```python
string[start:end]
```

---

# Example

```python
word = "Python"

print(word[0:3])
```

Output:

```text
Pyt
```

---

# String is Immutable

Cannot change characters directly.

---

# Wrong Example

```python
name = "Python"
name[0] = "J"
```

Error occurs.

---

# Viva Answer

## Q. Why strings are immutable?

> Because characters of string cannot be modified after creation.

---

# Common String Functions

|Function|Work|
|---|---|
|upper()|Uppercase|
|lower()|Lowercase|
|replace()|Replace text|
|find()|Search|
|len()|Length|

---

# Example

```python
name = "python"

print(name.upper())
print(len(name))
```

---

# Viva Questions

## Q. What is string?

> String is a sequence of characters enclosed in quotes.

---

## Q. Difference between character and string?

|Character|String|
|---|---|
|Single letter|Collection of letters|
|Example: 'A'|Example: "APPLE"|

---

# 5. Boolean Data Type (`bool`)

Boolean stores only:

- True
    
- False
    

---

# Examples

```python
a = True
b = False
```

---

# Important Point

Boolean values start with capital letter.

Correct:

```python
True
False
```

Wrong:

```python
true
false
```

---

# Checking Type

```python
x = True
print(type(x))
```

Output:

```python
<class 'bool'>
```

---

# Used in Conditions

```python
age = 18

print(age >= 18)
```

Output:

```python
True
```

---

# Boolean Internally

```text
True  = 1
False = 0
```

---

# Example

```python
print(True + True)
```

Output:

```text
2
```

---

# Viva Questions

## Q. What is bool datatype?

> bool datatype stores logical values True or False.

---

## Q. Where boolean values are used?

> Boolean values are used in conditions and decision-making statements.

---

# 6. None Data Type (`NoneType`) (IMPORTANT)

`None` means:

> No value / empty value

---

# Example

```python
x = None
print(type(x))
```

Output:

```python
<class 'NoneType'>
```

---

# Use of None

Used when variable has no value yet.

---

# Example

```python
result = None
```

---

# Viva Question

## Q. What is None in Python?

> None represents absence of value or null value in Python.

---

# Type Conversion (VERY IMPORTANT)

---

# Converting int → float

```python
a = 10
print(float(a))
```

Output:

```text
10.0
```

---

# float → int

```python
a = 10.9
print(int(a))
```

Output:

```text
10
```

Decimal part removed.

---

# int → str

```python
x = 100
print(str(x))
```

---

# str → int

```python
x = "50"
print(int(x))
```

---

# Viva Question

## Q. What is type casting?

> Type casting means converting one datatype into another datatype.

---

# Arithmetic Operators on Primitive Types

|Operator|Meaning|
|---|---|
|+|Addition|
|-|Subtraction|
|*|Multiplication|
|/|Division|
|//|Floor Division|
|%|Modulus|
|**|Power|

---

# 7. Floor Division (`//`) VERY IMPORTANT

Floor division removes decimal part.

---

# Example

```python
print(10 // 3)
```

Output:

```text
3
```

Because:

```text
10 / 3 = 3.333
```

Floor division returns lower integer.

---

# More Examples

```python
print(20 // 6)
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

VERY IMPORTANT:  
Python moves toward lower integer.

---

# Difference Between `/` and `//`

|`/`|`//`|
|---|---|
|Normal division|Floor division|
|Gives decimal|Removes decimal|

---

# Example

```python
print(10 / 3)
print(10 // 3)
```

Output:

```text
3.333333
3
```

---

# Viva Questions

## Q. What is floor division?

> Floor division returns quotient without decimal part using `//` operator.

---

## Q. Difference between `/` and `//`?

> `/` gives floating-point result while `//` gives floor value.

---

# 8. Modulus Operator (`%`)

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

# Use of Modulus

- Even/odd checking
    
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

# 9. Exponent Operator (`**`)

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

# Memory Table for Primitive Types

|Type|Stores|Example|
|---|---|---|
|int|Whole numbers|10|
|float|Decimal numbers|10.5|
|complex|Complex numbers|2+3j|
|str|Text|"Python"|
|bool|True/False|True|
|NoneType|No value|None|

---

# Most Important Viva Questions

# Q1. Difference between int and float?

|int|float|
|---|---|
|Whole number|Decimal number|

---

# Q2. What is immutable?

> Immutable means value cannot be changed after creation.

---

# Q3. Which primitive datatype is immutable?

- int
    
- float
    
- str
    
- bool
    
- complex
    

All primitive types are immutable.

---

# Q4. Why input() returns string?

> Because keyboard input is treated as text by default.

---

# Q5. What is floor division?

> Floor division removes decimal part and returns lower integer.

---

# Output Prediction Questions

# Q1

```python
print(10 / 3)
```

Output:

```text
3.333333333
```

---

# Q2

```python
print(10 // 3)
```

Output:

```text
3
```

---

# Q3

```python
print(2 ** 4)
```

Output:

```text
16
```

---

# Q4

```python
print(True + False)
```

Output:

```text
1
```

---

# One-Minute Revision Notes

- int → whole numbers
    
- float → decimal numbers
    
- complex → real + imaginary
    
- str → text data
    
- bool → True/False
    
- None → no value
    
- `/` → normal division
    
- `//` → floor division
    
- `%` → remainder
    
- `**` → power
    
- Primitive types are immutable
    

---

# Examiner Impression Tip

If examiner asks:  
“What is floor division?”

Do not say only:

> “It removes decimal.”

Say:

> “Floor division uses `//` operator and returns the greatest lower integer value after division.”

That sounds advanced and professional.

Now next best chapters are:

- Operators (very important)
    
- String in detail
    
- Type Casting
    
- Loops
    
- Functions
    

Send next topic.