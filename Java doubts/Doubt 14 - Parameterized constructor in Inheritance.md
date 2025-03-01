
```java
class Parentt {
    public Parentt() {
        System.out.println("non param of Parent");
    }
    
    public Parentt(int x) {
        System.out.println("param of Parent");
    }
}

class Childd extends Parentt {
    public Childd(int x) {
        System.out.println("param of Child");
    }
}

public class _03_InheritParameterizedConstructor {
    public static void main(String[] args) {
        Childd c2 = new Childd(3);
    }
}
```