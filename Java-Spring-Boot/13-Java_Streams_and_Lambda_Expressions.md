### **Day 13: Java Streams and Lambda Expressions**

Today, we’ll explore **Java Streams** and **Lambda Expressions**, two powerful features introduced in Java 8. These tools simplify working with data by enabling functional-style operations on collections and other data sources.

---

### **1. What are Streams?**

A **Stream** in Java is a sequence of elements that supports various methods to perform **pipeline operations** like filtering, mapping, and reducing.

#### **Key Characteristics**:

1. **Not a Data Structure**: Streams process data from collections, arrays, or I/O channels but do not store data.
2. **Functional Programming Style**: Enables concise, readable code.
3. **Lazy Execution**: Intermediate operations are not executed until a terminal operation is invoked.

---

### **2. Why Use Streams?**

- **Simplify Code**: Perform operations like filtering, sorting, and mapping concisely.
- **Parallel Processing**: Easily perform operations across multiple threads using `parallelStream()`.
- **Readability**: Functional style improves readability and reduces boilerplate code.

---

### **3. Stream Operations**

Streams have two types of operations:

#### **Intermediate Operations**:

These return another stream, allowing method chaining. Examples include:

- `filter()`: Filters elements based on a condition.
- `map()`: Transforms elements.
- `sorted()`: Sorts elements.

#### **Terminal Operations**:

These produce a result or a side effect. Examples include:

- `collect()`: Collects stream elements into a collection.
- `forEach()`: Performs an action for each element.
- `reduce()`: Reduces elements to a single value.

---

### **4. Lambda Expressions**

A **lambda expression** is a concise way to represent an anonymous function.

#### **Syntax**:

```java
(parameter) -> expression
```

#### **Example**:

```java
// Lambda to calculate the square of a number
x -> x * x
```

---

### **5. Examples of Streams and Lambdas**

#### **5.1 Basic Stream Operations**

```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");

        // Filter names starting with 'A'
        names.stream()
             .filter(name -> name.startsWith("A"))
             .forEach(System.out::println);

        // Transform names to uppercase
        names.stream()
             .map(String::toUpperCase)
             .forEach(System.out::println);

        // Sort names
        names.stream()
             .sorted()
             .forEach(System.out::println);
    }
}
```

**Output**:

```
Alice
ALICE
BOB
CHARLIE
DAVID
Alice
Bob
Charlie
David
```

---

#### **5.2 Collecting Stream Results**

```java
import java.util.List;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);

        // Collect even numbers into a new list
        List<Integer> evens = numbers.stream()
                                     .filter(num -> num % 2 == 0)
                                     .collect(Collectors.toList());

        System.out.println("Even Numbers: " + evens);
    }
}
```

**Output**:

```
Even Numbers: [2, 4, 6]
```

---

#### **5.3 Reduce Operation**

```java
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = List.of(1, 2, 3, 4, 5);

        // Sum all numbers using reduce
        int sum = numbers.stream()
                         .reduce(0, Integer::sum);

        System.out.println("Sum: " + sum);
    }
}
```

**Output**:

```
Sum: 15
```

---

#### **5.4 Parallel Streams**

```java
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        // Parallel stream to print numbers
        IntStream.range(1, 10)
                 .parallel()
                 .forEach(System.out::println);
    }
}
```

**Output** (Order may vary due to parallel execution):

```
3
1
2
4
6
7
5
9
8
```

---

### **6. Common Pitfalls and Best Practices**

#### **Pitfalls**:

1. **Modifying State in Streams**: Avoid changing external state inside streams.
   - Instead of modifying variables, collect results using `collect()`.
2. **Parallel Streams Misuse**: Parallel streams can introduce performance issues for small datasets or shared resources.

#### **Best Practices**:

1. **Use Method References**:

   - Instead of `x -> x.toUpperCase()`, prefer `String::toUpperCase` for readability.

2. **Combine Operations**:

   - Use method chaining to perform multiple transformations in a single pipeline.

3. **Limit Parallel Streams**:
   - Use parallel streams for compute-intensive tasks with large datasets.

---

### **7. Hands-On Exercises**

1. **Filtering and Transforming**:

   - Create a list of integers from 1 to 20.
   - Filter out odd numbers and transform the remaining ones into their squares.

2. **Using Collectors**:

   - Given a list of words, filter words with more than 4 characters and collect them into a `Set`.

3. **Reduce for Aggregation**:

   - Use `reduce()` to calculate the product of all elements in a list.

4. **Parallel Processing**:
   - Use `parallelStream()` to sum all even numbers from 1 to 1,000,000.

---
