# **Parameterization and Data-Driven Testing**

## **Concepts:**

1. **What is Parameterization in JMeter?**
   - **Parameterization** allows you to run a test with multiple sets of input data. This is important for testing how the system behaves with different user inputs.
   - For example, you can send different usernames and passwords to a login page, or test multiple search terms for an e-commerce site.
2. **Using CSV Data Set Config:**

   - **CSV Data Set Config** is a configuration element in JMeter that reads data from a CSV file and uses it as input for your requests.
   - The CSV file typically contains multiple rows of data, with each row representing a different set of parameters.
   - The **CSV Data Set Config** reads the values and dynamically substitutes them into your requests.

3. **Dynamic Parameters:**
   - You can use dynamic parameters in your HTTP requests (such as `{username}`) and map them to variables defined in the CSV file.
   - This allows you to simulate multiple users with different login credentials, or test multiple product IDs in a shopping cart scenario.

---

## **Hands-On Exercise:**

**Step 1: Preparing a CSV File**

1. Create a simple CSV file (`data.csv`) that contains user data:

   ```
   username,password
   user1,password1
   user2,password2
   user3,password3
   ```

2. Save this file in the **bin** folder of your JMeter directory or in any folder that’s accessible to your JMeter project.

**Step 2: Adding CSV Data Set Config**

1. Open your **Test Plan** and **Thread Group**.
2. Right-click on the **Thread Group** and select **Add → Config Element → CSV Data Set Config**.

3. **Configure the CSV Data Set Config:**
   - **Filename**: Browse to the CSV file you created (`data.csv`).
   - **Variable Names**: Set this to `username,password` (the column names in your CSV file).
   - **Delimiter**: The default delimiter for CSV files is a comma (`,`), but you can change it if needed.
   - **Recycle on EOF?**: Set it to `True` if you want JMeter to loop through the CSV file again when the end is reached. Set it to `False` if you want it to stop after reading the file once.
   - **Stop Thread on EOF?**: Set it to `False` if you want all threads to continue even after the CSV file is exhausted.

**Step 3: Using Parameters in HTTP Requests**

1. In one of your **HTTP Request** samplers (e.g., the **Login** page), replace static values with dynamic variables:
   - For example:
     - In the **username** field, use `${username}`.
     - In the **password** field, use `${password}`.
2. This will cause JMeter to pick the values from the CSV file for each user.

**Step 4: Running the Test**

1. **Run the Test:**
   - Click the **Start** button (green arrow).
   - In the **View Results Tree**, observe the requests being sent with the respective usernames and passwords for each iteration.
   - JMeter will iterate over each line in the CSV file, using the data for each request.

**Step 5: Validating Results**

1. Check the **View Results Tree** listener for the responses.
   - Make sure the correct usernames and passwords are being used in each request.

---

## **Task:**

1. **Create a Data-Driven Test:**

   - Create a CSV file with different inputs (e.g., product IDs, search terms, user credentials).
   - Use **CSV Data Set Config** to parameterize your test and simulate multiple user interactions.
   - Run the test and validate the data being sent in the requests.

2. **Experiment with Multiple Parameters:**
   - Add more columns to the CSV file (e.g., `email`, `age`) and use them in your test plan.
   - Observe how the input data is passed into the requests dynamically.

---

## **Summary:**

Today, you learned:

- The concept of **parameterization** in JMeter to simulate different user inputs using external data (CSV files).
- How to use the **CSV Data Set Config** element to load data and use it in requests.
- How to configure dynamic parameters in HTTP requests (e.g., `${username}`, `${password}`) based on CSV data.
- You ran a data-driven test and validated that multiple sets of input data were used in the requests.

---
