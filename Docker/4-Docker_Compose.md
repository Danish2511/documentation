# **Docker Compose**

## **Concepts:**

- **What is Docker Compose?**

  - Docker Compose is a tool that allows you to define and manage multi-container Docker applications. It uses a YAML file (`docker-compose.yml`) to configure the services (containers) that make up your application.
  - With Compose, you can easily start, stop, and manage multiple containers that work together, such as a web application and its database.

- **Why Use Docker Compose?**
  - **Multi-container applications**: When building apps that rely on multiple services (e.g., a frontend, backend, and database), Docker Compose allows you to define all the containers and their relationships in a single file.
  - **Simplified management**: Instead of running `docker run` for each container individually, you define the setup in one file and use simple commands to manage the entire stack.

## **Core Docker Compose Commands:**

1. **`docker-compose up`**: Starts the services defined in the `docker-compose.yml` file. If the images aren’t built yet, it will build them.

   - Example: `docker-compose up` (foreground)
   - Example: `docker-compose up -d` (detached mode)

2. **`docker-compose down`**: Stops and removes the containers, networks, and volumes created by `docker-compose up`.

   - Example: `docker-compose down`

3. **`docker-compose build`**: Builds the images defined in the `docker-compose.yml` file.
   - Example: `docker-compose build`

---

## **Hands-On Exercise:**

### 1. **Creating a Simple Multi-Container Application with Docker Compose:**

Let’s create a simple web app that uses two containers:

- A Python Flask web app.
- A Redis container to store data.

We'll define these two containers in a `docker-compose.yml` file.

- **Step 1: Create the project directory**:
  In your terminal, create a new directory and navigate into it:

  ```bash
  mkdir flask-redis-app
  cd flask-redis-app
  ```

- **Step 2: Create the Flask application**:
  Inside the `flask-redis-app` directory, create a file named `app.py` with the following content:

  ```python
  from flask import Flask, jsonify
  import redis

  app = Flask(__name__)

  # Connect to Redis
  r = redis.StrictRedis(host='redis', port=6379, db=0)

  @app.route('/')
  def hello():
      visits = r.incr('counter')
      return jsonify(message=f'Hello, World! You are visitor number {visits}.')

  if __name__ == "__main__":
      app.run(host='0.0.0.0', port=5000)
  ```

  - This app connects to a Redis server and increments a counter each time the root URL is visited.

- **Step 3: Create a `requirements.txt` file**:
  To specify the Python dependencies for the app, create a `requirements.txt` file:

  ```text
  flask
  redis
  ```

- **Step 4: Create a Dockerfile for the Flask app**:
  Create a file named `Dockerfile` in the same directory with the following content:

  ```dockerfile
  # Step 1: Use an official Python image from Docker Hub
  FROM python:3.8-slim

  # Step 2: Set the working directory inside the container
  WORKDIR /app

  # Step 3: Copy the app and requirements files
  COPY app.py /app/app.py
  COPY requirements.txt /app/requirements.txt

  # Step 4: Install the app dependencies
  RUN pip install -r requirements.txt

  # Step 5: Expose the port for Flask
  EXPOSE 5000

  # Step 6: Define the command to run the Flask app
  CMD ["python", "app.py"]
  ```

- **Step 5: Create the `docker-compose.yml` file**:
  Now, create a `docker-compose.yml` file that will define the two services: Flask app and Redis.

  ```yaml
  version: "3"
  services:
    web:
      build: .
      ports:
        - "5000:5000"
      depends_on:
        - redis
    redis:
      image: redis
  ```

  - **`web`**: Defines the Flask app. It builds the image from the current directory (`.`), maps port 5000 on your local machine to port 5000 inside the container, and specifies that it depends on the Redis service.
  - **`redis`**: Uses the official Redis image from Docker Hub.

### 2. **Start the Application with Docker Compose:**

Now, let’s start the application using Docker Compose.

```bash
docker-compose up
```

This will:

- Build the Flask app image from the Dockerfile.
- Pull the Redis image from Docker Hub (if not already downloaded).
- Start both containers (Flask app and Redis).

### 3. **Access the Application:**

- Open your browser and go to `http://localhost:5000`. You should see a message like:
  ```
  Hello, World! You are visitor number 1.
  ```
- Refresh the page to see the counter increase.

### 4. **Stopping the Application:**

To stop the application and remove the containers, use:

```bash
docker-compose down
```

---

## **Real-World Example:**

In a production environment, you might have a web application that uses multiple services like:

- A **backend API** (e.g., Flask, Django, Node.js).
- A **database** (e.g., PostgreSQL, MySQL, MongoDB).
- A **cache server** (e.g., Redis, Memcached).
- A **message queue** (e.g., RabbitMQ, Kafka).

With Docker Compose, you can define all these services in one `docker-compose.yml` file and easily manage them as a single unit.

---

## **Key Takeaways for Day 4:**

- **Docker Compose** is a tool for defining and managing multi-container applications.
- It uses a `docker-compose.yml` file to define services, networks, and volumes.
- **Key Docker Compose commands**:
  - `docker-compose up`: Starts the application.
  - `docker-compose down`: Stops the application and removes containers.
- Compose simplifies managing multiple containers that work together in your application.

---
