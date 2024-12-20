
---

# 📝 **Create Operations in MongoDB**

MongoDB provides methods for inserting documents into collections, making it easy to add data to your database. The primary methods for creating documents are:

1. **`insertOne()`** – Inserts a single document.
2. **`insertMany()`** – Inserts multiple documents.

Let's explore these methods in detail, including syntax, examples, and important considerations.

---

## 📚 **Table of Contents**

1. [Introduction to Create Operations](#introduction-to-create-operations)
2. [Inserting a Single Document (`insertOne`)](#inserting-a-single-document-insertone)
3. [Inserting Multiple Documents (`insertMany`)](#inserting-multiple-documents-insertmany)
4. [Auto-Generated `_id` Field](#auto-generated-_id-field)
5. [Error Handling for Insert Operations](#error-handling-for-insert-operations)

---

## 🚀 **Introduction to Create Operations**

MongoDB's create operations allow you to add new documents to a collection. Each document is stored in a flexible, JSON-like format called **BSON (Binary JSON)**. Unlike relational databases, MongoDB does not require a predefined schema, offering flexibility for storing different structures within the same collection.

---

## 📝 **Inserting a Single Document (`insertOne`)**

The `insertOne()` method inserts a single document into a collection.

### **Syntax**

```javascript
db.collection.insertOne(document);
```

- **`document`**: A JSON-like object representing the data to be inserted.

### **Example**

Insert a new user document into the `users` collection:

```javascript
db.users.insertOne({
  name: "Alice",
  age: 28,
  email: "alice@example.com"
});
```

**Output:**

```json
{
  "acknowledged": true,
  "insertedId": ObjectId("60f7a3d2c9e77f1b3d5b1234")
}
```

### **Explanation**

- The operation returns an object with:
  - **`acknowledged`**: Indicates if the operation was successful.
  - **`insertedId`**: The unique identifier (ObjectId) assigned to the new document.

---

## 📦 **Inserting Multiple Documents (`insertMany`)**

The `insertMany()` method inserts multiple documents into a collection at once.

### **Syntax**

```javascript
db.collection.insertMany([document1, document2, ...]);
```

- **`[document1, document2, ...]`**: An array of JSON-like objects to be inserted.

### **Example**

Insert multiple user documents into the `users` collection:

```javascript
db.users.insertMany([
  { name: "Bob", age: 35, email: "bob@example.com" },
  { name: "Charlie", age: 42, email: "charlie@example.com" },
  { name: "Diana", age: 30, email: "diana@example.com" }
]);
```

**Output:**

```json
{
  "acknowledged": true,
  "insertedIds": {
    "0": ObjectId("60f7a3d2c9e77f1b3d5b1235"),
    "1": ObjectId("60f7a3d2c9e77f1b3d5b1236"),
    "2": ObjectId("60f7a3d2c9e77f1b3d5b1237")
  }
}
```

### **Explanation**

- The operation returns an object with:
  - **`acknowledged`**: Indicates if the operation was successful.
  - **`insertedIds`**: A list of unique identifiers assigned to each inserted document.

---

## 🔑 **Auto-Generated `_id` Field**

When inserting documents, MongoDB automatically generates an `_id` field if one is not provided.

### **Example**

```javascript
db.products.insertOne({ name: "Laptop", price: 1200 });
```

**Resulting Document:**

```json
{
  "_id": ObjectId("60f7a3d2c9e77f1b3d5b1238"),
  "name": "Laptop",
  "price": 1200
}
```

### **Custom `_id`**

You can provide a custom `_id` if desired:

```javascript
db.products.insertOne({
  _id: "prod-123",
  name: "Phone",
  price: 800
});
```

---

## ⚠️ **Error Handling for Insert Operations**

### **Duplicate `_id` Error**

If a document with a duplicate `_id` is inserted, MongoDB will throw an error:

```javascript
db.users.insertOne({ _id: 1, name: "Eve" });
db.users.insertOne({ _id: 1, name: "Frank" }); // Error: Duplicate key
```

### **Handling Errors in `insertMany`**

By default, `insertMany()` stops inserting on the first error. To continue inserting remaining documents despite errors, use the `ordered` option set to `false`:

```javascript
db.users.insertMany(
  [
    { _id: 1, name: "Grace" },
    { _id: 1, name: "Hank" }, // Duplicate `_id` error
    { _id: 2, name: "Ivy" }
  ],
  { ordered: false }
);
```

**Result:**

- Document with `_id: 2` will still be inserted despite the error with `_id: 1`.

---

## 📚 **Summary**

| Method        | Description                    | Example                                              |
|---------------|--------------------------------|-----------------------------------------------------|
| `insertOne()` | Inserts a single document      | `db.users.insertOne({ name: "Alice" })`            |
| `insertMany()`| Inserts multiple documents     | `db.users.insertMany([{ name: "Bob" }, { name: "Charlie" }])` |

---

## 📖 **Additional Resources**

- [MongoDB Documentation: CRUD Operations](https://www.mongodb.com/docs/manual/crud/)
- [MongoDB University Free Courses](https://university.mongodb.com/)

---

**Happy Coding with MongoDB! 🚀**