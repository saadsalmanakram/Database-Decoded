
---

# 🐚 **MongoDB Shell (mongosh)**

The **MongoDB Shell** (`mongosh`) is an interactive command-line interface for MongoDB. It allows developers to connect to MongoDB instances, perform CRUD operations, manage databases, and execute administrative tasks using JavaScript syntax.

---

## 📦 **Table of Contents**

1. [What is MongoDB Shell?](#what-is-mongodb-shell)
2. [Key Features](#key-features)
3. [Installing MongoDB Shell](#installing-mongodb-shell)
4. [Connecting to MongoDB](#connecting-to-mongodb)
5. [Basic Commands](#basic-commands)
6. [Executing CRUD Operations](#executing-crud-operations)
7. [Administrative Commands](#administrative-commands)
8. [Shell Scripting](#shell-scripting)
9. [Differences Between `mongosh` and `mongo`](#differences-between-mongosh-and-mongo)
10. [Helpful Tips](#helpful-tips)

---

## 🌐 **What is MongoDB Shell?**

**MongoDB Shell** (`mongosh`) is the official CLI (Command-Line Interface) for interacting with MongoDB. It supports running JavaScript-based commands and scripts to interact with MongoDB databases.

### Key Uses:

- Perform CRUD (Create, Read, Update, Delete) operations.
- Manage MongoDB databases and collections.
- Execute administrative tasks such as indexing and backups.
- Run JavaScript-based scripts for database automation.

---

## 🚀 **Key Features**

- **Interactive Shell**: Run commands interactively.
- **JavaScript Syntax**: Use JavaScript for database operations.
- **History and Autocompletion**: Supports command history and tab-based autocompletion.
- **Integration with Modern MongoDB Drivers**: Works with MongoDB 4.4 and newer versions.
- **Rich Output**: Improved output formatting for better readability.
- **Scripting Support**: Automate tasks with scripts.

---

## 🛠️ **Installing MongoDB Shell**

### 📥 **Download and Install**

You can download the latest version of `mongosh` from the official MongoDB website:

- **Official Download Page**: [Download MongoDB Shell](https://www.mongodb.com/try/download/shell)

### **Installation on Different Platforms**

1. **Windows**:
   - Download the `.msi` installer.
   - Run the installer and follow the prompts.
   - Add `mongosh` to your system’s PATH.

2. **macOS**:
   - Using Homebrew:
     ```bash
     brew install mongosh
     ```

3. **Linux**:
   - Using `apt` (Debian/Ubuntu):
     ```bash
     wget https://downloads.mongodb.com/compass/mongosh-<version>-linux.tgz
     tar -xvzf mongosh-<version>-linux.tgz
     sudo mv mongosh /usr/local/bin
     ```

4. **Check Installation**:
   ```bash
   mongosh --version
   ```

---

## 🔗 **Connecting to MongoDB**

To connect to a MongoDB instance using `mongosh`, use the following command:

```bash
mongosh "mongodb://localhost:27017"
```

### Connecting to MongoDB Atlas

```bash
mongosh "mongodb+srv://<username>:<password>@cluster0.mongodb.net/?retryWrites=true&w=majority"
```

---

## 📋 **Basic Commands**

### 1. **List Databases**

```javascript
show dbs
```

### 2. **Switch Database**

```javascript
use <databaseName>
```

### 3. **List Collections**

```javascript
show collections
```

### 4. **Insert a Document**

```javascript
db.collectionName.insertOne({ name: "John", age: 30 })
```

### 5. **Find Documents**

```javascript
db.collectionName.find()
```

### 6. **Delete a Collection**

```javascript
db.collectionName.drop()
```

---

## 📝 **Executing CRUD Operations**

### **Create**

```javascript
db.users.insertOne({ name: "Alice", age: 25 })
db.users.insertMany([{ name: "Bob" }, { name: "Charlie" }])
```

### **Read**

```javascript
db.users.find()
db.users.find({ age: { $gte: 25 } })
```

### **Update**

```javascript
db.users.updateOne({ name: "Alice" }, { $set: { age: 26 } })
db.users.updateMany({}, { $set: { status: "active" } })
```

### **Delete**

```javascript
db.users.deleteOne({ name: "Bob" })
db.users.deleteMany({ age: { $lt: 20 } })
```

---

## 🛠️ **Administrative Commands**

### **Check Database Stats**

```javascript
db.stats()
```

### **Create an Index**

```javascript
db.users.createIndex({ name: 1 })
```

### **List Indexes**

```javascript
db.users.getIndexes()
```

### **Server Status**

```javascript
db.adminCommand({ serverStatus: 1 })
```

---

## 📜 **Shell Scripting**

You can write scripts and execute them using `mongosh`.

### **Example Script (script.js)**

```javascript
// script.js
use testDB
db.users.insertOne({ name: "Dave", age: 28 })
print("User added successfully")
```

### **Execute the Script**

```bash
mongosh < script.js
```

---

## 🔄 **Differences Between `mongosh` and `mongo`**

| Feature                         | `mongosh` (Modern)       | `mongo` (Legacy)       |
|---------------------------------|--------------------------|------------------------|
| **Language**                    | JavaScript ES6           | JavaScript (Legacy)    |
| **Interactive Features**        | Autocomplete, History   | Limited                |
| **Compatibility**               | MongoDB 4.4+            | MongoDB ≤ 4.2          |
| **Extensibility**               | Modern Plugins          | Limited                |

**Note**: `mongo` is deprecated in favor of `mongosh`.

---

## 💡 **Helpful Tips**

1. **Autocompletion**:
   - Use the `Tab` key for command and collection autocompletion.

2. **History**:
   - Use the **Up/Down** arrows to cycle through command history.

3. **Exit the Shell**:
   ```bash
   exit
   ```

4. **Help Command**:
   ```javascript
   help
   ```

---

## 🔗 **Additional Resources**

- [MongoDB Shell Documentation](https://www.mongodb.com/docs/mongodb-shell/)
- [MongoDB Shell GitHub Repo](https://github.com/mongodb-js/mongosh)
- [MongoDB University](https://university.mongodb.com/)

---

**Happy Scripting with MongoDB Shell! 🐚🚀**