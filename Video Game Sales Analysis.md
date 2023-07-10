# Video Game Sales Analysis

## Background <br>
The games industry is a massive entertainment field with recently almost 212 Americans partaking in the hobby. It is no surprise that many indsutries such as Netflix has started to enter the field along with major corporations such as Amazon with their Luna cloud gaming platform. Many publishers have had success in previous years, selling millions of units across the globe and continue to do so. To see how successful the industry is, we can take a look at the  sales numbers prior to 2022 all the way to the 1980's. The  dataset used for this project was taken from Kaggle, https://www.kaggle.com/datasets/gregorut/videogamesales, which contains data regarding video games sales in North America, Europe, Japan and Global Sales.

## Best Selling Game in North America
````sql
SELECT
	Name as "Best Selling Game in North America (2000 - 2009)",
	Publisher as "Published By",
	NA_Sales as "North American Sales"
FROM
	vgsales
WHERE
	NA_Sales = (SELECT
			max(NA_Sales)
		    FROM
			vgsales
		    WHERE
			Year 
		    BETWEEN 
			2000 
		     and 
		        2009)
````

## Best Selling Game in Europe 
````sql
SELECT
	Name as "Best Selling Game in Europe (2000 - 2009)",
	Publisher as "Published By",
	EU_Sales as "European Sales"
FROM
	vgsales
WHERE
	EU_Sales = (SELECT
			max(EU_Sales)
		    FROM
			vgsales
		    WHERE
			Year
		    BETWEEN
			2000
		    and
			2009)
````

## Best Selling Game in Japan
````sql

SELECT
	Name as "Best Selling Game in Japan",
	Publisher as "Published By",
	EU_Sales as "Japan Sales"
FROM
	vgsales
WHERE
JP_Sales = (SELECT
		max(JP_Sales)
	    FROM
		vgsales
	    WHERE
		Year
	    BETWEEN
		2000
	    and
		2009)
````
## Top Selling Publishers Between 2000 and 2009
What publishers sold the most between 2000 and 2009? What were their average sales (in millions) in during this period
````sql
select 
	Publisher,
	round(sum(Global_Sales), 2) as "Total Number of Sales between 2000 and 2009",
	round(avg(Global_Sales),2) as "Average Sales between 2000 and 2009"
FROM
	vgsales
where 
	year	
between 
	2000 
and 
	2009
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
	vgsales
where 
	year	
between 
	2000 
and 
	2009
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
	vgsales
where 
	Genre = "Action" 
AND
	Publisher = "Nintendo"
and
	year	
between 
	2000 
and 
	2009
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
	vgsales
where 
	Genre = "Action" 
and
	year	
between 
	2000 
and 
	2009
group by
	Publisher
ORDER by
	count(Genre) desc
````
# Works Cited
https://ca.finance.yahoo.com/news/video-games-remain-america-favorite-120000076.html <br>
https://www.forbes.com/sites/paultassi/2023/02/07/the-nintendo-switch-passes-ps4-and-game-boy-to-become-the-3rd-best-selling-console-ever/?sh=521f5c4963ec <br>
https://www.kaggle.com/datasets/jaimepazlopes/game-console-manufactor-and-sales

