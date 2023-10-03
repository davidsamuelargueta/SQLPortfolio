For this project, I used a data set from Maven Analytics which contains the data of a fictious pozzeria, which I named Toronto Pizzeria, the data contains the number of orders, pizza types and their prices, etc.
For my analysis I used the recommneded questions from Maven Analystics and created queries to answer the recommneded analysis questions, further more I used those same questions to create a PowerBI report on the Pizzeria.
<br>
First I created an ERR Diagram to view the relations between the four tables as shown below.
![torontopizzeria_eer](https://github.com/davidsamuelargueta/SQLProjects/assets/119771151/da4f832a-0d34-491b-8c8b-bd2a4638411d)


##### How many customers do we have each day? Are there any peak hours?
```` sql
select 
	o.date as 'Date',
	count(*) as 'Total Number of Customers'
from 
	torontopizzeria.orders o
group by
	o.date
````

##### How many pizzas are typically in an order? Do we have any bestsellers?
From the query below, we can see that there is at least one pizza in every order.
```` sql
select 
	od.order_id,
	count(od.pizza_id)
FROM
	torontopizzeria.order_details od
group by 
	od.order_id
````

Next, we want to obtain the names of the best selling pizza along with the number of times it was ordered. As we can see from the EER diagram, the Order Details Table only has the pizza ID but not the name, the only other table that also has the pizza ID is the Pizzas Table. Furthermore it has the pizza type ID which is also found in the Pizza Types Table which has the full name of each pizza sold, so we will have to join the three tables to get the name of the best selling pizza and the number of times it was ordered.
This is all done by the the query below.
```` sql
select 
	pt.name as 'Pizza Name', 
	count(od.pizza_id) as 'Number of Times Ordered'
FROM
	torontopizzeria.order_details od
join
	torontopizzeria.pizzas p 
on 
	od.pizza_id = p.pizza_id
join
	torontopizzeria.pizza_types pt
on
	p.pizza_type_id = pt.pizza_type_id
group by 
	pt.name, od.pizza_id 
order by 
	count(od.pizza_id) desc
````
As we can see from the table below, the top 10 best sellers include Thai Chicken Pizza, Five Cheese, and the best selling pizza of the year, The Big Meat Pizza

![image](https://github.com/davidsamuelargueta/SQLProjects/assets/119771151/26d0be54-7ace-486f-a9fb-d3635351d4a0)

#### How much money did we make this year? Can we indentify any seasonality in the sales?
To determine the amount the pizzeria made in 2015, we first need to multiply the prices by the number of pizza types ordered. we can reuse the query above and multiply the results by the price of the pizza types and store them in a subquery. We can then take the sum of that subquery to get the total amount of money earned before expenses. The query looks as such below,
```` sql
select 
	sum(prod_pizza) as 'Total Amount Earned in 2015 (before Expenses)'
from (select 
	p.pizza_id,
	count(od.pizza_id)*p.price prod_pizza
FROM
	torontopizzeria.order_details od
join
	torontopizzeria.pizzas p 
on 
	od.pizza_id = p.pizza_id
group by 
	od.pizza_id, p.price 
order by 
	count(od.pizza_id) desc) t
````
From the query, the amount of money earned in 2015 was $801,944.70, this is before our expenses. So to determine the profit we earned we will have to start deducting the expenses of operatiing a restaurant.

![image](https://github.com/davidsamuelargueta/SQLProjects/assets/119771151/6c78a27b-4b30-49bd-b04a-19d8fde80567)

###### Are there any pizzas we should take of the menu, or any promotions we could leverage?
We can use he previous query to find the best selling pizzas to determine the least sold pizzas through out the year, with editing the query to 
```` sql
select 
	pt.name as 'Pizza Name', 
	count(od.pizza_id) as 'Number of Times Ordered'
FROM
	torontopizzeria.order_details od
join
	torontopizzeria.pizzas p 
on 
	od.pizza_id = p.pizza_id
join
	torontopizzeria.pizza_types pt
on
	p.pizza_type_id = pt.pizza_type_id
group by 
	pt.name, od.pizza_id 
order by 
	count(od.pizza_id) 
````
![image](https://github.com/davidsamuelargueta/SQLProjects/assets/119771151/3123435b-03cc-47f0-a349-7ebe3b628451)
 The least sold pizzas include The Greek Pizza, The Green Garden Pizza, The Chicken Alfredo Pizza and The Calabrese Pizza, all of which sold less than 100. From this we could remove these pizzas of the menu to cut costs or run a promotion such as buy one get one free with the four pizzas mentioned above.
