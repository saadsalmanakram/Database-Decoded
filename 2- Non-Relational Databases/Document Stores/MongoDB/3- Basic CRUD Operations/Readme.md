
---

# 📝 **Basic CRUD Operations in MongoDB**

CRUD operations are fundamental to interacting with MongoDB. CRUD stands for:

1. **Create** – Insert documents into a collection.
2. **Read** – Query and retrieve documents.
3. **Update** – Modify existing documents.
4. **Delete** – Remove documents from a collection.

MongoDB provides methods like `insertOne()`, `find()`, `updateMany()`, and `deleteOne()` to perform these operations effectively.

---

## 📚 **Table of Contents**

1. [Create: Insert Documents](#create-insert-documents)
2. [Read: Querying Documents](#read-querying-documents)
3. [Update: Modifying Documents](#update-modifying-documents)
4. [Delete: Removing Documents](#delete-removing-documents)
5. [Upsert Operations](#upsert-operations)
6. [Query Filters and Operators](#query-filters-and-operators)

---

## 🚀 **Create: Insert Documents**

MongoDB offers two methods for inserting documents:

- `insertOne()` – Inserts a single document.
- `insertMany()` – Inserts multiple documents.

### **`insertOne()` Example**

```javascript
db.users.insertOne({
  name: "Alice",
  age: 28,
  email: "alice@example.com"
});
```

### **`insertMany()` Example**

```javascript
db.users.insertMany([
  { name: "Bob", age: 35, email: "bob@example.com" },
  { name: "Charlie", age: 42, email: "charlie@example.com" }
]);
```

---

## 🔍 **Read: Querying Documents**

MongoDB provides methods for reading and querying documents:

- `find()` – Retrieves multiple documents based on a query.
- `findOne()` – Retrieves a single document that matches the query.

### **`find()` Example**

Retrieve all users aged 30 or older:

```javascript
db.users.find({ age: { $gte: 30 } });
```

### **`findOne()` Example**

Retrieve a single user with the name "Alice":

```javascript
db.users.findOne({ name: "Alice" });
```

### **Projection in Queries**

Limit the fields returned by the query:

```javascript
db.users.find({ age: { $gte: 30 } }, { name: 1, email: 1 });
```

---

## 🔧 **Update: Modifying Documents**

MongoDB provides methods to update documents:

- `updateOne()` – Updates a single document.
- `updateMany()` – Updates multiple documents.
- `replaceOne()` – Replaces an entire document.

### **`updateOne()` Example**

Update the email of the user named "Alice":

```javascript
db.users.updateOne(
  { name: "Alice" },
  { $set: { email: "alice.new@example.com" } }
);
```

### **`updateMany()` Example**

Increase the age of all users by 1 year:

```javascript
db.users.updateMany(
  {},
  { $inc: { age: 1 } }
);
```

### **`replaceOne()` Example**

Replace an entire document for the user named "Bob":

```javascript
db.users.replaceOne(
  { name: "Bob" },
  { name: "Robert", age: 36, email: "robert@example.com" }
);
```

---

## 🗑️ **Delete: Removing Documents**

MongoDB provides methods for deleting documents:

- `deleteOne()` – Deletes a single document.
- `deleteMany()` – Deletes multiple documents.

### **`deleteOne()` Example**

Delete a user named "Charlie":

```javascript
db.users.deleteOne({ name: "Charlie" });
```

### **`deleteMany()` Example**

Delete all users aged 50 or older:

```javascript
db.users.deleteMany({ age: { $gte: 50 } });
```

---

## 🔄 **Upsert Operations**

An **upsert** is an operation that updates a document if it exists or inserts it if it doesn't.

### **Upsert Example**

Update the email of a user named "Eve" or insert a new document if "Eve" doesn't exist:

```javascript
db.users.updateOne(
  { name: "Eve" },
  { $set: { email: "eve@example.com", age: 29 } },
  { upsert: true }
);
```

---

## 🔍 **Query Filters and Operators**

MongoDB supports a wide range of query operators for filtering documents. Some common operators include:

| Operator | Description                              | Example                                  |
|----------|------------------------------------------|------------------------------------------|
| `$eq`    | Matches values that are equal           | `{ age: { $eq: 25 } }`                  |
| `$gt`    | Matches values greater than             | `{ age: { $gt: 30 } }`                  |
| `$lt`    | Matches values less than                | `{ age: { $lt: 50 } }`                  |
| `$gte`   | Matches values greater than or equal    | `{ age: { $gte: 25 } }`                 |
| `$lte`   | Matches values less than or equal       | `{ age: { $lte: 60 } }`                 |
| `$ne`    | Matches values not equal to             | `{ age: { $ne: 30 } }`                  |
| `$in`    | Matches any value in an array           | `{ name: { $in: ["Alice", "Bob"] } }`   |
| `$nin`   | Matches values not in an array          | `{ name: { $nin: ["Charlie", "Eve"] } }`|
| `$and`   | Combines multiple conditions            | `{ $and: [{ age: { $gt: 20 } }, { age: { $lt: 30 } }] }` |
| `$or`    | Matches if any condition is true        | `{ $or: [{ name: "Alice" }, { age: 35 }] }` |

### **Example Query Using Multiple Operators**

Find all users aged between 25 and 40 whose names are either "Alice" or "Bob":

```javascript
db.users.find({
  $and: [
    { age: { $gte: 25, $lte: 40 } },
    { name: { $in: ["Alice", "Bob"] } }
  ]
});
```

---

## 📚 **Additional Resources**

- [MongoDB CRUD Operations Documentation](https://www.mongodb.com/docs/manual/crud/)
- [MongoDB Query Operators](https://www.mongodb.com/docs/manual/reference/operator/query/)
- [MongoDB University Free Courses](https://university.mongodb.com/)

---

**Happy Coding with MongoDB! 🚀**