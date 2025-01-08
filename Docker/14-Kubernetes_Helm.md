# **Kubernetes Helm**

## **Concepts:**

Helm is a package manager for Kubernetes, allowing you to define, install, and manage applications on Kubernetes in a more structured and reusable way. Helm uses **charts**, which are pre-configured Kubernetes resources packaged together for easy deployment.

1. **Helm Basics**:

   - Helm packages Kubernetes resources into a **chart**, which is a collection of YAML files that describe a set of Kubernetes resources.
   - A Helm chart simplifies the deployment and management of applications, enabling you to reuse configurations, parameterize deployments, and manage complex applications as a unit.

2. **Helm Charts**:

   - A Helm chart typically includes:
     - **Templates**: Kubernetes manifest files that can be parameterized.
     - **Values**: A configuration file where users can specify variables to customize the chart.
     - **Chart.yaml**: Metadata about the chart, such as name, version, and description.

3. **Helm Repositories**:

   - Helm charts are stored in repositories, which can be public or private. The most common public Helm chart repository is [Helm Hub](https://artifacthub.io/).

4. **Helm Commands**:

   - `helm install`: Installs a Helm chart.
   - `helm upgrade`: Upgrades an existing Helm release.
   - `helm list`: Lists installed releases.
   - `helm rollback`: Rolls back to a previous release.
   - `helm delete`: Deletes a release.

5. **Helm Releases**:
   - When you install a chart using Helm, a **release** is created. A release is a running instance of a chart in your cluster. Helm can manage multiple versions of a release, making it easy to upgrade or roll back.

---

## **Key Topics:**

- **Helm Basics**: Understanding charts and releases.
- **Creating Helm Charts**: Packaging your own application.
- **Using Helm Repositories**: Installing charts from repositories.
- **Helm Commands**: Installing, upgrading, and managing releases.

---

## **Hands-On Exercise:**

### 1. **Install Helm**:

First, ensure you have Helm installed on your local machine. You can install it via a package manager or from the official Helm website.

- **On macOS (using Homebrew)**:

  ```bash
  brew install helm
  ```

- **On Linux (using Snap)**:

  ```bash
  sudo snap install helm --classic
  ```

- **Verify Installation**:
  Check that Helm is installed by running:
  ```bash
  helm version
  ```

### 2. **Add a Helm Repository**:

Helm uses repositories to store charts. Add the official Helm charts repository to your setup.

- **Step 1: Add Helm Stable Repository**:
  Run the following command to add the stable charts repository:

  ```bash
  helm repo add stable https://charts.helm.sh/stable
  ```

- **Step 2: Update the Repository**:
  Run the following command to update your repositories:

  ```bash
  helm repo update
  ```

- **Step 3: Search for Helm Charts**:
  Search for available charts, such as the `nginx` chart:
  ```bash
  helm search repo nginx
  ```

### 3. **Install a Helm Chart**:

Now, let’s install a Helm chart. We will install the `nginx` chart.

- **Step 1: Install the Nginx Chart**:
  Run the following command to install Nginx using Helm:

  ```bash
  helm install my-nginx stable/nginx-ingress
  ```

  - This installs Nginx ingress controller in your Kubernetes cluster with the default configuration.

- **Step 2: Verify the Installation**:
  Check the status of the Nginx release:

  ```bash
  helm list
  ```

  You should see the `my-nginx` release listed.

- **Step 3: Verify Nginx Deployment**:
  Verify that the Nginx pods are running:

  ```bash
  kubectl get pods
  ```

  You should see the Nginx pods running.

### 4. **Customizing Helm Releases with Values**:

Helm allows you to customize the chart by providing a values file or setting values directly via the command line.

- **Step 1: Customize with a Values File**:
  You can override default values by creating a custom `values.yaml` file. For example, create a file `nginx-values.yaml` with custom settings:

  ```yaml
  controller:
    replicaCount: 2
  ```

- **Step 2: Install with Custom Values**:
  Run the following command to install the Nginx chart with your custom values:

  ```bash
  helm install my-nginx-custom -f nginx-values.yaml stable/nginx-ingress
  ```

- **Step 3: Verify Customization**:
  Check if the number of replicas was updated:

  ```bash
  kubectl get pods
  ```

  You should now see 2 replicas of the Nginx ingress controller.

### 5. **Upgrading a Helm Release**:

You can upgrade your release to apply new configurations or use a new version of the chart.

- **Step 1: Upgrade the Release**:
  Update the release with a new version of the chart or changes to values:

  ```bash
  helm upgrade my-nginx-custom stable/nginx-ingress
  ```

- **Step 2: Verify the Upgrade**:
  After upgrading, check the release status:
  ```bash
  helm status my-nginx-custom
  ```

### 6. **Rolling Back a Helm Release**:

If you need to roll back to a previous version of the release, you can use the `helm rollback` command.

- **Step 1: Roll Back the Release**:
  List the release versions:

  ```bash
  helm history my-nginx-custom
  ```

  Roll back to a previous version:

  ```bash
  helm rollback my-nginx-custom 1
  ```

  - This rolls back the release to version 1.

- **Step 2: Verify the Rollback**:
  Check the release status after the rollback:
  ```bash
  helm status my-nginx-custom
  ```

### 7. **Uninstalling a Helm Release**:

Once you're done, you can uninstall the release to clean up your environment.

- **Step 1: Uninstall the Release**:
  Run the following command to uninstall the `my-nginx-custom` release:

  ```bash
  helm uninstall my-nginx-custom
  ```

- **Step 2: Verify Uninstallation**:
  Verify that the release is removed:
  ```bash
  helm list
  ```

---

## **Real-World Example:**

In real-world scenarios, Helm is often used to manage complex applications, such as:

- **CI/CD Pipelines**: Helm is used to deploy applications, services, or microservices in Kubernetes environments. CI/CD tools (e.g., Jenkins, GitLab CI) can automate Helm chart deployments based on source code changes.
- **Cloud-Native Apps**: Helm charts are used to deploy applications like databases, caches, web servers, or message brokers in a Kubernetes environment.
- **Multi-Tenant Platforms**: Helm charts can help in managing different applications, ensuring consistent deployments across various environments like staging and production.

For example, in a **microservices-based e-commerce platform**, Helm charts can be used to deploy various services such as payment gateways, inventory management, product catalog, etc. Helm simplifies the management, upgrading, and scaling of these services across multiple environments.

---

## **Key Takeaways for Day 14:**

- **Helm** simplifies Kubernetes application management by packaging resources into reusable charts.
- Helm allows you to **parameterize** deployments, making it easier to manage configurations across different environments.
- Helm provides commands like `install`, `upgrade`, `rollback`, and `delete` to manage applications in a Kubernetes cluster.
- You can **customize Helm charts** using values files and install applications quickly with minimal effort.

---
