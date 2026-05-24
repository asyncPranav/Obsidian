
---


# Python Dictionary Data Type (Detailed Viva Notes)

Dictionary is one of the MOST IMPORTANT topics in Python viva and practical.

Examiners frequently ask:

- What is dictionary?
    
- What are keys and values?
    
- Why keys must be unique?
    
- Difference between list and dictionary
    
- Dictionary methods
    
- Mutable vs immutable keys
    
- items(), keys(), values()
    

This topic is extremely important for both theory and coding.

---

# 1. What is Dictionary?

Dictionary is:

> A mutable collection that stores data in key-value pairs.

---

# Simple Definition for Viva

> Dictionary is an unordered mutable collection datatype that stores data in key-value pairs.

---

# Real-Life Analogy

Think of a real dictionary:

```text
Word → Meaning
```

Example:

```text
Apple → Fruit
Car → Vehicle
```

Similarly in Python:

```python
student = {
    "name": "Arpan",
    "age": 20
}
```

---

# 2. Features of Dictionary

|Feature|Meaning|
|---|---|
|Key-value pair|Data stored as pairs|
|Mutable|Can modify values|
|Keys are unique|No duplicate keys|
|Fast lookup|Efficient searching|
|Uses `{}`|Curly braces|

---

# 3. Creating Dictionary

---

# Empty Dictionary

```python
d = {}
```

---

# Dictionary with Data

```python
student = {
    "name": "Arpan",
    "age": 20,
    "city": "Lucknow"
}
```

---

# Checking Type

```python
print(type(student))
```

Output:

```python
<class 'dict'>
```

---

# 4. Key-Value Pair

Dictionary stores:

```text
key → value
```

---

# Example

```python
{
    "name": "Arpan"
}
```

Here:

- `"name"` → key
    
- `"Arpan"` → value
    

---

# Viva Question

## Q. What is key-value pair?

> Key-value pair means one value is associated with one unique key.

---

# 5. Accessing Dictionary Values

Using keys.

---

# Example

```python
student = {
    "name": "Arpan",
    "age": 20
}

print(student["name"])
```

Output:

```text
Arpan
```

---

# Important Point

Dictionary does NOT use indexing.

Wrong:

```python
student[0]
```

Error occurs.

---

# Why?

Because dictionaries work using keys.

---

# 6. Keys Must Be Unique

---

# Example

```python
d = {
    "a": 1,
    "a": 2
}

print(d)
```

Output:

```python
{'a': 2}
```

Old value overwritten.

---

# Viva Question

## Q. Why keys must be unique?

> Because duplicate keys create ambiguity during value lookup.

---

# 7. Keys Must Be Immutable

Allowed keys:

- int
    
- str
    
- tuple
    

NOT allowed:

- list
    
- set
    
- dictionary
    

---

# Correct Example

```python
d = {
    (1, 2): "Python"
}
```

---

# Wrong Example

```python
d = {
    [1, 2]: "Python"
}
```

Error:

```text
TypeError
```

---

# Why?

Because list is mutable.

---

# Viva Question

## Q. Why list cannot be dictionary key?

> Because dictionary keys must be immutable and hashable.

---

# 8. Dictionary is Mutable

You can:

- Add
    
- Remove
    
- Modify data
    

---

# Example

```python
student = {
    "name": "Arpan"
}

student["name"] = "Rahul"

print(student)
```

Output:

```python
{'name': 'Rahul'}
```

---

# 9. Adding Elements

---

# Example

```python
student = {
    "name": "Arpan"
}

student["age"] = 20

print(student)
```

Output:

```python
{'name': 'Arpan', 'age': 20}
```

---

# 10. Important Dictionary Methods (VERY IMPORTANT)

---

# (1) keys()

Returns all keys.

---

# Example

```python
student = {
    "name": "Arpan",
    "age": 20
}

print(student.keys())
```

Output:

```python
dict_keys(['name', 'age'])
```

---

# Viva Answer

## Q. What does keys() do?

> keys() returns all dictionary keys.

---

# (2) values()

Returns all values.

---

# Example

```python
print(student.values())
```

Output:

```python
dict_values(['Arpan', 20])
```

---

# (3) items()

Returns key-value pairs.

---

# Example

```python
print(student.items())
```

Output:

```python
dict_items([('name', 'Arpan'), ('age', 20)])
```

---

# Viva Question

## Q. What does items() return?

> items() returns key-value pairs as tuples.

---

# (4) get()

Safely accesses values.

---

# Example

```python
student = {
    "name": "Arpan"
}

print(student.get("name"))
```

Output:

```text
Arpan
```

---

# Difference Between [] and get()

---

# Using []

```python
print(student["age"])
```

Error if absent.

---

# Using get()

```python
print(student.get("age"))
```

Output:

```python
None
```

No error.

---

# Viva Question

## Q. Difference between get() and []?

|get()|[]|
|---|---|
|No error if absent|Gives KeyError|

---

# (5) update()

Adds or modifies data.

---

# Example

```python
student = {
    "name": "Arpan"
}

student.update({"age": 20})

print(student)
```

Output:

```python
{'name': 'Arpan', 'age': 20}
```

---

# (6) pop()

Removes using key.

---

# Example

```python
student = {
    "name": "Arpan",
    "age": 20
}

student.pop("age")

print(student)
```

Output:

```python
{'name': 'Arpan'}
```

---

# (7) popitem()

Removes last inserted pair.

---

# Example

```python
d = {
    "a": 1,
    "b": 2
}

d.popitem()

print(d)
```

Output:

```python
{'a': 1}
```

---

# (8) clear()

Removes all elements.

---

# Example

```python
d = {"a": 1}

d.clear()

print(d)
```

Output:

```python
{}
```

---

# (9) copy()

Creates copy of dictionary.

---

# Example

```python
d = {"a": 1}

x = d.copy()

print(x)
```

---

# 11. Iterating Through Dictionary

---

# Loop Through Keys

```python
for key in student:
    print(key)
```

---

# Loop Through Values

```python
for value in student.values():
    print(value)
```

---

# Loop Through Both

```python
for k, v in student.items():
    print(k, v)
```

---

# 12. Nested Dictionary

Dictionary inside dictionary.

---

# Example

```python
students = {
    "s1": {
        "name": "Arpan",
        "age": 20
    }
}
```

---

# Accessing Nested Values

```python
print(students["s1"]["name"])
```

Output:

```text
Arpan
```

---

# 13. Dictionary Comprehension (IMPORTANT)

Short way to create dictionary.

---

# Example

```python
d = {x: x*x for x in range(5)}

print(d)
```

Output:

```python
{0:0, 1:1, 2:4, 3:9, 4:16}
```

---

# Viva Question

## Q. What is dictionary comprehension?

> Dictionary comprehension is compact syntax for creating dictionaries.

---

# 14. Membership Operator

Checks keys only.

---

# Example

```python
d = {
    "name": "Arpan"
}

print("name" in d)
```

Output:

```python
True
```

---

# 15. Difference Between List and Dictionary

|List|Dictionary|
|---|---|
|Ordered collection|Key-value collection|
|Access using index|Access using key|
|Uses []|Uses {}|

---

# 16. Difference Between Set and Dictionary

|Set|Dictionary|
|---|---|
|Stores values only|Stores key-value pairs|
|No duplicate values|No duplicate keys|

---

# 17. Common Errors

---

# KeyError

```python
d["age"]
```

Key absent.

---

# TypeError

Using mutable key.

```python
d = {[1,2]: "Python"}
```

---

# 18. Important Programs

# Program 1 — Student Data

```python
student = {
    "name": "Arpan",
    "age": 20
}

print(student)
```

---

# Program 2 — Iteration

```python
for k, v in student.items():
    print(k, v)
```

---

# Program 3 — Add New Key

```python
student["city"] = "Lucknow"

print(student)
```

---

# Output Prediction Questions

# Q1

```python
d = {"a":1, "b":2}

print(d["a"])
```

Output:

```text
1
```

---

# Q2

```python
d = {"a":1, "a":2}

print(d)
```

Output:

```python
{'a': 2}
```

---

# Q3

```python
d = {}

print(type(d))
```

Output:

```python
<class 'dict'>
```

---

# Most Important Viva Questions

# Q1. What is dictionary?

> Dictionary is a mutable collection storing key-value pairs.

---

# Q2. Why keys must be unique?

> Duplicate keys create ambiguity during lookup.

---

# Q3. Why keys must be immutable?

> Immutable keys provide stable hashing for fast lookup.

---

# Q4. Difference between get() and []?

VERY IMPORTANT.

---

# Q5. Difference between list and dictionary?

VERY IMPORTANT.

---

# Q6. What does items() return?

> items() returns key-value pairs as tuples.

---

# One-Minute Revision Notes

- Dictionary uses `{}`
    
- Stores key-value pairs
    
- Mutable datatype
    
- Keys must be unique
    
- Keys must be immutable
    
- Access using keys
    
- keys() → returns keys
    
- values() → returns values
    
- items() → returns pairs
    
- get() → safe access
    
- update() → modify/add
    
- pop() → remove by key
    

---

# Memory Trick for Important Methods

## “KVIG UPC”

|Letter|Method|
|---|---|
|K|keys()|
|V|values()|
|I|items()|
|G|get()|
|U|update()|
|P|pop()|
|C|clear()|

---

# Examiner Impression Tip

If examiner asks:  
“Why dictionary lookup is fast?”

Say:

> “Dictionary uses hash tables internally, which provides very efficient key-based access.”

This sounds advanced and professional.

Next most important topics:

- Functions
    
- String in detail
    
- Operators
    
- Loops
    
- OOPs
    
- Exception Handling
    
- File Handling