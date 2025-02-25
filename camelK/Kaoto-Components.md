# Apache Camel K with Kaoto: Components Explained for webMethods Developers

**Date**: February 24, 2025  
**Purpose**: This guide lists Kaoto components in a series—basic, medium, advanced—explaining them simply with real-time examples, mapped to your webMethods background.

---

## Basics Series: Beginner-Friendly Components

These are like webMethods’ core services—easy to start with, foundational for any integration.

### 1. Timer

- **What It Does**: Triggers a route on a schedule—like a webMethods Scheduler Trigger.
- **How to Use**: Set as `from`, define `period` (e.g., every 5 seconds).
- **When to Use**: For periodic tasks (e.g., checking something regularly).
- **Real-Time Example**: A store checks inventory every minute.
  ```yaml
  - from:
      uri: timer:inventory
      parameters:
        period: "60000" # 1 minute
      steps:
        - log:
            message: "Checking inventory at ${date:now:yyyy-MM-dd HH:mm}"
  ```
  - **Output**: Logs “Checking inventory at 2025-02-24 21:30” every minute.

---

### 2. Log

- **What It Does**: Prints messages to the console—like `pub.flow:debugLog` in webMethods.
- **How to Use**: Add as a step, set a `message` (e.g., `${body}`).
- **When to Use**: To see what’s happening in your route (debugging).
- **Real-Time Example**: A bank logs a transaction amount.
  ```yaml
  - from:
      uri: timer:transaction
      parameters:
        period: "5000"
      steps:
        - setBody:
            expression:
              constant:
                expression: "100"
        - log:
            message: "Transaction amount: ${body}"
  ```
  - **Output**: Logs “Transaction amount: 100” every 5 seconds.

---

### 3. SetBody

- **What It Does**: Changes the message content—like a Transformer setting `%value%` in webMethods.
- **How to Use**: Add as a step, use `simple` or `constant` expressions.
- **When to Use**: To set or transform data (e.g., hardcoding a value).
- **Real-Time Example**: A chatbot sets a greeting.
  ```yaml
  - from:
      uri: timer:chat
      parameters:
        period: "10000"
      steps:
        - setBody:
            expression:
              constant:
                expression: "Hello, customer!"
        - log:
            message: "${body}"
  ```
  - **Output**: Logs “Hello, customer!” every 10 seconds.

---

## Medium Series: Intermediate Components

These are like webMethods’ adapters and flow controls—building on basics with more functionality.

### 4. File

- **What It Does**: Reads or writes files—like a webMethods File Adapter.
- **How to Use**: As `from` to read, `to` to write; set `directoryName` and `fileName`.
- **When to Use**: For file-based integrations (e.g., processing uploads).
- **Real-Time Example**: A school saves a student list to a file.
  ```yaml
  - from:
      uri: timer:students
      parameters:
        period: "5000"
      steps:
        - setBody:
            expression:
              constant:
                expression: "Alice, Bob"
        - to:
            uri: file://output
            parameters:
              directoryName: C:/my-camel-workspace/output
              fileName: students.txt
  ```
  - **Output**: Creates `students.txt` with “Alice, Bob” every 5 seconds.

---

### 5. Choice

- **What It Does**: Decides what to do based on conditions—like a webMethods Branch.
- **How to Use**: Add `choice`, define `when` (condition) and `otherwise`.
- **When to Use**: For conditional logic (e.g., high vs. low values).
- **Real-Time Example**: A store flags big orders.
  ```yaml
  - from:
      uri: timer:order
      parameters:
        period: "5000"
      steps:
        - setBody:
            expression:
              constant:
                expression: "75"
        - choice:
            when:
              - simple: "${body} > 50"
                steps:
                  - log:
                      message: "Big order: ${body}"
            otherwise:
              steps:
                - log:
                    message: "Small order: ${body}"
  ```
  - **Output**: Logs “Big order: 75” every 5 seconds.

---

### 6. Split

- **What It Does**: Breaks a list into pieces—like a webMethods LOOP over a Document List.
- **How to Use**: Add `split`, use `tokenize` to split (e.g., by comma).
- **When to Use**: To process multiple items (e.g., order items).
- **Real-Time Example**: A party logs guest names.
  ```yaml
  - from:
      uri: timer:party
      parameters:
        period: "5000"
      steps:
        - setBody:
            expression:
              constant:
                expression: "Anna,Ben,Clara"
        - split:
            expression:
              tokenize:
                token: ","
            steps:
              - log:
                  message: "Guest: ${body}"
  ```
  - **Output**: Logs “Guest: Anna”, “Guest: Ben”, “Guest: Clara” every 5 seconds.

---

## Advanced Series: Expert-Level Components

These are like webMethods’ advanced adapters and services—powerful tools for complex integrations.

### 7. Undertow

- **What It Does**: Creates a REST endpoint—like a webMethods HTTP Trigger.
- **How to Use**: As `from`, set `uri` (e.g., `http://localhost:8080/api`), restrict methods (e.g., `GET`).
- **When to Use**: For web APIs (e.g., exposing a service).
- **Real-Time Example**: A store offers a status API.
  ```yaml
  - from:
      uri: undertow:http://localhost:8080/status
      parameters:
        httpMethodRestrict: GET
      steps:
        - setBody:
            expression:
              simple:
                expression: '{"status": "Store is open"}'
  ```
  - **Test**: GET `http://localhost:8080/status` in Postman → `{"status": "Store is open"}`.

---

### 8. Marshal

- **What It Does**: Formats data (e.g., Map to JSON)—like `pub.json:documentToJSON`.
- **How to Use**: Add `marshal`, pick `dataFormat` (e.g., `json`, `csv`).
- **When to Use**: To prepare data for output (e.g., API response).
- **Real-Time Example**: A bakery formats an order as JSON.
  ```yaml
  - from:
      uri: timer:order
      parameters:
        period: "5000"
      steps:
        - setBody:
            expression:
              simple:
                expression: "{id: 1, item: 'cake'}"
        - marshal:
            dataFormat: json
        - log:
            message: "Order: ${body}"
  ```
  - **Output**: Logs “Order: {"id":1,"item":"cake"}” every 5 seconds.

---

### 9. Unmarshal

- **What It Does**: Parses data (e.g., JSON to Map)—like `pub.json:jsonStringToDocument`.
- **How to Use**: Add `unmarshal`, pick `dataFormat` (e.g., `json`).
- **When to Use**: To process incoming formatted data.
- **Real-Time Example**: A store reads a JSON order.
  ```yaml
  - from:
      uri: timer:order
      parameters:
        period: "5000"
      steps:
        - setBody:
            expression:
              simple:
                expression: '{"id": 1, "item": "cake"}'
        - unmarshal:
            dataFormat: json
        - log:
            message: "Item: ${body.item}"
  ```
  - **Output**: Logs “Item: cake” every 5 seconds.

---

### 10. SQL

- **What It Does**: Runs database queries—like a webMethods JDBC Adapter.
- **How to Use**: As `to`, set `uri` to an SQL query, link a `dataSource`.
- **When to Use**: For database operations (e.g., saving data).
- **Real-Time Example**: A shop saves an order total.
  ```yaml
  - from:
      uri: timer:order
      parameters:
        period: "5000"
      steps:
        - setBody:
            expression:
              constant:
                expression: "25"
        - to:
            uri: sql:INSERT INTO orders(amount) VALUES(:#${body})
            parameters:
              dataSource: "#sqliteDS"
  - beans:
      - name: sqliteDS
        type: org.sqlite.SQLiteDataSource
        properties:
          url: "jdbc:sqlite:C:/my-camel-workspace/orders.db"
  ```
  - **Output**: Inserts `25` into `orders` table every 5 seconds.

---

### 11. Mail

- **What It Does**: Sends emails—like a webMethods Email Adapter.
- **How to Use**: As `to`, set `uri` (e.g., `smtp://...`) with email details.
- **When to Use**: For notifications (e.g., order confirmation).
- **Real-Time Example**: A store emails a receipt.
  ```yaml
  - from:
      uri: timer:receipt
      parameters:
        period: "5000"
      steps:
        - setBody:
            expression:
              constant:
                expression: "Order total: $50"
        - to:
            uri: smtp://smtp.gmail.com:587
            parameters:
              username: "yourname@gmail.com"
              password: "your-app-password"
              to: "yourname@gmail.com"
              subject: "Receipt"
              from: "yourname@gmail.com"
              auth: true
              starttls: true
  ```
  - **Output**: Emails “Order total: $50” every 5 seconds.

---

## Notes for webMethods Devs

- **Kaoto Catalog**: These are the main components you’ll see in Kaoto’s right-hand catalog—similar to webMethods’ service palette.
- **Dependencies**: Add them via `camel init --deps=camel:component` (e.g., `camel:json-jackson` for `marshal`/`unmarshal` with JSON).
- **Pipeline Flow**: `${body}` is your `%pipeline%`—each component transforms or uses it.

---

## Running These Examples

- Save each YAML in `C:/my-camel-workspace`.
- Run: `camel run file-name.camel.yaml` (add dependencies as needed).
- Test: Check logs, files, emails, or Postman responses.

---
