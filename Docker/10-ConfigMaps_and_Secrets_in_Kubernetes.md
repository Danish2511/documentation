# **ConfigMaps and Secrets in Kubernetes**

## **Concepts:**

Kubernetes provides ways to manage configuration and sensitive information that applications require. These are typically externalized from the application code to keep it more flexible and secure.

1. **ConfigMaps**:

   - **ConfigMaps** are used to store non-sensitive configuration data in key-value pairs. These can be environment variables, command-line arguments, or configuration files used by your applications.
   - ConfigMaps allow you to decouple environment-specific configuration from the container images, making the application more portable.

2. **Secrets**:
   - **Secrets** are used to store sensitive data, such as passwords, tokens, certificates, or API keys. Secrets are similar to ConfigMaps, but Kubernetes ensures they are more securely managed and encoded in base64 format to prevent accidental exposure in the cluster.
   - It’s important to note that Secrets should be accessed securely (e.g., through environment variables, volumes) and encrypted in transit.

## **Key Topics:**

1. **Creating and Using ConfigMaps**:

   - **ConfigMaps** are often used for storing things like application configuration files or environment variables.

2. **Creating and Using Secrets**:
   - **Secrets** should be stored and accessed securely, especially for sensitive data like credentials or API keys.

---

## **Hands-On Exercise:**

### 1. **Creating a ConfigMap**:

Let’s create a ConfigMap to store application configuration data.

- **Step 1: Create a ConfigMap**:
  You can create a ConfigMap from a file or directly from key-value pairs. Here, we will create one with key-value pairs.

  Create a file named `configmap-example.yaml` with the following content:

  ```yaml
  apiVersion: v1
  kind: ConfigMap
  metadata:
    name: app-config
  data:
    app_name: "MyApp"
    log_level: "debug"
    max_connections: "100"
  ```

  - This YAML file defines a ConfigMap named `app-config` with some simple application configuration parameters.

- **Step 2: Apply the ConfigMap**:
  Apply the ConfigMap to your Kubernetes cluster:

  ```bash
  kubectl apply -f configmap-example.yaml
  ```

- **Step 3: Verify the ConfigMap**:
  Check that the ConfigMap has been created:

  ```bash
  kubectl get configmaps
  ```

  You should see `app-config` listed.

- **Step 4: Use the ConfigMap in a Pod**:
  Create a pod that uses this ConfigMap. Create a file named `pod-with-configmap.yaml` with the following content:

  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: app-pod
  spec:
    containers:
      - name: nginx
        image: nginx
        envFrom:
          - configMapRef:
              name: app-config
  ```

  - This YAML defines a pod running Nginx, with environment variables taken from the `app-config` ConfigMap.

- **Step 5: Apply the Pod**:
  Apply the pod definition:

  ```bash
  kubectl apply -f pod-with-configmap.yaml
  ```

- **Step 6: Verify the Pod**:
  Check the pod's environment variables:

  ```bash
  kubectl exec -it app-pod -- env
  ```

  You should see the variables `app_name`, `log_level`, and `max_connections` listed.

- **Step 7: Clean Up**:
  After testing, delete the pod and ConfigMap:
  ```bash
  kubectl delete pod app-pod
  kubectl delete configmap app-config
  ```

### 2. **Creating and Using Secrets**:

Now, let’s create a Secret to store sensitive data, such as a database password.

- **Step 1: Create a Secret**:
  You can create Secrets from literal values or files. Here, we’ll create one from a literal value for the database password.

  Run the following command to create a Secret:

  ```bash
  kubectl create secret generic db-secret --from-literal=password=mysecretpassword
  ```

  This command creates a Secret named `db-secret` with the key `password` and value `mysecretpassword`.

- **Step 2: Verify the Secret**:
  To verify the Secret has been created:

  ```bash
  kubectl get secrets
  ```

  The secret will be listed but note that it’s base64-encoded.

- **Step 3: Use the Secret in a Pod**:
  Let’s create a pod that uses this Secret. Create a file named `pod-with-secret.yaml` with the following content:

  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: app-with-secret
  spec:
    containers:
      - name: nginx
        image: nginx
        env:
          - name: DB_PASSWORD
            valueFrom:
              secretKeyRef:
                name: db-secret
                key: password
  ```

  - This YAML defines a pod that uses the `db-secret` Secret to set the environment variable `DB_PASSWORD`.

- **Step 4: Apply the Pod**:
  Apply the pod definition:

  ```bash
  kubectl apply -f pod-with-secret.yaml
  ```

- **Step 5: Verify the Secret in the Pod**:
  Exec into the pod to check the environment variable:

  ```bash
  kubectl exec -it app-with-secret -- printenv | grep DB_PASSWORD
  ```

  You should see the `DB_PASSWORD` variable with the value `mysecretpassword`.

- **Step 6: Clean Up**:
  After testing, delete the pod and Secret:
  ```bash
  kubectl delete pod app-with-secret
  kubectl delete secret db-secret
  ```

---

## **Real-World Example:**

In a real-world scenario, ConfigMaps and Secrets are used extensively:

- **ConfigMaps** are used for things like:

  - Application settings (e.g., database URLs, API keys).
  - External configuration for microservices (e.g., Kafka topic names, logging levels).
  - Configuration files that may be mounted into containers.

- **Secrets** are used for:
  - Storing sensitive information like passwords, API tokens, SSH keys, or certificates.
  - Ensuring that sensitive data is handled securely and not accidentally exposed in logs or other places.

For example, in a **database application**, you might use a ConfigMap for non-sensitive configuration like host addresses and a Secret to store the database password. This approach helps keep sensitive data safe while allowing other configuration to be more easily managed.

---

## **Key Takeaways for Day 10:**

- **ConfigMaps** are used to store non-sensitive configuration data and can be injected into pods as environment variables or mounted as files.
- **Secrets** are used to securely store sensitive information, like passwords, and can also be injected into pods as environment variables or mounted as files.
- Kubernetes provides a secure and efficient way to manage configurations and secrets separately from application code.

---
