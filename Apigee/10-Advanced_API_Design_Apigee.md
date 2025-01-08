### **Day 9: Advanced API Design with Apigee**

Today, we’ll focus on advanced API design principles and Apigee features to enhance scalability, maintainability, and usability of your APIs. Topics include shared flows, conditional flows, and modular design.

---

#### **1. Shared Flows**

**Shared flows** allow you to reuse common logic across multiple proxies, improving consistency and reducing duplication. Typical use cases include authentication, logging, and error handling.

##### **Steps to Create and Use a Shared Flow**

1. **Create a Shared Flow:**

   - Navigate to **Develop > Shared Flows** in the Apigee dashboard.
   - Click **+ Shared Flow**, give it a name (e.g., "AuthAndLogging"), and click **Create**.
   - Add policies like **Verify API Key**, **Message Logging**, or **Raise Fault**.

2. **Include a Shared Flow in an API Proxy:**

   - Open the API proxy where you want to use the shared flow.
   - In the proxy flow, add a **Flow Callout** step pointing to the shared flow:
     ```xml
     <FlowCallout name="CallSharedFlow">
         <SharedFlowBundle>AuthAndLogging</SharedFlowBundle>
     </FlowCallout>
     ```

3. **Deploy and Test:**
   - Deploy both the shared flow and the API proxy.
   - Test the API proxy to verify shared flow execution.

---

#### **2. Conditional Flows and Route Rules**

**Conditional flows** allow you to handle requests differently based on conditions, such as headers, query parameters, or payload content.

##### **Example: Routing Based on Query Parameter**

```xml
<Flows>
    <Flow name="WeatherAPI">
        <Condition>(request.verb == "GET") and (request.queryparam.location != null)</Condition>
        <Request>
            <Step>
                <Name>AssignLocation</Name>
            </Step>
        </Request>
    </Flow>
    <Flow name="HealthCheck">
        <Condition>(request.path == "/health")</Condition>
        <Response>
            <Step>
                <Name>HealthCheckResponse</Name>
            </Step>
        </Response>
    </Flow>
</Flows>
```

##### **Use Case: Load Balancing**

You can implement conditional logic to route requests to different backend endpoints based on geographic location or traffic.

---

#### **3. API Versioning**

API versioning helps you manage changes to your API while maintaining compatibility for existing clients.

##### **Best Practices for Versioning in Apigee**

- Use version numbers in the URL path:
  ```
  https://api.example.com/v1/weather
  ```
- Create separate API proxies for each version.
- Use shared flows for common logic across versions.

---

#### **4. Customizing Response Formats**

Provide different response formats (e.g., JSON, XML) based on client requests.

##### **Implementation Steps**

1. **Check the Request Header:**
   ```xml
   <Condition>(request.header.Accept == "application/xml")</Condition>
   ```
2. **Use Assign Message to Modify the Response:**
   ```xml
   <AssignMessage name="SetXMLResponse">
       <Set>
           <Headers>
               <Header name="Content-Type">application/xml</Header>
           </Headers>
           <Payload>
               <Your XML payload here>
           </Payload>
       </Set>
   </AssignMessage>
   ```

---

#### **5. Modular API Design**

Break down large APIs into smaller, modular APIs to improve maintainability.

##### **Steps to Design Modular APIs**

1. **Create Microservices for Core Features:**
   - Example: Separate APIs for **user management**, **authentication**, and **reporting**.
2. **Use API Composition:**
   - Create a composite API proxy to orchestrate calls to multiple microservices.

---

#### **6. Hands-On Exercises**

1. **Create a Shared Flow for Logging:**

   - Include a **Message Logging** policy and apply it to multiple API proxies.

2. **Implement Conditional Routing:**

   - Route requests to a different backend based on a custom header (e.g., `X-Region`).

3. **Add Versioning to an API:**

   - Clone an existing proxy, update the version in the base path, and test.

4. **Support Multiple Response Formats:**
   - Modify an existing proxy to respond with either JSON or XML based on the `Accept` header.

---

#### **7. Best Practices and Tips**

- **Reusability:** Use shared flows for repetitive tasks like authentication and logging.
- **Modularity:** Design smaller, focused APIs that can be composed into larger systems.
- **Versioning:** Always communicate breaking changes to API clients and provide deprecation timelines.
- **Performance:** Minimize latency in composite APIs by limiting the number of backend calls.
- **Scalability:** Use rate-limiting and caching policies to optimize API performance.

---
