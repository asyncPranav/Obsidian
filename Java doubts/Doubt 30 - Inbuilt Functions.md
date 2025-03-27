

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


|Method|Description|
|---|---|
|`Math.abs(int x)`|Returns the absolute value of `x`.|
|`Math.max(int a, int b)`|Returns the maximum of `a` and `b`.|
|`Math.min(int a, int b)`|Returns the minimum of `a` and `b`.|
|`Math.pow(double a, double b)`|Returns `a` raised to the power `b`.|
|`Math.sqrt(double x)`|Returns the square root of `x`.|
|`Math.cbrt(double x)`|Returns the cube root of `x`.|
|`Math.round(double x)`|Rounds `x` to the nearest integer.|
|`Math.floor(double x)`|Rounds `x` downward.|
|`Math.ceil(double x)`|Rounds `x` upward.|
|`Math.random()`|Returns a random number between `0.0` and `1.0`.|
|`Math.log(double x)`|Returns the natural logarithm (`ln(x)`).|
|`Math.log10(double x)`|Returns base-10 logarithm (`log₁₀(x)`).|
|`Math.sin(double x)`, `Math.cos(double x)`, `Math.tan(double x)`|Trigonometric functions.|



---

# **4. Arrays Class Methods (`java.util.Arrays`)**


| Method                                             | Description                                                                              |
| -------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| `int binarySearch(T[] array, T key)`               | Searches for `key` using **binary search** and returns the index (must be sorted).       |
| `int[] copyOf(int[] array, int newLength)`         | Creates a **new array** with the specified length and copies elements from the original. |
| `int[] copyOfRange(int[] array, int from, int to)` | Copies a **range of elements** into a new array.                                         |
| `boolean equals(int[] array1, int[] array2)`       | Checks if two arrays are **equal**.                                                      |
| `void fill(int[] array, int value)`                | Fills the array with a specific value.                                                   |
| `void sort(int[] array)`                           | Sorts the array in **ascending order**.                                                  |
| `void parallelSort(int[] array)`                   | Faster sorting using **parallel processing** (Java 8+).                                  |
| `String toString(int[] array)`                     | Converts an array into a **string representation**.                                      |
| `List<T> asList(T... array)`                       | Converts an array into a **fixed-size list**.                                            |
| `int hashCode(int[] array)`                        | Returns a **hash code** for the array.                                                   |
| `Stream<T> stream(T[] array)`                      | Converts an array into a **Stream** (Java 8+).                                           |

#### **Example Usage**

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] numbers = {5, 3, 8, 1, 9};
		
        // Sorting
        Arrays.sort(numbers);
        System.out.println(Arrays.toString(numbers)); // [1, 3, 5, 8, 9]
		
        // Binary Search
        int index = Arrays.binarySearch(numbers, 5);
        System.out.println("Index of 5: " + index); // Index of 5: 2
		
        // Copying Array
        int[] copy = Arrays.copyOf(numbers, 3);
        System.out.println(Arrays.toString(copy)); // [1, 3, 5]
		
        // Filling an array
        int[] filledArray = new int[5];
        Arrays.fill(filledArray, 7);
        System.out.println(Arrays.toString(filledArray)); // [7, 7, 7, 7, 7]
    }
}
```


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