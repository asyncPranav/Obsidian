arr[i] = i[arr] in c , please clear this statement i am confused in i[arr]

###### ChatGPT said:
In C, the expression `arr[i]` is equivalent to `i[arr]`. This may seem strange, but it's because of how array indexing works in C. Both `arr[i]` and `i[arr]` yield the same result because of the way pointer arithmetic operates.

### Explanation:

In C, `arr[i]` is simply shorthand for `*(arr + i)`. When you write `arr[i]`, the compiler interprets it as "take the pointer `arr`, add `i` to it, and dereference it to get the value at that position."

Similarly, `i[arr]` is interpreted as `*(i + arr)`, which is exactly the same thing, because addition is commutative (i.e., `arr + i` is the same as `i + arr`).

### Example:
```c
#include <stdio.h>

int main() {
    int arr[] = {10, 20, 30, 40, 50};
    int i = 2;
    
    printf("arr[i]: %d\n", arr[i]);   // Output: 30
    printf("i[arr]: %d\n", i[arr]);   // Output: 30
    
    return 0;
}
```
Both `arr[i]` and `i[arr]` will output the same value, `30`, because they both evaluate to the same expression `*(arr + i)`.

### Why This Works:

In C, the subscript operator `[]` is defined such that `a[b]` is equivalent to `*(a + b)`. Since `+` is commutative, `*(a + b)` is the same as `*(b + a)`, so `a[b]` is the same as `b[a]`.

This behavior might not be commonly used, but it’s a quirky and valid part of C's syntax.


##### You said:

but it take arr as pointer and add i to it in arr[i] then will it take i as pointer and arr to it in i[arr]

###### ChatGPT said:

Good question! While it may seem like `i[arr]` should take `i` as a pointer, it actually doesn't. In both `arr[i]` and `i[arr]`, the array `arr` is the base address, and `i` is simply an index, not a pointer. This is because of the way array indexing is defined in C.

When you write `arr[i]` or `i[arr]`, the compiler interprets it as `*(arr + i)` regardless of the order of `arr` and `i`. So, even if you write `i[arr]`, the compiler will treat `arr` as the array (or pointer) and `i` as the index.

### Why This Works

In C, the expression `a[b]` is always interpreted as `*(a + b)`. Here’s the logic behind it:

1. **Array Names as Pointers:** When you pass an array name like `arr`, it decays to a pointer to the first element (i.e., `&arr[0]`). So `arr` can be treated as a pointer.
    
2. **Pointer Arithmetic in Array Access:** When you write `arr[i]`, it’s actually shorthand for `*(arr + i)`, where `arr` is the base address and `i` is the offset.
    
3. **Commutative Property:** Since addition is commutative, `arr + i` is the same as `i + arr`. This is why `i[arr]` works exactly the same way as `arr[i]`.
    

So in `i[arr]`, even though it looks like `i` should be a pointer, the compiler still treats `arr` as the array and `i` as the offset.

### Example for Clarification

```c
#include <stdio.h>

int main() {
    int arr[] = {10, 20, 30, 40, 50};
    int i = 3;
    
    printf("arr[i]: %d\n", arr[i]);   // Accesses arr[3], which is 40
    printf("i[arr]: %d\n", i[arr]);   // Also accesses arr[3], which is 40
    
    return 0;
}
```
### Key Point:

In `i[arr]`, `arr` is still treated as the pointer, not `i`. The syntax works because `a[b]` is defined as `*(a + b)`, making `arr[i]` and `i[arr]` identical in C.