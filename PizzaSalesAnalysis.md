For this project, I took the role of an analyst to review the performance of a ficticous Toronto Pizzeria, called 'St. Claire Slices', the data set comes from Maven Analytics which contain the number of orders, pizza types, their ingridients and their prices, etc.
For my analysis I used the recommneded questions from the Maven Analystics and created queries to answer the recommneded analysis questions, furthermore I used those same questions to create a PowerBI report on the Pizzeria.
<br>
First I created an ERR Diagram to view the relations between the four tables as shown below.
![torontopizzeria_eer](https://github.com/davidsamuelargueta/SQLProjects/assets/119771151/da4f832a-0d34-491b-8c8b-bd2a4638411d)

2015 was a great year for St. Claire Slices, on we have seen almost more than 20 customers daily. Our most successful day was November 27, while our least busiest day was December 29, likely due to holidays hours and closing around 4pm for the New Years. Below is the query used to determine the number of customers that visted each day with the date.
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
	o.date as 'Date',
	od.order_id as 'Order ID',
	count(od.pizza_id) as 'Number of Pizzas Ordered'
FROM
	torontopizzeria.order_details od
join 
	torontopizzeria.orders o
on
	od.order_id = o.order_id
group by 
	o.date, od.order_id
````

The most pizzas in a single order was 14 which only occured three times in the year, once on January 6 and twice on Janurary 8th
. 
If we want To obtain the names of the best selling pizza along with the number of times it was ordered we must first join three tables. As we can see from the EER diagram, <em>the Order Details Table  </em> does not contain the names of the pizza, only the Pizza ID. But the only other table that also has a Pizza ID column is <em>the Pizzas Table</em>. Furthermore it has the pizza type ID which is also found in <em>the Pizza Types Table</em> which has the full name of each pizza sold, so we will have to join these three tables to get the name of the best selling pizza and the number of times it was ordered.
The query used to achieve this is shown below.
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
As we can see from the table made from the query, the top 10 best sellers include Thai Chicken Pizza, Five Cheese, and the best selling pizza of the year, The Big Meat Pizza

![image](https://github.com/davidsamuelargueta/SQLProjects/assets/119771151/26d0be54-7ace-486f-a9fb-d3635351d4a0)

###### Are there any pizzas we should take of the menu, or any promotions we could leverage?
We can reuse the previous query to find the best selling pizzas to determine the least sold pizzas through out the year, 
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

With our analysis done, we can compile it into PowerBI and visualize our findings.
![image](https://github.com/davidsamuelargueta/SQLProjects/assets/119771151/18f333b8-1161-41b7-bb68-6542b05b89ad)




