

## START 

## install
```
sudo apt-get install mysql-server
```

## open mysql

```
sudo mysql
```

# Show database

```
show databases;
```
# Create database
```
create database databasename;
```

``
databasename as your database name
``

# to use database
```
use databasename;
```

# Create tables 

```
create table tablename (insert_value_method);
```

[insert_value_method](#insert_value_method)

# insert a new value
```
INSERT INTO table_name(column_1,column_2,column_3) 
VALUES (value_1,value_2,value_3); 
```

# When inserting multiple rows into a MySQL table, the syntax is as follows

```
INSERT INTO table_name (column_1, column_2, column_3, ...) 
VALUES 
      (value_list_1), 
      (value_list_2), 
      ...
      (value_list_n); 
```


# MySQL INSERT Examples
## Let’s explore some examples of using a MySQL INSERT statement when performing data-related tasks.

## To start with, create three tables: Customers, Products01, and Smartphones by executing the following script:

```
CREATE TABLE Customers
(
  ID int,
  Age int,
  FirstName varchar(20),
  LastName varchar(20)
);
```
## Creating the Products table
```
CREATE TABLE Products01
(
  ID int,
  ProductName varchar(30) NOT NULL,
  Manufacturer varchar(20) NOT NULL,
  Price decimal NOT NULL,
  ProductCount int DEFAULT 0
);

CREATE TABLE Smartphones
(
  ID int,
  ProductName varchar(30) NOT NULL,
  Manufacturer varchar(20) NOT NULL,
  Price decimal NOT NULL,
  ProductCount int DEFAULT 0
);
```


  ON reviewers.id = reviews.reviewer_id;
-- find unreviewed series
SELECT title AS unreviewed_series
FROM series
LEFT JOIN reviews
  ON series.id = reviews.series_id
WHERE rating IS NULL;
-- find average genre rating
SELECT genre, 
  ROUND(AVG(rating), 2) AS avg_rating 
FROM series 
INNER JOIN reviews 
  ON series.id = reviews.series_id 
GROUP BY genre; 
-- find reviwers' status: name, rating, status
SELECT first_name, 
  last_name, 
  COUNT(rating) AS COUNT, 
  IFNULL(MIN(rating), 0) AS MIN, 
  IFNULL(MAX(rating), 0) AS MAX, 
  ROUND(Ifnull(AVG(rating), 0), 2) AS AVG, 
  IF(COUNT(rating) > 0, 'ACTIVE', 'INACTIVE') AS STATUS 
FROM reviewers 
LEFT JOIN reviews 
  ON reviewers.id = reviews.reviewer_id 
GROUP BY reviewers.id; 
-- find power users' status
SELECT first_name, 
  last_name, 
  COUNT(rating) AS COUNT, 
  IFNULL(MIN(rating), 0) AS MIN, 
  IFNULL(MAX(rating), 0) AS MAX, 
  ROUND(IFNULL(AVG(rating), 0), 2) AS AVG, 
  CASE 
    WHEN COUNT(rating) >= 10 THEN 'POWER USER' 
    WHEN COUNT(rating) > 0 THEN 'ACTIVE' 
    ELSE 'INACTIVE' 
  END AS STATUS 
FROM reviewers 
LEFT JOIN reviews 
  ON reviewers.id = reviews.reviewer_id 
GROUP BY reviewers.id; 
-- join 3 tables together to show the data
SELECT 
  title,
  rating,
  CONCAT(first_name,' ', last_name) AS reviewer
FROM reviewers
INNER JOIN reviews 
  ON reviewers.id = reviews.reviewer_id
INNER JOIN series
  ON series.id = reviews.series_id
ORDER BY title;
```

## Section 11: Design Instagram Database Clone
**1. Things to consider**
1. Photos
2. Users
3. Likes
4. Hashtags
5. Comments
6. Followers
7. Followees
etc ...

**2. Implement the instagram clone**
```sql
-- create the database
CREATE DATABASE ig_clone;
USE ig_clone;
-- user table
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(255) UNIQUE NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);
-- photos table
CREATE TABLE photos (
  id INT AUTO_INCREMENT PRIMARY KEY,
  image_url VARCHAR(255) NOT NULL,
  user_id INT NOT NULL, -- point to users table
  created_at TIMESTAMP DEFAULT NOW(),
  FOREIGN KEY(user_id) REFERENCES users(id)
);
-- comments table
CREATE TABLE comments (
  id INT AUTO_INCREMENT PRIMARY KEY,
  comment_text VARCHAR(255) NOT NULL,
  user_id INT NOT NULL, -- point to users table
  photo_id INT NOT NULL, -- point to photos table
  created_at TIMESTAMP DEFAULT NOW(),
  FOREIGN KEY(user_id) REFERENCES users(id),
  FOREIGN KEY(photo_id) REFERENCES photos(id)
);
-- likes table
CREATE TABLE likes (
  user_id INT NOT NULL,
  photo_id INT NOT NULL,
  created_at TIMESTAMP DEFAULT NOW(),
  FOREIGN KEY(user_id) REFERENCES users(id),
  FOREIGN KEY(photo_id) REFERENCES photos(id),
  PRIMARY KEY(user_id, photo_id) -- prevent duplications
);
-- follows table
CREATE TABLE follows (
  follower_id INT NOT NULL,
  followee_id INT NOT NULL,
  created_at TIMESTAMP DEFAULT NOW(),
  FOREIGN KEY(follower_id) REFERENCES users(id),
  FOREIGN KEY(followee_id) REFERENCES users(id),
  PRIMARY KEY(follower_id, followee_id) -- prevent duplications
);
-- tags table
CREATE TABLE tags (
  id INT AUTO_INCREMENT PRIMARY KEY,
  tag_name VARCHAR(255) UNIQUE,
  created_at TIMESTAMP DEFAULT NOW()
);
-- photo_tags table
CREATE TABLE photo_tags (
  photo_id INT NOT NULL,
  tag_id INT NOT NULL,
  FOREIGN KEY(photo_id) REFERENCES photos(id),
  FOREIGN KEY(tag_id) REFERENCES tags(id),
  PRIMARY KEY(photo_id, tag_id)
);
```

## Section 12: Working with instagram data
**1. Load the data**
see the ig_clone_data.sql file and load it
```sql
source ig_clone_data.sql
```

**2. Playing with data**
1. Find the top 5 oldest users
```sql
SELECT
  username, 
  created_at
FROM users
ORDER BY created_at
LIMIT 5;
```

2. Find the top 3 most popular registration day of a week
```sql
SELECT
  username, 
  DAYNAME(created_at) AS day,
  COUNT(*) AS total
FROM users
GROUP BY day
ORDER BY total DESC
LIMIT 3;
```

3. Find the inactive users who have never post a photo on instagram
```sql
SELECT username
FROM users
LEFT JOIN photos
  ON users.id = photos.user_id
WHERE photos.id IS NULL;
```

4. Find the most liked photo user
```sql
SELECT 
  username,
  photos.id,
  photos.image_url, 
  COUNT(*) AS total
FROM photos
INNER JOIN likes
  ON likes.photo_id = photos.id
INNER JOIN users
  ON photos.user_id = users.id
GROUP BY photos.id
ORDER BY total DESC
LIMIT 1;
```

5. Find the average posts a user make
total photos / total users
```sql
SELECT 
  (SELECT COUNT(*) FROM photos) /
  (SELECT COUNT(*) FROM users) 
  AS avg_post;
```

6. Find the top 5 popular hashtag
```sql
SELECT tags.tag_name AS tag, 
  COUNT(*) AS total 
FROM photo_tags 
JOIN tags 
  ON photo_tags.tag_id = tags.id 
GROUP BY tags.id 
ORDER BY total DESC 
LIMIT 5; 
```

7. Find users who have liked every single photo on a site
```sql
SELECT username, 
  COUNT(*) AS num_likes 
FROM users 
INNER JOIN likes 
  ON users.id = likes.user_id 
GROUP BY likes.user_id 
HAVING num_likes = (SELECT COUNT(*) 
  FROM photos); 
```

# insert_value_method

insert a character
```
varchar(255)
```
insert a number
```
int(255)
```

