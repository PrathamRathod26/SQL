# SQL 50 LeetCode solutions

## 1. [Combine Two Tables](https://leetcode.com/problems/combine-two-tables/description/)

```sql
select p.firstName, p.lastName, a.city, a.state
from Person p
left join Address a on p.personId = a.personId;
```
---
## 2. [Duplicate Emails](https://leetcode.com/problems/duplicate-emails/description/)

```sql
select email from Person
group by email
having count(email)>1;
```
---
## 3. [Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/description/)

```sql
Select product_id
from Products
where low_fats = 'Y'
and recyclable = 'Y';
```
---
## 4. [Find Customer Referee](https://leetcode.com/problems/find-customer-referee/description/)

```sql
select name
from Customer
where referee_id is null or referee_id != 2;
```
---
## 5. [Big Countries](https://leetcode.com/problems/big-countries/description/)

```sql
select name, population, area
from World
where area>=3000000 or population >= 25000000;
```
---
## 6. [Article Views I](https://leetcode.com/problems/article-views-i/description/)

```sql
select distinct author_id as id
from Views
where author_id = viewer_id 
order by id;
```
### Learning:
* use **distinct** key word to get unique values
---
## 7. [Invalid Tweets](https://leetcode.com/problems/invalid-tweets/description/)

```sql
select tweet_id
from Tweets
where length(content)>15;
```
---
## 8. [Replace Employee ID with The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/description/)

```sql
select e2.unique_id, e1.name
from Employees e1
left join EmployeeUNI e2
on e1.id = e2.id;
```
---
## 9. [Product Sales Analysis I](https://leetcode.com/problems/product-sales-analysis-i/description/)

```sql
select p.product_name, s.year, s.price
from Sales s
left join Product p
on s.product_id = p.product_id;
```
---
## 10. [Customer Who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/description/)

```sql
select v.customer_id, count(v.customer_id) as count_no_trans
from Visits v
left join Transactions t
on v.visit_id = t.visit_id
where transaction_id is null
group by v.customer_id
```
---
## 11. [Rising Temperature](https://leetcode.com/problems/rising-temperature/description/)

```sql
select w1.id
from Weather w1
join Weather w2
on w1.recordDate = w2.recordDate + INTERVAL '1 day'
where w1.temperature>w2.temperature;
```

### Learning:
* use **Interval** keyword to intervals between dates.
---
## 12. [Average Time to Process per Machine](https://leetcode.com/problems/average-time-of-process-per-machine/description/)

```sql
select a.machine_id,
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
 as processing_time
from Activity a
group by machine_id
```
### Learning:
* use **ROUND** keyword for rounding off to the required decimal places.
* you will need to type cast the values into **numeric** before rounding off.
---
## 13. [Employee Bonus](https://leetcode.com/problems/employee-bonus/description/)

```sql
select e.name, b.bonus
from Employee e
left join Bonus b
on e.empId = b.empId
where b.bonus<1000 or b.bonus is null;
```
---
## 14. [Students and Examinations](https://leetcode.com/problems/students-and-examinations/description/)

```sql
select s.student_id, s.student_name, c.subject_name, count(e.student_id) as attended_exams
from Students s
cross join Subjects c
left join Examinations e
on s.student_id = e.student_id and c.subject_name = e.subject_name
group by s.student_id, s.student_name, c.subject_name
order by s.student_id, c.subject_name
```
### learning:
* Since the **CROSS JOIN** defines the result at the (student, subject) level and student_name is selected but not aggregated, we must group by **student_id, student_name, and subject_name** to keep the result deterministic.
---
## 15. [Managers with at least 5 direct reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports/description/)

```sql
select e1.name
from employee e1
left join employee e2
on e1.id = e2.managerId
group by e1.id, e1.name
having count(e2.id)>=5
```
### learning:
* correct query syntax: SELECT -> FROM -> JOIN -> WHERE -> GROUP BY -> HAVING -> ORDER BY -> LIMIT / OFFSET
---
## 16. [Confirmation Rate](https://leetcode.com/problems/confirmation-rate/description/)

```sql
select s.user_id,round(
    avg(
        if(c.action = 'confirmed', 1.00, 0)
    )::NUMERIC, 2
) as confirmation_rate
from Signups s
left join confirmations c on s.user_id = c.user_id
group by s.user_id;
```
---
## 17. [Not Boring Movies](https://leetcode.com/problems/not-boring-movies/description/)

```sql
select * from Cinema
where id%2 = 1 and description != 'boring'
order by rating desc;
```
---
## 18. [Average Selling Price](https://leetcode.com/problems/average-selling-price/description/)

```sql
select s.user_id,round(
    avg(
        if(c.action = 'confirmed', 1.00, 0)
    )::NUMERIC, 2
) as confirmation_rate
from Signups s
left join confirmations c on s.user_id = c.user_id
group by s.user_id;
```
---
## 19. [Not Boring Movies](https://leetcode.com/problems/not-boring-movies/description/)

```sql
select * from Cinema
where id%2 = 1 and description != 'boring'
order by rating desc;
```
---
