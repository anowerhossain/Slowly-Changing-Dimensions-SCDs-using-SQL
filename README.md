# Slowly Changing Dimensions (SCDs) in Data Warehousing 🌐

## **What are SCDs?**  
Slowly Changing Dimensions (SCDs) are dimensions that change over time, but not frequently. These dimensions require special handling in data warehousing to preserve historical accuracy while maintaining the integrity of the data.

## **Types of Slowly Changing Dimensions (SCDs)**

### **1. SCD Type 1: Overwrite the Existing Data ✏️**
- **Definition:** The old data is **replaced** with new data, and history is **lost**.  
- **Use Case:** When **historical data is not needed**.

#### Example:

| Customer_ID | Name  | City     |
|-------------|-------|----------|
| 101         | John  | New York |

After John moves to **Los Angeles**, the data is updated to:

| Customer_ID | Name  | City        |
|-------------|-------|-------------|
| 101         | John  | Los Angeles |

- **Effect:** The previous city (New York) is lost.

---

### **2. SCD Type 2: Maintain History with Versioning or Effective Dates 📅**
- **Definition:** A **new record is added** when changes occur, keeping a history of **past values**.  
- **Use Case:** When **tracking historical changes is important**.

#### Example:

| Customer_ID | Name  | City         | Start_Date  | End_Date   | Active_Flag |
|-------------|-------|--------------|-------------|------------|-------------|
| 101         | John  | New York     | 2020-01-01  | 2023-06-30 | N           |
| 101         | John  | Los Angeles  | 2023-07-01  | NULL       | Y           |

- **Effect:** Historical data is retained, and the latest record is marked as active (`Active_Flag = Y`).

---

### **3. SCD Type 3: Track Limited History with Additional Columns 🕒**
- **Definition:** A **limited history** is maintained by storing old values in **separate columns**.  
- **Use Case:** When **only recent changes** need to be tracked.

#### Example:

| Customer_ID | Name  | Current_City | Previous_City | Change_Date |
|-------------|-------|--------------|---------------|-------------|
| 101         | John  | Los Angeles  | New York      | 2023-07-01  |

- **Effect:** Only the **previous value** is retained, and older changes are lost.

---

## **Key Benefits of Implementing SCDs 🌟**

1. **Historical Accuracy 📊:** Maintain accurate records of changes over time.
2. **Data Integrity 🔒:** Ensure that past data is preserved while reflecting current values.
3. **Easy Analysis 🔍:** Enable business intelligence tools to perform accurate trend analysis and decision-making based on historical data.
4. **Scalable Data Architecture 🏗️:** Choose the appropriate SCD type based on the business requirements and the frequency of changes.

---

## **Which SCD Type to Use? 🔄**
- **Type 1:** When historical tracking is **not required** (e.g., fixing data errors).
- **Type 2:** When **full historical tracking** is needed (e.g., tracking customer address changes).
- **Type 3:** When **limited historical tracking** is sufficient (e.g., tracking the last two addresses).

---

## **Implementation in SQL 🚀**

### **SCD Type 1: SQL Example**  
```sql
UPDATE customer
SET city = 'Los Angeles'
WHERE customer_id = 101;
```

### **SCD Type 1: SQL Example**  
```sql
-- Insert new record with the new city and mark the old record as inactive
UPDATE customer
SET end_date = '2023-06-30', active_flag = 'N'
WHERE customer_id = 101 AND active_flag = 'Y';

INSERT INTO customer (customer_id, name, city, start_date, end_date, active_flag)
VALUES (101, 'John', 'Los Angeles', '2023-07-01', NULL, 'Y');
```

### **SCD Type 1: SQL Example**  
```sql
UPDATE customer
SET previous_city = current_city, current_city = 'Los Angeles', change_date = '2023-07-01'
WHERE customer_id = 101;
```


Managing Slowly Changing Dimensions (SCDs) is essential for maintaining accurate and reliable data in a data warehouse. By carefully choosing the right type of SCD, businesses can track important changes while ensuring data integrity and historical accuracy.
