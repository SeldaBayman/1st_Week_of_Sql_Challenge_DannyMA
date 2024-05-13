# 1st_Week_of_Sql_Challenge
 Sql_Challenge_1
 
Case Study #1 - Danny's Diner
Danny Ma · May 1, 2021
                                                            
 Introduction
Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favourite foods: sushi, curry and ramen.
Danny’s Diner is in need of your assistance to help the restaurant stay afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business.
                                                           
 Problem Statement
Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favourite. Having this deeper connection with his customers will help him deliver a better and more personalised experience for his loyal customers.
He plans on using these insights to help him decide whether he should expand the existing customer loyalty program - additionally he needs help to generate some basic datasets so his team can easily inspect the data without needing to use SQL.
Danny has provided you with a sample of his overall customer data due to privacy issues - but he hopes that these examples are enough for you to write fully functioning SQL queries to help him answer his questions!
Danny has shared with you 3 key datasets for this case study:
sales
menu
members

You can inspect the entity relationship diagram and example data below.                    
Entity Relationship Diagram
 
                                                               
Example Datasets
All datasets exist within the dannys_diner database schema - be sure to include this reference within your SQL scripts as you start exploring the data and answering the case study questions.
Table 1: sales
The sales table captures all customer_id level purchases with an corresponding order_date and product_id information for when and what menu items were ordered.

customer_id	order_date	product_id
A	2021-01-01	1
A	2021-01-01	2
A	2021-01-07	2
A	2021-01-10	3
A	2021-01-11	3
A	2021-01-11	3
B	2021-01-01	2
B	2021-01-02	2
B	2021-01-04	1
B	2021-01-11	1
B	2021-01-16	3
B	2021-02-01	3
C	2021-01-01	3
C	2021-01-01	3
C	2021-01-07	3
Table 2: menu
The menu table maps the product_id to the actual product_name and price of each menu item.
product_id	product_name	price
1	sushi	10
2	curry	15
3	ramen	12
Table 3: members
The final members table captures the join_date when a customer_id joined the beta version of the Danny’s Diner loyalty program.
customer_id	join_date
A	2021-01-07
B	2021-01-09

Interactive SQL Session
You can use the embedded DB Fiddle below to easily access these example datasets - this interactive session has everything you need to start solving these questions using SQL.
You can click on the Edit on DB Fiddle link on the top right hand corner of the embedded session below and it will take you to a fully functional SQL editor where you can write your own queries to analyse the data.
 
https://www.db-fiddle.com/f/2rM8RAnq7h5LLDTzZiRWcd/138

You can feel free to choose any SQL dialect you’d like to use, the existing Fiddle is using PostgreSQL 13 as default.
Serious SQL students have access to a dedicated SQL script in the 8 Week SQL Challenge section of the course which they can use to generate relevant temporary tables like we’ve done throughout the entire course!

Case Study Questions
Each of the following case study questions can be answered using a single SQL statement:
What is the total amount each customer spent at the restaurant?
How many days has each customer visited the restaurant?
What was the first item from the menu purchased by each customer?
What is the most purchased item on the menu and how many times was it purchased by all customers?
Which item was the most popular for each customer?
Which item was purchased first by the customer after they became a member?
Which item was purchased just before the customer became a member?
What is the total items and amount spent for each member before they became a member?
If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?
Bonus Questions
Join All The Things
The following questions are related creating basic data tables that Danny and his team can use to quickly derive insights without needing to join the underlying tables using SQL.
Recreate the following table output using the available data:

customer_id	order_date	product_name	price	member
A	2021-01-01	curry	15	N
A	2021-01-01	sushi	10	N
A	2021-01-07	curry	15	Y
A	2021-01-10	ramen	12	Y
A	2021-01-11	ramen	12	Y
A	2021-01-11	ramen	12	Y
B	2021-01-01	curry	15	N
B	2021-01-02	curry	15	N
B	2021-01-04	sushi	10	N
B	2021-01-11	sushi	10	Y
B	2021-01-16	ramen	12	Y
B	2021-02-01	ramen	12	Y
C	2021-01-01	ramen	12	N
C	2021-01-01	ramen	12	N
C	2021-01-07	ramen	12	N

Rank All The Things
Danny also requires further information about the ranking of customer products, but he purposely does not need the ranking for non-member purchases so he expects null ranking values for the records when customers are not yet part of the loyalty program.

customer_id	order_date	product_name	price	member	ranking
A	2021-01-01	curry	15	N	null
A	2021-01-01	sushi	10	N	null
A	2021-01-07	curry	15	Y	1
A	2021-01-10	ramen	12	Y	2
A	2021-01-11	ramen	12	Y	3
A	2021-01-11	ramen	12	Y	3
B	2021-01-01	curry	15	N	null
B	2021-01-02	curry	15	N	null
B	2021-01-04	sushi	10	N	null
B	2021-01-11	sushi	10	Y	1
B	2021-01-16	ramen	12	Y	2
B	2021-02-01	ramen	12	Y	3
C	2021-01-01	ramen	12	N	null
C	2021-01-01	ramen	12	N	null
C	2021-01-07	ramen	12	N	null

CREATE SCHEMA dannys_diner;
SET search_path = dannys_diner;

CREATE TABLE sales (
  "customer_id" VARCHAR(1),
  "order_date" DATE,
  "product_id" INTEGER
);

INSERT INTO sales
  ("customer_id", "order_date", "product_id")
VALUES
  ('A', '2021-01-01', '1'),
  ('A', '2021-01-01', '2'),
  ('A', '2021-01-07', '2'),
  ('A', '2021-01-10', '3'),
  ('A', '2021-01-11', '3'),
  ('A', '2021-01-11', '3'),
  ('B', '2021-01-01', '2'),
  ('B', '2021-01-02', '2'),
  ('B', '2021-01-04', '1'),
  ('B', '2021-01-11', '1'),
  ('B', '2021-01-16', '3'),
  ('B', '2021-02-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-07', '3');
 

CREATE TABLE menu (
  "product_id" INTEGER,
  "product_name" VARCHAR(5),
  "price" INTEGER
);

INSERT INTO menu
  ("product_id", "product_name", "price")
VALUES
  ('1', 'sushi', '10'),
  ('2', 'curry', '15'),
  ('3', 'ramen', '12');
  

CREATE TABLE members (
  "customer_id" VARCHAR(1),
  "join_date" DATE
);

INSERT INTO members
  ("customer_id", "join_date")
VALUES
  ('A', '2021-01-07'),
  ('B', '2021-01-09');
QUESTIONS-1ST WEEK OF CHALLENGE_DANNY_MA
1-select * from members
select * from menu
select * from sales

 Case Study_1 Questions_DANNY_MA
-- 1. What is the total amount each customer spent at the restaurant?
--Restoranda her bir  müşterinin yaptığı toplam harcama nedir?
Select customer_id,
sum (price) as total_sales
from sales  as s inner join menu as m on s.product_id = m.product_id 
group by 1;

-- 2. How many days has each customer visited the restaurant?
--Her bir müşteri restoranı toplam kaç gün ziyaret etmiştir?

Select customer_id, 
count (distinct order_date) as total_days
from sales
group by 1;

-- 3. What was the first item from the menu purchased by each customer?
--Her bir müşterinin, menüden, ilk sipariş ettiği yemek nedir?
WITH new_table AS (
Select customer_id, order_date, product_name,
RANK () OVER (PARTITION BY customer_id order by order_date asc) as rnk
from sales as s inner join menu as m on m.product_id = s.product_id)
;
Select customer_id,order_date from new_table 
where rnk=1 ;
ANOTHER SOLUTION:
with tablo as(
SELECT customer_id,order_date,product_name,
rank() over(PARTITION BY customer_id order by order_date asc) rnk		
FROM sales s join menu m on m.product_id = s.product_id
);

select customer_id,product_name
from tablo where sn = 1;

-- 4. What is the most purchased item on the menu and how many times was it purchased by all customers?
--Menüde şipariş edilen en çok yemek ve tüm müşteriler tarafından kaç kez sipariş edildi?

Select customer_id,
count(product_name) as total_product
from sales  as s inner join menu as m on m.product_id =s.product_id
group by 1
order by 2 desc
limit 1;

-- 5. Which item was the most popular for each customer?
Her bir müşteri için şipariş edilen en popüler yemek hangisidir?

with popular as(
Select customer_id, product_name,
count (s.product_id) as orders,
rank () over (PARTITION BY customer_id order by s.product_id desc) as rnk
from sales as s inner join menu as m on s.product_id = m.product_id 
group by 1,2
order by 1 
);
Select customer_id,product_name from popular where  rnk=1;
ANOTHER SOLUTION:
With popular as 
(Select s.customer_id,m.product_name, count(*) as items_ordered 
from sales s join menu m using (product_id)	
group by 1,2)
Select customer_id,product_name from popular where items_ordered= 
(select max (items_ordered) from popular);

-- 6. Which item was purchased first by the customer after they became a member?
-- Müşteriler, üye olduktan sonra, ilk satın aldıkları yemek, hangisidir?

WITH first AS(
Select s.customer_id, m.join_date, s.order_date, m1.product_name,
rank ()over (PARTITION BY s.customer_id order by order_date asc) as rnk
from members) as m 
left join sales as s on s.customer_id = m.customer_id 
join menu as m1 on m1.product_id = s.product_id
where m.join_date < s.order_date
); 
Select customer_id, join_date,order_date,product_name from first where rnk = 1 ;
ANOTHER SOLUTION:
with first_food as (
select s.customer_id,join_date,product_name,order_date,
rank () over (PARTITION by s.customer_id order by order_date asc) as rnk
from sales as s join menu as m s.product_id = m.product_id
join members as mem on m.customer_id=mem.customer_id
where join_date <= order_date 
);
select s.customer_id,product_name,order_date from first_food where rnk=1 ;

-- 7. Which item was purchased just before the customer became a member?
--Müşteriler, üye olmadan önce, sipariş ettikleri ürün, nedir?

WITH customer AS (
select s.customer_id, m.order_date, mem.join_date, m.product_name,
rank () over (PARTITION BY s.customer_id order by order_date desc ) as rank,
row_number () over (PARTITION BY s.customer_id order by order_date desc) as row
from sales as s inner join menu as m on m.customer_id = s.customer_id 
join members as mem on mem.product_id = m.product_id
where join_date > order_date);
select s.customer_id,product_name from customer where rnk=1;

ANOTHER SOLUTION:
with first_food as (
select s.customer_id,
join_date
product_name,
order_date,
rank () over (PARTITION by s.customer_id order by order_date DESC) as rnk
from sales as s join menu as m s.product_id = m.product_id
join members as mem on m.customer_id=mem.customer_id
where join_date > order_date)
select s.customer_id,product_name,order_date from first_food where rnk=1 

-- 8. What is the total items and amount spent for each member before they became a member?
-- Her bir müşteri, üye olmadan önce, restoranda toplam ne kadar sipariş verdi ve toplam ne kadar ödeme yaptı?

Select s.customer_id,
count (product_name) as total_sale,
sum (price) as total_spent 
from sales as inner join menu as m on s.product_id=m.product_id 
inner join members as mem on mem.customer_id = m.customer_id 
where order_date < join_date 
group by 1;
ANOTHER SOLUTION:
select s.customer_id,
count (m.product_name) as total_sale,
sum (price) as total_price,
rank () over (PARTITION by s.customer_id order by order_date) as rnk
from sales as s join menu as m s.product_id = m.product_id
join members as mem on m.customer_id=mem.customer_id
where join_date > order_date;

-- 9.  If each $1 spent equates to 10 points and sushi has a 2x points multiplier - 
--how many points would each customer have?
--Eğer harcanan her bir dolar = 10 puan ise, suşi x2 puansa, her bir müşteri,
--toplamda, kaç puan kazanmıştır?

select s.customer_id, product_name,
sum (case when product_name = 'sushi' then price * 10 * 2 else price * 10 end)
as total_point 
from menu as m 
join sales as s on s.product_id = m.product_id 
group by customer_id;
ANOTHER SOLUTION:
select s.customer_id,product_name,price,
case when product_name = 'sushi' 
then price *2*10 else price *10 end as point
from sales as s join menu as m on s.product_id=m.product_id 
select s.customer_id,
sum (point) as total points
group by 1;

-- 10. In the first week after a customer joins the program (including their join date) 
---they earn 2x points on all items, not just sushi --how many points do customer A and B have at the end of January?
--İlk hafta programa katılan bir müşteri, aldığı her üründen x2 puan kazandı,yalnızca suşiden değil. -- Ocak ayının sonunda, A ve B müşterisi kaç puan kazanmıştır?
select s.customer_id,
sum (case when order_date between mem.join_date and dateadd ('day',6,mem.join_date) then price * 10*2
else price * 10 end) as total_point from menu as m 
inner join sales as s on s.product_id =m.product_id
inner join members as mem on mem.customer_id = s.customer_id 
where date_trunc ('month',order_date) = '2021-01-01' 
group by customer_id ;
ANOTHER SOLUTION:
WITH customer as (
select s.customer_id,
join_date as date,
join_date + 6 as join_date,
product_name,
price,
case when order_date between join_date and join_date +6 then price * 2* 10
when product_name = 'sushi' then price * 2*10 else price * 10 end as total_points
from menu as m join sales as s m.product_id =s.product_id  
join members as mem on mem.customer_id=	s.customer_id
where date_trunc ('month',order_date) = '2021-01-01' 
group by customer_id
)
select customer_id, 
sum (total_points) as points from customer
group by 1;

--Example Query:
SELECT
    product_id,
    product_name,
    price
FROM dannys_diner.menu
ORDER BY price DESC
LIMIT 5;
THE END!
