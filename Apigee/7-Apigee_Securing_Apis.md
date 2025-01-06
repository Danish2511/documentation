### **Day 6: Securing APIs with OAuth and API Keys**

Securing APIs is a crucial part of API management. Today, we’ll focus on implementing security mechanisms using **OAuth 2.0** and **API keys** in Apigee.

---

#### **1. Overview of API Security**

- **OAuth 2.0**: An open-standard authorization protocol allowing secure access to APIs without sharing credentials. Clients request access tokens to authenticate.
- **API Keys**: Simple tokens that allow applications to identify themselves when calling APIs.

---

#### **2. Implementing API Key Validation**

API keys are a straightforward way to secure APIs and ensure only authorized applications can access them.

##### **Steps to Implement API Key Validation**

1. **Enable API Key Validation:**

   - Open your API proxy in the Apigee dashboard.
   - Navigate to the **PreFlow** of the Proxy Endpoint.
   - Add the **Verify API Key** policy:
     ```xml
     <VerifyAPIKey name="VerifyKey">
         <APIKey ref="request.queryparam.apikey"/>
     </VerifyAPIKey>
     ```

2. **Provision an API Key:**

   - Navigate to **Publish > Developers** and create a developer account.
   - Create an application for the developer and associate it with your API product.
   - Generate and retrieve the API key for the application.

3. **Test the Proxy:**
   - Use a tool like Postman or curl to call your API with the API key:
     ```bash
     curl "https://<your-org-name>-<environment>.apigee.net/weather?apikey=<your-api-key>"
     ```

##### **Error Handling for Invalid Keys**

Use the **Raise Fault** policy to customize error responses for missing or invalid API keys.

---

#### **3. Securing APIs with OAuth 2.0**

##### **How OAuth 2.0 Works in Apigee**

1. A client application requests an access token by providing credentials (e.g., client ID and secret).
2. Apigee validates the credentials and issues an access token.
3. The client uses the token to access the protected API.

##### **Steps to Implement OAuth 2.0**

1. **Create an OAuth Token Endpoint:**

   - Create a new API proxy to act as the OAuth authorization server.
   - Add the **OAuth v2.0 Generate Access Token** policy:
     ```xml
     <OAuthV2 name="GenerateAccessToken">
         <Operation>GenerateAccessToken</Operation>
         <GrantType>client_credentials</GrantType>
     </OAuthV2>
     ```
   - Deploy the proxy and test token generation:
     ```bash
     curl -X POST "https://<your-org-name>-<environment>.apigee.net/oauth/token" \
     -d "grant_type=client_credentials&client_id=<client-id>&client_secret=<client-secret>"
     ```

2. **Secure the Target API Proxy with OAuth:**

   - Open your existing API proxy (e.g., **SampleWeatherAPI**).
   - Add the **Verify Access Token** policy in the **PreFlow**:
     ```xml
     <OAuthV2 name="VerifyAccessToken">
         <Operation>VerifyAccessToken</Operation>
         <AccessToken ref="request.header.Authorization" />
     </OAuthV2>
     ```

3. **Test the Flow:**
   - Use the token generated in step 1 to access the secured API:
     ```bash
     curl -H "Authorization: Bearer <access-token>" \
     "https://<your-org-name>-<environment>.apigee.net/weather"
     ```

---

#### **4. Hands-On Exercises**

1. **Implement Both API Key and OAuth Validation:**

   - Combine **Verify API Key** and **Verify Access Token** policies in the same API proxy.

2. **Customize Token Expiration:**

   - Configure the OAuth policy to set token expiration time to 1 hour:
     ```xml
     <ExpiresIn>3600</ExpiresIn>
     ```

3. **Simulate Invalid Scenarios:**
   - Test your proxy with missing or invalid API keys and tokens.
   - Observe and improve error responses with the **Raise Fault** policy.

---

#### **5. Best Practices and Troubleshooting Tips**

- **Use HTTPS:** Always deploy APIs over HTTPS to secure sensitive data like tokens.
- **Scope Tokens:** Use OAuth scopes to grant fine-grained access control to APIs.
- **Monitor Tokens:** Use Apigee Analytics to track token usage and identify potential misuse.
- **Debugging OAuth Issues:** Use the **Trace** tool to verify that tokens are correctly validated.

---
