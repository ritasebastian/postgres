

## 🔁 **Throughput vs. Latency** – What's the Difference?

| Concept     | Meaning | Analogy |
|-------------|---------|---------|
| **Latency** | Time taken to complete **a single request** | Time for **one car** to travel from A to B |
| **Throughput** | Number of **requests processed per second/minute** | Number of **cars** passing through a toll booth per minute |

---

## 💡 In Database Terms

| Metric      | Example |
|-------------|---------|
| **Latency** | Time taken for one `SELECT` or `INSERT` to complete |
| **Throughput** | Number of queries/transactions handled per second |

---

## 🎯 How to Measure & Improve Them in a Database (e.g., PostgreSQL)

---

### ✅ 1. **Measure Latency**

#### 🔍 Tools:
- PostgreSQL: `pg_stat_statements`, `EXPLAIN ANALYZE`
- Linux: `pgbench`, `iostat`, `strace`
- Cloud: RDS Performance Insights / CloudWatch

#### 🔧 Sample Check:
```sql
EXPLAIN ANALYZE SELECT * FROM orders WHERE customer_id = 101;
```

➡ Shows how long the query takes — **latency per query**

---

### ✅ 2. **Measure Throughput**

#### 🔍 Tools:
- `pg_stat_database`: Check `xact_commit`, `xact_rollback`
- `pgbench`: Benchmark test
- Prometheus + Grafana for real-time dashboards

#### 📊 Query:
```sql
SELECT datname,
       xact_commit + xact_rollback AS total_txns,
       blks_read + blks_hit AS total_blocks
FROM pg_stat_database;
```

➡ See how many transactions PostgreSQL is processing.

---

## 🔧 How to Improve Latency

1. **Indexing**: Add proper indexes to speed up reads
2. **Connection pooling**: Use PgBouncer to reduce connection overhead
3. **Caching**: Use Redis or PostgreSQL buffer cache
4. **Vacuum regularly**: Avoid table bloat and dead tuples
5. **Optimize queries**: Use `JOIN` efficiently, avoid large nested subqueries
6. **Faster disks (SSD/NVMe)**

---

## 🔧 How to Improve Throughput

1. **Connection Pooling**:
   - Reduces overhead of opening/closing DB connections
   - Tools: **PgBouncer**, **Pgpool-II**

2. **Parallelism**:
   - Enable parallel queries in PostgreSQL (`max_parallel_workers`)

3. **Tune WAL & Checkpoints**:
   ```conf
   wal_writer_delay = 10ms
   checkpoint_completion_target = 0.9
   ```

4. **Reduce locking/contention**:
   - Use `NOWAIT` or `SKIP LOCKED`
   - Normalize and partition tables

5. **Batch Inserts/Updates**:
   - Insert 1000 rows at once instead of one row at a time

6. **Scale reads using replicas**:
   - Setup **read replicas** to distribute read load (logical/physical replication)

---

## 🧪 Example: Benchmark with `pgbench`

```bash
pgbench -U postgres -h localhost -c 10 -j 4 -T 60 mydb
```

- `-c`: clients
- `-j`: threads
- `-T`: time (seconds)

📊 Output shows **latency per transaction** and **TPS (throughput)**.

---

## 🎯 Summary Table

| Metric     | Focuses on | Improve By |
|------------|------------|------------|
| Latency    | Single query speed | Indexes, cache, optimized queries |
| Throughput | Total queries/sec   | Connection pooling, parallelism, replicas |

---

