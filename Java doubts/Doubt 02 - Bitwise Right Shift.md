```java
System.out.println(-5/2); 
System.out.println(-5>>1);
```

Right shift operator ( >> )  :  number >> k  -------> $number/2^k$  

-5 >> 1 -----> $-5/2^1$ ---> $-5/2$

#DOUBT  **Then why output of both line is different ??**


The outputs of the two lines are different because they perform fundamentally different operations:

### Line 1: `System.out.println(-5 / 2);`

- This performs **integer division**.
- In integer division, `-5 / 2` evaluates to `-2`, as it truncates the fractional part toward **zero**.
- Calculation: 
	$-5 / 2 = -2.5$  ( fractional part discarded ) => -2

### Line 2: `System.out.println(-5 >> 1);`

- This performs a **signed right shift**.
- `-5` in binary (using two's complement for a 32-bit integer) is: 1111111111111111111111111111101111111111 11111111 11111111 1111101111111111111111111111111111111011
- Shifting right by 1 bit (`>>`) divides the number by 2 while preserving the **sign bit** (most significant bit), which ensures the result remains negative: 11111111111111111111111111111101(binary for -3)11111111 11111111 11111111 11111101 \quad \text{(binary for -3)}11111111111111111111111111111101(binary for -3)
- Hence, the result is `-3`.

### Why the Results Differ

- **Division** truncates the result toward zero (`-2`).
- **Right shift** divides by 2 but rounds toward negative infinity (`-3`), as it effectively shifts the binary representation.