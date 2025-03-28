Awesome! You're now diving into **how PostgreSQL executes joins internally** — like **Hash Join**, **Merge Join**, and **Nested Loop Join**. These are chosen by the **query planner** based on data size, indexes, and costs.

Let’s break it down 👇

---

## 🔍 What Are Join Algorithms in PostgreSQL?

When you write a SQL JOIN, PostgreSQL doesn't just "do it" — it **analyzes the best strategy** to combine the tables. These strategies are called **join algorithms**.

---

## ⚙️ 1. **Nested Loop Join**

🧠 **How it works**:
- For each row in the **outer table**, it checks every row in the **inner table**.

#### 📘 Best When:
- One table is **very small**
- There are **indexes** on the join condition

#### 🧪 Example:
```sql
EXPLAIN ANALYZE
SELECT * FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;
```

If few customers match, PostgreSQL may use:
```
Nested Loop
  -> Seq Scan on customers
  -> Index Scan on orders using orders_customer_id_idx
```

✅ **Fast for small datasets**, but **slow for large** ones.

---

## ⚙️ 2. **Hash Join**

🧠 **How it works**:
- PostgreSQL builds a **hash table** in memory from the smaller table
- Scans the larger table and **uses the hash to find matches**

#### 📘 Best When:
- **No index**
- Medium/large tables that can fit hash in memory

#### 🧪 Example:
```sql
EXPLAIN ANALYZE
SELECT * FROM orders o
JOIN products p ON o.product_id = p.product_id;
```

Planner output:
```
Hash Join
  -> Seq Scan on orders
  -> Hash
     -> Seq Scan on products
```

✅ Very efficient for **equality joins** (e.g., `=`).

---

## ⚙️ 3. **Merge Join**

🧠 **How it works**:
- Sorts both tables on the join key
- Then scans them **in order**, matching rows like merging two sorted lists

#### 📘 Best When:
- **Both sides are already sorted**
- **Indexes on join key**
- Large result sets

#### 🧪 Example:
```sql
EXPLAIN ANALYZE
SELECT * FROM orders o
JOIN dates d ON o.date_id = d.date_id;
```

Planner output:
```
Merge Join
  -> Index Scan on orders
  -> Index Scan on dates
```

✅ Very fast if sorted, but sorting adds cost if not already sorted.

---

## 🧠 When Does PostgreSQL Choose Which Join?

It depends on:
- Table size
- Presence of indexes
- Join condition type
- `work_mem` setting (for hash join memory)
- Estimated number of rows

You can check what was used with:
```sql
EXPLAIN ANALYZE SELECT ...
```

---

## 🔍 Force a Join Type (for testing/debugging)

You can **disable join methods** to force PostgreSQL to choose another:

```sql
SET enable_hashjoin = off;
SET enable_mergejoin = off;
```

Then run `EXPLAIN` again to compare.

---

## 🎯 Summary

| Join Type    | Best For                  | Index Needed? | Notes |
|--------------|---------------------------|----------------|-------|
| Nested Loop  | Small inner table         | ✅ Helps       | Slow for big tables |
| Hash Join    | Equality joins, medium data | ❌ No         | Fast in memory |
| Merge Join   | Pre-sorted large tables   | ✅ Recommended | Great for ranges |

---

Would you like:
- A sample dataset to test all 3 join types?
- A visual diagram showing join flow?

Let me know and I can generate hands-on exercises for you!
