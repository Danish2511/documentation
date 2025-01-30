# **Understanding Compatibility Modes in Kafka Schema Registry**

Compatibility modes in Kafka Schema Registry ensure that schema changes do not break existing producers or consumers. They define how new schemas can evolve while maintaining compatibility with older versions. Let’s break down the three main compatibility modes: **BACKWARD**, **FORWARD**, and **FULL**, with simple explanations and real-world examples.

---

## **1. What Are Compatibility Modes?**

Compatibility modes are rules that determine how a new schema version can differ from the previous version without causing issues for producers or consumers. They ensure that:

- **Producers** can write data using a new schema, and **consumers** can still read it using an old schema.
- **Consumers** can read data written with an old schema, even if they are using a new schema.

---

## **2. Compatibility Modes Explained**

### **BACKWARD Compatibility**

- **What it means**: Data written with the **new schema** can be read by consumers using the **old schema**.
- **Example**: You add a new field to the schema, but consumers using the old schema can still read the data (they just ignore the new field).
- **Use Case**: When you want to ensure that consumers using older schemas can still process new data.

#### **Realtime Example**

- **Old Schema**:
  ```json
  {
    "type": "record",
    "name": "User",
    "fields": [
      { "name": "id", "type": "int" },
      { "name": "name", "type": "string" }
    ]
  }
  ```
- **New Schema** (adds `email` field):
  ```json
  {
    "type": "record",
    "name": "User",
    "fields": [
      { "name": "id", "type": "int" },
      { "name": "name", "type": "string" },
      { "name": "email", "type": "string", "default": "" }
    ]
  }
  ```
- **What happens**: Consumers using the old schema can still read data produced with the new schema because the `email` field has a default value.

---

### **FORWARD Compatibility**

- **What it means**: Data written with the **old schema** can be read by consumers using the **new schema**.
- **Example**: You remove a field from the schema, but consumers using the new schema can still read data written with the old schema (they just ignore the missing field).
- **Use Case**: When you want to ensure that consumers using newer schemas can still process old data.

#### **Realtime Example**

- **Old Schema**:
  ```json
  {
    "type": "record",
    "name": "User",
    "fields": [
      { "name": "id", "type": "int" },
      { "name": "name", "type": "string" },
      { "name": "email", "type": "string" }
    ]
  }
  ```
- **New Schema** (removes `email` field):
  ```json
  {
    "type": "record",
    "name": "User",
    "fields": [
      { "name": "id", "type": "int" },
      { "name": "name", "type": "string" }
    ]
  }
  ```
- **What happens**: Consumers using the new schema can still read data produced with the old schema because the `email` field is ignored.

---

### **FULL Compatibility**

- **What it means**: Both **BACKWARD** and **FORWARD** compatibility are enforced. The new schema must be compatible with both older and newer versions.
- **Example**: You can add or remove fields with default values, but you cannot make breaking changes like renaming or changing the type of a field.
- **Use Case**: When you want to ensure maximum compatibility between all versions of the schema.

#### **Realtime Example**

- **Old Schema**:
  ```json
  {
    "type": "record",
    "name": "User",
    "fields": [
      { "name": "id", "type": "int" },
      { "name": "name", "type": "string" }
    ]
  }
  ```
- **New Schema** (adds `email` field with a default value):
  ```json
  {
    "type": "record",
    "name": "User",
    "fields": [
      { "name": "id", "type": "int" },
      { "name": "name", "type": "string" },
      { "name": "email", "type": "string", "default": "" }
    ]
  }
  ```
- **What happens**: Consumers using the old schema can read data produced with the new schema (BACKWARD), and consumers using the new schema can read data produced with the old schema (FORWARD).

---

## **3. Real-World Analogy**

Think of compatibility modes like **upgrading a smartphone app**:

- **BACKWARD Compatibility**: The new app version works with old data files (e.g., a new photo editor can still open photos saved by the old version).
- **FORWARD Compatibility**: The old app version works with new data files (e.g., the old photo editor can still open photos saved by the new version, even if it ignores new features).
- **FULL Compatibility**: Both the old and new app versions can work with each other’s data files seamlessly.

---

## **4. Summary of Compatibility Modes**

| Mode         | Old Schema → New Data | New Schema → Old Data | Use Case                                |
| ------------ | --------------------- | --------------------- | --------------------------------------- |
| **BACKWARD** | ✅ Yes                | ❌ No                 | Consumers using old schemas must work.  |
| **FORWARD**  | ❌ No                 | ✅ Yes                | Producers using old schemas must work.  |
| **FULL**     | ✅ Yes                | ✅ Yes                | Both producers and consumers must work. |

---

## **5. How to Set Compatibility Mode in Schema Registry**

You can set the compatibility mode for a subject (e.g., `user-value`) using the Schema Registry REST API:

```bash
curl -X PUT -H "Content-Type: application/vnd.schemaregistry.v1+json" \
  --data '{"compatibility": "BACKWARD"}' \
  http://localhost:8081/config/user-value
```

---

## **6. Best Practices**

- Use **BACKWARD** compatibility when evolving schemas in a consumer-driven system.
- Use **FORWARD** compatibility when evolving schemas in a producer-driven system.
- Use **FULL** compatibility for maximum flexibility and safety.
- Always test schema changes in a staging environment before deploying to production.

---
