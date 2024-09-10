- SQL in the Wizard World | DB Fiddle - SQL Database Playground
  - https://www.db-fiddle.com/f/9Jq8KfBPtcYRh84PnPUQWi/61

``` sql
# SQL Create Table

# CREATE TABLE table_name (
#     column1 datatype,
#     column2 datatype,
#     column3 datatype,
#    ....
# );
CREATE TABLE Students
                (first_name VARCHAR(255),
                last_name VARCHAR(255),
                login VARCHAR(255),
                age INTEGER,
                gpa REAL,
                house VARCHAR(255));

# SQL Insert Into   
                              
# INSERT INTO table_name (column1, column2, column3, ...)
# VALUES (value1, value2, value3, ...);
INSERT 
INTO Students(first_name, last_name, login, age, gpa, house)
VALUES 
('Harry', 'Potter','theboywholived', 15, 4.0, 'Gryffindor'),
('Hermionie', 'Granger','granger2', 15, 4.5, 'Gryffindor'),
('Ron', 'Weasley','weasley7', 15, 3.7, 'Gryffindor'),
('Draco', 'Malfoy','malfoy999', 15, 4.0, 'Slytherin'),
('Cedric', 'Diggory','diggory123', 15, 4.0, 'Hufflepuff');

# SQL Drop Table
# DROP TABLE table_name;
```

``` sql
# SQL Select

# SELECT column1, column2, ...
# FROM table_name;

SELECT first_name, last_name FROM Students;

# SQL Where

# SELECT column1, column2, ...
# FROM table_name
# WHERE condition;

SELECT * FROM Students
WHERE house='Gryffindor';

# SQL And

# SELECT column1, column2, ...
# FROM table_name
# WHERE condition1 AND condition2 AND condition3 ...;

SELECT * FROM Students
WHERE house='Gryffindor' AND gpa>3.8;

# SQL Or

# SELECT column1, column2, ...
# FROM table_name
# WHERE condition1 OR condition2 OR condition3 ...;

SELECT * FROM Students
WHERE house='Slytherin' OR house='Hufflepuff';

# SQL Like

# SELECT column1, column2, ...
# FROM table_name
# WHERE columnN LIKE pattern;

SELECT first_name, last_name FROM Students
WHERE first_name LIKE 'H%';

# SQL Limit

# SELECT column_name(s)
# FROM table_name
# WHERE condition
# LIMIT number;

SELECT * FROM Students LIMIT 3;

# SQL Order By

# SELECT column1, column2, ...
# FROM table_name
# ORDER BY column1, column2, ... ASC|DESC;

SELECT * FROM Students ORDER BY first_name;

# SQL Count

# SELECT COUNT(column_name)
# FROM table_name
# WHERE condition;

SELECT COUNT(first_name) FROM Students;

# SQL Group By

# SELECT column_name(s)
# FROM table_name
# WHERE condition
# GROUP BY column_name(s)
# ORDER BY column_name(s);

SELECT COUNT(first_name), house FROM Students GROUP BY house;
```

- Example | DB Fiddle - SQL Database Playground
  - https://www.db-fiddle.com/f/sWn6RS8GsfRhj5oXwS3Dso/0

``` sql
DROP TABLE IF EXISTS table_a;

CREATE TABLE table_a (
	email VARCHAR(50)
)
;

INSERT INTO table_a VALUES
	('a@test.de')
    ,('b@test.de')
    ,('c@test.de')
    ,('d@test.de')
    ,('e@test.de')
    ,('f@test.de')
;

ANALYZE table_a;

DROP TABLE IF EXISTS table_b;

CREATE TABLE table_b (
	email VARCHAR(50)
)
;

INSERT INTO table_b VALUES
	('d@test.de')
    ,('e@test.de')
    ,('f@test.de')
    ,('g@test.de')
    ,('h@test.de')
    ,('i@test.de')
;

ANALYZE table_b;
```

``` sql
SELECT
	a.email
    ,b.email
    ,CASE WHEN a.email = b.email THEN 'MAPPED' ELSE 'NOT MAPPED' END status
FROM
	table_a a
    FULL OUTER JOIN table_b b ON (a.email = b.email)
;
```

- Sample Database Fiddle | DB Fiddle - SQL Database Playground
  - https://www.db-fiddle.com/f/o2ohcGVAgHZQg4teg1s9jW/131

``` sql
CREATE TABLE movie (
  id INT PRIMARY KEY,
  year INT,
  title VARCHAR
);

CREATE TABLE viewer (
  id INT PRIMARY KEY,
  name VARCHAR
);

CREATE TABLE rating (
  movie_id INT REFERENCES movie(id),
  viewer_id INT REFERENCES viewer(id),
  rating INT,
  date_rated DATE
);

INSERT INTO movie VALUES (1, 2003, 'Dinosaur Planet');
INSERT INTO movie VALUES (5, 2004, 'The Rise and Fall of ECW');
INSERT INTO movie VALUES (29, 2001, 'Boycott');
INSERT INTO movie VALUES (13402, 2002, 'The Count of Monte Cristo');
INSERT INTO movie VALUES (9298, 1996, 'From Dusk Till Dawn');

INSERT INTO viewer VALUES (885013, 'Minh');
INSERT INTO viewer VALUES (2442, 'Raj');
INSERT INTO viewer VALUES (1601783, 'Sal');
INSERT INTO viewer VALUES (1945809, 'Yuki');

INSERT INTO rating VALUES (1, 885013, 4, '2005-10-19');
INSERT INTO rating VALUES (1, 2442, 3, '2004-04-14');
INSERT INTO rating VALUES (5, 885013, 5, '2005-05-15');
INSERT INTO rating VALUES (29, 2442, 3, '2005-08-16');
INSERT INTO rating VALUES (13402, 885013, 4, '2005-10-20');
INSERT INTO rating VALUES (13402, 1601783, 3, '2003-05-23');
INSERT INTO rating VALUES (13402, 1945809, 2, '2002-10-29');
INSERT INTO rating VALUES (9298, 1601783, 1, '2000-01-12');
```

``` sql
-- List all information about all movies, sorted by release year.
SELECT * FROM movie ORDER BY year;

-- List movies that have two consecutive l's in their titles.
SELECT * FROM movie WHERE title LIKE '%ll%';

-- List dates and ratings issued by viewer 885013 (Minh).
SELECT date_rated, rating FROM rating WHERE viewer_id = 885013;

-- Retrieve a comprehensive list of all ratings for all movies, sorted by movie title.
SELECT title, name, date_rated, rating FROM movie, viewer, rating WHERE rating.movie_id = movie.id AND rating.viewer_id = viewer.id ORDER BY title;

-- List the average rating received by each movie.
SELECT title, AVG(rating) FROM movie, rating WHERE movie.id = rating.movie_id GROUP BY title;
```

