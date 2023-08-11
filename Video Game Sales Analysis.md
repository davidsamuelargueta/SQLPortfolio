# Video Game Sales Analysis Mini Project

## Background <br>
The games industry is a massive entertainment field with recently almost 212 Americans partaking in the hobby. It is no surprise that many indsutries such as Netflix has started to enter the field along with major corporations such as Amazon with their Luna cloud gaming platform. Many publishers have had success in previous years, selling millions of units across the globe and continue to do so. To see how successful the industry is, we can take a look at the sales numbers prior to 2022 all the way to the 1980's. The  dataset used for this project was taken from Kaggle, https://www.kaggle.com/datasets/gregorut/videogamesales, which contains data regarding video games sales in North America, Europe, Japan and Global Sales.


## Setup <br>
First I create a Temporary Table as I want to create a new table to include the full names of the platforms and the type of platform the games are played on. 
````sql
create TEMPORARY TABLE 
	GlobalVideoGameSales 
AS SELECT
	vgsales.Name as "Name",
	consolesales.Console_Name as "Platform",
	consolesales.Type as "Type",
	vgsales.Year,
	vgsales.Genre,
	vgsales.Publisher,
	vgsales.NA_Sales,
	vgsales.EU_Sales,
	vgsales.JP_Sales,
	vgsales.Other_Sales,
	vgsales.Global_Sales
FROM
	vgsales
LEFT JOIN
	consolesales
on
	vgsales.Platform = consolesales.ConsoleID
````
As a result we will get our temporary table:
![Screenshot (10)](https://github.com/davidsamuelargueta/SQLProjects/assets/119771151/d83f39bd-3a75-4240-8fc8-27a6f7a3141c)


Consider this portion of the table,
![vgsales2](https://github.com/davidsamuelargueta/SQLProjects/assets/119771151/508a34c3-cec4-4363-9182-71b4ee76729c)
From viewing the data, we can see that there as are some games that have no year in them and noted as _N/A_. Now one could research and fill in the data for each of the games.
For example, the game Rock Band was released in 2008 on the Wii, so we can use query below to fill in the missing information
````sql
UPDATE
	GlobalVideoGameSales
set 
	year = 2008
WHERE
	Name = "Rock Band"
and 
	Platform = "Wii"
````
But if we we were count the number of games that had no years with the query
````sql
select 
	count(*)
FROM
	GlobalVideoGameSales
WHERE
	Year = 'N/A'
````
We would get a total of 271 games that have no years.So it would be best to ignore these games and remove them from the table with following query,
````sql
DELETE FROM
	GlobalVideoGameSales
WHERE
	Year = 'N/A'
````
Now that our data is cleaned, we can begin the analysis.

# Best Selling Game in Each Region
## North America
We can determine the Best Selling Game in North America in the period mentioned above. We can do so with the query:
````sql
SELECT
	Name as "Best Selling Game in North America (2000 - 2010)",
	Publisher as "Published By",
	Platform,
	Year,
	NA_Sales as "North American Sales"
FROM
	GlobalVideoGameSales
WHERE
	NA_Sales = (SELECT
			max(NA_Sales)
		    FROM
			GlobalVideoGameSales)
````
As a result, we get the following results:
![Screenshot (6)](https://github.com/davidsamuelargueta/SQLProjects/assets/119771151/72ee2ceb-c0b1-4afa-b364-2b2503e8f85e)

The best selling game from the time period is Wii Sports in 2006 released on the Wii, which sold approximately 41.5 million units.

## Europe 
We can use the same query for determing the Best Selling Game in the other regions, 
````sql
SELECT
	Name as "Best Selling Game in Europe (2000 - 2009)",
	Publisher as "Published By",
	Platform,
	Year,
	EU_Sales as "European Sales"
FROM
	GlobalVideoGameSales
WHERE
	EU_Sales = (SELECT
			max(EU_Sales)
		    FROM
			GlobalVideoGameSales)
````
![Screenshot (7)](https://github.com/davidsamuelargueta/SQLProjects/assets/119771151/0a38e006-0721-4790-b7c3-038a3b97d3b0)

Similarly, the Best Selling Game in Europe is also Wii Sports, but selling 29.02 million units.
## Japan
Again we use the query as before but for Japan Sales,
````sql
SELECT
	Name as "Best Selling Game in Japan",
	Publisher as "Published By",
	Platform,
	Year,
	JP_Sales as "Japan Sales"
FROM
	GlobalVideoGameSales
WHERE
JP_Sales = (SELECT
		max(JP_Sales)
	    FROM
		GlobalVideoGameSales)
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

