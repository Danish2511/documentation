# **Kubernetes Networking**

## **Concepts:**

Kubernetes networking involves the configuration and management of communication between pods, services, and external systems within a Kubernetes cluster. Understanding Kubernetes networking is crucial for deploying and managing applications in a scalable and secure way.

1. **Kubernetes Networking Model**:

   - **Pod-to-Pod Communication**: By default, Kubernetes provides a flat network where every pod can communicate with every other pod without any network address translation (NAT). Each pod gets its own unique IP address.
   - **Service Discovery and Load Balancing**: Kubernetes uses **Services** to expose a set of pods as a network service. Services provide DNS resolution and load balancing for pods.
   - **External Access**: To allow external traffic to access a service, Kubernetes uses **Ingress** controllers or **LoadBalancers** to expose services.

2. **Kubernetes Services**:

   - **ClusterIP**: The default type of service. It is accessible only within the Kubernetes cluster.
   - **NodePort**: Exposes the service on a specific port on each node in the cluster, allowing external access to the service via `<node-ip>:<node-port>`.
   - **LoadBalancer**: Automatically provisions a load balancer (on cloud platforms) to expose the service externally.
   - **ExternalName**: Maps a service to an external DNS name, allowing services within the cluster to refer to external resources.

3. **DNS in Kubernetes**:

   - Kubernetes includes a built-in DNS service that provides service discovery by resolving service names to their IP addresses.
   - Pods can access services by name, e.g., `my-service.my-namespace.svc.cluster.local`.

4. **Ingress Controllers**:
   - An **Ingress** controller is responsible for managing external access to services in a cluster, typically HTTP/HTTPS traffic.
   - **Ingress** allows you to define rules for routing traffic based on hostnames, paths, or other HTTP headers.
   - Ingress controllers use different backend services, such as Nginx, Traefik, or HAProxy, to manage the actual traffic routing.

---

## **Key Topics:**

- **Kubernetes Networking Model**: Pod-to-pod communication and service exposure.
- **Kubernetes Services**: ClusterIP, NodePort, LoadBalancer, and ExternalName.
- **DNS**: Service discovery and DNS resolution.
- **Ingress Controllers**: External access management and traffic routing.

---

## **Hands-On Exercise:**

### 1. **Create a Kubernetes Service (ClusterIP)**:

Let’s start by creating a simple **ClusterIP** service to expose a pod.

- **Step 1: Create a Simple Nginx Deployment**:
  Create a simple Nginx deployment to act as a service:

  ```bash
  kubectl create deployment nginx --image=nginx
  ```

- **Step 2: Expose the Nginx Deployment with ClusterIP**:
  Expose the Nginx deployment using a **ClusterIP** service:

  ```bash
  kubectl expose deployment nginx --port=80 --target-port=80 --name=nginx-service --type=ClusterIP
  ```

- **Step 3: Verify the Service**:
  Verify the service was created:

  ```bash
  kubectl get svc
  ```

  You should see the `nginx-service` listed with a ClusterIP.

- **Step 4: Access the Service from Another Pod**:
  Launch a temporary pod and try to access the Nginx service by using its service name (`nginx-service`):

  ```bash
  kubectl run -i --tty --rm busybox --image=busybox --restart=Never -- sh
  ```

  Inside the pod, use `wget` or `curl` to access the service:

  ```bash
  wget -qO- http://nginx-service
  ```

  You should get the HTML response from the Nginx service.

### 2. **Expose the Service with NodePort**:

Next, let’s expose the service externally using the **NodePort** type.

- **Step 1: Expose the Nginx Service with NodePort**:

  ```bash
  kubectl expose deployment nginx --port=80 --target-port=80 --name=nginx-service-nodeport --type=NodePort
  ```

- **Step 2: Verify the Service**:
  Verify that the NodePort has been assigned:

  ```bash
  kubectl get svc
  ```

  Look for the `nginx-service-nodeport` entry, and note the assigned port in the `PORT(S)` column (e.g., `30001:80`).

- **Step 3: Access the Service Externally**:
  If you're using Minikube, use `minikube service` to open the service in your browser:

  ```bash
  minikube service nginx-service-nodeport
  ```

  If you're using Docker Desktop or another Kubernetes setup, access the service via `<node-ip>:<node-port>`.

### 3. **Create an Ingress Resource**:

Let’s expose the service externally using an **Ingress**.

- **Step 1: Install an Ingress Controller (Nginx Ingress)**:
  If you don’t have an Ingress controller installed, you can install it using the following command:

  ```bash
  kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
  ```

  This will install the Nginx Ingress controller in your cluster.

- **Step 2: Create an Ingress Resource**:
  Create an Ingress resource that will route traffic to the Nginx service. Create a file `nginx-ingress.yaml`:

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

  - This Ingress resource routes requests to `nginx.local` to the `nginx-service` on port 80.

- **Step 3: Apply the Ingress Resource**:
  Apply the Ingress configuration:

  ```bash
  kubectl apply -f nginx-ingress.yaml
  ```

- **Step 4: Modify `/etc/hosts` to Access Ingress Locally**:
  Add the following entry to your `/etc/hosts` file to route traffic to your cluster’s IP (replace `<cluster-ip>` with the IP of your cluster):

  ```plaintext
  <cluster-ip> nginx.local
  ```

- **Step 5: Verify Access to Nginx via Ingress**:
  Open a browser and go to `http://nginx.local`. You should see the Nginx welcome page.

### 4. **Clean Up**:

Once you are done with the exercises, clean up the resources:

```bash
kubectl delete deployment nginx
kubectl delete svc nginx-service nginx-service-nodeport
kubectl delete ingress nginx-ingress
```

---

## **Real-World Example:**

In real-world scenarios, Kubernetes networking is used to:

- **Expose services to external users**: A typical use case is exposing your frontend or API services to the internet via **Ingress**.
- **Service Discovery**: Services like **databases** or **caches** are usually accessed by other internal services using their service names (e.g., `postgres-service`).
- **Load Balancing**: With **LoadBalancer** services, Kubernetes can automatically configure an external load balancer for distributing traffic, such as when deploying applications on cloud providers like AWS or GCP.

For example, in a **microservices architecture**, a service like a **frontend** can communicate with the backend services like **payment processing** or **user authentication** using Kubernetes **Services** and **DNS**. **Ingress controllers** can be used to route traffic based on the URL path to different services, enabling a secure and scalable API gateway pattern.

---

## **Key Takeaways for Day 15:**

- **Kubernetes Services** allow pods to communicate internally and externally using types like `ClusterIP`, `NodePort`, and `LoadBalancer`.
- **Ingress** controllers manage external access to services, allowing for advanced routing and load balancing.
- **DNS** in Kubernetes simplifies service discovery by enabling pods to access services by name.

---
