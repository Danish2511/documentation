### **Day 11: Java Collections Framework (Introduction)**

Today, we’ll dive into the **Java Collections Framework (JCF)**, an essential library for managing groups of objects efficiently. Collections are a cornerstone of Java programming, enabling developers to handle data structures like lists, sets, and maps effectively.

---

### **1. What are Collections?**

A **collection** is a framework in Java used to store, retrieve, manipulate, and communicate aggregate data. It provides standard implementations of common data structures.

---

### **2. Why Use Collections?**

1. **Dynamic Resizing**: Collections can grow or shrink in size dynamically.
2. **Ease of Use**: Pre-built methods make operations like searching, sorting, and insertion straightforward.
3. **Polymorphic Usage**: Collections work with generic types, enabling type safety.
4. **Performance**: Optimized for various use cases like fast access (`ArrayList`) or unique elements (`HashSet`).

---

### **3. Key Interfaces in Collections Framework**

#### **Hierarchy**:

The primary interfaces of the Java Collections Framework include:

1. **`Collection`**:
   - Super interface of all collections.
   - Subinterfaces: `List`, `Set`, `Queue`.
2. **`List`**:
   - Ordered, allows duplicates.
   - Implementations: `ArrayList`, `LinkedList`.
3. **`Set`**:
   - Unordered, no duplicates.
   - Implementations: `HashSet`, `TreeSet`.
4. **`Queue`**:
   - FIFO (First In First Out).
   - Implementation: `PriorityQueue`.
5. **`Map`** (Not part of `Collection`):
   - Key-value pairs.
   - Implementations: `HashMap`, `TreeMap`.

---

### **4. Key Classes and Examples**

#### **4.1 ArrayList**

- A resizable array.
- Best for fast random access and frequent read operations.

**Example: Basic Operations on ArrayList**

```java
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> fruits = new ArrayList<>();

        // Add elements
        fruits.add("Apple");
        fruits.add("Banana");
        fruits.add("Cherry");

        // Access elements
        System.out.println("First fruit: " + fruits.get(0));

        // Modify elements
        fruits.set(1, "Blueberry");

        // Remove elements
        fruits.remove("Cherry");

        // Loop through list
        for (String fruit : fruits) {
            System.out.println(fruit);
        }
    }
}
```

**Output**:

```
First fruit: Apple
Apple
Blueberry
```

---

#### **4.2 HashSet**

- A set that does not allow duplicates.
- Uses hashing for fast operations.

**Example: Using HashSet**

```java
import java.util.HashSet;

public class Main {
    public static void main(String[] args) {
        HashSet<Integer> numbers = new HashSet<>();

        // Add elements
        numbers.add(1);
        numbers.add(2);
        numbers.add(3);
        numbers.add(2); // Duplicate, will be ignored

        // Check existence
        System.out.println("Contains 2? " + numbers.contains(2));

        // Remove elements
        numbers.remove(3);

        // Loop through set
        for (int num : numbers) {
            System.out.println(num);
        }
    }
}
```

**Output**:

```
Contains 2? true
1
2
```

---

#### **4.3 HashMap**

- A map that stores key-value pairs.
- Keys are unique, values can be duplicated.

**Example: Using HashMap**

```java
import java.util.HashMap;

public class Main {
    public static void main(String[] args) {
        HashMap<String, Integer> ageMap = new HashMap<>();

        // Add key-value pairs
        ageMap.put("Alice", 25);
        ageMap.put("Bob", 30);

        // Access value by key
        System.out.println("Alice's age: " + ageMap.get("Alice"));

        // Update value
        ageMap.put("Alice", 26);

        // Remove key-value pair
        ageMap.remove("Bob");

        // Loop through map
        for (String key : ageMap.keySet()) {
            System.out.println(key + ": " + ageMap.get(key));
        }
    }
}
```

**Output**:

```
Alice's age: 25
Alice: 26
```

---

### **5. Common Methods in Collections**

| Method              | Description                         | Example                     |
| ------------------- | ----------------------------------- | --------------------------- |
| `add(element)`      | Adds an element to the collection.  | `list.add("Apple");`        |
| `remove(element)`   | Removes an element.                 | `set.remove(2);`            |
| `size()`            | Returns the size of the collection. | `list.size();`              |
| `contains(element)` | Checks if the element exists.       | `map.containsKey("Alice");` |
| `clear()`           | Clears the collection.              | `list.clear();`             |

---

### **6. Common Pitfalls and Best Practices**

#### **Pitfalls**:

1. **Null Values in Collections**:
   - Some collections like `TreeMap` don’t allow null keys.
2. **Concurrent Modifications**:
   - Modifying a collection during iteration throws `ConcurrentModificationException`.

#### **Best Practices**:

1. Use **generics** to ensure type safety.
   ```java
   ArrayList<String> list = new ArrayList<>();
   ```
2. Prefer **`List`**, **`Set`**, or **`Map`** interfaces over concrete implementations.
   ```java
   List<String> list = new ArrayList<>();
   ```

---

### **7. Hands-On Exercises**

1. **Using ArrayList**:

   - Create an `ArrayList` to store names of cities.
   - Add, remove, and display cities.
   - Sort the list alphabetically.

2. **Unique Values with HashSet**:

   - Write a program to store unique student IDs in a `HashSet`.
   - Add duplicate IDs and verify they aren’t added twice.

3. **Key-Value Pair with HashMap**:

   - Create a `HashMap` to store employee names and salaries.
   - Retrieve, update, and display entries.

4. **Combining Collections**:
   - Create a `HashMap` where keys are student names and values are `ArrayList` of grades.

---

### **Day 16: Java Collections Framework**

Today, we’ll dive into the **Java Collections Framework (JCF)**, which provides data structures and algorithms to handle groups of objects efficiently. You'll learn about lists, sets, maps, and queues, along with their real-world use cases.

---

### **1. What is the Java Collections Framework?**

The **Java Collections Framework (JCF)** is a set of classes and interfaces in the `java.util` package designed to store and manipulate groups of objects (collections).

#### **Key Features**:

1. Predefined data structures like `ArrayList`, `HashSet`, and `HashMap`.
2. Algorithms for searching, sorting, and iterating over collections.
3. Thread-safe variants for concurrent programming (e.g., `ConcurrentHashMap`).

---

### **2. Core Interfaces in Collections Framework**

| **Interface** | **Description**                         | **Example Implementations**   |
| ------------- | --------------------------------------- | ----------------------------- |
| `List`        | Ordered collection (allows duplicates). | `ArrayList`, `LinkedList`     |
| `Set`         | Unordered collection (no duplicates).   | `HashSet`, `TreeSet`          |
| `Map`         | Key-value pairs (unique keys).          | `HashMap`, `TreeMap`          |
| `Queue`       | Elements processed in FIFO order.       | `LinkedList`, `PriorityQueue` |

---

### **3. List Interface**

The `List` interface maintains insertion order and allows duplicate elements.

#### **Example: ArrayList**

```java
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();

        // Adding elements
        list.add("Apple");
        list.add("Banana");
        list.add("Cherry");

        // Accessing elements
        System.out.println("First Element: " + list.get(0));

        // Iterating
        for (String item : list) {
            System.out.println(item);
        }

        // Removing elements
        list.remove("Banana");
        System.out.println("After Removal: " + list);
    }
}
```

**Output**:

```
First Element: Apple
Apple
Banana
Cherry
After Removal: [Apple, Cherry]
```

---

### **4. Set Interface**

The `Set` interface does not allow duplicate elements.

#### **Example: HashSet**

```java
import java.util.HashSet;

public class Main {
    public static void main(String[] args) {
        HashSet<String> set = new HashSet<>();

        // Adding elements
        set.add("Dog");
        set.add("Cat");
        set.add("Dog"); // Duplicate, ignored

        // Iterating
        for (String item : set) {
            System.out.println(item);
        }
    }
}
```

**Output** (order may vary):

```
Dog
Cat
```

---

### **5. Map Interface**

The `Map` interface stores key-value pairs. Keys must be unique.

#### **Example: HashMap**

```java
import java.util.HashMap;

public class Main {
    public static void main(String[] args) {
        HashMap<String, Integer> map = new HashMap<>();

        // Adding key-value pairs
        map.put("Alice", 25);
        map.put("Bob", 30);
        map.put("Alice", 28); // Updates value for "Alice"

        // Accessing values
        System.out.println("Alice's Age: " + map.get("Alice"));

        // Iterating
        for (String key : map.keySet()) {
            System.out.println(key + ": " + map.get(key));
        }
    }
}
```

**Output**:

```
Alice's Age: 28
Alice: 28
Bob: 30
```

---

### **6. Queue Interface**

The `Queue` interface represents a collection designed for holding elements before processing (FIFO).

#### **Example: PriorityQueue**

```java
import java.util.PriorityQueue;

public class Main {
    public static void main(String[] args) {
        PriorityQueue<Integer> queue = new PriorityQueue<>();

        // Adding elements
        queue.add(10);
        queue.add(5);
        queue.add(20);

        // Accessing and removing elements
        while (!queue.isEmpty()) {
            System.out.println(queue.poll()); // Retrieves and removes head
        }
    }
}
```

**Output**:

```
5
10
20
```

---

### **7. Sorting Collections**

The `Collections` class provides methods to sort and manipulate collections.

#### **Example: Sorting a List**

```java
import java.util.ArrayList;
import java.util.Collections;

public class Main {
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>();
        list.add(3);
        list.add(1);
        list.add(2);

        // Sorting
        Collections.sort(list);
        System.out.println("Sorted List: " + list);

        // Reverse Sorting
        Collections.sort(list, Collections.reverseOrder());
        System.out.println("Reversed List: " + list);
    }
}
```

**Output**:

```
Sorted List: [1, 2, 3]
Reversed List: [3, 2, 1]
```

---

### **8. Common Pitfalls and Best Practices**

#### **Pitfalls**:

1. **Choosing the Wrong Collection**: For example, using a `LinkedList` when random access is needed (use `ArrayList` instead).
2. **Modifying a Collection During Iteration**: Avoid `ConcurrentModificationException` by using iterators or `ConcurrentHashMap`.

#### **Best Practices**:

1. **Choose the Right Data Structure**:
   - Use `ArrayList` for frequent reads, `LinkedList` for frequent inserts/removals.
   - Use `HashMap` for fast lookups, `TreeMap` for sorted keys.
2. **Thread-Safe Variants**: For concurrent applications, use `ConcurrentHashMap` or `CopyOnWriteArrayList`.
3. **Minimize Direct Iteration**: Use Java 8+ Streams for cleaner code.

---

### **9. Hands-On Exercises**

1. **List Operations**:

   - Create a `List` of integers and find the maximum value using `Collections.max()`.

2. **Set Operations**:

   - Create a `Set` of student names and check for duplicates.

3. **Map Operations**:

   - Create a `Map` of employee IDs and names. Retrieve names by ID and iterate over all entries.

4. **Queue Operations**:

   - Simulate a ticket counter using a `Queue`. Process customers in the order they arrive.

5. **Sorting**:
   - Create a list of strings and sort them in ascending and descending order.

---
