
```c
//read and display information of a student using nested structure

#include<stdio.h>
int main(){
    struct name{
        char fname[20];
        char mname[20];
        char lname[20];
    };
    struct dob{
        int day;
        int month;
        int year;
    };
    struct student{
        struct name sname;
        struct dob sdob;
        int roll;
    };
    
    struct student s1;
    
    s1.sname.fname = "pranav";
    s1.sname.mname = "singh";
    s1.sname.lname = "chandel";
    s1.sdob.day = 23;
    s1.sdob.month = 02;
    s1.sdob.year = 2005;
    s1.roll = 42;
  
    printf("Name = %s %s %s\n",s1.sname.fname, s1.sname.mname, s1.sname.lname);
    printf("DOB = %d-%d-%d\n",s1.sdob.day, s1.sdob.month, s1.sdob.year);
    printf("Rollno = %d\n",s1.roll);
  
    return 0;
}
```

Your code has an issue with assigning string values to the character arrays within the `struct name`. In C, you cannot directly assign strings to arrays using the `=` operator; instead, you need to use `strcpy` from `string.h`. Here’s the corrected code:

```c
#include <stdio.h>
#include <string.h>
int main() {
    struct name {
        char fname[20];
        char mname[20];
        char lname[20];
    };
    struct dob {
        int day;
        int month;
        int year;
    };
    struct student {
        struct name sname;
        struct dob sdob;
        int roll;
    };
    
    struct student s1;
	
    strcpy(s1.sname.fname, "pranav");
    strcpy(s1.sname.mname, "singh");
    strcpy(s1.sname.lname, "chandel");
    s1.sdob.day = 23;
    s1.sdob.month = 2;
    s1.sdob.year = 2005;
    s1.roll = 42;
	
    printf("Name = %s %s %s\n", s1.sname.fname, s1.sname.mname, s1.sname.lname);
    printf("DOB = %d-%d-%d\n", s1.sdob.day, s1.sdob.month, s1.sdob.year);
    printf("Rollno = %d\n", s1.roll);
	
    return 0;
}
```

### Explanation of Changes:

1. `#include <string.h>` is added to enable the use of `strcpy`.
2. `strcpy(s1.sname.fname, "pranav");` (and similar lines) replaces the direct assignment, which is not allowed with arrays in C. `strcpy` copies the string into the character array correctly.

Now, this program will display the student's full name, date of birth, and roll number as expected.
