<h1 align="center" style="color:blue; font-weight: bold;">SQL Best Practices </h1>

## Overview
Writing efficient and readable SQL queries is crucial for database performance and maintainability. Adhering to **Best Practices**
can greatly enhance the efficiency, readability, and scalability of your SQL code. Below are several key SQL **Best Practices**
, along with examples, to guide you in writing optimal SQL queries.

## 1. SQL Syntax

- **Best Practice**

Write clean, readable SQL by properly formatting your queries. Use consistent indentation, and meaningful aliases, and avoid unnecessary complexity.

- **Good Example**

```
SELECT 
    customer_id,
    first_name,
    last_name
FROM 
    customers
WHERE 
    active = 1
ORDER BY 
    last_name;
```
- **Bad Example**

```
SELECT customer_id, first_name, last_name FROM customers WHERE active = 1 ORDER BY last_name;
```

## 2. Select Column List Over SELECT *

- **Best Practice**

  Specify the columns you need in the SELECT statement instead of using SELECT *. This improves performance and readability.

- **Good Example**

```
SELECT 
    customer_id,
    first_name,
    last_name
FROM 
    customers;
```

- **Bad Example**

```
SELECT * FROM customers;
```
## 3. CTE Over Sub-query

- **Best Practice**

  Use Common Table Expressions (CTEs) to make complex queries more readable and maintainable instead of using sub-queries.

- **Good Example**

```
WITH OrderTotals AS (
    SELECT 
        customer_id,
        SUM(order_amount) AS total_amount
    FROM 
        orders
    GROUP BY 
        customer_id
)
SELECT 
    c.customer_id,
    c.first_name,
    ot.total_amount
FROM 
    customers c
JOIN 
    OrderTotals ot ON c.customer_id = ot.customer_id;
```

- **Bad Example**

```
SELECT 
    c.customer_id,
    c.first_name,
    (
        SELECT 
            SUM(o.order_amount)
        FROM 
            orders o
        WHERE 
            o.customer_id = c.customer_id
    ) AS total_amount
FROM 
    customers c;
```


## 4. Early Filter Where Possible

- **Best Practice**

  Filter records as early as possible in the query to reduce the number of rows processed by subsequent operations.

- **Good Example**

```
WITH FilteredOrders AS (
    SELECT 
        order_id,
        customer_id,
        order_date
    FROM 
        orders
    WHERE 
        order_date >= '2023-01-01'
)
SELECT 
    c.customer_id,
    c.first_name,
    fo.order_id,
    fo.order_date
FROM 
    customers c
JOIN 
    FilteredOrders fo ON c.customer_id = fo.customer_id;
```
- **Bad Example**

```
SELECT 
    c.customer_id,
    c.first_name,
    o.order_id,
    o.order_date
FROM 
    customers c
JOIN 
    orders o ON c.customer_id = o.customer_id
WHERE 
    o.order_date >= '2023-01-01';
```

## 5. WHERE Clause Instead of HAVING

- **Best Practice**

  Use the WHERE clause to filter records before aggregation, and use HAVING only to filter records after aggregation.

- **Good Example**

```
SELECT 
    customer_id,
    COUNT(order_id) as order_count
FROM 
    orders
WHERE 
    order_date >= '2023-01-01'
GROUP BY 
    customer_id
HAVING 
    COUNT(order_id) > 5;
```

- **Bad Example**

```
SELECT 
    customer_id,
    COUNT(order_id) as order_count
FROM 
    orders
GROUP BY 
    customer_id
HAVING 
    order_date >= '2023-01-01'
AND 
    COUNT(order_id) > 5;
```

## 6. INNER JOIN Over LEFT/RIGHT JOIN

- **Best Practice**

  Use INNER JOIN when you need only the matched records from both tables. LEFT JOIN or RIGHT JOIN should be used only when you need all records from one table and matched records from the other.

- **Good Example**

```
SELECT 
    c.customer_id,
    c.first_name,
    o.order_id
FROM 
    customers c
INNER JOIN 
    orders o ON c.customer_id = o.customer_id;
```

- **Bad Example**

```
SELECT 
    c.customer_id,
    c.first_name,
    o.order_id
FROM 
    customers c
LEFT JOIN 
    orders o ON c.customer_id = o.customer_id;
```

## 7. Join on Index or Integer Column

- **Best Practice**

  Join tables on indexed or integer columns to improve performance. Avoid joining on string columns or non-indexed columns.

- **Good Example**

```
SELECT 
    o.order_id,
    o.order_date,
    c.customer_name
FROM 
    orders o
JOIN 
    customers c ON o.customer_id = c.customer_id;
```

- **Bad Example**

```
SELECT 
    o.order_id,
    o.order_date,
    c.customer_name
FROM 
    orders o
JOIN 
    customers c ON o.customer_email = c.customer_email;
```

## 8. Use EXISTS() Instead of COUNT()

- **Best Practice**

  Use EXISTS for checking the existence of rows, which is generally faster than using COUNT(*).

- **Good Example**

```
IF EXISTS (SELECT 1 FROM customers WHERE customer_id = 1)
BEGIN
    PRINT 'Customer exists';
END;
```
- **Bad Example**

```
IF (SELECT COUNT(*) FROM customers WHERE customer_id = 1) > 0
BEGIN
    PRINT 'Customer exists';
END;
```

## 9. Use Comments for Complex Logic

- **Best Practice**

  Add comments to your SQL code to explain complex logic or any non-obvious parts of the query.
  
- **Example**

```
- Calculate the total order amount for each customer
WITH OrderTotals AS (
    SELECT 
        customer_id,
        SUM(order_amount) AS total_amount
    FROM 
        orders
    GROUP BY 
        customer_id
)
-- Select customers along with their total order amount
SELECT 
    c.customer_id,
    c.first_name,
    ot.total_amount
FROM 
    customers c
JOIN 
    OrderTotals ot ON c.customer_id = ot.customer_id;
```

# Snowflake Specific Query Performance and Compute Cost Optimization Tips:

  Optimizing query performance and managing compute costs effectively in Snowflake can significantly enhance the efficiency of your data operations. Below are some best practices to help you achieve these goals:

##1. Use LIMIT Clause to Restrict Result Volume

- **Best Practice**
  
  When working with large datasets, it's beneficial to use the LIMIT clause to restrict the number of rows returned by a query. This not only speeds up query execution but also reduces the amount of data transferred, leading to lower compute costs.

- **Example**

```
-- Retrieve only the first 100 rows from the large_table
SELECT * FROM large_table
WHERE condition = 'value'
LIMIT 100;
```

# 2. Use Temp Table Creation to Materialize a Complex Query Output

- **Best Practice**
  For complex queries involving multiple joins, aggregations, or sub-queries, creating a temporary table to store intermediate results can improve performance. This approach allows you to break down complex queries into simpler, more manageable steps and reuse the intermediate results efficiently.

- **Example**

```
-- Create a temporary table to store the intermediate results
CREATE TEMPORARY TABLE temp_results AS
SELECT column1, SUM(column2) AS total
FROM large_table
WHERE condition = 'value'
GROUP BY column1;

-- Use the temporary table in subsequent queries
SELECT column1, total
FROM temp_results
WHERE total > 100;
```

# 3. Use Snowsight to Filter Columns and Get Insights from Already Executed Queries

- **Best Practice**
  
  Snowsight, Snowflake’s built-in analytics interface, provides powerful tools for analyzing query performance and getting insights from executed queries. By   using Snowsight, you can easily filter columns, view execution details, and optimize your queries based on past performance.

- **Example**
  - Filter Columns
  - Distinct Values
  - Population Percentages
