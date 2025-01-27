# Publishing and Subscribing to Documents in webMethods

## Introduction

In the **webMethods system**, Integration Servers exchange documents through the **publish-and-subscribe model**.

- **Publishing**: An Integration Server sends a document.
- **Subscribing**: One or more Integration Servers receive and process that document.

This chapter explains how Integration Server interacts with the webMethods messaging provider to:

- Publish documents to the messaging provider.
- Retrieve documents from the messaging provider.
- Publish and subscribe to documents locally.

---

## Overview of Publishing to a webMethods Messaging Provider

### Key Scenarios:

1. **Publishing Documents to the Messaging Provider**:  
   Integration Server publishes documents, and the messaging provider routes them to all subscribers.

2. **Publishing When the Messaging Provider Is Unavailable**:  
   Documents are queued for later delivery using the client-side queue (CSQ).

3. **Publishing and Waiting for a Reply**:  
   A request/reply pattern where the publisher expects a response from the subscriber.

---

## Publishing Documents to the webMethods Messaging Provider

Integration Server interacts with a messaging provider (e.g., **Universal Messaging** or **Broker**) in two modes:

- **Publish**: Broadcasts the document to all subscribers.
- **Deliver**: Sends the document to a specific subscriber.

---

### Process Overview: Publishing to the Messaging Provider

| **Step** | **Description**                                                                                                 |
| -------- | --------------------------------------------------------------------------------------------------------------- |
| **1**    | A **publishing service** or **adapter notification** sends a document to the **dispatcher**.                    |
|          | - If validation is enabled, the document is validated against its publishable document type.                    |
|          | - Validation errors result in an exception.                                                                     |
| **2**    | The dispatcher sends the document to the messaging provider.                                                    |
|          | - If unavailable, the document is routed to the **outbound document store** (if guaranteed and CSQ is enabled). |
| **3**    | The messaging provider determines how to store the document:                                                    |
|          | - **Volatile**: Stored in memory only.                                                                          |
|          | - **Guaranteed**: Stored in memory and on disk.                                                                 |
| **4**    | The messaging provider routes the document to subscribers:                                                      |
|          | - **Published document**: Broadcasts to all subscribers.                                                        |
|          | - **Delivered document**: Enqueues for the specified subscriber.                                                |
|          | - Unsubscribed documents are routed to the **dead letter queue** or discarded if no subscribers exist.          |
| **5**    | Integration Server resumes the publishing service and executes the next step.                                   |

---

### Notes:

- **Subscriber Storage**:

  - For **Broker**: Documents are queued.
  - For **Universal Messaging**: Documents are placed on a channel.

- **Time-to-Live (TTL)**:  
  Documents are discarded if not picked up before their TTL expires.

- **Dead Letter Handling**:  
  If no subscribers exist, documents can be sent to a dead letter queue (if configured).

---

### Real-Life Example:

Consider an **Order Processing System**:

1. A customer places an order on a website.
2. The **Order Management Service** publishes a `PurchaseOrder` document to Universal Messaging.
3. **Subscribers** (e.g., Inventory and Billing services):
   - The Inventory service processes the document to check stock levels.
   - The Billing service processes the document to generate an invoice.
4. If Universal Messaging is unavailable, the document is stored in the CSQ for later processing.

---

# Publishing Documents When the webMethods Messaging Provider Is Unavailable

Integration Server monitors its connection to the webMethods messaging provider and dynamically adjusts the publishing path if the messaging provider becomes unavailable. The server’s response depends on several factors, such as:

1. **The messaging provider in use** (Universal Messaging or Broker).
2. **Configuration of the Client Side Queue (CSQ)**.
3. **Document storage type** (guaranteed or volatile).

---

## Key Factors

### Messaging Provider:

- **Universal Messaging**:

  - CSQ configuration is per connection alias.
  - Determined by the **Enable CSQ** checkbox in the Universal Messaging connection alias settings.

- **Broker**:
  - CSQ configuration is controlled by the `watt.server.publish.useCSQ` parameter:
    - `always` (default): Stores documents in the CSQ.
    - `never`: Throws a `ServiceException` if the provider is unavailable.

---

## Publishing Workflow

| **Step** | **Description**                                                                                                      |
| -------- | -------------------------------------------------------------------------------------------------------------------- |
| **1**    | A publishing service sends a document to the dispatcher.                                                             |
|          | - If validation is enabled, the document is validated. Validation errors result in an exception.                     |
| **2**    | The dispatcher detects the messaging provider is unavailable.                                                        |
|          | - Integration Server waits for reconnection for a specified duration:                                                |
|          | - **Universal Messaging**: Controlled by the **Publish Wait Time While Reconnecting** field in the connection alias. |
|          | - **Broker**: Waits for 400 milliseconds.                                                                            |
|          | - After the wait time, server behavior depends on document type and CSQ configuration:                               |
|          | - **Guaranteed Document & CSQ Enabled**: Routed to the CSQ.                                                          |
|          | - **Guaranteed Document & CSQ Disabled**: Throws an `ISRuntimeException`.                                            |
|          | - **Volatile Document**: Discarded with an exception thrown.                                                         |
| **3**    | When the connection is restored, documents are sent from the CSQ to the messaging provider:                          |
|          | - **Universal Messaging**:                                                                                           |
|          | - Controlled by **Drain CSQ in Order** setting.                                                                      |
|          | - **Broker**: Controlled by `watt.server.publish.drainCSQInOrder` parameter:                                         |
|          | - `true` (default): Ensures publication order by routing new documents to CSQ until it is fully drained.             |
|          | - `false`: Sends new documents directly to the messaging provider while draining the CSQ.                            |
| **4**    | The messaging provider stores the document based on its storage type:                                                |
|          | - **Guaranteed**: Stored in memory and on disk.                                                                      |
|          | - **Volatile**: Stored in memory only.                                                                               |
| **5**    | The messaging provider routes the document to subscribers:                                                           |
|          | - **Published Document**: Broadcast to all subscribers.                                                              |
|          | - **Delivered Document**: Enqueued for the specified subscriber.                                                     |
| **6**    | Integration Server removes the document from the CSQ.                                                                |

---

## Additional Notes

1. **CSQ Configuration**:

   - **Universal Messaging**:

     - Maximum queue size controlled by the **Maximum CSQ Size** field.
     - Publishing fails with an `ISRuntimeException` if the queue is full.

   - **Broker**:
     - Maximum queue size controlled by the `watt.server.control.maxPersist` parameter.
     - Threads are paused when the outbound document store is full.

2. **Draining CSQ**:

   - Documents are sent in batches for faster processing.
   - Batch size can be configured in **Settings > Resources > Store Settings > Edit Document Store Settings** under the **Maximum Documents to Send per Transaction** field.

3. **Retries for Publishing from CSQ**:

   - Universal Messaging:

     - Controlled by `watt.server.messaging.csq.maxRedeliveryCount`.
     - After reaching the maximum redelivery attempts, the document is discarded.

   - Broker:
     - Controlled by `watt.server.publish.maxCSQRedeliveryCount`.
     - Documents that exceed the retry limit are logged with a status of `STATUS_TOO_MANY_TRIES`.

4. **Universal Messaging Pause Mode**:

   - If **PauseServerPublishing** is enabled, publishing fails with `nPublishPauseException`. Integration Server handles this as if the connection alias is unavailable.

5. **Document Validation**:

   - Can be disabled for publishable document types to improve performance.

6. **Monitoring and Resubmission**:
   - Use **webMethods Monitor** to find and resubmit failed documents with statuses like `STATUS_TOO_MANY_TRIES` or `FAILED`.

---

### Real-World Example

Imagine an **Inventory Update System**:

1. An external service publishes `InventoryUpdate` documents to Universal Messaging.
2. Due to network issues, Universal Messaging becomes unavailable.
3. The documents are stored in the CSQ of the Integration Server.
4. Once the connection is restored, the CSQ drains and publishes the pending updates in order.

---

# Publishing Documents and Waiting for a Reply (Request/Reply Model)

In a **publish-and-wait** scenario, a service publishes a document (the request) and waits for a reply document. This can be either **synchronous** or **asynchronous**:

1. **Synchronous**: The service halts execution and waits for a response before resuming.
2. **Asynchronous**: The service proceeds with execution and retrieves the reply asynchronously later.

The messaging provider can either be **Broker** or **Universal Messaging**.

---

## Workflow for Synchronous Request/Reply

| **Step** | **Description**                                                                                                              |
| -------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **1**    | The publishing service sends the request document to the dispatcher, including a unique tag field for reply matching.        |
|          | - The service waits for a reply and enters a waiting state until a response arrives or the wait time expires.                |
|          | - If validation is configured for the document, the service validates the document before publishing it.                     |
|          | - If the document is invalid, an exception is thrown, and the service exits with an exception.                               |
| **2**    | The dispatcher sends the request to the webMethods messaging provider.                                                       |
| **3**    | The messaging provider stores the request document:                                                                          |
|          | - **Volatile**: Stored in memory.                                                                                            |
|          | - **Guaranteed**: Stored in both memory and disk.                                                                            |
| **4**    | The messaging provider routes the document to subscribers.                                                                   |
|          | - **Published**: Broadcast to all subscribers.                                                                               |
|          | - **Delivered**: Sent to a specific subscriber as defined by the delivery request.                                           |
|          | - If the document expires or has no subscribers, it may be routed to the dead letter queue (if configured).                  |
| **5**    | Subscribers retrieve and process the document. They reply by publishing a response using `pub.publish:reply` service.        |
|          | - The reply document is populated with the same tag as the request, ensuring the reply is matched to the correct request.    |
| **6**    | One or more replies are sent back to the messaging provider and stored in memory.                                            |
| **7**    | The requesting Integration Server retrieves the reply documents, using the tag field to match the response with the request. |
| **8**    | The reply document is placed in the pipeline of the waiting service, and the service resumes execution.                      |

---

## Important Considerations

### Reply Handling

- **Multiple Replies**: If multiple replies are sent, only the **first reply** received by the server is processed. Other replies are discarded.
- **Volatile Documents**: All reply documents are **volatile**, meaning they are stored only in memory. If the server is restarted or the connection is lost, these documents will be lost.
- **Reply Document Type**: The reply document must conform to the **publishable document type** specified by the requesting service, if configured.

### Timeout Handling

- If the wait time expires before receiving a reply, the service times out and returns a **null document**. The next step in the flow is executed.
- If a reply arrives after the timeout, the document is rejected, and a journal log message is generated indicating it was rejected due to no waiting thread.

### Validation

- Validation can be disabled for publishable document types, allowing for performance improvement when strict validation is not necessary.

---

## Real-World Example

### Use Case: **Order Processing System**

1. An order processing system sends an order request to an external system via webMethods Messaging.
2. The system waits for a confirmation response to proceed with order fulfillment.
3. The service processes the confirmation reply and continues to the next step, such as updating the database.

---

## Messaging Provider Behavior

### Universal Messaging:

- **Encoding**: Reply documents are encoded as `IData`.
- **Storage**: Both **volatile** and **guaranteed** documents are stored in memory. Guaranteed documents may also be stored on disk.

### Broker:

- **Dead Letter Queue**: If there is no subscriber or the document expires, the message is sent to a dead letter queue (if configured).
- **Request/Reply Queue**: Replies are stored in a specific queue/channel and are routed based on the tag matching the request.

---

### Performance Tips:

- To improve the reliability of message delivery, ensure the **time-to-live** (TTL) for documents is set appropriately to avoid premature message expiration.
- Use **retry logic** to handle intermittent failures or delays in processing.

---
