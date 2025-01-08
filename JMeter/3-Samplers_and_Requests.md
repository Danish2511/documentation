# **Samplers and Requests**

## **Concepts:**

1. **What are Samplers in JMeter?**
   - **Samplers** are the elements in JMeter that send requests to the server. Each sampler defines a type of request that JMeter will send.
   - **Types of Samplers** include:
     - **HTTP Request**: The most commonly used sampler for testing web applications (requests over HTTP or HTTPS).
     - **FTP Request**: For testing FTP servers.
     - **JDBC Request**: For testing databases using JDBC (SQL queries).
     - **JMS Request**: For testing Java Message Service.
2. **Configuring an HTTP Request Sampler:**

   - The **HTTP Request Sampler** is used to send HTTP requests to the server and receive responses.
   - Key parameters to configure:
     - **Server Name or IP**: The target server (e.g., `www.example.com`).
     - **Protocol**: `http` or `https`.
     - **Method**: Choose `GET`, `POST`, `PUT`, etc., depending on the request type.
     - **Path**: The specific URL or endpoint (e.g., `/login`, `/product/123`).

3. **Other Types of Samplers:**
   - **FTP Request**: For interacting with FTP servers (useful for file uploads/downloads).
   - **JDBC Request**: For executing SQL queries against a database.
   - **JMS Request**: For testing message queues using JMS.

---

## **Hands-On Exercise:**

**Step 1: Adding Multiple HTTP Request Samplers**

1. Open your **Test Plan** from the previous day or create a new one.
2. Inside the **Thread Group**, right-click and select **Add → Sampler → HTTP Request**.
3. Configure the **HTTP Request Sampler**:
   - **Server Name or IP**: Set it to `www.example.com` or any valid domain.
   - **Protocol**: Set it to `https` (or `http` if needed).
   - **Path**: Set it to `/about` (this simulates a request to the “About Us” page).
4. Add another **HTTP Request Sampler** under the same **Thread Group** to simulate a request to a different page:
   - Set the **Path** to `/contact` for a contact page.

**Step 2: Configure the Request Method**

1. For one of the samplers, change the **Method** to `POST` (useful for submitting form data). For example, test a **login** form by setting the **Path** to `/login` and entering data in the **Parameters** section.

   - Add parameters such as:
     - **Name**: `username`
     - **Value**: `testuser`
     - **Name**: `password`
     - **Value**: `password123`

2. You can also change the **Method** to `PUT` or `DELETE` for testing other HTTP verbs, depending on the API you're testing.

**Step 3: Add Listeners for Results**

1. Right-click on the **Thread Group** and select **Add → Listener → View Results Tree** to view the request/response data.
2. Add another listener like **Summary Report** to gather overall statistics (e.g., throughput, response times).

**Step 4: Running the Test Plan**

1. Click the **Start** button (green arrow).
2. In the **View Results Tree**, you will see the requests being made to the `/about`, `/contact`, and `/login` pages.
   - Each HTTP Request will show status, response time, and data sent/received.

---

## **Task:**

1. **Run Your Test with Multiple Requests:**

   - Make sure the test includes multiple **HTTP Request Samplers** (different paths and methods).
   - Examine the responses for each HTTP request. Look at the status code, response time, and data returned.

2. **Experiment with POST Requests:**
   - Test submitting form data (e.g., login) using the `POST` method.
   - Check the results to see if the server processes the form correctly.

---

## **Summary:**

Today, you learned:

- The role of **Samplers** in JMeter and how they send requests to the server.
- How to configure **HTTP Request Samplers** for different request types (GET, POST, etc.).
- How to add multiple samplers to simulate more complex user behavior (e.g., navigating through pages, submitting forms).
- You ran tests with multiple HTTP requests and explored responses using **Listeners**.

---
