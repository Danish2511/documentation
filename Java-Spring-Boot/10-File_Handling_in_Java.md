### **Day 10: File Handling in Java**

Today, we will focus on **File Handling** in Java, which enables reading from and writing to files. This is an essential skill for tasks like logging, data persistence, and interacting with external data sources.

---

### **1. What is File Handling?**

File handling refers to the process of creating, reading, writing, and deleting files. Java provides classes in the `java.io` and `java.nio` packages for file handling.

---

### **2. Classes for File Handling**

#### **Important Classes in `java.io`**:

1. **`File`**: Represents a file or directory.
2. **`FileReader`**: Reads character streams from a file.
3. **`FileWriter`**: Writes character streams to a file.
4. **`BufferedReader` and `BufferedWriter`**: For efficient reading and writing.

---

### **3. Creating and Checking Files**

#### **Example: Create a File**

```java
import java.io.File;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try {
            File file = new File("example.txt");
            if (file.createNewFile()) {
                System.out.println("File created: " + file.getName());
            } else {
                System.out.println("File already exists.");
            }
        } catch (IOException e) {
            System.out.println("An error occurred.");
            e.printStackTrace();
        }
    }
}
```

---

### **4. Writing to a File**

#### **Example: Write Text to a File**

```java
import java.io.FileWriter;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try {
            FileWriter writer = new FileWriter("example.txt");
            writer.write("Hello, world!\nWelcome to Java File Handling.");
            writer.close();
            System.out.println("Successfully wrote to the file.");
        } catch (IOException e) {
            System.out.println("An error occurred.");
            e.printStackTrace();
        }
    }
}
```

---

### **5. Reading from a File**

#### **Example: Read Text from a File**

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        try {
            File file = new File("example.txt");
            Scanner scanner = new Scanner(file);
            while (scanner.hasNextLine()) {
                String line = scanner.nextLine();
                System.out.println(line);
            }
            scanner.close();
        } catch (FileNotFoundException e) {
            System.out.println("File not found.");
            e.printStackTrace();
        }
    }
}
```

---

### **6. Using BufferedReader and BufferedWriter**

#### **Example: Efficient Reading**

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("example.txt"))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.out.println("An error occurred.");
            e.printStackTrace();
        }
    }
}
```

#### **Example: Efficient Writing**

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("example.txt", true))) {
            writer.write("\nAppended text using BufferedWriter.");
            System.out.println("Text successfully appended.");
        } catch (IOException e) {
            System.out.println("An error occurred.");
            e.printStackTrace();
        }
    }
}
```

---

### **7. Deleting a File**

#### **Example: Delete a File**

```java
import java.io.File;

public class Main {
    public static void main(String[] args) {
        File file = new File("example.txt");
        if (file.delete()) {
            System.out.println("Deleted the file: " + file.getName());
        } else {
            System.out.println("Failed to delete the file.");
        }
    }
}
```

---

### **8. Handling File Paths**

Java allows you to work with **absolute** and **relative paths**:

- **Absolute Path**: Full path from the root directory.
- **Relative Path**: Path relative to the project directory.

#### **Example: Using Relative Paths**

```java
File file = new File("src/resources/example.txt");
```

#### **Example: Using Absolute Paths**

```java
File file = new File("C:\\Users\\YourName\\example.txt");
```

---

### **9. Common Pitfalls and Best Practices**

#### **Pitfalls**:

1. **Not closing file streams**:  
   Always close streams to avoid memory leaks. Use try-with-resources for automatic closure.
2. **Hardcoding file paths**:  
   Avoid hardcoding paths; use configuration files or environment variables.

#### **Best Practices**:

1. Use `BufferedReader` and `BufferedWriter` for efficiency with large files.
2. Always check if a file exists before performing operations.
3. Use exception handling for robust file operations.

---

### **10. Hands-On Exercises**

1. **File Creation and Writing**:

   - Write a program to create a file named `student.txt`.
   - Write details like name, age, and grade to the file.

2. **Reading and Counting Words**:

   - Read the contents of a text file and count the number of words in it.

3. **Log File Example**:

   - Create a program to write logs (e.g., "Application started", "Error occurred") to a `log.txt` file.
   - Append new logs instead of overwriting.

4. **File Copy**:

   - Write a program to copy the contents of one file to another file.

5. **Directory Listing**:
   - List all files and subdirectories of a given directory.

---

### **Day 18: File Handling in Java**

Today, we'll explore how to work with files in Java using the **java.io** and **java.nio** packages. You'll learn how to create, read, write, and manipulate files with practical examples.

---

### **1. What Is File Handling?**

**File Handling** in Java allows you to create, read, update, and delete files on the file system. Java provides the following APIs for file operations:

- **`java.io` Package**: Provides classes like `File`, `FileReader`, `FileWriter`, `BufferedReader`, etc.
- **`java.nio` Package**: Modern API for file handling, offering better performance and support for non-blocking I/O.

---

### **2. File Class**

The `File` class in `java.io` represents a file or directory.

#### **Example: Creating a File**

```java
import java.io.File;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        File file = new File("example.txt");
        try {
            if (file.createNewFile()) {
                System.out.println("File created: " + file.getName());
            } else {
                System.out.println("File already exists.");
            }
        } catch (IOException e) {
            System.out.println("An error occurred.");
            e.printStackTrace();
        }
    }
}
```

**Output**:

```
File created: example.txt
```

---

### **3. Writing to a File**

Use the `FileWriter` class to write data to a file.

#### **Example: Writing Text to a File**

```java
import java.io.FileWriter;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try {
            FileWriter writer = new FileWriter("example.txt");
            writer.write("Hello, World!");
            writer.close();
            System.out.println("Successfully wrote to the file.");
        } catch (IOException e) {
            System.out.println("An error occurred.");
            e.printStackTrace();
        }
    }
}
```

**Output**:

```
Successfully wrote to the file.
```

---

### **4. Reading from a File**

Use the `FileReader` or `BufferedReader` class to read data from a file.

#### **Example: Reading Text from a File**

```java
import java.io.FileReader;
import java.io.BufferedReader;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try {
            FileReader reader = new FileReader("example.txt");
            BufferedReader bufferedReader = new BufferedReader(reader);

            String line;
            while ((line = bufferedReader.readLine()) != null) {
                System.out.println(line);
            }

            bufferedReader.close();
        } catch (IOException e) {
            System.out.println("An error occurred.");
            e.printStackTrace();
        }
    }
}
```

**Output**:

```
Hello, World!
```

---

### **5. Deleting a File**

The `delete()` method in the `File` class deletes a file.

#### **Example: Deleting a File**

```java
import java.io.File;

public class Main {
    public static void main(String[] args) {
        File file = new File("example.txt");
        if (file.delete()) {
            System.out.println("Deleted the file: " + file.getName());
        } else {
            System.out.println("Failed to delete the file.");
        }
    }
}
```

**Output**:

```
Deleted the file: example.txt
```

---

### **6. Using java.nio for File Handling**

The `java.nio.file` package provides modern APIs for file handling.

#### **Example: Writing and Reading Files Using `Files` Class**

```java
import java.nio.file.*;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        Path path = Paths.get("example.txt");

        try {
            // Writing to a file
            Files.write(path, "Hello, Java NIO!".getBytes());
            System.out.println("File written successfully.");

            // Reading from a file
            String content = Files.readString(path);
            System.out.println("File content: " + content);
        } catch (IOException e) {
            System.out.println("An error occurred.");
            e.printStackTrace();
        }
    }
}
```

**Output**:

```
File written successfully.
File content: Hello, Java NIO!
```

---

### **7. Common Pitfalls and Best Practices**

#### **Pitfalls**:

1. **File Not Found**: Ensure the file path is correct.
2. **Resource Leaks**: Always close streams to avoid memory leaks.
3. **Permission Issues**: Check file permissions for reading/writing.

#### **Best Practices**:

1. **Use `try-with-resources`**: Automatically close streams.
   ```java
   try (FileWriter writer = new FileWriter("example.txt")) {
       writer.write("Hello, World!");
   }
   ```
2. **Check File Existence**: Use `file.exists()` to verify if a file exists before performing operations.
3. **Prefer `java.nio`**: Use modern APIs for better performance and non-blocking I/O.

---

### **8. Hands-On Exercises**

1. **Create a File**:

   - Write a program to create a file named `data.txt`.

2. **Write and Read**:

   - Write "Java File Handling" to a file and read it back.

3. **List Files in a Directory**:

   - Write a program to list all files in a directory.

4. **Delete a File**:

   - Write a program to delete a file named `temp.txt`.

5. **Copy a File**:
   - Use `Files.copy()` from `java.nio.file` to copy a file from one location to another.

---
