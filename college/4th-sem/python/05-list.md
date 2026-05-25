
---

# Python List Data Type (Detailed Viva Notes)

List is one of the MOST IMPORTANT topics in Python practical and viva.

Almost every examiner asks:

- What is list?
    
- Why list is mutable?
    
- Difference between list and tuple
    
- List methods
    
- append vs extend
    
- remove vs pop
    

So learn this chapter very carefully.

---

# 1. What is List?

A list is:

> An ordered, mutable collection used to store multiple items in a single variable.

---

# Simple Definition for Viva

> List is a collection datatype in Python that stores multiple values in ordered form and allows modification.

---

# Real-Life Analogy

Think of a shopping list:

```text
Milk
Bread
Eggs
Butter
```

Items are:

- Stored together
    
- Ordered
    
- Can be changed
    

Same as Python list.

---

# 2. Features of List

|Feature|Meaning|
|---|---|
|Ordered|Elements keep position|
|Mutable|Can be changed|
|Indexed|Access using index|
|Allows duplicates|Same value allowed|
|Heterogeneous|Different datatypes allowed|

---

# Example List

```python
numbers = [10, 20, 30]
```

---

# Mixed Datatype List

```python
data = [10, "Python", 5.5, True]
```

Python allows different datatypes in one list.

---

# 3. Creating Lists

---

# Empty List

```python
a = []
```

---

# List with Values

```python
fruits = ["Apple", "Banana", "Mango"]
```

---

# Using list() Constructor

```python
x = list((1, 2, 3))
```

---

# Viva Question

## Q. Why are lists called ordered?

> Because elements maintain insertion order.

---

# 4. Indexing in List

Every item has position/index.

---

# Example

```text
A    B     C
0    1     2
```

---

# Code

```python
fruits = ["Apple", "Banana", "Mango"]

print(fruits[0])
print(fruits[1])
```

Output:

```text
Apple
Banana
```

---

# Negative Indexing

```text
Apple   Banana   Mango
-3       -2       -1
```

---

# Example

```python
print(fruits[-1])
```

Output:

```text
Mango
```

---

# Viva Question

## Q. What is negative indexing?

> Negative indexing accesses elements from the end of list.

---

# 5. List Slicing

Slicing extracts part of list.

---

# Syntax

```python
list[start:end]
```

---

# Example

```python
nums = [10, 20, 30, 40, 50]

print(nums[1:4])
```

Output:

```text
[20, 30, 40]
```

---

# Important Rule

End index excluded.

---

# Examples

```python
print(nums[:3])
print(nums[2:])
print(nums[:])
```

---

# 6. List is Mutable (VERY IMPORTANT)

Mutable means:

> Values can be changed after creation.

---

# Example

```python
nums = [10, 20, 30]

nums[1] = 99

print(nums)
```

Output:

```text
[10, 99, 30]
```

---

# Viva Question

## Q. Why are lists mutable?

> Because elements inside list can be modified after creation.

---

# 7. List Operations

---

# Concatenation (`+`)

```python
a = [1, 2]
b = [3, 4]

print(a + b)
```

Output:

```text
[1, 2, 3, 4]
```

---

# Repetition (`*`)

```python
print([1, 2] * 3)
```

Output:

```text
[1, 2, 1, 2, 1, 2]
```

---

# Membership Operator

```python
nums = [10, 20, 30]

print(20 in nums)
```

Output:

```text
True
```

---

# 8. Important List Methods (VERY IMPORTANT)

---

# (1) append()

Adds element at END.

---

# Syntax

```python
list.append(value)
```

---

# Example

```python
nums = [1, 2, 3]

nums.append(4)

print(nums)
```

Output:

```text
[1, 2, 3, 4]
```

---

# Important Point

Adds only ONE element.

---

# Viva Question

## Q. What does append() do?

> append() adds an element at the end of list.

---

# (2) extend()

Adds multiple elements.

---

# Example

```python
a = [1, 2]
b = [3, 4]

a.extend(b)

print(a)
```

Output:

```text
[1, 2, 3, 4]
```

---

# Difference: append vs extend

---

# append()

```python
a = [1, 2]
a.append([3, 4])

print(a)
```

Output:

```text
[1, 2, [3, 4]]
```

---

# extend()

```python
a = [1, 2]
a.extend([3, 4])

print(a)
```

Output:

```text
[1, 2, 3, 4]
```

---

# Viva Question (MOST ASKED)

## Q. Difference between append() and extend()?

|append()|extend()|
|---|---|
|Adds single element|Adds multiple elements|
|Nested insertion possible|Elements added individually|

---

# (3) insert()

Adds element at specific position.

---

# Syntax

```python
list.insert(index, value)
```

---

# Example

```python
nums = [1, 2, 4]

nums.insert(2, 3)

print(nums)
```

Output:

```text
[1, 2, 3, 4]
```

---

# (4) remove()

Removes element by VALUE.

---

# Example

```python
nums = [10, 20, 30]

nums.remove(20)

print(nums)
```

Output:

```text
[10, 30]
```

---

# Important Point

Removes FIRST occurrence only.

---

# Viva Question

## Q. What does remove() do?

> remove() removes element using value.

---

# (5) pop()

Removes element using INDEX.

---

# Example

```python
nums = [10, 20, 30]

nums.pop(1)

print(nums)
```

Output:

```text
[10, 30]
```

---

# Without Index

```python
nums.pop()
```

Removes LAST element.

---

# Difference: remove vs pop

|remove()|pop()|
|---|---|
|Removes by value|Removes by index|
|No returned value|Returns removed item|

---

# Example

```python
nums = [10, 20, 30]

x = nums.pop()

print(x)
```

Output:

```text
30
```

---

# (6) clear()

Removes all elements.

---

# Example

```python
nums = [1, 2, 3]

nums.clear()

print(nums)
```

Output:

```text
[]
```

---

# (7) index()

Returns position of element.

---

# Example

```python
nums = [10, 20, 30]

print(nums.index(20))
```

Output:

```text
1
```

---

# (8) count()

Counts occurrences.

---

# Example

```python
nums = [1, 1, 2, 3]

print(nums.count(1))
```

Output:

```text
2
```

---

# (9) sort()

Sorts list.

---

# Example

```python
nums = [5, 2, 8, 1]

nums.sort()

print(nums)
```

Output:

```text
[1, 2, 5, 8]
```

---

# Descending Order

```python
nums.sort(reverse=True)
```

---

# Viva Question

## Q. Difference between sort() and sorted()?

|sort()|sorted()|
|---|---|
|Changes original list|Creates new sorted list|

---

# (10) reverse()

Reverses list.

---

# Example

```python
nums = [1, 2, 3]

nums.reverse()

print(nums)
```

Output:

```text
[3, 2, 1]
```

---

# (11) copy()

Creates copy of list.

---

# Example

```python
a = [1, 2, 3]

b = a.copy()

print(b)
```

---

# 9. Length of List

Using `len()`.

---

# Example

```python
nums = [1, 2, 3]

print(len(nums))
```

Output:

```text
3
```

---

# 10. Nested Lists

List inside list.

---

# Example

```python
data = [[1, 2], [3, 4]]
```

---

# Accessing Nested Elements

```python
print(data[0][1])
```

Output:

```text
2
```

---

# 11. Iterating Through List

Using loop.

---

# Example

```python
nums = [10, 20, 30]

for i in nums:
    print(i)
```

---

# 12. Common Errors

---

# IndexError

```python
nums = [1, 2]

print(nums[5])
```

---

# ValueError

```python
nums.remove(100)
```

Element not found.

---

# 13. Important Built-in Functions for Lists

|Function|Work|
|---|---|
|len()|Length|
|max()|Largest|
|min()|Smallest|
|sum()|Sum|
|sorted()|Sorting|

---

# Example

```python
nums = [1, 2, 3]

print(max(nums))
print(sum(nums))
```

---

# 14. List Comprehension (IMPORTANT)

Short way to create lists.

---

# Example

```python
nums = [x for x in range(5)]

print(nums)
```

Output:

```text
[0, 1, 2, 3, 4]
```

---

# Even Numbers Example

```python
evens = [x for x in range(10) if x % 2 == 0]
```

---

# Viva Question

## Q. What is list comprehension?

> List comprehension is a compact way to create lists using loops and conditions.

---

# Most Important Viva Questions

# Q1. What is list?

> List is an ordered mutable collection datatype.

---

# Q2. Why list is mutable?

> Because elements can be modified after creation.

---

# Q3. Difference between list and tuple?

|List|Tuple|
|---|---|
|Mutable|Immutable|
|Uses []|Uses ()|

---

# Q4. Difference between append() and extend()?

Already VERY important.

---

# Q5. Difference between remove() and pop()?

|remove()|pop()|
|---|---|
|Removes by value|Removes by index|

---

# Q6. Can list store different datatypes?

> Yes, Python lists support heterogeneous data.

---

# Output Prediction Questions

# Q1

```python
a = [1, 2]
a.append([3, 4])

print(a)
```

Output:

```text
[1, 2, [3, 4]]
```

---

# Q2

```python
a = [1, 2]
a.extend([3, 4])

print(a)
```

Output:

```text
[1, 2, 3, 4]
```

---

# Q3

```python
nums = [10, 20, 30]
print(nums[-1])
```

Output:

```text
30
```

---

# One-Minute Revision Notes

- List uses `[]`
    
- Ordered collection
    
- Mutable datatype
    
- Allows duplicates
    
- Supports indexing
    
- append() → add one item
    
- extend() → add multiple items
    
- remove() → remove by value
    
- pop() → remove by index
    
- sort() → sorting (change original list)
	
- sorted() → sorting (creates new sorted list)
    
- reverse() → reverse order
    
- clear() → remove all items
	
- count() → count occurrences


---

# Examiner Impression Tip

If examiner asks:  
“What is list?”

Don’t say:

> “List stores many values.”

Say:

> “List is an ordered and mutable collection datatype used to store multiple heterogeneous elements.”

This sounds much more professional.
