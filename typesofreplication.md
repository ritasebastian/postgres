

## 🔄 Types of Replication in PostgreSQL

PostgreSQL supports multiple types of replication to copy data from one server (primary) to another (standby/replica), usually for **high availability**, **read scaling**, or **disaster recovery**.

---

### 1. 🔁 **Physical Replication**

#### ✅ Description:
Copies the **binary-level data files** (WAL logs) from the primary to the standby.

#### 🔸 Methods:
- **Streaming Replication** (live WAL streaming)
- **Archive-based Replication** (via WAL archive)
- **Base Backups** (via `pg_basebackup`)

#### 🔸 Features:
- Standby is **read-only**
- Exact copy of the primary (same schema, data, etc.)

#### 🔸 Use Cases:
- High availability (HA)
- Disaster recovery (DR)
- Warm or hot standby setup

---

### 2. 🧠 **Logical Replication**

#### ✅ Description:
Copies **specific tables/data** using SQL-level replication (row-based), not the binary files.

#### 🔸 Introduced in PostgreSQL 10.

#### 🔸 Works via:
- **Publications** on primary
- **Subscriptions** on replica

#### Example:
```sql
-- On primary
CREATE PUBLICATION mypub FOR TABLE employees;

-- On subscriber
CREATE SUBSCRIPTION mysub CONNECTION 'host=...' PUBLICATION mypub;
```

#### 🔸 Features:
- Target can be a different PostgreSQL version.
- Supports selective replication (not whole DB).
- **Writable target** (you can modify non-replicated tables).

#### 🔸 Use Cases:
- Replicating only some tables
- Upgrades / migrations
- BI systems, reporting

---

### 3. 🔄 **Synchronous vs Asynchronous Replication**

✅ This is not a replication *type*, but a **mode** for physical streaming replication.

| Mode | Description |
|------|-------------|
| **Synchronous** | Transaction on primary waits for standby to confirm write. No data loss, but slower. |
| **Asynchronous** | Primary doesn’t wait. Faster, but there's a risk of data loss during failover. |

---

### 4. 🌐 **Cascading Replication**

#### ✅ Description:
A standby server acts as a source for another standby — like a chain.

#### 🔸 Use Case:
Reduces load on the primary when you have many replicas.

---

### 5. 📂 **Snapshot Replication** (Manual)

Not built-in, but you can implement:
- Periodic data dumps (`pg_dump`)
- CRON jobs + imports

✅ Good for:
- Non-critical backups
- Manual sync across environments

---

## 🧠 Summary Table

| Replication Type | Level | Readable? | Customizable? | Use Case |
|------------------|-------|-----------|----------------|----------|
| Physical | Binary | Yes (read-only) | No | HA, DR |
| Streaming | Binary (live) | Yes (read-only) | No | Real-time replication |
| Logical | Row-level | Yes (read/write) | Yes (choose tables) | Migrations, BI |
| Snapshot | Manual | Yes | Yes | Backups, staging copy |
| Cascading | Binary | Yes | No | Scaling read replicas |

---
