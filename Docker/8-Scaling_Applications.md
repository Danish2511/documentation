# **Scaling Applications with Deployments and Rolling Updates**

## **Concepts:**

- **Scaling in Kubernetes** refers to adjusting the number of pods in a deployment to handle changes in traffic, load, or other demands.

  - **Horizontal Scaling**: Increasing or decreasing the number of replicas (pods) running for an application.
  - **Vertical Scaling**: Increasing or decreasing the resources (CPU, memory) allocated to a pod.

- **Deployments** in Kubernetes allow you to manage scaling and updates for your applications, and they provide rolling updates to ensure that updates are applied smoothly without downtime.

## **Key Topics:**

1. **Scaling Deployments**:

   - Kubernetes allows you to scale your applications by changing the number of replicas in a deployment. This can be done manually or automatically using **Horizontal Pod Autoscaling**.

   - To manually scale a deployment, you simply modify the number of replicas.

2. **Rolling Updates**:
   - **Rolling Updates** are a strategy in Kubernetes to deploy changes to an application without downtime. With rolling updates, Kubernetes gradually replaces the old pods with new ones, ensuring that the application remains available.

---

## **Hands-On Exercise:**

### 1. **Scaling a Deployment**:

Let’s scale the **nginx-deployment** we created previously.

- **Step 1: Scale the Deployment Manually**:
  Run the following command to scale the `nginx-deployment` to 5 replicas:

  ```bash
  kubectl scale deployment nginx-deployment --replicas=5
  ```

- **Step 2: Verify the Scaling**:
  Check the status of the deployment and verify the number of pods:

  ```bash
  kubectl get deployments
  kubectl get pods
  ```

  You should now see 5 pods running for the `nginx-deployment`.

- **Step 3: Reduce the Number of Replicas**:
  Scale the deployment back down to 3 replicas:

  ```bash
  kubectl scale deployment nginx-deployment --replicas=3
  ```

- **Step 4: Clean Up**:
  After testing, you can delete the deployment with:
  ```bash
  kubectl delete deployment nginx-deployment
  ```

### 2. **Rolling Updates in Kubernetes**:

Now, let’s look at how Kubernetes handles updates to applications using **rolling updates**.

- **Step 1: Create a New Deployment Version**:
  First, let’s update the **nginx-deployment** to use a different version of the Nginx image (e.g., `nginx:1.21` instead of `nginx`).

  Create a file named `nginx-deployment-v2.yaml` with the following content:

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
            image: nginx:1.21 # Update to a different version
            ports:
              - containerPort: 80
  ```

- **Step 2: Apply the Update**:
  Apply the new deployment configuration:

  ```bash
  kubectl apply -f nginx-deployment-v2.yaml
  ```

- **Step 3: Verify the Rolling Update**:
  Kubernetes will perform a rolling update, gradually replacing the old Nginx pods with new ones. Check the pods:

  ```bash
  kubectl get pods
  ```

  You should see old pods being replaced with new ones over time. Kubernetes ensures that at least one pod remains available throughout the update.

- **Step 4: Check the Deployment Status**:
  You can monitor the progress of the update with:

  ```bash
  kubectl rollout status deployment nginx-deployment
  ```

  Once the update is complete, all pods will be running the new version of the image.

- **Step 5: Rollback the Update (if needed)**:
  If you encounter any issues with the new deployment, Kubernetes allows you to roll back to a previous version:

  ```bash
  kubectl rollout undo deployment nginx-deployment
  ```

- **Step 6: Clean Up**:
  After testing, delete the deployment:
  ```bash
  kubectl delete deployment nginx-deployment
  ```

---

## **Real-World Example:**

In a real-world environment, rolling updates and scaling are essential to ensure your applications remain responsive:

- **Scaling** allows your applications to handle more traffic by adjusting the number of pods running for your services. For example, an e-commerce site may scale its checkout service during peak shopping hours.
- **Rolling Updates** ensure that changes, such as upgrading an app or fixing bugs, happen without causing downtime or disruption to users. During a rolling update, users will continue to interact with the app even as new versions are deployed.

---

## **Key Takeaways for Day 8:**

- **Scaling Deployments** allows you to adjust the number of replicas running to meet changing demand.
- **Rolling Updates** ensure that application updates are applied gradually to avoid downtime.
- Kubernetes makes it easy to manage the scaling and updating of your applications with minimal disruption.

---
