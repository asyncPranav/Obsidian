```c
#include <stdio.h>
int main() {
    int n;
    
    // Taking order of matrix
    printf("Enter the order of the square matrices: ");
    scanf("%d", &n);
	
    // Declaring matrix
    int result[n][n];
	
    // Initializing the result matrix to zero
    result[n][n] = {0};
	
    // Print result matrix
    printf("\nResultant matrix:\n");
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            printf("%d ", result[i][j]);
        }
        printf("\n");
    }
    
    return 0;
}
```

 #DOUBT - why `result[n][n] = {0}` does not initialize result matrix with zero

The issue in code arises from the incorrect initialization of the `result` matrix. Specifically, the line:

```c 
result[n][n] = { 0 }; 
```

This is not a valid syntax in C. Array initialization like this is only valid when defining array with the constant sizes. You cannot initialize a dynamically sized array like this after declaring it. You can either initialize the array element-by-element or use specific methods like `memset` to zero it out.

### Correcting the Code

To fix your code, you should initialize the array with zeros in a loop or when declaring the array. Here's the corrected version using loops to initialize the array:

### Solution 1: Use loops to initialize the matrix

```c
#include <stdio.h>
int main() {
    int n;
	
    // Taking order of matrix
    printf("Enter the order of the square matrices: ");
    scanf("%d", &n);
	
    // Declaring matrix
    int result[n][n];
	
    // Initializing the result matrix to zero using loops
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            result[i][j] = 0;
        }
    }
	
    // Print result matrix
    printf("\nResultant matrix:\n");
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            printf("%d ", result[i][j]);
        }
        printf("\n");
    }
	
    return 0;
}
```

### Solution 2: Use `memset` for initialization (if supported in your environment)

You can also use `memset` to initialize the matrix, which can be more efficient in some cases. Here's how:

```c
#include <stdio.h>
#include <string.h> // for memset

int main() {
    int n;
	
    // Taking order of matrix
    printf("Enter the order of the square matrices: ");
    scanf("%d", &n);
	
    // Declaring matrix
    int result[n][n];
	
    // Initializing the result matrix to zero using memset
    memset(result, 0, sizeof(result));
	
    // Print result matrix
    printf("\nResultant matrix:\n");
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            printf("%d ", result[i][j]);
        }
        printf("\n");
    }
	
    return 0;
}
```
