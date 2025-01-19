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

### 5. Who is the best customer? The customer who has spent the most money will be declared the best customer. 
### Write a query that returns the person who has spent the most money.
``` sql
Select a.customer_id, first_name, last_name, total_amt from
(Select customer_id, sum(total) as total_amt from invoice
group by customer_id) as a
join 
(Select customer_id, first_name, last_name from customer) as b
on a.customer_id=b.customer_id
order by total_amt desc limit 1;
```

### 6. Write query to return the email, first name, last name, & Genre of all Rock Music listeners. 
### Return your list ordered alphabetically by email starting with A. */
``` sql
Select distinct email, first_name, last_name from customer
join invoice on customer.customer_id=invoice.customer_id
join invoice_line on invoice_line.invoice_id=invoice.invoice_id
where track_id in
(Select track_id from 
track join genre on track.genre_id=genre.genre_id
where genre.name like 'Rock')
order by email;
```

### 7. Let's invite the artists who have written the most rock music in our dataset. 
### Write a query that returns the Artist name and total track count of the top 10 rock bands.
``` sql
Select artist.artist_id,artist.name, count(track.name) as songs from album join
artist on album.artist_id=artist.artist_id 
join track on album.album_id=track.album_id
join genre on track.genre_id=genre.genre_id
where genre.name like 'Rock'
group by 1
order by songs desc limit 10;
```

### 8. Return all the track names that have a song length longer than the average song length. 
### Return the Name and Milliseconds for each track. Order by the song length with the longest songs listed first.
``` sql
Select name,milliseconds from track
where milliseconds >(Select avg(milliseconds) from track)
order by milliseconds desc;
```

### 9. Find how much amount spent by each customer on artists? 
### Write a query to return customer name, artist name and total spent.
``` sql
with bsa as 
(Select artist.artist_id,artist.name as artist_name,sum(invoice_line.unit_price*invoice_line.quantity) as amount 
from track join album on track.album_id=album.album_id
join invoice_line on track.track_id=invoice_line.track_id
join artist on artist.artist_id=album.artist_id
group by artist.artist_id
order by amount desc limit 1)
Select customer.customer_id,customer.first_name,customer.last_name,
 bsa.artist_name, sum(invoice_line.unit_price*invoice_line.quantity) as Spent 
 from customer
join invoice on customer.customer_id=invoice.customer_id
join invoice_line on invoice.invoice_id=invoice_line.invoice_id
join track on track.track_id=invoice_line.track_id
join album on album.album_id=track.album_id
join bsa on album.artist_id=bsa.artist_id
group by customer.customer_id,bsa.artist_name;
```

### 10.  We want to find out the most popular music Genre for each country.  We determine the most popular genre as the genre with the highest amount of 
### purchases. Write a query that returns each country along with the top Genre. For countries where the maximum number of purchases is shared return all Genres. 
``` sql
Select track_id,country,purchases, genre_id, name from
(Select invoice_line.track_id,customer.country,
sum(invoice_line.quantity) as purchases,track.genre_id,
genre.name, row_number() over(Partition by customer.country order by sum(invoice_line.quantity) desc) as rnk
from invoice_line
join invoice on invoice_line.invoice_id=invoice.invoice_id
join  customer on customer.customer_id=invoice.customer_id
join track on invoice_line.track_id=track.track_id
join genre on genre.genre_id=track.genre_id
group by invoice_line.track_id,track.genre_id,genre.name,customer.country
order by purchases desc) as a
where rnk=1;
```

### 11. Write a query that determines the customer that has spent the most on music for each country. Write a query that returns the country along with the top ### customer and how much they spent. For countries where the top amount spent is shared, provide all customers who spent this amount.
``` sql
Select country,customer_id,first_name,last_name,round(spent) as spent from
(Select customer.customer_id,
customer.first_name,customer.last_name,
customer.country,sum(total) as spent,
row_number() over(partition by customer.country order by sum(total) desc) as rnk
from invoice_line
join invoice on invoice_line.invoice_id=invoice.invoice_id
join  customer on customer.customer_id=invoice.customer_id
join track on invoice_line.track_id=track.track_id
join genre on genre.genre_id=track.genre_id
group by customer.customer_id,
customer.first_name,customer.last_name,
customer.country
order by country asc,spent desc) as a
where rnk=1;
```
