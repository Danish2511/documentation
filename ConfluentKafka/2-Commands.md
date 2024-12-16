# Confluent Kafka Commands Cheat Sheet

This document lists essential Kafka commands with descriptions and examples to help you practice and understand Kafka operations.

## Kafka Topics Management

### 1. **Create a Topic**
Creates a new Kafka topic with the specified name, partitions, and replication factor.
```bash
kafka-topics --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 3 --topic my-topic
```
- **Parameters**:
  - `--bootstrap-server`: Address of the Kafka broker.
  - `--replication-factor`: Number of replicas for fault tolerance.
  - `--partitions`: Number of partitions for the topic.
  - `--topic`: Name of the topic.

### 2. **List Topics**
Lists all existing topics in the Kafka cluster.
```bash
kafka-topics --list --bootstrap-server localhost:9092
```

### 3. **Describe a Topic**
Displays detailed information about a topic, including partitions and replication.
```bash
kafka-topics --describe --bootstrap-server localhost:9092 --topic my-topic
```

### 4. **Delete a Topic**
Deletes a specified topic (if deletion is enabled in the Kafka configuration).
```bash
kafka-topics --delete --bootstrap-server localhost:9092 --topic my-topic
```

---

## Kafka Producers

### 1. **Produce Messages to a Topic**
Sends messages to a specified topic interactively.
```bash
kafka-console-producer --broker-list localhost:9092 --topic my-topic
```
- After running the command, type messages and press Enter to send each message to the topic.

### 2. **Produce Messages with a Key**
Send messages with a key-value pair for partitioning.
```bash
kafka-console-producer --broker-list localhost:9092 --topic my-topic --property "key.separator=:" --property "parse.key=true"
```
- Example input:
  ```
  key1:value1
  key2:value2
  ```

---

## Kafka Consumers

### 1. **Consume Messages from a Topic**
Reads messages from the beginning or latest offset of a topic.
```bash
kafka-console-consumer --bootstrap-server localhost:9092 --topic my-topic --from-beginning
```
- **Parameters**:
  - `--from-beginning`: Start consuming from the earliest offset.

### 2. **Consume Messages with Keys**
Reads key-value messages from a topic.
```bash
kafka-console-consumer --bootstrap-server localhost:9092 --topic my-topic --from-beginning --property print.key=true --property key.separator=:
```
- Output example:
  ```
  key1:value1
  key2:value2
  ```

---

## Kafka Consumer Groups

### 1. **List Consumer Groups**
Lists all active consumer groups in the Kafka cluster.
```bash
kafka-consumer-groups --bootstrap-server localhost:9092 --list
```

### 2. **Describe a Consumer Group**
Provides information about a specific consumer group, including its topic, partitions, and offsets.
```bash
kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group my-group
```

### 3. **Reset Offsets for a Consumer Group**
Resets the offsets for a consumer group to a specific value or position.
```bash
kafka-consumer-groups --bootstrap-server localhost:9092 --group my-group --topic my-topic --reset-offsets --to-earliest --execute
```
- **Options**:
  - `--to-earliest`: Reset to the earliest offset.
  - `--to-latest`: Reset to the latest offset.
  - `--shift-by [n]`: Shift offset forward or backward by `n` messages.

---

## Kafka Utilities

### 1. **Check Cluster Metadata**
Displays metadata about the Kafka cluster, including broker and topic information.
```bash
kafka-metadata-quorum --bootstrap-server localhost:9092 --describe
```

### 2. **Monitor Consumer Lag**
Checks the lag for a specific consumer group.
```bash
kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group my-group
```

### 3. **Delete Records from a Topic**
Deletes records from a topic up to a specific offset.
```bash
kafka-delete-records --bootstrap-server localhost:9092 --offset-json-file offsets.json
```
- Example `offsets.json`:
  ```json
  {
    "partitions": [
      { "topic": "my-topic", "partition": 0, "offset": 10 }
    ]
  }
  ```

---

## Kafka Schema Registry Commands

### 1. **Register a Schema**
Register a new Avro schema with the Schema Registry.
```bash
curl -X POST -H "Content-Type: application/vnd.schemaregistry.v1+json" \
     --data '{"schema": "{\"type\":\"record\",\"name\":\"User\",\"fields\":[{\"name\":\"id\",\"type\":\"int\"}]}"}' \
     http://localhost:8081/subjects/my-topic-value/versions
```

### 2. **List Registered Schemas**
List all registered schemas in the Schema Registry.
```bash
curl -X GET http://localhost:8081/subjects
```

### 3. **Get a Schema by ID**
Retrieve a schema by its unique ID.
```bash
curl -X GET http://localhost:8081/schemas/ids/1
```

### 4. **Check Schema Compatibility**
Check if a new schema is compatible with the existing schema.
```bash
curl -X POST -H "Content-Type: application/vnd.schemaregistry.v1+json" \
     --data '{"schema": "{\"type\":\"record\",\"name\":\"User\",\"fields\":[{\"name\":\"id\",\"type\":\"string\"}]}"}' \
     http://localhost:8081/compatibility/subjects/my-topic-value/versions/latest
```

---

## Practice Workflow
1. **Create a topic**: `my-topic`.
2. **Produce messages**: Send a few messages interactively or with keys.
3. **Consume messages**: Read messages from the topic.
4. **Explore consumer groups**: Create a consumer group and check its offsets.
5. **Use Schema Registry**: Register a schema and validate its compatibility.

These commands cover essential Kafka operations for hands-on practice. Feel free to expand this list based on your POC requirements!
