

---

# **1. String Class Methods (`java.lang.String`)**

The `String` class provides methods for text manipulation.

### **Basic String Operations**

|Method|Description|
|---|---|
|`int length()`|Returns the length of the string.|
|`char charAt(int index)`|Returns the character at the specified index.|
|`boolean isEmpty()`|Checks if the string is empty (`length() == 0`).|
|`boolean isBlank()`|Checks if the string contains only whitespace (Java 11+).|

### **String Comparison**

| Method                               | Description                                   |
| ------------------------------------ | --------------------------------------------- |
| `boolean equals(String s)`           | Compares two strings (case-sensitive).        |
| `boolean equalsIgnoreCase(String s)` | Compares two strings (case-insensitive).      |
| `int compareTo(String s)`            | Compares two strings lexicographically.       |
| `compareToIgnoreCase(String another) | Same as `compareTo()` but case-insensitive.\| |
| `boolean matches(String regex)`      | Checks if the string matches a given regex.   |

### **Searching in Strings**

|Method|Description|
|---|---|
|`boolean contains(CharSequence s)`|Checks if a string contains a substring.|
|`int indexOf(String s)`|Finds the first occurrence of a substring.|
|`int lastIndexOf(String s)`|Finds the last occurrence of a substring.|
|`boolean startsWith(String prefix)`|Checks if a string starts with a given prefix.|
|`boolean endsWith(String suffix)`|Checks if a string ends with a given suffix.|

### **Modifying Strings**

|Method|Description|
|---|---|
|`String toUpperCase()`|Converts to uppercase.|
|`String toLowerCase()`|Converts to lowercase.|
|`String trim()`|Removes leading and trailing spaces.|
|`String replace(char old, char new)`|Replaces a character.|
|`String replaceAll(String regex, String replacement)`|Replaces matches of a regex pattern.|

### **Extracting Substrings**

|Method|Description|
|---|---|
|`String substring(int beginIndex)`|Returns substring from a given index.|
|`String substring(int beginIndex, int endIndex)`|Returns substring from `beginIndex` to `endIndex - 1`.|

### **String Conversion**

|Method|Description|
|---|---|
|`char[] toCharArray()`|Converts string to character array.|
|`static String valueOf(anyType x)`|Converts int, double, boolean, etc., to a string.|

---

# **2. Integer Class Methods (`java.lang.Integer`)**

Used for working with `int` values.

### **Integer Creation & Conversion**

|Method|Description|
|---|---|
|`static Integer valueOf(int n)`|Returns an `Integer` object.|
|`static int parseInt(String s)`|Converts a string to an `int`.|
|`static String toString(int n)`|Converts `int` to `String`.|

### **Integer Properties**

|Method|Description|
|---|---|
|`Integer.MAX_VALUE`|2,147,483,647 (maximum `int`).|
|`Integer.MIN_VALUE`|-2,147,483,648 (minimum `int`).|

### **Bitwise Operations**

|Method|Description|
|---|---|
|`static int bitCount(int n)`|Counts `1` bits in binary.|
|`static int numberOfLeadingZeros(int n)`|Counts leading zeros.|

### **Comparing Integers**

|Method|Description|
|---|---|
|`static int compare(int a, int b)`|Compares two integers.|

---

# **3. Math Class Methods (`java.lang.Math`)**

For mathematical operations.

|Method|Description|
|---|---|
|`static int abs(int x)`|Absolute value.|
|`static double sqrt(double x)`|Square root.|
|`static double pow(double a, double b)`|Power function.|
|`static double log(double x)`|Natural logarithm (`ln(x)`).|
|`static double round(double x)`|Rounds a floating-point number.|
|`static double random()`|Generates a random number between `0.0` and `1.0`.|

---

# **4. Arrays Class Methods (`java.util.Arrays`)**

For working with arrays.

|Method|Description|
|---|---|
|`static void sort(int[] arr)`|Sorts the array.|
|`static int binarySearch(int[] arr, int key)`|Searches using binary search.|
|`static int[] copyOf(int[] arr, int newLength)`|Copies an array.|
|`static void fill(int[] arr, int value)`|Fills the array with a value.|
|`static boolean equals(int[] arr1, int[] arr2)`|Compares two arrays.|

---

# **5. Collections Class Methods (`java.util.Collections`)**

For working with lists.

|Method|Description|
|---|---|
|`static void sort(List<T> list)`|Sorts a list.|
|`static void reverse(List<T> list)`|Reverses a list.|
|`static void shuffle(List<T> list)`|Randomly shuffles a list.|
|`static T max(Collection<T> col)`|Finds the maximum element.|

---

# **6. File Handling Methods (`java.io.File`)**

For file operations.

|Method|Description|
|---|---|
|`boolean createNewFile()`|Creates a new file.|
|`boolean exists()`|Checks if file exists.|
|`boolean delete()`|Deletes a file.|
|`long length()`|Gets file size.|

---

# **7. Random Class Methods (`java.util.Random`)**

For generating random numbers.

|Method|Description|
|---|---|
|`int nextInt(int bound)`|Generates a random `int` from `0` to `bound-1`.|
|`double nextDouble()`|Generates a random double from `0.0` to `1.0`.|

**Example:**

java

Copy code

`import java.util.Random; public class Main {     public static void main(String[] args) {         Random rand = new Random();         System.out.println(rand.nextInt(100)); // Random number from 0 to 99     } }`

---

## **Conclusion**

This covers **most important Java methods** from core classes. Let me know if you want explanations, examples, or deep dives into any of these topics!