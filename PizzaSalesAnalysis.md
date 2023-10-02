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

    Are there any pizzas we should take of the menu, or any promotions we could leverage?
