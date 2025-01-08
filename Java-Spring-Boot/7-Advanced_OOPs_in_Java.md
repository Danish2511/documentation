### **Day 7: Advanced Object-Oriented Programming in Java**

Today, we’ll expand our knowledge of OOP by learning **composition**, **static keyword usage**, **final keyword**, and **nested classes**. These concepts strengthen your understanding of Java’s object-oriented nature.

---

### **1. Composition**

**Composition** is a design principle where an object is composed of other objects. It models a "has-a" relationship. For example, a `Car` "has-a" `Engine`.

#### **Example: Composition**

```java
class Engine {
    void start() {
        System.out.println("Engine starts...");
    }
}

class Car {
    private Engine engine; // Composition relationship

    // Constructor
    Car(Engine engine) {
        this.engine = engine;
    }

    void drive() {
        engine.start(); // Using Engine's functionality
        System.out.println("Car is driving...");
    }
}

public class Main {
    public static void main(String[] args) {
        Engine engine = new Engine();
        Car car = new Car(engine);
        car.drive();
    }
}
```

**Output**:

```
Engine starts...
Car is driving...
```

---

### **2. The `static` Keyword**

The `static` keyword in Java indicates that a member (field or method) belongs to the class rather than any specific object.

#### **Static Fields and Methods**

- **Static Field**: Shared across all instances of a class.
- **Static Method**: Can be called without creating an object.

**Example**:

```java
class Calculator {
    static int add(int a, int b) {
        return a + b;
    }
}

public class Main {
    public static void main(String[] args) {
        // Call static method directly
        System.out.println("Sum: " + Calculator.add(10, 20));
    }
}
```

**Output**:

```
Sum: 30
```

#### **Static Block**

Executed when the class is loaded.

```java
class StaticDemo {
    static {
        System.out.println("Static block executed.");
    }

    static void greet() {
        System.out.println("Hello from static method!");
    }
}

public class Main {
    public static void main(String[] args) {
        StaticDemo.greet(); // Triggers static block (if not already triggered)
    }
}
```

---

### **3. The `final` Keyword**

The `final` keyword is used to create constants or prevent inheritance and method overriding.

1. **Final Variable**: Its value cannot be changed once assigned.

   ```java
   final int MAX_VALUE = 100;
   ```

2. **Final Method**: Cannot be overridden by a subclass.

   ```java
   class Parent {
       final void show() {
           System.out.println("Final method.");
       }
   }
   ```

3. **Final Class**: Cannot be extended.
   ```java
   final class Vehicle {}
   ```

---

### **4. Nested Classes**

A class defined inside another class is a **nested class**. Java supports three types:

1. **Static Nested Class**.
2. **Inner Class** (Non-static nested class).
3. **Anonymous Inner Class**.

#### **Static Nested Class**

Accessed without creating an instance of the outer class.

```java
class Outer {
    static class Nested {
        void display() {
            System.out.println("Static nested class.");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Outer.Nested nested = new Outer.Nested();
        nested.display();
    }
}
```

#### **Inner Class**

Needs an instance of the outer class to be created.

```java
class Outer {
    class Inner {
        void display() {
            System.out.println("Inner class.");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner();
        inner.display();
    }
}
```

#### **Anonymous Inner Class**

Created for one-time use and usually implements an interface or extends a class.

```java
interface Greeting {
    void sayHello();
}

public class Main {
    public static void main(String[] args) {
        Greeting greeting = new Greeting() { // Anonymous class
            @Override
            public void sayHello() {
                System.out.println("Hello from anonymous class!");
            }
        };
        greeting.sayHello();
    }
}
```

---

### **5. Hands-On Exercises**

1. **Composition**:  
   Create a `Library` class that "has-a" list of `Books`. Implement methods to add and display books.

2. **Static Keyword**:  
   Create a `Counter` class with a static field to count the number of objects created.

3. **Final Keyword**:  
   Implement a `Shape` class with a `final` method `getName()` and a subclass `Circle`. Try overriding the method to observe the error.

4. **Nested Classes**:
   - Create a `Computer` class with a `static` nested class `Processor` and an inner class `RAM`.
   - Write a program to access both nested classes.

---
