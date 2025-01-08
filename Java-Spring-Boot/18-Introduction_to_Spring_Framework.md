### **Day 21: Introduction to Spring Framework and Dependency Injection**

Today, we’ll begin our journey into **Spring Framework** and **Spring Boot** by understanding the core concepts of the Spring Framework. You’ll learn about **Dependency Injection (DI)** and how Spring simplifies application development.

---

### **1. What is the Spring Framework?**

The **Spring Framework** is a powerful and flexible Java framework for building enterprise-level applications. It is modular, meaning you can use only the parts you need.

#### **Key Features of Spring Framework**:

1. **Dependency Injection**: Manages object dependencies.
2. **Aspect-Oriented Programming (AOP)**: Separates cross-cutting concerns like logging and security.
3. **Data Access**: Simplifies interaction with databases.
4. **Spring MVC**: Provides tools for building web applications.
5. **Integration**: Works seamlessly with frameworks like Hibernate, Kafka, and RabbitMQ.

---

### **2. What is Dependency Injection (DI)?**

**Dependency Injection** is a design pattern where an object’s dependencies are provided by an external system rather than being created by the object itself.

#### **Why Use DI?**

- **Loose Coupling**: Promotes modularity and testability.
- **Easier Maintenance**: Dependencies are managed externally, making it easier to replace or update them.
- **Improved Testability**: Mock dependencies can be easily injected for testing.

#### **How DI Works in Spring?**

Spring provides two ways to achieve DI:

1. **XML Configuration** (older style, rarely used now)
2. **Java-based Configuration** (modern approach)

---

### **3. Setting Up a Spring Application**

#### **Step 1: Add Spring Dependencies**

Using Maven:

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.25</version>
</dependency>
```

Using Gradle:

```groovy
implementation 'org.springframework:spring-context:5.3.25'
```

---

#### **Step 2: Create a Spring Project**

Create a simple Java project with the following structure:

```
src/main/java
  └── com.example
      ├── AppConfig.java
      ├── Service.java
      └── Main.java
```

---

### **4. Example: Dependency Injection with Spring**

#### **Step 1: Define a Service**

```java
package com.example;

public class Service {
    public void serve() {
        System.out.println("Service is serving...");
    }
}
```

#### **Step 2: Create a Configuration Class**

```java
package com.example;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {
    @Bean
    public Service service() {
        return new Service();
    }
}
```

#### **Step 3: Use the Spring Context to Get Beans**

```java
package com.example;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

        // Get the Service bean
        Service service = context.getBean(Service.class);
        service.serve();
    }
}
```

**Output**:

```
Service is serving...
```

---

### **5. Spring Annotations for DI**

1. **`@Component`**: Marks a class as a Spring-managed component.

   ```java
   @Component
   public class Service {
       public void serve() {
           System.out.println("Service is serving...");
       }
   }
   ```

2. **`@Autowired`**: Automatically wires dependencies.

   ```java
   @Component
   public class Consumer {
       private final Service service;

       @Autowired
       public Consumer(Service service) {
           this.service = service;
       }

       public void process() {
           service.serve();
       }
   }
   ```

3. **`@Configuration` and `@Bean`**: Define beans explicitly.

   ```java
   @Configuration
   public class AppConfig {
       @Bean
       public Service service() {
           return new Service();
       }
   }
   ```

4. **`@ComponentScan`**: Automatically scans for Spring components.
   ```java
   @Configuration
   @ComponentScan(basePackages = "com.example")
   public class AppConfig {
   }
   ```

---

### **6. Common DI Scopes in Spring**

1. **`singleton`** (default): One instance per Spring context.
2. **`prototype`**: A new instance is created each time the bean is requested.
3. **Request and Session Scopes**: Used in web applications.

#### **Example: Setting Scope**

```java
@Bean
@Scope("prototype")
public Service service() {
    return new Service();
}
```

---

### **7. Common Pitfalls and Best Practices**

#### **Pitfalls**:

1. **Overuse of DI**: Can make simple applications unnecessarily complex.
2. **Improper Scope Management**: Mismanaging scopes can lead to performance issues.
3. **Tight Coupling with Framework**: Excessive use of Spring-specific annotations can reduce portability.

#### **Best Practices**:

1. Prefer **Constructor Injection** over field injection for better testability.
2. Use **Java-based Configuration** for modern applications.
3. Keep configuration and business logic separate.

---

### **8. Hands-On Exercises**

1. **Basic DI**:

   - Create a Spring application with a `Service` bean injected into a `Consumer` class.

2. **Component Scanning**:

   - Use `@ComponentScan` to detect beans automatically.

3. **Scope Experiment**:

   - Create two beans with different scopes (`singleton` and `prototype`) and observe their behavior.

4. **Custom Configuration**:
   - Define beans in a configuration class using `@Configuration` and `@Bean`.

---
