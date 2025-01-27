# **Basic Elements in the Publish-and-Subscribe Model**

This section provides an overview of the fundamental building blocks used in an integration solution built on the publish-and-subscribe model.

---

## 1. Documents

### What Are Documents?

Documents are the primary data carriers in the publish-and-subscribe model. They encapsulate the data exchanged between applications and webMethods components.

- **Real-Life Examples**:
  - A **Purchase Order (PO)** document represents a new order placed by a customer.
  - A **Shipping Notice** document represents goods ready for dispatch.
  - A **New Employee Record** document represents an HR onboarding event.

### Document Structure

Each document includes:

- **Body**: The actual data content representing a business event.
- **Envelope**: Metadata for routing and control, such as sender address, timestamp, and sequence numbers.  
  _Similar to an email header, the envelope ensures the document is processed correctly in transit._

---

## 2. Publishable Document Types

### What Are Publishable Document Types?

A publishable document type is a schema-like structure that defines the format and content of a document that can be published or subscribed to.

- **Key Features**:
  - Defines the structure of the document (fields and data types).
  - Associated with a messaging provider for routing.

### Real-Life Scenario

In a purchase order system:

- A **Publishable Document Type** named `PurchaseOrder` describes fields like `OrderID`, `CustomerName`, and `OrderDate`.
- Instances of this type can be published by the order management system and consumed by the inventory and billing systems.

---

## 3. Triggers (webMethods Messaging Triggers)

### What Are Triggers?

Triggers establish subscriptions to specific publishable document types and link these subscriptions to processing services.

- **Key Functions**:
  - Subscribe to documents based on their type.
  - Route received documents to specific services for processing.

### Example Workflow

- **Trigger**: Subscribes to `PurchaseOrder` documents.
- **Service**: Processes the purchase order, updates inventory, and sends a confirmation.

---

## 4. Services

### What Are Services?

Services are the logic handlers that process documents. They can:

- Extract data from documents.
- Interact with backend systems.
- Publish new documents.

### Example Use

When a **PurchaseOrder** document is received:

- A service extracts order details.
- It validates stock availability in the inventory database.
- It publishes a `StockUpdate` document if inventory changes are required.

---

## 5. Adapter Notifications

### What Are Adapter Notifications?

Adapter notifications monitor external resources (like databases) for specific events and publish corresponding documents.

- **Example**:
  - A **JDBC Adapter Notification** monitors a database table for new entries.
  - When a new row is added, the adapter notification publishes a document, e.g., `NewCustomerPublishDocument`.

### Integration Workflow

- Adapter notifications generate a document upon detecting an event.
- Triggers subscribe to the document and pass it to a service for further processing.

---

## 6. Canonical Documents

### What Are Canonical Documents?

Canonical documents are standardized data representations used as an intermediate format for integration. They enable seamless communication between systems by reducing dependencies on specific data formats.

### Example Use Case

- A retail company receives orders in various formats (e.g., XML, JSON).
- These orders are converted into a **Canonical Purchase Order** document format for internal processing.
- Subscribers then convert the canonical document into formats specific to their application (e.g., SAP, Oracle).

### Benefits of Canonical Documents

- Simplifies integration by introducing a neutral format.
- Reduces the need for direct mappings between multiple publishers and subscribers.

---

## Summary

| **Element**                    | **Purpose**                                                               | **Example**                               |
| ------------------------------ | ------------------------------------------------------------------------- | ----------------------------------------- |
| **Documents**                  | Encapsulate business data for exchange.                                   | Purchase Order, Shipping Notice           |
| **Publishable Document Types** | Define the schema of documents that can be published and subscribed to.   | PurchaseOrder, StockUpdate                |
| **Triggers**                   | Subscribe to documents and route them to services.                        | Process Purchase Orders                   |
| **Services**                   | Contain the logic to process documents and interact with backend systems. | Validate inventory, publish stock updates |
| **Adapter Notifications**      | Publish documents based on events detected in external resources.         | Publish database changes                  |
| **Canonical Documents**        | Provide a standardized data format for integration.                       | Canonical Purchase Order                  |

---
