# **Docker Images and Containers**

## **Concepts:**
- **What Are Docker Images?**
  - A **Docker image** is a snapshot of an application and its dependencies at a certain point in time. It is a read-only template used to create Docker containers. Think of it as the blueprint for a container.
  - An image can contain the app code, libraries, binaries, configurations, and anything the app needs to run.

- **What Are Docker Containers?**
  - A **Docker container** is a running instance of a Docker image. It includes everything defined in the image but is mutable while running, meaning you can modify or interact with the container (e.g., install software, run commands).
  - Containers are isolated from each other and from the host system. They can interact with each other via networking.

- **How Docker Images and Containers Work Together:**
  - You first build or pull an image (using `docker pull`), and then run that image as a container (using `docker run`).
  - Containers are created from images, and you can have multiple containers running from the same image.

---

## **Hands-On Exercise:**

### 1. **Pulling a Docker Image**:
   - Open your terminal and pull an image from Docker Hub. Let's pull the `nginx` image, which is a popular web server.
   ```bash
   docker pull nginx
   ```

   **What Happens?**
   - Docker will search for the `nginx` image on Docker Hub and download it to your local machine.

### 2. **Running a Container**:
   - After the image is pulled, let's run the `nginx` container:
   ```bash
   docker run -d -p 8080:80 nginx
   ```

   **What Happens?**
   - The `-d` flag tells Docker to run the container in detached mode (in the background).
   - The `-p 8080:80` flag maps port 8080 on your host machine to port 80 in the container (where the Nginx web server is listening).
   - The `nginx` at the end specifies which image to use.

   Now, open your browser and go to `http://localhost:8080`. You should see the default Nginx welcome page!

### 3. **Listing Containers**:
   - To see the list of running containers, use:
   ```bash
   docker ps
   ```

   **Stopping the Container**:
   - To stop the container you just started, use:
   ```bash
   docker stop <container_id>
   ```
   - You can find the container ID from the output of `docker ps`.

### 4. **Inspecting Containers**:
   - To inspect the details of the container, including the configuration and environment variables, use:
   ```bash
   docker inspect <container_id>
   ```

### 5. **Removing Containers**:
   - To remove a container after stopping it, use:
   ```bash
   docker rm <container_id>
   ```

---

### **Real-World Example:**
Let's say you're a developer building a web application. You want to run the application locally with a web server like Nginx but don’t want to install it on your local machine. Instead, you can pull the Nginx Docker image, run it in a container, and it will serve as your local web server for testing purposes.

In production, you might have multiple web servers running as Docker containers, all using the same image but with different configurations or scaling based on demand.

---

### **Key Takeaways for Day 2:**
- **Docker Images** are the templates used to create containers.
- **Docker Containers** are the running instances of those images.
- You can pull pre-built images from **Docker Hub** and run them as containers on your machine.
- Docker containers are lightweight, portable, and isolated environments for running applications.

---