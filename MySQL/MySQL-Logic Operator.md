# MySql Basics - Logic Operator

## Logic Operator
### Somethng Old
1. ! : NOT
2. \> : greater
3. < : smaller
4. = : equal
7. \>= , <= : greater or equal, smaller or equal
5. && : AND
6. || : OR
7. % : MODULO

#### Select books that were NOT released in 2017
```sql
SELECT title FROM books
WHERE year != 2017;
```
#### Select books with titles that DON'T start with 'W'
```sql
SELECT title FROM books
WHERE title NOT LIKE 'W%';
``
#### Select books released AFTER the year 2000
```sql
SELECT * FROM books
WHERE released_year > 2000;
```
#### AND
```sql
SELECT * FROM books
WHERE author_lname='Eggers' AND
released_year > 2010 AND
title LIKE '%novel%';
```
#### OR
```sql
SELECT * FROM books
WHERE author_lname='Eggers' ||
released_year > 2010;
```

### Somthing New
> Note for Vodka: **IN 和 BETWEEN都是逻辑判断！不是某种操作**
#### TRUE and FALSE in MySql
`SELECT 99 > 1` will return 1. TRUE is 1 and FALSE is 0 in MySql.
> Notice: in MySql, it's not case sensitive. So `'a' = 'A'` will return 1

#### BETWEEN - between two data(both inclusive)
```sql sql
SELECT title, released_year FROM
books WHERE released_year >= 2004 &&
released_year <= 2015;
```
is equivalent with
 ```sql
 SELECT title, released_year FROM books
 WHERE released_year BETWEEN 2004 AND 2015;
 ```
We can also use NOT with BETWEEN
```sql
SELECT title, released_year FROM books
WHERE released_year NOT BETWEEN 2004 AND 2015;
```
> Notice that, when compare data in MySql, it's always better to cast them into same type of data, then compare, for example:
```sql
SELECT
    name,
    birthdt
FROM people
WHERE
    birthdt BETWEEN CAST('1980-01-01' AS DATETIME)
    AND CAST('2000-01-01' AS DATETIME);
```
#### IN - In a certain set of data
For example, select all books written by Carver, Lahiri, or Smith:
```sql
SELECT title, author_lname FROM books
WHERE author_lname='Carver' OR
      author_lname='Lahiri' OR
      author_lname='Smith';
```
is equivalent with
```sql
SELECT title, author_lname FROM books
WHERE author_lname IN ('Carver', 'Lahiri', 'Smith');
```
In another example, we use NOT with IN:
```sql
SELECT title, released_year FROM books
WHERE released_year NOT IN
(2000,2002,2004,2006,2008,2010,2012,2014,2016);
```
and, so we know, this can be done easier with `%`
```sql
SELECT title, released_year FROM books
WHERE released_year % 2 != 0;
```
#### Case statement
Use **Case statement** when selecting from table.
```sql
SELECT title, stock_quantity,
    CASE
        WHEN stock_quantity BETWEEN 0 AND 50 THEN '*'
        WHEN stock_quantity BETWEEN 51 AND 100 THEN '**'
        ELSE '***'
    END AS STOCK
FROM books;   
```
will generate this:
```
+-----------------------------------------------------+----------------+-------+
| title                                               | stock_quantity | STOCK |
+-----------------------------------------------------+----------------+-------+
| The Namesake                                        |             32 | *     |
| Norse Mythology                                     |             43 | *     |
| American Gods                                       |             12 | *     |
| Interpreter of Maladies                             |             97 | **    |
| A Hologram for the King: A Novel                    |            154 | ***   |
| The Circle                                          |             26 | *     |
| The Amazing Adventures of Kavalier & Clay           |             68 | **    |
| Just Kids                                           |             55 | **    |
| A Heartbreaking Work of Staggering Genius           |            104 | ***   |
| Coraline                                            |            100 | **    |
| What We Talk About When We Talk About Love: Stories |             23 | *     |
| Where I'm Calling From: Selected Stories            |             12 | *     |
| White Noise                                         |             49 | *     |
| Cannery Row                                         |             95 | **    |
| Oblivion: Stories                                   |            172 | ***   |
| Consider the Lobster                                |             92 | **    |
| 10% Happier                                         |             29 | *     |
| fake_book                                           |            287 | ***   |
| Lincoln In The Bardo                                |           1000 | ***   |
+-----------------------------------------------------+----------------+-------+
```
> Notice that: **STOCK** in the table is determined by rules given by case statement when selecting table. And it is **dynamic!**

.
