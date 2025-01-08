# **Persistent Storage in Kubernetes**

## **Concepts:**

Kubernetes manages ephemeral (temporary) containers and pods, meaning that once a pod is terminated or restarted, all data stored in it is lost. However, for many applications, especially databases, caching systems, or file storage, data persistence is essential. Kubernetes provides **Persistent Volumes (PVs)** and **Persistent Volume Claims (PVCs)** to manage persistent storage.

## **Key Concepts:**

1. **Persistent Volumes (PVs)**:

   - A **Persistent Volume (PV)** is a piece of storage in the Kubernetes cluster that has been provisioned by an administrator. PVs can be backed by various storage systems (local disk, NFS, cloud storage like AWS EBS, GCP Persistent Disk, etc.).
   - PVs are independent of the lifecycle of pods, meaning that they persist even when pods are deleted.

2. **Persistent Volume Claims (PVCs)**:

   - A **Persistent Volume Claim (PVC)** is a request for storage by a user. A PVC specifies the size and access mode required for the storage.
   - Kubernetes matches the PVC with an available PV, and once the claim is satisfied, the storage is mounted into the pod.

3. **Storage Classes**:

   - **Storage Class** allows you to define different types of storage (e.g., fast SSDs, cheaper HDDs) that can be dynamically provisioned when a PVC is created. It is useful when you want to define different types of storage to meet specific performance or cost needs.

4. **Access Modes**:
   - **ReadWriteOnce (RWO)**: The volume can be mounted as read-write by a single node.
   - **ReadOnlyMany (ROX)**: The volume can be mounted as read-only by many nodes.
   - **ReadWriteMany (RWX)**: The volume can be mounted as read-write by many nodes.

---

## **Hands-On Exercise:**

### 1. **Creating a Persistent Volume (PV) and Persistent Volume Claim (PVC)**:

Let’s create a simple local storage PV and a PVC for a pod to use.

- **Step 1: Create a Persistent Volume (PV) Definition**:
  Create a file named `pv-definition.yaml` with the following content:

  ```yaml
  apiVersion: v1
  kind: PersistentVolume
  metadata:
    name: local-pv
  spec:
    capacity:
      storage: 1Gi
    volumeMode: Filesystem
    accessModes:
      - ReadWriteOnce
    persistentVolumeReclaimPolicy: Retain
    storageClassName: manual
    hostPath:
      path: /mnt/data
  ```

  - This YAML defines a **local PV** with 1Gi of storage, accessible by a single node (`ReadWriteOnce`), and using the `hostPath` of the Kubernetes node's file system at `/mnt/data`.

- **Step 2: Apply the PV Definition**:
  Run the following command to create the PV:

  ```bash
  kubectl apply -f pv-definition.yaml
  ```

- **Step 3: Verify the PV**:
  Check the status of the PV:

  ```bash
  kubectl get pv
  ```

  You should see `local-pv` listed as available.

- **Step 4: Create a Persistent Volume Claim (PVC) Definition**:
  Create a file named `pvc-definition.yaml` with the following content:

  ```yaml
  apiVersion: v1
  kind: PersistentVolumeClaim
  metadata:
    name: local-pvc
  spec:
    accessModes:
      - ReadWriteOnce
    resources:
      requests:
        storage: 1Gi
    storageClassName: manual
  ```

  - This YAML defines a PVC requesting 1Gi of storage with the access mode `ReadWriteOnce` and using the `manual` storage class.

- **Step 5: Apply the PVC Definition**:
  Run the following command to create the PVC:

  ```bash
  kubectl apply -f pvc-definition.yaml
  ```

- **Step 6: Verify the PVC**:
  Check the status of the PVC:

  ```bash
  kubectl get pvc
  ```

  The PVC should be bound to the `local-pv` you created earlier.

### 2. **Using the PVC in a Pod**:

Let’s now use the PVC to mount the persistent storage into a pod.

- **Step 1: Create a Pod Definition with PVC**:
  Create a file named `pod-with-pvc.yaml` with the following content:

  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: nginx-pod
  spec:
    containers:
      - name: nginx
        image: nginx
        volumeMounts:
          - mountPath: /usr/share/nginx/html
            name: nginx-storage
    volumes:
      - name: nginx-storage
        persistentVolumeClaim:
          claimName: local-pvc
  ```

  - This YAML defines a pod that runs the Nginx container and mounts the persistent storage (from the PVC `local-pvc`) at `/usr/share/nginx/html`.

- **Step 2: Apply the Pod Definition**:
  Run the following command to create the pod:

  ```bash
  kubectl apply -f pod-with-pvc.yaml
  ```

- **Step 3: Verify the Pod**:
  Check that the pod is running:

  ```bash
  kubectl get pods
  ```

  You should see `nginx-pod` running.

- **Step 4: Access the Pod’s Mounted Volume**:
  To verify the PVC is working correctly, exec into the pod and check the mounted volume:

  ```bash
  kubectl exec -it nginx-pod -- /bin/bash
  ```

  Once inside the pod, navigate to the mounted path `/usr/share/nginx/html`:

  ```bash
  ls /usr/share/nginx/html
  ```

  You should see the Nginx default index file.

- **Step 5: Clean Up**:
  After testing, delete the pod, PVC, and PV:
  ```bash
  kubectl delete pod nginx-pod
  kubectl delete pvc local-pvc
  kubectl delete pv local-pv
  ```

---

## **Real-World Example:**

In a real-world scenario, persistent storage is often used to store:

- **Database data**: Ensuring that data persists even if the application container is restarted.
- **Logs**: Keeping logs across pod restarts, so they are not lost.
- **Shared file storage**: Sharing files between multiple pods using persistent volumes with `ReadWriteMany` access mode.

For example, you could use **AWS EBS** or **Google Cloud Persistent Disk** as a PV in a cloud-based Kubernetes cluster to provide persistent storage for your applications.

---

## **Key Takeaways for Day 9:**

- **Persistent Volumes (PVs)** are used to manage storage in Kubernetes and persist data beyond pod lifecycles.
- **Persistent Volume Claims (PVCs)** allow users to request and manage storage resources.
- **HostPath** is an easy way to use local storage for testing, but in production, cloud providers’ storage solutions like **AWS EBS** or **GCP Persistent Disks** are commonly used.
- Persistent storage is critical for applications that need to retain data, such as databases and file systems.

---
