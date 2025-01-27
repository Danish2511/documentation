# Overview of Local Publishing in webMethods Integration Server

**Local publishing** refers to the process where a document is published within the **same Integration Server** without involving an external messaging provider like Broker or Universal Messaging. This means that only subscribers running on the same server can receive and process the document.

Although convenient for some use cases, **local publishing is not scalable** and is generally discouraged for production environments. Software AG recommends using a messaging provider for better scalability.

### When Local Publishing Occurs

A publishable document type is published locally in these cases:

- The **Broker connection alias** for the publishable document type is assigned, and the service specifies that the document should be published locally.
- The **messaging connection alias** assigned to the publishable document type is set to `IS_LOCAL_CONNECTION`.
- The **default messaging connection alias** is `IS_LOCAL_CONNECTION`.
- A document type configured for **Universal Messaging** cannot be published locally.

---

### Steps for Local Publishing

| **Step** | **Description**                                                                                                                                                                                |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1**    | The publishing service sends a document to the **dispatcher**. If validation is configured, the document is validated before publishing. If invalid, an exception is thrown.                   |
| **2**    | The **dispatcher** determines if there are any triggers subscribed to the document. It places the document in the subscriber’s trigger queue or discards the document if no subscribers exist. |
| **3**    | A thread is obtained from the **server thread pool**, the document is pulled from the trigger queue, and evaluated against the trigger conditions.                                             |
| **4**    | If the document matches a trigger condition, the associated **trigger service** is executed. If not, the document is discarded or acknowledged, and the server thread is returned.             |
| **5**    | After the trigger service completes:                                                                                                                                                           |
|          | - If successful: Integration Server acknowledges the document (for guaranteed documents), removes it from the queue, and returns the thread.                                                   |
|          | - If there is an exception: Integration Server rejects the document, removes it from the queue, and returns the thread. If the document is guaranteed, it is acknowledged.                     |
|          | - If there is a transient error: Integration Server retries the service based on a retry interval. After exceeding retries, it treats the last failure as a service error.                     |

---

### Important Notes

#### Validation and Document Types

- **Document Validation**: By default, documents may be validated before publishing. This can be disabled for performance.
- **Guaranteed Documents**: Locally published guaranteed documents are saved in a **trigger document store**. If the Integration Server is restarted before processing these documents, they will be recovered. However, volatile documents are **not recovered** if the server is restarted.
- **Trigger Queue Overflow**: If the trigger queue exceeds capacity, Integration Server can be configured to reject locally published documents for that queue.

#### Error Handling and Retry Mechanism

- **Transient Errors**: In the case of transient errors (such as a temporary network failure), the Integration Server will retry the trigger service after waiting for the retry interval. If the retry count is exhausted, the error is treated as a service error.
- **Audit Data**: Services can be configured to generate audit logs, allowing re-invocation of failed trigger services via **webMethods Monitor**.

#### Processing Mode for Triggers

- **Serial Processing**: Documents are processed one at a time in the order they are received in the trigger queue.
- **Concurrent Processing**: Documents can be processed concurrently, potentially out of the order in which they were placed in the queue.

#### Time-to-Live (TTL) for Documents

- Integration Server can enforce the TTL for locally published documents. If the document expires before processing, it will be discarded, preventing unnecessary processing.

---

### Example Scenario: Local Publishing for User Registration

Consider a scenario where a system needs to register new users:

1. A service publishes a user registration document locally.
2. The **dispatcher** checks for triggers that subscribe to the registration document type.
3. The trigger service processes the registration (e.g., storing the user information in the database).
4. If successful, the service acknowledges the document; otherwise, it is rejected, and an exception is logged.

---

### Key Considerations

- **Scalability**: Local publishing only works within the same server and does not scale across multiple servers. For larger, distributed systems, using **Universal Messaging** or **Broker** is recommended.
- **Guaranteed vs. Volatile**: **Guaranteed documents** are persisted and recoverable in case of failure, while **volatile documents** are temporary and are lost if the server crashes or is restarted.

---
