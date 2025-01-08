### **Day 22: Introduction to Spring Boot and Setting Up a Basic Application**

Today, we’ll explore **Spring Boot**, a framework built on top of the Spring Framework to simplify the development of Java applications. You’ll learn about its features, how it works, and how to create your first Spring Boot application.

---

### **1. What is Spring Boot?**

Spring Boot simplifies Spring-based application development by:

1. **Auto-Configuration**: Reduces the need for manual configurations.
2. **Embedded Servers**: Includes built-in web servers like Tomcat, Jetty, or Undertow.
3. **Starter Dependencies**: Predefined dependencies for common functionalities (e.g., `spring-boot-starter-web`).
4. **Production-Ready Features**: Includes monitoring, metrics, and health checks via Actuator.

---

### **2. Why Use Spring Boot?**

#### **Key Advantages**:

- **Simplifies Configuration**: Uses sensible defaults.
- **Standalone Applications**: Runs directly without an external server.
- **Rapid Development**: Reduces boilerplate code.
- **Integration**: Easy integration with databases, security frameworks, and messaging systems.

---

### **3. Setting Up a Spring Boot Project**

#### **Step 1: Using Spring Initializr**

1. Go to [Spring Initializr](https://start.spring.io/).
2. Configure the project:
   - Project: **Maven** (or Gradle)
   - Language: **Java**
   - Spring Boot Version: **3.1.x**
   - Dependencies: Add **Spring Web**
3. Download the generated project and unzip it.

---

#### **Step 2: Maven/Gradle Configuration**

**Maven `pom.xml`:**

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.1.2</version>
</parent>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

**Gradle `build.gradle`:**

```groovy
plugins {
    id 'org.springframework.boot' version '3.1.2'
    id 'io.spring.dependency-management' version '1.1.3'
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
}
```

---

#### **Step 3: Project Structure**

Spring Boot projects follow a standard structure:

```
src/main/java
  └── com.example.demo
      ├── DemoApplication.java (Main Class)
      ├── controllers (Controller Layer)
      ├── services (Business Logic Layer)
      └── models (Data Models)
src/main/resources
  ├── application.properties (Configuration File)
  └── static (Static Content)
```

---

### **4. Create Your First Spring Boot Application**

#### **Step 1: Main Class**

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

#### **Explanation**:

- **`@SpringBootApplication`**: Combines `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan`.
- **`SpringApplication.run()`**: Starts the application.

---

### **5. Creating a REST Controller**

#### **Step 1: Define a REST Endpoint**

```java
package com.example.demo.controllers;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, Spring Boot!";
    }
}
```

#### **Explanation**:

- **`@RestController`**: Combines `@Controller` and `@ResponseBody`.
- **`@GetMapping`**: Maps HTTP GET requests to the `sayHello()` method.

---

### **6. Run the Application**

1. Open a terminal in the project directory.
2. Run the application:
   - **Maven**: `./mvnw spring-boot:run`
   - **Gradle**: `./gradlew bootRun`
3. Open your browser and go to `http://localhost:8080/hello`.

**Output**:

```
Hello, Spring Boot!
```

---

### **7. Understanding Application Properties**

Spring Boot uses the `application.properties` (or `application.yml`) file for configuration.

#### **Examples**:

```properties
# Change the default server port
server.port=9090

# Add custom application properties
app.name=DemoApplication
```

Restart the application and visit `http://localhost:9090/hello`.

---

### **8. Common Spring Boot Annotations**

1. **`@SpringBootApplication`**: Entry point for a Spring Boot application.
2. **`@RestController`**: Combines `@Controller` and `@ResponseBody`.
3. **`@GetMapping`, `@PostMapping`**: Map HTTP requests to controller methods.
4. **`@Autowired`**: Injects dependencies automatically.
5. **`@Component`**: Marks a class as a Spring-managed component.

---

### **9. Debugging Tips and Best Practices**

#### **Debugging Tips**:

1. Check logs for detailed error messages.
2. Use **Spring Boot DevTools** for automatic restarts during development:
   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-devtools</artifactId>
       <scope>runtime</scope>
   </dependency>
   ```
3. Enable detailed logging in `application.properties`:
   ```properties
   logging.level.org.springframework=DEBUG
   ```

#### **Best Practices**:

1. Keep the project structure clean (controllers, services, and models).
2. Use meaningful endpoint names.
3. Avoid hardcoding values; use properties or environment variables.

---

### **10. Hands-On Exercises**

1. **Create a Custom Endpoint**:

   - Add a new endpoint `/greet` that accepts a query parameter (e.g., `/greet?name=John`) and returns a greeting message.

2. **Change the Default Port**:

   - Modify the application to run on port 8081.

3. **Return JSON Response**:

   - Create a controller that returns a JSON response instead of plain text.

4. **Use DevTools**:
   - Add `spring-boot-devtools` and observe automatic application restarts.

---
