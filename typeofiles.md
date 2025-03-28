Here's a list of the **different types of files in PostgreSQL** along with their **locations** and **usage**. These files are essential for data storage, configuration, logging, and transaction handling.

---

## 📁 PostgreSQL File Types & Their Usage

### 1. **Data Files (Base Directory)**
📌 **Location**: `$PGDATA/base/`

- Contains actual table and index data in binary format.
- Each table or index has its own file, named by its `relfilenode` (OID).

📘 **Usage**:
Stores the persistent data. For example, when you create a table, PostgreSQL creates a corresponding file here.

---

### 2. **WAL (Write-Ahead Log) Files**
📌 **Location**: `$PGDATA/pg_wal/` (PostgreSQL 10+)

- Files that store **transaction logs** before they're applied to data files.
- File format: 16MB binary segments (e.g., `00000001000000000000000A`).

📘 **Usage**:
Ensures durability and crash recovery. WAL is also used in streaming replication.

---

### 3. **Configuration Files**
📌 **Location**: `$PGDATA/`

| File | Purpose |
|------|---------|
| `postgresql.conf` | Main config file for database settings (e.g., memory, WAL, ports). |
| `pg_hba.conf` | Controls client authentication (who can connect, from where, using what). |
| `pg_ident.conf` | Maps OS usernames to PostgreSQL usernames. |

📘 **Usage**:
Used for tuning performance, security, and database behavior.

---

### 4. **Control File**
📌 **Location**: `$PGDATA/global/pg_control`

- Binary file that tracks critical metadata: checkpoint location, timeline ID, system identifier, etc.

📘 **Usage**:
Vital for crash recovery. Corruption here can prevent startup.

---

### 5. **Commit Log / Transaction Log (pg_xact)**
📌 **Location**: `$PGDATA/pg_xact/`

- Stores commit status (commit, abort) for each transaction.
- Previously called `pg_clog` (before v10).

📘 **Usage**:
Ensures atomicity of transactions (part of ACID compliance).

---

### 6. **Multixact Files**
📌 **Location**: `$PGDATA/pg_multixact/`

- Tracks shared locks for multiple transactions on the same row.

📘 **Usage**:
Used in cases like shared row locking (`FOR SHARE`, `FOR UPDATE` with multiple transactions).

---

### 7. **Temporary Files**
📌 **Location**: Usually inside `$PGDATA/base/pgsql_tmp/` or system’s temp directory.

📘 **Usage**:
Created for large sorts, hash joins, or intermediate query results when memory is insufficient.

---

### 8. **Log Files**
📌 **Location**: Configurable using `log_directory` in `postgresql.conf`.

📘 **Usage**:
Stores database logs, error messages, and access logs (if enabled).

---

### 9. **Replication Slots & Stats**
| Folder | Purpose |
|--------|---------|
| `$PGDATA/pg_replslot/` | Info about logical/physical replication slots |
| `$PGDATA/pg_stat/` | Persistent statistics (for system views) |

---

### 10. **Tablespace Directories**
📌 **Location**: `$PGDATA/pg_tblspc/`

- Contains symbolic links to external storage locations used for user-defined tablespaces.

📘 **Usage**:
Used to place certain tables or indexes on different disks for performance or space management.

---

### 11. **PID File**
📌 **Location**: `$PGDATA/postmaster.pid`

📘 **Usage**:
Contains the PID of the main PostgreSQL process. Prevents multiple instances from running on the same data directory.

---

### 12. **Backup Label File**
📌 **File**: `backup_label`

📘 **Usage**:
Created during `pg_basebackup` or `pg_start_backup()`. Helps in recovery by marking backup start point.

---

## 🧠 Bonus Tip: Use `pg_ls_dir()` and `pg_stat_file()` to inspect file info directly in SQL!

---

