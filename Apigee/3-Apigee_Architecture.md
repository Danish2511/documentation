### **Day 2: Understanding Apigee Architecture**

---

#### **1. Key Components of Apigee**

Apigee consists of several critical components, each serving a specific purpose in the API lifecycle:

- **API Proxies**  
   These are the core of Apigee. An API proxy acts as a facade between the client and the backend service. It provides abstraction, security, and other features like rate limiting or caching.  

- **Policies**  
   Reusable logic components applied to API traffic. Examples include transformations, authentication, caching, and rate limiting.

- **Developer Portal**  
   A consumer-facing platform where developers can explore APIs, register apps, and get API keys.

- **Analytics**  
   Insights into API usage, traffic patterns, error rates, and performance metrics.

---

#### **2. API Proxy Workflow**

Here’s how Apigee processes a typical API request:

1. **Client Request:**  
   The client (e.g., mobile app, frontend application) sends a request to the API proxy.

2. **Proxy Processing:**  
   The proxy enforces policies like authentication, rate limiting, or transformations.

3. **Backend Service Call:**  
   If the request passes all checks, it is forwarded to the backend service.

4. **Backend Response:**  
   The response from the backend is processed by the proxy (e.g., logging or additional transformations) before returning to the client.

---

#### **3. Apigee Edge Architecture**

Apigee's architecture has two main planes:

- **Management Plane:**  
   This is where you design, deploy, and monitor APIs. It includes tools like the Apigee dashboard and API management features.

- **Runtime Plane:**  
   This is where API traffic is handled. It processes incoming requests, applies policies, and communicates with backend services.

---

#### **Real-World Example**

Consider a ride-hailing service like Uber. Apigee acts as a gateway between the mobile app and backend services like user authentication, trip management, and payment processing.

---

#### **Hands-On Exercise**

1. **Draw the Workflow:**

   - Sketch out the following workflow on paper or a tool like Lucidchart:\n 1. Client -> API Proxy -> Policies -> Backend -> Response\n\n2. **Explore Apigee Dashboard:**
   - Go to the \"Trace\" section to see how requests are processed in real-time (we'll explore it more in Day 4).

2. **Identify Components in Your Domain:**
   - Think about a system you're familiar with and identify which parts could map to Apigee's architecture.

---

#### **Troubleshooting and Tips**

- If unsure how traffic flows through Apigee, use the **Trace** tool in the dashboard to debug.\n- For complex systems, use shared flows to manage reusable policies (we\u2019ll cover this in Day 5).

---
