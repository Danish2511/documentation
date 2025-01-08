# **Kubernetes Troubleshooting and Debugging**

## **Concepts:**

Troubleshooting and debugging in Kubernetes are essential skills for diagnosing issues in applications, clusters, and workloads. Kubernetes provides a set of tools and strategies to help you identify and resolve problems efficiently.

1. **Pod and Container Troubleshooting**:

   - **kubectl logs**: This command is used to view the logs of a pod. It helps identify issues such as application crashes, errors, or misconfigurations.
     - Example: `kubectl logs <pod-name>` will show the logs of the pod.
     - Example for a specific container: `kubectl logs <pod-name> -c <container-name>`
   - **kubectl describe**: This command shows detailed information about a resource, such as a pod, and includes events that might explain issues like crashes, scheduling failures, or resource limits.
     - Example: `kubectl describe pod <pod-name>`
   - **kubectl exec**: This allows you to run commands inside a container. You can use it to inspect the container's file system or interact with the application directly.
     - Example: `kubectl exec -it <pod-name> -- /bin/bash` to start a shell in the container.

2. **Pod Lifecycle and Restart Policies**:

   - Kubernetes handles pod restarts based on defined **restart policies**. Pods with a `restartPolicy: Always` will restart automatically if a container crashes, while `restartPolicy: OnFailure` will restart only if the container exits with a non-zero exit code.
   - You can check if a pod was restarted using `kubectl get pods` and inspecting the `RESTARTS` column.

3. **Kubernetes Events**:

   - Events are automatically created in Kubernetes whenever certain actions happen (e.g., a pod is scheduled, an error occurs, or a container fails). Events can help you understand why a pod isn't starting, why a deployment isn't rolling out, or why a service isn't accessible.
   - You can list events using `kubectl get events`. Sorting events by creation time can help track the sequence of issues.

4. **Resource Constraints and Limits**:

   - Pods have resource **requests** and **limits** for CPU and memory. When a pod exceeds its memory or CPU allocation, Kubernetes can terminate the pod or throttle it. This can lead to pod restarts or poor application performance.
   - You can check the current usage of resources with `kubectl top pods` and adjust requests and limits as needed.

5. **Network Issues**:
   - Network issues can prevent pods from communicating with each other. Use **Network Policies** to control communication and **kubectl port-forward** to access services directly.
   - Checking the logs of **kube-proxy** and networking plugins can help diagnose network issues within the cluster.

---

## **Key Topics:**

- **kubectl logs**: Viewing container logs for debugging.
- **kubectl describe**: Inspecting pod and resource details.
- **kubectl exec**: Running commands inside a pod for inspection.
- **Pod Lifecycle**: Understanding restarts and failures.
- **Kubernetes Events**: Tracking cluster state and resource issues.
- **Resource Constraints**: Identifying and resolving resource-related problems.
- **Network Troubleshooting**: Debugging network communication between pods.

---

## **Hands-On Exercise:**

### 1. **View Pod Logs**:

Let’s start by viewing the logs of a pod to troubleshoot an issue.

- **Step 1: Deploy a Pod with a Simple Application**:
  Create a simple pod that runs a web server. For example, use the `nginx` image:

  ```bash
  kubectl run nginx --image=nginx
  ```

- **Step 2: Check Logs for the Pod**:
  View the logs of the `nginx` pod to check if there are any issues:

  ```bash
  kubectl logs nginx
  ```

  If you have multiple containers in the pod, specify the container name:

  ```bash
  kubectl logs nginx -c <container-name>
  ```

  In case the pod has crashed, you can view the previous logs:

  ```bash
  kubectl logs nginx --previous
  ```

### 2. **Describe a Pod**:

Use the `kubectl describe` command to get detailed information about a pod, including events and resource information.

- **Step 1: Describe the Pod**:
  Describe the `nginx` pod to get more insights:

  ```bash
  kubectl describe pod nginx
  ```

  This will show you events, pod status, and any error messages related to the pod.

### 3. **Check Resource Usage**:

Check the current resource usage of the pod and cluster.

- **Step 1: Install the Metrics Server** (if not already installed):
  Install the **Metrics Server** if you haven’t already done so:

  ```bash
  kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.5.0/components.yaml
  ```

- **Step 2: Check Resource Usage**:
  Use `kubectl top` to see resource usage:

  ```bash
  kubectl top pods
  kubectl top nodes
  ```

  If a pod is consuming too many resources, you can modify its resource requests and limits to avoid evictions or throttling.

### 4. **Use kubectl exec for Interactive Debugging**:

Sometimes, you need to get inside the container to inspect its file system, check application logs, or run commands directly.

- **Step 1: Exec into the Pod**:
  Exec into the `nginx` pod:

  ```bash
  kubectl exec -it nginx -- /bin/bash
  ```

  Once inside the container, you can run commands like `ps`, `top`, or check specific logs or files.

- **Step 2: Check the Application Logs Inside the Pod**:
  Navigate to the logs or configuration files to troubleshoot the application behavior.

### 5. **Monitor Kubernetes Events**:

Monitor the events in the cluster to spot any anomalies or issues.

- **Step 1: Get Kubernetes Events**:
  List the events in the cluster:

  ```bash
  kubectl get events --sort-by='.metadata.creationTimestamp'
  ```

  Events will provide valuable information like pod scheduling failures, resource issues, or failures in deployment.

### 6. **Simulate a Pod Crash and Check Logs**:

To simulate an issue, we will forcefully terminate a pod and check its logs for any useful information.

- **Step 1: Delete the Pod**:
  Delete the `nginx` pod to simulate a crash:

  ```bash
  kubectl delete pod nginx
  ```

- **Step 2: Check for Events and Logs**:
  Use `kubectl get events` to see if there are any issues related to the pod crash. Check the logs for any errors:
  ```bash
  kubectl logs nginx --previous
  ```

### 7. **Check Network Connectivity Between Pods**:

To check if there are network-related issues between pods, use `kubectl exec` to run a `ping` or `curl` command inside the pod.

- **Step 1: Create Two Pods**:
  Create two pods and simulate network communication between them:

  ```bash
  kubectl run pod1 --image=busybox --command -- sleep 3600
  kubectl run pod2 --image=busybox --command -- sleep 3600
  ```

- **Step 2: Exec into One Pod**:
  Exec into `pod1` and ping `pod2` to check if network communication is working:

  ```bash
  kubectl exec -it pod1 -- ping pod2
  ```

  If the ping fails, it could be due to network policies, firewall rules, or issues with the networking layer.

---

## **Real-World Example:**

In real-world scenarios, troubleshooting and debugging are integral to keeping your applications running smoothly. Here’s an example:

- **Pod Crash**: If an application in a pod crashes, use `kubectl logs` and `kubectl describe` to check the logs and events for the root cause. You might find out that the container ran out of memory, causing the pod to restart.
- **Networking Issue**: If a service isn’t reachable, you can use `kubectl exec` to test connectivity between pods. If necessary, inspect network policies or check if the kube-proxy is functioning correctly.
- **Resource Exhaustion**: In a situation where your pod is throttled or evicted due to resource exhaustion, you can adjust the resource requests and limits based on `kubectl top` output to ensure the pod gets sufficient resources.

---

## **Key Takeaways for Day 18:**

- **kubectl logs** helps you inspect logs for debugging issues in your containers.
- **kubectl describe** provides detailed information and events about resources, helping identify issues.
- **kubectl exec** allows you to run commands inside containers for more in-depth debugging.
- **Resource usage** and **events** help track issues related to resource exhaustion or scheduling.
- **Network troubleshooting** is vital for diagnosing issues with pod-to-pod communication.

---
