
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


