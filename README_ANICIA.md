CREATE TABLE Customers (
customer_id VARCHAR(20) PRIMARY KEY,
first_name VARCHAR(50) NOT NULL,
last_name VARCHAR(50),
phone_number VARCHAR(20),
email VARCHAR(50) NOT NULL,
customer_city VARCHAR(15),
country VARCHAR(20)
);

INSERT INTO Customers
(customer_id, first_name, last_name, email, customer_city, country, phone_number)
VALUES
('D0011', 'Kush', 'Felix', 'kusanpi@dmail.com', 'Paris', 'France', '+337845454'),
('D0015', 'John', 'Oore', 'johmo@dmail.com', 'London', 'UK', '+4478659787'),
('D00112', 'Melly', 'Demons', 'demoi@dmail.com', 'Chicago', 'USA', '+124554545'),
('D00121', 'John', 'Bezoz', 'juanpi@dmail.com', 'Madrid', 'Spain', '+345475554'),
('D00124', 'Kah', 'Lis', 'juanpi@dmail.com', 'Levante', 'Spain', '+34874163531'),
('D00175', 'Luzz', 'Ray', 'lusanpi@dmail.com', 'London', 'UK', '+4478659187'),
('D0066', 'Dominic', 'Ohanaka', 'domhgg@dmail.com', 'Owerri', 'Nigeria', '+23480875555');

CREATE TABLE Products (
product_id VARCHAR(20) PRIMARY KEY,
product_type VARCHAR(30),
title VARCHAR(30),
genre VARCHAR(30),
release_year INT,
unit_price_$ FLOAT,
description VARCHAR(100),
product_manager VARCHAR(50),
language VARCHAR(20)
);

INSERT INTO Products
(product_id, product_type, title, genre, release_year, unit_price_$, description,
product_manager, language)
VALUES
('P2127', 'music', 'Pac Man', 'Rap', 2018, 8.00, 'Eminem continues to diss MGK and mumble
rappers', 'Jack Ma', 'English'),
('P2128', 'music', 'Bongo', 'African', 2022, 79.00, 'An African highlife music from South East
Nigeria by Sunny BOBO', 'Jack Ma', 'Igbo'),
('P2527', 'book', 'Romeo and Juliet', 'Romance', 1752, 57.00, 'A sweet love story that ended in
the deaths of the couple', 'Dominic Leo', 'English'),
('P2627', 'movie', 'Faceoff', 'Action', 2001, 25.00, 'John Travolta pursues a terrorist to the point
of losing everything, even his identity', 'John Liu', 'English'),
('P2927', 'book', 'Sneak', 'Crime', 2022, 102.00, 'In this crime story, everybody is a suspect —
including the investigator', 'Dominic Leo', 'Italian'),
('P2027', 'movie', 'The Exorcist', 'Horror', 2012, 29.00, 'Evil is upon us; only one man is willing to
risk his life to face it', 'John Liu', 'Italian');

CREATE TABLE Orders (
order_id VARCHAR(10),
order_date VARCHAR(10),
product_id VARCHAR(20),
customer_id VARCHAR(20),
units INT,
PRIMARY KEY (order_id, product_id, customer_id),
FOREIGN KEY (product_id) REFERENCES Products(product_id),
FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);
INSERT INTO orders
(order_id, order_date, product_id, customer_id, units)
VALUES
(11245, '2022-03-20', 'P2127', 'D0011', 8),
(11246, '2022-12-01', 'P2128', 'D00112', 3),
(11247, '2022-12-02', 'P2527', 'D00121', 7),
(11248, '2022-12-12', 'P2627', 'D00121', 5),
(11250, '2022-03-13', 'P2927', 'D00121', 14),
(11249, '2022-11-14', 'P2927', 'D0011', 1);

CREATE TABLE Payments (
order_id VARCHAR(10) PRIMARY KEY,
card_type VARCHAR(20),
payment_status VARCHAR(20),
FOREIGN KEY (order_id) REFERENCES Orders(order_id)
);
INSERT INTO payments (order_id, card_type, payment_status)
VALUES
(11245, 'credit_card', 'successful'),
(11247, 'debit_card', 'failed'),
(11246, 'credit_card', 'failed'),
(11248, 'debit_card', 'successful'),
(11249, 'credit_card', 'successful');

CREATE TABLE Employees (
employee_id INT,
employee_fname VARCHAR(20),
employee_lname VARCHAR(20),
order_id VARCHAR(10),
delivery_status VARCHAR(20),
PRIMARY KEY (employee_id, order_id),
FOREIGN KEY (order_id) REFERENCES Orders(order_id)
);
INSERT INTO employees (employee_id, employee_fname, employee_lname, order_id)
VALUES
(5427136, 'Dominic', 'Ohanaka', 11245),
(5427137, 'John', 'Max', 11246),
(2427139, 'Lily', 'Payne', 11248),
(2457131, 'Rosamary', 'Sunak', 11247),
(2437132, 'Jincy', 'Nwabueze', 11249);

**Sample SQL Queries**

**Customers in London:**
SELECT * FROM Customers WHERE customer_city = 'London';

**All Available Rap Music:**
SELECT * FROM Products WHERE product_type = 'music' AND genre = 'Rap';

**Average Unit Price of Products:**
SELECT AVG(unit_price_$) AS 'Average Price' FROM Products;

**Undelivered Orders:**
SELECT * FROM Orders JOIN Employees ON Orders.order_id = Employees.order_id WHERE
delivery_status = 'pending';
**Credit Card Payments for Music:**
SELECT product_type, title, card_type, payment_status FROM Products, Orders, Payments
WHERE Products.product_id = Orders.product_id AND Payments.order_id = Orders.order_id
AND product_type = 'music' AND card_type = 'credit_card';
