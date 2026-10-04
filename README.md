# Employee Performance & HR Analytics

![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=for-the-badge)
![SQL](https://img.shields.io/badge/SQL-Analysis-orange?style=for-the-badge)
![Power BI](https://img.shields.io/badge/Power_BI-Dashboard-F2C811?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-In_Progress-yellow?style=for-the-badge)
![Data](https://img.shields.io/badge/Data-Synthetic-lightgrey?style=for-the-badge)

An end-to-end HR analytics project: a relational database built in **MySQL**, populated with realistic workforce data, and visualised in a **Power BI** dashboard.

> **Project status: ongoing.** The database layer is complete. The analysis queries and the Power BI dashboard are still being developed and refined. See the [Roadmap](#roadmap).

---

## Table of Contents

- [Project Overview](#project-overview)
- [Tech Stack](#tech-stack)
- [Database Design](#database-design)
- [Dataset Summary](#dataset-summary)
- [SQL Concepts Used](#sql-concepts-used)
- [Screenshots](#screenshots)
- [Sample Query and Output](#sample-query-and-output)
- [Power BI Dashboard](#power-bi-dashboard)
- [Roadmap](#roadmap)
- [How to Run](#how-to-run)
- [Repository Structure](#repository-structure)

---

## Project Overview

The goal is to answer common people-analytics questions for a mid-sized company:

- How is headcount distributed across departments and locations?
- How do salaries compare between departments?
- How many employees have left, and from which teams?
- How do performance scores trend year over year?
- What do attendance and remote-work patterns look like?

---

## Tech Stack

| Layer | Tool |
|---|---|
| Database | MySQL 8.0 (Community Server) |
| Querying | MySQL Shell, SQL |
| Visualisation | Power BI Desktop |
| Version Control | Git and GitHub |

---

## Database Design

The database is named `hr_analytics` and has four tables.

| Table | Purpose | Rows |
|---|---|---|
| `department_hierarchies` | Departments with a self-referencing parent department and location | 10 |
| `employee_records` | Employee master data with a self-referencing manager | 100 |
| `performance_reviews` | Yearly performance scores per employee (2023 to 2025) | 244 |
| `attendance_logs` | Daily attendance and hours worked (September 2025) | 1,870 |

**Relationships**

- `department_hierarchies.parent_dept_id` references `department_hierarchies.dept_id`
- `employee_records.dept_id` references `department_hierarchies.dept_id`
- `employee_records.manager_id` references `employee_records.emp_id`
- `performance_reviews.emp_id` and `reviewer_id` reference `employee_records.emp_id`
- `attendance_logs.emp_id` references `employee_records.emp_id`

---

## Dataset Summary

The data is **synthetic** and was generated with SQL for learning and portfolio purposes.

- 10 departments across Mumbai, Pune, Delhi and Bengaluru
- 100 employees: 85 Active, 9 Resigned, 6 Terminated
- Executive team, department heads, managers and individual contributors
- 244 performance reviews with scores between 1.0 and 5.0
- 1,870 attendance records with Present, Absent, Leave and Remote statuses

**Headcount and average salary by department**

| Department | Headcount | Avg. Salary |
|---|---|---|
| Sales | 14 | 860,714 |
| Software Development | 14 | 1,086,429 |
| Customer Support | 13 | 497,692 |
| Finance | 11 | 954,545 |
| Operations | 11 | 897,273 |
| Information Technology | 10 | 1,021,000 |
| Marketing | 10 | 887,000 |
| Human Resources | 8 | 890,000 |
| Recruitment | 8 | 678,750 |
| Executive | 1 | 9,000,000 |

---

## SQL Concepts Used

- Database and table creation with primary and foreign keys
- Self-referencing foreign keys for department and manager hierarchies
- `CHECK` constraints (gender, performance score range)
- `ENUM` data types for employment and attendance status
- Bulk `INSERT` and multi-row inserts
- `UPDATE` with `CASE` to assign managers
- String functions (`CONCAT`, `LOWER`) to generate emails
- Recursive CTE (`WITH RECURSIVE`) to build a date series
- `INSERT ... SELECT` with joins and derived tables to generate reviews and attendance
- Aggregations with `GROUP BY`, `COUNT`, `AVG`, `ROUND`

---

## Screenshots

### 1. Database setup and department table

![Database setup](<img width="1920" height="1080" alt="1st" src="https://github.com/user-attachments/assets/083ebd29-24d8-4305-8377-cb14daa776c2" />)

### 2. Employee records and performance review tables

![Employee and review tables](<img width="1920" height="1080" alt="2nd" src="https://github.com/user-attachments/assets/3ff36808-7014-4498-b6ba-c08551d5b523" />)

### 3. Attendance table and department data insert

![Attendance table and department inserts](<img width="1920" height="1080" alt="3rd" src="https://github.com/user-attachments/assets/6fdecc5f-8fa1-4c1f-8014-f527c3523306" />)

### 4. Employee data insert (part 1)

![Employee inserts part 1](<img width="1920" height="1080" alt="4th" src="https://github.com/user-attachments/assets/3ae35969-0b6a-46d6-afc7-992b66693277" />)

### 5. Manager hierarchy, emails and performance review generation

![Manager hierarchy and reviews](<img width="1920" height="1080" alt="5th" src="https://github.com/user-attachments/assets/5d9eb69d-9b70-4b71-a793-f33c496c840c" />)

### 6. Performance reviews and attendance log generation

![Reviews and attendance generation](<img width="1920" height="1080" alt="6th" src="https://github.com/user-attachments/assets/ee618a25-d0c3-40dc-b4bc-78372753b6a3" />)

### 7. Data validation queries

![Validation queries](<img width="1920" height="1080" alt="7th" src="https://github.com/user-attachments/assets/5792d36a-ce99-4704-ac44-19e03c1396d8" />)

### 8. Headcount and salary by department

![Department analysis](<img width="1920" height="1080" alt="8th" src="https://github.com/user-attachments/assets/db725369-7fa0-4bcb-ac70-2de3935508fb" />)

---

## Sample Query and Output

```sql
SELECT d.dept_name,
       COUNT(*) AS headcount,
       ROUND(AVG(e.annual_salary)) AS avg_salary
FROM employee_records e
JOIN department_hierarchies d USING (dept_id)
GROUP BY d.dept_name
ORDER BY headcount DESC;
```

Attrition check:

```sql
SELECT employment_status, COUNT(*)
FROM employee_records
GROUP BY employment_status;
```

| employment_status | COUNT(*) |
|---|---|
| Active | 85 |
| Resigned | 9 |
| Terminated | 6 |

---

## Power BI Dashboard

The dashboard file is included in the repository: `Power_Bi_dashboard.pbix`

The dashboard is a work in progress. Planned views:

- Workforce overview (headcount, attrition, average salary)
- Department and location breakdown
- Performance score trends by year and department
- Attendance and remote-work analysis

---

## Roadmap

- [x] Design database schema
- [x] Create tables with constraints and relationships
- [x] Load sample data (departments, employees)
- [x] Generate performance reviews and attendance logs
- [x] Validate row counts and basic aggregations
- [ ] Write advanced analysis queries (attrition rate, tenure, top performers)
- [ ] Add window functions and manager-level reporting
- [ ] Connect MySQL to Power BI
- [ ] Complete dashboard pages and DAX measures
- [ ] Add dashboard screenshots to this README
- [ ] Document key insights and findings

---

## How to Run

1. Install MySQL 8.0 and MySQL Shell.
2. Connect to your server:
   ```
   \connect <username>@localhost:3306
   ```
3. Run the SQL script in the `sql/` folder (it drops and recreates `hr_analytics`, builds the tables and loads the data).
4. Verify the load:
   ```sql
   SELECT COUNT(*) FROM employee_records;     -- 100
   SELECT COUNT(*) FROM performance_reviews;  -- 244
   SELECT COUNT(*) FROM attendance_logs;      -- 1870
   ```
5. Open `Power_Bi_dashboard.pbix` in Power BI Desktop and point the data source to your local MySQL instance.

---

## Repository Structure

```
employee-performance-hr-analytics/
|-- sql/
|   `-- hr_analytics.sql
|-- screenshots/
|   |-- 1st.png
|   |-- ...
|   `-- 8th.png
|-- Power_Bi_dashboard.pbix
`-- README.md
```

---

## Author

**Tandrima**

Feedback and suggestions are welcome while the project is in progress.
