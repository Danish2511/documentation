### **Day 4: Working with Policies in Apigee**

---

#### **1. What are Policies?**

Policies in Apigee are reusable components that implement logic or enforce rules on API traffic. These include transformations, security, caching, and traffic control, among others. Policies allow you to modify API behavior without altering backend code.

---

#### **2. Common Policies in Apigee**

Here are a few widely used policies with their purposes:

1. **Security Policies:**

   - **Verify API Key:** Ensures that requests contain valid API keys.
   - **OAuth v2.0:** Enables secure access using tokens.

2. **Traffic Management Policies:**

   - **Quota:** Limits the number of requests within a defined time period.
   - **Spike Arrest:** Protects against sudden spikes in traffic.

3. **Mediation Policies:**

   - **JSON-to-XML and XML-to-JSON:** Converts data formats as needed.
   - **Assign Message:** Modifies requests or responses.

4. **Caching Policy:**
   - **Response Cache:** Caches backend responses to improve performance.

---

#### **3. Adding Policies to an API Proxy**

Let’s add a few policies to the proxy you created earlier (**SampleWeatherAPI**).

##### **Example 1: Add a Response Cache Policy**

1. Go to **Develop > API Proxies** and open your proxy.
2. Navigate to the **Develop** tab and click on the **PreFlow** step of the Proxy Endpoint.
3. Add the **Response Cache** policy:
   - Click **+ Step** and select **Response Cache** from the list.
   - Configure the policy to cache responses for 60 seconds:
     ```xml
     <ResponseCache name="CacheWeatherResponse">
         <DisplayName>Cache Weather Response</DisplayName>
         <Properties>
             <Property name="expirationTimeInSec">60</Property>
         </Properties>
     </ResponseCache>
     ```
4. Save and deploy the changes.
5. Test by making multiple requests to the same endpoint; observe that only the first call hits the backend, while subsequent responses are served from the cache.

##### **Example 2: Add a Spike Arrest Policy**

1. Navigate to the **PreFlow** step.
2. Add a **Spike Arrest** policy to limit requests to 5 per second:
   ```xml
   <SpikeArrest name="LimitRequests">
       <Rate>5ps</Rate>
   </SpikeArrest>
   ```
3. Deploy the proxy and test by sending more than 5 requests per second. Observe how excessive requests are throttled.

---

#### **4. Hands-On Exercise**

1. **Modify Your Proxy:**

   - Add a **Quota** policy to your proxy to limit requests to 100 per day.
   - Test with a looped script or a tool like Postman to verify the quota limit.

2. **Try Transformation Policies:**
   - Use **Assign Message** to modify the response body, e.g., add a custom message or remove fields.
   ```xml
   <AssignMessage name="ModifyResponse">
       <AssignTo type="response" />
       <Set>
           <Payload>{"message": "Customized response"}</Payload>
       </Set>
   </AssignMessage>
   ```

---

#### **5. Troubleshooting and Tips**

- **Policy Errors:** Review error messages in the Apigee Trace tool. Most issues arise from missing or misconfigured parameters.
- **Policy Placement:** Ensure policies are applied at the correct flow step (PreFlow, PostFlow, etc.).
- **Testing:** Use tools like curl or Postman to validate policies in isolation.

---
