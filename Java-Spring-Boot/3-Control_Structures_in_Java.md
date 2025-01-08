# **Control Structures in Java**

Today, we’ll learn how to control the flow of execution in a Java program using **conditional statements** and **loops**.

---

## **1. Conditional Statements**

Conditional statements allow your program to make decisions and execute different code based on certain conditions.

### **Types of Conditional Statements**:

1. **`if` Statement**  
   Executes a block of code if the condition is true.

   ```java
   if (condition) {
       // Code to execute
   }
   ```

   **Example:**

   ```java
   int age = 18;
   if (age >= 18) {
       System.out.println("You are eligible to vote.");
   }
   ```

2. **`if-else` Statement**  
   Executes one block of code if the condition is true and another if it’s false.

   ```java
   if (condition) {
       // Code if condition is true
   } else {
       // Code if condition is false
   }
   ```

   **Example:**

   ```java
   int number = -5;
   if (number > 0) {
       System.out.println("Positive number");
   } else {
       System.out.println("Negative number");
   }
   ```

3. **`if-else if` Ladder**  
   Use multiple conditions.

   ```java
   if (condition1) {
       // Code for condition1
   } else if (condition2) {
       // Code for condition2
   } else {
       // Code if all conditions are false
   }
   ```

   **Example:**

   ```java
   int marks = 85;
   if (marks >= 90) {
       System.out.println("Grade: A+");
   } else if (marks >= 75) {
       System.out.println("Grade: A");
   } else {
       System.out.println("Grade: B");
   }
   ```

4. **`switch` Statement**  
   A better choice for multiple conditions based on a single variable.

   ```java
   switch (variable) {
       case value1:
           // Code for value1
           break;
       case value2:
           // Code for value2
           break;
       default:
           // Code if no match
   }
   ```

   **Example:**

   ```java
   int day = 3;
   switch (day) {
       case 1:
           System.out.println("Monday");
           break;
       case 2:
           System.out.println("Tuesday");
           break;
       case 3:
           System.out.println("Wednesday");
           break;
       default:
           System.out.println("Invalid day");
   }
   ```

---

## **2. Loops in Java**

Loops are used to execute a block of code repeatedly.

### **Types of Loops**:

1. **`for` Loop**  
   Executes a block of code for a fixed number of times.

   ```java
   for (initialization; condition; update) {
       // Code to execute
   }
   ```

   **Example:**

   ```java
   for (int i = 1; i <= 5; i++) {
       System.out.println("Count: " + i);
   }
   ```

2. **`while` Loop**  
   Executes a block of code as long as the condition is true.

   ```java
   while (condition) {
       // Code to execute
   }
   ```

   **Example:**

   ```java
   int count = 1;
   while (count <= 5) {
       System.out.println("Count: " + count);
       count++;
   }
   ```

3. **`do-while` Loop**  
   Executes the code block once, then repeats as long as the condition is true.

   ```java
   do {
       // Code to execute
   } while (condition);
   ```

   **Example:**

   ```java
   int count = 1;
   do {
       System.out.println("Count: " + count);
       count++;
   } while (count <= 5);
   ```

---

## **3. Example Program: Combining Conditionals and Loops**

```java
public class ControlStructuresDemo {
    public static void main(String[] args) {
        // Using if-else
        int number = 10;
        if (number % 2 == 0) {
            System.out.println(number + " is even.");
        } else {
            System.out.println(number + " is odd.");
        }

        // Using a for loop
        System.out.println("First 5 natural numbers:");
        for (int i = 1; i <= 5; i++) {
            System.out.println(i);
        }

        // Using a switch statement
        char grade = 'A';
        switch (grade) {
            case 'A':
                System.out.println("Excellent!");
                break;
            case 'B':
                System.out.println("Good job!");
                break;
            default:
                System.out.println("Keep trying!");
        }
    }
}
```

---

## **Hands-On Exercises**

1. **Odd or Even Check:**  
   Write a program that takes an integer and checks if it is odd or even.

2. **Print Multiplication Table:**  
   Write a program that prints the multiplication table of a number using a loop.

3. **Number Guessing Game:**  
   Create a program where the user has to guess a predefined number using conditional statements and loops.

4. **Sum of Numbers in a Range:**  
   Write a program to calculate the sum of all numbers between 1 and 100 using a loop.

---
