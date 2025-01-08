### **Day 8: Java Collections Framework**

Today, we’ll learn about the **Java Collections Framework**, which is used to store, retrieve, and manipulate data efficiently. Collections are essential for working with dynamic data structures like lists, sets, and maps.

---

### **1. What is the Java Collections Framework?**

The **Java Collections Framework (JCF)** provides a unified architecture to work with collections (groups of objects).

- It includes interfaces, classes, and algorithms to work with data structures.
- Commonly used interfaces: `List`, `Set`, `Queue`, `Map`.

#### **Hierarchy of Java Collections Framework**

- **Interfaces**: Define the types of collections (e.g., `List`, `Set`, `Map`).
- **Implementations**: Concrete classes (e.g., `ArrayList`, `HashSet`, `HashMap`).

---

### **2. Commonly Used Interfaces and Classes**

#### **List Interface**

- **Allows duplicates.**
- Preserves insertion order.

**Implementation Classes**:

1. **ArrayList**: Dynamic array.
2. **LinkedList**: Doubly-linked list.

**Example: Using ArrayList**

```java
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();
        names.add("Alice");
        names.add("Bob");
        names.add("Alice"); // Duplicates allowed

        System.out.println(names); // Output: [Alice, Bob, Alice]

        // Access elements
        System.out.println("First Name: " + names.get(0));

        // Loop through list
        for (String name : names) {
            System.out.println(name);
        }
    }
}
```

---

#### **Set Interface**

- **Does not allow duplicates.**
- No guarantee of insertion order.

**Implementation Classes**:

1. **HashSet**: Unordered.
2. **LinkedHashSet**: Maintains insertion order.

**Example: Using HashSet**

```java
import java.util.HashSet;

public class Main {
    public static void main(String[] args) {
        HashSet<Integer> numbers = new HashSet<>();
        numbers.add(1);
        numbers.add(2);
        numbers.add(1); // Duplicate, ignored

        System.out.println(numbers); // Output: [1, 2] (order may vary)
    }
}
```

---

#### **Map Interface**

- **Key-value pairs.**
- Keys are unique, but values can be duplicated.

**Implementation Classes**:

1. **HashMap**: Unordered key-value pairs.
2. **LinkedHashMap**: Maintains insertion order.

**Example: Using HashMap**

```java
import java.util.HashMap;

public class Main {
    public static void main(String[] args) {
        HashMap<String, Integer> scores = new HashMap<>();
        scores.put("Alice", 90);
        scores.put("Bob", 85);
        scores.put("Alice", 95); // Updates Alice's score

        System.out.println(scores); // Output: {Bob=85, Alice=95}
    }
}
```

---

#### **Queue Interface**

- **FIFO (First-In-First-Out)**.
- Used for tasks like scheduling.

**Implementation Class**:

1. **PriorityQueue**: Orders elements based on their natural ordering or a custom comparator.

**Example: Using PriorityQueue**

```java
import java.util.PriorityQueue;

public class Main {
    public static void main(String[] args) {
        PriorityQueue<Integer> queue = new PriorityQueue<>();
        queue.add(10);
        queue.add(5);
        queue.add(15);

        while (!queue.isEmpty()) {
            System.out.println(queue.poll()); // Removes elements in ascending order
        }
    }
}
```

---

### **3. Iterating Over Collections**

#### **For-Each Loop**

Used for simple iteration.

```java
for (String item : list) {
    System.out.println(item);
}
```

#### **Iterator**

Provides more control, like removing elements during iteration.

```java
import java.util.ArrayList;
import java.util.Iterator;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();
        names.add("Alice");
        names.add("Bob");

        Iterator<String> iterator = names.iterator();
        while (iterator.hasNext()) {
            String name = iterator.next();
            if (name.equals("Alice")) {
                iterator.remove(); // Remove "Alice"
            }
        }

        System.out.println(names); // Output: [Bob]
    }
}
```

---

### **4. Comparable vs Comparator**

- **Comparable**: Used to define natural ordering.
- **Comparator**: Used for custom ordering.

**Example: Comparable**

```java
import java.util.ArrayList;
import java.util.Collections;

class Student implements Comparable<Student> {
    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public int compareTo(Student other) {
        return this.age - other.age; // Ascending order by age
    }

    @Override
    public String toString() {
        return name + " (" + age + ")";
    }
}

public class Main {
    public static void main(String[] args) {
        ArrayList<Student> students = new ArrayList<>();
        students.add(new Student("Alice", 22));
        students.add(new Student("Bob", 20));

        Collections.sort(students); // Sort using Comparable
        System.out.println(students); // Output: [Bob (20), Alice (22)]
    }
}
```

---

### **5. Hands-On Exercises**

1. **Using List**:  
   Create an `ArrayList` of integers, add 5 numbers, sort the list, and display the largest number.

2. **Using Set**:  
   Create a `HashSet` to store unique names. Add 5 names, try adding a duplicate, and display the set.

3. **Using Map**:  
   Create a `HashMap` to store employee IDs and their salaries. Display all employees earning above a certain threshold.

4. **PriorityQueue**:  
   Implement a task scheduler using `PriorityQueue`, where each task has a priority.

5. **Comparable and Comparator**:
   - Create a class `Book` with attributes `title` and `price`.
   - Use `Comparable` to sort by price and `Comparator` to sort by title.

---
