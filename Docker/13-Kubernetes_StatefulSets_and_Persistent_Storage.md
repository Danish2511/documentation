# **Kubernetes StatefulSets and Persistent Storage**

## **Concepts:**

Stateful applications, such as databases, caches, and applications that maintain data between restarts, require a different management strategy in Kubernetes compared to stateless applications. Kubernetes offers **StatefulSets** and **Persistent Volumes (PVs)** to manage the state and data persistence for these applications.

1. **StatefulSets**:

   - A **StatefulSet** is a Kubernetes resource used to manage stateful applications. It ensures that each pod in the StatefulSet has a stable, unique identity and persistent storage, even after rescheduling or restarting.
   - StatefulSets provide features like **stable network IDs**, **persistent storage** (via Persistent Volumes), and **ordered pod deployment and scaling**.

2. **Persistent Volumes (PV) and Persistent Volume Claims (PVC)**:

   - **Persistent Volumes (PVs)** are a Kubernetes resource that represents storage in the cluster, typically backed by cloud storage, network-attached storage (NAS), or local storage.
   - **Persistent Volume Claims (PVCs)** are requests for storage. A PVC binds to a suitable PV that satisfies its storage requirements.
   - **StorageClasses** allow you to define the type of storage to be used for your PVs, such as SSD, HDD, or cloud-based storage options.

3. **StatefulSet vs. Deployment**:

   - **StatefulSets** are similar to **Deployments** but provide additional guarantees necessary for stateful applications. StatefulSets ensure each pod gets its own persistent storage and maintains a stable identity across pod restarts.

4. **Configuring StatefulSets with Persistent Storage**:
   - StatefulSets rely on **PVCs** to manage storage for each replica, allowing data to persist independently of the pod lifecycle.

---

## **Key Topics:**

- **StatefulSets**: Stateful application management in Kubernetes.
- **Persistent Volumes (PV)**: Cluster-wide storage resources.
- **Persistent Volume Claims (PVC)**: Requests for storage.
- **StorageClasses**: Managing different storage options.
- **StatefulSet vs. Deployment**: Key differences and when to use each.

---

## **Hands-On Exercise:**

### 1. **Create a StatefulSet with Persistent Storage**:

Let’s start by creating a StatefulSet for an Nginx application with persistent storage.

- **Step 1: Create a Persistent Volume (PV)**:
  First, create a Persistent Volume (PV) that will be used by the StatefulSet. Create a file named `nginx-pv.yaml`:

  ```yaml
  apiVersion: v1
  kind: PersistentVolume
  metadata:
    name: nginx-pv
  spec:
    capacity:
      storage: 1Gi
    accessModes:
      - ReadWriteOnce
    persistentVolumeReclaimPolicy: Retain
    hostPath:
      path: /mnt/data/nginx
  ```

  - This PV uses a local path `/mnt/data/nginx` on the node and provides 1Gi of storage.

- **Step 2: Apply the Persistent Volume**:
  Apply the PV configuration:

  ```bash
  kubectl apply -f nginx-pv.yaml
  ```

- **Step 3: Create a Persistent Volume Claim (PVC)**:
  Create a PVC that will request storage from the PV. Create a file named `nginx-pvc.yaml`:

  ```yaml
  apiVersion: v1
  kind: PersistentVolumeClaim
  metadata:
    name: nginx-pvc
  spec:
    accessModes:
      - ReadWriteOnce
    resources:
      requests:
        storage: 1Gi
  ```

  - This PVC will request 1Gi of storage.

- **Step 4: Apply the Persistent Volume Claim**:
  Apply the PVC configuration:

  ```bash
  kubectl apply -f nginx-pvc.yaml
  ```

- **Step 5: Create a StatefulSet with PVC**:
  Now, create a StatefulSet for Nginx that uses the PVC. Create a file `nginx-statefulset.yaml`:

  ```yaml
  apiVersion: apps/v1
  kind: StatefulSet
  metadata:
    name: nginx-statefulset
  spec:
    serviceName: "nginx"
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
            volumeMounts:
              - name: nginx-storage
                mountPath: /usr/share/nginx/html
    volumeClaimTemplates:
      - metadata:
          name: nginx-storage
        spec:
          accessModes:
            - ReadWriteOnce
          resources:
            requests:
              storage: 1Gi
  ```

  - This StatefulSet will create 3 replicas, each with its own persistent volume using the PVC.

- **Step 6: Apply the StatefulSet**:
  Apply the StatefulSet configuration:

  ```bash
  kubectl apply -f nginx-statefulset.yaml
  ```

- **Step 7: Verify StatefulSet Pods**:
  Check the StatefulSet and verify the pods:

  ```bash
  kubectl get statefulset
  kubectl get pods
  ```

  You should see 3 pods created, each with its unique name like `nginx-statefulset-0`, `nginx-statefulset-1`, and `nginx-statefulset-2`.

### 2. **Test the Persistent Storage**:

Now, let’s test the persistence of data between pod restarts.

- **Step 1: Access the Nginx Pods**:
  First, exec into one of the Nginx pods:

  ```bash
  kubectl exec -it nginx-statefulset-0 -- /bin/bash
  ```

- **Step 2: Write Data to Persistent Storage**:
  Inside the pod, write some data to the mounted storage:

  ```bash
  echo "Hello from nginx-statefulset-0" > /usr/share/nginx/html/index.html
  ```

- **Step 3: Verify the Data**:
  Access the Nginx service (use port forwarding or expose the StatefulSet as a service) and verify that the content is available.

- **Step 4: Restart a Pod**:
  Restart the pod to simulate a failure and check if the data persists:

  ```bash
  kubectl delete pod nginx-statefulset-0
  ```

  The pod should be recreated automatically, and the data should persist in the same location since it’s backed by a Persistent Volume.

### 3. **Clean Up**:

Once you're done testing, delete the StatefulSet, PVC, and PV:

```bash
kubectl delete statefulset nginx-statefulset
kubectl delete pvc nginx-pvc
kubectl delete pv nginx-pv
```

---

## **Real-World Example:**

In a real-world scenario, **StatefulSets** are commonly used to manage applications like databases (e.g., MySQL, PostgreSQL, MongoDB) and caching systems (e.g., Redis, Cassandra) that require persistent data storage and stable network identities.

- **StatefulSets** guarantee that each pod has its own persistent storage, which ensures that data survives restarts.
- **Persistent Volumes and PVCs** allow Kubernetes to manage the storage resources dynamically, abstracting the underlying storage infrastructure.

For example, in a **banking application**, you would use **StatefulSets** to ensure that each replica of the database has its own volume and stable identity, which is crucial for managing transactional data that cannot be lost.

---

## **Key Takeaways for Day 13:**

- **StatefulSets** manage stateful applications, providing stable network identities and persistent storage.
- **Persistent Volumes (PV)** and **Persistent Volume Claims (PVC)** provide a way to request and manage persistent storage in Kubernetes.
- **StorageClasses** help define different types of storage based on needs (e.g., SSD, HDD).
- StatefulSets ensure data persistence and stability for stateful applications like databases.

---
