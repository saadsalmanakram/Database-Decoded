### **JSON Schema Validation in MongoDB**

**JSON Schema Validation** in MongoDB allows you to define rules and constraints for the structure of your documents using a JSON-like syntax. MongoDB uses **BSON** (Binary JSON) for storing data, but **JSON Schema** is used to define and validate the structure of those BSON documents. The `$jsonSchema` operator is used to define these validation rules for collections, providing flexibility in enforcing data integrity.

---

### **Why Use JSON Schema Validation in MongoDB?**

JSON Schema validation is useful for:

- **Ensuring Consistent Document Structure**: Guarantees that documents adhere to a certain format, including required fields, data types, and specific values.
- **Data Integrity**: Prevents invalid or inconsistent data from being inserted or updated in the database.
- **Defining Business Rules**: Enforces domain-specific validation logic directly at the database level.
- **Improved Query Performance**: By ensuring that data conforms to a schema, it can make querying more predictable and efficient.

---

### **How JSON Schema Validation Works in MongoDB**

MongoDB uses the `$jsonSchema` operator in the `createCollection` and `collMod` commands to define schema validation rules. These rules are applied when documents are inserted or updated in the collection. MongoDB will reject documents that do not meet the validation criteria.

---

### **Basic Structure of JSON Schema in MongoDB**

The JSON Schema used in MongoDB is based on the [JSON Schema Draft 7](https://json-schema.org/draft-07/) standard, with some MongoDB-specific extensions. The schema is defined as part of a **validator** and is applied to collections.

#### **Example 1: Basic JSON Schema Validation**

Here’s an example of creating a collection with basic JSON Schema validation that checks for specific field types and requirements:

```javascript
db.createCollection("users", {
  validator: {
    $jsonSchema: {
      bsonType: "object",  // The document should be an object
      required: ["name", "email", "age"],  // Specifies that 'name', 'email', and 'age' are required fields
      properties: {
        name: {
          bsonType: "string",  // The 'name' field must be a string
          description: "Name is required and must be a string"
        },
        email: {
          bsonType: "string",  // The 'email' field must be a string
          pattern: "^\\S+@\\S+\\.\\S+$",  // Email must match the regex pattern for valid email
          description: "Email is required and must be a valid email address"
        },
        age: {
          bsonType: "int",  // The 'age' field must be an integer
          minimum: 18,  // The 'age' must be at least 18
          description: "Age is required and must be an integer greater than or equal to 18"
        }
      }
    }
  }
});
```

- **`bsonType`**: Specifies the data type of a field (e.g., string, int, object, array, etc.).
- **`required`**: A list of fields that must be present in the document.
- **`properties`**: Defines validation rules for each field.

---

### **Common Validation Rules in JSON Schema**

Here are some commonly used rules in JSON Schema for MongoDB:

- **`bsonType`**: Specifies the expected data type for a field. Common values include:
  - `"string"`, `"int"`, `"double"`, `"object"`, `"array"`, `"date"`, etc.
  
- **`required`**: A list of fields that are mandatory in the document.

- **`pattern`**: Validates that a string field matches a regular expression. For example, for an email address, you can ensure the value matches a valid email format.

- **`minimum` and `maximum`**: Validates that numeric values are within a specific range. For example, an age must be at least 18.

- **`enum`**: Specifies a list of valid values for a field. For example, a `status` field might only accept `"active"`, `"inactive"`, or `"pending"`.

- **`items`**: Used to validate items in an array. For example, ensuring that each element of an array is of a specific type.

- **`dependencies`**: Specifies dependencies between fields. For example, the presence of one field might depend on the presence of another field.

---

### **Example of Advanced JSON Schema Validation**

Here’s an advanced example where you combine multiple validation features for a more complex document structure. Suppose you have a collection of `orders` where each order has a `customer`, `items`, and `total`:

```javascript
db.createCollection("orders", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["customer", "items", "total"],
      properties: {
        customer: {
          bsonType: "object",
          required: ["name", "email"],
          properties: {
            name: {
              bsonType: "string",
              description: "Customer name must be a string"
            },
            email: {
              bsonType: "string",
              pattern: "^\\S+@\\S+\\.\\S+$",
              description: "Customer email must be a valid email address"
            }
          }
        },
        items: {
          bsonType: "array",
          items: {
            bsonType: "object",
            required: ["productId", "quantity"],
            properties: {
              productId: {
                bsonType: "objectId",
                description: "Each item must have a valid product ID"
              },
              quantity: {
                bsonType: "int",
                minimum: 1,
                description: "Quantity must be a positive integer"
              }
            }
          },
          description: "Items must be an array of products with valid product IDs and quantities"
        },
        total: {
          bsonType: "double",
          minimum: 0,
          description: "Total price must be a non-negative number"
        }
      }
    }
  }
});
```

- **Embedded Objects**: The `customer` field contains a nested object, and `items` is an array of objects.
- **Validation in Arrays**: The `items` array must contain objects with `productId` and `quantity`, and each item must adhere to the validation rules defined in the `items` field.

---

### **Validating Updates Using JSON Schema**

You can apply schema validation not only during inserts but also during updates. MongoDB will validate documents whenever they are inserted or updated.

Example: When updating an existing document, MongoDB checks whether the new document matches the validation rules:

```javascript
db.orders.updateOne(
  { _id: 1 },
  { $set: { total: 100.50, items: [{ productId: ObjectId("abc123"), quantity: 2 }] } }
);
```

If the updated document doesn't comply with the schema (e.g., if `items` has an invalid product ID or missing `quantity`), the update operation will fail.

---

### **Validation Action in MongoDB**

MongoDB allows you to specify what happens when a document does not conform to the defined validation rules using the `validationAction` option:

- **`"error"`**: The operation is rejected if the document doesn't pass validation.
- **`"warn"`**: The operation is allowed, but MongoDB issues a warning if the document violates the validation rules.

```javascript
db.createCollection("orders", {
  validator: {
    $jsonSchema: { /* schema definition here */ }
  },
  validationAction: "error"  // Rejects invalid documents
});
```

You can set `validationAction` to `"warn"` if you want MongoDB to proceed with the operation but log a warning.

---

### **Modifying Schema Validation**

You can modify the schema validation for an existing collection using the `collMod` command. For instance, you may need to add new fields to the validation rules or modify existing constraints.

Example:

```javascript
db.runCommand({
  collMod: "orders",
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["customer", "items", "total", "status"],
      properties: {
        status: {
          bsonType: "string",
          enum: ["pending", "shipped", "delivered"],
          description: "Status must be one of 'pending', 'shipped', or 'delivered'"
        }
      }
    }
  }
});
```

This example adds a new `status` field to the `orders` collection schema, where the status must be one of the specified values.

---

### **Conclusion**

**JSON Schema Validation** in MongoDB is a powerful way to enforce data consistency and integrity at the database level. By defining validation rules using the `$jsonSchema` operator, MongoDB ensures that documents adhere to a defined structure and content type, even in a schema-less environment. This approach offers flexibility and allows you to define complex validation logic for your collections, ensuring that only valid data is stored, which helps improve data quality and application reliability.
