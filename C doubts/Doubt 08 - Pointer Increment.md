- unary increment & decrement are also valid on pointers
- unary increment & decrement has higher precedence as compare to the dereference (*) operator
- *ptr++ act as *(ptr++), because precedence of ++ operator is greater than * operator
- therefore the expression increase the value of ptr and it will now point to the next memory location
- this means that *ptr++ does not perform intended task
- therefore, to increase the value of variable whose address is stored in ptr, you should write (*ptr)++
```c
int num6 = 69;
int *ptr8 = &num6;

printf("\nnum6 = %d\n", *ptr8);  // Outputs 69

(*ptr8)++;  // Increments the value at the address pointed to by ptr8, so num6 becomes 70
printf("num6 = %d\n", *ptr8);  // Outputs 70

*ptr8++;    // Increments the pointer 'ptr8' itself, not the value
printf("num6 = %d\n", *ptr8);  // Outputs the value at the new address (unexpected)
```

In C, when you increment a pointer, it doesn’t move to the _next byte_; it moves to the _next element_ of the type it points to. For example:

- If `ptr8` is an `int *` (pointer to an `int`), then `ptr8++` moves to the next `int`, which is 4 bytes away on a 32-bit system.
- This means `ptr8++` skips over all 4 bytes that `num6` occupies and points to the memory address _after_ `num6`.

### Why This Causes Unexpected Behavior

Here's a detailed breakdown of what happens in your code:
```c
int num6 = 69;      // num6 is stored in 4 contiguous bytes
int *ptr8 = &num6;  // ptr8 points to the first byte of num6
```
At this point:

- `ptr8` points to the starting address of `num6`.

Now, when you do `ptr8++`:

- `ptr8` is incremented by the size of an `int`, which is 4 bytes, so it skips over `num6` entirely and now points to the next 4-byte block in memory.

### Accessing Out-of-Bounds Memory

Since `ptr8` no longer points to `num6`, accessing `*ptr8` after `ptr8++` results in accessing an unintended memory location. This location might contain any value, leading to undefined behavior and unexpected output. In your case, this could explain why it prints `4` or another seemingly random value.

### Visualization of Memory Layout

Here’s a simplified memory visualization to clarify:

|Address|Value|Description|
|---|---|---|
|`0x1000`|69|`num6` (4 bytes)|
|`0x1004`|???|Next memory location (unused)|

- `ptr8` starts at `0x1000`, pointing to `num6`.
- `ptr8++` makes `ptr8` point to `0x1004`, which is outside the bounds of `num6`.

Since this address (`0x1004`) does not belong to `num6`, any value you see there is arbitrary, leading to unexpected output