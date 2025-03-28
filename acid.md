

## 🔐 What is ACID?

ACID stands for:

| Letter | Stands For | Meaning |
|--------|-------------|---------|
| A | **Atomicity** | All operations in a transaction succeed or none do |
| C | **Consistency** | The database moves from one valid state to another |
| I | **Isolation** | Concurrent transactions don't interfere with each other |
| D | **Durability** | Once committed, changes are saved permanently—even after a crash |

Let’s see how **PostgreSQL** implements each of them:

---

## ✅ A – Atomicity

**In PostgreSQL:**
- PostgreSQL uses **transactions** (`BEGIN`, `COMMIT`, `ROLLBACK`) to group SQL statements.
- If any query fails, you can `ROLLBACK` and **undo all previous changes** in that transaction.

### Example:
```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

➡ If any `UPDATE` fails, `ROLLBACK` ensures **no partial changes**.

---

## ✅ C – Consistency

**In PostgreSQL:**
- Constraints like `FOREIGN KEY`, `CHECK`, `NOT NULL`, `UNIQUE`, and `TRIGGERS` ensure that data remains valid.
- PostgreSQL guarantees that **all rules and constraints** are satisfied **before a transaction commits**.

---

## ✅ I – Isolation

**In PostgreSQL:**
- PostgreSQL supports **MVCC (Multi-Version Concurrency Control)** to give each transaction a consistent view of the data.
- It supports multiple isolation levels:
  - `READ COMMITTED` (default)
  - `REPEATABLE READ`
  - `SERIALIZABLE` (strongest)

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

➡ Prevents dirty reads, non-repeatable reads, and phantom reads based on the level.

---

## ✅ D – Durability

**In PostgreSQL:**
- Uses **WAL (Write-Ahead Logging)**.
- Before committing a transaction, it writes all changes to the WAL log.
- Even if PostgreSQL crashes, the WAL is replayed to **recover all committed data**.

```conf
# Controlled by
fsync = on
synchronous_commit = on
```

---

## 🧠 Real-World Example

### Bank Transfer:
```sql
BEGIN;
-- Withdraw from A
UPDATE accounts SET balance = balance - 500 WHERE id = 1;

-- Deposit to B
UPDATE accounts SET balance = balance + 500 WHERE id = 2;

-- Commit
COMMIT;
```

PostgreSQL ensures:
- ✅ **Atomic**: Both updates succeed or none.
- ✅ **Consistent**: Total money remains the same.
- ✅ **Isolated**: No one else sees partial changes.
- ✅ **Durable**: Even power loss won’t lose committed changes.

---

