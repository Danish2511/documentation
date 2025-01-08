### **Day 5: Object-Oriented Programming (OOP) Basics in Java**

Today, we’ll dive into the core principles of **Object-Oriented Programming (OOP)**, which is the foundation of Java. We’ll cover **classes**, **objects**, **methods**, and OOP principles like **encapsulation**.

---

### **1. What is OOP?**

**Object-Oriented Programming (OOP)** organizes code into objects that encapsulate data and behavior.

#### Key Concepts:

1. **Class**: A blueprint for creating objects.
2. **Object**: An instance of a class.
3. **Attributes**: Data stored in an object (variables inside a class).
4. **Methods**: Actions or behaviors an object can perform (functions inside a class).

---

### **2. Classes and Objects**

#### **Creating a Class**

A class contains attributes (fields) and methods.

```java
class Car {
    // Attributes
    String brand;
    String color;
    int speed;

    // Method
    void drive() {
        System.out.println(brand + " is driving at " + speed + " km/h.");
    }
}
```

#### **Creating Objects**

An object is created using the `new` keyword.

```java
public class Main {
    public static void main(String[] args) {
        Car car1 = new Car(); // Create object

        // Assign values to attributes
        car1.brand = "Toyota";
        car1.color = "Red";
        car1.speed = 120;

        // Call method
        car1.drive();
    }
}
```

**Output**:

```
Toyota is driving at 120 km/h.
```

---

### **3. Encapsulation**

**Encapsulation** is the practice of bundling data (attributes) and methods (behaviors) inside a class and restricting direct access to the attributes.

#### **Why Encapsulation?**

- Improves code security.
- Provides controlled access to data using **getters** and **setters**.

#### Example with Getters and Setters:

```java
class Student {
    private String name;
    private int age;

    // Getter for name
    public String getName() {
        return name;
    }

    // Setter for name
    public void setName(String name) {
        this.name = name;
    }

    // Getter for age
    public int getAge() {
        return age;
    }

    // Setter for age
    public void setAge(int age) {
        if (age > 0) {
            this.age = age;
        } else {
            System.out.println("Age must be positive.");
        }
    }
}
```

#### Using the Encapsulated Class:

```java
public class Main {
    public static void main(String[] args) {
        Student student = new Student();
        student.setName("Alice");
        student.setAge(20);

        System.out.println("Name: " + student.getName());
        System.out.println("Age: " + student.getAge());
    }
}
```

**Output**:

```
Name: Alice
Age: 20
```

---

### **4. Constructors**

A **constructor** is a special method used to initialize objects. It has the same name as the class and no return type.

#### Example of Constructor:

```java
class Person {
    String name;
    int age;

    // Constructor
    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    void display() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

public class Main {
    public static void main(String[] args) {
        Person person1 = new Person("John", 25); // Constructor called
        person1.display();
    }
}
```

**Output**:

```
Name: John, Age: 25
```

---

### **5. OOP Hands-On Exercises**

1. **Create a Class for Bank Accounts**

   - Attributes: `accountNumber`, `accountHolderName`, `balance`.
   - Methods: `deposit(amount)`, `withdraw(amount)`.
   - Include encapsulation with getters and setters.

2. **Model a Class for a Library Book**

   - Attributes: `title`, `author`, `isIssued`.
   - Methods: `issueBook()`, `returnBook()`.

3. **Design a Class for Employees**
   - Attributes: `name`, `designation`, `salary`.
   - Methods: Calculate annual salary and display employee details.

---

Let me know how you’re doing with these exercises or if you’d like me to explain anything further! 😊
