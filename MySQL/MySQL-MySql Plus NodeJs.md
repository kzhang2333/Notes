# MySQL + NodeJs
## Install mysql & faker Package
`npm install mysql --save`

`npm install faker`
faker pachage can generate random fake data for testing.
#### Usage of faker
```javascript
// Print a random email
console.log(faker.internet.email());
// Print a random past date
console.log(faker.date.past());
// Print a random city
console.log(faker.address.city());
```
## Selecting with JS

#### To SELECT all users from database:
```javascript
var q = 'SELECT * FROM users ';
connection.query(q, function (error, results, fields) {
  if (error) throw error;
  console.log(results);
});
```

#### To count the number of users in the database:
```javascript
var q = 'SELECT COUNT(*) AS total FROM users ';
connection.query(q, function (error, results, fields) {
  if (error) throw error;
  console.log(results[0].total);
});
```
> Notice that in "result[0].total", "total" is defined variable name in query by "AS total"

## Inserting with JS
#### Inserting by raw query
```javascript
var q = 'INSERT INTO users (email) VALUES ("rusty_the_dog@gmail.com")';

connection.query(q, function (error, results, fields) {
  if (error) throw error;
  console.log(results);
});
```
#### Inserting by mysql package defined method
```javascript
var person = {
    email: faker.internet.email(),
    created_at: faker.date.past()
};

var end_result = connection.query('INSERT INTO users SET ?', person, function(err, result) {
  if (err) throw err;
  console.log(result);
});
```
> Notice that, 'INSERT INTO users SET ?' is not a query language, it is from mysql package to make inserting easier.

## Bulk Inserting
```javascript
// put raw data into a array
var data = [];
for(var i = 0; i < 500; i++){
    data.push([
        faker.internet.email(),
        faker.date.past()
    ]);
}
// use speical defined function from mysql package
var q = 'INSERT INTO users (email, created_at) VALUES ?';
connection.query(q, [data], function(err, result) {
  console.log(err);
  console.log(result);
});
```
> Notice that  "[data]" with square brackets is a flag that tells package inside brackets is a array of arrays, iterativly insert every element in this array using the query I wrote for you.

## Practice with 500 users
Nothing to do with NodeJs or javascript. Pure MySQL query Practice.
```sql
-- Firest user
SELECT
    DATE_FORMAT(MIN(created_at), "%M %D %Y") as earliest_date
FROM users;

-- Find email of first user
SELECT
    email,
    created_at
FROM users
WHERE created_at = (SELECT MIN(created_at) FROM users);

-- OR
SELECT * FROM users ORDER BY created_at LIMIT 1;

-- count users number joined in each month
SELECT
    monthname(created_at) AS month,
    COUNT(*)
FROM users
GROUP BY month DESC;

-- count number of users with yahoo email
SELECT COUNT(*) FROM users WHERE email LIKE ('%@yahoo.com');

-- count users number of different email host
SELECT
    CASE
        WHEN email LIKE ('%@yahoo.com') THEN 'yahoo'
        WHEN email LIKE ('%@gmail.com') THEN 'gmail'
        WHEN email LIKE ('%@hotmail.com') THEN 'stupid'
        ELSE 'other'
    END AS host,
    COUNT(*) AS total_user
FROM users
GROUP BY host
ORDER BY total_user DESC;
```
