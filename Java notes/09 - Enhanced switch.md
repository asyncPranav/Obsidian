
---

# 🚦 **Enhanced `switch` in Java**


---


## 📅 **Introduced In:**

- **Preview:** Java 12 and Java 13
- **Standardized:** Java 14

## 🎯 **Why Enhanced `switch`?**

The enhanced `switch` provides:

- **Conciseness:** Reduced boilerplate code.
- **Safety:** Eliminates accidental fall-through.
- **Readability:** Simplifies syntax for simple mappings.
- **Flexibility:** Allows expressions and multi-label case statements.

---

## 🆚 **Traditional vs. Enhanced `switch` Syntax**

### 🛠️ **Traditional Syntax:**

```java
switch (day) {
    case 1:
        System.out.println("Monday");
        break;
    case 2:
        System.out.println("Tuesday");
        break;
    default:
        System.out.println("Invalid day");
}
```

### 🚀 **Enhanced Syntax:**

```java
switch (day) {
    case 1 -> System.out.println("Monday");
    case 2 -> System.out.println("Tuesday");
    default -> System.out.println("Invalid day");
}
```

### 💡 **Key Differences:**

|Feature|Traditional `switch`|Enhanced `switch`|
|---|---|---|
|**Syntax**|`case: ... break;`|`case -> ...`|
|**Fall-Through**|Possible, requires `break`|Prevented by design|
|**Multiple Statements**|Use `{}` with `break`|Use `{}` directly|
|**Return as Expression**|Not allowed in older versions|Allows `switch` as an expression|
|**Support for Patterns**|No|Yes (with pattern matching, Java 17+)|

---

## 📝 **1. Enhanced `switch` as a Statement**

### 🔍 **Example:**

```java
int day = 3;

switch (day) {
    case 1 -> System.out.println("Monday");
    case 2 -> System.out.println("Tuesday");
    case 3 -> {
        System.out.println("Wednesday");
        System.out.println("Midweek day");
    }
    default -> System.out.println("Invalid day");
}
```

### 💡 **Explanation:**

- **Single Statement:** No need for `{}` or `break`.
- **Multiple Statements:** Use `{}` to group them.
- **No Fall-Through:** Each case is independent.

---

## 🎲 **2. Enhanced `switch` as an Expression**

### 🔍 **Example:**

```java
String dayName = switch (day) {
    case 1 -> "Monday";
    case 2 -> "Tuesday";
    case 3, 4, 5 -> "Midweek";
    case 6, 7 -> "Weekend";
    default -> "Invalid day";
};

System.out.println(dayName);
```

### 💡 **Key Points:**

- The `switch` can directly return a value.
- Supports **multi-label case statements** (`case 3, 4, 5 ->`).
- **`default` is mandatory** when used as an expression to ensure all cases are covered.

---

## 🔄 **3. Yielding Values in `switch` Expressions**

### 🔍 **Example:**

```java
int number = 2;
String result = switch (number) {
    case 1 -> "One";
    case 2 -> {
        String temp = "Two";
        yield temp; // Return value using 'yield'
    }
    default -> "Unknown";
};
System.out.println(result);
```

### 💡 **`yield` Keyword:**

- **Purpose:** Return a value from a block in `switch` expressions.
- **When to Use:** When using a block `{}` with multiple statements.

---

## 🎯 **4. Multi-Label Case Statements**

### 🔍 **Example:**

```java
String typeOfDay = switch (day) {
    case 1, 2, 3, 4, 5 -> "Weekday";
    case 6, 7 -> "Weekend";
    default -> "Invalid day";
};
```

### 💡 **When to Use:**

- Grouping cases with the same outcome.
- Avoids repeated code and improves clarity.

---

## 🧠 **5. Using `switch` with Enums**

```java
enum Day { MON, TUE, WED, THU, FRI, SAT, SUN }

Day day = Day.MON;
String result = switch (day) {
    case MON, TUE, WED, THU, FRI -> "Weekday";
    case SAT, SUN -> "Weekend";
};
System.out.println(result);
```

### 💡 **Advantages:**

- **Type Safety:** Enums prevent invalid values.
- **Readability:** Self-documenting code with meaningful enum names.

---

## 🔍 **6. Pattern Matching in `switch` (Java 17+)**

```java
Object obj = "Hello";

String result = switch (obj) {
    case String s -> "It's a string: " + s;
    case Integer i -> "It's an integer: " + i;
    default -> "Unknown type";
};
System.out.println(result);
```

### 💡 **Pattern Matching:**

- **Instance Check and Cast in One Step:** `String s` both checks and casts `obj` to `String`.
- **Reduces the Need for `instanceof` and Casting:** Cleaner and safer code.

---

## 🧪 **7. Common Pitfalls to Avoid**

|❌ **Mistake**|✅ **Solution**|
|---|---|
|Missing `default` in expressions|Always provide a `default` case|
|Using `break` with `->`|Not needed, avoid it|
|Not using `yield` in blocks|Use `yield` to return values|
|Using traditional `case:`|Avoid mixing old and new styles|

---

## 🚀 **When to Use Enhanced `switch`:**

- When you need **concise and readable code**.
- When **fall-through behavior is not needed**.
- When using **switch as an expression**.
- When dealing with **pattern matching** (Java 17+).

---

## 💡 **Summary:**

- Enhanced `switch` offers **simplicity, safety**, and **powerful new features**.
- Supports **expressions, pattern matching**, and **multi-labels**.
- Improves code **readability** and **maintainability**.

**NEXT**
