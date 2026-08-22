# WellDev Interview Prep — DBMS ও SQL — উত্তরসহ

> Source: CPS Academy — "WellDev Interview Prep — Bangla" course, Module 15 (SQL ও Database), Lessons 65–67.

## সূচি
- SQL Basics ও Query (SELECT, WHERE, GROUP BY, HAVING, execute order, DISTINCT, NULL)
- JOIN ও Duplicate খোঁজা (JOIN types, duplicate rows, Nth highest salary, subquery)
- Normalization ও ACID (1NF/2NF/3NF, ACID, Primary/Foreign Key)

---

## Lesson 65 — SQL Basics ও Query (Database-এর ভাষা)

Sample table `employees(id, name, department, salary)` ধরে নিচের ধারণাগুলো:

**১. Basic SELECT**
```sql
SELECT name, salary FROM employees
WHERE department = 'Eng'
ORDER BY salary DESC;
```
SELECT=column বাছা, FROM=টেবিল, WHERE=row filter, ORDER BY=সাজানো (DESC=বড়→ছোট, ASC=ছোট→বড়, default ASC)। `SELECT *`=সব column; `LIMIT n`=প্রথম n row; একাধিক শর্ত `AND`/`OR` দিয়ে।

**২. GROUP BY ও Aggregate**
```sql
SELECT department, AVG(salary) AS avg_salary
FROM employees GROUP BY department
HAVING AVG(salary) > 55000;
```
Aggregate function: COUNT(), SUM(), AVG(), MAX(), MIN()। GROUP BY একই মানের row একসাথে জড়ো করে প্রতি group-এ aggregate হিসাব করে।
**WHERE vs HAVING:** WHERE group করার **আগে** row filter করে (aggregate WHERE-এ চলে না); HAVING group করার **পরে** group filter করে (aggregate-এর শর্ত এখানে দিতে হয়)।

**৩. SQL Execute Order (গুরুত্বপূর্ণ, প্রায়ই জিজ্ঞেস করা)**
লেখার ক্রম: `SELECT → FROM → WHERE → GROUP BY → HAVING → ORDER BY`
**Execute-এর আসল ক্রম:** `FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT`
এটা ব্যাখ্যা করে:
- WHERE-এ SELECT-এর column alias ব্যবহার করা যায় না (SELECT পরে execute হয়, alias তখনো তৈরি হয়নি) — কিন্তু ORDER BY-তে alias চলে (ORDER BY SELECT-এর পরে execute হয়)।
- WHERE-এ aggregate function চলে না (GROUP BY-এর আগে execute হয়, তখনো group নেই) — তাই HAVING লাগে।

**৪. DISTINCT ও NULL**
- `SELECT DISTINCT department` — ডুপ্লিকেট বাদ দিয়ে unique মান।
- NULL মানে "মান নাই/অজানা" (0 বা খালি string না)। `= NULL` কখনো true হয় না — `IS NULL` / `IS NOT NULL` ব্যবহার করতে হয়। NULL-সহ যেকোনো গণনা → NULL। `COUNT(column)` NULL বাদ দিয়ে গোনে, `COUNT(*)` সব row গোনে।
- ফাঁদ: `WHERE department != 'Eng'` — department NULL হলে সেই row আসবে না (তুলনা true হয় না), আলাদা `OR department IS NULL` দিতে হয়।

**এক নজরে:**
| ধারণা | এক লাইনে |
|---|---|
| SELECT/FROM/WHERE/ORDER BY | column বাছা, টেবিল, filter, সাজানো |
| GROUP BY | একই মান জড়ো + aggregate |
| WHERE vs HAVING | row-এ (আগে) vs group-এ (পরে) |
| Execute ক্রম | FROM→WHERE→GROUP BY→HAVING→SELECT→ORDER BY→LIMIT |
| DISTINCT | ডুপ্লিকেট বাদ |
| NULL | মান নাই; `IS NULL` (= NULL চলে না) |

Practice: LeetCode SQL — 595 (Big Countries), 1757 (multi-condition WHERE), 596 (GROUP BY+HAVING), 1741 (GROUP BY+SUM)

---

## Lesson 66 — JOIN ও Duplicate খোঁজা (একাধিক টেবিল জোড়া)

**১. JOIN ও ধরন**
JOIN দুইটা টেবিলকে একটা common column দিয়ে জোড়ে। চার ধরন:
- **INNER JOIN** — দুই টেবিলে মিল আছে এমন row-ই আসে।
- **LEFT JOIN** — বাম টেবিলের সব row + ডান-এ মিল থাকলে তা, না থাকলে NULL।
- **RIGHT JOIN** — ডান টেবিলের সব + বাম-এর মিল।
- **FULL JOIN** — দুই টেবিলের সব।
```sql
SELECT e.name, d.dept_name FROM employees e
INNER JOIN departments d ON e.dept_id = d.id;   -- মিল আছে যা শুধু
-- LEFT JOIN হলে dept_id NULL থাকা employee-ও আসবে, dept_name NULL সহ
```

**২. Duplicate খোঁজা**
```sql
SELECT email, COUNT(*) AS cnt FROM users
GROUP BY email HAVING COUNT(*) > 1;
```
GROUP BY দিয়ে একই email জড়ো, COUNT দিয়ে গোনা, HAVING >1 দিয়ে duplicate বাছা। **HAVING লাগে, WHERE না** — কারণ COUNT aggregate, group-এর পরে হিসাব হয়।
Duplicate মুছতে (একটা রেখে):
```sql
DELETE FROM users WHERE id NOT IN (SELECT MIN(id) FROM users GROUP BY email);
```

**৩. দ্বিতীয় সর্বোচ্চ Salary (Classic!)**
উপায় ১ (subquery):
```sql
SELECT MAX(salary) AS second_highest FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```
উপায় ২ (DISTINCT + ORDER BY + OFFSET):
```sql
SELECT DISTINCT salary FROM employees
ORDER BY salary DESC LIMIT 1 OFFSET 1;
```
DISTINCT জরুরি (নাহলে duplicate সর্বোচ্চ salary ভুল ফল দেবে)। N-তম সর্বোচ্চ চাইলে `OFFSET N-1`। Edge case: দ্বিতীয় সর্বোচ্চ না থাকলে (সবার salary সমান) — subquery-পদ্ধতি নিরাপদে NULL দেয়, OFFSET-পদ্ধতি খালি result দেয়।

**৪. Subquery**
Query-র ভিতরে query — ভিতরের ফল বাইরে ব্যবহৃত হয়। তিন জায়গায় ব্যবহার: WHERE (filter), FROM (derived table), SELECT (একটা মান)।
```sql
SELECT name, salary FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);  -- গড়ের বেশি যারা
```
Subquery vs JOIN — অনেক ক্ষেত্রে একই কাজ করা যায়; JOIN সাধারণত দ্রুত (বড় data-তে), subquery কখনো পড়তে সহজ। **Correlated subquery** — ভিতরের query বাইরের প্রতি row-এর উপর নির্ভর করে (row-ভিত্তিক execute, শক্তিশালী কিন্তু ধীর)।

**এক নজরে:**
| ধারণা | এক লাইনে |
|---|---|
| INNER JOIN | দুই টেবিলে মিল আছে যা |
| LEFT JOIN | বাম-এর সব + ডান-এর মিল (নাহলে NULL) |
| Duplicate খোঁজা | GROUP BY + COUNT(*) + HAVING > 1 |
| ২য় সর্বোচ্চ | subquery (MAX < MAX) বা DISTINCT+ORDER+OFFSET |
| Subquery | query-র ভিতরে query; WHERE/FROM/SELECT-এ |

Practice: LeetCode SQL — 175 (Combine Two Tables, LEFT JOIN), 181 (Employees Earning More Than Managers, self JOIN), 182 (Duplicate Emails), 176 (Second Highest Salary — classic!), 184 (Department Highest Salary, JOIN+subquery)

---

## Lesson 67 — Normalization ও ACID (Database Design ও নির্ভরযোগ্যতা)

**১. Normalization কী ও কেন**
টেবিলগুলোকে এমনভাবে সাজানো যাতে redundancy (একই data বারবার) কমে ও consistency বজায় থাকে — মূলনীতি: "এক data এক জায়গায়"।
সমস্যা (unnormalized টেবিলে, যেমন orders-এ customer_name+phone প্রতি row-এ):
- **Update anomaly** — একই customer-এর phone একাধিক জায়গায় বদলাতে হয় (ভুললে অসামঞ্জস্য)
- **Insert anomaly** — নতুন customer যোগ করতে একটা order লাগে
- **Delete anomaly** — order মুছলে customer তথ্যও হারায়
সমাধান: আলাদা টেবিল (customers, orders) + foreign key দিয়ে জোড়া।

**২. 1NF / 2NF / 3NF**
- **1NF** — প্রতি ঘরে একটা মাত্র atomic মান, কোনো list/repeating group না।
- **2NF** — 1NF + প্রতিটা non-key column পুরো (composite) primary key-এর উপর নির্ভর করে, শুধু অংশের উপর না (partial dependency নিষেধ)।
- **3NF** — 2NF + কোনো non-key column আরেকটা non-key column-এর উপর নির্ভর না (transitive dependency নিষেধ)।
মনে রাখার সূত্র: প্রতিটা column নির্ভর করে **"the key (1NF), the whole key (2NF), and nothing but the key (3NF)"** এর উপর।
Trade-off: বেশি normalization = বেশি টেবিল = বেশি JOIN (ধীর হতে পারে) — তাই read-heavy system-এ ইচ্ছাকৃত **denormalization** করা হয়।

**৩. ACID (Transaction-এর ৪ গ্যারান্টি)**
Transaction = কয়েকটা operation-এর group যা একসাথে সফল বা একসাথে ব্যর্থ হয়।
- **A — Atomicity**: সব operation একসাথে হয়, নয়তো একটাও না। (ব্যাংক transfer: টাকা কাটা+যোগ — মাঝপথে crash হলে সম্পূর্ণ rollback)
- **C — Consistency**: transaction database-কে এক বৈধ অবস্থা থেকে আরেক বৈধ অবস্থায় নেয়, নিয়ম/constraint ভাঙে না। (transfer-এর আগে-পরে মোট টাকা সমান)
- **I — Isolation**: একসাথে চলা transaction একে অপরকে প্রভাবিত করে না — যেন serially চলছে। (দুইজন একসাথে একই account থেকে তুললে একে অপরের অসম্পূর্ণ অবস্থা দেখে না)
- **D — Durability**: transaction সফল হলে ফল স্থায়ী — crash/power-loss-এও থাকে (disk-এ লেখা)।

**৪. Primary Key vs Foreign Key**
| | Primary Key | Foreign Key |
|---|---|---|
| কাজ | row-কে unique চেনায় | অন্য টেবিলের সাথে সম্পর্ক তৈরি |
| NULL | না (NOT NULL) | হতে পারে |
| প্রতি টেবিলে কয়টা | একটা | একাধিক থাকতে পারে |
| Unique | হ্যাঁ | না (একই মান বহুবার) |
Foreign key **referential integrity** রক্ষা করে — যে customer_id customers টেবিলে নেই, সেটা orders-এ ব্যবহার করা যাবে না।

**এক নজরে:**
| ধারণা | এক লাইনে |
|---|---|
| Normalization | redundancy কমানো — আলাদা টেবিল + foreign key |
| 1NF/2NF/3NF | the key, the whole key, nothing but the key |
| ACID | Atomicity, Consistency, Isolation, Durability |
| Primary/Foreign Key | unique পরিচয় vs সম্পর্ক তৈরি |

Practice (design exercises, coding না): e-commerce DB design (users/products/orders normalize করা), library system 3NF-এ ভাঙা, 1731. Employees Reporting (self-referencing FK), ACID লঙ্ঘনের scenario ভাবা।

---

## Practice Questions (WellDev-এ সরাসরি জিজ্ঞাসিত — Question Bank থেকে)

- Write a SQL query to show only those rows that are repeated (duplicated) in a table. → 🔗 [182. Duplicate Emails](https://leetcode.com/problems/duplicate-emails/)
- Write a SQL query to show all duplicate rows in a table. → 🔗 [196. Delete Duplicate Emails](https://leetcode.com/problems/delete-duplicate-emails/)
- Write a SQL query to find the second highest salary. → 🔗 [176. Second Highest Salary](https://leetcode.com/problems/second-highest-salary/) · [177. Nth Highest Salary](https://leetcode.com/problems/nth-highest-salary/)
- Write a SQL query to list salaries in descending order / ranking. → 🔗 [178. Rank Scores](https://leetcode.com/problems/rank-scores/) · [184. Department Highest Salary](https://leetcode.com/problems/department-highest-salary/)
- Find the unique column of a database. → Theory — primary key / unique constraint (see উপরে)
- Explain the order of SQL query execution. → দেখো Lesson 65-এর "Execute ক্রম" অংশ
- Given a table with redundant data, how would you optimize it? → Normalization (Lesson 67)
- What are the ACID properties in DBMS? → দেখো Lesson 67
- Explain the ER Diagram in a relational database. → Entities (টেবিল), Attributes (column), Relationships (1:1, 1:N, N:M) — visual design tool for database schema
- What is the difference between a LEFT OUTER JOIN and a RIGHT OUTER JOIN? → দেখো Lesson 66-এর JOIN অংশ
