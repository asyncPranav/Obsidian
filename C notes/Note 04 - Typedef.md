```c
#include<stdio.h>
int main(){
int age = 20;
printf("%d",age); //--------> 20
}
```

we can rename an existing or user defined data type using keyword `typedef`

```c
#include<stdio.h>
int main(){
typedef int pranav;
pranav age = 20;
printf("%d",age); //--------> 20
}
```

 now we have renamed `int` as `pranav` , we can declare new integer variables using new keyword `pranav`
 