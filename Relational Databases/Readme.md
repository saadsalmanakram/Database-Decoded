---

### **Relational Databases (RDBMS)**
A **relational database** organizes data into **tables (relations)** with predefined schemas. Each table consists of rows and columns, where each row represents a record (entry) and each column represents an attribute (field). These databases use **Structured Query Language (SQL)** to interact with and manipulate data.

#### **Key Features:**
1. **Schema-based Structure**: The schema defines the structure of data, including tables, columns, and relationships.
2. **Tables and Rows**: Data is stored in tables, with each row representing a record.
3. **Relationships**: Tables can be linked using **primary keys** (unique identifiers) and **foreign keys** (references to other tables).
4. **ACID Compliance**: Ensures transactions are:
   - **Atomic**: All or nothing execution.
   - **Consistent**: Database remains in a valid state.
   - **Isolated**: Transactions do not interfere with each other.
   - **Durable**: Data remains intact after a transaction.

#### **Examples of RDBMS:**
- **MySQL**
- **PostgreSQL**
- **Oracle Database**
- **Microsoft SQL Server**

#### **Advantages of Relational Databases:**
1. **Structured Data**: Ideal for applications requiring structured, well-defined relationships.
2. **Data Integrity**: Enforces data accuracy and consistency via constraints (primary keys, foreign keys, etc.).
3. **Complex Queries**: Supports advanced queries using SQL.
4. **Security**: Fine-grained permissions and role-based access control.
5. **Scalability**: Vertical scaling (adding more power to a single server).

#### **When to Use Relational Databases:**
- When data consistency and integrity are critical.
- For applications with complex relationships between data (e.g., financial systems, ERP systems).
- For systems that require robust querying capabilities.

---
