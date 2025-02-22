```c
#include <stdio.h>
int main() {
    struct student {
        char name[30];
        int roll;
        char course[50];
        int fee;
    };
    
    struct student stud1;
    
    printf("Enter name: ");
    fgets(stud1.name, sizeof(stud1.name), stdin);
    
    printf("Enter roll no: ");
    scanf("%d", &stud1.roll);
	
    printf("Enter course: ");
    fgets(stud1.course, sizeof(stud1.course), stdin);
    
    printf("Enter fee: ");
    scanf("%d", &stud1.fee);
	
    printf("\nName = %s", stud1.name);
    printf("Roll no = %d\n", stud1.roll);
    printf("Course = %s", stud1.course);
    printf("Fee = %d\n", stud1.fee);
	
    return 0;
}
```

The code you've provided will still have an issue when reading `stud1.course`. After `scanf("%d", &stud1.roll);`, the newline character left in the input buffer will cause the next `fgets` (for `stud1.course`) to read an empty line instead of prompting for input.

#### SOLUTION :
To fix this, add a line to clear the newline character after reading `roll`. Here’s the corrected version:

```c
#include <stdio.h>
int main() {
    struct student {
        char name[30];
        int roll;
        char course[50];
        int fee;
    };
    
    struct student stud1;
    
    printf("Enter name: ");
    fgets(stud1.name, sizeof(stud1.name), stdin);
    
    printf("Enter roll no: ");
    scanf("%d", &stud1.roll);
	
    // Clear newline character left in the buffer
    while (getchar() != '\n');
	
    printf("Enter course: ");
    fgets(stud1.course, sizeof(stud1.course), stdin);
    
    printf("Enter fee: ");
    scanf("%d", &stud1.fee);
	
    printf("\nName = %s", stud1.name);
    printf("Roll no = %d\n", stud1.roll);
    printf("Course = %s", stud1.course);
    printf("Fee = %d\n", stud1.fee);
	
    return 0;
}
```

### Explanation of the Fix:

- The line `while (getchar() != '\n');` after `scanf` clears the newline character from the buffer.
- This ensures that the next `fgets` call for `stud1.course` works correctly, prompting the user to input the course name.

Now, this code will work as expected for all inputs.



#DOUBT - **working of while() loop**

After calling `scanf("%d", &stud1.roll);`, the newline character (`'\n'`) that you pressed after entering the integer remains in the input buffer. This leftover newline causes issues for the next `fgets` call, which will read this newline as an empty line instead of waiting for actual user input.

### How `while (getchar() != '\n');` Works

The line `while (getchar() != '\n');` clears everything left in the input buffer up to and including the newline character.

Here’s the detailed breakdown:

1. **`getchar()` Call:**
    
    - `getchar()` reads the next character from the input buffer (standard input).
    - After `scanf`, the buffer still contains the newline character (`'\n'`) that was left over.
2. **Loop Execution:**
    
    - The `while` loop checks if the character read by `getchar()` is NOT a newline (`'\n'`).
    - If `getchar()` reads any character other than `'\n'`, it keeps looping and reading the next character.
3. **End Condition:**
    
    - Once `getchar()` reads the newline character (`'\n'`), the condition `getchar() != '\n'` becomes false, and the loop stops.
    - This effectively clears all characters in the buffer up to and including the newline, leaving the buffer empty for the next `fgets` call.

### Why This Matters

After this loop runs, the input buffer is now empty. So when you call `fgets`, it will correctly prompt the user for new input rather than immediately returning an empty line from the leftover newline character.

### Example Walkthrough

Imagine this sequence of inputs:

1. You enter `42` for `stud1.roll` and press `Enter`.
    
    - The buffer now contains: `4`, `2`, `\n`
    - `scanf("%d", &stud1.roll);` reads `42`, leaving `\n` in the buffer.
2. `while (getchar() != '\n');` executes:
    
    - `getchar()` reads `\n` and stops immediately because it found the newline.
3. The input buffer is now clear. The next `fgets` call will correctly wait for new input from the user.
    

This `while` loop effectively ensures that the input buffer is empty before the next read operation.