# Setup: Installing and Configuring Confluent Kafka on WSL Ubuntu

This document provides a detailed guide to setting up Confluent Kafka on a WSL (Windows Subsystem for Linux) Ubuntu environment. Follow the steps below to ensure a smooth installation and configuration.

## Prerequisites

Before starting, make sure you have:

1. **Windows with WSL Installed**:

   - Install WSL and set up Ubuntu. Follow the official [WSL installation guide](https://learn.microsoft.com/en-us/windows/wsl/install).

2. **Java Installed**:

   - Confluent Kafka requires Java 8 or higher. Check if Java is installed:
     ```bash
     java -version
     ```
     If not installed, run:
     ```bash
     sudo apt update
     sudo apt install openjdk-17-jdk -y
     ```

3. **cURL Installed**:

   - Required for downloading the Confluent Platform. Install cURL if not already present:
     ```bash
     sudo apt install curl -y
     ```

4. **Kafka-UI Jar**:
   - Download the Kafka-UI jar file from the [official Kafka-UI repository](https://github.com/provectus/kafka-ui/releases).

## Step-by-Step Installation

### Step 1: Download Confluent Kafka

1. Use the cURL command to download the Confluent Platform archive:

   ```bash
   curl -O https://packages.confluent.io/archive/7.8/confluent-7.8.0.zip
   ```

2. Install unzip if not already present:

   ```bash
   sudo apt install unzip -y
   ```

3. Extract the contents of the downloaded ZIP file:

   ```bash
   unzip confluent-7.8.0.zip
   ```

   After extraction, you should see the following directories:

   | Folder      | Description                                      |
   | ----------- | ------------------------------------------------ |
   | `/bin/`     | Driver scripts for starting/stopping services    |
   | `/etc/`     | Configuration files                              |
   | `/lib/`     | Systemd services                                 |
   | `/libexec/` | Multi-platform CLI binaries                      |
   | `/share/`   | Jars and licenses                                |
   | `/src/`     | Source files requiring platform-dependent builds |

### Step 2: Configure Environment Variables

1. Open the `~/.bashrc` file for editing:

   ```bash
   nano ~/.bashrc
   ```

2. Add the following lines to set up `CONFLUENT_HOME` and update the `PATH`:

   ```bash
   export CONFLUENT_HOME=~/confluent-7.8.0
   export PATH=$PATH:$CONFLUENT_HOME/bin
   ```

3. Save the file and reload the configuration:
   ```bash
   source ~/.bashrc
   ```

### Step 3: Start the Services

#### Start ZooKeeper

ZooKeeper coordinates the distributed Kafka setup. Run:

```bash
zookeeper-server-start $CONFLUENT_HOME/etc/kafka/zookeeper.properties
```

#### Start Kafka Broker

Start the Kafka server (broker):

```bash
kafka-server-start $CONFLUENT_HOME/etc/kafka/server.properties
```

#### Start Schema Registry

Run the Schema Registry server to manage schemas:

```bash
schema-registry-start $CONFLUENT_HOME/etc/schema-registry/schema-registry.properties
```

#### Start Kafka-UI

Kafka-UI provides a graphical interface to interact with Kafka. Run:

```bash
java -Dspring.config.additional-location=application-local.yml --add-opens java.rmi/javax.rmi.ssl=ALL-UNNAMED -jar kafka-ui.jar
```

### Step 4: Verify the Setup

1. Access Kafka-UI:

   - Open a browser and navigate to `http://localhost:<configured-port>`.

2. Confirm all services are running:

   - ZooKeeper
   - Kafka Broker
   - Schema Registry
   - Kafka-UI

3. Test message production and consumption using Kafka commands.

## Troubleshooting

| Issue                          | Possible Solution                                       |
| ------------------------------ | ------------------------------------------------------- |
| Ports are already in use       | Use `netstat` to check and kill conflicting processes.  |
| ZooKeeper/Kafka fails to start | Check logs in the `logs` directory for errors.          |
| Kafka-UI not loading           | Ensure `application-local.yml` is correctly configured. |

## Next Steps

- Proceed to [SchemaRegistry.md](./SchemaRegistry.md) to explore Schema Registry concepts and usage.
- Test the setup by creating Kafka topics, producing, and consuming messages.
