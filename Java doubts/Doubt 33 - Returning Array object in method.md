
---



Here’s a simple example in Java where the **return type of a method is an array object**:

java

Copy code

`public class ArrayReturnExample {      // Method that returns an array of integers     public static int[] getNumbers() {         int[] numbers = {10, 20, 30, 40, 50};         return numbers;     }      public static void main(String[] args) {         int[] result = getNumbers(); // calling the method and storing returned array          // Printing the returned array         System.out.println("Returned array:");         for (int num : result) {             System.out.print(num + " ");         }     } }`

### Explanation:

- `getNumbers()` is a **static method** that returns an **array of integers (`int[]`)**.
    
- In `main()`, we call `getNumbers()` and store the result in an array named `result`.
    
- Then, we print the elements of the array using a **for-each loop**.
    

Would you like a similar example with a different data type (e.g., `String[]`, `double[]`)?

4o

##### You said:

return type is an array not an array object

###### ChatGPT said:

Let’s clarify your confusion first:

In **Java**, arrays are **objects**, regardless of the data type (primitive or reference). So when a method returns an array like `int[]`, it’s actually returning a **reference to an array object** on the heap.

So technically, **every array is an object** in Java.

### ✅ Example:

When you write:

java

Copy code

`public static int[] getArray() {     return new int[]{1, 2, 3}; }`

The return type `int[]` is an **array**, but since arrays are objects, you're returning an array object.

If your question means "returning just an array, not a wrapper object like `ArrayList`", then this example is exactly what you need:

### ✅ Example: Returning a plain array

java

Copy code

`public class Test {     public static int[] getNumbers() {         return new int[] {1, 2, 3, 4, 5};  // returning a plain array     }      public static void main(String[] args) {         int[] nums = getNumbers();  // storing returned array          for (int n : nums) {             System.out.print(n + " ");         }     } }`

### Output:

Copy code

`1 2 3 4 5`

✅ **Conclusion**: You _are_ returning an array (e.g., `int[]`), but in Java, even that is an object behind the scenes.