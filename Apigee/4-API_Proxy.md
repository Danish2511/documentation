### **Day 3: Creating Your First API Proxy**

---

#### **1. What is an API Proxy?**

An API proxy acts as an intermediary between the client and your backend service. It abstracts backend complexity, adds security, and enables monitoring and analytics. Proxies allow you to implement policies like authentication, rate limiting, and caching without altering the backend.

---

#### **2. Types of Proxies in Apigee**

- **Reverse Proxy:** Forwards requests to an existing backend service.
- **Target Endpoint-Free Proxy:** Used for mock APIs or processing requests without a backend.

---

#### **3. Creating an API Proxy**

Let’s create a simple reverse proxy to expose a public REST API:

1. **Navigate to the Apigee Console:**

   - Log in to your Apigee account and go to **Develop > API Proxies**.

2. **Click \"+ Proxy\" to Create a New Proxy:**

   - Choose **Reverse Proxy** as the proxy type.
   - Enter the following details:
     - **Name:** `SampleWeatherAPI`
     - **Base Path:** `/weather`
     - **Target URL:** `https://api.weatherapi.com/v1/current.json` (Replace with your desired backend endpoint).

3. **Configure Target Endpoint:**

   - Apigee will handle requests to `/weather` and forward them to the target URL.

4. **Deploy the Proxy:**

   - Choose an environment (e.g., `test` or `prod`) and deploy the proxy.

5. **Test Your Proxy:**
   - Use a tool like Postman or curl to send requests:
     ```bash
     curl "https://<your-org-name>-<environment>.apigee.net/weather?key=<api_key>&q=London"
     ```

---

#### **4. Hands-On Exercise**

Complete the following steps to build your confidence:

1. **Create a Proxy for Any Public API:**

   - Use a service like `https://jsonplaceholder.typicode.com/`.
   - Proxy `/posts` to `/sample-posts`.

2. **Deploy and Test:**
   - Send requests to your proxy endpoint and verify responses.

---

#### **5. Troubleshooting and Tips**

- **Connection Errors:** Ensure the target URL is reachable from Apigee.\n- **Base Path Conflicts:** Avoid reusing base paths already in use.\n- **Debugging:** Use the **Trace** tool in the Apigee dashboard to identify issues.\n\n---

---
[embed](docs/Create_an_API_proxy.pdf)[/embed]