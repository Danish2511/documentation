### **Day 12: Advanced Java Collections**

Today, we’ll explore more advanced topics in the **Java Collections Framework**, such as `LinkedList`, `TreeSet`, `PriorityQueue`, and iterators. These data structures are essential for handling complex scenarios like sorting, prioritization, and hierarchical organization.

---

### **1. LinkedList**

A `LinkedList` is a doubly-linked list that implements the `List` and `Deque` interfaces. It allows efficient insertions and deletions compared to `ArrayList`.

#### **When to Use LinkedList**:

- When frequent insertions/deletions occur in the middle of the list.
- When traversal is less frequent than modification.

#### **Example: Using LinkedList**

```java
import java.util.LinkedList;

public class Main {
    public static void main(String[] args) {
        LinkedList<String> tasks = new LinkedList<>();

        // Add elements
        tasks.add("Task 1");
        tasks.add("Task 2");
        tasks.add("Task 3");

        // Add at specific positions
        tasks.addFirst("Urgent Task");
        tasks.addLast("Low Priority Task");

        // Access elements
        System.out.println("First Task: " + tasks.getFirst());
        System.out.println("Last Task: " + tasks.getLast());

        // Remove elements
        tasks.removeFirst();
        tasks.removeLast();

        // Display tasks
        for (String task : tasks) {
            System.out.println(task);
        }
    }
}
```

**Output**:

```
First Task: Urgent Task
Last Task: Low Priority Task
Task 1
Task 2
```

---

### **2. TreeSet**

A `TreeSet` is a collection that implements the `Set` interface. It is sorted in natural order or using a custom comparator.

#### **When to Use TreeSet**:

- When you need a sorted, unique collection.
- When frequent searches are required.

#### **Example: Using TreeSet**

```java
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> numbers = new TreeSet<>();

        // Add elements
        numbers.add(5);
        numbers.add(3);
        numbers.add(9);
        numbers.add(1);

        // Duplicate elements are ignored
        numbers.add(5);

        // Display elements in sorted order
        for (int num : numbers) {
            System.out.println(num);
        }

        // Check for specific elements
        System.out.println("Contains 3? " + numbers.contains(3));

        // Get subsets
        System.out.println("Subset: " + numbers.headSet(5));
    }
}
```

**Output**:

```
1
3
5
9
Contains 3? true
Subset: [1, 3]
```

---

### **3. PriorityQueue**

A `PriorityQueue` is a queue where elements are ordered based on their priority (natural or custom).

#### **When to Use PriorityQueue**:

- When processing elements in a specific order (e.g., smallest first).
- In tasks like job scheduling or event management.

#### **Example: Using PriorityQueue**

```java
import java.util.PriorityQueue;

public class Main {
    public static void main(String[] args) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();

        // Add elements
        pq.add(10);
        pq.add(5);
        pq.add(20);
        pq.add(1);

        // Poll elements (removes the smallest first)
        while (!pq.isEmpty()) {
            System.out.println(pq.poll());
        }
    }
}
```

**Output**:

```
1
5
10
20
```

---

### **4. Iterators**

An **iterator** is used to traverse collections like `List`, `Set`, and `Map`. It provides methods to iterate, remove, and access elements.

#### **Example: Using Iterator**

```java
import java.util.ArrayList;
import java.util.Iterator;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();
        names.add("Alice");
        names.add("Bob");
        names.add("Charlie");

        // Create an iterator
        Iterator<String> iterator = names.iterator();

        // Traverse using the iterator
        while (iterator.hasNext()) {
            String name = iterator.next();
            System.out.println(name);

            // Remove "Bob"
            if (name.equals("Bob")) {
                iterator.remove();
            }
        }

        System.out.println("Updated List: " + names);
    }
}
```

**Output**:

```
Alice
Bob
Charlie
Updated List: [Alice, Charlie]
```

---

### **5. Common Pitfalls and Best Practices**

#### **Pitfalls**:

1. **ConcurrentModificationException**:

   - Modifying a collection directly during iteration causes this exception. Always use iterators.

2. **Choosing the Wrong Data Structure**:
   - Using `ArrayList` for frequent deletions can be inefficient. Consider `LinkedList` or `HashSet`.

#### **Best Practices**:

1. **Understand the Requirements**:

   - Use `HashSet` for uniqueness, `TreeSet` for sorted uniqueness, and `ArrayList` for ordered collections.

2. **Use Enhanced Loops When Possible**:

   - For simple traversals, prefer `for-each` loops for cleaner code.

   ```java
   for (String item : list) {
       System.out.println(item);
   }
   ```

3. **Avoid Overusing Collections**:
   - Choose collections based on the use case to optimize performance.

---

### **6. Hands-On Exercises**

1. **Task Management with LinkedList**:

   - Create a `LinkedList` of daily tasks.
   - Add, remove, and display tasks.

2. **Sorted Words with TreeSet**:

   - Write a program to add words to a `TreeSet` and display them in alphabetical order.

3. **Priority Queue Simulation**:

   - Simulate a priority queue for patients in a hospital where lower numbers indicate higher priority.

4. **Removing Elements with Iterator**:

   - Create an `ArrayList` of numbers.
   - Use an iterator to remove all even numbers.

5. **Combine Data Structures**:
   - Use a `TreeMap` where each key is a department name, and the value is a `PriorityQueue` of employees in that department.

---
