# MariaDB

We Will Be Learning about how to Use MariaDB by Performing Various Operations

Databases:

How to Create ,Drop Databases.

In SQL, different types of statements are categorized into several groups based on their functionality. Here’s a breakdown of the main categories:

### 1. Data Definition Language (DDL)
DDL commands are used to define and manage all database objects, such as tables and schemas. Common DDL commands include:

- **CREATE**: Create a new database or table.
- **ALTER**: Modify an existing database object (e.g., table structure).
- **DROP**: Remove a database or table.
- **TRUNCATE**: Remove all records from a table but keep the structure.
- **RENAME**: Change the name of a database object.
### 2. Data Manipulation Language (DML)
DML commands are used to manipulate data within existing database objects. Common DML commands include:

- **SELECT**: Retrieve data from one or more tables.
- **INSERT**: Add new records to a table.
- **UPDATE**: Modify existing records in a table.
- **DELETE**: Remove records from a table.
- **MERGE**: Combine rows from two tables based on a related column.
### 3. Data Control Language (DCL)
DCL commands are used to control access to data within the database. Common DCL commands include:

- **GRANT**: Provide user access privileges to database objects.
- **REVOKE**: Remove user access privileges.
### 4. Transaction Control Language (TCL)
TCL commands are used to manage transactions in the database. Common TCL commands include:

- **COMMIT**: Save all changes made during the current transaction.
- **ROLLBACK**: Undo changes made during the current transaction.
- **SAVEPOINT**: Set a point within a transaction to which you can later roll back.
This categorization helps in understanding the various functionalities SQL offers for managing databases and their data effectively.



Lets Understand Fully From FoodDelivery DB Example

### Database Name: `FoodDelivery`  
#### Tables
1. **Users**
    - `user_id`  (INT, Primary Key, Auto Increment)
    - `username`  (VARCHAR)
    - `email`  (VARCHAR)
    - `password`  (VARCHAR)
    - `phone_number`  (VARCHAR)
    - `address`  (VARCHAR)
    - `registration_date`  (DATETIME)
2. **Restaurants**
    - `restaurant_id`  (INT, Primary Key, Auto Increment)
    - `name`  (VARCHAR)
    - `cuisine_type`  (VARCHAR)
    - `location`  (VARCHAR)
    - `rating`  (FLOAT)
    - `contact_number`  (VARCHAR)
3. **MenuItems**
    - `item_id`  (INT, Primary Key, Auto Increment)
    - `restaurant_id`  (INT, Foreign Key referencing Restaurants)
    - `item_name`  (VARCHAR)
    - `description`  (TEXT)
    - `price`  (DECIMAL)
    - `availability`  (BOOLEAN)
4. **Orders**
    - `order_id`  (INT, Primary Key, Auto Increment)
    - `user_id`  (INT, Foreign Key referencing Users)
    - `restaurant_id`  (INT, Foreign Key referencing Restaurants)
    - `order_date`  (DATETIME)
    - `total_amount`  (DECIMAL)
    - `status`  (ENUM('Pending', 'In Progress', 'Delivered', 'Cancelled'))
5. **OrderItems**
    - `order_item_id`  (INT, Primary Key, Auto Increment)
    - `order_id`  (INT, Foreign Key referencing Orders)
    - `item_id`  (INT, Foreign Key referencing MenuItems)
    - `quantity`  (INT)
6. **Reviews**
    - `review_id`  (INT, Primary Key, Auto Increment)
    - `user_id`  (INT, Foreign Key referencing Users)
    - `restaurant_id`  (INT, Foreign Key referencing Restaurants)
    - `rating`  (INT) // Assuming rating is between 1 and 5
    - `comment`  (TEXT)
    - `review_date`  (DATETIME)
Lets Try To Create Respective Tables and insert data in them .We Will Use the Following Commands as we have Learnt.



Commands to Create Tables 

```
-- Create the database
CREATE DATABASE IF NOT EXISTS FoodDelivery;

-- Use the created database
USE FoodDelivery;

-- Create Users table
CREATE TABLE Users (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50),
    email VARCHAR(100),
    password VARCHAR(255),
    phone_number VARCHAR(15),
    address VARCHAR(255),
    registration_date DATETIME DEFAULT NOW()
);

-- Create Restaurants table
CREATE TABLE Restaurants (
    restaurant_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    cuisine_type VARCHAR(50),
    location VARCHAR(100),
    rating FLOAT,
    contact_number VARCHAR(15)
);

-- Create MenuItems table
CREATE TABLE MenuItems (
    item_id INT PRIMARY KEY AUTO_INCREMENT,
    restaurant_id INT,
    item_name VARCHAR(100),
    description TEXT,
    price DECIMAL(10, 2),
    availability BOOLEAN,
    FOREIGN KEY (restaurant_id) REFERENCES Restaurants(restaurant_id) ON DELETE CASCADE
);

-- Create Orders table
CREATE TABLE Orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    restaurant_id INT,
    order_date DATETIME DEFAULT NOW(),
    total_amount DECIMAL(10, 2),
    status ENUM('Pending', 'In Progress', 'Delivered', 'Cancelled'),
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE,
    FOREIGN KEY (restaurant_id) REFERENCES Restaurants(restaurant_id) ON DELETE CASCADE
);

-- Create OrderItems table
CREATE TABLE OrderItems (
    order_item_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT,
    item_id INT,
    quantity INT,
    FOREIGN KEY (order_id) REFERENCES Orders(order_id) ON DELETE CASCADE,
    FOREIGN KEY (item_id) REFERENCES MenuItems(item_id) ON DELETE CASCADE
);

-- Create Reviews table
CREATE TABLE Reviews (
    review_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    restaurant_id INT,
    rating INT CHECK (rating BETWEEN 1 AND 5),
    comment TEXT,
    review_date DATETIME DEFAULT NOW(),
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE,
    FOREIGN KEY (restaurant_id) REFERENCES Restaurants(restaurant_id) ON DELETE CASCADE
);
```
Lets Write Commands to insert Data into Tables

```
-- Insert sample users
INSERT INTO Users (username, email, password, phone_number, address) VALUES
('john_doe', 'john@example.com', 'password123', '1234567890', '123 Elm St'),
('jane_smith', 'jane@example.com', 'password456', '0987654321', '456 Oak St');

-- Insert sample restaurants
INSERT INTO Restaurants (name, cuisine_type, location, rating, contact_number) VALUES
('Pizza Place', 'Italian', 'Downtown', 4.5, '1112223333'),
('Burger Joint', 'American', 'Uptown', 4.0, '4445556666');

-- Insert sample menu items
INSERT INTO MenuItems (restaurant_id, item_name, description, price, availability) VALUES
(1, 'Margherita Pizza', 'Classic pizza with fresh mozzarella and basil.', 12.99, TRUE),
(1, 'Pepperoni Pizza', 'Pizza topped with pepperoni and cheese.', 14.99, TRUE),
(2, 'Cheeseburger', 'Juicy beef burger with cheese and toppings.', 10.99, TRUE),
(2, 'Fries', 'Crispy golden fries.', 3.99, TRUE);

-- Insert sample orders
INSERT INTO Orders (user_id, restaurant_id, total_amount, status) VALUES
(1, 1, 26.98, 'Pending'),
(2, 2, 14.98, 'Delivered');

-- Insert sample order items
INSERT INTO OrderItems (order_id, item_id, quantity) VALUES
(1, 1, 2), -- 2 Margherita Pizzas
(1, 3, 1), -- 1 Cheeseburger
(2, 2, 1), -- 1 Pepperoni Pizza
(2, 4, 1); -- 1 Fries

-- Insert sample reviews
INSERT INTO Reviews (user_id, restaurant_id, rating, comment) VALUES
(1, 1, 5, 'Absolutely loved the pizza!'),
(2, 2, 4, 'Great burger, but the fries were a bit soggy.');
```
In Case we Need More than Lets Insert More Data in Respective Tables

**Users Table**

```
INSERT INTO Users (username, email, password, phone_number, address) VALUES
('alice_wonder', 'alice@example.com', 'password789', '1233211234', '789 Pine St'),
('bob_builder', 'bob@example.com', 'password321', '9876543210', '321 Birch St'),
('charlie_brown', 'charlie@example.com', 'password654', '4567890123', '654 Cedar St'),
('daisy_duck', 'daisy@example.com', 'password987', '7890123456', '987 Willow St');
```
**Restaurants**

```
INSERT INTO Restaurants (name, cuisine_type, location, rating, contact_number) VALUES
('Sushi World', 'Japanese', 'City Center', 4.8, '5556667777'),
('Taco Heaven', 'Mexican', 'East Side', 4.2, '8889990000'),
('Curry House', 'Indian', 'West End', 4.6, '2223334444'),
('Dessert Paradise', 'Desserts', 'Central Plaza', 4.9, '3334445555');
```
**Menu Items**

```
INSERT INTO MenuItems (restaurant_id, item_name, description, price, availability) VALUES
(1, 'California Roll', 'Delicious California roll with crab and avocado.', 8.99, TRUE),
(1, 'Sushi Platter', 'Assorted sushi platter for two.', 29.99, TRUE),
(2, 'Tacos', 'Soft tacos with choice of chicken or beef.', 9.99, TRUE),
(2, 'Nachos', 'Crispy nachos with cheese and jalapeños.', 7.49, TRUE),
(3, 'Butter Chicken', 'Creamy butter chicken with naan.', 13.99, TRUE),
(3, 'Vegetable Curry', 'Mixed vegetables in spicy curry sauce.', 11.99, TRUE),
(4, 'Chocolate Cake', 'Rich chocolate cake with frosting.', 5.99, TRUE),
(4, 'Ice Cream Sundae', 'Vanilla ice cream topped with chocolate syrup.', 4.99, TRUE);
```
**Orders**

```
INSERT INTO Orders (user_id, restaurant_id, total_amount, status) VALUES
(3, 1, 38.98, 'In Progress'),
(4, 2, 17.48, 'Pending'),
(1, 3, 25.98, 'Delivered'),
(2, 4, 10.98, 'Delivered'),
(3, 2, 19.98, 'Cancelled'),
(4, 1, 29.99, 'In Progress');
```
**Order Items**

```
INSERT INTO OrderItems (order_id, item_id, quantity) VALUES
(3, 1, 2), -- 2 California Rolls
(3, 2, 1), -- 1 Sushi Platter
(4, 3, 2), -- 2 Tacos
(4, 5, 1), -- 1 Nachos
(5, 4, 1), -- 1 Butter Chicken
(5, 6, 1), -- 1 Vegetable Curry
(6, 1, 1), -- 1 Sushi Platter
(6, 3, 1); -- 1 Nachos
```
**Reviews**

```
INSERT INTO Reviews (user_id, restaurant_id, rating, comment) VALUES
(1, 1, 5, 'Best sushi in town! Fresh and delicious.'),
(2, 2, 3, 'Tacos were okay, but could use more filling.'),
(3, 3, 4, 'Great butter chicken, but a bit too spicy for me.'),
(4, 4, 5, 'Heavenly desserts! Highly recommend the chocolate cake.'),
(1, 2, 4, 'Tacos were good, but the nachos were amazing!'),
(3, 1, 5, 'The sushi platter was fantastic, will order again!'),
(4, 3, 4, 'Loved the curry, very flavorful!');
```
Reaching at this Point Means we have learnt How to Create Database, Create Tables , Insert Data in Tables .

Quickly we will Go and Learn More Commands and we will also know the Categories in which commands can be classified 😀. 

Lets Learn More Here

> ALTER TABLE 

> UPDATE

> DELETE

> RENAME

> DROP

> TRUNCATE

> CREATE INDEX

> DROP INDEX

> CHECK CONSTRAINT

### 1. **ALTER TABLE**
The `ALTER TABLE` command is used to modify an existing table's structure. This includes adding, deleting, or modifying columns, as well as adding or dropping constraints. It allows you to make changes to the table without having to recreate it.

### 2. **UPDATE**
The `UPDATE` command is used to modify existing records in a table. It allows you to change the values of one or more columns for all rows that meet a specified condition. If no condition is provided, all records in the table will be updated.

### 3. **DELETE**
The `DELETE` command is used to remove existing records from a table. You can specify which records to delete using a condition. If no condition is provided, all records in the table will be deleted. However, the structure of the table remains intact.

### 4. **RENAME**
The `RENAME` command is used to change the name of an existing database object, such as a table or column. This command allows for better organization and clarity in database design, especially when restructuring or renaming entities.

### 5. **DROP**
The `DROP` command is used to permanently remove a database object, such as a table, view, or database, along with all its data and structure. Once a table is dropped, it cannot be recovered unless a backup is available.

### 6. **TRUNCATE**
The `TRUNCATE` command is used to delete all records in a table while keeping the table structure intact. Unlike `DELETE`, which can be filtered with conditions, `TRUNCATE` removes all rows and resets any auto-increment counters, if applicable.

### 7. **CREATE INDEX**
The `CREATE INDEX` command is used to create an index on one or more columns of a table to improve the speed of data retrieval operations. Indexes are particularly useful for enhancing the performance of queries that involve searching and sorting.

### 8. **DROP INDEX**
The `DROP INDEX` command is used to remove an index from a table. This can be necessary if the index is no longer needed or if it is impacting performance negatively. Dropping an index can also help reduce storage space.

### 9. **CHECK CONSTRAINT**
A `CHECK CONSTRAINT` is a rule that defines a condition that must be met for values in a column. When added to a table, it ensures that all values in the specified column adhere to the defined condition. This helps maintain data integrity by preventing invalid data entries.



Lets See With Examples



Sure! Here’s a list of common SQL commands for modifying tables and records, along with their syntaxes:

### 1. **ALTER TABLE**
#### Example: Adding a New Column
Suppose we want to add a column for `loyalty_points` to the `Users` table to track points earned by users.

```sql
ALTER TABLE Users
ADD loyalty_points INT DEFAULT 0;
```
### 2. **UPDATE**
#### Example: Updating a User's Email
Let’s say we want to update the email address of the user with `user_id = 1`.

```sql
UPDATE Users
SET email = 'john_doe_new@example.com'
WHERE user_id = 1;
```
#### Example: Modifying a Restaurant's Rating
If the rating of a restaurant needs to be updated, you can do so as follows:

```sql
UPDATE Restaurants
SET rating = 4.7
WHERE restaurant_id = 1;  -- For Pizza Place
```
### 3. **DELETE**
#### Example: Deleting a Menu Item
If we want to remove a menu item, such as the "Fries" from the `MenuItems` table, we can do the following:

```sql
DELETE FROM MenuItems
WHERE item_id = 4;  -- Assuming 4 is the ID for "Fries"
```
#### Example: Deleting a User's Review
To delete a specific review by `review_id`, you can use:

```sql
DELETE FROM Reviews
WHERE review_id = 2;  -- Assuming 2 is the ID of the review to delete
```
### 4. **RENAME TABLE**
Used to rename an existing table.

```sql
RENAME TABLE old_table_name TO new_table_name;
```
**Example:**

```sql
RENAME TABLE Users TO Customers;
```
### 5. **RENAME COLUMN**
Used to rename an existing column in a table (syntax may vary by SQL dialect).

```sql
ALTER TABLE table_name
RENAME COLUMN old_column_name TO new_column_name;
```
**Example:**

```sql
ALTER TABLE Users
RENAME COLUMN date_of_birth TO birth_date;
```
### 6. **DROP TABLE**
Used to delete a table and its data.

```sql
DROP TABLE table_name;
```
**Example:**

```sql
DROP TABLE OrderItems;
```
### 7. **TRUNCATE TABLE**
Used to delete all records in a table without removing the table itself.

```sql
TRUNCATE TABLE table_name;
```
**Example:**

```sql
TRUNCATE TABLE Users;
```
### 8. **CREATE INDEX**
Used to create an index on a table to improve query performance.

```sql
CREATE INDEX index_name
ON table_name (column_name);
```
**Example:**

```sql
CREATE INDEX idx_restaurant_name
ON Restaurants (name);
```
### 9. **DROP INDEX**
Used to remove an index from a table.

```sql
DROP INDEX index_name;
```
**Example:**

```sql
DROP INDEX idx_restaurant_name;
```
### 10. **CHECK CONSTRAINT**
Used to add a check constraint to ensure that values in a column meet a specific condition.

```sql
ALTER TABLE table_name
ADD CONSTRAINT constraint_name CHECK (condition);
```
**Example:**

```sql
ALTER TABLE MenuItems
ADD CONSTRAINT chk_price CHECK (price > 0);
```






### 1. **ALTER TABLE**
#### Example: Adding a New Column
Suppose we want to add a column for `loyalty_points` to the `Users` table to track points earned by users.

```sql
ALTER TABLE Users
ADD loyalty_points INT DEFAULT 0;
```
### 2. **UPDATE**
#### Example: Updating a User's Email
Let’s say we want to update the email address of the user with `user_id = 1`.

```sql
UPDATE Users
SET email = 'john_doe_new@example.com'
WHERE user_id = 1;
```
#### Example: Modifying a Restaurant's Rating
If the rating of a restaurant needs to be updated, you can do so as follows:

```sql
UPDATE Restaurants
SET rating = 4.7
WHERE restaurant_id = 1;  -- For Pizza Place
```
### 3. **DELETE**
#### Example: Deleting a Menu Item
If we want to remove a menu item, such as the "Fries" from the `MenuItems` table, we can do the following:

```sql
DELETE FROM MenuItems
WHERE item_id = 4;  -- Assuming 4 is the ID for "Fries"
```
#### Example: Deleting a User's Review
To delete a specific review by `review_id`, you can use:

```sql
DELETE FROM Reviews
WHERE review_id = 2;  -- Assuming 2 is the ID of the review to delete
```


lets try  Running Advanced Commands 😎

Let’s explore **views** and **stored procedures** in SQL, including their definitions, purposes, and examples.

### Views
A **view** is a virtual table that is based on the result of a SQL query. It doesn’t store the data itself but provides a way to simplify complex queries, encapsulate business logic, and improve data security by restricting access to specific columns or rows.

#### Creating a View
**Syntax:**

```sql
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```
#### Example: Creating a View for Restaurant Ratings
Let’s create a view that shows the restaurant name and its average rating.

```sql
CREATE VIEW RestaurantRatings AS
SELECT r.name AS restaurant_name, AVG(re.rating) AS average_rating
FROM Restaurants r
JOIN Reviews re ON r.restaurant_id = re.restaurant_id
GROUP BY r.name;
```
#### Using the View
You can query the view just like a regular table:

```sql
SELECT * FROM RestaurantRatings;
```
### Stored Procedures
A **stored procedure** is a precompiled collection of one or more SQL statements that can be executed as a single unit. Stored procedures are useful for encapsulating business logic, improving performance, and managing complex operations.

#### Creating a Stored Procedure
**Syntax:**

```sql
CREATE PROCEDURE procedure_name (parameters)
BEGIN
    SQL statements;
END;
```
#### Example: Creating a Stored Procedure to Add a New Order
Let’s create a stored procedure that allows adding a new order along with its items.

```sql
DELIMITER $$

CREATE PROCEDURE AddOrder(
    IN p_user_id INT,
    IN p_restaurant_id INT,
    IN p_total_amount DECIMAL(10, 2)
)
BEGIN
    DECLARE new_order_id INT;

    -- Insert a new order
    INSERT INTO Orders (user_id, restaurant_id, total_amount, status)
    VALUES (p_user_id, p_restaurant_id, p_total_amount, 'Pending');

    -- Get the ID of the newly inserted order
    SET new_order_id = LAST_INSERT_ID();

    -- Example: Add a default order item (you could modify this to accept more parameters)
    INSERT INTO OrderItems (order_id, item_id, quantity)
    VALUES (new_order_id, 1, 1);  -- Assuming item_id 1 is a valid item

END $$

DELIMITER ;
```
#### Executing the Stored Procedure
To execute the stored procedure, you can use the following command:

```sql
CALL AddOrder(1, 1, 25.00);  -- Example values for user_id, restaurant_id, and total_amount
```
### Summary
- **Views** simplify complex queries and enhance security by providing a controlled way to access data.
- **Stored Procedures** encapsulate business logic and can execute multiple SQL statements in a single call, improving performance and maintainability.




Additional Tips 

CTE  🫡 :

### Common Table Expression (CTE) in MariaDB
A **Common Table Expression (CTE)** is a temporary result set that you can reference within a `SELECT`, `INSERT`, `UPDATE`, or `DELETE` statement. It is particularly useful for breaking down complex queries, improving readability, and reusing query logic.

MariaDB supports CTEs using the `WITH` clause. Here is an example based on the **FoodDelivery** database:

---

#### Use Case: Find Restaurants with Average Ratings Above 4.5
We want to find all restaurants whose average customer rating exceeds 4.5, using the **Reviews** table for reference.

---

```sql
WITH AverageRatings AS (
    SELECT
        restaurant_id,
        AVG(rating) AS avg_rating
    FROM
        Reviews
    GROUP BY
        restaurant_id
)
SELECT
    r.name AS RestaurantName,
    ar.avg_rating AS AverageRating
FROM
    AverageRatings ar
    JOIN Restaurants r ON ar.restaurant_id = r.restaurant_id
WHERE
    ar.avg_rating > 4.5;
```
---

### Explanation:
1. **CTE Definition (**`**AverageRatings**` **)**:
    - Computes the average rating for each restaurant by grouping data from the **Reviews** table.
    - Uses `AVG(rating)`  to calculate the average.
    - The result includes two columns: `restaurant_id`  and `avg_rating` .
2. **Main Query**:
    - Joins the **AverageRatings** CTE with the **Restaurants** table to get the restaurant names.
    - Filters restaurants where the average rating is greater than 4.5.
---

### Result (Sample Output):
| RestaurantName | AverageRating |
| ----- | ----- |
| Dessert Paradise | 4.9 |
| Sushi World | 4.8 |




Here’s an explanation of SQL joins in MariaDB with examples and diagrams illustrating table relationships and results.

---

### **Base Tables**
We’ll use two example tables:

#### `**users**`** Table**
| **user_id** | **username** | **email** |
| ----- | ----- | ----- |
| 1 | Alice |  |
| 2 | Bob |  |
| 3 | Charlie |  |
| 4 | Diana |  |
#### `**orders**`** Table**
| **order_id** | **user_id** | **total_amount** |
| ----- | ----- | ----- |
| 101 | 1 | 250 |
| 102 | 2 | 450 |
| 103 | 1 | 300 |
| 104 | 5 | 500 |
---

### **1. INNER JOIN**
An **INNER JOIN** retrieves rows with matching values in both tables.

#### **Query**:
```sql
SELECT u.user_id, u.username, o.order_id, o.total_amount
FROM users u
INNER JOIN orders o
ON u.user_id = o.user_id;
```
#### **Result**:
| **user_id** | **username** | **order_id** | **total_amount** |
| ----- | ----- | ----- | ----- |
| 1 | Alice | 101 | 250 |
| 1 | Alice | 103 | 300 |
| 2 | Bob | 102 | 450 |
#### **Diagram**:
The join only includes matching rows:

```plaintext
users                    orders
+-----------+            +-----------+
| user_id   |            | user_id   |
| username  |            | order_id  |
|           |------------| total_amt |
|           |            +-----------+
+-----------+
Result:
+-----------+
| user_id   |
| username  |
| order_id  |
+-----------+
```
---

### **2. LEFT JOIN**
A **LEFT JOIN** retrieves all rows from the left table (`users`) and matches from the right table (`orders`). Unmatched rows from the right table will have `NULL`.

#### **Query**:
```sql
SELECT u.user_id, u.username, o.order_id, o.total_amount
FROM users u
LEFT JOIN orders o
ON u.user_id = o.user_id;
```
#### **Result**:
| **user_id** | **username** | **order_id** | **total_amount** |
| ----- | ----- | ----- | ----- |
| 1 | Alice | 101 | 250 |
| 1 | Alice | 103 | 300 |
| 2 | Bob | 102 | 450 |
| 3 | Charlie | NULL | NULL |
| 4 | Diana | NULL | NULL |
#### **Diagram**:
The left table (`users`) always appears full, even without matching rows in the right table.

```plaintext
users                    orders
+-----------+            +-----------+
| user_id   |            | user_id   |
| username  |            | order_id  |
|           |------------| total_amt |
|           |            +-----------+
+-----------+
Unmatched:
NULL values.
```
---

### **3. RIGHT JOIN**
A **RIGHT JOIN** retrieves all rows from the right table (`orders`) and matches from the left table (`users`). Unmatched rows from the left table will have `NULL`.

#### **Query**:
```sql
SELECT u.user_id, u.username, o.order_id, o.total_amount
FROM users u
RIGHT JOIN orders o
ON u.user_id = o.user_id;
```
#### **Result**:
| **user_id** | **username** | **order_id** | **total_amount** |
| ----- | ----- | ----- | ----- |
| 1 | Alice | 101 | 250 |
| 1 | Alice | 103 | 300 |
| 2 | Bob | 102 | 450 |
| NULL | NULL | 104 | 500 |
#### **Diagram**:
The right table (`orders`) always appears in full, even without matching rows in the left table.

---

### **4. FULL JOIN (Simulated with UNION)**
A **FULL JOIN** retrieves all rows from both tables. Rows without matches will have `NULL` in non-matching columns. MariaDB does not natively support `FULL JOIN` but can be simulated using `LEFT JOIN` and `UNION`.

#### **Query**:
```sql
SELECT u.user_id, u.username, o.order_id, o.total_amount
FROM users u
LEFT JOIN orders o
ON u.user_id = o.user_id
UNION
SELECT u.user_id, u.username, o.order_id, o.total_amount
FROM users u
RIGHT JOIN orders o
ON u.user_id = o.user_id;
```
#### **Result**:
| **user_id** | **username** | **order_id** | **total_amount** |
| ----- | ----- | ----- | ----- |
| 1 | Alice | 101 | 250 |
| 1 | Alice | 103 | 300 |
| 2 | Bob | 102 | 450 |
| 3 | Charlie | NULL | NULL |
| 4 | Diana | NULL | NULL |
| NULL | NULL | 104 | 500 |
---

### **5. CROSS JOIN**
A **CROSS JOIN** creates a Cartesian product between two tables, combining every row of the left table with every row of the right table.

#### **Query**:
```sql
SELECT u.user_id, u.username, o.order_id, o.total_amount
FROM users u
CROSS JOIN orders o;
```
#### **Result**:
| **user_id** | **username** | **order_id** | **total_amount** |
| ----- | ----- | ----- | ----- |
| 1 | Alice | 101 | 250 |
| 1 | Alice | 102 | 450 |
| 1 | Alice | 103 | 300 |
| 1 | Alice | 104 | 500 |
| 2 | Bob | 101 | 250 |
| 2 | Bob | 102 | 450 |
| ... | ... | ... | ... |
#### **Diagram**:
Every row of `users` is combined with every row of `orders`.

```plaintext
users × orders
= all combinations
```
---

### **6. SELF JOIN**
A **SELF JOIN** is when a table is joined with itself. This is useful for hierarchical or comparative data.

#### **Example Query**:
Find pairs of users where one user referred another.

```
SELECT u1.username AS Referrer, u2.username AS Referred
FROM users u1
INNER JOIN users u2
ON u1.user_id = u2.referred_by;
```
﻿ 





