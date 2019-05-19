---
layout: post
title: "SQL Cheatsheet"
categories:
  - guide
tags:
  - guide
---

Here's a WIP cheatsheet for SQL commands

## Retrieve data:

`SELECT * FROM book`

`SELECT book_id, title from book`

`SELECT book_id, title from book WHERE book_id='B1'`

`SELECT firstname from author WHERE firstname like 'R%'``
'%' is the wildcard character

`SELECT title,pages from book where pages>=290 AND pages <= 300`
or
`SELECT title, pages from book where pages between 290 AND 300`

`SELECT firstname, lastname, country from author where country IN ('AU', 'BR')``

## Sorting Result Set
`SELECT title FROM book ORDER BY title`

`SELECT title FROM book ORDER BY title DESC`

`SELECT title, pages from book ORDER BY 2`

`SELECT * FROM employees ORDER BY DEP_ID DESC, L_NAME DESC;``

## Grouping Result Set
`SELECT DISTINCT(country) FROM author`

Group author by country and display the per-country count:
`SELECT country, count(country) AS count FROM author GROUP BY country`

Only display the countries who have 5 or more authors. HAVING clause works with group by
`SELECT country, count(country) AS count FROM author GROUP BY country HAVING count(country) > 4`

## JOIN
Inner join: Intersection. Only the rows that match the join criteria
```
SELECT B.borrower_ID, B.LastNAME, B.CONTRY, L.BORROWER_ID, L.LOAD_DATE
FROM BORROWER B
INNER JOIN LOAD L ON B.BORROWER_ID = L.BORROWER_ID
```

Left outer join: All rows from the left table and any matching rows from the right table

Right outer join: All rows from the right able and any matching rows from the left table

Full join: All rows from both table


**References:**
