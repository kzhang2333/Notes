# MySql - Relationships
## Real Word data is Messsy
> Real world Data is **Messsy** and **Interrelated** >

For example, Books, Authors, Versions.....
- Books can have multiple Authors
- Books can have multiple Versions
- Books are related with Orders, Customers, Reviews

## Relationships Basics
> Different ways data can be realated

1. One to One Relationship
2. One to Many Relationship  **<----Most common**
 For example, books (One) to Reviews (Many)
3. Many to Many Relationship

## 1 : MANY
#### Example: Customers and Orders
A customer can have many orders. But each order can only has one customer.
We want to store
- customer's first and last name - varchar
- customer's email - carchar
- date of purchase - datetime
- price of the order - decimal

Disadvantage of put everything together: too many duplicate data; data are not separated...

### Foreign Key
Foreign Key is key refer to a row in other table.

### Join
#### Inner Join
Select all records from A and B where the join condition is met

#### Left Join
Select everything from A, along with any matching records in B

#### Right join
Select everything from B, along with any matching records in A

> Notice: tableA LEFT JOIN tableB is equivalent with tableB RIGHT JOIN tableA

## 1 : MANY 实操
### 数据：学生，论文成绩
```sql
INSERT INTO students (first_name) VALUES
('Caleb'), ('Samantha'), ('Raj'), ('Carlos'), ('Lisa');

INSERT INTO papers (student_id, title, grade ) VALUES
(1, 'My First Book Report', 60),
(1, 'My Second Book Report', 75),
(2, 'Russian Lit Through The Ages', 94),
(2, 'De Montaigne and The Art of The Essay', 98),
(4, 'Borges and Magical Realism', 89);
```
### Table Schema
```sql
CREATE TABLE students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(100)
);

CREATE TABLE papers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(100),
    grade DECIMAL,
    student_id INT,
    FOREIGN KEY (student_id) REFERENCES students(id)
    ON DELETE CASCADE
);
```
### Problem # 1
```sql
SELECT first_name, title, grade
FROM students
JOIN papers
    ON students.id = papers.student_id
ORDER BY papers.grade DESC;
```
### problem # 2
```sql
SELECT first_name, title, grade
FROM students
LEFT JOIN papers
    ON students.id = papers.student_id;
```
### problem # 3
```sql
SELECT
    first_name,
    IFNULL(title, 'Missing'),
    IFNULL(grade, 0)
FROM students
LEFT JOIN papers
    ON students.id = papers.student_id;
```
### problem # 4
```sql
SELECT
    first_name,
    IFNULL(AVG(grade), 0) as average
FROM students
LEFT JOIN papers
    ON students.id = papers.student_id
GROUP BY student.id
ORDER BY AVG(grade) DESC;
```
### Problem # 5
```sql
SELECT
    first_name,
    IFNULL(AVG(grade), 0) as average,
    CASE
        WHEN AVG(grade) < 75 || grade IS NULL THEN 'FAILING'
        ELSE 'PASSING'
    END AS passing_status
FROM students
LEFT JOIN papers
    ON students.id = papers.student_id
GROUP BY students.id
ORDER BY AVG(grade) DESC;
```
