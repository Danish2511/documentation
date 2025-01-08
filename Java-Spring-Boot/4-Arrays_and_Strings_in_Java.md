### **Day 4: Arrays and Strings in Java**

Today, we’ll explore **arrays** and **strings**, essential concepts for handling collections of data and text manipulation in Java.

---

### **1. Arrays**

An **array** is a collection of elements of the same data type, stored in contiguous memory locations.

#### **Declaring and Initializing Arrays**

1. **Declaration**:

   ```java
   dataType[] arrayName;
   ```

   Example:

   ```java
   int[] numbers;
   ```

2. **Instantiation** (Allocating Memory):

   ```java
   arrayName = new dataType[size];
   ```

   Example:

   ```java
   numbers = new int[5];
   ```

3. **Initialization**:  
   You can initialize during declaration or later.  
   Example:
   ```java
   int[] numbers = {10, 20, 30, 40, 50};
   ```

#### **Accessing Array Elements**

- Use indices (starting from `0`):
  ```java
  System.out.println(numbers[0]); // Outputs 10
  ```

#### **Example: Working with Arrays**

```java
public class ArrayDemo {
    public static void main(String[] args) {
        int[] numbers = {10, 20, 30, 40, 50};

        // Access elements
        System.out.println("First Element: " + numbers[0]);

        // Loop through the array
        for (int i = 0; i < numbers.length; i++) {
            System.out.println("Element at index " + i + ": " + numbers[i]);
        }

        // Enhanced for loop
        for (int num : numbers) {
            System.out.println("Element: " + num);
        }
    }
}
```

#### **Multidimensional Arrays**

- Declaring and Initializing:
  ```java
  int[][] matrix = {
      {1, 2, 3},
      {4, 5, 6},
      {7, 8, 9}
  };
  ```
- Accessing elements:
  ```java
  System.out.println(matrix[0][1]); // Outputs 2
  ```

#### **Hands-On Exercises**

1. Write a program to find the largest number in an array.
2. Write a program to calculate the sum of all elements in an array.
3. Implement a matrix addition program using 2D arrays.

---

### **2. Strings in Java**

A **string** is a sequence of characters. In Java, strings are objects of the `String` class.

#### **Creating Strings**

1. Using string literals:

   ```java
   String name = "John";
   ```

2. Using the `new` keyword:
   ```java
   String name = new String("John");
   ```

#### **Common String Methods**

| Method                          | Description                                  |
| ------------------------------- | -------------------------------------------- |
| `length()`                      | Returns the length of the string             |
| `charAt(index)`                 | Returns the character at the specified index |
| `substring(start, end)`         | Extracts a substring                         |
| `toLowerCase()`/`toUpperCase()` | Converts to lowercase/uppercase              |
| `equals()`/`equalsIgnoreCase()` | Compares strings for equality                |
| `indexOf(char)`                 | Finds the index of a character               |
| `replace(old, new)`             | Replaces characters or substrings            |
| `trim()`                        | Removes leading and trailing spaces          |

#### **Example: String Operations**

```java
public class StringDemo {
    public static void main(String[] args) {
        String greeting = "   Hello, World!   ";

        // Basic operations
        System.out.println("Original String: " + greeting);
        System.out.println("Length: " + greeting.length());
        System.out.println("Trimmed: " + greeting.trim());
        System.out.println("Uppercase: " + greeting.toUpperCase());
        System.out.println("Substring: " + greeting.substring(7, 12));

        // Character operations
        System.out.println("Character at index 1: " + greeting.charAt(1));

        // Comparison
        String anotherGreeting = "hello, world!";
        System.out.println("Equals: " + greeting.trim().equals(anotherGreeting));
        System.out.println("Equals Ignore Case: " + greeting.trim().equalsIgnoreCase(anotherGreeting));
    }
}
```

---

### **3. StringBuilder and StringBuffer**

- **StringBuilder**: Used for mutable strings. Faster and preferred for single-threaded applications.
- **StringBuffer**: Similar to `StringBuilder` but thread-safe (synchronized).

#### Example with StringBuilder:

```java
public class StringBuilderDemo {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder("Hello");

        sb.append(", World!");
        System.out.println("Appended: " + sb);

        sb.insert(5, " Java");
        System.out.println("Inserted: " + sb);

        sb.replace(6, 10, "Beautiful");
        System.out.println("Replaced: " + sb);

        sb.reverse();
        System.out.println("Reversed: " + sb);
    }
}
```

---

### **Hands-On Exercises**

1. Write a program to count the number of vowels in a string.
2. Reverse a string without using the built-in `reverse()` method.
3. Check if a given string is a palindrome (reads the same forward and backward).
4. Use `StringBuilder` to create a string and perform operations like `append`, `insert`, and `reverse`.

---

Let me know how you’re progressing, and feel free to ask for clarifications or help with the exercises! 😊
