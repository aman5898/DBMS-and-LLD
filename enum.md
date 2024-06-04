# Using ENUM in MySQL

## ENUM in MySQL

MySQL supports enumerated types (ENUM) which can be used to store one value from a defined list of values. ENUM is particularly useful for columns that can only hold a predefined set of values, ensuring data integrity and saving storage space.

### Example of Using ENUM

```sql
CREATE TABLE shirts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    size ENUM('small', 'medium', 'large', 'extra large')
);

INSERT INTO shirts (size) VALUES ('medium');
INSERT INTO shirts (size) VALUES ('large');
-- This will throw an error because 'extra small' is not a defined ENUM value
-- INSERT INTO shirts (size) VALUES ('extra small');

SELECT * FROM shirts;
```

### Internal Storage of ENUM Values

In MySQL, ENUM values are stored internally as integers. Each value in the ENUM list is associated with an integer index based on its position in the list, starting from 1. The index of each ENUM value is stored in the table, and this index is used to retrieve the actual string value when querying the table.

For example, consider the following ENUM definition:

```sql
CREATE TABLE shirts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    size ENUM('small', 'medium', 'large', 'extra large')
);
```

Here, the internal representation of the ENUM values would be as follows:

- 'small' = 1
- 'medium' = 2
- 'large' = 3
- 'extra large' = 4

When you insert a row with the value 'medium', MySQL stores the integer 2 in the `size` column. This saves storage space because integers typically require less storage than strings.

To demonstrate, let's insert and query some data:

```sql
INSERT INTO shirts (size) VALUES ('medium');
INSERT INTO shirts (size) VALUES ('large');
INSERT INTO shirts (size) VALUES ('small');
```

Internally, the `shirts` table would look something like this:

| id | size |
|----|------|
| 1  | 2    |
| 2  | 3    |
| 3  | 1    |

When you run a SELECT query:

```sql
SELECT * FROM shirts;
```

MySQL retrieves the stored integers and maps them back to their corresponding string values, so the output would be:

| id | size  |
|----|-------|
| 1  | medium|
| 2  | large |
| 3  | small |

By storing ENUM values as integers, MySQL optimizes storage and retrieval efficiency. This also means that the ENUM data type can provide performance benefits compared to storing the same values as plain strings.

### Using ENUM as a Foreign Key

Suppose you have two tables: `orders` and `order_statuses`. The `order_statuses` table contains the possible statuses for an order, and the `orders` table references these statuses.

1. Create the `order_statuses` table with an ENUM column:

```sql
CREATE TABLE order_statuses (
    status ENUM('pending', 'shipped', 'delivered', 'canceled') PRIMARY KEY
);
```

2. Populate the `order_statuses` table with the allowed statuses:

```sql
INSERT INTO order_statuses (status) VALUES 
('pending'), 
('shipped'), 
('delivered'), 
('canceled');
```

3. Create the `orders` table with a foreign key that references the `status` column in the `order_statuses` table:

```sql
CREATE TABLE orders (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    product_name VARCHAR(255),
    status ENUM('pending', 'shipped', 'delivered', 'canceled'),
    FOREIGN KEY (status) REFERENCES order_statuses(status)
);
```

4. Insert data into the `orders` table:

```sql
INSERT INTO orders (product_name, status) VALUES ('Laptop', 'pending');
INSERT INTO orders (product_name, status) VALUES ('Phone', 'shipped');
-- This will throw an error because 'processing' is not a defined ENUM value
-- INSERT INTO orders (product_name, status) VALUES ('Tablet', 'processing');
```

5. Query the `orders` table:

```sql
SELECT * FROM orders;
```

This will return:

| order_id | product_name | status   |
|----------|--------------|----------|
| 1        | Laptop       | pending  |
| 2        | Phone        | shipped  |


### Beautiful Article of Enum 
https://www.scaler.com/topics/enums-in-mysql/
