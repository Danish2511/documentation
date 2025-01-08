### **Day 17: Java Generics**

Today, we’ll learn about **Generics** in Java, a feature that allows us to write flexible, type-safe, and reusable code. Generics enable you to define classes, methods, and interfaces that operate on types specified by the user at compile time.

---

### **1. What Are Generics?**

Generics allow a type (class or method) to operate on objects of various types without compromising type safety. For example, instead of creating separate classes for handling `Integer` and `String`, we can create a generic class to handle any type.

#### **Key Benefits**:

1. **Type Safety**: Errors are caught at compile time.
2. **Code Reusability**: Write code once, use it for any type.
3. **Elimination of Type Casting**: Avoid explicit casting when retrieving objects.

---

### **2. Generic Syntax**

A **type parameter** is specified using angle brackets (`<>`).

#### **Generic Class Example**

```java
class Box<T> {
    private T item;

    public void setItem(T item) {
        this.item = item;
    }

    public T getItem() {
        return item;
    }
}

public class Main {
    public static void main(String[] args) {
        Box<String> stringBox = new Box<>();
        stringBox.setItem("Hello");
        System.out.println("String Box: " + stringBox.getItem());

        Box<Integer> integerBox = new Box<>();
        integerBox.setItem(123);
        System.out.println("Integer Box: " + integerBox.getItem());
    }
}
```

**Output**:

```
String Box: Hello
Integer Box: 123
```

---

### **3. Generic Methods**

A method can be generic by specifying the type parameter before the return type.

#### **Example: Generic Method**

```java
public class Main {
    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.println(element);
        }
    }

    public static void main(String[] args) {
        String[] stringArray = {"Apple", "Banana", "Cherry"};
        Integer[] intArray = {1, 2, 3};

        printArray(stringArray);
        printArray(intArray);
    }
}
```

**Output**:

```
Apple
Banana
Cherry
1
2
3
```

---

### **4. Bounded Type Parameters**

You can restrict a generic type to a specific range using `extends` for classes and interfaces.

#### **Example: Upper Bound**

```java
class Calculator<T extends Number> {
    public double add(T num1, T num2) {
        return num1.doubleValue() + num2.doubleValue();
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator<Integer> intCalc = new Calculator<>();
        System.out.println("Sum: " + intCalc.add(5, 10));

        Calculator<Double> doubleCalc = new Calculator<>();
        System.out.println("Sum: " + doubleCalc.add(3.5, 2.2));
    }
}
```

**Output**:

```
Sum: 15
Sum: 5.7
```

---

### **5. Wildcards in Generics**

Wildcards (`?`) represent an unknown type and are used when you don’t know the exact type.

#### **5.1 Unbounded Wildcard**

Accepts any type:

```java
public static void printList(List<?> list) {
    for (Object item : list) {
        System.out.println(item);
    }
}
```

#### **5.2 Upper-Bounded Wildcard**

Accepts any type that is a subclass of a specific type:

```java
public static void sumList(List<? extends Number> list) {
    double sum = 0;
    for (Number num : list) {
        sum += num.doubleValue();
    }
    System.out.println("Sum: " + sum);
}
```

#### **5.3 Lower-Bounded Wildcard**

Accepts any type that is a superclass of a specific type:

```java
public static void addElements(List<? super Integer> list) {
    list.add(10);
    list.add(20);
}
```

---

### **6. Generic Collections**

Collections, such as `ArrayList`, use Generics to ensure type safety.

#### **Example: Using Generics in Collections**

```java
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("Hello");
        list.add("World");

        for (String item : list) {
            System.out.println(item);
        }
    }
}
```

**Output**:

```
Hello
World
```

---

### **7. Common Pitfalls and Best Practices**

#### **Pitfalls**:

1. **Raw Types**: Avoid using raw types (e.g., `List` instead of `List<String>`).
   - Example: `List list = new ArrayList();` leads to runtime errors.
2. **Type Erasure**: Generics are implemented using type erasure, meaning type information is removed at runtime.
   - Cannot use `instanceof` with generics: `if (list instanceof ArrayList<Integer>)`.

#### **Best Practices**:

1. Always specify type parameters to avoid raw types.
2. Use bounded types (`<T extends X>`) to constrain types when necessary.
3. Prefer using `Collections` utility methods (e.g., `Collections.emptyList()`).

---

### **8. Hands-On Exercises**

1. **Generic Class**:

   - Create a generic class `Pair` that holds two values of any type and provides getter methods.

2. **Generic Method**:

   - Write a method `swap` that swaps two elements in an array of any type.

3. **Bounded Generics**:

   - Implement a generic method that calculates the average of numbers in a list.

4. **Wildcard Usage**:

   - Write a method to print all elements of a `List<?>` and test it with `ArrayList<String>` and `ArrayList<Integer>`.

5. **Custom Collection**:
   - Create a custom generic stack (`GenericStack<T>`) with `push`, `pop`, and `peek` methods.

---
