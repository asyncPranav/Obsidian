
```java
package $18_Exception_Handling;  
  
public class _18_FinallyBlock {  
    public static void meth1() throws Exception{  
        /*  
        throw new Exception();  
        //------> I want to print this final statement even if method is throwing an exception, but this statement is unreachable because throwing exception means method will terminate in that line and control will pass to the calling method        System.out.println("Finally statement");         */  
        // You can make it executable using Finally block  
        try{  
            throw new Exception();  
        }  
        finally {  
            System.out.println("Finally statement");  
        }  
    }  
    public static void main(String[] args) throws Exception{  
        meth1();  
    }  
}
```


### **Expected Execution Flow**

1. **An exception is thrown** inside the `try` block in `meth1()`.
    
2. **The `finally` block executes** before the method exits.
    
3. **The exception propagates** to `main()`, which doesn't handle it, so the program crashes.
    

### **Your Expected Output**

You expected:

```sh
Finally statement  
Exception in thread "main" java.lang.Exception  
    at $18_Exception_Handling._18_FinallyBlock.meth1(_18_FinallyBlock.java:14)  
    at $18_Exception_Handling._18_FinallyBlock.main(_18_FinallyBlock.java:21)  
``` 

but you got:

```sh
Exception in thread "main" java.lang.Exception  
    at $18_Exception_Handling._18_FinallyBlock.meth1(_18_FinallyBlock.java:14)  
    at $18_Exception_Handling._18_FinallyBlock.main(_18_FinallyBlock.java:21)  
Finally statement  
```  

### **Why Is the Stack Trace Printed First?**

- **Java prints the exception stack trace immediately** when an exception is thrown.
    
- But **`finally` still executes before the method exits.**
    
- However, Java internally **buffers console outputs differently** for exceptions and normal print statements (`System.out.println`).
    
- The **stack trace gets printed first** because Java flushes error messages (`System.err`) before regular console output (`System.out`).
    

### **Solution to Force the Expected Order**

If you want `"Finally statement"` to be printed **before** the exception, add this before the `throw` statement:

java

CopyEdit

`System.out.flush();  // Ensures all console output is printed before exception`

Updated `meth1()`:

java

CopyEdit

`public static void meth1() throws Exception {     try {         System.out.flush();  // Flush console output buffer         throw new Exception();     } finally {         System.out.println("Finally statement");     } }`

Now, your expected output will appear as:

pgsql

CopyEdit

`Finally statement   Exception in thread "main" java.lang.Exception       at $18_Exception_Handling._18_FinallyBlock.meth1(_18_FinallyBlock.java:14)       at $18_Exception_Handling._18_FinallyBlock.main(_18_FinallyBlock.java:21)`  

---

### **Final Summary**

✔ **The order of execution is still correct (finally executes before exception propagates).**  
✔ **The difference in output order is due to Java's internal handling of console output (`System.out`) vs. error messages (`System.err`).**  
✔ **Using `System.out.flush()` ensures `"Finally statement"` is printed first.**