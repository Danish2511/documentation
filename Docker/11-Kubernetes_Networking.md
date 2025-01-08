# **Kubernetes Networking**

## **Concepts:**

Networking is a key part of any Kubernetes environment, as it enables communication between pods, services, and external resources. Kubernetes provides several abstractions and components to manage networking in a cluster.

1. **Kubernetes Networking Model**:

   - In Kubernetes, every pod gets its own unique IP address, which allows pods to communicate with each other directly.
   - Pods within the same cluster can reach each other by their IP addresses. However, pods in different clusters or environments often communicate through services or ingress resources.

2. **Services**:

   - **Services** in Kubernetes provide a stable IP address and DNS name for a set of pods, enabling them to be accessed by other pods or external clients.
   - Kubernetes supports different types of services to manage network traffic to pods, including **ClusterIP**, **NodePort**, **LoadBalancer**, and **ExternalName**.

3. **Ingress**:

   - **Ingress** is an API object that manages external access to services, typically HTTP or HTTPS traffic. It allows routing based on hostnames, paths, and can integrate with SSL/TLS termination.

4. **Network Policies**:
   - **Network Policies** allow you to control the traffic flow between pods, enabling more granular control over which pods can communicate with each other.

---

## **Key Topics:**

- **ClusterIP**: The default service type that exposes a service inside the cluster.
- **NodePort**: Exposes a service on a static port across all nodes.
- **LoadBalancer**: Exposes a service externally using a cloud provider’s load balancer.
- **ExternalName**: Maps a service to an external DNS name.
- **Ingress**: Provides HTTP(S) routing to services.
- **Network Policies**: Allows defining rules for controlling pod-to-pod communication.

---

## **Hands-On Exercise:**

### 1. **Creating a Service**:

Let’s create a service to expose an Nginx container inside the Kubernetes cluster.

- **Step 1: Create a Deployment for Nginx**:
  First, create a simple deployment for Nginx. Create a file named `nginx-deployment.yaml`:

  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: nginx-deployment
  spec:
    replicas: 3
    selector:
      matchLabels:
        app: nginx
    template:
      metadata:
        labels:
          app: nginx
      spec:
        containers:
          - name: nginx
            image: nginx
            ports:
              - containerPort: 80
  ```

  - This deployment will run 3 replicas of the Nginx container.

- **Step 2: Apply the Deployment**:
  Apply the deployment to create the pods:

  ```bash
  kubectl apply -f nginx-deployment.yaml
  ```

- **Step 3: Create a Service (ClusterIP)**:
  Now, expose the Nginx pods through a **ClusterIP** service. Create a file named `nginx-service.yaml`:

  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: nginx-service
  spec:
    selector:
      app: nginx
    ports:
      - protocol: TCP
        port: 80
        targetPort: 80
    type: ClusterIP
  ```

  - This service will expose port 80 inside the cluster and map it to the same port on the Nginx containers.

- **Step 4: Apply the Service**:
  Apply the service to expose Nginx:

  ```bash
  kubectl apply -f nginx-service.yaml
  ```

- **Step 5: Verify the Service**:
  Check that the service is running:

  ```bash
  kubectl get services
  ```

  You should see `nginx-service` listed with an internal cluster IP.

- **Step 6: Access the Service from Inside the Cluster**:
  To test the service, you can run a temporary pod that uses the service. Create a simple pod definition `test-pod.yaml`:

  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: test-pod
  spec:
    containers:
      - name: curl
        image: curlimages/curl
        command:
          - sleep
          - "3600"
  ```

  Apply the pod:

  ```bash
  kubectl apply -f test-pod.yaml
  ```

  Once the pod is running, exec into it and try to access the Nginx service:

  ```bash
  kubectl exec -it test-pod -- curl nginx-service:80
  ```

  You should see the HTML content returned from the Nginx service.

### 2. **Exposing a Service with NodePort**:

Next, let’s expose the Nginx service using a **NodePort**, which allows you to access the service externally through a port on any node in the cluster.

- **Step 1: Modify the Service**:
  Edit the `nginx-service.yaml` to change the service type to `NodePort`:

  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: nginx-service
  spec:
    selector:
      app: nginx
    ports:
      - protocol: TCP
        port: 80
        targetPort: 80
        nodePort: 30080
    type: NodePort
  ```

  - This configuration exposes the service on port `30080` on each node of the cluster.

- **Step 2: Apply the Updated Service**:
  Apply the modified service:

  ```bash
  kubectl apply -f nginx-service.yaml
  ```

- **Step 3: Access the Service Externally**:
  If you are running Kubernetes on your local machine (e.g., using Minikube or Docker Desktop), you can access the service using any node's IP address and the `nodePort` (e.g., `http://<node-ip>:30080`).

  If you're running Kubernetes on a cloud provider, the external IP of the node can be used along with the NodePort.

### 3. **Setting Up Ingress for HTTP Routing**:

Let’s now create an **Ingress** to manage HTTP traffic to the service.

- **Step 1: Create an Ingress Resource**:
  First, make sure you have an Ingress controller set up. If you are using Minikube, you can enable the Ingress controller using:

  ```bash
  minikube addons enable ingress
  ```

  Now, create an `ingress.yaml` file for Nginx:

  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: nginx-ingress
  spec:
    rules:
      - host: nginx.local
        http:
          paths:
            - path: /
              pathType: Prefix
              backend:
                service:
                  name: nginx-service
                  port:
                    number: 80
  ```

  - This Ingress will route traffic to the `nginx-service` when the hostname `nginx.local` is accessed.

- **Step 2: Apply the Ingress Resource**:
  Apply the Ingress:

  ```bash
  kubectl apply -f ingress.yaml
  ```

- **Step 3: Access the Service via Ingress**:
  - Add `nginx.local` to your `/etc/hosts` file, mapping it to your Minikube or Kubernetes cluster's external IP:
    ```
    127.0.0.1 nginx.local
    ```
  - Now, access the service via `http://nginx.local`.

### 4. **Clean Up**:

After testing, delete the deployment, service, and ingress:

```bash
kubectl delete deployment nginx-deployment
kubectl delete service nginx-service
kubectl delete ingress nginx-ingress
```

---

## **Real-World Example:**

In a real-world scenario, services and ingress are typically used to manage traffic between various microservices and expose external APIs. For instance:

- **ClusterIP** is often used for internal communication between microservices within the cluster.
- **NodePort** or **LoadBalancer** can be used when you need external access to your service, such as a public API or web application.
- **Ingress** is commonly used in production to manage external HTTP(S) traffic, providing SSL/TLS termination and routing traffic based on URLs or hostnames.

For example, in an **e-commerce platform**, you might use an **Ingress** to expose services for different parts of the application (e.g., `/api` for the backend API, `/checkout` for the checkout service), and **Services** would handle routing to these different microservices.

---

## **Key Takeaways for Day 11:**

- Kubernetes offers a flexible networking model with **services** and **ingress** to handle internal and external traffic.
- **Services** provide stable IP addresses for pods and allow load balancing, while **Ingress** manages HTTP routing.
- **NodePort** and **LoadBalancer** expose services externally, with Ingress adding advanced routing capabilities.

---
