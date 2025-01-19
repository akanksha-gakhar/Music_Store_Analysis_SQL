# SQL Project: Music Store Analysis
## Overview
In this project, you will analyze the data from a music store's database which include information about customers, playlists and artist. Using sql queries, you will extract relevant data to provide answer key questions. These insights can help the store make informed decisions about inventory management, marketing, customer preferences, and sales strategies.

## Project Structure
* **Database Setup:** Creation of music_store database and the required tables.
* **Business Problems:** Solving some specific business problems using SQL queries.
  
### 1. Who is the senior most employee based on job title?
``` sql
Select title, first_name, last_name from employee
order by levels desc limit 1;
```
### 2. Which contries have the most invoices?
``` sql
Select billing_country,count(*) as invoices from invoice
group by billing_country
order by invoices desc;
```

### 3. What are top 3 values of total invoice?
``` sql
Select distinct total from invoice
order by total desc limit 3;
```

### 4. Which city has the best customers? We would like to throw a promotional Music Festival in the city we made the most money. Write a query that returns one  city that has the highest sum of invoice totals. Return both the city name & sum of all invoice totals.
``` sql
Select billing_city,sum(total) as total from invoice
group by billing_city
order by total desc limit 1;
```

### 5. Who is the best customer? The customer who has spent the most money will be declared the best customer. Write a query that returns the person who has spent the most money.
``` sql
Select a.customer_id, first_name, last_name, total_amt from
(Select customer_id, sum(total) as total_amt from invoice
group by customer_id) as a
join 
(Select customer_id, first_name, last_name from customer) as b
on a.customer_id=b.customer_id
order by total_amt desc limit 1;
```

