# Experiment 10: PL/SQL – Triggers

## AIM
To write and execute PL/SQL trigger programs for automating actions in response to specific table events like INSERT, UPDATE, or DELETE.

---

## THEORY

A **trigger** is a stored PL/SQL block that is automatically executed or fired when a specified event occurs on a table or view. Triggers can be used for enforcing business rules, auditing changes, or automatic updates.

### Types of Triggers:
- **Before Trigger**: Executes before the operation (INSERT, UPDATE, DELETE).
- **After Trigger**: Executes after the operation.
- **Row-level Trigger**: Executes for each affected row.
- **Statement-level Trigger**: Executes once for the triggering statement.

**Basic Syntax:**
```sql
CREATE OR REPLACE TRIGGER trigger_name
BEFORE|AFTER INSERT|UPDATE|DELETE ON table_name
[FOR EACH ROW]
BEGIN
   -- trigger logic
END;
```

## 1. Write a trigger to log every insertion into a table.
**Steps:**
- Create two tables: `employees` (for storing data) and `employee_log` (for logging the inserts).
- Write an **AFTER INSERT** trigger on the `employees` table to log the new data into the `employee_log` table.

**Expected Output:**
- A new entry is added to the `employee_log` table each time a new record is inserted into the `employees` table.

## Program:
```
CREATE TABLE employees (
  emp_id      NUMBER PRIMARY KEY,
  emp_name    VARCHAR2(50),
  designation VARCHAR2(50),
  salary      NUMBER
);
CREATE TABLE employee_log (
  log_id      NUMBER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
  emp_id      NUMBER,
  emp_name    VARCHAR2(50),
  designation VARCHAR2(50),
  log_time    DATE
);

CREATE OR REPLACE TRIGGER trg_employee_insert_log
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
  INSERT INTO employee_log(emp_id, emp_name, designation, log_time)
  VALUES (:NEW.emp_id, :NEW.emp_name, :NEW.designation, SYSDATE);
END;
/
INSERT INTO employees VALUES (1, 'Ravi',  'Developer', 30000);
INSERT INTO employees VALUES (2, 'Meena', 'Tester',    25000);
COMMIT;
SELECT * FROM employee_log;

```

## Output:

<img width="696" height="320" alt="image" src="https://github.com/user-attachments/assets/2ae9e81d-390e-47b3-8ccf-acf92d0764d5" />

---

## 2. Write a trigger to prevent deletion of records from a sensitive table.
**Steps:**
- Write a **BEFORE DELETE** trigger on the `sensitive_data` table.
- Use `RAISE_APPLICATION_ERROR` to prevent deletion and issue a custom error message.

**Expected Output:**
- If an attempt is made to delete a record from `sensitive_data`, an error message is raised, e.g., `ERROR: Deletion not allowed on this table.`

## Program:
```
CREATE TABLE sensitive_data (
  record_id   NUMBER PRIMARY KEY,
  info        VARCHAR2(100)
);
INSERT INTO sensitive_data VALUES (1, 'Top Secret File');
INSERT INTO sensitive_data VALUES (2, 'Confidential Report');
COMMIT;
CREATE OR REPLACE TRIGGER trg_no_delete_sensitive
BEFORE DELETE ON sensitive_data
FOR EACH ROW
BEGIN
  RAISE_APPLICATION_ERROR(-20001, 'ERROR: Deletion not allowed on this table.');
END;
/
DELETE FROM sensitive_data WHERE record_id = 1;
SELECT * FROM sensitive_data;

```

## Output:

<img width="624" height="256" alt="image" src="https://github.com/user-attachments/assets/e8237766-7407-4cbc-aafe-082e39ca3a1b" />

---

## 3. Write a trigger to automatically update a `last_modified` timestamp.
**Steps:**
- Add a `last_modified` column to the `products` table.
- Write a **BEFORE UPDATE** trigger on the `products` table to set the `last_modified` column to the current timestamp whenever an update occurs.

**Expected Output:**
- The `last_modified` column in the `products` table is updated automatically to the current date and time when any record is updated.

## Program:
```
CREATE TABLE products (
  product_id     NUMBER PRIMARY KEY,
  product_name   VARCHAR2(50),
  price          NUMBER,
  last_modified  DATE
);
INSERT INTO products VALUES (1, 'Laptop', 50000, SYSDATE);
INSERT INTO products VALUES (2, 'Mouse',  500,    SYSDATE);
COMMIT;
CREATE OR REPLACE TRIGGER trg_update_last_modified
BEFORE UPDATE ON products
FOR EACH ROW
BEGIN
  :NEW.last_modified := SYSDATE;
END;
/
UPDATE products SET price = price + 1000 WHERE product_id = 1;
COMMIT;
SELECT * FROM products;

```

## Output:

<img width="686" height="299" alt="image" src="https://github.com/user-attachments/assets/e5338f66-4b5a-4e4f-a24d-8cb079f89090" />


---

## 4. Write a trigger to keep track of the number of updates made to a table.
**Steps:**
- Create an `audit_log` table with a counter column.
- Write an **AFTER UPDATE** trigger on the `customer_orders` table to increment the counter in the `audit_log` table every time a record is updated.

**Expected Output:**
- The `audit_log` table will maintain a count of how many updates have been made to the `customer_orders` table.

## Program:
```
CREATE TABLE customer_orders (
  order_id     NUMBER PRIMARY KEY,
  customer_name VARCHAR2(50),
  amount        NUMBER
);

INSERT INTO customer_orders VALUES (1, 'Ravi', 1000);
INSERT INTO customer_orders VALUES (2, 'Meena', 1500);
COMMIT;
CREATE TABLE audit_log (
  update_count NUMBER
);

-- Initialize the counter to 0
INSERT INTO audit_log VALUES (0);
COMMIT;
CREATE OR REPLACE TRIGGER trg_count_updates
AFTER UPDATE ON customer_orders
BEGIN
  UPDATE audit_log
  SET update_count = update_count + 1;
END;
/
UPDATE customer_orders SET amount = amount + 500 WHERE order_id = 1;
UPDATE customer_orders SET amount = amount + 100 WHERE order_id = 2;
COMMIT;
SELECT * FROM audit_log;

```

## Output:

<img width="184" height="169" alt="image" src="https://github.com/user-attachments/assets/9e4e603a-f127-427c-b674-5cb2d708fb19" />

---

## 5. Write a trigger that checks a condition before allowing insertion into a table.
**Steps:**
- Write a **BEFORE INSERT** trigger on the `employees` table to check if the inserted salary meets a specific condition (e.g., salary must be greater than 3000).
- If the condition is not met, raise an error to prevent the insert.

**Expected Output:**
- If the inserted salary in the `employees` table is below the condition (e.g., salary < 3000), the insert operation is blocked, and an error message is raised, such as: `ERROR: Salary below minimum threshold.`

## Program:
```
CREATE TABLE employees (
  emp_id      NUMBER PRIMARY KEY,
  emp_name    VARCHAR2(50),
  designation VARCHAR2(50),
  salary      NUMBER
);
CREATE OR REPLACE TRIGGER trg_check_salary
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
  IF :NEW.salary < 3000 THEN
    RAISE_APPLICATION_ERROR(-20002, 'ERROR: Salary below minimum threshold.');
  END IF;
END;
/
INSERT INTO employees VALUES (1, 'Ravi', 'Developer', 2500);
INSERT INTO employees VALUES (2, 'Meena', 'Tester', 3500);
COMMIT;
SELECT * FROM employees;

```

## Output:


<img width="643" height="205" alt="image" src="https://github.com/user-attachments/assets/084a1ea5-cade-41c7-bbd5-587e66cc90ce" />

## RESULT
Thus, the PL/SQL trigger programs were written and executed successfully.


