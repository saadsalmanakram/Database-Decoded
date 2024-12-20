### **MongoDB Transactions**

MongoDB transactions provide a way to ensure atomicity, consistency, isolation, and durability (ACID properties) across multiple documents and operations. Transactions allow MongoDB to support complex workflows where multiple updates across collections or documents must either all succeed or all fail. This is particularly important when working with distributed systems or applications that require data consistency and reliability.

---

### **1. Introduction to ACID Properties in MongoDB**

ACID (Atomicity, Consistency, Isolation, Durability) properties ensure reliable processing of database transactions. MongoDB supports ACID transactions starting from version 4.0. Here’s what each property means in the context of MongoDB:

- **Atomicity**: Ensures that a transaction is treated as a single unit, meaning all operations in the transaction are executed successfully or none of them are applied (rollback).
  
- **Consistency**: Ensures that a transaction moves the database from one valid state to another. After the transaction is committed, the database is in a valid state according to the rules (schemas, constraints, etc.).
  
- **Isolation**: Ensures that transactions are isolated from each other. No transaction can see the intermediate results of other concurrent transactions. MongoDB supports different levels of isolation for transactions.
  
- **Durability**: Ensures that once a transaction has been committed, its changes are permanent, even if the system crashes.

MongoDB's support for ACID transactions applies to operations that span one or more documents, even across multiple collections, and ensures the guarantees of atomicity, consistency, isolation, and durability.

---

### **2. Multi-Document Transactions**

MongoDB supports **multi-document transactions**, which allow multiple documents to be modified atomically within a single transaction. Prior to MongoDB 4.0, transactions could only span a single document. With multi-document transactions, you can ensure that updates to multiple documents across different collections are performed as a single, atomic operation.

#### **Example: Multi-Document Transaction**

```javascript
const session = client.startSession();

try {
  session.startTransaction();

  // Update account balance in the "accounts" collection
  db.accounts.updateOne(
    { _id: "account1" },
    { $inc: { balance: -100 } },
    { session }
  );

  // Update transaction record in the "transactions" collection
  db.transactions.insertOne(
    { accountId: "account1", amount: -100, date: new Date() },
    { session }
  );

  // Commit the transaction if all operations succeed
  session.commitTransaction();
} catch (error) {
  // Abort the transaction if an error occurs
  session.abortTransaction();
  console.error("Transaction failed", error);
} finally {
  // End the session
  session.endSession();
}
```

**Use Case Explanation**:
- **Scenario**: Transferring money between accounts.
- **Operations**: Updates the account balance and inserts a transaction record.
- **Atomicity**: If either operation fails, none of the operations are applied, maintaining data consistency.
- **Commit/Rollback**: The transaction is committed if both operations succeed; otherwise, the transaction is rolled back.

---

### **3. Handling Transactions with Session API**

To use transactions in MongoDB, you must interact with the **session API**. A session represents a unit of work that can be used for transactions. You create a session using `startSession()` and pass it to the operations that should be part of the transaction. The session allows you to manage and control the transaction (begin, commit, abort) programmatically.

#### **Basic Steps for Handling Transactions with Session API**:
- **Start a session** using `client.startSession()`.
- **Start a transaction** with `session.startTransaction()`.
- **Perform operations** as part of the transaction, ensuring each operation uses the session.
- **Commit the transaction** with `session.commitTransaction()`.
- **Abort the transaction** with `session.abortTransaction()` if an error occurs.
- **End the session** with `session.endSession()`.

#### **Example: Session API with Rollback**

```javascript
const session = client.startSession();

try {
  session.startTransaction();
  
  // Perform some operations
  db.orders.updateOne({ _id: 1 }, { $set: { status: "completed" } }, { session });
  
  // Simulate a failure to trigger rollback
  throw new Error("Simulated Error");
  
  session.commitTransaction();
} catch (error) {
  console.log("Transaction failed. Rolling back.");
  session.abortTransaction();
} finally {
  session.endSession();
}
```

**Explanation**:
- **Rollback**: If an error is thrown, the transaction is rolled back using `session.abortTransaction()`.
- **Atomic Operations**: The transaction ensures that either all operations succeed, or none are applied.

---

### **4. Transaction Isolation Levels and Rollback**

MongoDB supports **different transaction isolation levels**, allowing you to control the visibility of uncommitted data in concurrent transactions. The isolation level determines how the operations within a transaction are isolated from other transactions and whether or not other transactions can see intermediate changes made by the current transaction.

#### **Isolation Levels in MongoDB**:

1. **Read Uncommitted**:
   - Allows a transaction to read uncommitted changes made by other transactions. This can result in "dirty reads" but increases concurrency.
   - MongoDB does not directly support "Read Uncommitted" isolation level; it defaults to a higher level of isolation.

2. **Read Committed** (Default):
   - Ensures that a transaction only reads data that has been committed. The transaction cannot see uncommitted changes from other transactions.
   - MongoDB’s default isolation level for multi-document transactions is **Read Committed**.

3. **Repeatable Read**:
   - Ensures that once a transaction reads a value, it will see the same value if it reads it again during the transaction, even if other transactions modify that value.
   - MongoDB does not currently support this isolation level directly, but it is the closest to its behavior when the transaction is running.

4. **Serializable**:
   - Ensures the strictest isolation level. It prevents transactions from reading data that another transaction might concurrently modify, effectively making transactions execute serially.
   - MongoDB does not support this isolation level in the same way traditional RDBMS do but guarantees serializable behavior for certain operations like `findAndModify()`.

#### **Rollback in Transactions**
- If any operation within a transaction fails or an error is caught, MongoDB will automatically **rollback the transaction** using `session.abortTransaction()`.
- Rollback ensures that partial or invalid updates do not corrupt the data, maintaining the integrity of the database.

#### **Example: Using Read Committed Isolation and Rollback**

```javascript
const session = client.startSession();

try {
  session.startTransaction();

  // Simulating an update operation that reads the current value
  const user = db.users.findOne({ _id: "user123" }, { session });

  // Simulating another operation that depends on the previous read value
  if (user.balance < 100) {
    db.users.updateOne({ _id: "user123" }, { $set: { status: "Inactive" } }, { session });
  }

  session.commitTransaction();
} catch (error) {
  session.abortTransaction();
  console.error("Transaction failed, rolled back.");
} finally {
  session.endSession();
}
```

**Explanation**:
- **Read Committed** isolation is used by default to ensure that the transaction only sees committed data.
- **Rollback**: If any error occurs, the transaction is aborted using `session.abortTransaction()`, ensuring no partial updates are applied.

---

### **Conclusion**

MongoDB transactions provide a robust mechanism for maintaining data consistency and integrity, particularly in use cases where multiple documents need to be updated atomically. By leveraging the **session API**, you can create multi-document transactions and ensure ACID compliance across your application. Understanding the available **transaction isolation levels** and how to handle **rollback** scenarios ensures that you can control the visibility and consistency of data in concurrent workloads. MongoDB’s transaction support allows for reliable and complex workflows, making it suitable for high-stakes applications like banking, e-commerce, and healthcare.