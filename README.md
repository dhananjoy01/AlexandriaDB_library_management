
# 📚 AlexandriaDB - Library Management System Using SQL

## 📘 Project Overview

**Project Title**: AlexandriaDB - SQL-Based Library System  
**Level**: Intermediate  
**Database**: `alexandria_db`

This project simulates a functional **library management system** built entirely using SQL. It covers core concepts of database design, normalization, foreign key relationships, procedural SQL, and query optimization for reporting. 

![Library Architecture](ChatGPT%20Image%20Jun%2018,%202025,%2009_06_14%20AM.png)

---

## 🎯 Objectives

1. 📂 Design & create a relational database system for library operations.
2. 🛠️ Use **DDL** and **DML** for CRUD operations and data management.
3. 📈 Apply **CTAS** (Create Table As Select) for reporting.
4. 🔁 Implement stored procedures for issuing and returning books.
5. 📊 Generate analytical insights on members, employees, and branch performance.

---

## 🏗️ Schema Design

### Tables Included:
- **branch** - Branch details and manager mapping  
- **employee** - Employee info with branch relationship  
- **books** - Books with availability and pricing info  
- **members** - Library members  
- **issued_table** - Tracks which book was issued by whom and when  
- **return_status** - Tracks book returns  

Foreign key constraints ensure relational integrity. Salary column datatype was updated to FLOAT, and contact numbers were expanded for global compatibility.

---

## 🔧 Core Functionalities

### ✅ Basic Operations:
- Insert new books and members  
- Update member addresses  
- Delete issued records  
- Retrieve books issued by specific employees  
- List members who issued more than one book

### 📋 Sample Queries:

```sql
-- Insert a new book
INSERT INTO books 
VALUES('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.');

-- Update a member's address
UPDATE members 
SET member_address = '145 Pine st' 
WHERE member_address = '123 Main St';

-- Retrieve books issued by employee E101
SELECT * FROM issued_table 
WHERE issued_emp_id = 'E101';
```

---

## 📑 CTAS Reports

### Task 6: Book Issue Summary Table

```sql
CREATE TABLE book_count AS
SELECT b.isbn, book_title, COUNT(ist.issued_book_isbn) AS total_issued
FROM books b
JOIN issued_table ist ON b.isbn = ist.issued_book_isbn
GROUP BY 1, 2;
```

### Task 11: Expensive Books Report

```sql
CREATE TABLE expensive_books AS
SELECT book_title, rental_price 
FROM books 
WHERE rental_price > 7.00;
```

---

## 🧠 Advanced SQL & Procedures

### Task 13: Overdue Books Report

```sql
SELECT
	ist.issued_member_id,
	mem.member_name,
	bk.book_title,
	ist.issued_date,
	CURRENT_DATE - ist.issued_date AS overdue_days
FROM issued_table ist
JOIN members mem ON ist.issued_member_id = mem.member_id
JOIN books bk ON ist.issued_book_isbn = bk.isbn
LEFT JOIN return_status rs ON ist.issued_id = rs.issued_id
WHERE rs.return_date IS NULL AND CURRENT_DATE - ist.issued_date > 30;
```

### Task 14: Return Procedure

```sql
CALL add_return_records('RS190', 'IS106', 'Good');
```

### Task 19: Issue Procedure (with stock check)

```sql
CALL updating_status_of_books('IS155', 'C108', '978-0-553-29698-2', 'E104');
```

---

## 📊 Analytical Insights

- **Branch Performance** (Task 15): Number of books issued & returned, total revenue.
- **Top Employees** (Task 17): Based on issue count.
- **Active Members** (Task 16): Issued books in the last 2 months.
- **Overdue & Fine Calculation** (Task 20 - recommended enhancement).

---

## 🚀 How to Use

1. Clone this repository to your local system.
2. Open your SQL client (e.g., pgAdmin, DBeaver).
3. Run the full script from `library_database.sql`.
4. Explore and run queries as per your interest.

---

## 👨‍💻 Author: Dhanan Joy Chandro Roy

If you liked this project or want to collaborate:

- 💼 [LinkedIn](https://www.linkedin.com)  
- 📷 [Instagram](https://www.instagram.com)  
- 🧠 YouTube/Portfolio: Coming soon!  

---

## 📁 Notes

- You can replace the local image path with a hosted version once uploaded to GitHub.
- Ensure PostgreSQL procedural language `plpgsql` is enabled for procedure execution.

---

> 🏛️ "Alexandria was once the greatest library. Now, it's in your database." – Dhanan Joy
