# SQL 50 LeetCode solutions

## 1. [Combine Two Tables](https://leetcode.com/problems/combine-two-tables/description/)

```sql
SELECT p.firstName, p.lastName, a.city, a.state
FROM Person p
LEFT JOIN Address a ON p.personId = a.personId;
```
---
## 2. [Duplicate Emails](https://leetcode.com/problems/duplicate-emails/description/)

```sql
SELECT email FROM Person
GROUP BY email
HAVING COUNT(email)>1;
```
---
## 3. [Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/description/)

```sql
SELECT product_id
FROM Products
WHERE low_fats = 'Y'
AND recyclable = 'Y';
```
---
## 4. [Find Customer Referee](https://leetcode.com/problems/find-customer-referee/description/)

```sql
SELECT name
FROM Customer
WHERE referee_id IS NULL OR referee_id != 2;
```
---
## 5. [Big Countries](https://leetcode.com/problems/big-countries/description/)

```sql
SELECT name, population, area
FROM World
WHERE area>=3000000 OR population >= 25000000;
```
---
## 6. [Article Views I](https://leetcode.com/problems/article-views-i/description/)

```sql
SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id 
ORDER BY id;
```
### Learning:
* use **DISTINCT** key word to get unique values
---
## 7. [Invalid Tweets](https://leetcode.com/problems/invalid-tweets/description/)

```sql
SELECT tweet_id
FROM Tweets
WHERE LENGTH(content)>15;
```
---
## 8. [Replace Employee ID with The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/description/)

```sql
SELECT e2.unique_id, e1.name
FROM Employees e1
LEFT JOIN EmployeeUNI e2
ON e1.id = e2.id;
```
---
## 9. [Product Sales Analysis I](https://leetcode.com/problems/product-sales-analysis-i/description/)

```sql
SELECT p.product_name, s.year, s.price
FROM Sales s
LEFT JOIN Product p
ON s.product_id = p.product_id;
```
---
## 10. [Customer Who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/description/)

```sql
SELECT v.customer_id, COUNT(v.customer_id) AS count_no_trans
FROM Visits v
LEFT JOIN Transactions t
ON v.visit_id = t.visit_id
WHERE transaction_id IS NULL
GROUP BY v.customer_id
```
---
## 11. [Rising Temperature](https://leetcode.com/problems/rising-temperature/description/)

```sql
SELECT w1.id
FROM Weather w1
JOIN Weather w2
ON w1.recordDate = w2.recordDate + INTERVAL '1 day'
WHERE w1.temperature>w2.temperature;
```

### Learning:
* use **INTERVAL** keyword to intervals between dates.
---
## 12. [Average Time to Process per Machine](https://leetcode.com/problems/average-time-of-process-per-machine/description/)

```sql
SELECT a.machine_id,
ROUND(
  (
    (SELECT AVG(a1.timestamp) FROM Activity a1
     WHERE a1.activity_type = 'end'
       AND a1.machine_id = a.machine_id)
  -
    (SELECT AVG(a1.timestamp) FROM Activity a1
     WHERE a1.activity_type = 'start'
       AND a1.machine_id = a.machine_id)
  )::NUMERIC,
  3
)
 AS processing_time
FROM Activity a
GROUP BY machine_id
```
### Learning:
* use **ROUND** keyword for rounding off to the required decimal places.
* you will need to type cast the values into **NUMERIC** before rounding off.
---
## 13. [Employee Bonus](https://leetcode.com/problems/employee-bonus/description/)

```sql
SELECT e.name, b.bonus
FROM Employee e
LEFT JOIN Bonus b
ON e.empId = b.empId
WHERE b.bonus<1000 OR b.bonus IS NULL;
```
---
## 14. [Students and Examinations](https://leetcode.com/problems/students-and-examinations/description/)

```sql
SELECT s.student_id, s.student_name, c.subject_name, COUNT(e.student_id) AS attended_exams
FROM Students s
CROSS JOIN Subjects c
LEFT JOIN Examinations e
ON s.student_id = e.student_id AND c.subject_name = e.subject_name
GROUP BY s.student_id, s.student_name, c.subject_name
ORDER BY s.student_id, c.subject_name
```
### learning:
* Since the **CROSS JOIN** defines the result at the (student, subject) level and student_name is selected but not aggregated, we must group by **student_id, student_name, and subject_name** to keep the result deterministic.
---
## 15. [Managers with at least 5 direct reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports/description/)

```sql
SELECT e1.name
FROM employee e1
LEFT JOIN employee e2
ON e1.id = e2.managerId
GROUP BY e1.id, e1.name
HAVING COUNT(e2.id)>=5
```
### learning:
* correct query syntax: SELECT -> FROM -> JOIN -> WHERE -> GROUP BY -> HAVING -> ORDER BY -> LIMIT / OFFSET
---
## 16. [Confirmation Rate](https://leetcode.com/problems/confirmation-rate/description/)

```sql
SELECT s.user_id,ROUND(
    AVG(
        CASE
            WHEN c.action = 'confirmed' THEN 1.00
            ELSE 0
        END
    )::NUMERIC, 2
) AS confirmation_rate
FROM Signups s
LEFT JOIN confirmations c ON s.user_id = c.user_id
GROUP BY s.user_id;
```
---
## 17. [Not Boring Movies](https://leetcode.com/problems/not-boring-movies/description/)

```sql
SELECT * FROM Cinema
WHERE id%2 = 1 AND description != 'boring'
ORDER BY rating DESC;
```
---
## 18. [Average Selling Price](https://leetcode.com/problems/average-selling-price/description/)

```sql
SELECT p.product_id, COALESCE(
    ROUND(SUM(u.units * p.price)/SUM(u.units)::NUMERIC,2),0
) AS average_price
FROM UnitsSold u
FULL JOIN Prices p
    ON p.product_id = u.product_id
    AND u.purchase_date BETWEEN p.start_date AND p.end_date
GROUP BY p.product_id ORDER BY p.product_id
```
### learning:
* use **COALESCE** keyword to replace null values with the required values.
* use **FULL JOIN** keyword to join two tables.
---
## 19. [Project Employees I](https://leetcode.com/problems/project-employees-i/description/)

```sql
SELECT p.project_id, ROUND(
    AVG(e.experience_years),2
) AS average_years
FROM Project p
JOIN Employee e 
ON p.employee_id = e.employee_id
GROUP BY p.project_id
ORDER BY p.project_id
```
---
## 20. [Percentage of Users Attended a Contest](https://leetcode.com/problems/percentage-of-users-attended-a-contest/description/)

```sql
SELECT contest_id, ROUND(
    (
        COUNT(DISTINCT user_id) * 100 / (SELECT COUNT(user_id) from Users)
    )::numeric, 2) AS percentage
FROM Register
GROUP BY contest_id
ORDER BY percentage desc, contest_id 
```
---
## 21. [Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage/description/)

```sql
SELECT
    query_name,
    ROUND(SUM(rating::numeric / position) / COUNT(*), 2) AS quality,
    ROUND(
        SUM(CASE WHEN rating < 3 THEN 1 ELSE 0 END)::numeric * 100 / COUNT(*),
        2
    ) AS poor_query_percentage
FROM Queries
GROUP BY query_name
ORDER BY quality DESC;
```
---
## 22. [Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i/description/)

```sql
SELECT
    TO_CHAR(DATE_TRUNC('MONTH', trans_date), 'YYYY-MM') AS month,
    country,
    COUNT(*) AS trans_count,
    SUM(CASE WHEN state = 'approved' THEN 1 ELSE 0 END) AS approved_count,
    SUM(amount) AS trans_total_amount,
    SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM Transactions
GROUP BY 1,2
```
### learning:
* use **DATE_TRUNC** keyword to truncate the date to the required format.
* use **TO_CHAR** keyword to format the date to the required format.
* use **1,2** in **GROUP BY** to refer to the first and second columns.
---
