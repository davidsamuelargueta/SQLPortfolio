This project is currently a work in progress and not complete.

###### Which countries have the most ski resorts? Are there noticable clusters?
```` sql
select
  R.Country,
  count(R.Country) as 'Number of Resorts'
from
  skiresorts.resorts R
group by
  R.Country
order by
  count(R.Country) desc
````

###### How do ski seasons vary by location? Does the snow cover reflect this?

###### Which resorts have the highest mountain peaks and elevation changes?

###### What are the best resorts for beginners? What about experts?
