### **Purpose of This POC**

The main purpose of this **Proof of Concept (POC)** is to **demonstrate and validate the integration of different technologies** (webMethods, Kafka, ELK Stack, and MySQL) to manage an **insurance system**. It aims to ensure that the system can handle **policy creation**, **claims processing**, **audit logging**, and provide **real-time insights** into system activity.

---

### **Key Objectives**
1. **Seamless Data Flow**:
   - Ensure data flows smoothly between the **frontend**, **webMethods**, **Kafka**, and **MySQL database**.
   - Example: When a policy is created, it should be stored in the database and trigger downstream events via Kafka.

2. **Real-Time Event Processing**:
   - Use Kafka for real-time event streaming so that other systems (like notifications) can act on events (e.g., `PolicyCreated`, `ClaimFiled`).
   - Example: When a claim is filed, the system should immediately notify the customer or the agent.

3. **Audit Logging and Monitoring**:
   - Log every API interaction (request/response) and system event in **audit logs** for traceability.
   - Use **ELK Stack** to monitor and visualize these logs in real-time.

4. **Scalability**:
   - Validate that the architecture can handle a large number of policies, claims, and events without performance issues.

---

### **Real-Time Example**

#### **Scenario: John Buys a Health Insurance Policy**
Here’s how the POC works step-by-step:

1. **Policy Creation**:
   - John fills in the details on the frontend and clicks "Create Policy."
   - The system creates the policy, stores it in the `insurance_details` table, and publishes a `PolicyCreated` event to Kafka.
   - Kafka notifies other services (e.g., a notification service sends an email to John confirming the policy creation).

2. **Audit Logging**:
   - Every step (API request and response) is logged in `audit-logs` (via Kafka).
   - Example: The `Create Policy` API logs the request (John's details) and response (success message).

3. **Monitoring with ELK**:
   - Administrators monitor all API calls, events, and errors in real-time via **Kibana dashboards**.
   - Example: If there’s an issue in the policy creation process (e.g., database timeout), the error is logged and visible in Kibana.

4. **Claims Processing**:
   - Later, John files a claim for a medical expense.
   - The claim is recorded in the `claim_info` table, and a `ClaimFiled` event is published to Kafka.
   - A claim processing service consumes the event, evaluates the claim, and sends updates to John.

---

### **Why This POC is Important**

1. **Business Value**:
   - Automates the insurance lifecycle, reducing manual effort and errors.
   - Enables real-time notifications and tracking for customers, agents, and administrators.

2. **Technical Validation**:
   - Tests the integration of webMethods (for API management), Kafka (for real-time streaming), and ELK (for monitoring).
   - Validates the architecture’s ability to scale and handle large transaction volumes.

3. **Traceability and Monitoring**:
   - Ensures all operations (policy creation, updates, claims) are logged and monitored, making it easy to debug issues.

---

### **Simple Summary**

This POC is like building a **test system for an insurance company**. It ensures:
- Policies and claims can be created, updated, and logged properly.
- Notifications are sent instantly when events happen.
- Administrators can monitor the system in real-time.

---

I understand your confusion! Let me explain the **real-time process** in **detail**, including how the tables interact, what data is required, and how the relationships between the tables work.

---

### **Detailed Example: Real-Time Flow**

#### **Tables Recap**:
We have the following tables, each representing a specific aspect of the insurance system:
1. **insurance_details** (Policy-related information).
2. **customer_info** (Owner/Customer details).
3. **agency_info** (Agent and agency information).
4. **claim_info** (Claims filed against policies).

#### **Relationships**:
- `insurance_details.Owner_ID` → `customer_info.Owner_ID` (Foreign Key).
- `customer_info.Agent_ID` → `agency_info.Agent_ID` (Foreign Key).
- `claim_info.Agent_ID` → `agency_info.Agent_ID` (Foreign Key).
- `claim_info.InsuredID` → `insurance_details.InsuredID` (Foreign Key).

These relationships mean that creating or working with a **policy** or **claim** requires some **existing related data** in the other tables.

---

### **Step-by-Step Example**

#### **1. Prepare Data: Owner and Agent Information**
Before creating a policy, the **customer (Owner)** and their **agent (Agent)** must exist in the database. This ensures the **foreign key relationships** are valid.

##### **Insert Agent Information**:
1. Use the `agency_info` table to store the agent and agency details.
2. Example entry:
   ```sql
   INSERT INTO agency_info (Agent_ID, Agent_Name, Agency_Name, Agent_Location)
   VALUES ('AG001', 'Alice Brown', 'Global Insurance', 'New York');
   ```

##### **Insert Customer Information**:
1. Use the `customer_info` table to store the owner's details, linking them to the agent.
2. Example entry:
   ```sql
   INSERT INTO customer_info (Owner_ID, Owner_Name, Nominee1, Nominee2, Agent_ID, Address, ZipCode)
   VALUES ('OW123', 'John Doe', 'Jane Doe', 'Jack Doe', 'AG001', '123 Elm Street', '12345');
   ```

---

#### **2. Create a Policy**
Once the **agent** and **owner** information exists, a policy can be created for the owner.

##### **Policy Creation Input**:
From the frontend, the user provides details such as:
- Policy information (e.g., `Policy_Type`, `Insurance_Type`).
- The associated **Owner_ID** (e.g., `OW123`).

##### **Flow for Policy Creation**:
1. **Validate Input**:
   - Ensure `Owner_ID` exists in the `customer_info` table.
   - If `Owner_ID` does not exist, return an error (e.g., "Owner does not exist").

2. **Insert Policy into `insurance_details`**:
   - Use the provided input to create a policy.
   - Example SQL:
     ```sql
     INSERT INTO insurance_details 
     (InsuredID, NRIC, Insurance_Type, Policy_Type, Coverage_No, Owner_ID, EffDate, PlanCode, PremiumStatus)
     VALUES 
     ('IG26486739040', 'C0213467', 'Individual', 'Health', 'COV123', 'OW123', '2025-01-01', 'PLAN001', 'Active');
     ```

3. **Publish PolicyCreated Event**:
   - Publish an event to the Kafka topic `insurance-policy-events` with the policy details:
     ```json
     {
       "eventType": "PolicyCreated",
       "InsuredID": "IG26486739040",
       "Policy_Type": "Health",
       "Owner_ID": "OW123",
       "timestamp": "2025-01-18T10:00:00Z"
     }
     ```

---

#### **3. File a Claim**
Once the policy is created, the owner can file a claim against it.

##### **Claim Filing Input**:
From the frontend, the user provides:
- The `InsuredID` of the policy.
- Claim details (`Reason`, `Agent_ID`, etc.).

##### **Flow for Claim Filing**:
1. **Validate Input**:
   - Check if `InsuredID` exists in `insurance_details`.
   - Check if `Agent_ID` exists in `agency_info`.

2. **Insert Claim into `claim_info`**:
   - Example SQL:
     ```sql
     INSERT INTO claim_info 
     (Claim_ID, claim_Date, Reason, Agent_ID, Currency, Claim_Status, Payment_Status, InsuredID)
     VALUES 
     ('CLM001', '2025-01-20', 'Medical Expense', 'AG001', 'USD', 'Filed', 'Pending', 'IG26486739040');
     ```

3. **Publish ClaimFiled Event**:
   - Publish an event to Kafka:
     ```json
     {
       "eventType": "ClaimFiled",
       "Claim_ID": "CLM001",
       "InsuredID": "IG26486739040",
       "Reason": "Medical Expense",
       "timestamp": "2025-01-20T12:00:00Z"
     }
     ```

---

#### **4. Approve the Claim**
After a claim is filed, an admin or system process can approve or reject it.

##### **Claim Approval Input**:
The user provides the `Claim_ID` and the updated status (`Approved` or `Rejected`).

##### **Flow for Claim Approval**:
1. **Update Claim Status**:
   - Example SQL:
     ```sql
     UPDATE claim_info 
     SET Claim_Status = 'Approved', Payment_Status = 'Paid'
     WHERE Claim_ID = 'CLM001';
     ```

2. **Publish ClaimUpdated Event**:
   - Publish an event to Kafka:
     ```json
     {
       "eventType": "ClaimApproved",
       "Claim_ID": "CLM001",
       "InsuredID": "IG26486739040",
       "timestamp": "2025-01-20T15:00:00Z"
     }
     ```

---

### **Key Considerations**
1. **Foreign Key Dependencies**:
   - Always ensure that **related data exists** (e.g., `Owner_ID`, `Agent_ID`) before creating a policy or filing a claim.

2. **Policy Creation Flow**:
   - Requires the `Owner_ID` to link the policy to a customer.
   - Optionally validate the associated `Agent_ID` if required.

3. **Claim Filing Flow**:
   - Requires both `InsuredID` (from `insurance_details`) and `Agent_ID` (from `agency_info`).

4. **Audit Logging**:
   - Log every request and response during policy creation, claim filing, and approval for traceability.

---

### **Real-Time Example Recap**
1. **Step 1**: Add agent Alice Brown (`agency_info`) and customer John Doe (`customer_info`).
2. **Step 2**: Create a policy for John Doe (`insurance_details`), linking it to his `Owner_ID`.
3. **Step 3**: File a claim for this policy (`claim_info`) and link it to the agent.
4. **Step 4**: Approve the claim and update its status.

---

Here’s a **detailed explanation of Kafka topics**, the **events to publish**, and how they fit into the real-time flow of this POC. I’ll explain the **purpose of each topic**, the **data structure of events**, and the **downstream processing** to show how it all works in real time.

---

### **Purpose of Kafka in This POC**
Kafka acts as a **real-time message broker**, enabling asynchronous communication between systems. It ensures:
1. **Decoupling**: Frontend, backend, and downstream services don’t directly depend on each other.
2. **Event Streaming**: Every significant action (policy creation, claim filing) is published as an event to Kafka topics.
3. **Real-Time Processing**: Other services (e.g., notification or claim settlement systems) consume these events and act accordingly.

---

### **Kafka Topics and Their Purpose**

1. **`insurance-policy-events`**:
   - **Purpose**: Publishes events related to policies (e.g., created, updated, renewed, or cancelled).
   - **Producers**: webMethods API for policy operations.
   - **Consumers**: 
     - Notification Service: Sends emails or SMS to customers.
     - Monitoring Systems: Tracks policy activities.

2. **`claim-events`**:
   - **Purpose**: Publishes events related to claims (e.g., filed, approved, rejected, or paid).
   - **Producers**: webMethods API for claim operations.
   - **Consumers**: 
     - Settlement Service: Processes claim payouts.
     - Monitoring Systems: Tracks claim statuses.

3. **`audit-logs`**:
   - **Purpose**: Captures API logs (request/response) for auditing and monitoring.
   - **Producers**: All webMethods APIs (e.g., `/policy`, `/claim`).
   - **Consumers**: 
     - Logstash: Parses logs and indexes them into Elasticsearch.
     - Kibana: Visualizes API activities, errors, and performance metrics.

---

### **Real-Time Flow for Kafka**

#### **1. Policy Creation**
- **Topic**: `insurance-policy-events`
- **Event Published**:
  When a policy is successfully created, the following event is published:
  ```json
  {
    "eventType": "PolicyCreated",
    "InsuredID": "IG26486739040",
    "Insurance_Type": "Individual",
    "Policy_Type": "Health",
    "Owner_ID": "OW123",
    "timestamp": "2025-01-18T10:00:00Z"
  }
  ```

- **Consumers**:
  1. **Notification Service**:
     - Reads the event and sends an email to the customer: 
       > "Your Health policy (IG26486739040) has been successfully created."
  2. **Monitoring Systems**:
     - Tracks policy creation events and identifies trends (e.g., number of policies created daily).

---

#### **2. Policy Update**
- **Topic**: `insurance-policy-events`
- **Event Published**:
  When a policy is updated, an event like this is published:
  ```json
  {
    "eventType": "PolicyUpdated",
    "InsuredID": "IG26486739040",
    "updatedFields": {
      "PlanCode": "PLAN002",
      "PremiumStatus": "Inactive"
    },
    "timestamp": "2025-01-18T12:00:00Z"
  }
  ```

- **Consumers**:
  1. **Notification Service**:
     - Sends an email to the customer: 
       > "Your policy (IG26486739040) has been updated. New Plan: PLAN002."
  2. **Monitoring Systems**:
     - Tracks changes in policy data for reporting purposes.

---

#### **3. Claim Filing**
- **Topic**: `claim-events`
- **Event Published**:
  When a claim is filed, the following event is published:
  ```json
  {
    "eventType": "ClaimFiled",
    "Claim_ID": "CLM001",
    "InsuredID": "IG26486739040",
    "Reason": "Medical Expense",
    "Agent_ID": "AG001",
    "Currency": "USD",
    "timestamp": "2025-01-20T10:00:00Z"
  }
  ```

- **Consumers**:
  1. **Settlement Service**:
     - Consumes the event and triggers the settlement process.
     - Example: Validates the claim and marks it for processing.
  2. **Monitoring Systems**:
     - Tracks claim filings to generate reports or detect anomalies.

---

#### **4. Claim Approval**
- **Topic**: `claim-events`
- **Event Published**:
  When a claim is approved or rejected, an event is published:
  ```json
  {
    "eventType": "ClaimApproved",
    "Claim_ID": "CLM001",
    "InsuredID": "IG26486739040",
    "Claim_Status": "Approved",
    "Payment_Status": "Paid",
    "timestamp": "2025-01-20T15:00:00Z"
  }
  ```

- **Consumers**:
  1. **Notification Service**:
     - Sends an email to the customer:
       > "Your claim (CLM001) has been approved. Payment status: Paid."
  2. **Monitoring Systems**:
     - Tracks claim approvals to measure efficiency and detect bottlenecks.

---

#### **5. Audit Logs**
- **Topic**: `audit-logs`
- **Event Published**:
  Every API interaction is logged and published to this topic:
  ```json
  {
    "timestamp": "2025-01-18T10:00:00Z",
    "apiName": "/policy",
    "method": "POST",
    "request": {
      "InsuredID": "IG26486739040",
      "Policy_Type": "Health",
      "Owner_ID": "OW123"
    },
    "response": {
      "status": "success",
      "message": "Policy created successfully"
    },
    "clientIp": "192.168.1.10"
  }
  ```

- **Consumers**:
  1. **Logstash**:
     - Reads logs, parses the JSON, and indexes them into Elasticsearch.
  2. **Kibana**:
     - Visualizes the logs in dashboards for monitoring API activity and debugging issues.

---

### **Summary of Kafka Topics**

| **Topic Name**            | **Purpose**                              | **Producer**            | **Consumers**                       |
|---------------------------|------------------------------------------|-------------------------|--------------------------------------|
| `insurance-policy-events` | Publish policy creation and update events| webMethods (Policy APIs)| Notification, Monitoring            |
| `claim-events`            | Publish claim-related events            | webMethods (Claim APIs) | Settlement, Monitoring              |
| `audit-logs`              | Log API requests/responses              | All APIs                | Logstash (for ELK), Monitoring      |

---

### **Real-Time Example Flow**
1. **Policy Creation**:
   - Publish `PolicyCreated` to `insurance-policy-events`.
   - Notify the customer and track trends.
2. **Claim Filing**:
   - Publish `ClaimFiled` to `claim-events`.
   - Trigger settlement workflows.
3. **Audit Everything**:
   - Publish logs for every API call to `audit-logs` for real-time monitoring in Kibana.

---

Here’s a detailed breakdown of the **ELK (Elasticsearch, Logstash, Kibana)** flow, focusing on **real-time operations** in this POC, explaining **what data is sent to Elasticsearch**, **how Logstash processes it**, and **how Kibana visualizes it**. 

---

### **Purpose of ELK in This POC**

1. **Centralized Logging**:
   - All API logs (e.g., request/response, errors) and system events are sent to Elasticsearch via Logstash.
   - Enables centralized storage and analysis of logs.

2. **Real-Time Monitoring**:
   - Kibana dashboards provide real-time insights into API usage, error rates, and system health.

3. **Debugging and Auditing**:
   - Elasticsearch stores detailed logs, making it easy to trace issues and analyze system behavior.

---

### **Data Sent to Elasticsearch**
The **`audit-logs` Kafka topic** acts as the primary source for logs that are ingested into Elasticsearch.

#### **Example Data Published to `audit-logs`**
1. **Successful API Interaction**:
   ```json
   {
     "timestamp": "2025-01-18T10:00:00Z",
     "apiName": "/policy",
     "method": "POST",
     "request": {
       "InsuredID": "IG26486739040",
       "Policy_Type": "Health",
       "Owner_ID": "OW123"
     },
     "response": {
       "status": "success",
       "message": "Policy created successfully"
     },
     "clientIp": "192.168.1.10"
   }
   ```

2. **Error Log**:
   ```json
   {
     "timestamp": "2025-01-18T10:05:00Z",
     "apiName": "/policy",
     "method": "POST",
     "request": {
       "InsuredID": "IG26486739040",
       "Policy_Type": "Health"
     },
     "error": {
       "message": "Owner_ID is missing",
       "stackTrace": "java.lang.Exception: Validation failed..."
     },
     "clientIp": "192.168.1.10"
   }
   ```

---

### **Flow of Data Through ELK**

#### **Step 1: Logstash Reads Logs**
- Logstash acts as a **consumer** for the `audit-logs` Kafka topic.

**Logstash Configuration Example**:
```yaml
input:
  kafka:
    bootstrap_servers: "localhost:9092"
    topics: ["audit-logs"]
    group_id: "logstash-group"

filter:
  json:
    source: "message"

output:
  elasticsearch:
    hosts: ["http://localhost:9200"]
    index: "audit-logs-%{+YYYY.MM.dd}"
```

1. **Input**:
   - Logstash reads JSON-formatted messages from Kafka.
   - Each log message represents an API interaction or event.
   
2. **Filter**:
   - Parses the JSON payload.
   - Adds metadata like `@timestamp`, Kafka partition, or log type (success/error).

3. **Output**:
   - Sends the parsed log to Elasticsearch.
   - Logs are indexed in **daily indices** (e.g., `audit-logs-2025.01.18`).

---

#### **Step 2: Elasticsearch Stores Logs**
- Elasticsearch stores logs for querying and visualization.
- **Index Structure**:
  - Each log is stored as a document in the corresponding index.
  - Example document structure:
    ```json
    {
      "@timestamp": "2025-01-18T10:00:00Z",
      "apiName": "/policy",
      "method": "POST",
      "request": {
        "InsuredID": "IG26486739040",
        "Policy_Type": "Health",
        "Owner_ID": "OW123"
      },
      "response": {
        "status": "success",
        "message": "Policy created successfully"
      },
      "clientIp": "192.168.1.10"
    }
    ```

- **Why Elasticsearch?**:
  - Logs are stored in a **searchable format**.
  - Full-text search, aggregations, and filtering make it easy to analyze logs.

---

#### **Step 3: Visualize Logs in Kibana**
- Kibana provides dashboards for real-time monitoring and analytics.

**Dashboard Examples**:
1. **API Usage**:
   - Shows total API calls, grouped by endpoint (e.g., `/policy`, `/claim`).
   - Useful to track system activity and identify high-usage endpoints.

   **Kibana Visualization**: Bar chart
   - X-axis: API Names
   - Y-axis: Count of Calls

2. **Error Monitoring**:
   - Shows errors grouped by API name or type.
   - Helps identify frequently failing APIs.

   **Kibana Visualization**: Pie chart
   - Slices: Error types (e.g., validation errors, database errors).

3. **Request Latency**:
   - Tracks the average response time of APIs.
   - Helps identify performance bottlenecks.

   **Kibana Visualization**: Line chart
   - X-axis: Time
   - Y-axis: Response Time (ms)

4. **Top IP Addresses**:
   - Shows which IPs are making the most requests.
   - Useful for detecting abuse or malicious activity.

   **Kibana Visualization**: Table
   - Columns: Client IP, Request Count

---

### **Real-Time Example with ELK**

#### **Scenario: Policy Creation Logs**

1. **API Call**:
   - Frontend calls `/policy` to create a new policy.

2. **Log Published**:
   - webMethods publishes the following log to `audit-logs`:
     ```json
     {
       "timestamp": "2025-01-18T10:00:00Z",
       "apiName": "/policy",
       "method": "POST",
       "request": {
         "InsuredID": "IG26486739040",
         "Policy_Type": "Health",
         "Owner_ID": "OW123"
       },
       "response": {
         "status": "success",
         "message": "Policy created successfully"
       },
       "clientIp": "192.168.1.10"
     }
     ```

3. **Logstash Processing**:
   - Reads the log from Kafka.
   - Adds metadata (e.g., `@timestamp`, topic, partition).
   - Sends the log to Elasticsearch.

4. **Elasticsearch Storage**:
   - Stores the log in the index `audit-logs-2025.01.18`.

5. **Kibana Visualization**:
   - The admin views:
     - Total calls to `/policy`.
     - Response times for `/policy`.
     - Any errors for `/policy`.

---

### **Data Sent to Elasticsearch**

#### **Log Fields**:
1. **Common Fields** (for all logs):
   - `@timestamp`: Time of the API call.
   - `apiName`: Name of the API (e.g., `/policy`, `/claim`).
   - `method`: HTTP method (e.g., `POST`, `GET`).
   - `clientIp`: IP address of the caller.

2. **Request Fields**:
   - Original request payload (e.g., `InsuredID`, `Policy_Type`).

3. **Response Fields**:
   - Status (`success` or `failure`).
   - Response message or error details.

4. **Error Logs**:
   - Includes additional fields like `error.message` and `error.stackTrace`.

---

### **Real-Time Insights Enabled by ELK**

1. **API Health Monitoring**:
   - Quickly identify which APIs are failing and why.
   - Example: If `/claim` has a high error rate, Kibana will highlight this.

2. **Performance Analysis**:
   - Identify slow endpoints based on average response times.

3. **Operational Visibility**:
   - Track the number of policies created, claims filed, and approvals processed in real time.

4. **Debugging**:
   - Trace issues by searching for specific `InsuredID` or `Claim_ID`.

---

Let’s break this down into **a detailed explanation** covering every part of the process:

1. **End-to-End Process Flow**:
   - Including **Flow Service logic**, **exception handling**, and **publishing to Kafka**.
2. **Kafka Subscription**:
   - Detailing **why and how services subscribe to Kafka topics**.
   - Logic and **pseudocode** for subscribing services.

---

### **1. End-to-End Process Flow**
#### **Scenario**: Policy Creation API
We’ll follow the flow when a user creates a policy and ensure every detail is captured.

---

#### **Step 1: API Call (Policy Creation)**
**Input**:
- A `POST /policy` request is sent from the frontend to webMethods.
- Example payload:
  ```json
  {
    "InsuredID": "IG26486739040",
    "NRIC": "C0213467",
    "Insurance_Type": "Individual",
    "Policy_Type": "Health",
    "Coverage_No": "COV123",
    "Owner_ID": "OW123",
    "EffDate": "2025-01-01",
    "PlanCode": "PLAN001",
    "PremiumStatus": "Active"
  }
  ```

---

#### **Step 2: Flow Service in webMethods**
The Flow Service `PolicyCreation` handles the logic.

##### **Flow Design**
```plaintext
1. SEQUENCE (Main, Exit on: SUCCESS)
    1.1 SEQUENCE (Validate Input, Exit on: SUCCESS)
        - BRANCH (Check Required Fields)
            - $null --> pub.flow:throwExceptionForRetry (Message: "Missing required fields.")
    1.2 SEQUENCE (Main Logic - TRY, Exit on: SUCCESS)
        - INVOKE: JDBC Adapter (SP_InsertPolicy)
        - INVOKE: Kafka Producer (Publish to insurance-policy-events)
        - INVOKE: JDBC Adapter (SP_CreateAuditLog for success)
        - INVOKE: Kafka Producer (Publish to audit-logs)
    1.3 SEQUENCE (Error Handling - CATCH, Exit on: DONE)
        - INVOKE: pub.flow:getLastError
        - MAP: Construct error log payload
        - INVOKE: JDBC Adapter (SP_CreateAuditLog for error)
        - INVOKE: Kafka Producer (Publish error to audit-logs)
        - MAP: Construct error response
        - EXIT: Return error response to frontend.
```

---

##### **Detailed Steps**

1. **Validate Input**:
   - Use a `BRANCH` step to validate required fields (`InsuredID`, `Policy_Type`, etc.).
   - If a field is missing:
     - Log the error.
     - Return a `400 Bad Request` response.

2. **Insert Policy**:
   - Call the stored procedure `SP_InsertPolicy` via a JDBC Adapter service.
   - Handle SQL exceptions if the insert fails.

3. **Publish to Kafka (insurance-policy-events)**:
   - Publish the following message:
     ```json
     {
       "eventType": "PolicyCreated",
       "InsuredID": "IG26486739040",
       "Insurance_Type": "Individual",
       "Policy_Type": "Health",
       "Owner_ID": "OW123",
       "timestamp": "2025-01-18T10:00:00Z"
     }
     ```

4. **Audit Log for Success**:
   - Call the stored procedure `SP_CreateAuditLog` to log the request and response in the database.
   - Publish the log to Kafka’s `audit-logs` topic.

5. **Error Handling (CATCH Block)**:
   - Capture errors using `pub.flow:getLastError`.
   - Log the error to the database and publish it to Kafka.
   - Return an error response to the frontend.

---

#### **Step 3: Publish to Kafka**
- **Topics**:
  - `insurance-policy-events`: Captures policy-related events.
  - `audit-logs`: Captures API request/response logs.

- **Logic for Kafka Producer Service**:
  - Connect to Kafka brokers.
  - Publish the event to the appropriate topic.
  - Ensure retries in case of failures.

---

### **2. Kafka Subscription**

After publishing to Kafka, **consumers subscribe** to relevant topics to process events in real-time.

#### **Why Subscribe to Kafka Topics?**
1. **Decoupling**:
   - Systems like notification or monitoring services can consume data without directly calling webMethods APIs.

2. **Real-Time Processing**:
   - Events like `PolicyCreated` trigger immediate actions (e.g., sending emails).

---

#### **Example Subscriptions**

1. **Notification Service**:
   - Subscribes to `insurance-policy-events` to send notifications.

2. **Logstash**:
   - Subscribes to `audit-logs` to process API logs and store them in Elasticsearch.

---

#### **Kafka Consumer Logic**
##### **Notification Service Logic**
1. Connect to Kafka topic `insurance-policy-events`.
2. Parse incoming messages.
3. Trigger appropriate actions (e.g., send an email/SMS to the customer).

##### **Pseudocode for Notification Service**
```python
from kafka import KafkaConsumer
import json

# Connect to Kafka topic
consumer = KafkaConsumer(
    'insurance-policy-events',
    bootstrap_servers='localhost:9092',
    group_id='notification-service',
    value_deserializer=lambda m: json.loads(m.decode('utf-8'))
)

# Consume messages
for message in consumer:
    event = message.value
    if event['eventType'] == 'PolicyCreated':
        insured_id = event['InsuredID']
        policy_type = event['Policy_Type']
        owner_id = event['Owner_ID']

        # Send notification
        send_email(
            to=get_email_from_owner_id(owner_id),
            subject="Policy Created",
            body=f"Your {policy_type} policy ({insured_id}) has been successfully created."
        )
```

##### **Key Points for Notification Service**
1. Parse fields like `InsuredID`, `Policy_Type`, and `Owner_ID`.
2. Trigger appropriate notifications using email/SMS APIs.

---

##### **Logstash Consumer Logic**
1. Subscribes to `audit-logs` to process API logs.
2. Parses JSON logs and enriches them (e.g., adding Kafka metadata).
3. Sends logs to Elasticsearch for indexing.

---

#### **3. Real-Time Dashboard in Kibana**
1. **Visualizations**:
   - Total policies created (count of `PolicyCreated` events).
   - Errors logged (from `audit-logs`).
   - Most active APIs (grouped by `apiName`).

2. **Example Queries**:
   - **Show all errors**:
     ```elasticsearch
     GET /audit-logs-2025.01.18/_search
     {
       "query": {
         "match": {
           "response.status": "failure"
         }
       }
     }
     ```
   - **Count policies created today**:
     ```elasticsearch
     GET /audit-logs-2025.01.18/_count
     {
       "query": {
         "match": {
           "eventType": "PolicyCreated"
         }
       }
     }
     ```

---

### **Summary**

#### **webMethods Flow Service (PolicyCreation)**:
1. Validate input.
2. Insert policy into MySQL.
3. Publish events to Kafka (`insurance-policy-events`, `audit-logs`).
4. Log errors in the CATCH block and publish them to Kafka.

#### **Kafka Subscription**:
1. Notification Service subscribes to `insurance-policy-events` and sends customer notifications.
2. Logstash subscribes to `audit-logs` to process API logs for Elasticsearch.

#### **Kibana**:
1. Visualizes API activity (e.g., success vs. failure rates).
2. Tracks real-time system health and usage patterns.

---