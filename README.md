CREATE TABLE customers (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_name VARCHAR(100),
    city VARCHAR(50),
    state VARCHAR(50)
);

CREATE TABLE products (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10,2)
);

CREATE TABLE sales_orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT,
    product_id INT,
    quantity INT,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);

INSERT INTO customers (customer_name, city, state) VALUES
('Aman Dubey', 'Mumbai', 'Maharashtra'),
('Ravi Kumar', 'Delhi', 'Delhi'),
('Sneha Patil', 'Pune', 'Maharashtra'),
('Arjun Mehta', 'Bangalore', 'Karnataka'),
('Priya Singh', 'Chennai', 'Tamil Nadu');

INSERT INTO products (product_name, category, price) VALUES
('iPhone 15', 'Electronics', 80000),
('Samsung TV 55"', 'Electronics', 55000),
('Nike Shoes', 'Fashion', 6000),
('Laptop HP', 'Electronics', 65000),
('Wooden Table', 'Furniture', 12000);

INSERT INTO sales_orders (customer_id, product_id, quantity, order_date) VALUES
(1, 1, 1, '2025-01-11'),
(2, 4, 2, '2025-02-01'),
(3, 5, 1, '2025-02-05'),
(4, 3, 3, '2025-03-12'),
(1, 2, 1, '2025-03-15'),
(5, 1, 1, '2025-04-01'),
(2, 3, 2, '2025-04-10');

CREATE VIEW sales_data AS
SELECT 
    s.order_id,
    c.customer_name,
    c.city,
    c.state,
    p.product_name,
    p.category,
    p.price,
    s.quantity,
    (p.price * s.quantity) AS total_amount,
    s.order_date
FROM sales_orders s
JOIN customers c ON s.customer_id = c.customer_id
JOIN products p ON s.product_id = p.product_id;

select sum(total_amount) as total_revenue from sales_data;

select category,sum(total_amount) as revenue
from sales_data
group by category
order by revenue desc;

select
	customer_name,
    total_amount as total_spent
from sales_data
order by total_spent desc
limit 3;
SELECT 
    DATE_FORMAT(order_date, '%Y-%m') AS month,
    SUM(total_amount) AS monthly_revenue
FROM sales_data
GROUP BY month
ORDER BY month;

SELECT product_name, SUM(quantity) AS total_sold
FROM sales_data
GROUP BY product_name
ORDER BY total_sold DESC;

SELECT 
    product_name,
    category,
    total_sold,
    RANK() OVER (PARTITION BY category ORDER BY total_sold DESC) AS rank_in_category
FROM (
    SELECT product_name, category, SUM(quantity) AS total_sold
    FROM sales_data
    GROUP BY product_name, category
) AS x;

SELECT 
    city,
    SUM(total_amount) AS city_revenue,
    COUNT(DISTINCT customer_name) AS total_customers
FROM sales_data
GROUP BY city
ORDER BY city_revenue DESC;

select sum(total_amount) as total_revenue from sales_data;

select
	category,
    sum(total_amount) as earning
from sales_data
group by category
order by earning desc;

select
	customer_name,
    sum(total_amount) as total_spent
from sales_data
group by customer_name
order by total_spent desc;

select
	product_name,
    sum(total_amount) as total_spent
from sales_data
group by product_name
order by total_spent desc;

select
	city,
    sum(total_amount) as total_spent
from sales_data
group by city
order by total_spent desc;

SELECT
    DATE_FORMAT(order_date, '%Y-%m') AS month,
    SUM(total_amount) AS monthly_revenue
FROM sales_data
WHERE MONTH(order_date) BETWEEN 1 AND 4
GROUP BY month
ORDER BY month;

SELECT YEAR(order_date) FROM sales_data;
