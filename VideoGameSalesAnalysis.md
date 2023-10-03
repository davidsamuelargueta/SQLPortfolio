# Video Game Sales Analysis Mini Project

## Background <br>
The games industry is a massive entertainment field with recently almost 212 Americans partaking in the hobby. It is no surprise that many indsutries such as Netflix has started to enter the field along with major corporations such as Amazon with their Luna cloud gaming platform. Many publishers have had success in previous years, selling millions of units across the globe and continue to do so. To see how successful the industry is, we can take a look at the sales numbers prior to 2022 all the way to the 1980's. The  dataset used for this project was taken from Kaggle, https://www.kaggle.com/datasets/gregorut/videogamesales, which contains data regarding video games sales in North America, Europe, Japan and Global Sales.

# Best Selling Game in Each Region
## North America
We can determine the Best Selling Game in North America in the period mentioned above. We can do so with the query:
````sql
SELECT
	v.Name as "Name",
	c.Console_Name as "Platform",
	c.Type as "Type",
	v.Year,
	v.Genre,
	v.Publisher,
	v.NA_Sales
FROM
	videogamesales.vgsales v
LEFT JOIN
	videogamesales.console c
on
	v.Platform = c.ConsoleID
where 
	v.NA_Sales = (select max(v.NA_Sales) FROM videogamesales.vgsales v) 
````
The result of the query shows that the best selling game in North America is Wii Sports which sold approximately 41.5 million units.
![image](https://github.com/davidsamuelargueta/SQLProjects/assets/119771151/3a9136f9-0a94-4014-bfbb-4503c3fe6a1c)


## Europe 
We can use the same query for determing the Best Selling Game in the other regions, 
````sql
SELECT
	v.Name as "Name",
	c.Console_Name as "Platform",
	c.Type as "Type",
	v.Year,
	v.Genre,
	v.Publisher,
	v.EU_Sales as 'EU Sales'
FROM
	videogamesales.vgsales v
LEFT JOIN
	videogamesales.console c
on
	v.Platform = c.ConsoleID
where 
	v.EU_Sales = (select max(v.EU_Sales) FROM videogamesales.vgsales v) 
````
![image](https://github.com/davidsamuelargueta/SQLProjects/assets/119771151/cbc76a7d-f625-42ee-a81b-5b3af7e55e6c)
Similarly, the Best Selling Game in Europe is also Wii Sports, but selling 29.02 million units.

##### Japan
Again we use the query as before but for Japan Sales,
````sql
SELECT
	v.Name as "Name",
	c.Console_Name as "Platform",
	c.Type as "Type",
	v.Year,
	v.Genre,
	v.Publisher,
	v.JP_Sales as 'Japan Sales'
FROM
	videogamesales.vgsales v
LEFT JOIN
	videogamesales.console c
on
	v.Platform = c.ConsoleID
where 
	v.JP_Sales = (select max(v.JP_Sales) FROM videogamesales.vgsales v) 
````

We can see that the best selling game in Japan 
<br>
When Wii Sports was released, it was bundled with the Wii in NA and Eu but not Japan, which would could be attributed to its high sale numbers. If the game was bundled in Japan there is no doubt it would have also been the high selling game of the decade. Notice how in all three regions, Nintendo was the Publisher of the best selling games. 

## Top Selling Publishers from 2000 to 2010
Nintendo is a powerhouse in the industry even to this day in 2023. Nintendo is finding massive success with its games and outselling big releases on the Playstation and Xbox. But it begs the next question, if they were responsible for the best selling games in the early to late 2000's, were they also finding success in the 2000's.
````sql
select 
	Publisher,
	round(sum(Global_Sales), 2) as "Total Number of Sales between 2000 and 2009",
	round(avg(Global_Sales),2) as "Average Sales between 2000 and 2009"
FROM
	GlobalVideoGameSales
where 
	year	
between 
	2000 
and 
	2010
group by
 Publisher
 order by
	round(sum(Global_Sales), 2) desc
limit 	
	10
````
 Which Genre sold the most units in this time period
````sql
select 
	Genre,
	round(sum(Global_Sales),2) as "Global Sales"
FROM
	GlobalVideoGameSales
where 
	year	
between 
	2000 
and 
	2010
group by 
	Genre
order by 
round(sum(Global_Sales),2) desc
````

Since Nintendo was one of the most profitable publishers in this time period, we can see how many units in the Action genre Nintendo sold that may have contributed to their sales.
````sql
SELECT
	Publisher,
	Genre,
	count(Genre) as "Number of Games in the Genre"
from 
	GlobalVideoGameSales
where 
	Genre = "Action" 
AND
	Publisher = "Nintendo"
and
	year	
between 
	2000 
and 
	2010
group by
	Publisher
ORDER by
	count(Genre) desc
````
So we see Nintendo makes up a small portion of the units in the Action Genre, so while they were successful overall in this tiem period, it was not because of the Action Genre entirely. <br>
But first, we can alter our previous query slightly to see which publisher sold the most units in the action genre
````sql
SELECT
	Publisher,
	Genre,
	count(Genre) as "Number of Games in the Genre"
from 
	GlobalVideoGameSales
where 
	Genre = "Action" 
and
	year	
between 
	2000 
and 
	2010
group by
	Publisher
ORDER by
	count(Genre) desc
````
# Works Cited
https://ca.finance.yahoo.com/news/video-games-remain-america-favorite-120000076.html <br>
https://www.forbes.com/sites/paultassi/2023/02/07/the-nintendo-switch-passes-ps4-and-game-boy-to-become-the-3rd-best-selling-console-ever/?sh=521f5c4963ec <br>
https://www.kaggle.com/datasets/jaimepazlopes/game-console-manufactor-and-sales
https://www.ign.com/articles/animal-crossing-new-horizons-japans-best-selling-game-of-all-time

