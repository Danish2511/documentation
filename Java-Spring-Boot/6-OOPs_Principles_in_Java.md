### **Day 6: Object-Oriented Programming (OOP) Principles in Java**

Today, we’ll explore the core principles of OOP: **Inheritance**, **Polymorphism**, **Abstraction**, and **Encapsulation**, continuing from where we left off. We’ll implement examples and practice each concept.

---

### **1. Inheritance**

Inheritance allows a class to acquire properties and behaviors (methods) of another class.

- The **parent class** (or superclass) provides the attributes and methods.
- The **child class** (or subclass) inherits from the parent class.

#### **How to Implement Inheritance**

- Use the `extends` keyword.

**Example:**

```java
// Parent class
class Animal {
    String name;

    void eat() {
        System.out.println(name + " is eating.");
    }
}

// Child class
class Dog extends Animal {
    void bark() {
        System.out.println(name + " is barking.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.name = "Buddy"; // Access parent attribute
        dog.eat();          // Access parent method
        dog.bark();         // Access child method
    }
}
```

**Output**:

```
Buddy is eating.
Buddy is barking.
```

---

### **2. Polymorphism**

Polymorphism allows one method to perform different behaviors based on the context.  
There are two types:

1. **Compile-Time Polymorphism** (Method Overloading).
2. **Run-Time Polymorphism** (Method Overriding).

#### **Method Overloading (Compile-Time Polymorphism)**

Multiple methods with the same name but different parameter lists.  
**Example**:

```java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        System.out.println("Sum of 2 numbers: " + calc.add(10, 20));
        System.out.println("Sum of 3 numbers: " + calc.add(10, 20, 30));
    }
}
```

**Output**:

```
Sum of 2 numbers: 30
Sum of 3 numbers: 60
```

#### **Method Overriding (Run-Time Polymorphism)**

A child class provides a specific implementation for a method in the parent class.  
**Example**:

```java
class Animal {
    void sound() {
        System.out.println("Some generic animal sound.");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Woof! Woof!");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Dog(); // Upcasting
        animal.sound();            // Calls the overridden method
    }
}
```

**Output**:

```
Woof! Woof!
```

---

### **3. Abstraction**

**Abstraction** hides the implementation details of a class and shows only the essential features.  
Achieved using:

1. **Abstract Classes**.
2. **Interfaces**.

#### **Abstract Classes**

- Declared using the `abstract` keyword.
- Cannot be instantiated.
- May contain abstract methods (methods without a body) and concrete methods.

**Example**:

```java
abstract class Shape {
    abstract void draw(); // Abstract method

    void display() {
        System.out.println("This is a shape.");
    }
}

class Circle extends Shape {
    @Override
    void draw() {
        System.out.println("Drawing a Circle.");
    }
}

public class Main {
    public static void main(String[] args) {
        Shape shape = new Circle();
        shape.display();
        shape.draw();
    }
}
```

**Output**:

```
This is a shape.
Drawing a Circle.
```

#### **Interfaces**

- Declared using the `interface` keyword.
- All methods are public and abstract by default.

**Example**:

```java
interface Vehicle {
    void start();
}

class Car implements Vehicle {
    @Override
    public void start() {
        System.out.println("Car is starting.");
    }
}

public class Main {
    public static void main(String[] args) {
        Vehicle car = new Car();
        car.start();
    }
}
```

**Output**:

```
Car is starting.
```

---

### **4. Encapsulation** (Recap)

Encapsulation was covered in **Day 5**, but it’s one of the four pillars of OOP. It restricts direct access to an object's data using private fields and public getters/setters.

---

### **5. Hands-On Exercises**

1. **Inheritance:**  
   Create a `Person` class with attributes `name` and `age`. Create a `Student` subclass with an additional attribute `grade`. Add methods to display details of both classes.

2. **Polymorphism:**  
   Implement a class hierarchy where a parent class `Shape` has a `calculateArea()` method. Override this method in subclasses like `Rectangle` and `Circle` to calculate areas for specific shapes.

3. **Abstraction:**  
   Create an abstract class `Animal` with abstract methods like `sound()` and concrete methods like `eat()`. Implement the abstract methods in child classes like `Dog` and `Cat`.

4. **Interface Practice:**  
   Define an interface `Playable` with a method `play()`. Create classes `Guitar` and `Piano` that implement the interface and provide their specific implementations.

---
