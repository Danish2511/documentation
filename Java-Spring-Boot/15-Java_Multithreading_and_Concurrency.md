### **Day 15: Java Multithreading and Concurrency**

Today, we’ll explore **Java Multithreading**, which allows programs to execute multiple tasks concurrently. You’ll learn how to create and manage threads, work with thread synchronization, and use concurrency utilities.

---

### **1. What is Multithreading?**

Multithreading is the ability to execute multiple threads (independent sequences of execution) within a single program.

#### **Advantages**:

1. **Improved Performance**: Better CPU utilization by parallelizing tasks.
2. **Responsive Applications**: Keeps UI and background tasks running simultaneously.
3. **Efficient Resource Use**: Shares memory and resources between threads.

---

### **2. Threads in Java**

In Java, a thread can be created in two main ways:

1. **Extending the `Thread` class**.
2. **Implementing the `Runnable` interface**.

---

### **3. Creating Threads**

#### **3.1 Extending `Thread` Class**

```java
class MyThread extends Thread {
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println("Thread " + Thread.currentThread().getId() + ": " + i);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread thread1 = new MyThread();
        MyThread thread2 = new MyThread();

        thread1.start();
        thread2.start();
    }
}
```

**Output** (order may vary):

```
Thread 11: 1
Thread 11: 2
Thread 12: 1
Thread 12: 2
...
```

---

#### **3.2 Implementing `Runnable` Interface**

```java
class MyRunnable implements Runnable {
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println("Runnable Thread: " + i);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Thread thread = new Thread(new MyRunnable());
        thread.start();
    }
}
```

---

### **4. Thread Lifecycle**

A thread has the following states:

1. **New**: Created but not started (`new Thread()`).
2. **Runnable**: Ready to run after calling `start()`.
3. **Running**: Actively executing.
4. **Waiting/Blocked**: Waiting for resources or another thread.
5. **Terminated**: Completed execution.

---

### **5. Thread Synchronization**

Synchronization ensures that shared resources are accessed by only one thread at a time to avoid race conditions.

#### **Example: Synchronized Block**

```java
class Counter {
    private int count = 0;

    public void increment() {
        synchronized (this) {
            count++;
        }
    }

    public int getCount() {
        return count;
    }
}

public class Main {
    public static void main(String[] args) {
        Counter counter = new Counter();

        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        };

        Thread thread1 = new Thread(task);
        Thread thread2 = new Thread(task);

        thread1.start();
        thread2.start();

        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Final Count: " + counter.getCount());
    }
}
```

**Output**:

```
Final Count: 2000
```

---

### **6. Concurrency Utilities**

Java provides the **java.util.concurrent** package for advanced multithreading and concurrency.

#### **6.1 Using `ExecutorService`**

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Main {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(3);

        for (int i = 1; i <= 5; i++) {
            final int task = i;
            executor.submit(() -> {
                System.out.println("Task " + task + " executed by " + Thread.currentThread().getName());
            });
        }

        executor.shutdown();
    }
}
```

**Output**:

```
Task 1 executed by pool-1-thread-1
Task 2 executed by pool-1-thread-2
...
```

---

#### **6.2 Using `Callable` and `Future`**

`Callable` allows returning results from threads.

```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class Main {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newSingleThreadExecutor();

        Callable<Integer> task = () -> {
            Thread.sleep(1000);
            return 42;
        };

        Future<Integer> future = executor.submit(task);

        try {
            System.out.println("Result: " + future.get());
        } catch (Exception e) {
            e.printStackTrace();
        }

        executor.shutdown();
    }
}
```

**Output**:

```
Result: 42
```

---

### **7. Common Pitfalls and Best Practices**

#### **Pitfalls**:

1. **Deadlocks**: Occurs when two threads wait indefinitely for each other to release locks.
   - Use **tryLocks** or design locks carefully.
2. **Resource Contention**: Too many threads can lead to performance degradation.

#### **Best Practices**:

1. **Use Executors**: Avoid manually creating and managing threads; use `ExecutorService`.
2. **Minimize Synchronized Blocks**: Only synchronize the critical section of code.
3. **Thread Safety**: Use thread-safe classes like `ConcurrentHashMap` and `CopyOnWriteArrayList`.

---

### **8. Hands-On Exercises**

1. **Basic Threads**:

   - Create a program with two threads that print numbers from 1 to 10 alternately.

2. **Thread Synchronization**:

   - Write a program where multiple threads increment a shared counter safely.

3. **Using ExecutorService**:

   - Use a fixed thread pool to execute 10 tasks that each print the square of their index.

4. **Callable and Future**:
   - Write a program to calculate factorial of a number using `Callable`.

---

### **Day 19: Multithreading and Concurrency in Java**

Today, we’ll explore **Multithreading and Concurrency**, one of the most powerful features of Java for improving application performance and responsiveness. You’ll learn how to create, manage, and synchronize threads in Java.

---

### **1. What is Multithreading?**

**Multithreading** is the process of executing multiple threads simultaneously. A **thread** is the smallest unit of a program that can execute independently.

#### **Benefits of Multithreading**:

1. **Improved Performance**: Tasks run in parallel.
2. **Better Resource Utilization**: Efficient CPU usage.
3. **Responsive Applications**: Keeps GUIs responsive by handling tasks in the background.

---

### **2. Thread Life Cycle**

A thread can be in one of the following states:

1. **New**: Thread is created but not started.
2. **Runnable**: Thread is ready to run and waiting for CPU.
3. **Running**: Thread is actively executing.
4. **Blocked/Waiting**: Thread is waiting for resources or another thread.
5. **Terminated**: Thread has completed its task.

---

### **3. Creating Threads**

Java provides two ways to create threads:

1. **Extending the `Thread` class**
2. **Implementing the `Runnable` interface**

#### **Example 1: Extending `Thread`**

```java
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread is running: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread thread = new MyThread();
        thread.start();
    }
}
```

**Output**:

```
Thread is running: Thread-0
```

---

#### **Example 2: Implementing `Runnable`**

```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Thread is running: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        Thread thread = new Thread(new MyRunnable());
        thread.start();
    }
}
```

**Output**:

```
Thread is running: Thread-0
```

---

### **4. Thread Methods**

Some useful methods for thread management:

1. **`start()`**: Starts the thread.
2. **`run()`**: Contains the code executed by the thread.
3. **`sleep(milliseconds)`**: Puts the thread to sleep.
4. **`join()`**: Waits for the thread to complete.
5. **`isAlive()`**: Checks if the thread is still running.

#### **Example: Using `sleep` and `join`**

```java
class MyThread extends Thread {
    @Override
    public void run() {
        try {
            System.out.println("Thread: " + Thread.currentThread().getName() + " is running");
            Thread.sleep(1000);
            System.out.println("Thread: " + Thread.currentThread().getName() + " has finished");
        } catch (InterruptedException e) {
            System.out.println("Thread interrupted.");
        }
    }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {
        MyThread thread1 = new MyThread();
        MyThread thread2 = new MyThread();

        thread1.start();
        thread2.start();

        thread1.join(); // Main thread waits for thread1 to finish
        thread2.join();

        System.out.println("All threads have finished.");
    }
}
```

**Output**:

```
Thread: Thread-0 is running
Thread: Thread-1 is running
Thread: Thread-0 has finished
Thread: Thread-1 has finished
All threads have finished.
```

---

### **5. Thread Synchronization**

When multiple threads access shared resources, **synchronization** is used to prevent data inconsistency.

#### **Example: Synchronized Block**

```java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();

        Thread thread1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });

        Thread thread2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });

        thread1.start();
        thread2.start();

        thread1.join();
        thread2.join();

        System.out.println("Final count: " + counter.getCount());
    }
}
```

**Output**:

```
Final count: 2000
```

---

### **6. Executor Framework**

The `Executor` framework provides a high-level API for managing threads.

#### **Example: Using `ExecutorService`**

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Main {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(2);

        Runnable task = () -> {
            System.out.println("Thread: " + Thread.currentThread().getName() + " is running");
        };

        executor.execute(task);
        executor.execute(task);

        executor.shutdown();
    }
}
```

**Output**:

```
Thread: pool-1-thread-1 is running
Thread: pool-1-thread-2 is running
```

---

### **7. Common Pitfalls and Best Practices**

#### **Pitfalls**:

1. **Race Conditions**: Occurs when multiple threads access shared data without synchronization.
2. **Deadlocks**: Two or more threads are waiting for each other indefinitely.
3. **Excessive Thread Creation**: Can exhaust system resources.

#### **Best Practices**:

1. Use synchronization only when necessary to avoid performance bottlenecks.
2. Prefer higher-level abstractions like `ExecutorService` over manually managing threads.
3. Avoid `Thread.sleep()` unless absolutely needed; prefer proper thread signaling mechanisms.

---

### **8. Hands-On Exercises**

1. **Create a Thread**:

   - Write a program to create a thread using the `Runnable` interface to print numbers from 1 to 10.

2. **Thread Synchronization**:

   - Write a program where two threads increment a shared counter, ensuring the result is consistent using `synchronized`.

3. **Deadlock Simulation**:

   - Simulate a deadlock scenario and then resolve it using proper synchronization techniques.

4. **Executor Framework**:

   - Use the `ExecutorService` to run 5 tasks in a thread pool of size 3.

5. **Thread Communication**:
   - Implement a producer-consumer pattern using `wait()` and `notify()`.

---
