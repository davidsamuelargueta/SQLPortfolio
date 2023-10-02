## Background
For this project, I took a dataset off of Maven Analytics and used SQL queries to answer the recommended questions as a way to practice my SQL knowledge. <br>

#### What percentage of games were won by white? How many ended in a draw?
To start we will determine the percentage of games won by white, black and games that ended in a draw. To do so we will need to apply the cast() function to divide the results of using the count() function .
First we can create the query below to determine the percentage of games won by White
```` sql
select 
	count(*) as 'Total Number of Games',
        (select count(*) from chessgame.chess_games c where c.winner = 'White') as 'Total Number of Games Won by White',
        round(cast((select count(*) from chessgame.chess_games c where c.winner = 'White') as float) / cast(count(*) as float) * 100, 2) as 'Percentage of Games Won by White'
        
from 
	chessgame.chess_games c
````
As a result, we obtain a row with the number of games in the set,  the number of games won by white and the percentage of games won by white. <br>

![image](https://github.com/davidsamuelargueta/SQLProjects/assets/119771151/845992de-fcb5-463a-9f6b-a248767557e4)


We can see that the games won by white make up almost half of the wins in the data set.

Before going forward, we can edit the query used to find the percentage of games won by white, to find the remaining percentages of games won by black and games that ended in a draw
    
First we can determine the percentage of games won by black.
```` sql
select 
	count(*) as 'Total Number of Games',
        (select count(*) from chessgame.chess_games c where c.winner = 'Black') as 'Total Number of Games Won by Black',
        round(cast((select count(*) from chessgame.chess_games c where c.winner = 'Black') as float) / cast(count(*) as float) * 100, 2) as 'Percentage of Games Won by Black'
        
from 
	chessgame.chess_games c
````
The query returns the following row,
![image](https://github.com/davidsamuelargueta/SQLProjects/assets/119771151/4e116610-3511-4789-b306-dd914127dc4c)

As we can see, the number of games won by Black was around the 45% mark, with 9107 wins.

Lastly, lets apply our query to determine the percentage of games ending with a draw, again by making a simple edit to our query.
```` sql
select 
	count(*) as 'Total Number of Games',
        (select count(*) from chessgame.chess_games c where c.winner = 'Draw') as 'Total Number of Games Ended with a Draw',
        round(cast((select count(*) from chessgame.chess_games c where c.winner = 'Draw') as float) / cast(count(*) as float) * 100, 2) as 'Percentage of Games Ended with a Draw'
        
from 
	chessgame.chess_games c
````
The query returns the following row,
![image](https://github.com/davidsamuelargueta/SQLProjects/assets/119771151/f819139c-0413-41f2-bfe0-3eb1b6b83e1f)
As we can see, the number of games that ended with a draw was less than 5%, with 950 games.

In conclusion the percentage of games won by white is 49.86%, the percentage of games won by Black is 45.4% and the percentage of games ending with a draw is 4.74%.

#### Which opening move was most frequently used in games in which black won? What about when white won?
From viewing the opening names column of the table, we can see the full name of the opening move, so we will need this column. We can use the count() function again to determine which opening move was most occuring opening move. However we want to return the most frequently used opening move.
As such we can apply the max() function, but we must create a subquery first so that we can obtain a column with the number of times each opening move was used and ghen apply the max() function on our result

####  What percentage of games are won by the player with the higher rating? Does this vary by piece color?
First we will need to know the player with the highest rating in both white and black, this done by the following query
```` sql
select
  c.white_id as 'Player with the Highest Rating in White',
  c.white_rating as 'Rating in White'
from
  chessgame.chess_games c
where
  c.white_rating = (select
                      max(c.white_rating)
                    from
                      chessgame.chess_games c)
group by
  c.white_id, c.white_rating
````

```` sql
select
  c.black_id as 'Player with the Highest Rating in Black',
  c.black_rating as 'Rating in Black'
from
  chessgame.chess_games c
where
  c.black_rating = (select
                      max(c.black_rating)
                    from
                      chessgame.chess_games c)
group by
  c.black_id, c.black_rating

````

####  Which user won the most amount of games? In what percentage of those games was the user the higher rated player?

