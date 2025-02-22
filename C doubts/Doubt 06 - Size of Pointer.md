```c
#include<stdio.h>
int main(){
    int *pnum;
    char *pchar;
    float *pfloat;
    double *pdouble;
    //size of all pointers of diff data-type is same
    printf("integer pointer size = %d\n",sizeof(pnum));
    printf("character pointer size = %d\n",sizeof(pchar));
    printf("float pointer size = %d\n",sizeof(pfloat));
    printf("double pointer size = %d\n",sizeof(pdouble));
    return 0;
}
```

Yes, the size of pointers in a given system (for example, 32-bit or 64-bit) is generally the same across all data types. This is because a pointer holds a memory address, and the size of an address is determined by the system's architecture, not by the type of data to which the pointer points.

Here’s a breakdown:

- **32-bit systems**: All pointers are typically 4 bytes (32 bits) in size.
- **64-bit systems**: All pointers are typically 8 bytes (64 bits) in size.

However, the data type a pointer refers to (e.g., `int`, `float`, `char`) affects how the program interprets the data stored at that address, but not the size of the pointer itself.

### Exceptions

1. **Different Compilers**: Some compilers may implement pointer sizes differently, especially in specialized systems (embedded systems, for example).
2. **Function Pointers**: In some cases, function pointers might have different sizes from data pointers.
3. **Memory Models**: In segmented memory models (e.g., some older 16-bit systems), pointers can vary in size based on segment offsets.

In most modern applications, though, pointer sizes are uniform across data types on a given architecture