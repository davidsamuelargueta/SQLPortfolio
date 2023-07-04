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
