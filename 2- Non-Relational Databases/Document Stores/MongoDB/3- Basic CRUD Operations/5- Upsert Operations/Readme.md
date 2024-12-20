
---

# 📝 **Upsert Operations in MongoDB**

Upsert operations in MongoDB are a powerful feature that allows you to either update an existing document or insert a new document if one does not already exist. This is achieved by using the **`upsert`** option in the update methods.

### **Key Upsert Methods:**
1. **`updateOne()`** – Updates a single document if it exists, or inserts a new document if no document matches the query.
2. **`updateMany()`** – Updates multiple documents if they exist, or inserts new documents if no documents match the query (not common, as this is usually only for updates).
3. **`findOneAndUpdate()`** – Finds a document and updates it, returning the updated document. If no document is found, it will insert a new document.

---

## 📚 **Table of Contents**

1. [What is an Upsert?](#what-is-an-upsert)
2. [Syntax for Upsert Operations](#syntax-for-upsert-operations)
3. [Using `updateOne()` with Upsert](#using-updateone-with-upsert)
4. [Using `updateMany()` with Upsert](#using-updatemany-with-upsert)
5. [Using `findOneAndUpdate()` with Upsert](#using-findoneandupdate-with-upsert)
6. [Error Handling in Upsert Operations](#error-handling-in-upsert-operations)

---

## 🚀 **What is an Upsert?**

The term **"Upsert"** is a combination of "Update" and "Insert". It allows you to:
- **Update** an existing document if it matches the provided filter.
- **Insert** a new document if no document matches the filter.

This operation is useful when you are unsure whether the document already exists and want to avoid checking the existence manually.

---

## 📝 **Syntax for Upsert Operations**

All the update methods in MongoDB support the `upsert` option. Here’s how the syntax works for the different methods:

### `updateOne()` Syntax

```javascript
db.collection.updateOne(
  filter,    // The filter to find the document
  update,    // The update operation
  { upsert: true }  // Option to insert if no document is found
);
```

### `updateMany()` Syntax

```javascript
db.collection.updateMany(
  filter,    // The filter to find the documents
  update,    // The update operation
  { upsert: true }  // Option to insert if no documents are found
);
```

### `findOneAndUpdate()` Syntax

```javascript
db.collection.findOneAndUpdate(
  filter,    // The filter to find the document
  update,    // The update operation
  { upsert: true }  // Option to insert if no document is found
);
```

---

## 🛠️ **Using `updateOne()` with Upsert**

The `updateOne()` method allows you to update a single document, or if no document matches the filter, insert a new document.

### **Example**

```javascript
db.users.updateOne(
  { username: "alice" },   // Find user with username "alice"
  { $set: { age: 25 } },   // Set age to 25
  { upsert: true }         // Insert if no such user exists
);
```

**Explanation:**
- If a user with `username: "alice"` exists, their age will be updated to 25.
- If no such user exists, a new user document with `username: "alice"` and `age: 25` will be inserted.

---

## 🧑‍🤝‍🧑 **Using `updateMany()` with Upsert**

The `updateMany()` method is similar to `updateOne()`, but it updates multiple documents that match the filter.

### **Example**

```javascript
db.users.updateMany(
  { age: { $gt: 30 } },  // Find users older than 30
  { $set: { status: "senior" } },  // Set their status to "senior"
  { upsert: true }  // Insert if no such users exist
);
```

**Explanation:**
- All users older than 30 will have their `status` set to "senior".
- If no such users exist, a new document with the filter `{ age: { $gt: 30 } }` and `status: "senior"` will be inserted.

---

## 🔄 **Using `findOneAndUpdate()` with Upsert**

The `findOneAndUpdate()` method finds a single document, updates it, and returns the updated document. If no document matches the filter, a new document will be inserted.

### **Example**

```javascript
db.users.findOneAndUpdate(
  { username: "bob" },   // Find user with username "bob"
  { $set: { age: 28 } },  // Set age to 28
  { upsert: true, returnDocument: 'after' }  // Insert if no such user exists and return the updated document
);
```

**Explanation:**
- If a user with `username: "bob"` exists, their age will be updated to 28 and the updated document will be returned.
- If no such user exists, a new document with `username: "bob"` and `age: 28` will be inserted.

---

## ⚠️ **Error Handling in Upsert Operations**

Like other update operations, upsert operations may fail due to various reasons, such as invalid queries or data types. It is crucial to handle errors to ensure your application continues to run smoothly.

### **Example of Error Handling in Node.js**

```javascript
try {
  const result = await db.users.updateOne(
    { username: "alice" },    // Filter to find the document
    { $set: { age: 25 } },    // Update operation
    { upsert: true }          // Upsert option
  );
  console.log("Upsert successful", result);
} catch (err) {
  console.error("Error occurred during upsert operation:", err);
}
```

---

## 📚 **Summary**

| Method                      | Description                                                                 | Example                                                       |
|-----------------------------|-----------------------------------------------------------------------------|---------------------------------------------------------------|
| `updateOne()`               | Updates a single document or inserts a new one if no match is found         | `db.users.updateOne({ username: "alice" }, { $set: { age: 25 } }, { upsert: true })` |
| `updateMany()`              | Updates multiple documents or inserts new ones if no matches are found      | `db.users.updateMany({ age: { $gt: 30 } }, { $set: { status: "senior" } }, { upsert: true })` |
| `findOneAndUpdate()`        | Finds and updates a single document or inserts a new one if no match is found | `db.users.findOneAndUpdate({ username: "bob" }, { $set: { age: 28 } }, { upsert: true })` |

---

## 📖 **Additional Resources**

- [MongoDB Documentation: Upsert](https://www.mongodb.com/docs/manual/reference/method/db.collection.update/#upsert)
- [MongoDB University Free Courses](https://university.mongodb.com/)

---

**Happy Coding with MongoDB! 🚀**