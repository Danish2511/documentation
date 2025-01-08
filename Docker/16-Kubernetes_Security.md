# **Kubernetes Security**

## **Concepts:**

Kubernetes security involves protecting the Kubernetes cluster and the applications running within it. It includes managing user access, securing sensitive data, controlling network communication, and ensuring the integrity of workloads.

1. **RBAC (Role-Based Access Control)**:

   - RBAC is a security mechanism used to control who can access resources within a Kubernetes cluster.
   - Kubernetes defines **Roles** and **RoleBindings** to grant permissions to users or service accounts.
     - **Role**: Defines a set of permissions (verbs like `get`, `list`, `create`, etc.) for resources within a specific namespace.
     - **ClusterRole**: Similar to Role but applicable across the entire cluster.
     - **RoleBinding**: Grants a **Role** to a user or service account within a specific namespace.
     - **ClusterRoleBinding**: Grants a **ClusterRole** across the entire cluster.

2. **Service Accounts**:

   - A **ServiceAccount** provides an identity for processes running in a pod to interact with the Kubernetes API.
   - Kubernetes automatically creates a default service account for every namespace, but you can create custom service accounts as needed.

3. **Network Policies**:

   - **Network Policies** define rules that control the traffic flow between pods.
   - They allow you to specify which pods can communicate with each other, providing network isolation between services.
   - You can restrict incoming and outgoing traffic based on labels, namespaces, and IP blocks.

4. **Secrets and ConfigMaps**:

   - **Secrets**: Securely store sensitive data such as passwords, API keys, and certificates.
     - Secrets are encoded in base64 and can be mounted into pods as environment variables or volumes.
   - **ConfigMaps**: Store non-sensitive configuration data, such as environment variables or configuration files, that can be consumed by containers.

5. **Pod Security Policies (PSP)** (Deprecated in Kubernetes 1.21):
   - PSPs were used to enforce security settings for pod specifications (e.g., preventing privilege escalation, disallowing the use of host namespaces).
   - As of Kubernetes 1.21, PSPs have been deprecated and will be removed in 1.25. However, alternatives like **OPA-Gatekeeper** can enforce similar security policies.

---

## **Key Topics:**

- **RBAC (Role-Based Access Control)**: Managing access to resources using roles and bindings.
- **Service Accounts**: Identity management for pods.
- **Network Policies**: Securing pod communication and isolating services.
- **Secrets and ConfigMaps**: Storing and managing sensitive and non-sensitive configuration data.
- **Pod Security Policies**: (Deprecated, but good to know alternatives like OPA-Gatekeeper).

---

## **Hands-On Exercise:**

### 1. **Create and Apply RBAC Roles**:

Let’s start by creating roles and role bindings to control access within a namespace.

- **Step 1: Create a Role**:
  Create a file `role.yaml` to define a role with permissions to list pods in the `default` namespace:

  ```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    namespace: default
    name: pod-reader
  rules:
    - apiGroups: [""]
      resources: ["pods"]
      verbs: ["list"]
  ```

- **Step 2: Apply the Role**:
  Apply the role definition to your cluster:

  ```bash
  kubectl apply -f role.yaml
  ```

- **Step 3: Create a RoleBinding**:
  Create a file `rolebinding.yaml` to bind the `pod-reader` role to a service account:

  ```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: RoleBinding
  metadata:
    name: read-pods-binding
    namespace: default
  subjects:
    - kind: ServiceAccount
      name: default
      namespace: default
  roleRef:
    kind: Role
    name: pod-reader
    apiGroup: rbac.authorization.k8s.io
  ```

- **Step 4: Apply the RoleBinding**:
  Apply the role binding:

  ```bash
  kubectl apply -f rolebinding.yaml
  ```

- **Step 5: Verify Access**:
  Verify that the service account has the permissions:

  ```bash
  kubectl auth can-i list pods --as=system:serviceaccount:default:default --namespace=default
  ```

  You should see `yes`, indicating the service account has permission to list pods.

### 2. **Create a Service Account**:

Now, let’s create a custom service account.

- **Step 1: Create the Service Account**:
  Create a file `serviceaccount.yaml`:

  ```yaml
  apiVersion: v1
  kind: ServiceAccount
  metadata:
    name: custom-service-account
  ```

- **Step 2: Apply the Service Account**:
  Apply the service account:

  ```bash
  kubectl apply -f serviceaccount.yaml
  ```

- **Step 3: Verify the Service Account**:
  Verify the service account has been created:
  ```bash
  kubectl get serviceaccounts
  ```

### 3. **Create and Use a Network Policy**:

Let’s now define a network policy to restrict pod communication.

- **Step 1: Create a Network Policy**:
  Create a file `network-policy.yaml` to restrict all traffic except from a specific label:

  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: NetworkPolicy
  metadata:
    name: allow-only-from-app
  spec:
    podSelector: {}
    ingress:
      - from:
          - podSelector:
              matchLabels:
                role: app
  ```

- **Step 2: Apply the Network Policy**:
  Apply the network policy:

  ```bash
  kubectl apply -f network-policy.yaml
  ```

- **Step 3: Verify Network Policy**:
  Verify that the network policy is working by trying to access a pod without the `role: app` label and see if the access is blocked.

### 4. **Create and Access Kubernetes Secrets**:

Now, let’s create a **Secret** to store sensitive data.

- **Step 1: Create a Secret**:
  Create a file `secret.yaml` to define a secret for storing an API key:

  ```yaml
  apiVersion: v1
  kind: Secret
  metadata:
    name: api-key
  type: Opaque
  data:
    api-key: c29tZXNlY3JldGFwaXg= # "somesecretkey" in base64
  ```

- **Step 2: Apply the Secret**:
  Apply the secret:

  ```bash
  kubectl apply -f secret.yaml
  ```

- **Step 3: Access the Secret in a Pod**:
  Create a pod and mount the secret as an environment variable:

  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: secret-reader
  spec:
    containers:
      - name: secret-reader
        image: busybox
        env:
          - name: API_KEY
            valueFrom:
              secretKeyRef:
                name: api-key
                key: api-key
  ```

- **Step 4: Verify the Secret**:
  Apply the pod and verify that the secret is available as an environment variable:

  ```bash
  kubectl apply -f secret-reader.yaml
  kubectl exec -it secret-reader -- printenv API_KEY
  ```

  You should see the decoded API key.

---

## **Real-World Example:**

In a real-world scenario, Kubernetes security is crucial for ensuring that only authorized users or services can access certain resources, and that sensitive data is protected. For example:

- **Service Accounts** are used to manage permissions for applications running inside your pods.
- **RBAC** helps enforce the principle of least privilege, ensuring that users or services only have the permissions they need.
- **Network Policies** provide isolation between services to prevent unauthorized communication between pods, for example, preventing internal communication between a payment service and a user service.
- **Secrets** store sensitive information securely, preventing hardcoded credentials in application code.

For instance, in a **multi-tenant platform** like a SaaS application, you might use RBAC and network policies to ensure that different customers' data is isolated and that only authorized users can access specific resources.

---

## **Key Takeaways for Day 16:**

- **RBAC** is essential for controlling user and service access to Kubernetes resources.
- **Service Accounts** provide identities for pods to interact with the Kubernetes API.
- **Network Policies** enable network-level security by controlling which pods can communicate with each other.
- **Secrets and ConfigMaps** store and manage configuration data securely.

---
