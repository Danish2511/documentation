# **Introduction to Kubernetes**

## **Concepts:**

- **What is Kubernetes?**
  - **Kubernetes (K8s)** is an open-source container orchestration platform designed to automate the deployment, scaling, and management of containerized applications. It works well with Docker and other container runtimes to manage clusters of machines running containers.
  - Kubernetes abstracts the underlying infrastructure, making it easier to deploy and manage applications in a cloud-native environment.

## **Key Kubernetes Concepts:**

1. **Cluster**:

   - A Kubernetes **cluster** is a set of machines (called nodes) that run containerized applications. The cluster consists of two main components:
     - **Master Node**: Controls and manages the Kubernetes cluster (the control plane).
     - **Worker Nodes**: Run the containerized applications (pods) and communicate with the master node.

2. **Pod**:

   - A **pod** is the smallest unit in Kubernetes and represents a single instance of a running process in the cluster. A pod can contain one or more containers that share the same network and storage resources.

3. **Deployment**:

   - A **deployment** manages the desired state of a set of pods. It ensures that the correct number of pod replicas are running and can handle updates seamlessly (e.g., rolling updates).

4. **Service**:

   - A **service** is a Kubernetes abstraction that exposes a set of pods as a network service. Services enable communication between pods, external clients, and the internet.

5. **ConfigMap and Secrets**:

   - **ConfigMaps** store non-sensitive configuration data (like environment variables) that can be used by pods.
   - **Secrets** store sensitive data (such as passwords or API keys), and Kubernetes ensures they are managed securely.

6. **Ingress**:
   - **Ingress** is a Kubernetes resource that allows external HTTP and HTTPS traffic to reach services inside the cluster.

---

## **Setting Up Kubernetes Locally:**

We’ll use **Minikube** to set up a local Kubernetes cluster. Minikube is a tool that runs a single-node Kubernetes cluster in a virtual machine on your local machine, making it easier to develop and test Kubernetes applications.

### **Step-by-Step Setup:**

1. **Install Minikube**:

   - Follow the instructions to install Minikube on your machine from the official site: https://minikube.sigs.k8s.io/docs/
   - Ensure that you also have **kubectl** (the Kubernetes command-line tool) installed. Minikube will also install kubectl if it’s not already on your system.

2. **Start Minikube**:
   Open a terminal and run:

   ```bash
   minikube start
   ```

   This command will start a Kubernetes cluster in a virtual machine on your local machine. The process may take a few minutes.

3. **Verify Kubernetes Cluster**:
   Once Minikube is up and running, verify the cluster status by running:

   ```bash
   kubectl cluster-info
   ```

   This will display information about the cluster, including the master node and service endpoints.

4. **Check Nodes**:
   To see the nodes in your Kubernetes cluster, run:
   ```bash
   kubectl get nodes
   ```
   You should see a single node (the Minikube VM) with a `Ready` status.

---

## **Hands-On Exercise:**

### 1. **Deploying Your First Pod**:

Let’s start by deploying a simple pod that runs an Nginx web server.

- **Step 1: Create a Pod Definition (YAML)**:
  Create a file named `nginx-pod.yaml` with the following content:

  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: nginx-pod
  spec:
    containers:
      - name: nginx
        image: nginx
        ports:
          - containerPort: 80
  ```

  - This YAML file defines a pod that contains a single container (nginx) and exposes port 80.

- **Step 2: Apply the Pod Definition**:
  Run the following command to create the pod from the YAML file:

  ```bash
  kubectl apply -f nginx-pod.yaml
  ```

- **Step 3: Verify the Pod is Running**:
  Check the pod’s status:

  ```bash
  kubectl get pods
  ```

  You should see the `nginx-pod` in a `Running` state.

- **Step 4: Access the Nginx Web Server**:
  Since the pod is running in a cluster, you need to expose it through a service. Let’s expose it using `kubectl port-forward`:

  ```bash
  kubectl port-forward nginx-pod 8080:80
  ```

  Now, open your browser and go to `http://localhost:8080`. You should see the Nginx default welcome page.

- **Step 5: Clean Up**:
  After testing, you can delete the pod with:
  ```bash
  kubectl delete pod nginx-pod
  ```

### 2. **Creating a Deployment**:

Next, we’ll create a deployment to manage the Nginx pods.

- **Step 1: Create a Deployment Definition (YAML)**:
  Create a file named `nginx-deployment.yaml` with the following content:

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

  - This YAML file defines a deployment that manages 3 replicas of Nginx pods.

- **Step 2: Apply the Deployment Definition**:
  Run the following command to create the deployment:

  ```bash
  kubectl apply -f nginx-deployment.yaml
  ```

- **Step 3: Verify the Deployment**:
  Check the deployment status:

  ```bash
  kubectl get deployments
  kubectl get pods
  ```

  You should see 3 pods running for the `nginx-deployment`.

- **Step 4: Clean Up**:
  After testing, you can delete the deployment with:
  ```bash
  kubectl delete deployment nginx-deployment
  ```

---

## **Real-World Example:**

In real-world applications, Kubernetes is used to manage large-scale applications that consist of many services running in multiple containers. A Kubernetes deployment manages the lifecycle of these services:

- It ensures that the specified number of replicas are always running.
- It provides automated rolling updates to applications (e.g., upgrading an app without downtime).

---

## **Key Takeaways for Day 6:**

- **Kubernetes** is a powerful tool for orchestrating containerized applications at scale.
- **Pods** are the smallest deployable units in Kubernetes, containing one or more containers.
- **Deployments** manage a set of replica pods and allow for automated scaling and updates.
- **Minikube** helps you set up a local Kubernetes cluster for learning and testing.

---
