# MySQL-notes

FLOAT // 7 digits accuracy

CREATE DATABASE databasename;
CREATE TABLE <table_name> (<column_name> <data_type> NOT NULL DEFAULT <deafult_name_as_you_want>, <2nd_column_name> <2nd_data_type>, id INT NOT NULL AUTO_INCREMENT, ......);

CREATE TABLE unique_cats2 (
    cat_id INT NOT NULL AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    age INT,
    PRIMARY KEY (cat_id)
);

SELECT DATABASE();  // show the current database

SHOW DATABASES;
SHOW TABLES;
DESC <table>;  // describe the set of datas

DROP <databaseName>;
DROP <table>;

INSERT INTO cats(name, age)
VALUES ('Jetson', 7), ("Jane", "20");

SELECT * FROM <table_name>;  // * for all
SELECT cat_id, name, age, breed FROM cats;
SELECT name AS 'cat name', breed AS 'kitty breed' FROM cats;

UPDATE cats SET breed='Shorthair' WHERE breed='Tabby';
UPDATE cats SET age=14 WHERE name='Misty';

DELETE FROM cats WHERE name='egg';
DELETE FROM cats WHERE cat_id=age;

SELECT
    CONCAT(author_fname, ' ', author_lname)
FROM books;

SELECT 
    CONCAT_WS(' - ', title, author_fname, author_lname) 
FROM books;  // outputs separated by "-"

SELECT SUBSTRING('Hello World', -3); // output "rld"
SELECT SUBSTRING('Hello World', 3, 8); // output "llo Worl"

SELECT CONCAT
    (
        SUBSTRING(title, 1, 10),
        '...'
    ) AS 'short title'
FROM books;

SELECT
    REPLACE('cheese bread coffee', ' ', ' and ');
// output "cheese and bread and coffee"

SELECT REVERSE('Hello World'); // output "dlroW olleH"

SELECT CHAR_LENGTH('Hello World'); // output 11

SELECT UPPER('Hello World');  // output "HELLO WORLD"
SELECT LOWER('Hello World'); // output "hello world"
// both only take one argument

SELECT DISTINCT author_fname, author_lname FROM books;
// outputs will be distinct full names

SELECT author_lname FROM books ORDER BY author_lname DESC;
// DESC descending / ASC ascending
SELECT title, author_fname, author_lname 
FROM books ORDER BY 2; // 2 means 2nd argument(author_fname)

SELECT title, released_year FROM books 
ORDER BY released_year DESC LIMIT 10,1;

SELECT title, author_fname FROM books WHERE author_fname LIKE '%da%';
SELECT title, stock_quantity FROM books WHERE stock_quantity LIKE '__';

SELECT MAX(pages), title
FROM books;

SELECT
  CONCAT(author_fname, ' ', author_lname) AS author,
  MAX(pages) AS 'longest book'
FROM books
GROUP BY author_lname,
         author_fname;

SELECT author_fname,
       author_lname,
       SUM(pages)
FROM books
GROUP BY
    author_lname,
    author_fname;

SELECT released_year, AVG(stock_quantity) 
FROM books 
GROUP BY released_year;

SELECT CONCAT(author_fname, ' ', author_lname) FROM books
WHERE pages = (SELECT Max(pages) FROM books);
// BELOW ONE IS MORE EFFICIENT	 
SELECT CONCAT(author_fname, ' ', author_lname) FROM books
ORDER BY pages DESC LIMIT 1;

SELECT DATE_FORMAT(birthdt, 'Was born on a %W') FROM people;
SELECT DATE_FORMAT(birthdt, '%m/%d/%Y at %h:%i') FROM people;

SELECT MONTH() // 2015.5.5 -> 5
SELECT MONTHNAME() // 2015.5.5 -> May

SELECT DATEDIFF(NOW(), birthdate) FROM people;
SELECT birthdt, DATE_ADD(birthdt, INTERVAL 3 QUARTER) FROM people;
SELECT birthdt, birthdt + INTERVAL 15 MONTH + INTERVAL 10 HOUR FROM people; // or minus "-"

CREATE TABLE comments2 (
    content VARCHAR(100),
    changed_at TIMESTAMP DEFAULT NOW() ON UPDATE CURRENT_TIMESTAMP
); // TIMESTAMP is used for auto update

AND / &&
OR / ||
WHERE released_year BETWEEN 2004 AND 2015 // BETWEEN min AND max
NOT BETWEEN
IN
WHERE author_lname IN ('Carver', 'Lahiri', 'Smith');
NOT IN
WHERE released_year NOT IN (2003, 2006, 2008)
%
WHERE released_year % 2 = 0
CASE
SELECT title, stock_quantity,
    CASE 
        WHEN stock_quantity BETWEEN 0 AND 50 THEN '*'
        WHEN stock_quantity BETWEEN 51 AND 100 THEN '**'
        WHEN stock_quantity BETWEEN 101 AND 150 THEN '***'
        ELSE '****'
    END AS STOCK
FROM books;

SELECT title FROM books WHERE title NOT LIKE 'W%';
SELECT title, stock_quantity FROM books WHERE stock_quantity >= 100;
SELECT "h" < "q"; // true
SELECT "Q" = "q"; // true

SELECT * FROM customers
JOIN orders
    ON customers.id = orders.customer_id;

SELECT 
    first_name, 
    last_name,
    IFNULL(SUM(amount), 0) AS total_spent
FROM customers
LEFT JOIN orders
    ON customers.id = orders.customer_id
GROUP BY customers.id
ORDER BY total_spent;
