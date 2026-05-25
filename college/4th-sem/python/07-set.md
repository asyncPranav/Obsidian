
---

# Python Set Data Type (Detailed Viva Notes)

Set is one of the most important collection datatypes in Python.

Examiners often ask:

- What is set?
    
- Why set does not allow duplicates?
    
- Difference between set and list
    
- Why set is unordered?
    
- Set methods
    
- Union vs intersection
    

This topic is VERY important for viva and practical.

---

# 1. What is Set?

A set is:

> An unordered, mutable collection of unique elements.

---

# Simple Definition for Viva

> Set is a collection datatype used to store unique values in unordered form.

---

# Real-Life Analogy

Think about a classroom attendance list.

Even if a student’s name is written multiple times:

```text
Rahul
Rahul
Rahul
```

Final attendance contains only:

```text
Rahul
```

Same happens in set.

Duplicates are automatically removed.

---

# 2. Features of Set

|Feature|Meaning|
|---|---|
|Unordered|No fixed position|
|Mutable|Can add/remove elements|
|Unique elements|No duplicates|
|Unindexed|No indexing|
|Fast searching|Efficient operations|

---

# 3. Creating Sets

---

# Empty Set (IMPORTANT TRAP)

```python
s = set()
```

---

# IMPORTANT

```python
s = {}
```

This creates dictionary, NOT set.

---

# Viva Question (MOST ASKED)

## Q. How to create empty set?

> Empty set is created using `set()` function.

---

# Set with Values

```python
s = {1, 2, 3}
```

---

# Mixed Datatype Set

```python
data = {10, "Python", 5.5, True}
```

---

# Checking Type

```python
s = {1, 2, 3}

print(type(s))
```

Output:

```python
<class 'set'>
```

---

# 4. Duplicate Elements Removed Automatically

---

# Example

```python
s = {1, 2, 2, 3, 1}

print(s)
```

Output:

```python
{1, 2, 3}
```

---

# Why?

Because set stores only unique values.

---

# Viva Question

## Q. Why set does not allow duplicates?

> Because sets are designed to store only unique elements.

---

# 5. Set is Unordered

Set does NOT maintain insertion order.

---

# Example

```python
s = {10, 5, 1}

print(s)
```

Output may be:

```python
{1, 10, 5}
```

Order can change.

---

# Important Point

You cannot predict exact order.

---

# Viva Question

## Q. Why set is unordered?

> Because set uses hashing internally and does not store elements by position.

---

# 6. Set Does NOT Support Indexing

---

# Wrong Example

```python
s = {1, 2, 3}

print(s[0])
```

Error:

```text
TypeError
```

---

# Why?

Because sets are unordered.

No fixed positions.

---

# 7. Set is Mutable

You can:

- Add elements
    
- Remove elements
    

But individual elements must be immutable.

---

# Example

```python
s = {1, 2, 3}

s.add(4)

print(s)
```

---

# 8. Important Set Methods (VERY IMPORTANT)

---

# (1) add()

Adds single element.

---

# Syntax

```python
set.add(value)
```

---

# Example

```python
s = {1, 2}

s.add(3)

print(s)
```

Output:

```python
{1, 2, 3}
```

---

# Viva Question

## Q. What does add() do?

> add() inserts a single element into set.

---

# (2) update()

Adds multiple elements.

---

# Example

```python
s = {1, 2}

s.update([3, 4])

print(s)
```

Output:

```python
{1, 2, 3, 4}
```

---

# Difference Between add() and update()

|add()|update()|
|---|---|
|Adds one element|Adds multiple elements|

---

# Example

```python
s = {1, 2}

s.add((3, 4))
print(s)
```

Output:

```python
{1, 2, (3, 4)}
```

---

# update Example

```python
s = {1, 2}

s.update((3, 4))
print(s)
```

Output:

```python
{1, 2, 3, 4}
```

---

# (3) remove()

Removes element.

---

# Example

```python
s = {1, 2, 3}

s.remove(2)

print(s)
```

Output:

```python
{1, 3}
```

---

# Important Point

If element not found:

```text
KeyError
```

---

# (4) discard()

Also removes element.

BUT:  
No error if element absent.

---

# Example

```python
s = {1, 2, 3}

s.discard(10)

print(s)
```

No error.

---

# Difference: remove vs discard

|remove()|discard()|
|---|---|
|Gives error if absent|No error|

---

# Viva Question

## Q. Difference between remove() and discard()?

> remove() raises error for missing element while discard() does not.

---

# (5) pop()

Removes random element.

---

# Example

```python
s = {10, 20, 30}

print(s.pop())
```

Random output possible.

---

# Important Point

Because set is unordered.

---

# Viva Question

## Q. Why pop() removes random element in set?

> Because sets are unordered collections.

---

# (6) clear()

Removes all elements.

---

# Example

```python
s = {1, 2, 3}

s.clear()

print(s)
```

Output:

```python
set()
```

---

# (7) copy()

Creates copy of set.

---

# Example

```python
a = {1, 2, 3}

b = a.copy()

print(b)
```

---

# 9. Mathematical Set Operations (VERY IMPORTANT)

This is the main power of sets.

---

# (1) union()

Combines all unique elements.

---

# Example

```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a.union(b))
```

Output:

```python
{1, 2, 3, 4, 5}
```

---

# Using `|`

```python
print(a | b)
```

---

# Viva Question

## Q. What is union in set?

> Union combines all unique elements from both sets.

---

# (2) intersection()

Common elements only.

---

# Example

```python
a = {1, 2, 3}
b = {2, 3, 4}

print(a.intersection(b))
```

Output:

```python
{2, 3}
```

---

# Using `&`

```python
print(a & b)
```

---

# Viva Question

## Q. What is intersection?

> Intersection returns common elements from sets.

---

# (3) difference()

Elements present in first set only.

---

# Example

```python
a = {1, 2, 3}
b = {2, 3}

print(a.difference(b))
```

Output:

```python
{1}
```

---

# Using `-`

```python
print(a - b)
```

---

# (4) symmetric_difference()

Elements NOT common.

---

# Example

```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a.symmetric_difference(b))
```

Output:

```python
{1, 2, 4, 5}
```

---

# 10. Subset and Superset

---

# subset()

Checks if all elements exist inside another set.

---

# Example

```python
a = {1, 2}
b = {1, 2, 3}

print(a.issubset(b))
```

Output:

```python
True
```

---

# superset()

```python
print(b.issuperset(a))
```

Output:

```python
True
```

---

# 11. Frozen Set (IMPORTANT)

Frozen set is immutable set.

---

# Example

```python
fs = frozenset([1, 2, 3])

print(fs)
```

---

# Cannot modify frozen set.

---

# Viva Question

## Q. What is frozenset?

> frozenset is immutable version of set.

---

# 12. Iterating Through Set

---

# Example

```python
s = {1, 2, 3}

for i in s:
    print(i)
```

---

# 13. Set vs List (IMPORTANT)

|List|Set|
|---|---|
|Ordered|Unordered|
|Allows duplicates|No duplicates|
|Supports indexing|No indexing|
|Uses []|Uses {}|

---

# 14. Set vs Tuple

|Set|Tuple|
|---|---|
|Mutable|Immutable|
|Unordered|Ordered|
|No indexing|Indexing allowed|

---

# 15. Common Errors

---

# KeyError

```python
s.remove(100)
```

---

# TypeError

Trying mutable element.

```python
s = {[1, 2]}
```

List cannot be inside set.

---

# Why?

Because list is mutable and unhashable.

---

# 16. Important Programs

# Program 1 — Remove Duplicates

```python
nums = [1, 2, 2, 3, 1]

s = set(nums)

print(s)
```

---

# Program 2 — Union

```python
a = {1, 2}
b = {2, 3}

print(a.union(b))
```

---

# Program 3 — Intersection

```python
a = {1, 2, 3}
b = {2, 3, 4}

print(a.intersection(b))
```

---

# Output Prediction Questions

# Q1

```python
s = {1, 2, 2, 3}

print(s)
```

Output:

```python
{1, 2, 3}
```

---

# Q2

```python
s = set()

print(type(s))
```

Output:

```python
<class 'set'>
```

---

# Q3

```python
s = {}

print(type(s))
```

Output:

```python
<class 'dict'>
```

VERY IMPORTANT.

---

# Most Important Viva Questions

# Q1. What is set?

> Set is an unordered mutable collection of unique elements.

---

# Q2. Why set does not allow duplicates?

> Because sets store only unique values.

---

# Q3. Why set does not support indexing?

> Because sets are unordered collections.

---

# Q4. Difference between remove() and discard()?

VERY IMPORTANT.

---

# Q5. Difference between list and set?

VERY IMPORTANT.

---

# Q6. What is frozenset?

> Immutable version of set.

---

# One-Minute Revision Notes

- Set uses `{}`
    
- Unordered collection
    
- Mutable datatype
    
- No duplicates
    
- No indexing
    
- add() → add one element
    
- update() → add multiple
    
- remove() → error if absent
    
- discard() → no error
    
- union() → combine sets
    
- intersection() → common values
    
- difference() → unique values
    
- frozenset → immutable set
    

---

# Memory Trick for Set Operations

## “UIDS”

|Letter|Operation|
|---|---|
|U|Union|
|I|Intersection|
|D|Difference|
|S|Symmetric Difference|

---

# Examiner Impression Tip

If examiner asks:  
“Why set is faster for searching?”

Say:

> “Set uses hashing internally, which provides very fast lookup operations.”

This sounds advanced and professional.
