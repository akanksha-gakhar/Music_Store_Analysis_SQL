# SQL Project: Data Analysis for Zomato
## Overview
This project showcases my SQL problem-solving abilities through the analysis of data for Zomato, a leading food brand in India. It includes setting up databases, importing data, addressing null values, and resolving various business challenges using advanced SQL queries.

## Project Structure
* **Database Setup:** Creation of zomato_db database and the required tables.
* **Data Import:** Inserting sample data into the tables.
* **Data Cleaning:** Handling null values and ensuring data integrity.
* **Business Problems:** Solving 20 specific business problems using SQL queries.
  
## Database Setup
``` sql
Create Database zomato_db;
````
### 1. Creating Data Tables
``` sql
-- Zomato Data Analysis
create database zomato_db;
use zomato_db;
drop table if exists orders;
drop table if exists customers;
drop table if exists restaurants;
drop table if exists riders;
drop table if exists deliveries;

Create Table customers(
customer_id INT PRIMARY KEY, 
customer_name varchar(25),
reg_date date
);
Create Table restaurants(
restaurant_id INT PRIMARY KEY, 
restaurant_name varchar(55),
city varchar(15),
opening_hour varchar(55)
);
Create Table orders(
order_id INT PRIMARY KEY, 
customer_id INT,
restaurant_id INT,
order_item varchar(55),
order_date date,
order_time time,
order_satus varchar(55),
total_amount float
);
-- adding FK Constraint
Alter Table orders
Add constraint fk_customers
foreign key (customer_id)
references customers(customer_id);
-- adding FK Constraint
Alter Table restaurants
Add constraint fk_restaurants
foreign key (restaurant_id)
references restaurants(restaurant_id);
Create Table riders(
rider_id INT PRIMARY KEY, 
rider_name varchar(25),
sign_up date
);

Create table deliveries
(deliver_id int PRIMARY KEY,
order_id int,	
delivery_status varchar(35),
delivery_time time,
rider_id int,
constraint fk_orders foreign key (order_id) references orders(order_id),
constraint fk_riders foreign key (rider_id) references riders(rider_id)
);
```
