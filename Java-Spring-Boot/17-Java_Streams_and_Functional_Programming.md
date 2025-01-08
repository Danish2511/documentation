### **Day 20: Java Streams and Functional Programming**

Today, we’ll explore the **Stream API** and **Functional Programming** in Java. These modern features allow you to write concise, declarative, and efficient code for processing collections of data.

---

### **1. What is the Stream API?**

The **Stream API**, introduced in Java 8, is used for functional-style operations on collections. It allows for data processing using operations like **filter**, **map**, and **reduce** in a declarative manner.

#### **Key Features of Stream API**:

1. **Declarative**: Focus on _what_ to do, not _how_.
2. **Lazy Evaluation**: Operations are only performed when terminal operations (like `collect`) are invoked.
3. **Parallel Processing**: Streams can easily work in parallel for performance optimization.

---

### **2. How Does a Stream Work?**

A Stream pipeline consists of:

1. **Source**: The data source (e.g., `List`, `Set`, `Map`, arrays, etc.).
2. **Intermediate Operations**: Transformations (e.g., `filter`, `map`) that return a new Stream.
3. **Terminal Operations**: Final operations (e.g., `collect`, `forEach`) that produce a result or a side effect.

---

### **3. Creating Streams**

#### **Example: Stream from a List**

```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
        names.stream().forEach(System.out::println);
    }
}
```

**Output**:

```
Alice
Bob
Charlie
```

---

### **4. Intermediate Operations**

Intermediate operations transform a Stream and are **lazy**, meaning they don't execute until a terminal operation is called.

#### **Common Intermediate Operations**:

1. **`filter`**: Filters elements based on a condition.

   ```java
   List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
   numbers.stream()
          .filter(n -> n % 2 == 0)
          .forEach(System.out::println);
   // Output: 2, 4
   ```

2. **`map`**: Transforms elements.

   ```java
   List<String> names = Arrays.asList("Alice", "Bob");
   names.stream()
        .map(String::toUpperCase)
        .forEach(System.out::println);
   // Output: ALICE, BOB
   ```

3. **`sorted`**: Sorts elements.

   ```java
   List<Integer> numbers = Arrays.asList(5, 1, 4);
   numbers.stream()
          .sorted()
          .forEach(System.out::println);
   // Output: 1, 4, 5
   ```

4. **`distinct`**: Removes duplicates.
   ```java
   List<Integer> numbers = Arrays.asList(1, 2, 2, 3);
   numbers.stream()
          .distinct()
          .forEach(System.out::println);
   // Output: 1, 2, 3
   ```

---

### **5. Terminal Operations**

Terminal operations trigger the execution of the Stream pipeline.

#### **Common Terminal Operations**:

1. **`forEach`**: Performs an action for each element.

   ```java
   List<Integer> numbers = Arrays.asList(1, 2, 3);
   numbers.stream().forEach(System.out::println);
   ```

2. **`collect`**: Converts a Stream into a Collection.

   ```java
   List<String> names = Arrays.asList("Alice", "Bob");
   List<String> upperCaseNames = names.stream()
                                      .map(String::toUpperCase)
                                      .collect(Collectors.toList());
   System.out.println(upperCaseNames); // Output: [ALICE, BOB]
   ```

3. **`count`**: Returns the number of elements in the Stream.

   ```java
   List<Integer> numbers = Arrays.asList(1, 2, 3);
   long count = numbers.stream().count();
   System.out.println(count); // Output: 3
   ```

4. **`reduce`**: Reduces the elements to a single value.
   ```java
   List<Integer> numbers = Arrays.asList(1, 2, 3);
   int sum = numbers.stream().reduce(0, Integer::sum);
   System.out.println(sum); // Output: 6
   ```

---

### **6. Functional Programming**

Java supports **Functional Programming** using:

1. **Lambda Expressions**: Anonymous functions for short code.
2. **Functional Interfaces**: Interfaces with a single abstract method (e.g., `Predicate`, `Function`).

#### **Example: Lambda Expressions**

```java
List<Integer> numbers = Arrays.asList(1, 2, 3);
numbers.forEach(n -> System.out.println(n * 2)); // Output: 2, 4, 6
```

#### **Example: Using Functional Interfaces**

```java
import java.util.function.Predicate;

public class Main {
    public static void main(String[] args) {
        Predicate<Integer> isEven = n -> n % 2 == 0;
        System.out.println(isEven.test(4)); // Output: true
    }
}
```

---

### **7. Parallel Streams**

Parallel Streams divide tasks into subtasks and process them concurrently.

#### **Example: Using Parallel Streams**

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
numbers.parallelStream()
       .filter(n -> n % 2 == 0)
       .forEach(System.out::println);
```

---

### **8. Common Pitfalls and Best Practices**

#### **Pitfalls**:

1. **Overusing Parallel Streams**: Can degrade performance for small datasets.
2. **Stateful Operations**: Avoid modifying shared state in Stream operations.
3. **Chaining Without Termination**: Ensure terminal operations are present.

#### **Best Practices**:

1. **Use Readable Pipelines**: Chain operations logically for clarity.
2. **Prefer Immutable Data**: Avoid modifying original collections in Stream pipelines.
3. **Measure Performance**: Profile parallel streams before using them in production.

---

### **9. Hands-On Exercises**

1. **Filter and Print**:

   - Write a program to filter even numbers from a list and print them.

2. **Transform and Collect**:

   - Convert a list of names to uppercase and collect them into a new list.

3. **Find Maximum**:

   - Use the `reduce` method to find the maximum number in a list.

4. **Parallel Processing**:

   - Write a program to calculate the sum of a large list using parallel streams.

5. **Custom Functional Interface**:
   - Create a custom functional interface and use a lambda expression to implement it.

---
