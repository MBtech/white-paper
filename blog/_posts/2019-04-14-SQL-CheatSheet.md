---
layout: post
title: "SQL Cheatsheet"
categories:
  - guide
tags:
  - guide
---

Here's a WIP cheatsheet for SQL commands

Updated 01-07-2019
## Retrieve data:
```sql
SELECT * FROM book

SELECT book_id, title from book

SELECT book_id, title from book WHERE book_id='B1'

SELECT firstname from author WHERE firstname like 'R%'
-- '%' is the wildcard character

SELECT title,pages from book where pages>=290 AND pages <= 300
-- or
SELECT title, pages from book where pages between 290 AND 300

SELECT firstname, lastname, country from author where country IN ('AU', 'BR')
```

## Sorting Result Set
```sql
SELECT title FROM book ORDER BY title

SELECT title FROM book ORDER BY title DESC

SELECT title, pages from book ORDER BY 2

SELECT * FROM employees ORDER BY DEP_ID DESC, L_NAME DESC;
```

## Grouping Result Set
```sql
SELECT DISTINCT(country) FROM author
```

Group author by country and display the per-country count:
```sql
SELECT country, count(country) AS count
FROM author GROUP BY country
```

Only display the countries who have 5 or more authors. HAVING clause works with group by

```sql
SELECT country, count(country) AS count
FROM author GROUP BY country HAVING count(country) > 4
```
## JOIN
Inner join: Intersection. Only the rows that match the join criteria

```sql
SELECT B.borrower_ID, B.LastNAME, B.CONTRY, L.BORROWER_ID, L.LOAD_DATE
FROM BORROWER B
INNER JOIN LOAD L ON B.BORROWER_ID = L.BORROWER_ID
```

Left outer join: All rows from the left table and any matching rows from the right table

Right outer join: All rows from the right able and any matching rows from the left table

Full join: All rows from both table

## TOP SELECT
Select the second highest entry by getting the top 2 and then wrap in a select to pick top 1:

```sql
SELECT TOP 1 *
FROM (
   SELECT TOP 2 *
   FROM table
   WHERE <criteria match>
   ORDER BY amount DESC
) AS newTable
ORDER BY amount ASC
```
**References:**
