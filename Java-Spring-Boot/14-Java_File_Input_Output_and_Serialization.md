### **Day 14: Java File I/O and Serialization**

Today, we’ll focus on **Java File I/O** and **Serialization**, two essential concepts for reading, writing, and storing data. You'll learn how to interact with files, directories, and streams, and how to serialize and deserialize objects.

---

### **1. File I/O Basics**

Java provides the `java.io` and `java.nio.file` packages for file operations.

#### **Key Classes in File I/O**:

1. **File**: Represents a file or directory path.
2. **FileReader/FileWriter**: For character-based file operations.
3. **BufferedReader/BufferedWriter**: For efficient character input/output.
4. **FileInputStream/FileOutputStream**: For byte-based file operations.

---

### **2. Working with Files**

#### **Example: Creating and Checking Files**

```java
import java.io.File;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        File file = new File("example.txt");

        try {
            // Create a new file
            if (file.createNewFile()) {
                System.out.println("File created: " + file.getName());
            } else {
                System.out.println("File already exists.");
            }

            // Check file properties
            System.out.println("File exists: " + file.exists());
            System.out.println("Is directory: " + file.isDirectory());
            System.out.println("File size: " + file.length() + " bytes");

        } catch (IOException e) {
            System.out.println("An error occurred.");
            e.printStackTrace();
        }
    }
}
```

**Output** (if `example.txt` doesn’t already exist):

```
File created: example.txt
File exists: true
Is directory: false
File size: 0 bytes
```

---

### **3. Writing to a File**

#### **Example: Writing Data**

```java
import java.io.FileWriter;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try (FileWriter writer = new FileWriter("example.txt")) {
            writer.write("Hello, World!\n");
            writer.write("Java File I/O is easy!");
            System.out.println("Data written to file.");
        } catch (IOException e) {
            System.out.println("An error occurred.");
            e.printStackTrace();
        }
    }
}
```

**Output**:
The file `example.txt` will now contain:

```
Hello, World!
Java File I/O is easy!
```

---

### **4. Reading from a File**

#### **Example: Reading Data**

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

**Output** (if `example.txt` contains the previous data):

```
Hello, World!
Java File I/O is easy!
```

---

### **5. Serialization and Deserialization**

**Serialization** is the process of converting an object into a byte stream, and **deserialization** is the reverse process.

#### **Steps for Serialization**:

1. Implement the `Serializable` interface.
2. Use `ObjectOutputStream` to write the object.
3. Use `ObjectInputStream` to read the object.

#### **Example: Serialization**

```java
import java.io.FileOutputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;

class Student implements Serializable {
    private static final long serialVersionUID = 1L; // For version control
    String name;
    int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

public class Main {
    public static void main(String[] args) {
        Student student = new Student("Alice", 20);

        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("student.ser"))) {
            oos.writeObject(student);
            System.out.println("Student object serialized.");
        } catch (Exception e) {
            System.out.println("An error occurred.");
            e.printStackTrace();
        }
    }
}
```

---

#### **Example: Deserialization**

```java
import java.io.FileInputStream;
import java.io.ObjectInputStream;

public class Main {
    public static void main(String[] args) {
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("student.ser"))) {
            Student student = (Student) ois.readObject();
            System.out.println("Student Name: " + student.name);
            System.out.println("Student Age: " + student.age);
        } catch (Exception e) {
            System.out.println("An error occurred.");
            e.printStackTrace();
        }
    }
}
```

**Output**:

```
Student Name: Alice
Student Age: 20
```

---

### **6. Common Pitfalls and Best Practices**

#### **Pitfalls**:

1. **Forgetting `close()`**: Always close file resources. Use `try-with-resources` to handle this automatically.
2. **Overwriting Files**: Writing to an existing file will overwrite its contents unless you explicitly append data.

#### **Best Practices**:

1. **Use Buffered Streams**: For performance, wrap streams with `BufferedReader` or `BufferedWriter`.
2. **Check File Existence**: Always verify file existence before reading.
3. **Use Serialization Safely**: Always include a `serialVersionUID` for versioning serialized classes.

---

### **7. Hands-On Exercises**

1. **File Creation and Properties**:

   - Create a file named `notes.txt` and display its properties (e.g., name, size, and path).

2. **Reading and Writing**:

   - Write a program to read from one file and copy its content into another file.

3. **Serialization Practice**:

   - Serialize an object containing a list of students with their names and marks.
   - Deserialize the object and print the data.

4. **Directory Operations**:
   - Write a program to list all files and subdirectories within a given directory.

---
