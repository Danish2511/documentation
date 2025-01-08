# **Kubernetes Resources and Autoscaling**

## **Concepts:**

In Kubernetes, **autoscaling** allows your applications to automatically adjust the number of replicas based on resource utilization or custom metrics. This ensures efficient resource usage and helps handle varying levels of traffic or load.

1. **Resource Requests and Limits**:

   - **Resource Requests** specify the minimum resources that a container requires to run. Kubernetes uses this information to place the container on a node with sufficient resources.
   - **Resource Limits** specify the maximum resources that a container can consume. If a container exceeds the limit, Kubernetes may throttle or kill the container.
   - Setting both requests and limits is important to prevent resource contention and to ensure that containers have enough resources while not overloading the cluster.

2. **Horizontal Pod Autoscaler (HPA)**:

   - The **Horizontal Pod Autoscaler** (HPA) automatically scales the number of pod replicas based on observed CPU utilization or other selected metrics.
   - HPA helps in adjusting to traffic spikes or downtime, ensuring that applications perform efficiently.

3. **Vertical Pod Autoscaler (VPA)**:

   - The **Vertical Pod Autoscaler** automatically adjusts the CPU and memory requests and limits for containers in a pod, ensuring that they are properly configured based on observed usage.

4. **Cluster Autoscaler**:
   - The **Cluster Autoscaler** automatically adjusts the number of nodes in your Kubernetes cluster based on resource usage. If pods cannot be scheduled due to resource shortages, the Cluster Autoscaler will add more nodes to the cluster.

---

## **Key Topics:**

- **Setting Resource Requests and Limits**: Defining resources for containers.
- **Horizontal Pod Autoscaling (HPA)**: Automatically scaling pods based on metrics.
- **Vertical Pod Autoscaling (VPA)**: Adjusting resource allocation dynamically.
- **Cluster Autoscaling**: Managing nodes in the cluster based on load.

---

## **Hands-On Exercise:**

### 1. **Setting Resource Requests and Limits**:

Let’s start by creating a deployment with resource requests and limits for an Nginx container.

- **Step 1: Create a Deployment with Resources**:
  Create a file `nginx-deployment-resources.yaml` with the following content:

  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: nginx-deployment-resources
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
            resources:
              requests:
                memory: "64Mi"
                cpu: "250m"
              limits:
                memory: "128Mi"
                cpu: "500m"
            ports:
              - containerPort: 80
  ```

  - This deployment sets resource **requests** (minimum resources) and **limits** (maximum resources) for the Nginx container.

- **Step 2: Apply the Deployment**:
  Apply the deployment to the cluster:

  ```bash
  kubectl apply -f nginx-deployment-resources.yaml
  ```

- **Step 3: Verify Resource Settings**:
  To verify the resource settings of the pod, run:

  ```bash
  kubectl describe deployment nginx-deployment-resources
  ```

  Under the **Containers** section, you should see the requests and limits for `cpu` and `memory`.

### 2. **Setting Up Horizontal Pod Autoscaler (HPA)**:

Now let’s set up a **Horizontal Pod Autoscaler** for the Nginx deployment based on CPU utilization.

- **Step 1: Create HPA for the Nginx Deployment**:
  Run the following command to create an HPA:

  ```bash
  kubectl autoscale deployment nginx-deployment-resources --cpu-percent=50 --min=1 --max=5
  ```

  - This command tells Kubernetes to scale the Nginx deployment between 1 and 5 replicas based on CPU utilization, aiming for 50% average CPU utilization.

- **Step 2: Verify the HPA**:
  You can check the status of the HPA with:

  ```bash
  kubectl get hpa
  ```

  This should show the number of replicas and current CPU utilization.

- **Step 3: Simulate Load to Trigger Autoscaling**:
  To trigger the autoscaler, you can generate CPU load on the pods. For example:

  ```bash
  kubectl run -i --tty load-generator --image=busybox --restart=Never -- /bin/sh
  ```

  Inside the pod, run a CPU-intensive command:

  ```bash
  while true; do echo "load" | md5sum; done
  ```

  This will consume CPU, and Kubernetes should scale up the pods in response.

- **Step 4: Verify Scaling**:
  Check the deployment to see if the number of pods has increased:

  ```bash
  kubectl get deployment nginx-deployment-resources
  ```

  You should see the pod count has increased based on CPU load.

- **Step 5: Clean Up**:
  Delete the load generator pod and HPA:
  ```bash
  kubectl delete pod load-generator
  kubectl delete hpa nginx-deployment-resources
  ```

### 3. **Setting Up Vertical Pod Autoscaler (VPA)**:

Next, we will set up a **Vertical Pod Autoscaler** for the Nginx deployment to automatically adjust resource requests and limits.

- **Step 1: Install VPA (if necessary)**:
  If the Vertical Pod Autoscaler is not installed in your cluster, you can install it by following the official [VPA installation guide](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler).

- **Step 2: Create a VPA Resource**:
  Create a file `nginx-vpa.yaml` with the following content:

  ```yaml
  apiVersion: autoscaling.k8s.io/v1
  kind: VerticalPodAutoscaler
  metadata:
    name: nginx-vpa
  spec:
    targetRef:
      apiVersion: apps/v1
      kind: Deployment
      name: nginx-deployment-resources
    updatePolicy:
      updateMode: "Auto"
  ```

  - This VPA resource will automatically adjust the resource requests and limits for the Nginx deployment.

- **Step 3: Apply the VPA**:
  Apply the VPA configuration:

  ```bash
  kubectl apply -f nginx-vpa.yaml
  ```

- **Step 4: Monitor VPA Updates**:
  The VPA will automatically adjust resource requests and limits based on usage. To monitor changes:

  ```bash
  kubectl get vpa
  ```

- **Step 5: Clean Up**:
  After testing, delete the VPA:
  ```bash
  kubectl delete vpa nginx-vpa
  ```

---

## **Real-World Example:**

In real-world applications, autoscaling is often used in microservices architectures to handle varying workloads:

- **Horizontal Pod Autoscaler** is commonly used to scale services based on CPU or memory usage, ensuring that applications can handle increased traffic automatically (e.g., for a user-facing application).
- **Vertical Pod Autoscaler** is useful when resource requirements are not fixed, such as in workloads with dynamic resource needs (e.g., memory-intensive batch jobs).
- **Cluster Autoscaler** ensures that the Kubernetes cluster can grow or shrink dynamically to match the demands of the applications running on it.

For instance, in a **shopping website**, you might use HPA to scale the product catalog service up when traffic increases during sales, and VPA to adjust resource allocation for the checkout service based on the complexity of the transactions.

---

## **Key Takeaways for Day 12:**

- **Resource requests and limits** ensure efficient resource utilization and prevent resource contention.
- **Horizontal Pod Autoscaler (HPA)** scales the number of replicas based on metrics like CPU utilization.
- **Vertical Pod Autoscaler (VPA)** adjusts resource requests and limits for pods dynamically based on actual usage.
- **Cluster Autoscaler** adjusts the number of nodes in the cluster based on load.

---
