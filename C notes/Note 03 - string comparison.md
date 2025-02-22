In C, strings are compared **character by character** based on the **ASCII values** of the characters. The comparison continues from the first character until either a difference is found or both strings terminate with the null character `\0`.

Here's a detailed explanation of how strings are compared in C:

### 1. **Character-by-Character Comparison**:

- C strings are arrays of characters terminated by a null character (`\0`).
- When comparing two strings, each corresponding character (from the same position) in the two strings is compared.
- This comparison is done using the ASCII values of the characters.
    - For example, the ASCII value of `'A'` is `65`, while the ASCII value of `'a'` is `97`. This means that `'A'` is considered **less than** `'a'` when comparing them lexicographically.

### 2. **Null Terminator Check**:

- The comparison stops either when a mismatch is found between two characters or when both strings reach the null terminator (`\0`).
- If one string is shorter than the other (i.e., one reaches `\0` before the other), the shorter string is considered "less" than the longer string.

### 3. **Lexicographical (Dictionary) Order**:

- The comparison follows lexicographical or dictionary order, where strings are compared based on the order of their characters according to their ASCII values.
    - For example, `"Apple"` comes before `"Banana"` because the ASCII value of `'A'` (65) is less than `'B'` (66).
    - `"Apple"` also comes before `"apple"` because lowercase letters have higher ASCII values than uppercase letters.

### 4. **Return Value of `strcmp` (Standard String Comparison Function)**:

- While you may not be using `strcmp` in your manual comparison, it's helpful to understand its behavior since it's part of the C standard library (`string.h`):
    - **0**: The two strings are equal.
    - **Negative value**: The first string is lexicographically less than the second.
    - **Positive value**: The first string is lexicographically greater than the second.

### ASCII Value Table for Reference:

Here are some examples of ASCII values:

|Character|ASCII Value|
|---|---|
|'A'|65|
|'B'|66|
|'a'|97|
|'b'|98|
|'0'|48|
|'1'|49|
|' ' (space)|32|
|'\0' (null)|0|

### Example of Character-by-Character Comparison:

If you compare `"Hello"` and `"hello"`:

- First character: `'H'` (ASCII 72) vs `'h'` (ASCII 104). Since `'H'` is less than `'h'`, `"Hello"` is lexicographically less than `"hello"`.

If you compare `"abc"` and `"abcd"`:

- The first three characters are the same.
- The string `"abc"` has reached the null terminator `\0`, while `"abcd"` has another character (`'d'`). Thus, `"abc"` is considered smaller than `"abcd"`.

### How to Manually Compare Strings:

When manually comparing two strings in C, you follow this process:

1. Compare characters starting from the first position.
2. Stop when:
    - A mismatch is found (one character is less than or greater than the other).
    - One string ends (reaches `\0`) and the other does not