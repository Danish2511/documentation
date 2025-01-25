### **API 1: Policy Creation**

**Purpose**: Create a new insurance policy.

**Steps**:

1. **Frontend** sends a `POST` request to the `/policy` endpoint with policy details like `InsuredID`, `PolicyType`, etc.
2. **webMethods** validates the input to ensure required fields are present.
3. Calls the stored procedure `SP_InsertPolicy` in MySQL to insert the new policy details into the `insurance_details` table.
4. Publishes a `PolicyCreated` event to the Kafka topic `insurance-policy-events`.
5. Logs the API request and response by calling `SP_CreateAuditLog` and publishes the log to the `audit-logs` Kafka topic.
6. **API Response**: Returns a success or error message to the frontend.
7. **Downstream**: Kafka consumers, like a notification service, use the `PolicyCreated` event to notify the customer.

---

### **API 2: Policy Update**

**Purpose**: Update an existing insurance policy.

**Steps**:

1. **Frontend** sends a `PUT` request to `/policy/{InsuredID}` with updated policy details.
2. **webMethods** validates the input and checks if the `InsuredID` exists.
3. Calls the stored procedure `SP_UpdatePolicy` in MySQL to update the policy record in the `insurance_details` table.
4. Publishes a `PolicyUpdated` event to `insurance-policy-events` Kafka topic.
5. Logs the transaction using `SP_CreateAuditLog` and publishes it to the `audit-logs` Kafka topic.
6. **API Response**: Returns success or error to the frontend.

---

### **API 3: Policy Renewal Fetch**

**Purpose**: Retrieve policies due for renewal within the next 30 days.

**Steps**:

1. **Frontend** sends a `GET` request to `/policy/renewals`.
2. **webMethods** calls `SP_GetRenewals` in MySQL to fetch policies with renewal dates within 30 days.
3. Logs the request and response using `SP_CreateAuditLog`.
4. **API Response**: Returns a list of policies due for renewal.

---

### **API 4: Policy Cancellation**

**Purpose**: Cancel an insurance policy.

**Steps**:

1. **Frontend** sends a `DELETE` request to `/policy/{InsuredID}`.
2. **webMethods** validates the input and checks if the `InsuredID` exists.
3. Calls `SP_CancelPolicy` in MySQL to delete the policy from `insurance_details`.
4. Publishes a `PolicyCancelled` event to `insurance-policy-events` Kafka topic.
5. Logs the transaction using `SP_CreateAuditLog`.
6. **API Response**: Returns success or error.

---

### **API 5: Claim Filing**

**Purpose**: File a new insurance claim.

**Steps**:

1. **Frontend** sends a `POST` request to `/claim` with claim details like `Claim_ID`, `Reason`, etc.
2. **webMethods** validates the input.
3. Calls `SP_FileClaim` in MySQL to insert the claim into the `claim_info` table.
4. Publishes a `ClaimFiled` event to `insurance-policy-events` Kafka topic.
5. Logs the request and response using `SP_CreateAuditLog`.
6. **API Response**: Returns success or error.

---

### **API 6: Claim Approval/Status Update**

**Purpose**: Approve, reject, or update the status of a claim.

**Steps**:

1. **Frontend** sends a `PUT` request to `/claim/{ClaimID}/status` with the updated status (e.g., `Approved` or `Rejected`).
2. **webMethods** validates the input.
3. Calls `SP_ApproveClaim` in MySQL to update the `Claim_Status` in the `claim_info` table.
4. Publishes a `ClaimUpdated` event to `insurance-policy-events` Kafka topic.
5. Logs the transaction using `SP_CreateAuditLog`.
6. **API Response**: Returns success or error.

---

### **API 7: Claim Status Fetch**

**Purpose**: Retrieve the current status of a claim.

**Steps**:

1. **Frontend** sends a `GET` request to `/claim/{ClaimID}`.
2. **webMethods** calls `SP_GetClaimStatus` in MySQL to fetch the claim’s details.
3. Logs the request and response using `SP_CreateAuditLog`.
4. **API Response**: Returns the claim details to the frontend.

---

### **API 8: Customer Details Fetch**

**Purpose**: Retrieve details of a customer.

**Steps**:

1. **Frontend** sends a `GET` request to `/customer/{OwnerID}`.
2. **webMethods** calls `SP_GetCustomerDetails` in MySQL to fetch customer information.
3. Logs the request and response using `SP_CreateAuditLog`.
4. **API Response**: Returns the customer details to the frontend.

---

### **API 9: Customer Details Update**

**Purpose**: Update customer details like name, address, or nominees.

**Steps**:

1. **Frontend** sends a `PUT` request to `/customer/{OwnerID}` with the updated details.
2. **webMethods** validates the input.
3. Calls `SP_UpdateCustomer` in MySQL to update the record in `customer_info`.
4. Logs the transaction using `SP_CreateAuditLog`.
5. **API Response**: Returns success or error.

---

### **API 10: Agent Details Fetch**

**Purpose**: Retrieve details of an agent or agency.

**Steps**:

1. **Frontend** sends a `GET` request to `/agent/{AgentID}`.
2. **webMethods** calls `SP_GetAgentDetails` in MySQL to fetch agent/agency details.
3. Logs the request and response using `SP_CreateAuditLog`.
4. **API Response**: Returns agent/agency details.

---

### **API 11: Agent Details Update**

**Purpose**: Update details of an agent or agency.

**Steps**:

1. **Frontend** sends a `PUT` request to `/agent/{AgentID}` with updated details.
2. **webMethods** validates the input.
3. Calls `SP_UpdateAgentDetails` in MySQL to update the record in `agency_info`.
4. Logs the transaction using `SP_CreateAuditLog`.
5. **API Response**: Returns success or error.

---

### **API 12: Audit Logging**

**Purpose**: Log all API interactions for traceability and monitoring.

**Steps**:

1. Each API logs its request and response details using `SP_CreateAuditLog`.
2. The log is published to the `audit-logs` Kafka topic.
3. **Logstash** reads logs from `audit-logs` and sends them to Elasticsearch.
4. **Kibana** visualizes the logs for debugging and monitoring.

---
