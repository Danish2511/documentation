### **Day 23: Building REST APIs with Spring Boot**

Today, we’ll dive deeper into **Spring Boot REST APIs**. You’ll learn how to create endpoints, handle HTTP requests, return JSON responses, and use path variables and query parameters. This lesson will include practical examples and exercises to solidify your understanding.

---

### **1. What is a REST API?**

A **REST API (Representational State Transfer API)** is a way to build lightweight web services using HTTP methods:

- **GET**: Retrieve data.
- **POST**: Create new resources.
- **PUT**: Update existing resources.
- **DELETE**: Delete resources.

**Key Features of REST APIs**:

1. Stateless: Each request is independent.
2. Resource-based: Identifies resources using URLs.
3. Uses standard HTTP methods and status codes.

---

### **2. Setting Up the Project**

1. Start a new Spring Boot project using **Spring Initializr**.
2. Add the **Spring Web** dependency.

---

### **3. Creating REST Endpoints**

#### **Step 1: Basic GET Endpoint**

```java
package com.example.demo.controllers;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {

    @GetMapping("/greeting")
    public String greeting() {
        return "Hello, REST API!";
    }
}
```

- **`@RestController`**: Indicates this class will handle REST requests.
- **`@GetMapping("/greeting")`**: Maps HTTP GET requests to the `greeting()` method.

---

#### **Step 2: Returning JSON Response**

```java
package com.example.demo.controllers;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.HashMap;
import java.util.Map;

@RestController
public class GreetingController {

    @GetMapping("/json-greeting")
    public Map<String, String> jsonGreeting() {
        Map<String, String> response = new HashMap<>();
        response.put("message", "Hello, REST API!");
        return response;
    }
}
```

**Output**:

```json
{
  "message": "Hello, REST API!"
}
```

---

#### **Step 3: Using Path Variables**

```java
package com.example.demo.controllers;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {

    @GetMapping("/greet/{name}")
    public String greetByName(@PathVariable String name) {
        return "Hello, " + name + "!";
    }
}
```

- **`@PathVariable`**: Binds a path segment to a method parameter.
- Example URL: `http://localhost:8080/greet/John`
- **Output**: `Hello, John!`

---

#### **Step 4: Using Query Parameters**

```java
package com.example.demo.controllers;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {

    @GetMapping("/custom-greet")
    public String customGreet(@RequestParam(defaultValue = "Guest") String name) {
        return "Welcome, " + name + "!";
    }
}
```

- **`@RequestParam`**: Extracts query parameters from the URL.
- Example URL: `http://localhost:8080/custom-greet?name=Alice`
- **Output**: `Welcome, Alice!`

---

### **4. Creating a POST Endpoint**

#### **Step 1: Accepting JSON Input**

```java
package com.example.demo.controllers;

import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {

    @PostMapping("/create-greeting")
    public String createGreeting(@RequestBody Map<String, String> requestBody) {
        return "Greeting created for: " + requestBody.get("name");
    }
}
```

- **`@PostMapping`**: Maps HTTP POST requests.
- **`@RequestBody`**: Maps the JSON body to a Java object.

**Input**:

```json
{
  "name": "Alice"
}
```

**Output**:

```
Greeting created for: Alice
```

---

### **5. HTTP Status Codes and Responses**

#### Setting HTTP Status Codes

Spring Boot allows you to customize HTTP status codes using:

1. **`ResponseEntity`**:

   ```java
   @PostMapping("/status-example")
   public ResponseEntity<String> statusExample() {
       return new ResponseEntity<>("Resource created", HttpStatus.CREATED);
   }
   ```

2. **Annotations**:
   ```java
   @PostMapping("/example")
   @ResponseStatus(HttpStatus.CREATED)
   public void example() {
       // Business logic here
   }
   ```

#### Common HTTP Status Codes

- **200 OK**: Request succeeded.
- **201 Created**: Resource created successfully.
- **400 Bad Request**: Invalid request data.
- **404 Not Found**: Resource not found.
- **500 Internal Server Error**: Server encountered an error.

---

### **6. Best Practices for REST API Development**

1. Use meaningful and consistent endpoint names:

   - Good: `/users`, `/users/{id}`
   - Bad: `/doStuff`, `/fetchData`

2. Stick to HTTP method semantics:

   - **GET**: Retrieve data.
   - **POST**: Create new resources.
   - **PUT/PATCH**: Update resources.
   - **DELETE**: Remove resources.

3. Use proper status codes to communicate outcomes.

4. Validate user input using tools like **Hibernate Validator**.

5. Document your APIs using tools like **Swagger** or **SpringDoc OpenAPI**.

---

### **7. Debugging Tips**

1. Enable detailed logs for HTTP requests in `application.properties`:

   ```properties
   logging.level.org.springframework.web=DEBUG
   ```

2. Use tools like **Postman** or **cURL** to test APIs.

3. Inspect JSON responses to verify data correctness.

---

### **8. Hands-On Exercises**

1. **Basic CRUD**:

   - Create endpoints for a simple resource, such as "Books."
     - **GET /books**: List all books.
     - **POST /books**: Add a new book.
     - **GET /books/{id}**: Retrieve a book by ID.
     - **DELETE /books/{id}**: Delete a book.

2. **Error Handling**:

   - Add proper error messages and status codes for invalid inputs or missing resources.

3. **Query Parameters**:
   - Create an endpoint that filters resources based on query parameters (e.g., `/books?author=John`).

---
