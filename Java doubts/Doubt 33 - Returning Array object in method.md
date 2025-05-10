
---



Here’s a simple example in Java where the **return type of a method is an array object**:

```java
public class ArrayReturnExample {

    // Method that returns an array of integers
    public static int[] getNumbers() {
        int[] numbers = {10, 20, 30, 40, 50};
        return numbers;
    }
		
    public static void main(String[] args) {
        int[] result = getNumbers(); // calling the method and storing returned array
		
        // Printing the returned array
        System.out.println("Returned array:");
        for (int num : result) {
            System.out.print(num + " ");
        }
    }
}
```

### Explanation:

- `getNumbers()` is a **static method** that returns an **array of integers (`int[]`)**.
    
- In `main()`, we call `getNumbers()` and store the result in an array named `result`.
    
- Then, we print the elements of the array using a **for-each loop**.
    


---


### **Return type is an array not an array object**



In **Java**, arrays are **objects**, regardless of the data type (primitive or reference). So when a method returns an array like `int[]`, it’s actually returning a **reference to an array object** on the heap.

So technically, **every array is an object** in Java.

### ✅ Example:

When you write:

```java
public static int[] getArray() {
    return new int[]{1, 2, 3};
}
```

The return type `int[]` is an **array**, but since arrays are objects, you're returning an array object.

If your question means "returning just an array, not a wrapper object like `ArrayList`", then this example is exactly what you need:

### ✅ Example: Returning a plain array

```java
public class Test {
    public static int[] getNumbers() {
        return new int[] {1, 2, 3, 4, 5};  // returning a plain array
    }
		
    public static void main(String[] args) {
        int[] nums = getNumbers();  // storing returned array
		
        for (int n : nums) {
            System.out.print(n + " ");
        }
    }
}
```

### Output:

```sh
1 2 3 4 5
```

✅ **Conclusion**: You _are_ returning an array (e.g., `int[]`), but in Java, even that is an object behind the scenes.

---

Great! Here's an example using a **`String[]`** return type and a **2D array** (`int[][]`) return type to show both:



### ✅ Example 1: Method returns a `String[]` (1D array)

```java
public class StringArrayExample {
    public static String[] getNames() {
        return new String[] {"Alice", "Bob", "Charlie"};
    }
		
    public static void main(String[] args) {
        String[] names = getNames();
		
        for (String name : names) {
            System.out.println(name);
        }
    }
}
```

**Output:**

```sh
Alice
Bob
Charlie
```

---

### ✅ Example 2: Method returns an `int[][]` (2D array)

```java
public class TwoDArrayExample {
    public static int[][] getMatrix() {
        return new int[][] {
            {1, 2, 3},
            {4, 5, 6}
        };
    }
	
    public static void main(String[] args) {
        int[][] matrix = getMatrix();
		
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                System.out.print(matrix[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```

**Output:**

```sh
1 2 3
4 5 6
```

