# Mappings and Data Types in Elasticsearch

Mappings are the way you define the structure of the documents in an index. They specify:

- **Field Names:** The keys in each document.
- **Field Data Types:** Such as text, keyword, integer, date, etc.
- **Field Properties:** Like analyzers for text fields or formats for date fields.

Having control over mappings is crucial because it ensures data is stored and retrieved correctly. Today, we’ll learn how to create custom mappings, and we’ll update the mapping for our employees index.

## 1. Understanding Data Types in Elasticsearch

Some common data types you’ll use:

- **Text:** Used for full-text search (e.g., `name`, `position`).
- **Keyword:** Used for exact matches or aggregations (e.g., `department`).
- **Integer:** Whole numbers (e.g., `age`, `salary`).
- **Float/Double:** Decimal numbers.
- **Date:** Dates in various formats (e.g., `hire_date`).

Each type serves a specific purpose, so choosing the correct one helps with data analysis and retrieval performance.

## 2. Creating a Custom Mapping

Elasticsearch automatically assigns data types when you create documents, but it’s often better to define your own mapping.

### Step 1: Delete the Existing Index

Let’s delete the current `employee` index so we can start fresh with a custom mapping:

- **Method**: `DELETE`
- **URL**: `http://localhost:9200/employee?pretty`

Click **Send**. You will get back the document as a JSON response.

- This command delete the employee index.

### Step 2: Create an Index with Custom Mappings

We’ll create a new `employee` index and define the mappings explicitly. Run this command to set up the new index with a specific mapping:

- **Method**: `PUT`
- **URL**: `http://localhost:9200/employee?pretty`
- **Body** (select `raw` and choose `JSON` format):

Click **Send**. You will get back the document as a JSON response.

```json
{
  "mappings": {
    "properties": {
      "name": { "type": "text" },
      "position": { "type": "text" },
      "age": { "type": "integer" },
      "salary": { "type": "integer" },
      "hire_date": { "type": "date", "format": "yyyy-MM-dd" },
      "department": { "type": "keyword" }
    }
  }
}
```

- **name** and **position** are set to **text** because we want these fields to be searchable.
- **age** and **salary** are **integers**.
- **hire_date** is of type **date** with a specific format (`yyyy-MM-dd`).
- **department** is a **keyword**, which will allow for filtering and exact matches.

If the mapping is created successfully, you should see an acknowledgment message in the JSON response.

### Step 3: Verify the Mapping

You can view the mapping structure of an index with:

- **Method**: `GET`
- **URL**: `http://localhost:9200/employee/_mapping?pretty`

Click **Send**. You will get back the document as a JSON response.

```json
{
  "employee": {
    "mappings": {
      "properties": {
        "age": {
          "type": "integer"
        },
        "department": {
          "type": "keyword"
        },
        "hire_date": {
          "type": "date",
          "format": "yyyy-MM-dd"
        },
        "name": {
          "type": "text"
        },
        "position": {
          "type": "text"
        },
        "salary": {
          "type": "integer"
        }
      }
    }
  }
}
```

## 3. Re-Add Documents to the New Index
Now let’s add the employee documents back to the `employee` index using the new mapping.

### Add Employee 1 (John Doe):
