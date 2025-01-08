### **Day 9: Exception Handling in Java**

Today, we’ll learn about **exception handling**, which helps manage errors gracefully in Java programs. This is essential for writing robust and fault-tolerant applications.

---

### **1. What is an Exception?**

An **exception** is an event that disrupts the normal flow of a program. It occurs during runtime due to errors like invalid user input, file not found, or dividing by zero.

#### **Types of Exceptions**

1. **Checked Exceptions**:

   - Checked at compile-time.
   - Must be handled using `try-catch` or declared with `throws`.
   - Examples: `IOException`, `SQLException`.

2. **Unchecked Exceptions**:

   - Occur at runtime.
   - Usually caused by programming errors.
   - Examples: `NullPointerException`, `ArrayIndexOutOfBoundsException`.

3. **Errors**:
   - Represent critical issues that the application cannot recover from.
   - Examples: `OutOfMemoryError`, `StackOverflowError`.

---

### **2. Exception Handling Mechanism**

Java provides four key constructs to handle exceptions:

1. **`try`**: Code that might throw an exception.
2. **`catch`**: Handles the exception.
3. **`finally`**: Executes code regardless of exception occurrence.
4. **`throw`**: Used to explicitly throw an exception.

---

### **3. Using `try-catch`**

**Basic Syntax**:

```java
try {
    // Code that may throw an exception
} catch (ExceptionType e) {
    // Handle exception
}
```

#### **Example: Handling Division by Zero**

```java
public class Main {
    public static void main(String[] args) {
        try {
            int result = 10 / 0; // Throws ArithmeticException
            System.out.println(result);
        } catch (ArithmeticException e) {
            System.out.println("Error: Division by zero is not allowed.");
        }
    }
}
```

**Output**:

```
Error: Division by zero is not allowed.
```

---

### **4. The `finally` Block**

The `finally` block always executes, even if an exception is not thrown. Commonly used to release resources like closing files or database connections.

#### **Example: Using `finally`**

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = null;

        try {
            scanner = new Scanner(new File("nonexistent.txt")); // May throw FileNotFoundException
        } catch (FileNotFoundException e) {
            System.out.println("File not found.");
        } finally {
            if (scanner != null) {
                scanner.close();
                System.out.println("Scanner closed.");
            }
        }
    }
}
```

**Output**:

```
File not found.
Scanner closed.
```

---

### **5. Throwing Exceptions**

You can explicitly throw exceptions using the `throw` keyword.

#### **Example: Throwing an Exception**

```java
public class Main {
    static void validateAge(int age) {
        if (age < 18) {
            throw new IllegalArgumentException("Age must be 18 or above.");
        }
    }

    public static void main(String[] args) {
        validateAge(16); // Throws IllegalArgumentException
    }
}
```

---

### **6. Declaring Exceptions with `throws`**

The `throws` keyword is used to declare exceptions that a method might throw. This is mandatory for checked exceptions.

#### **Example: Declaring Exceptions**

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class Main {
    static void readFile() throws FileNotFoundException {
        Scanner scanner = new Scanner(new File("example.txt")); // May throw FileNotFoundException
    }

    public static void main(String[] args) {
        try {
            readFile();
        } catch (FileNotFoundException e) {
            System.out.println("File not found.");
        }
    }
}
```

---

### **7. Custom Exceptions**

You can create your own exception class by extending the `Exception` class (for checked exceptions) or `RuntimeException` class (for unchecked exceptions).

#### **Example: Custom Exception**

```java
class InvalidAgeException extends Exception {
    public InvalidAgeException(String message) {
        super(message);
    }
}

public class Main {
    static void validateAge(int age) throws InvalidAgeException {
        if (age < 18) {
            throw new InvalidAgeException("Age must be 18 or above.");
        }
    }

    public static void main(String[] args) {
        try {
            validateAge(16);
        } catch (InvalidAgeException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

---

### **8. Common Pitfalls and Best Practices**

#### **Pitfalls**:

1. **Catching Generic Exceptions**:  
   Avoid catching `Exception` unless necessary; catch specific exceptions instead.

   ```java
   catch (Exception e) { ... } // Not recommended
   ```

2. **Swallowing Exceptions**:  
   Don’t catch exceptions and do nothing with them.
   ```java
   try { ... } catch (Exception e) { /* Empty */ } // Bad Practice
   ```

#### **Best Practices**:

1. Use `finally` for resource cleanup.
2. Handle exceptions at appropriate levels.
3. Avoid using exceptions for flow control.
4. Use custom exceptions to add clarity.

---

### **9. Hands-On Exercises**

1. **Basic Exception Handling**:  
   Write a program that accepts two numbers from the user and divides them. Handle the case where the user enters zero for the divisor.

2. **Custom Exception**:  
   Create a `BankAccount` class with a method `withdraw(amount)` that throws a custom exception if the balance is insufficient.

3. **Checked Exception**:  
   Write a method `readFile(filename)` that reads a file and throws a `FileNotFoundException`. Handle the exception in the main method.

4. **Finally Block**:  
   Write a program to open and close a database connection using the `finally` block to ensure it always closes, even in case of an exception.

---
