# **Data Types, Variables, and Operators**

Today, we’ll cover the foundation of Java programming: **data types**, **variables**, and **operators**.

---

## **1. Data Types in Java**

Java has two types of data:

1. **Primitive Data Types**: Basic building blocks.
2. **Non-Primitive Data Types**: Classes, Arrays, and Interfaces (we'll cover these later).

### **Primitive Data Types**

| Data Type | Description                   | Example         | Size    | Default Value |
| --------- | ----------------------------- | --------------- | ------- | ------------- |
| `byte`    | Small integer                 | 8-bit           | 1 byte  | `0`           |
| `short`   | Small integer                 | 16-bit          | 2 bytes | `0`           |
| `int`     | Integer                       | 32-bit          | 4 bytes | `0`           |
| `long`    | Large integer                 | 64-bit          | 8 bytes | `0L`          |
| `float`   | Decimal (single precision)    | 32-bit          | 4 bytes | `0.0f`        |
| `double`  | Decimal (double precision)    | 64-bit          | 8 bytes | `0.0d`        |
| `char`    | Single character              | 'A', '9', etc.  | 2 bytes | `'\u0000'`    |
| `boolean` | Logical values (`true/false`) | `true`, `false` | 1 bit   | `false`       |

### Example:

```java
public class DataTypesDemo {
    public static void main(String[] args) {
        int age = 25;
        double salary = 50000.75;
        char grade = 'A';
        boolean isActive = true;

        System.out.println("Age: " + age);
        System.out.println("Salary: " + salary);
        System.out.println("Grade: " + grade);
        System.out.println("Is Active: " + isActive);
    }
}
```

---

## **2. Variables**

A **variable** is a container for storing data values.

### **Declaring Variables**

- Syntax: `dataType variableName = value;`

### Example:

```java
int num = 10;
String name = "John";
```

### **Types of Variables in Java**

1. **Local Variable**: Declared inside a method and accessible only within that method.
2. **Instance Variable**: Declared inside a class but outside methods. Each object has its copy.
3. **Static Variable**: Declared using `static` keyword; shared among all objects of a class.

---

## **3. Operators**

### **Types of Operators**

1. **Arithmetic Operators**: Perform mathematical operations.

   - `+`, `-`, `*`, `/`, `%`
   - Example:
     ```java
     int a = 10, b = 3;
     System.out.println("Sum: " + (a + b));
     System.out.println("Division: " + (a / b));
     System.out.println("Modulus: " + (a % b));
     ```

2. **Relational Operators**: Compare values.

   - `==`, `!=`, `>`, `<`, `>=`, `<=`
   - Example:
     ```java
     System.out.println(a > b);  // true
     System.out.println(a == b); // false
     ```

3. **Logical Operators**: Used for boolean logic.

   - `&&` (AND), `||` (OR), `!` (NOT)
   - Example:
     ```java
     boolean x = true, y = false;
     System.out.println(x && y);  // false
     System.out.println(x || y);  // true
     System.out.println(!x);      // false
     ```

4. **Assignment Operators**: Assign values to variables.
   - `=`, `+=`, `-=`, `*=`, `/=`
   - Example:
     ```java
     int num = 5;
     num += 3;  // num = num + 3
     System.out.println(num); // 8
     ```

---

## **4. Example Program**

```java
public class OperatorsDemo {
    public static void main(String[] args) {
        int num1 = 10, num2 = 5;

        // Arithmetic Operators
        System.out.println("Sum: " + (num1 + num2));
        System.out.println("Difference: " + (num1 - num2));

        // Relational Operators
        System.out.println("Is num1 greater? " + (num1 > num2));

        // Logical Operators
        boolean isAdult = true, isStudent = false;
        System.out.println("Is adult and student? " + (isAdult && isStudent));
    }
}
```

---

## **Hands-On Exercises**

1. Write a program to calculate the area of a rectangle.  
   Formula: `area = length * breadth`

2. Write a program to check if a number is **positive**, **negative**, or **zero**.

3. Write a program that takes two numbers and swaps their values without using a third variable.

---
