# **Introduction to Docker**

## **Concepts:**

- **What is Docker?**
  - Docker is a platform that enables developers to package applications and their dependencies into isolated environments called **containers**. These containers can run on any machine with Docker installed, making it easier to develop, test, and deploy applications consistently.
- **Why Use Docker?**
  - **Portability**: Docker containers can run the same way on any machine (development, testing, production).
  - **Consistency**: The app and all its dependencies are packaged together, avoiding "it works on my machine" issues.
  - **Efficiency**: Containers are lightweight and use fewer resources compared to traditional virtual machines.
- **Containerization vs Virtualization**:
  - **Virtualization**: VMs run entire operating systems, which can be heavy on resources. Each VM requires its own OS instance.
  - **Containerization**: Containers share the host OS but run isolated environments, which makes them lightweight and fast.

## **Key Concepts in Docker**:

- **Images**: A blueprint or template used to create containers. Images are read-only and include the application code, libraries, and dependencies.
- **Containers**: Running instances of Docker images. Containers are isolated from each other and from the host system but can interact with each other through networking.
- **Docker Engine**: The core component of Docker that runs and manages containers.

---

## **Hands-On Exercise:**

### 1. **Install Docker on Your Machine**:

- **Windows/Mac**: Download and install **[Docker Desktop](https://www.docker.com/products/docker-desktop)**.
- **Linux**: Follow installation steps from Docker’s official guide: **[Install Docker on Linux](https://docs.docker.com/get-docker/)**.

After installation, verify the installation by running the following command in the terminal/command prompt:

```bash
docker --version
```

### 2. **Run Your First Docker Container**:

Let's run a simple Docker container called `hello-world` to ensure everything is set up correctly.

Open your terminal and run:

```bash
docker run hello-world
```

**What Happens?**

- Docker will pull the `hello-world` image from Docker Hub (if not already downloaded).
- The container will run and print a success message confirming that Docker is installed and working properly.

### 3. **List Running Containers**:

After running a container, you can see the list of active containers using:

```bash
docker ps
```

If you want to see all containers (running or stopped), use:

```bash
docker ps -a
```

---

## **Real-World Example:**

In a typical software development pipeline, Docker allows developers to run applications in isolated containers. For example, a web developer can use Docker to run a local instance of a web server (like Nginx) alongside a database (like PostgreSQL), ensuring that the application will run the same way on a developer’s machine, testing environment, and production.

---

## **Key Takeaways for Day 1:**

- **Docker** is a platform that enables you to create, deploy, and run applications inside containers.
- Containers are isolated environments that contain everything an application needs to run.
- Docker makes development more portable, consistent, and efficient.

---
