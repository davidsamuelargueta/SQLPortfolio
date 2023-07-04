#Video Game Sales Analysis
-- The following dataset was taken from https://www.kaggle.com/datasets/gregorut/videogamesales, which contains data regarding video games sales in North America, Europe, Japan and Global Sales.

````sql
-- Best Selling Game in North America
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

#Best Selling Game in Europe 
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

````sql
-- Best Selling Game in Japan
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
#Top Selling Publishers Between 2000 and 2009
````sql
-- What publishers sold the most between 2000 and 2009
select 
	Publisher,
	round(sum(Global_Sales), 2) as "Total Number of Sales between 2000 and 2009"
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
````sql
-- Which Genre sold the most units in this time period
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
````sql
--Since Nintendo was one of the most profitable publishers in this time period, out of curiousity, we can see how many units in the Action genre Nintendo sold
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
````sql
--So we see Nintendo makes up a small portion of the units in the Action Genre, so we can alter our previous query slightly to see which publisher sold the most units in the action genre
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
