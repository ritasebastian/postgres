

## 📘 What is a Slowly Changing Dimension (SCD)?
In a data warehouse, **dimensions** (like customer or product info) can change over time.  
To keep track of these changes, we use **SCD strategies** like Type 1, Type 2, and Type 3.

---

## 🔹 Type 1 – **Overwrite (No History)**

### 💡 What it means:
Just **overwrite the old value** with the new one.  
You **do not keep history**.

### ✅ Use Case:
When you **don’t care about historical data**, just want the latest.

### 🧪 Example:
| Customer_ID | Name     | City     |
|-------------|----------|----------|
| 101         | Alice    | Mumbai   |

➡ Alice moves to Delhi  
Update → `City = 'Delhi'`

| Customer_ID | Name     | City     |
|-------------|----------|----------|
| 101         | Alice    | Delhi    |

---

## 🔸 Type 2 – **Track History (Versioning)**

### 💡 What it means:
**Keep the old record**, and **insert a new row** with the updated value.  
Usually includes `start_date`, `end_date`, or an `is_current` flag.

### ✅ Use Case:
When you need **full historical tracking** of data changes.

### 🧪 Example:
| Customer_ID | Name     | City     | Version | Is_Current |
|-------------|----------|----------|---------|------------|
| 101         | Alice    | Mumbai   | 1       | FALSE      |
| 101         | Alice    | Delhi    | 2       | TRUE       |

---

## 🔹 Type 3 – **Partial History (Add Column)**

### 💡 What it means:
**Add a new column** to store the **previous value**, but only for **one change**.

### ✅ Use Case:
When you just need to know the **current and immediate previous state**.

### 🧪 Example:
| Customer_ID | Name     | City_Current | City_Previous |
|-------------|----------|--------------|----------------|
| 101         | Alice    | Delhi        | Mumbai         |

---

## 🧠 Summary Table

| Type | History Tracked? | How? | Use Case |
|------|------------------|------|----------|
| **Type 1** | ❌ No | Overwrite value | Latest value is enough |
| **Type 2** | ✅ Full | Add new row with version/date | Need full audit/history |
| **Type 3** | ✅ Partial | Add extra column for previous value | Need only current + last |

---
