- Dataset Obtained from Kaggle: https://www.kaggle.com/datasets/gregorut/videogamesales

# What is the Best Selling Game in North America?
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

# What is the Best Selling Game in Europe?
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

# What isBest Selling Game in Japan?
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
