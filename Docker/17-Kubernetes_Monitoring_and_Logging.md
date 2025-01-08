# **Kubernetes Monitoring and Logging**

## **Concepts:**

Monitoring and logging are critical for maintaining the health and performance of applications and clusters in Kubernetes. These practices help in detecting issues early, analyzing system behavior, and improving the reliability and performance of your workloads.

1. **Monitoring Kubernetes Clusters**:
   Monitoring Kubernetes involves tracking the health of the cluster, its nodes, and the workloads running inside it. You typically use tools like **Prometheus** and **Grafana** for collecting and visualizing metrics.

   - **Prometheus**: A powerful open-source monitoring system designed for cloud-native applications. Prometheus collects metrics from various sources and stores them in a time-series database.
   - **Grafana**: A visualization tool that can display the metrics collected by Prometheus in dashboards.

2. **Kubernetes Metrics Server**:

   - The **Kubernetes Metrics Server** is a lightweight, resource monitoring tool used to collect CPU and memory usage data for nodes and pods. It feeds data into **Horizontal Pod Autoscaling** and **kubectl top** for cluster monitoring.

3. **Logging in Kubernetes**:
   Logging provides insight into what is happening inside the cluster. Logs can help you debug issues and monitor applications.

   - **Fluentd**: An open-source data collector that is commonly used to aggregate logs and send them to destinations like **Elasticsearch** or **Cloud-based logging services**.
   - **Elasticsearch, Fluentd, Kibana (EFK Stack)**: A common stack used for log aggregation and analysis in Kubernetes.
     - **Elasticsearch**: A distributed search and analytics engine used to store logs.
     - **Fluentd**: A log shipper that collects logs from various sources and forwards them to a destination like Elasticsearch.
     - **Kibana**: A visualization tool that works with Elasticsearch to create dashboards for viewing and analyzing logs.

4. **Kubernetes Events**:
   - Kubernetes generates events that describe what's happening in the cluster (e.g., pods being scheduled, nodes being evicted). These events can be accessed via `kubectl get events` and are helpful in troubleshooting.

---

## **Key Topics:**

- **Monitoring**: Using tools like Prometheus and Grafana to collect and visualize metrics.
- **Logging**: Implementing logging solutions like the EFK stack for log aggregation.
- **Kubernetes Metrics Server**: Collecting resource usage metrics for scaling decisions.
- **Kubernetes Events**: Tracking cluster activity and troubleshooting issues.

---

## **Hands-On Exercise:**

### 1. **Install Prometheus and Grafana in Kubernetes**:

Let’s start by installing **Prometheus** and **Grafana** for monitoring your Kubernetes cluster.

- **Step 1: Install Helm**:
  Helm is a package manager for Kubernetes that simplifies the installation of tools like Prometheus and Grafana.

  Install Helm using the following command (if you don’t have it already):

  ```bash
  curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
  ```

- **Step 2: Add the Prometheus and Grafana Helm Repositories**:

  ```bash
  helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
  helm repo add grafana https://grafana.github.io/helm-charts
  helm repo update
  ```

- **Step 3: Install Prometheus and Grafana Using Helm**:
  Install **Prometheus** and **Grafana** using Helm charts:

  ```bash
  helm install prometheus prometheus-community/kube-prometheus-stack
  helm install grafana grafana/grafana
  ```

- **Step 4: Verify the Installations**:
  Verify that Prometheus and Grafana pods are running:

  ```bash
  kubectl get pods -l "release=prometheus"
  kubectl get pods -l "app=grafana"
  ```

- **Step 5: Access the Grafana Dashboard**:
  Once Grafana is running, you can access the dashboard via port forwarding:

  ```bash
  kubectl port-forward svc/grafana 3000:80
  ```

  Then open a browser and visit `http://localhost:3000` to access Grafana. The default username is `admin` and the password is `prom-operator`.

- **Step 6: Configure Prometheus Data Source in Grafana**:
  Once you’ve logged into Grafana, configure Prometheus as the data source:

  - Go to `Configuration` > `Data Sources` > `Add data source`.
  - Choose **Prometheus** and set the URL as `http://prometheus-server:80`.

- **Step 7: Import Prometheus Dashboards**:
  You can now import pre-configured Prometheus dashboards to monitor your cluster:
  - Go to `Create` > `Import`.
  - Enter the dashboard ID (for example, `3119` for the Kubernetes Cluster Monitoring dashboard) and click `Load`.

### 2. **Use Kubernetes Metrics Server**:

- **Step 1: Install the Metrics Server**:
  Install the **Metrics Server** in your Kubernetes cluster using the following command:

  ```bash
  kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.5.0/components.yaml
  ```

- **Step 2: Verify Metrics Server**:
  Once the Metrics Server is installed, verify its deployment:

  ```bash
  kubectl get deployment metrics-server -n kube-system
  ```

- **Step 3: Use `kubectl top`**:
  Use the `kubectl top` command to view resource usage (CPU and memory) for nodes and pods:

  ```bash
  kubectl top nodes
  kubectl top pods
  ```

  This will show you the resource consumption for each pod and node in your cluster.

### 3. **Set Up Logging with Fluentd and Elasticsearch**:

Now, let's configure basic logging using Fluentd and Elasticsearch (EFK stack).

- **Step 1: Install Elasticsearch**:
  You can install Elasticsearch using Helm:

  ```bash
  helm install elasticsearch elastic/elasticsearch
  ```

- **Step 2: Install Fluentd**:
  Install Fluentd to collect logs and send them to Elasticsearch:

  ```bash
  helm install fluentd fluent/fluentd
  ```

- **Step 3: Install Kibana**:
  Install Kibana to visualize logs:

  ```bash
  helm install kibana elastic/kibana
  ```

- **Step 4: Verify the EFK Stack**:
  Check if the pods for Elasticsearch, Fluentd, and Kibana are running:

  ```bash
  kubectl get pods -l app=elasticsearch
  kubectl get pods -l app=fluentd
  kubectl get pods -l app=kibana
  ```

- **Step 5: Access Kibana**:
  Once the EFK stack is running, you can access Kibana to view logs by port-forwarding:

  ```bash
  kubectl port-forward svc/kibana 5601:5601
  ```

  Then visit `http://localhost:5601` to access Kibana and start viewing logs.

### 4. **View Kubernetes Events**:

Kubernetes events provide information about the cluster's state. To view events, run:

```bash
kubectl get events --sort-by='.metadata.creationTimestamp'
```

This will display events in the cluster, such as pod creations, errors, and other status changes.

---

## **Real-World Example:**

In real-world scenarios, logging and monitoring are essential for:

- **Detecting issues**: For instance, if a pod is consuming too much CPU or memory, Prometheus can trigger an alert that can be viewed in Grafana.
- **Troubleshooting**: Logs from Fluentd in Kibana can help troubleshoot application issues (e.g., why a pod failed to start).
- **Scaling decisions**: With Kubernetes Metrics Server, you can set up Horizontal Pod Autoscalers that automatically scale the number of replicas of a pod based on CPU or memory usage.

For example, in a **production environment**, monitoring with Prometheus and Grafana ensures you can track the performance of your application and quickly detect bottlenecks or failures. Logging with Fluentd and Elasticsearch helps you identify issues at the application level.

---

## **Key Takeaways for Day 17:**

- **Prometheus** and **Grafana** are commonly used for cluster monitoring and visualization of resource usage.
- **Kubernetes Metrics Server** provides CPU and memory usage data for scaling and resource management.
- **Fluentd**, **Elasticsearch**, and **Kibana** (EFK Stack) allow you to aggregate, store, and visualize logs.
- **Kubernetes Events** provide important information about the state of the cluster.

---
