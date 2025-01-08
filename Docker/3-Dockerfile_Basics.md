# **Dockerfile Basics**

## **Concepts:**

- **What is a Dockerfile?**
  - A **Dockerfile** is a text file that contains a set of instructions for building a Docker image. The instructions specify how to set up the environment inside the container, including installing dependencies, copying files, and defining the entry point for the application.
  - A Dockerfile automates the process of building a Docker image, making it repeatable and versioned.

## **Basic Dockerfile Instructions:**

1. **`FROM`**: Specifies the base image for the new image. Every Dockerfile starts with `FROM`.

   - Example: `FROM ubuntu:20.04` – This starts with an official Ubuntu image.

2. **`RUN`**: Executes a command during the build process to install dependencies or make changes to the image.

   - Example: `RUN apt-get update && apt-get install -y python3`

3. **`COPY`**: Copies files or directories from the host machine to the container.

   - Example: `COPY . /app` – This copies everything from the current directory to `/app` in the container.

4. **`CMD`**: Defines the default command to run when the container starts. Only one `CMD` can be specified.

   - Example: `CMD ["python3", "app.py"]`

5. **`EXPOSE`**: Exposes a port to allow communication with the container.
   - Example: `EXPOSE 80`

---

## **Hands-On Exercise:**

### 1. **Creating Your First Dockerfile:**

Let’s build a simple Python-based web app.

- **Step 1: Create a directory for your project**:
  In your terminal, create a new directory and navigate into it:

  ```bash
  mkdir my-python-app
  cd my-python-app
  ```

- **Step 2: Create a simple Python script (`app.py`)**:
  Inside the `my-python-app` directory, create a file named `app.py` with the following content:

  ```python
  from http.server import SimpleHTTPRequestHandler
  from socketserver import TCPServer

  PORT = 8080
  Handler = SimpleHTTPRequestHandler
  with TCPServer(("", PORT), Handler) as httpd:
      print(f"Serving on port {PORT}")
      httpd.serve_forever()
  ```

- **Step 3: Create a `Dockerfile`**:
  In the same directory, create a `Dockerfile` with the following content:

  ```dockerfile
  # Step 1: Use an official Python image from Docker Hub
  FROM python:3.8-slim

  # Step 2: Copy the app code into the container
  COPY app.py /app/app.py

  # Step 3: Set the working directory inside the container
  WORKDIR /app

  # Step 4: Expose the port that the app will run on
  EXPOSE 8080

  # Step 5: Define the command to run the application
  CMD ["python3", "app.py"]
  ```

**What this Dockerfile does:**

- It starts with an official Python image.
- Copies your `app.py` file into the container.
- Sets the working directory inside the container to `/app`.
- Exposes port 8080 so the container can accept incoming requests.
- Runs the Python script using `CMD`.

### 2. **Build the Docker Image:**

Now, let's build the Docker image from the Dockerfile:

```bash
docker build -t my-python-app .
```

- The `-t` flag assigns a name to the image (`my-python-app`).
- The `.` represents the current directory, which contains the Dockerfile.

### 3. **Run the Container:**

Once the image is built, you can run the container:

```bash
docker run -d -p 8080:8080 my-python-app
```

- This runs the container in detached mode (`-d`).
- It maps port 8080 on your machine to port 8080 inside the container, where the app is running.

### 4. **Verify the Application:**

Open your browser and go to `http://localhost:8080`. You should see a simple message from the Python web server, confirming the app is running.

### 5. **Stop the Container:**

Once you're done, you can stop the running container:

```bash
docker ps  # To find the container ID
docker stop <container_id>
```

---

## **Real-World Example:**

Imagine you're developing a microservice that needs a specific Python version and dependencies. Instead of configuring each environment manually, you write a `Dockerfile` that defines everything, ensuring the app runs consistently across all machines (whether local, testing, or production).

In a larger setup, you might include more complex dependencies, like databases or cache servers, which you can add to the Dockerfile using `RUN` to install packages, or `COPY` to add more files.

---

## **Key Takeaways for Day 3:**

- A **Dockerfile** automates the creation of Docker images.
- **Basic Dockerfile instructions**:
  - `FROM`: Specifies the base image.
  - `RUN`: Executes commands to install dependencies.
  - `COPY`: Copies files from your host machine to the container.
  - `CMD`: Defines the default command to run when the container starts.
  - `EXPOSE`: Exposes ports for communication.
- Dockerfiles are used to ensure consistency in building and running applications in containers.

---
