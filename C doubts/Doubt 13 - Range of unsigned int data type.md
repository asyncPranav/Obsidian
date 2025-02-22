```c
 Range :  %u < %lu < %llu
```

###  Arbitrary-Precision Libraries

For numbers even larger than what `unsigned long long int` can hold (like 50!50!50! or 100!100!100!), you’ll need an **arbitrary-precision library**. In C, the most common choice is **GMP (GNU Multiple Precision Arithmetic Library)**, which supports very large integers.

#### Example with GMP:

Here’s how you would use GMP to compute and store large factorials:

1. **Install GMP** (usually with `sudo apt install libgmp-dev` on Linux).
2. Include GMP’s header and use its `mpz_t` type for large integers.

```c
#include <stdio.h>
#include <gmp.h>
int main() {
    mpz_t factorial;
    mpz_init(factorial);  // Initialize the big integer
    mpz_fac_ui(factorial, 20);  // Calculate 20! and store in factorial
    gmp_printf("20! is: %Zd\n", factorial);  // Print the result
    mpz_clear(factorial);  // Clear the memory allocated for factorial
    return 0;
}
```

This program uses `mpz_t` to handle large integers and `mpz_fac_ui` to calculate factorials.

### Summary

- For 20!, `unsigned long long int` (64-bit) is sufficient.
- For even larger values, use **GMP** or another arbitrary-precision library.