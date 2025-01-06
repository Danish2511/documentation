### **Day 5: Advanced Topics in Apigee**

Today, we’ll dive into **shared flows**, **flow variables**, and **rate limiting**, which are essential for building scalable, maintainable, and efficient API solutions.

---

#### **1. Shared Flows**

Shared flows are reusable sets of policies that can be applied across multiple API proxies, reducing redundancy and simplifying maintenance.

##### **When to Use Shared Flows**

- Common functionality like authentication, logging, or CORS headers.
- Standardized error handling across APIs.
- Global rate limiting or spike arrest policies.

---

##### **Creating a Shared Flow**

1. Navigate to **Develop > Shared Flows** in the Apigee dashboard.
2. Click **+ Shared Flow** and give it a name (e.g., `CommonErrorHandler`).
3. Add policies to the flow, such as logging or error handling. For example:
   ```xml
   <RaiseFault name="DefaultErrorHandler">
       <FaultResponse>
           <Set>
               <Headers>
                   <Header name="Content-Type">application/json</Header>
               </Headers>
               <Payload>{"error": "Something went wrong"}</Payload>
           </Set>
           <StatusCode>500</StatusCode>
       </FaultResponse>
   </RaiseFault>
   ```
4. Save and deploy the shared flow.

##### **Using a Shared Flow in a Proxy**

1. Open an API proxy and navigate to the **PreFlow** or **PostFlow** step.
2. Click **+ Step**, choose **Flow Callout**, and select the shared flow you created.
3. Save and deploy the proxy.

---

#### **2. Flow Variables**

Flow variables are placeholders used to access or modify runtime information, such as headers, query parameters, and request/response payloads.

##### **Common Flow Variables**

- `request.queryparam.<param_name>`: Access query parameters.
- `response.header.<header_name>`: Access response headers.
- `client.ip`: Get the client’s IP address.
- `apiproxy.name`: Retrieve the proxy’s name.

##### **Example: Logging Flow Variables**

Use the **Message Logging** policy to log client details:

```xml
<MessageLogging name="LogClientDetails">
    <Syslog>
        <Message>
            {"client_ip": "{client.ip}", "proxy_name": "{apiproxy.name}"}
        </Message>
    </Syslog>
</MessageLogging>
```

---

#### **3. Rate Limiting with Quota Policies**

Rate limiting ensures fair usage of APIs by limiting the number of requests a client can make within a defined period.

##### **Example: Add a Quota Policy**

1. Navigate to the **PreFlow** of your proxy.
2. Add the following Quota policy:
   ```xml
   <Quota name="DailyQuota">
       <Allow count="1000"/>
       <Interval>1</Interval>
       <TimeUnit>day</TimeUnit>
   </Quota>
   ```
3. Test by sending more than 1000 requests in a day to verify the limit.

---

#### **4. Hands-On Exercises**

1. **Build a Shared Flow for CORS**:

   - Create a shared flow that adds the required CORS headers. For example:
     ```xml
     <AssignMessage name="AddCORSHeaders">
         <Set>
             <Headers>
                 <Header name="Access-Control-Allow-Origin">*</Header>
                 <Header name="Access-Control-Allow-Methods">GET, POST, PUT, DELETE</Header>
             </Headers>
         </Set>
     </AssignMessage>
     ```
   - Apply this shared flow to an API proxy.

2. **Use Flow Variables for Dynamic Behavior**:

   - Modify a response message dynamically based on query parameters using the **Assign Message** policy:
     ```xml
     <AssignMessage name="DynamicMessage">
         <AssignTo type="response"/>
         <Set>
             <Payload>{"message": "{request.queryparam.message}"}</Payload>
         </Set>
     </AssignMessage>
     ```

3. **Experiment with Rate Limiting**:
   - Implement both **Quota** and **Spike Arrest** policies in the same proxy to see how they work together.

---

#### **5. Troubleshooting and Tips**

- **Shared Flows:** Ensure all proxies using a shared flow are compatible with its logic.
- **Flow Variables:** Use the **Trace** tool to inspect available flow variables in real-time.
- **Rate Limits:** Avoid too aggressive limits in production environments; monitor analytics to adjust thresholds.

---
