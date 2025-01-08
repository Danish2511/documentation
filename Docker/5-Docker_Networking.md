# **Docker Networking**

## **Concepts:**

- **What is Docker Networking?**
  - Docker provides networking capabilities to allow containers to communicate with each other and with the outside world. Docker creates virtual networks where containers can be connected and exchange data.
  - By default, Docker uses the **bridge network** for containers, but you can create custom networks for more control over container communication.

## **Docker Networking Types:**

1. **Bridge Network**:

   - The default network mode for containers.
   - Containers on the bridge network can communicate with each other using IP addresses.
   - Containers are isolated from the host machine but can access the outside world.

2. **Host Network**:

   - Containers share the host machine's network stack.
   - Containers are not isolated from the host, so they can access the host network directly.

3. **None Network**:

   - Containers have no network access.
   - Useful when you want to isolate a container completely from networking.

4. **Custom Networks**:
   - You can create custom networks to give containers more control over how they communicate with each other.

## **Important Networking Commands:**

- **`docker network ls`**: Lists all Docker networks.
- **`docker network inspect <network_name>`**: Shows detailed information about a specific network.
- **`docker network create <network_name>`**: Creates a new custom network.

---

## **Hands-On Exercise:**

### 1. **Using the Default Bridge Network:**

Let’s start by running two containers that will communicate using the default bridge network.

- **Step 1: Run Two Containers**:
  Run a `redis` container and a `nginx` container:

  ```bash
  docker run -d --name redis redis
  docker run -d --name nginx -p 8080:80 nginx
  ```

  - The `redis` container will run a Redis server.
  - The `nginx` container will run a web server.

- **Step 2: Check the Bridge Network**:
  By default, both containers will be on the bridge network. To inspect the bridge network:

  ```bash
  docker network inspect bridge
  ```

  You should see both containers listed under the `Containers` section, confirming they are on the same network.

- **Step 3: Test Communication Between Containers**:
  We can now test whether the containers can communicate with each other. Start an interactive session inside the `nginx` container:

  ```bash
  docker exec -it nginx bash
  ```

  Once inside, try to ping the `redis` container:

  ```bash
  ping redis
  ```

  You should see a successful ping response, confirming that containers on the same network can communicate with each other by their container name (`redis` in this case).

- **Step 4: Stop and Remove Containers**:
  After testing, stop and remove the containers:
  ```bash
  docker stop redis nginx
  docker rm redis nginx
  ```

### 2. **Creating a Custom Network:**

Now, let’s create a custom network and run containers that will communicate within this network.

- **Step 1: Create a Custom Network**:

  ```bash
  docker network create my_network
  ```

- **Step 2: Run Containers on the Custom Network**:
  Run the same `redis` and `nginx` containers, but this time on the custom network:

  ```bash
  docker run -d --name redis --network my_network redis
  docker run -d --name nginx --network my_network -p 8080:80 nginx
  ```

- **Step 3: Inspect the Custom Network**:
  To ensure that the containers are on the custom network, inspect the network:

  ```bash
  docker network inspect my_network
  ```

  You should see both containers listed under `Containers`.

- **Step 4: Test Communication Between Containers on the Custom Network**:
  Just like before, test the communication by running an interactive session in the `nginx` container and pinging the `redis` container:

  ```bash
  docker exec -it nginx bash
  ping redis
  ```

  You should again see a successful ping.

- **Step 5: Stop and Remove Containers and Network**:
  After testing, stop and remove the containers and the custom network:
  ```bash
  docker stop redis nginx
  docker rm redis nginx
  docker network rm my_network
  ```

---

## **Real-World Example:**

In production environments, you may use custom networks to isolate different parts of your application:

- For example, you might have an **API backend** and a **database**. By creating a custom network, you can ensure that only your backend can access the database container, and external traffic is blocked.
- Custom networks also allow you to configure **service discovery**, where containers can refer to each other by name, simplifying communication.

---

## **Key Takeaways for Day 5:**

- **Docker networking** allows containers to communicate with each other and the outside world.
- The **bridge network** is the default network for containers, but you can create custom networks for more control.
- Containers on the same network can communicate using their container names, and custom networks provide better isolation and flexibility.
- **Key Docker networking commands**:
  - `docker network ls`: List all networks.
  - `docker network create`: Create a custom network.
  - `docker network inspect`: Inspect a network’s details.

---
