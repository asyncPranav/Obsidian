

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

| Method                                | Description                                   |
| ------------------------------------- | --------------------------------------------- |
| `boolean equals(String s)`            | Compares two strings (case-sensitive).        |
| `boolean equalsIgnoreCase(String s)`  | Compares two strings (case-insensitive).      |
| `int compareTo(String s)`             | Compares two strings lexicographically.       |
| `compareToIgnoreCase(String another)` | Same as `compareTo()` but case-insensitive.\| |
| `boolean matches(String regex)`       | Checks if the string matches a given regex.   |

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
|`String toUpperCase()`|Converts the string to uppercase.|
|`String toLowerCase()`|Converts the string to lowercase.|
|`String trim()`|Removes leading and trailing spaces.|
|`String strip()`|Similar to `trim()`, but also removes Unicode whitespace (Java 11+).|
|`String stripLeading()`|Removes leading spaces (Java 11+).|
|`String stripTrailing()`|Removes trailing spaces (Java 11+).|
|`String replace(char old, char new)`|Replaces all occurrences of a character.|
|`String replace(String old, String new)`|Replaces all occurrences of a substring.|
|`String replaceAll(String regex, String replacement)`|Replaces all matches of a regex pattern.|
|`String replaceFirst(String regex, String replacement)`|Replaces the first match of a regex pattern.|


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


### **Splitting and Joining**

|Method|Description|
|---|---|
|`split(String regex)`|Splits a string based on a regex pattern.|
|`split(String regex, int limit)`|Splits a string into a limited number of parts.|
|`join(CharSequence delimiter, CharSequence... elements)`|Joins multiple strings with a delimiter.|


### **Conversion Methods**

|Method|Description|
|---|---|
|`String.valueOf(anyType)`|Converts int, double, boolean, char, etc., to a string.|
|`getBytes()`|Converts a string into a byte array.|
|`toCharArray()`|Converts a string into a character array.|

---

### **String Formatting**

|Method|Description|
|---|---|
|`format(String format, Object... args)`|Returns a formatted string.|
|`printf(String format, Object... args)`|Prints a formatted string to the console.|

Example:

```java
String formatted = String.format("Hello, %s!", "John");
System.out.println(formatted);  // Output: Hello, John!
```

---

### **StringBuilder & StringBuffer Methods (For Mutable Strings)**

|Method|Description|
|---|---|
|`append(String s)`|Adds a string at the end.|
|`insert(int offset, String s)`|Inserts a string at a specified position.|
|`replace(int start, int end, String s)`|Replaces part of the string.|
|`delete(int start, int end)`|Deletes part of the string.|
|`reverse()`|Reverses the string.|


---

# **2. Integer Class Methods (`java.lang.Integer`)**

Used for working with `int` values.

### **Integer Creation & Conversion**

|Method|Description|
|---|---|
|`Integer.valueOf(int n)`|Returns an `Integer` object from an `int`.|
|`Integer.valueOf(String s)`|Returns an `Integer` object from a `String`.|
|`Integer.parseInt(String s)`|Converts a string to a primitive `int`.|
|`Integer.parseInt(String s, int radix)`|Converts a string in a specific base (e.g., binary, octal) to an `int`.|
|`Integer.toString(int n)`|Converts an `int` to a `String`.|
|`Integer.toString(int n, int radix)`|Converts an `int` to a `String` in a specific base.|

**Example:**

```java
int num = Integer.parseInt("123");
System.out.println(num); // Output: 123
```

---

### **Integer Properties**

|Method|Description|
|---|---|
|`Integer.MAX_VALUE`|Returns the maximum value of `int` (2,147,483,647).|
|`Integer.MIN_VALUE`|Returns the minimum value of `int` (-2,147,483,648).|
|`Integer.SIZE`|Returns the number of bits used to represent `int` (32).|
|`Integer.BYTES`|Returns the number of bytes used to store `int` (4).|
|`Integer.TYPE`|Returns the primitive `int` type (`int.class`).|

---

### **Bitwise Operations**

|Method|Description|
|---|---|
|`Integer.bitCount(int n)`|Returns the number of `1` bits in the binary representation of `n`.|
|`Integer.highestOneBit(int n)`|Returns the highest power of 2 ≤ `n`.|
|`Integer.lowestOneBit(int n)`|Returns the lowest power of 2 in `n`.|
|`Integer.numberOfLeadingZeros(int n)`|Counts leading zeros in the binary form of `n`.|
|`Integer.numberOfTrailingZeros(int n)`|Counts trailing zeros in the binary form of `n`.|
|`Integer.reverse(int n)`|Reverses the bit order of `n`.|
|`Integer.reverseBytes(int n)`|Reverses the byte order of `n`.|
|`Integer.rotateLeft(int n, int distance)`|Rotates bits of `n` left by `distance`.|
|`Integer.rotateRight(int n, int distance)`|Rotates bits of `n` right by `distance`.|
|`Integer.signum(int n)`|Returns `1` if `n` is positive, `-1` if negative, and `0` if zero.|

**Example:**

```java
int n = 23;
System.out.println(Integer.bitCount(n)); // Output: 4 (10111 has 4 ones)
```

---

### **Comparing & Checking Integers**

|Method|Description|
|---|---|
|`Integer.compare(int a, int b)`|Compares two integers (`a - b`).|
|`Integer.compareUnsigned(int a, int b)`|Compares two integers as unsigned.|
|`Integer.equals(Object obj)`|Checks if an `Integer` object equals another.|

**Example:**

```java
System.out.println(Integer.compare(10, 20)); // Output: -1
```

---

### **Base Conversion (Binary, Octal, Hex)**

|Method|Description|
|---|---|
|`Integer.toBinaryString(int n)`|Converts `n` to a binary string.|
|`Integer.toOctalString(int n)`|Converts `n` to an octal string.|
|`Integer.toHexString(int n)`|Converts `n` to a hexadecimal string.|

**Example:**
```java
int num = 23;
System.out.println(Integer.toBinaryString(num)); // Output: 10111
System.out.println(Integer.toOctalString(num));  // Output: 27
System.out.println(Integer.toHexString(num));   // Output: 17
```

---

### **Unsigned Integer Operations (Java 8+)**

|Method|Description|
|---|---|
|`Integer.divideUnsigned(int a, int b)`|Performs unsigned division.|
|`Integer.remainderUnsigned(int a, int b)`|Finds the remainder using unsigned division.|
|`Integer.toUnsignedLong(int n)`|Converts `int` to `long` without sign extension.|

**Example:**

```java
int a = -5, b = 3;
System.out.println(Integer.divideUnsigned(a, b)); // Treats -5 as unsigned
```


---

# **3. Math Class Methods (`java.lang.Math`)**


### **1. Basic Mathematical Operations**

|Method|Return Type|Description|
|---|---|---|
|`int abs(int a)`|`int`|Returns the absolute value of `a`.|
|`double abs(double a)`|`double`|Returns the absolute value of `a`.|
|`int max(int a, int b)`|`int`|Returns the larger of `a` and `b`.|
|`double max(double a, double b)`|`double`|Returns the larger of `a` and `b`.|
|`int min(int a, int b)`|`int`|Returns the smaller of `a` and `b`.|
|`double min(double a, double b)`|`double`|Returns the smaller of `a` and `b`.|

🔹 **Example**:

```java
System.out.println(Math.abs(-10)); // 10
System.out.println(Math.max(10, 20)); // 20
System.out.println(Math.min(10, 20)); // 10
```


### **2. Exponentiation and Logarithms**

|Method|Return Type|Description|
|---|---|---|
|`double pow(double a, double b)`|`double`|Returns `a` raised to the power `b` (`a^b`).|
|`double sqrt(double a)`|`double`|Returns the square root of `a`.|
|`double cbrt(double a)`|`double`|Returns the cube root of `a`.|
|`double exp(double a)`|`double`|Returns `e^a`.|
|`double log(double a)`|`double`|Returns the natural logarithm (`ln(a)`).|
|`double log10(double a)`|`double`|Returns the base 10 logarithm of `a`.|

🔹 **Example**:

```java
System.out.println(Math.pow(2, 3)); // 8.0
System.out.println(Math.sqrt(16)); // 4.0
System.out.println(Math.log(2.718)); // Approx. 1.0
System.out.println(Math.log10(100)); // 2.0
```


### **3. Rounding and Formatting**

|Method|Return Type|Description|
|---|---|---|
|`double ceil(double a)`|`double`|Rounds `a` **up** to the nearest integer.|
|`double floor(double a)`|`double`|Rounds `a` **down** to the nearest integer.|
|`long round(double a)`|`long`|Rounds `a` to the nearest integer (returns `long`).|

🔹 **Example**:

```java
System.out.println(Math.ceil(4.2)); // 5.0
System.out.println(Math.floor(4.8)); // 4.0
System.out.println(Math.round(4.5)); // 5
System.out.println(Math.round(4.4)); // 4
```

---

### **4. Trigonometric Functions**

|Method|Return Type|Description|
|---|---|---|
|`double sin(double a)`|`double`|Returns the sine of `a` (radians).|
|`double cos(double a)`|`double`|Returns the cosine of `a` (radians).|
|`double tan(double a)`|`double`|Returns the tangent of `a` (radians).|
|`double asin(double a)`|`double`|Returns the arcsine (inverse sine) of `a` (radians).|
|`double acos(double a)`|`double`|Returns the arccosine (inverse cosine) of `a` (radians).|
|`double atan(double a)`|`double`|Returns the arctangent (inverse tangent) of `a` (radians).|

🔹 **Example**:

```java
System.out.println(Math.sin(Math.PI / 2)); // 1.0
System.out.println(Math.cos(0)); // 1.0
System.out.println(Math.tan(Math.PI / 4)); // 1.0
```


### **5. Random Number Generation**

|Method|Return Type|Description|
|---|---|---|
|`double random()`|`double`|Returns a random number between `0.0` and `1.0`.|

🔹 **Example**:

```java
System.out.println(Math.random()); // Random number between 0.0 and 1.0
```

🔹 To generate a random number in a **range**:

```java
int min = 1, max = 100;
int randomNumber = min + (int)(Math.random() * (max - min + 1));
System.out.println(randomNumber); // Random number between 1 and 100
```

---

### **6. Constants in `Math` Class**

|Constant|Description|
|---|---|
|`Math.PI`|The constant **π** (`3.141592653589793`).|
|`Math.E`|The constant **e** (`2.718281828459045`).|

🔹 **Example**:

```java
System.out.println(Math.PI); // 3.141592653589793
System.out.println(Math.E); // 2.718281828459045
```

---

### **Summary**

- **Basic Operations**: `abs()`, `max()`, `min()`
    
- **Exponents & Logs**: `pow()`, `sqrt()`, `log()`, `log10()`
    
- **Rounding**: `ceil()`, `floor()`, `round()`
    
- **Trigonometry**: `sin()`, `cos()`, `tan()`, `asin()`, `acos()`, `atan()`
    
- **Random Numbers**: `random()`
    
- **Constants**: `PI`, `E`


---

# **4. Arrays Class Methods (`java.util.Arrays`)**


### **1. Sorting and Searching**

|Method|Return Type|Description|
|---|---|---|
|`void sort(int[] array)`|`void`|Sorts the array in ascending order.|
|`void sort(int[] array, int fromIndex, int toIndex)`|`void`|Sorts a subarray from `fromIndex` (inclusive) to `toIndex` (exclusive).|
|`int binarySearch(int[] array, int key)`|`int`|Returns the index of `key` if found, otherwise returns `-(insertion point) - 1`.|

🔹 **Example**:

```java
import java.util.Arrays;

int[] arr = {5, 2, 8, 1, 9};
Arrays.sort(arr);
System.out.println(Arrays.toString(arr)); // [1, 2, 5, 8, 9]

int index = Arrays.binarySearch(arr, 5);
System.out.println(index); // 2 (found at index 2)
```



### **2. Filling and Copying**

|Method|Return Type|Description|
|---|---|---|
|`void fill(int[] array, int value)`|`void`|Fills the array with `value`.|
|`int[] copyOf(int[] original, int newLength)`|`int[]`|Copies the array to a new array of size `newLength`.|
|`int[] copyOfRange(int[] original, int from, int to)`|`int[]`|Copies elements from `from` (inclusive) to `to` (exclusive).|

🔹 **Example**:

```java
int[] arr = new int[5];
Arrays.fill(arr, 7);
System.out.println(Arrays.toString(arr)); // [7, 7, 7, 7, 7]

int[] copied = Arrays.copyOf(arr, 3);
System.out.println(Arrays.toString(copied)); // [7, 7, 7]
```


### **3. Comparing and Checking Equality**

|Method|Return Type|Description|
|---|---|---|
|`boolean equals(int[] a, int[] b)`|`boolean`|Returns `true` if both arrays are equal (same elements & order).|
|`boolean deepEquals(Object[] a, Object[] b)`|`boolean`|Returns `true` for deep comparison of nested arrays.|
|`int compare(int[] a, int[] b)`|`int`|Compares lexicographically (`-1`, `0`, or `1`).|
|`boolean mismatch(int[] a, int[] b)`|`int`|Returns the index of the first mismatch, or `-1` if equal.|

🔹 **Example**:

```java
int[] arr1 = {1, 2, 3};
int[] arr2 = {1, 2, 3};
System.out.println(Arrays.equals(arr1, arr2)); // true

int[] arr3 = {1, 2, 4};
System.out.println(Arrays.mismatch(arr1, arr3)); // 2 (index where mismatch occurs)
```


### **4. Converting Arrays to String**

|Method|Return Type|Description|
|---|---|---|
|`String toString(int[] array)`|`String`|Converts array to string (`[1, 2, 3]`).|
|`String deepToString(Object[] array)`|`String`|Converts **nested arrays** to string.|

🔹 **Example**:

```java
int[] arr = {1, 2, 3};
System.out.println(Arrays.toString(arr)); // [1, 2, 3]

int[][] nested = {{1, 2}, {3, 4}};
System.out.println(Arrays.deepToString(nested)); // [[1, 2], [3, 4]]
```


### **5. Parallel Operations (Faster Sorting)**

|Method|Return Type|Description|
|---|---|---|
|`void parallelSort(int[] array)`|`void`|Faster multi-threaded sorting.|
|`void parallelPrefix(int[] array, IntBinaryOperator op)`|`void`|Applies a function cumulatively to array elements.|
|`void parallelSetAll(int[] array, IntUnaryOperator op)`|`void`|Sets each element based on the given function.|

🔹 **Example**:

```java
Arrays.parallelSort(arr); // Faster than sort() for large arrays
```

---

### **6. Summary**

- **Sorting & Searching**: `sort()`, `binarySearch()`
    
- **Filling & Copying**: `fill()`, `copyOf()`, `copyOfRange()`
    
- **Comparing**: `equals()`, `mismatch()`
    
- **String Conversion**: `toString()`, `deepToString()`
    
- **Parallel Operations**: `parallelSort()`, `parallelSetAll()`

---

# **5. Collections Class Methods (`java.util.Collections`)**

For working with lists.

|Method|Description|
|---|---|
|`void sort(List<T> list)`|Sorts the list in **ascending order** (natural order).|
|`void sort(List<T> list, Comparator<? super T> c)`|Sorts the list using a **custom comparator**.|
|`int binarySearch(List<T> list, T key)`|Searches for `key` using **binary search** and returns the index (must be sorted).|
|`void reverse(List<?> list)`|Reverses the order of elements in the list.|
|`void shuffle(List<?> list)`|Randomly shuffles the elements in the list.|
|`void swap(List<?> list, int i, int j)`|Swaps two elements at indices `i` and `j`.|
|`void fill(List<? super T> list, T obj)`|Replaces all elements with the specified object.|
|`T min(Collection<? extends T> coll)`|Returns the **minimum element** in the collection.|
|`T max(Collection<? extends T> coll)`|Returns the **maximum element** in the collection.|
|`void rotate(List<?> list, int distance)`|Rotates elements in the list by the specified distance.|
|`int frequency(Collection<?> coll, Object obj)`|Returns **frequency count** of an element in the collection.|
|`boolean disjoint(Collection<?> c1, Collection<?> c2)`|Returns `true` if two collections have **no common elements**.|
|`void copy(List<? super T> dest, List<? extends T> src)`|Copies elements from `src` to `dest`.|
|`void replaceAll(List<T> list, T oldVal, T newVal)`|Replaces all occurrences of `oldVal` with `newVal`.|
|`Enumeration<T> enumeration(Collection<T> c)`|Converts a collection into an **Enumeration**.|
|`ArrayList<T> list(Enumeration<T> e)`|Converts an **Enumeration** into a `List`.|
|`Set<T> singleton(T obj)`|Returns an **immutable set** with one element.|
|`List<T> singletonList(T obj)`|Returns an **immutable list** with one element.|
|`Map<K, V> singletonMap(K key, V value)`|Returns an **immutable map** with one key-value pair.|
|`Collection<T> synchronizedCollection(Collection<T> c)`|Returns a **thread-safe** collection.|
|`List<T> synchronizedList(List<T> list)`|Returns a **thread-safe** list.|
|`Set<T> synchronizedSet(Set<T> set)`|Returns a **thread-safe** set.|
|`Map<K, V> synchronizedMap(Map<K, V> map)`|Returns a **thread-safe** map.|
|`NavigableMap<K, V> synchronizedNavigableMap(NavigableMap<K, V> map)`|Returns a **thread-safe** navigable map.|

---

### **Example Usage**

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>(Arrays.asList(5, 3, 8, 1, 9));
		
        // Sorting
        Collections.sort(numbers);
        System.out.println("Sorted: " + numbers); // [1, 3, 5, 8, 9]
		
        // Reversing
        Collections.reverse(numbers);
        System.out.println("Reversed: " + numbers); // [9, 8, 5, 3, 1]
		
        // Shuffling
        Collections.shuffle(numbers);
        System.out.println("Shuffled: " + numbers);
		
        // Finding min and max
        System.out.println("Min: " + Collections.min(numbers)); // Smallest element
        System.out.println("Max: " + Collections.max(numbers)); // Largest element
		
        // Frequency of element
        System.out.println("Frequency of 3: " + Collections.frequency(numbers, 3));
    }
}
```



---



# **6. File Handling Methods (`java.io.File` & `java.nio.file` )**

Java provides **file handling** through classes in the `java.io` and `java.nio.file` packages. Below is a **detailed list** of important methods for working with files.

---

### **1. Creating and Deleting Files (`File` Class - `java.io`)**

| Method                    | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| `boolean createNewFile()` | Creates a new empty file and returns `true` if successful.   |
| `boolean delete()`        | Deletes the file and returns `true` if deleted successfully. |
| `boolean exists()`        | Checks if the file exists.                                   |
| `boolean mkdir()`         | Creates a new **directory**.                                 |
| `boolean mkdirs()`        | Creates **multiple directories** (parent + child).           |

**Example:**

```java
import java.io.File;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        File file = new File("test.txt");
		
        try {
            if (file.createNewFile()) {
                System.out.println("File created: " + file.getName());
            } else {
                System.out.println("File already exists.");
            }
        } catch (IOException e) {
            System.out.println("An error occurred.");
        }
    }
}
```

---

### **2. Reading and Writing Files (`FileReader`, `FileWriter`)**

|Method|Description|
|---|---|
|`void write(String text)`|Writes text to a file.|
|`void flush()`|Forces any buffered output to be written to the file.|
|`int read()`|Reads a single character from a file (returns `-1` at the end).|
|`int read(char[] array)`|Reads multiple characters into an array.|
|`void close()`|Closes the file.|

**Example: Writing to a File**

```java
import java.io.FileWriter;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try {
            FileWriter writer = new FileWriter("test.txt");
            writer.write("Hello, Java File Handling!");
            writer.close();
            System.out.println("Successfully written to the file.");
        } catch (IOException e) {
            System.out.println("An error occurred.");
        }
    }
}
```



**Example: Reading from a File**

```java
import java.io.FileReader;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try {
            FileReader reader = new FileReader("test.txt");
            int character;
            while ((character = reader.read()) != -1) {
                System.out.print((char) character);
            }
            reader.close();
        } catch (IOException e) {
            System.out.println("An error occurred.");
        }
    }
}
```


---

### **3. Working with File Attributes (`File` Methods)**

|Method|Description|
|---|---|
|`boolean canRead()`|Checks if the file is readable.|
|`boolean canWrite()`|Checks if the file is writable.|
|`boolean isFile()`|Checks if it's a file (not a directory).|
|`boolean isDirectory()`|Checks if it's a directory.|
|`long length()`|Returns the file size in bytes.|

---

### **4. Using `Files` Class (`java.nio.file.Files`)**

|Method|Description|
|---|---|
|`Path createFile(Path path)`|Creates a new file.|
|`Path createDirectory(Path path)`|Creates a new directory.|
|`void write(Path path, byte[] bytes)`|Writes bytes to a file.|
|`byte[] readAllBytes(Path path)`|Reads all file contents as bytes.|
|`List<String> readAllLines(Path path)`|Reads all lines as a list of strings.|
|`boolean deleteIfExists(Path path)`|Deletes a file if it exists.|

**Example: Using `Files` to Read a File**

```java
import java.nio.file.*;
import java.io.IOException;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        try {
            List<String> lines = Files.readAllLines(Paths.get("test.txt"));
            for (String line : lines) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.out.println("Error reading file.");
        }
    }
}
```

---

### **Summary**

- **`File` class** → Creating, deleting, checking file properties.
    
- **`FileReader`, `FileWriter`** → Reading and writing character-based files.
    
- **`Files` class (`java.nio.file`)** → Modern file operations (better performance).
    


---

# **7. Random Class Methods (`java.util.Random`)**

The `Random` class in Java (`java.util.Random`) is used to generate **pseudo-random numbers** of different data types like integers, doubles, booleans, and more.

---

### **1. Creating a Random Object**

Before using `Random` class methods, create an instance:

```java
import java.util.Random;

Random random = new Random(); // Default constructor
Random randomWithSeed = new Random(42); // Creates with a fixed seed
```

If you use the same seed, you will always get the same random numbers.

---

### **2. Methods of `Random` Class**

|Method|Return Type|Description|
|---|---|---|
|`int nextInt()`|`int`|Returns a random integer (can be positive or negative).|
|`int nextInt(int bound)`|`int`|Returns a random integer between `0` (inclusive) and `bound` (exclusive).|
|`long nextLong()`|`long`|Returns a random long number.|
|`float nextFloat()`|`float`|Returns a random float between `0.0` and `1.0`.|
|`double nextDouble()`|`double`|Returns a random double between `0.0` and `1.0`.|
|`boolean nextBoolean()`|`boolean`|Returns a random boolean (`true` or `false`).|
|`void nextBytes(byte[] bytes)`|`void`|Fills the given byte array with random bytes.|
|`double nextGaussian()`|`double`|Returns a random number with **Gaussian distribution** (mean `0.0`, standard deviation `1.0`).|
|`void setSeed(long seed)`|`void`|Sets a new seed for reproducible random numbers.|

---

### **3. Example Usage of `Random` Methods**

```java
import java.util.Random;

public class Main {
    public static void main(String[] args) {
        Random random = new Random(); // Random object
		
        // Generating random numbers
        System.out.println("Random int: " + random.nextInt());
        System.out.println("Random int (0-99): " + random.nextInt(100)); // 0 to 99
        System.out.println("Random long: " + random.nextLong());
        System.out.println("Random float: " + random.nextFloat());
        System.out.println("Random double: " + random.nextDouble());
        System.out.println("Random boolean: " + random.nextBoolean());
		
        // Generating a random Gaussian number
        System.out.println("Random Gaussian: " + random.nextGaussian());
		
        // Generating random bytes
        byte[] bytes = new byte[5];
        random.nextBytes(bytes);
        System.out.print("Random bytes: ");
        for (byte b : bytes) {
            System.out.print(b + " ");
        }
    }
}
```

---

### **4. Generating Random Numbers in a Range**

To generate a number in a **custom range** (e.g., between `min` and `max`):

```java
int min = 50;
int max = 100;
int randomNumber = min + random.nextInt(max - min + 1);
System.out.println("Random number between " + min + " and " + max + ": " + randomNumber);
```

---

### **5. Using `ThreadLocalRandom` (Faster Alternative)**

Instead of `Random`, you can use `ThreadLocalRandom` (available in Java 7+) for better performance in multi-threaded applications:

```java
import java.util.concurrent.ThreadLocalRandom;

public class Main {
    public static void main(String[] args) {
        int randomInt = ThreadLocalRandom.current().nextInt(50, 101); // 50 to 100
        double randomDouble = ThreadLocalRandom.current().nextDouble(1.5, 5.0); // 1.5 to 5.0
        System.out.println("Random int: " + randomInt);
        System.out.println("Random double: " + randomDouble);
    }
}
```

---

### **6. Summary**

- Use `nextInt(bound)` to get numbers in a range `0` to `bound-1`.
    
- Use `nextFloat()` or `nextDouble()` for random decimal values.
    
- Use `nextBoolean()` for a random `true/false` value.
    
- Use `ThreadLocalRandom` for better performance in multi-threading.
    



---

## **Conclusion**

This covers **most important Java methods** from core classes. Let me know if you want explanations, examples, or deep dives into any of these topics!