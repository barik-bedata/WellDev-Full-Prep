# WellDev Interview Prep - DBMS ও SQL - উত্তরসহ

> Source: CPS Academy - "WellDev Interview Prep - Bangla" course, Module 15 (SQL ও Database), Lessons 65-67.

## সূচি
- SQL Basics ও Query (SELECT, WHERE, GROUP BY, HAVING, Execute Order, DISTINCT, NULL)
- JOIN ও Duplicate খোঁজা (JOIN types, duplicate rows, Nth highest salary, subquery)
- Normalization ও ACID (1NF/2NF/3NF, ACID, Primary/Foreign Key)

---

## Lesson 65 - SQL Basics ও Query (Database-এর ভাষা)

ধরা যাক আমাদের কাছে একটি `employees(id, name, department, salary)` টেবিল আছে। নিচের ধারণাগুলো এই টেবিলের ওপর ভিত্তি করে তৈরি:

### ১. Basic SELECT
```sql
SELECT name, salary FROM employees
WHERE department = 'Eng'
ORDER BY salary DESC;
```
- **SELECT:** কলাম নির্বাচন করে। `SELECT *` মানে সব কলাম।
- **FROM:** কোন টেবিল থেকে ডেটা আসবে তা ঠিক করে।
- **WHERE:** শর্ত দিয়ে Row ফিল্টার করে (একাধিক শর্তের জন্য `AND` / `OR` ব্যবহার করা যায়)।
- **ORDER BY:** ডেটা সাজানোর জন্য ব্যবহৃত হয়। (DESC = বড় থেকে ছোট, ASC = ছোট থেকে বড়, ডিফল্ট ASC থাকে)।
- **LIMIT n:** প্রথম n-টি Row দেখায়।

### ২. GROUP BY ও Aggregate
```sql
SELECT department, AVG(salary) AS avg_salary
FROM employees 
GROUP BY department
HAVING AVG(salary) > 55000;
```
- **Aggregate function:** `COUNT()`, `SUM()`, `AVG()`, `MAX()`, `MIN()`। 
- **GROUP BY:** একই মানের Row-গুলোকে একসাথে জড়ো করে এবং প্রতি গ্রুপের জন্য Aggregate হিসাব করে।
- **WHERE vs HAVING:** `WHERE` গ্রুপ করার **আগে** ফিল্টার করে (তাই Aggregate ফাংশন `WHERE`-এ কাজ করে না)। অন্যদিকে, `HAVING` গ্রুপ করার **পরে** ফিল্টার করে (Aggregate ফাংশনের শর্ত এখানেই দিতে হয়)।

### ৩. SQL Execute Order (খুবই গুরুত্বপূর্ণ!)
- **কোড লেখার ক্রম:** `SELECT -> FROM -> WHERE -> GROUP BY -> HAVING -> ORDER BY`
- **আসল Execute-এর ক্রম:** `FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY -> LIMIT`

**ব্যাখ্যা:**
- `WHERE`-এর ভেতর `SELECT`-এর alias ব্যবহার করা যায় না, কারণ `SELECT` অনেক পরে এক্সিকিউট হয়। কিন্তু `ORDER BY`-তে alias কাজ করে।
- `WHERE`-এ aggregate ফাংশন কাজ করে না, কারণ গ্রুপ তৈরি হওয়ার আগেই এটি রান করে। এজন্য `HAVING` দরকার হয়।

### ৪. DISTINCT ও NULL
- **DISTINCT:** `SELECT DISTINCT department` - ডুপ্লিকেট বাদ দিয়ে শুধুমাত্র ইউনিক মানগুলো দেখায়।
- **NULL:** এর মানে "মান নেই বা অজানা" (এটি 0 বা খালি স্ট্রিং নয়)। `column = NULL` লিখলে কাজ করবে না, এর বদলে `IS NULL` বা `IS NOT NULL` ব্যবহার করতে হয়। NULL-সহ যেকোনো যোগ-বিয়োগের ফল NULL হয়। 
  - `COUNT(column)` শুধু নন-নাল (NULL নয়) মান গোনে, আর `COUNT(*)` সব Row গোনে।
- **ফাঁদ:** `WHERE department != 'Eng'` দিলে যাদের department NULL তারা আসবে না। কারণ NULL-এর সাথে তুলনা করা যায় না। তাই লিখতে হয় `OR department IS NULL`।

**এক নজরে:**
| ধারণা | এক লাইনে |
|---|---|
| SELECT/FROM/WHERE/ORDER BY | Column বাছা, টেবিল, Filter করা, সাজানো |
| GROUP BY | একই মান জড়ো করা + Aggregate ফাংশন |
| WHERE vs HAVING | Row-এর ওপর (আগে) vs Group-এর ওপর (পরে) |
| Execute ক্রম | FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY -> LIMIT |
| DISTINCT | ডুপ্লিকেট বাদ দেওয়া |
| NULL | মান নেই; চেক করতে `IS NULL` লাগে (`=` দিয়ে চেক করা যায় না) |

Practice: LeetCode SQL - 595 (Big Countries), 1757 (multi-condition WHERE), 596 (GROUP BY+HAVING), 1741 (GROUP BY+SUM)

---

## Lesson 66 - JOIN ও Duplicate খোঁজা (একাধিক টেবিল জোড়া লাগানো)

### ১. JOIN ও তার ধরন
JOIN দুটি টেবিলকে একটি কমন কলামের ওপর ভিত্তি করে জোড়া লাগায়। এর চার ধরন:
- **INNER JOIN:** দুই টেবিলেই মিল আছে এমন Row-গুলোই শুধু আসবে।
- **LEFT JOIN:** বাম টেবিলের সব Row আসবে + ডান টেবিলে মিল থাকলে তা আসবে (না থাকলে ডান দিকে NULL দেখাবে)।
- **RIGHT JOIN:** ডান টেবিলের সব Row + বাম টেবিলের মিল থাকা অংশ।
- **FULL JOIN:** দুই টেবিলের সব ডেটা নিয়ে আসবে।

```sql
SELECT e.name, d.dept_name 
FROM employees e
INNER JOIN departments d ON e.dept_id = d.id;   -- শুধু মিল থাকা ডেটা আসবে
```

### ২. Duplicate ডেটা খোঁজা
```sql
SELECT email, COUNT(*) AS cnt 
FROM users
GROUP BY email 
HAVING COUNT(*) > 1;
```
`GROUP BY` দিয়ে একই ইমেইল জড়ো করা হয়েছে, `COUNT` দিয়ে গোনা হয়েছে, এবং `HAVING > 1` দিয়ে ডুপ্লিকেটগুলো ফিল্টার করা হয়েছে। এখানে `HAVING` ব্যবহার করতেই হবে কারণ `COUNT` একটি Aggregate ফাংশন।

**ডুপ্লিকেট মুছে ফেলার কুয়েরি (একটি রেখে বাকিগুলো):**
```sql
DELETE FROM users 
WHERE id NOT IN (SELECT MIN(id) FROM users GROUP BY email);
```

### ৩. দ্বিতীয় সর্বোচ্চ Salary বের করা (Classic Interview Question!)
**উপায় ১ (Subquery):**
```sql
SELECT MAX(salary) AS second_highest 
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```
**উপায় ২ (DISTINCT + ORDER BY + OFFSET):**
```sql
SELECT DISTINCT salary 
FROM employees
ORDER BY salary DESC 
LIMIT 1 OFFSET 1;
```
*DISTINCT জরুরি, কারণ ডুপ্লিকেট সর্বোচ্চ স্যালারি থাকলে ফল ভুল আসবে। N-তম সর্বোচ্চ বের করতে `OFFSET N-1` ব্যবহার করা হয়। (সবার স্যালারি সমান হলে Subquery নিরাপদে NULL দেয়, আর OFFSET খালি রেজাল্ট দেয়)।*

### ৪. Subquery
Query-র ভেতরে আরেকটি Query। ভেতরের কুয়েরির রেজাল্ট বাইরের কুয়েরিতে ব্যবহার করা হয়। এটি `WHERE` (ফিল্টার করতে), `FROM` (derived table), বা `SELECT` (একটি নির্দিষ্ট মান পেতে) এ ব্যবহার করা যায়।

```sql
SELECT name, salary 
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);  -- গড়ের চেয়ে বেশি স্যালারি যাদের
```
- **Subquery vs JOIN:** অনেক কাজ দুটো দিয়েই করা যায়। তবে বড় ডেটার ক্ষেত্রে সাধারণত JOIN বেশি দ্রুত হয়। 
- **Correlated Subquery:** ভেতরের কুয়েরি যখন বাইরের কুয়েরির প্রতি Row-এর ওপর নির্ভর করে। এটি খুব শক্তিশালী কিন্তু বেশ ধীর গতির।

**এক নজরে:**
| ধারণা | এক লাইনে |
|---|---|
| INNER JOIN | দুই টেবিলে যাদের মাঝে মিল আছে |
| LEFT JOIN | বাম টেবিলের সব + ডান টেবিলের মিল থাকা অংশ (না থাকলে NULL) |
| Duplicate খোঁজা | `GROUP BY` + `COUNT(*)` + `HAVING COUNT(*) > 1` |
| ২য় সর্বোচ্চ Salary | Subquery (`MAX < MAX`) বা `DISTINCT` + `ORDER BY` + `OFFSET` |
| Subquery | Query-র ভেতরে Query; `WHERE`/`FROM`/`SELECT`-এ বসে |

Practice: LeetCode SQL - 175 (Combine Two Tables, LEFT JOIN), 181 (Employees Earning More Than Managers, self JOIN), 182 (Duplicate Emails), 176 (Second Highest Salary - classic!), 184 (Department Highest Salary, JOIN+subquery)

---

## Lesson 67 - Normalization ও ACID (Database Design ও নির্ভরযোগ্যতা)

### ১. Normalization কী ও কেন?
ডেটাবেসের টেবিলগুলোকে এমনভাবে সাজানো যাতে Redundancy (একই ডেটা বারবার থাকা) কমে এবং Consistency বজায় থাকে। মূলনীতি: "এক ডেটা এক জায়গায় থাকবে"।

**Unnormalized ডেটাবেসের সমস্যা:**
- **Update Anomaly:** একই কাস্টমারের ফোন নম্বর একাধিক জায়গায় থাকলে বদলাতে সমস্যা হয় (ভুল হলে অসামঞ্জস্য তৈরি হয়)।
- **Insert Anomaly:** নতুন কাস্টমার যোগ করার জন্য একটি ভুয়া অর্ডার তৈরি করতে হয়।
- **Delete Anomaly:** কোনো অর্ডার ডিলিট করলে কাস্টমারের তথ্যও মুছে যায়।
**সমাধান:** টেবিল ভেঙে আলাদা করা (customers, orders) এবং Foreign Key দিয়ে তাদের মাঝে সম্পর্ক তৈরি করা।

### ২. 1NF / 2NF / 3NF
- **1NF:** প্রতিটি সেলে শুধুমাত্র একটি Atomic (অবিভাজ্য) মান থাকবে। কোনো লিস্ট বা কমা দিয়ে একাধিক মান রাখা যাবে না।
- **2NF:** 1NF হতে হবে + প্রতিটি Non-key কলাম পুরো Primary Key-এর ওপর নির্ভর করবে (কোনো আংশিক বা Partial Dependency থাকবে না)।
- **3NF:** 2NF হতে হবে + কোনো Non-key কলাম অন্য আরেকটি Non-key কলামের ওপর নির্ভর করবে না (Transitive Dependency থাকবে না)।

> মনে রাখার সূত্র: প্রতিটি কলাম নির্ভর করবে **"The Key (1NF), The Whole Key (2NF), and Nothing But The Key (3NF)"**-এর ওপর।

*(দ্রষ্টব্য: বেশি Normalization মানে বেশি টেবিল এবং বেশি JOIN, যা সিস্টেমকে ধীর করে দিতে পারে। তাই রিড-হেভি সিস্টেমে কখনো কখনো ইচ্ছাকৃতভাবে Denormalization করা হয়।)*

### ৩. ACID প্রোপার্টিজ (Transaction-এর ৪টি গ্যারান্টি)
Transaction হলো কয়েকটি অপারেশনের একটি গ্রুপ যা হয় একসাথে সফল হবে, নয়তো পুরোপুরি ব্যর্থ হবে।
- **A - Atomicity (অবিভাজ্যতা):** সব অপারেশন একসাথে হবে, নয়তো একটিও হবেবিধা হবে না। (যেমন: ব্যাংক ট্রান্সফারে টাকা কাটা + জমা হওয়া, মাঝপথে সার্ভার ক্র্যাশ করলে পুরোটা বাতিল বা Rollback হবে)।
- **C - Consistency (সামঞ্জস্যতা):** ট্রানজেকশনের আগে ও পরে ডেটাবেস সবসময় একটি বৈধ অবস্থায় থাকবে এবং কোনো নিয়ম ভাঙবে না। (যেমন: ট্রান্সফারের আগে ও পরে দুই অ্যাকাউন্টের মোট টাকা সমান থাকবে)।
- **I - Isolation (বিচ্ছিন্নতা):** একসাথে চলা একাধিক ট্রানজেকশন একে অপরকে প্রভাবিত করবে না। (যেমন: দুজন একসাথে একই অ্যাকাউন্ট থেকে টাকা তুলতে চাইলে কেউ কারও অসম্পূর্ণ অবস্থা দেখবে না)।
- **D - Durability (স্থায়িত্ব):** ট্রানজেকশন সফল হলে ডেটা স্থায়ীভাবে ডিস্কে সেভ হবে। এরপর কারেন্ট চলে গেলেও ডেটা হারাবে না।

### ৪. Primary Key বনাম Foreign Key
| বৈশিষ্ট্য | Primary Key | Foreign Key |
|---|---|---|
| প্রধান কাজ | Row-কে ইউনিকভাবে চেনা | অন্য টেবিলের সাথে সম্পর্ক তৈরি করা |
| NULL | হতে পারে না (`NOT NULL`) | NULL হতে পারে |
| প্রতি টেবিলে সংখ্যা | মাত্র একটি | একাধিক থাকতে পারে |
| ইউনিকনেস | অবশ্যই ইউনিক হবে | ইউনিক হওয়া বাধ্যতামূলক নয় |

Foreign Key মূলত **Referential Integrity** রক্ষা করে। অর্থাৎ, যে কাস্টমার আইডি `customers` টেবিলে নেই, সেই আইডি দিয়ে `orders` টেবিলে কোনো অর্ডার তৈরি করা যাবে না।

**এক নজরে:**
| ধারণা | এক লাইনে |
|---|---|
| Normalization | Redundancy কমানো - আলাদা টেবিল তৈরি + Foreign Key ব্যবহার |
| 1NF/2NF/3NF | The Key, The Whole Key, Nothing But The Key |
| ACID | Atomicity, Consistency, Isolation, Durability |
| Primary / Foreign Key | ইউনিক পরিচয় vs সম্পর্ক তৈরি |

Practice: e-commerce DB design (users/products/orders normalize করা), library system 3NF-এ ভাঙা, 1731. Employees Reporting (self-referencing FK), ACID লঙ্ঘনের scenario ভাবা।

---

## Practice Questions (WellDev-এ সরাসরি জিজ্ঞাসিত - Question Bank থেকে)

- Write a SQL query to show only those rows that are repeated (duplicated) in a table. -> 🔗 [182. Duplicate Emails](https://leetcode.com/problems/duplicate-emails/)
- Write a SQL query to show all duplicate rows in a table. -> 🔗 [196. Delete Duplicate Emails](https://leetcode.com/problems/delete-duplicate-emails/)
- Write a SQL query to find the second highest salary. -> 🔗 [176. Second Highest Salary](https://leetcode.com/problems/second-highest-salary/) · [177. Nth Highest Salary](https://leetcode.com/problems/nth-highest-salary/)
- Write a SQL query to list salaries in descending order / ranking. -> 🔗 [178. Rank Scores](https://leetcode.com/problems/rank-scores/) · [184. Department Highest Salary](https://leetcode.com/problems/department-highest-salary/)
- Find the unique column of a database. -> Theory - primary key / unique constraint (see ওপরে)
- Explain the order of SQL query execution. -> দেখো Lesson 65-এর "Execute-এর ক্রম" অংশ
- Given a table with redundant data, how would you optimize it? -> Normalization (Lesson 67)
- What are the ACID properties in DBMS? -> দেখো Lesson 67
- Explain the ER Diagram in a relational database. -> Entities (টেবিল), Attributes (কলাম), Relationships (1:1, 1:N, N:M) - visual design tool for database schema
- What is the difference between a LEFT OUTER JOIN and a RIGHT OUTER JOIN? -> দেখো Lesson 66-এর JOIN অংশ
