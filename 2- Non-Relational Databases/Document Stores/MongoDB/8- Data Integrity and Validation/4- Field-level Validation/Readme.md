### **Field-level Validation in MongoDB**

Field-level validation in MongoDB allows you to enforce constraints on individual fields within documents, ensuring that data inserted or updated adheres to the specified rules. This can be done using **JSON Schema validation** within MongoDB to define validation criteria for each field.

Field-level validation ensures that each field follows its required format, value range, or other constraints before being persisted in the database.

---

### **Types of Field-level Validation**

1. **Type Validation (bsonType)**
2. **Required Fields Validation (required)**
3. **Value Constraints Validation (enum, minimum, maximum, etc.)**
4. **Pattern Matching (pattern)**
5. **Array Field Validation (items, minItems, maxItems)**
6. **Subdocument Field Validation (properties, required)**
7. **Range or Limit Validation (minimum, maximum, exclusiveMinimum, exclusiveMaximum)**

---

### **1. Type Validation (`bsonType`)**

This is the most basic form of field-level validation, where you specify the data type of the field. MongoDB supports various BSON data types, such as `string`, `int`, `double`, `date`, `objectId`, `boolean`, etc.

#### **Example:**

```javascript
{
  bsonType: "object",
  properties: {
    name: {
      bsonType: "string",  // 'name' field must be a string
      description: "Name must be a string"
    },
    age: {
      bsonType: "int",  // 'age' field must be an integer
      description: "Age must be an integer"
    }
  }
}
```

---

### **2. Required Fields Validation (`required`)**

The `required` rule specifies which fields are mandatory in the document. If any required field is missing, MongoDB will reject the document.

#### **Example:**

```javascript
{
  bsonType: "object",
  required: ["name", "email"],  // 'name' and 'email' are required fields
  properties: {
    name: {
      bsonType: "string"
    },
    email: {
      bsonType: "string"
    }
  }
}
```

---

### **3. Value Constraints Validation (enum, minimum, maximum, etc.)**

MongoDB supports various value constraints to restrict the allowable values for a field. This includes:

- **`enum`**: Specifies a set of allowable values.
- **`minimum` and `maximum`**: Validates that numeric values fall within the specified range.
- **`exclusiveMinimum` and `exclusiveMaximum`**: Ensures the value is strictly greater or less than a specified value.

#### **Example:**

```javascript
{
  bsonType: "object",
  properties: {
    status: {
      bsonType: "string",
      enum: ["active", "inactive", "pending"],  // 'status' must be one of these values
      description: "Status must be one of 'active', 'inactive', or 'pending'"
    },
    age: {
      bsonType: "int",
      minimum: 18,  // Age must be at least 18
      maximum: 100,  // Age must not exceed 100
      description: "Age must be between 18 and 100"
    }
  }
}
```

---

### **4. Pattern Matching (`pattern`)**

The `pattern` rule allows you to use regular expressions to validate string fields. This can be useful for validating formats like emails, phone numbers, or other structured data.

#### **Example:**

```javascript
{
  bsonType: "object",
  properties: {
    email: {
      bsonType: "string",
      pattern: "^[\\w-\\.]+@([\\w-]+\\.)+[\\w-]{2,4}$",  // Email format validation
      description: "Email must be in a valid format"
    },
    phoneNumber: {
      bsonType: "string",
      pattern: "^\\+?[0-9]{1,4}?[-.\\s]?[0-9]{1,4}[-.\\s]?[0-9]{1,4}[-.\\s]?[0-9]{1,9}$",  // Phone number validation
      description: "Phone number must be in a valid format"
    }
  }
}
```

---

### **5. Array Field Validation**

If a field is an array, MongoDB allows you to specify additional validation rules for the elements within the array. You can define the type of items in the array, and set constraints such as `minItems`, `maxItems`, and others.

#### **Example:**

```javascript
{
  bsonType: "object",
  properties: {
    tags: {
      bsonType: "array",
      items: {
        bsonType: "string",  // Each item in 'tags' array must be a string
        description: "Each tag must be a string"
      },
      minItems: 1,  // Array must contain at least one item
      maxItems: 5,  // Array can contain at most 5 items
      description: "Tags must be an array with 1 to 5 string items"
    }
  }
}
```

---

### **6. Subdocument Field Validation**

For embedded documents (subdocuments), you can validate the structure and content of the fields within those documents. This is done by using the `properties` and `required` keywords inside the schema for the subdocument.

#### **Example:**

```javascript
{
  bsonType: "object",
  properties: {
    address: {
      bsonType: "object",
      required: ["street", "city", "zipcode"],  // Fields inside the subdocument must be present
      properties: {
        street: {
          bsonType: "string",  // 'street' field must be a string
          description: "Street must be a string"
        },
        city: {
          bsonType: "string",  // 'city' field must be a string
          description: "City must be a string"
        },
        zipcode: {
          bsonType: "string",  // 'zipcode' field must be a string
          pattern: "^[0-9]{5}$",  // Zipcode must match the 5-digit pattern
          description: "Zipcode must be a 5-digit number"
        }
      }
    }
  }
}
```

---

### **7. Range or Limit Validation (`minimum`, `maximum`, `exclusiveMinimum`, `exclusiveMaximum`)**

MongoDB allows validation of numeric fields with range constraints. These constraints ensure that a field's value is within a specific range or satisfies an exclusive range condition.

#### **Example:**

```javascript
{
  bsonType: "object",
  properties: {
    price: {
      bsonType: "double",
      minimum: 0,  // Price must be at least 0
      maximum: 10000,  // Price must not exceed 10,000
      description: "Price must be between 0 and 10,000"
    },
    quantity: {
      bsonType: "int",
      exclusiveMinimum: 0,  // Quantity must be strictly greater than 0
      description: "Quantity must be greater than 0"
    }
  }
}
```

---

### **Using Field-level Validation in MongoDB**

To use field-level validation in MongoDB, you typically define the validation rules when creating or modifying a collection. Here’s how you can apply field-level validation:

#### **Example:**

```javascript
db.createCollection("products", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["name", "price", "quantity"],
      properties: {
        name: {
          bsonType: "string",
          description: "Product name must be a string"
        },
        price: {
          bsonType: "double",
          minimum: 0,
          description: "Product price must be a positive number"
        },
        quantity: {
          bsonType: "int",
          minimum: 1,
          description: "Quantity must be at least 1"
        }
      }
    }
  }
});
```

---

### **Conclusion**

Field-level validation in MongoDB provides fine-grained control over the data structure and ensures that each field in a document follows the desired constraints. Using **JSON Schema validation**, you can define rules for field types, required fields, value constraints, pattern matching, and more. This makes MongoDB a powerful tool for managing data integrity at the field level, helping prevent invalid data from being inserted or updated in the database.