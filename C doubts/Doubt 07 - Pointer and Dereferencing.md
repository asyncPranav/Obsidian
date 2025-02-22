Dereferencing is the process of accessing or manipulating the data stored in a memory location through a pointer. In other words, it means getting the actual value stored at the address a pointer is pointing to, rather than the address itself.

In C, you use the `*` operator to dereference a pointer. Here’s a basic example:
```c
int num = 10;
int *ptr = &num;   // 'ptr' is now a pointer to 'num'
printf("%d\n", *ptr); // Dereferencing 'ptr' to get the value of 'num'

```

In this example:

- `ptr` holds the memory address of `num`.
- `*ptr` (dereferencing `ptr`) gives us access to the value stored at that address, which is `10`.

### Key Concepts:

- **Pointer**: A variable that stores the memory address of another variable.
- **Dereference**: Accessing the value at the memory address stored in a pointer.

Dereferencing is useful because it allows indirect manipulation of variables. For example, by dereferencing `ptr`, you can change the value of `num`:

```c
*ptr = 20; // Changes the value of 'num' to 20
```

After this line, `num` will hold the value `20` because `*ptr` refers to `num`.