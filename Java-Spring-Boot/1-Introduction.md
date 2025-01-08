# **Introduction to Java and Basic Syntax**

## **What is Java?**

Java is a high-level, object-oriented programming language developed in 1995 by James Gosling at Sun Microsystems. It is one of the most popular languages for building secure, robust, and platform-independent applications.

## Key Features:

1. **Platform Independence**: Java's "Write Once, Run Anywhere" philosophy allows programs to run on any device with a Java Virtual Machine (JVM).
2. **Object-Oriented**: Focuses on objects and classes, making code reusable and easier to maintain.
3. **Secure**: Has built-in security features, including runtime checks.
4. **Multithreaded**: Supports concurrent execution of code for better performance.

---

## **History of Java**

- **1991**: Initially named "Oak" as part of a project to develop a language for embedded systems.
- **1995**: Renamed "Java" after Java coffee and released for the internet as a platform-independent language.
- **Present**: Widely used for web development, mobile apps (Android), and enterprise-level systems.

---

## **How Java Works**

1. **Write**: You write source code in `.java` files.
2. **Compile**: The `javac` compiler converts `.java` files into `.class` files (bytecode).
3. **Execute**: The JVM reads and executes bytecode on the host machine.

## Diagram:

```
Source Code (.java) -> Compiler (javac) -> Bytecode (.class) -> JVM -> Machine Code
```

---

## **Key Terminologies**

- **JDK (Java Development Kit)**: Contains tools like `javac` and `java` for development.
- **JRE (Java Runtime Environment)**: Provides libraries and JVM for running Java applications.
- **JVM (Java Virtual Machine)**: Executes bytecode on your machine.

---

## **Set Up Your Development Environment**

1. **Download JDK**: [Oracle](https://www.oracle.com/java/technologies/javase-downloads.html) or [OpenJDK](https://openjdk.org/).
2. **Install an IDE**: Recommended - IntelliJ IDEA, Eclipse, or Visual Studio Code.
3. **Verify Installation**:
   Open a terminal or command prompt and type:
   ```
   java -version
   javac -version
   ```

---

## **Your First Java Program**

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

## Steps:

1. Save the file as `HelloWorld.java`.
2. Open the terminal and navigate to the folder containing the file.
3. Compile: `javac HelloWorld.java`
4. Run: `java HelloWorld`

---

## **Hands-On Exercises**

1. **Print Your Name and Age**:
   Write a program that prints your name and age in two separate lines.

2. **Sum of Two Numbers**:
   Write a program that accepts two numbers as variables and prints their sum.

---
