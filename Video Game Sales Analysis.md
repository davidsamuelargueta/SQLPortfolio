#Video Game Sales Analysis
-- The following dataset was taken from https://www.kaggle.com/datasets/gregorut/videogamesales, which contains data regarding video games sales in North America, Europe, Japan and Global Sales.

````sql
SELECT
	Name as "Best Selling Game in North America",
	Publisher as "Published By",
	NA_Sales as "North American Sales"
FROM
	vgsales
WHERE
	NA_Sales = (SELECT
			max(NA_Sales)
		    FROM
			vgsales);
````

-- Best Selling Game in Europe
````sql
SELECT
	Name as "Best Selling Game in Europe",
	Publisher as "Published By",
	EU_Sales as "European Sales"
FROM
	vgsales
WHERE
	EU_Sales = (SELECT
					max(EU_Sales)
				FROM
					vgsales);
````
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
					vgsales)
