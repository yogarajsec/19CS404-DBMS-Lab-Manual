# Experiment 6: Joins

## AIM
To study and implement different types of joins.

## THEORY

SQL Joins are used to combine records from two or more tables based on a related column.

### 1. INNER JOIN
Returns records with matching values in both tables.

**Syntax:**
```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.column = table2.column;
```

### 2. LEFT JOIN
Returns all records from the left table, and matched records from the right.

**Syntax:**

```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.column = table2.column;
```
### 3. RIGHT JOIN
Returns all records from the right table, and matched records from the left.

**Syntax:**

```sql
SELECT columns
FROM table1
RIGHT JOIN table2
ON table1.column = table2.column;
```
### 4. FULL OUTER JOIN
Returns all records when there is a match in either left or right table.

**Syntax:**

```sql
SELECT columns
FROM table1
FULL OUTER JOIN table2
ON table1.column = table2.column;
```

**Question 1**
--
Write a SQL statement to join the tables salesman, customer and orders so that the same column of each table appears once and only the relational rows are returned. 
```sql
SELECT 
    o.ord_no,
    o.purch_amt,
    o.ord_date,
    c.cust_name,
    c.city AS customer_city,
    c.grade,
    s.name AS salesman_name,
    s.city AS salesman_city,
    s.commission
FROM orders o
INNER JOIN customer c
    ON o.customer_id = c.customer_id
INNER JOIN salesman s
    ON o.salesman_id = s.salesman_id;

```

**Output:**

<img width="1391" height="860" alt="image" src="https://github.com/user-attachments/assets/0cf90f85-fa56-4af5-a39e-245ac897dcb0" />

**Question 2**
---
Write the SQL query that achieves the selection of the "cust_name" column from the "customer" table (aliased as "c"), and the "ord_no," "ord_date," and "purch_amt" columns from the "orders" table (aliased as "o"), with a left join on the "customer_id" column and a condition filtering for orders with a purchase amount greater than 1000.

```sql
SELECT 
    c.cust_name,
    o.ord_no,
    o.ord_date,
    o.purch_amt
FROM customer c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
WHERE o.purch_amt > 1000;

```

**Output:**

<img width="1285" height="780" alt="image" src="https://github.com/user-attachments/assets/a7735900-0164-40b8-b6c6-46330ff2ddb8" />

**Question 3**
---
From the following tables write a SQL query to locate those salespeople who do not live in the same city where their customers live and have received a commission of more than 12% from the company. Return Customer Name, customer city, Salesman, salesman city, commission.  

```sql
SELECT
  c.cust_name      AS "Customer Name",
  c.city           AS "city",
  s.name           AS "Salesman",
  s.city           AS "city",
  s.commission
FROM customer c
JOIN salesman s
  ON c.salesman_id = s.salesman_id
WHERE c.city <> s.city
  AND s.commission > 0.12;
```

**Output:**

<img width="1269" height="700" alt="image" src="https://github.com/user-attachments/assets/edf85f38-35ab-4323-89d8-a40e26ebcc6f" />


**Question 4**
---
 From the following tables write a SQL query to find salespeople who received commissions of more than 12 percent from the company. Return Customer Name, customer city, Salesman, commission.  

```sql
SELECT 
    c.cust_name AS "Customer Name",
    c.city,
    s.name AS "Salesman",
    s.commission
FROM customer c
INNER JOIN salesman s
    ON c.salesman_id = s.salesman_id
WHERE s.commission > 0.12;

```

**Output:**

<img width="1259" height="835" alt="image" src="https://github.com/user-attachments/assets/f0a9663e-017b-4c40-a672-5053b8342d7b" />

**Question 5**
---
Write the SQL query that achieves the selection of all columns from the "patients" table (aliased as "p"), with an inner join on the "patient_id" column and a condition filtering for appointments with an appointment date between '2024-02-01' and '2024-02-28'.

```sql
SELECT p.*
FROM patients AS p
INNER JOIN appointments AS a ON p.patient_id = a.patient_id
WHERE a.appointment_date BETWEEN '2024-02-01' AND '2024-02-28';
```

**Output:**

<img width="1239" height="485" alt="image" src="https://github.com/user-attachments/assets/a9df7b32-dc36-470a-8e4f-1516d6bd41bd" />


**Question 6**
---
Write the SQL query that achieves the selection of all columns from the "salesman" table (aliased as "s"), with a left join on the "salesman_id" column and a condition filtering for customers with the name 'Fabian Johns'.

```sql
SELECT s.*
FROM salesman AS s
LEFT JOIN customer AS c ON s.salesman_id = c.salesman_id
WHERE c.cust_name = 'Fabian Johns';
```

**Output:**

<img width="1248" height="480" alt="image" src="https://github.com/user-attachments/assets/a356c41b-c5f9-4f14-89e6-8415a3c283b4" />


**Question 7**
---
 From the following tables write a SQL query to find the salesperson(s) and the customer(s) he represents. Return Customer Name, city, Salesman, commission.

```sql
SELECT 
    c.cust_name AS "Customer Name",
    c.city AS "city",
    s.name AS "Salesman",
    s.commission
FROM 
    customer c
INNER JOIN 
    salesman s ON c.salesman_id = s.salesman_id;
```

**Output:**

<img width="1238" height="852" alt="image" src="https://github.com/user-attachments/assets/a03e5313-4437-4e92-94bb-0251576f6797" />


**Question 8**
---
Write the SQL query that achieves the selection of all columns from the "patients" table, with an inner join on the "doctor_id" column, and includes a condition filtering for patients whose doctors have the first name 'John' and last name 'Smith'.

```sql
SELECT p.*
FROM patients p
INNER JOIN doctors d
  ON p.doctor_id = d.doctor_id
WHERE d.first_name = 'John'
  AND d.last_name = 'Smith';
```

**Output:**

<img width="1278" height="452" alt="image" src="https://github.com/user-attachments/assets/98f34431-c136-43de-ac04-e6d0817f5453" />

**Question 9**
---
From the following tables write a SQL query to find those orders where the order amount exists between 500 and 2000. Return ord_no, purch_amt, cust_name, city.

```sql
SELECT 
    o.ord_no,
    o.purch_amt,
    c.cust_name,
    c.city
FROM 
    orders o
INNER JOIN 
    customer c ON o.customer_id = c.customer_id
WHERE 
    o.purch_amt BETWEEN 500 AND 2000;
```

**Output:**

<img width="1199" height="478" alt="image" src="https://github.com/user-attachments/assets/00c77778-0500-4707-b053-9be51cf8365c" />


**Question 10**
---
Write the SQL query that achieves the selection of the first name from the "patients" table (aliased as "patient_name") and all columns from the "test_results" table (aliased as "t"), with an inner join on the "patient_id" column.

```sql
SELECT patient_name.first_name AS patient_name, t.*
FROM patients AS patient_name
INNER JOIN test_results AS t ON patient_name.patient_id = t.patient_id;
```

**Output:**

<img width="1282" height="568" alt="image" src="https://github.com/user-attachments/assets/4975f67e-a529-4a8c-8efc-9065ceccd893" />

**SEB Grades**

<img width="1063" height="82" alt="Screenshot 2025-11-13 213252" src="https://github.com/user-attachments/assets/74481875-c244-49de-b983-f4357a759475" />


## RESULT
Thus, the SQL queries to implement different types of joins have been executed successfully.
