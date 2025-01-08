# **Services and Networking in Kubernetes**

## **Concepts:**

- **What is a Service in Kubernetes?**
  - In Kubernetes, a **Service** is an abstraction that defines a logical set of pods and a policy by which to access them. A Service enables communication between different parts of an application by providing a stable endpoint (IP address and DNS name) for accessing pods, even when they are dynamically created or destroyed.
  - Kubernetes Services can expose pods internally within the cluster or externally to the outside world.

## **Types of Services in Kubernetes:**

1. **ClusterIP (Default)**:

   - The **ClusterIP** service type exposes the service on a cluster-internal IP. It is only accessible within the Kubernetes cluster and cannot be accessed from outside.
   - This is the default service type.

2. **NodePort**:

   - The **NodePort** service type exposes the service on a static port on each node’s IP address. This allows you to access the service externally by hitting `<NodeIP>:<NodePort>`.
   - It’s useful when you need to expose a service outside the cluster but don’t need a load balancer.

3. **LoadBalancer**:

   - The **LoadBalancer** service type automatically provisions an external load balancer (if your cloud provider supports it) and assigns a public IP to the service. It’s used for accessing the service externally in a highly available manner.

4. **ExternalName**:
   - The **ExternalName** service type allows you to map a service to an external DNS name (like `myapp.example.com`) without creating any pods or resources inside Kubernetes.

---

## **Hands-On Exercise:**

### 1. **Creating a ClusterIP Service:**

Let’s start by exposing the Nginx deployment we created earlier using a **ClusterIP** service.

- **Step 1: Create a Service Definition (YAML)**:
  Create a file named `nginx-service-clusterip.yaml` with the following content:

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

  - This YAML defines a service that selects pods with the label `app: nginx` and exposes port 80.

- **Step 2: Apply the Service Definition**:
  Run the following command to create the service:

  ```bash
  kubectl apply -f nginx-service-clusterip.yaml
  ```

- **Step 3: Verify the Service**:
  Check that the service has been created:

  ```bash
  kubectl get services
  ```

  You should see the `nginx-service` listed with a `ClusterIP`.

- **Step 4: Access the Service**:
  Since this is a **ClusterIP** service, you can only access it from within the cluster. Let’s start a temporary pod to test connectivity:

  ```bash
  kubectl run -i --tty --rm debug --image=busybox --serviceaccount default --restart=Never -- /bin/sh
  ```

  Once inside the pod, try to access the Nginx service:

  ```bash
  wget -qO- nginx-service
  ```

  You should see the HTML content returned from the Nginx server.

- **Step 5: Clean Up**:
  After testing, delete the service:
  ```bash
  kubectl delete service nginx-service
  ```

### 2. **Creating a NodePort Service:**

Next, let’s create a **NodePort** service that will allow you to access the Nginx deployment from outside the Kubernetes cluster.

- **Step 1: Create a Service Definition (YAML)**:
  Create a file named `nginx-service-nodeport.yaml` with the following content:

  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: nginx-service-nodeport
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

  - This YAML defines a NodePort service that exposes port 80 on port 30080 on each node in the cluster.

- **Step 2: Apply the Service Definition**:
  Run the following command to create the service:

  ```bash
  kubectl apply -f nginx-service-nodeport.yaml
  ```

- **Step 3: Verify the Service**:
  Check that the NodePort service has been created:

  ```bash
  kubectl get services
  ```

  You should see the `nginx-service-nodeport` with a `NodePort` of 30080.

- **Step 4: Access the Service**:
  To access the service, you can use the Minikube IP. Run the following command to get the Minikube IP:

  ```bash
  minikube ip
  ```

  This will give you the IP address of the Minikube VM. Open a browser and go to `http://<minikube-ip>:30080`. You should see the Nginx default welcome page.

- **Step 5: Clean Up**:
  After testing, delete the service:
  ```bash
  kubectl delete service nginx-service-nodeport
  ```

---

## **Real-World Example:**

In real-world Kubernetes environments, services are used to expose applications to other services or to the external world:

- **ClusterIP** is ideal for internal communication within the cluster, like between microservices.
- **NodePort** can be used in situations where you need to expose a service externally but without relying on cloud-specific load balancers.
- **LoadBalancer** is commonly used in cloud environments to provide a highly available external service.

---

## **Key Takeaways for Day 7:**

- Kubernetes **Services** allow you to expose your applications running in pods.
- **ClusterIP** services are internal to the cluster, while **NodePort** services can be accessed externally.
- You can manage communication between your pods using services, ensuring reliability and scalability in your application.

---
