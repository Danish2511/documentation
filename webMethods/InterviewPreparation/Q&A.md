### **1. What is webMethods?**

**Answer:**
webMethods is an **integration platform** developed by Software AG. It helps businesses connect different applications, systems, and data sources to work together seamlessly.

**Example:**
Imagine a company uses Salesforce for CRM and SAP for accounting. webMethods can integrate these two systems so that when a new customer is added in Salesforce, their details are automatically sent to SAP for invoicing.

---

### **2. What are the main components of webMethods?**

**Answer:**
The main components are:

1. **Integration Server**: The core engine that processes integrations.
2. **webMethods Designer**: A tool to design integration workflows.
3. **Universal Messaging**: For real-time messaging between systems.
4. **Trading Networks**: For B2B integrations (e.g., EDI).
5. **Adapters**: Pre-built connectors for systems like SAP, Oracle, etc.
6. **Monitor**: To track and analyze integration performance.

**Example:**
If you want to integrate an e-commerce website with a payment gateway, you’d use the Integration Server to create the workflow and an adapter to connect to the payment system.

---

### **3. What is a Pipeline in webMethods?**

**Answer:**
A **pipeline** is a temporary storage area in webMethods that holds data as it moves between services. It consists of **pipeline variables** that store input/output data.

**Example:**
If you’re processing an order, the pipeline might hold the order ID, customer name, and payment details as the data flows from one service to another.

---

### **4. What is a Service in webMethods?**

**Answer:**
A **service** is a reusable piece of code or functionality in webMethods. It performs a specific task, like transforming data or calling an external API.

**Example:**
A service could be created to convert a JSON request into an XML format before sending it to another system.

---

### **5. What is the difference between a Flow Service and a Java Service?**

**Answer:**

- **Flow Service**: Built using webMethods Designer. It’s a graphical way to create integrations using drag-and-drop components.
- **Java Service**: Written in Java code for more complex logic that cannot be achieved with Flow Services.

**Example:**
If you need to perform a simple data transformation, use a Flow Service. For advanced calculations or custom logic, use a Java Service.

---

### **6. What are Adapters in webMethods?**

**Answer:**
Adapters are pre-built connectors that allow webMethods to communicate with external systems like databases, ERP systems, or cloud applications.

**Example:**
The JDBC Adapter connects webMethods to a database like MySQL or Oracle to fetch or update data.

---

### **7. What is IS (Integration Server) in webMethods?**

**Answer:**
The **Integration Server** is the heart of webMethods. It executes integration workflows, processes data, and manages communication between systems.

**Example:**
When an order is placed on an e-commerce site, the Integration Server processes the order, updates the inventory, and sends a confirmation email.

---

### **8. What is Pub-Sub in webMethods?**

**Answer:**
**Pub-Sub (Publish-Subscribe)** is a messaging pattern where one system (publisher) sends a message, and multiple systems (subscribers) receive it.

**Example:**
In a stock trading system, when a stock price changes, the publisher sends the update, and all subscribed systems (like trading apps) receive the new price.

---

### **9. What is a Trigger in webMethods?**

**Answer:**
A **trigger** is an event that starts a workflow or service in webMethods. It can be time-based (e.g., every hour) or event-based (e.g., when a file is uploaded).

**Example:**
A trigger could be set to run a service every night at 12 AM to generate a daily sales report.

---

### **10. What is the difference between webMethods and other integration tools like MuleSoft or Dell Boomi?**

**Answer:**

- **webMethods**: Known for its robustness, scalability, and support for complex integrations, especially in enterprise environments.
- **MuleSoft**: Focuses on API-led connectivity and is cloud-native.
- **Dell Boomi**: A low-code platform with a strong focus on ease of use and quick deployments.

**Example:**
If you’re working in a large enterprise with legacy systems, webMethods might be a better choice. For cloud-based integrations, MuleSoft or Boomi could be more suitable.

---

### **11. How do you handle errors in webMethods?**

**Answer:**
Errors can be handled using:

1. **Try-Catch blocks** in Flow Services.
2. **Error Handlers** to define custom error responses.
3. **Retry mechanisms** for transient errors.

**Example:**
If a service fails to connect to a database, you can use a Try-Catch block to log the error and retry the connection after 5 minutes.

---

### **12. What is Trading Networks in webMethods?**

**Answer:**
**Trading Networks** is a module for B2B integrations. It supports protocols like EDI, AS2, and RosettaNet for exchanging business documents.

**Example:**
A retail company can use Trading Networks to send purchase orders to suppliers using EDI.

---

### **13. What is the difference between SOAP and REST in webMethods?**

**Answer:**

- **SOAP**: A protocol for exchanging structured information using XML. It’s more rigid but supports advanced features like security.
- **REST**: A lightweight architecture using HTTP methods (GET, POST, etc.). It’s simpler and faster.

**Example:**
If you’re integrating with a legacy system that requires high security, use SOAP. For modern APIs, REST is preferred.

---

### **14. What is a Document Type in webMethods?**

**Answer:**
A **Document Type** defines the structure of data (like XML or JSON) used in webMethods. It ensures data is correctly formatted and validated.

**Example:**
If you’re sending customer data, you can define a Document Type with fields like `customerID`, `name`, and `email`.

---

### **15. How do you optimize performance in webMethods?**

**Answer:**

- Use **caching** for frequently accessed data.
- Optimize **database queries**.
- Use **asynchronous services** for long-running tasks.
- Monitor and tune the **Integration Server**.

**Example:**
If a service fetches product details from a database, cache the results to avoid repeated queries.

---

### **16. What is the difference between webMethods Developer and Designer?**

**Answer:**

- **webMethods Developer**: A modern IDE for developing integrations (replaces Designer).
- **webMethods Designer**: The older tool for creating Flow Services and integrations.

**Example:**
If you’re starting a new project, use webMethods Developer for better features and support.

---

### **17. What is a Broker in webMethods?**

**Answer:**
The **Broker** is part of Universal Messaging. It handles message queuing and ensures reliable delivery between systems.

**Example:**
In a banking system, the Broker ensures that transaction messages are delivered even if one system is temporarily down.

---

### **18. What is the difference between webMethods and SAP PI/PO?**

**Answer:**

- **webMethods**: A general-purpose integration platform.
- **SAP PI/PO**: Specifically designed for SAP integrations.

**Example:**
If your organization uses SAP extensively, SAP PI/PO might be a better fit. For non-SAP integrations, webMethods is more versatile.

---

### **19. What is a WmPublic package?**

**Answer:**
**WmPublic** is a default package in webMethods that contains reusable services for common tasks like string manipulation, date formatting, etc.

**Example:**
If you need to convert a date format, you can use the `pub.date:dateToString` service from WmPublic.

---

### **20. How do you secure webMethods integrations?**

**Answer:**

- Use **SSL/TLS** for secure communication.
- Implement **authentication** (e.g., OAuth, API keys).
- Use **encryption** for sensitive data.
- Regularly update and patch the platform.

**Example:**
When sending credit card details, encrypt the data and use HTTPS to ensure security.

---

### **Tips for the Interview:**

1. **Understand the Basics**: Be clear on core concepts like Integration Server, Flow Services, and Adapters.
2. **Practice Real-World Scenarios**: Think of examples where webMethods can solve integration challenges.
3. **Be Honest**: If you don’t know something, admit it and show willingness to learn.

---

### **1. What is webMethods Adapter for JDBC?**

**Answer:**
The **webMethods Adapter for JDBC** is an add-on to the webMethods Integration Server that allows seamless and real-time communication with relational databases using a JDBC driver. It enables Integration Server clients to create and run services that perform database operations like retrieving, inserting, and updating data.

**Example:**
If you need to fetch customer details from a MySQL database, you can use the JDBC Adapter to create a service that executes a SQL query and returns the results.

---

### **2. Explain the Architecture Overview of Adapter for JDBC.**

**Answer:**
The **Adapter for JDBC** provides a set of user interfaces (UI), services, and templates to integrate with databases using a JDBC driver. It consists of the following components:

1. **Adapter Connections**: Enable Integration Server to connect to the database at runtime.
2. **Adapter Services**: Allow Integration Server to perform database operations (e.g., SELECT, INSERT, UPDATE).
3. **Adapter Notifications**: Monitor the database and notify Integration Server when specific actions occur (e.g., a new record is added).

**Example:**
You can configure an adapter connection to connect to an Oracle database, create an adapter service to fetch employee details, and set up an adapter notification to trigger a workflow when a new employee is added.

---

### **3. What are JDBC Drivers?**

**Answer:**
**JDBC Drivers** are required to connect to a relational database using the JDBC Adapter. There are two ways to configure JDBC Drivers:

1. **DataDirect Drivers**: Provided by Software AG. You specify the DataDirect driver in the Data Source Class section of the JDBC Adapter connection.
2. **Custom JDBC Drivers**: Place the database-specific JAR files in the Integration Server directory (`/instances/instance name/WmJDBCAdapter/code/jars`) and specify the Data Source Class.

**Example:**
To connect to a PostgreSQL database, you can either use the DataDirect driver provided by Software AG or place the PostgreSQL JDBC JAR file in the specified directory.

---

### **4. What are the Transaction Types in JDBC Connections?**

**Answer:**
There are three transaction types in JDBC Connections:

1. **NO_TRANSACTION**: No transaction control. Operations are auto-committed.
2. **LOCAL_TRANSACTION**: All operations on the same connection are committed or rolled back together within a transaction boundary.
3. **XA_TRANSACTION**: Supports two-phase commits across multiple databases. All operations on multiple connections are committed or rolled back together.

**Example:**

- Use **NO_TRANSACTION** for read-only operations.
- Use **LOCAL_TRANSACTION** for batch operations where you need to commit or roll back multiple SQL statements together.
- Use **XA_TRANSACTION** for distributed transactions involving multiple databases.

---

### **5. When to Use LOCAL_TRANSACTION?**

**Answer:**
Use **LOCAL_TRANSACTION** when:

- You need to perform batch operations (e.g., multiple INSERT/UPDATE/DELETE statements).
- You want Integration Server to control when to commit or roll back the transaction.
- You need to ensure that all operations succeed before committing.

**Example:**
If you’re inserting 100 records into a database, use LOCAL_TRANSACTION to ensure that all records are inserted successfully. If one record fails, the entire transaction is rolled back.

---

### **6. When to Use NO_TRANSACTION?**

**Answer:**
Use **NO_TRANSACTION** when:

- You are performing read-only operations (e.g., SELECT queries).
- You don’t need transaction control (e.g., auto-commit is acceptable).

**Example:**
If you’re fetching a list of products from a database, use NO_TRANSACTION since no data modification is involved.

---

### **7. Why Do We Use Boundaries?**

**Answer:**
**Boundaries** are used to explicitly define the start, commit, and rollback points of a transaction. This gives the developer full control over the transaction instead of the adapter.

**Example:**
In a flow service, you can set a boundary to start a transaction, perform multiple database operations, and then commit the transaction only if all operations succeed.

---

### **8. Explain the Order of Events While Using Adapter Service.**

**Answer:**
The steps to use an Adapter Service are:

1. **Create JDBC Adapter Connection**: Configure the connection to the database.
2. **Create Database Table**: Ensure the table exists in the database.
3. **Create and Configure JDBC Adapter Service**: Define the service to perform the required database operation.
4. **Select the Table**: Choose the table to interact with in the Adapter Service.
5. **Provide Inputs and Outputs**: Define the input and output parameters for the service.
6. **Run the Service**: Execute the service to perform the database operation.

**Example:**
To update employee details:

1. Create a JDBC Adapter connection to the HR database.
2. Ensure the `Employees` table exists.
3. Create an Adapter Service to update the `Employees` table.
4. Select the `Employees` table in the service.
5. Provide the employee ID and new details as inputs.
6. Run the service to update the database.

---

### **9. What is the Difference Between NO_TRANSACTION and LOCAL_TRANSACTION?**

**Answer:**

- **NO_TRANSACTION**: No transaction control. Each operation is auto-committed immediately.
- **LOCAL_TRANSACTION**: Provides transaction control. All operations within a transaction boundary are committed or rolled back together.

**Example:**

- Use **NO_TRANSACTION** for reading data (e.g., SELECT queries).
- Use **LOCAL_TRANSACTION** for writing data (e.g., INSERT/UPDATE/DELETE) where you need to ensure all operations succeed before committing.

---

### **10. What Happens if a Batch Operation Fails in LOCAL_TRANSACTION?**

**Answer:**
If a batch operation fails in **LOCAL_TRANSACTION**, all changes made within the transaction boundary are rolled back. This ensures data consistency.

**Example:**
If you’re inserting 100 records and the 50th record fails, all 49 previously inserted records are rolled back.

---

### **11. Can Batch Operations Be Configured with NO_TRANSACTION?**

**Answer:**
No, batch operations **cannot** be configured with **NO_TRANSACTION** because each operation is auto-committed immediately. If one operation fails, there’s no way to roll back the previous operations.

**Example:**
If you’re inserting 100 records with NO_TRANSACTION and the 50th record fails, the first 49 records are already committed, leading to data inconsistency.

---

### **12. What Are the Advantages of Using XA_TRANSACTION?**

**Answer:**
**XA_TRANSACTION** supports distributed transactions across multiple databases. It ensures that all operations across different databases are committed or rolled back together.

**Example:**
If you’re updating records in both an Oracle database and a MySQL database, XA_TRANSACTION ensures that both updates are committed only if both succeed. If one fails, both are rolled back.

---

### **13. What Are the Key Considerations When Using JDBC Adapter?**

**Answer:**

- Ensure the correct JDBC driver is configured.
- Use appropriate transaction types based on the operation (read vs. write).
- Optimize database queries for performance.
- Handle errors and exceptions gracefully.

**Example:**
When fetching large datasets, use pagination to avoid performance issues.

---

### **1. Explain the Order of Events While Using Adapter Notifications.**

**Answer:**
Adapter Notifications are used to monitor database changes and trigger workflows in webMethods. Here’s the order of events:

1. **Create Adapter Notification**: Use the JDBC Adapter connection to create a notification.
2. **Publishable Document**: A new publishable document is automatically created in the same folder.
3. **Polling Notification**: A new polling notification is created in the IS Admin Console.
4. **Enable Polling Notification**: Enable the polling notification in the IS Console.
5. **Buffer Table & Trigger**: A buffer table and trigger are created in the database.
6. **Publishing Flow Service**: Create a flow service to modify the database table.
7. **Subscribing Service**: Create a subscribing service to handle the notification.
8. **Trigger in Designer**: Create a trigger in Designer using the publishable document and subscriber.

**Flow of Events:**

- When the publishing service modifies the database table, the database trigger is activated.
- The buffer table is updated, and the publishable document is modified.
- The Integration Server trigger notifies the subscriber, which then processes the changes.

**Example:**
If a new order is added to the `Orders` table, the database trigger updates the buffer table, and the subscriber service sends a confirmation email to the customer.

---

### **2. What is the Difference Between Custom SQL and Dynamic SQL?**

**Answer:**

- **Custom SQL**: The SQL query is fixed at design time. Input variables are provided at design time.
- **Dynamic SQL**: The SQL query is not fixed and can be constructed dynamically at runtime.

**Key Differences:**

- **Custom SQL**: Faster because it is pre-compiled. Used when the SQL query is static.
- **Dynamic SQL**: More versatile because the query can change at runtime. Used when the SQL query needs to be dynamic.

**Example:**

- **Custom SQL**: `SELECT * FROM Employees WHERE Department = :DeptName` (fixed query).
- **Dynamic SQL**: A query constructed at runtime based on user input, e.g., `SELECT * FROM Employees WHERE Department = 'Sales' AND Age > 30`.

---

### **3. What is a Stored Procedure?**

**Answer:**
A **stored procedure** is a precompiled SQL code stored in the database. It can be reused and executed multiple times. Stored procedures can accept parameters and perform complex database operations.

**Uses:**

- Data validation.
- Access control.
- Centralizing business logic in the database.

**Example:**
A stored procedure `GetEmployeeDetails` can be created to fetch employee details based on an employee ID.

---

### **4. What is a Stored Procedure with Signature Service?**

**Answer:**
A **StoredProcedureWithSignature** service automatically introspects the stored procedure’s parameters at design time. This eliminates the need to manually specify parameters.

**Advantages:**

- Parameters are automatically detected.
- Parameters can be changed at runtime.

**Example:**
If a stored procedure `UpdateSalary` has parameters `EmployeeID` and `NewSalary`, the StoredProcedureWithSignature service will automatically detect these parameters.

---

### **5. What is a Stored Procedure Without Signature Service?**

**Answer:**
A **StoredProcedureWithoutSignature** service requires manual specification of parameters at design time. Parameters cannot be changed at runtime.

**Example:**
For the same `UpdateSalary` stored procedure, you must manually specify `EmployeeID` and `NewSalary` as input parameters in the adapter service.

---

### **6. Explain Built-in Transaction Management Services.**

**Answer:**
Built-in transaction management services in webMethods allow you to define explicit transaction boundaries in flow services. These services are part of the `WMArt` package.

**Key Services:**

1. **startTransaction**: Starts a new transaction.
2. **commitTransaction**: Commits the transaction.
3. **rollbackTransaction**: Rolls back the transaction.

**Example:**

```plaintext
1. startTransaction
2. Perform database operations (e.g., INSERT, UPDATE).
3. If all operations succeed, commitTransaction.
4. If any operation fails, rollbackTransaction.
```

**Use Case:**
When performing multiple database operations in a single flow service, use transaction management to ensure all operations are committed or rolled back together.

---

### **7. How Do You Use Built-in Transaction Management Services?**

**Answer:**

1. **Start a Transaction**: Use `startTransaction` to begin a transaction.
2. **Perform Operations**: Execute database operations within the transaction boundary.
3. **Commit or Rollback**: Use `commitTransaction` to save changes or `rollbackTransaction` to undo changes if an error occurs.

**Example:**

```plaintext
1. startTransaction
2. Insert a new record into the `Orders` table.
3. Update the `Inventory` table.
4. If both operations succeed, commitTransaction.
5. If any operation fails, rollbackTransaction.
```

---

### **8. What Are the Advantages of Using Stored Procedures?**

**Answer:**

- **Reusability**: The same code can be executed multiple times.
- **Performance**: Stored procedures are precompiled, reducing execution time.
- **Security**: Access control can be implemented at the database level.
- **Centralization**: Business logic is centralized in the database.

**Example:**
A stored procedure `CalculateBonus` can be used to calculate employee bonuses based on performance metrics.

---

### **9. What Are the Limitations of Stored Procedures in webMethods?**

**Answer:**

- **Array/Struct Parameters**: Stored procedures with array or struct as OUT parameters are not supported.
- **MySQL Functions**: Stored procedure services do not support MySQL functions.

**Example:**
If a stored procedure returns an array, it cannot be used directly in webMethods.

---

### **10. When to Use Stored Procedures vs. Custom/Dynamic SQL?**

**Answer:**

- **Stored Procedures**: Use for complex, reusable database operations.
- **Custom SQL**: Use for simple, fixed queries.
- **Dynamic SQL**: Use for queries that need to be constructed dynamically at runtime.

**Example:**

- Use a stored procedure to calculate taxes.
- Use Custom SQL to fetch a list of products.
- Use Dynamic SQL to construct a search query based on user input.

---

### **11. What Are the Key Considerations When Using Adapter Notifications?**

**Answer:**

- Ensure the buffer table and trigger are correctly configured in the database.
- Use polling notifications for real-time monitoring.
- Handle errors and exceptions in the subscribing service.

**Example:**
If the buffer table is not updated correctly, the notification may not trigger the subscriber service.

---

### **12. What Are the Performance Implications of Dynamic SQL?**

**Answer:**

- **Dynamic SQL** is slower than **Custom SQL** because it is compiled at runtime.
- Use Dynamic SQL only when necessary to avoid performance bottlenecks.

**Example:**
If you need to construct a query based on user input, use Dynamic SQL. For fixed queries, use Custom SQL.

---

### **1. Explain Implicit and Explicit Transactions.**

**Answer:**

- **Implicit Transactions**: These are automatically managed by the Integration Server transaction manager. No explicit boundaries are defined. The transaction starts and ends automatically based on the adapter configuration (NO, LOCAL, or XA Transaction without boundaries).
- **Explicit Transactions**: These are manually defined by the developer using built-in services from the `WmART` package. Boundaries are explicitly set to define where the transaction starts, commits, and ends. Explicit transactions can only be configured with LOCAL or XA Transaction with boundaries.

**Example:**

- **Implicit Transaction**: When using NO_TRANSACTION, each SQL statement is auto-committed without any manual control.
- **Explicit Transaction**: Use `startTransaction`, `commitTransaction`, and `rollbackTransaction` services to control the transaction boundaries in a flow service.

---

### **2. How Does Class Loading Work in webMethods?**

**Answer:**
Class loading in webMethods determines how and when Java classes (e.g., JDBC drivers) are loaded into the Integration Server. The location where you place the JAR files affects their availability and scope.

---

### **3. Why Place Adapter Drivers in `/instances/instance_name/WmJDBCAdapter/code/jars`?**

**Answer:**
Placing JDBC drivers in `/instances/instance_name/WmJDBCAdapter/code/jars` ensures that only the **JDBC Adapter package** has access to the drivers. This provides better isolation and avoids conflicts with other packages or instances.

**Advantages:**

- **Isolation**: Drivers are only available to the JDBC Adapter package.
- **Ease of Deployment**: Drivers are loaded when the package is loaded.

---

### **4. Why Not Place Drivers in `WmJDBCAdapter/code/static/jars`?**

**Answer:**
If drivers are placed in `WmJDBCAdapter/code/static/jars`, they become available to **all packages in the instance**. This can lead to conflicts or unintended usage of the drivers by other packages.

**Disadvantages:**

- **Lack of Isolation**: Drivers are accessible to all packages.
- **Deployment Complexity**: Drivers are loaded before the packages, which may cause issues during deployment.

---

### **5. Why Not Place Drivers in `IntegrationServer\instances\default\lib\jars\custom`?**

**Answer:**
Placing drivers in `IntegrationServer\instances\default\lib\jars\custom` makes them available to **all packages in the instance**. However, the drivers are only accessible after the package is loaded, which can complicate deployments.

**Disadvantages:**

- **Deployment Hardness**: Drivers are only available after the package loads.
- **Limited Isolation**: Drivers are accessible to all packages in the instance.

---

### **6. Why Not Place Drivers in `IntegrationServer\lib`?**

**Answer:**
Placing drivers in `IntegrationServer\lib` makes them available to **all instances of the Integration Server**. This can lead to conflicts if different instances require different versions of the same driver.

**Disadvantages:**

- **Global Scope**: Drivers are accessible to all instances.
- **Version Conflicts**: Different instances may require different driver versions.

---

### **7. Why Not Place Drivers in `common\lib\ext`?**

**Answer:**
Placing drivers in `common\lib\ext` makes them available to **all Software AG products**. This can lead to unnecessary resource consumption and potential conflicts.

**Disadvantages:**

- **Global Scope**: Drivers are accessible to all Software AG products.
- **Resource Overhead**: Unnecessary loading of drivers for products that don’t need them.

---

### **8. Summary of Driver Placement Locations**

| **Location**                                          | **Scope**                           | **Advantages**                         | **Disadvantages**                                                 |
| ----------------------------------------------------- | ----------------------------------- | -------------------------------------- | ----------------------------------------------------------------- |
| `/instances/instance_name/WmJDBCAdapter/code/jars`    | Only the JDBC Adapter package       | Isolation, easier deployment           | Limited to JDBC Adapter package                                   |
| `WmJDBCAdapter/code/static/jars`                      | All packages in the instance        | Drivers loaded before packages         | Lack of isolation, potential conflicts                            |
| `IntegrationServer\instances\default\lib\jars\custom` | All packages in the instance        | Centralized location                   | Drivers available only after package loads, deployment complexity |
| `IntegrationServer\lib`                               | All instances of Integration Server | Centralized location for all instances | Global scope, potential version conflicts                         |
| `common\lib\ext`                                      | All Software AG products            | Centralized location for all products  | Global scope, resource overhead, potential conflicts              |

---

### **9. Best Practices for Driver Placement**

- Use `/instances/instance_name/WmJDBCAdapter/code/jars` for **JDBC Adapter-specific drivers** to ensure isolation and ease of deployment.
- Avoid placing drivers in global locations like `common\lib\ext` or `IntegrationServer\lib` unless absolutely necessary.
- Use `WmJDBCAdapter/code/static/jars` only if you need the drivers to be available to all packages in the instance.

---

### **10. Example Scenario**

**Scenario**: You need to connect to a MySQL database using the JDBC Adapter.

- **Step 1**: Place the MySQL JDBC driver JAR file in `/instances/instance_name/WmJDBCAdapter/code/jars`.
- **Step 2**: Configure the JDBC Adapter connection in webMethods.
- **Step 3**: Create and run adapter services to interact with the MySQL database.

**Why This Approach?**

- The driver is isolated to the JDBC Adapter package.
- Deployment is easier as the driver is loaded with the package.
- No risk of conflicts with other packages or instances.

---
