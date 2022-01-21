# Advanced SQL - DB: Postgres

---

# Window Function

---

## Introduction

---

```sql
SELECT
	year, 
    event, 
    country
FROM Summer_Medals
WHERE 1=1 
	AND Medal = 'Gold'
;
```

![](img/1.png)

---

- Using `ROW_NUMBER`

```sql
SELECT
	year, 
	event, 
	country,
	ROW_NUMBER() OVER () AS Row_N
FROM Summer_Medals
WHERE 1=1
	AND Medal = 'Gold'
;
```

![](img/2.png)

### Anatomy of a window function

FUNCTION_NAME() OVER (...)
- ORDER BY
- PARTITION BY
- ROWS/RANGE PRECEDING/FOLLOWING/UNBOUNDED

> ORDER BY in OVER orders the rows related to the current rows. Example: Ordering by year in descending order in ROW_NUMBER 's OVER clause will assign 1 to the most recent year's rows

- Order By

```sql
SELECT
	year, 
	event, 
	country,
	ROW_NUMBER() OVER (ORDER BY year DESC) AS Row_N
FROM Summer_Medals
WHERE 1=1
	AND Medal = 'Gold'
;
```

![](img/3.png)

---

- Order by multiple columns

```sql
SELECT
	year, 
	event, 
	country,
	ROW_NUMBER() OVER (ORDER BY year DESC, event ASC) AS Row_N
FROM Summer_Medals
WHERE 1=1
	AND Medal = 'Gold'
;
```

![](img/4.png)

---

- Ordering in- and outside OVER

```sql
SELECT
	year, 
	event, 
	country,
	ROW_NUMBER() OVER (ORDER BY year DESC, event ASC) AS Row_N
FROM Summer_Medals
WHERE 1=1
	AND Medal = 'Gold'
ORDER BY country ASC, Row_N ASC
;
```

![](img/5.png)

> **ORDER BY** `inside` OVER takes effect before **ORDER BY** `outside` OVER

---

### LEAD, LAG

- Current Champion

```sql
SELECT
	year, 
	country AS Champion
FROM summer_medals
WHERE
AND year IN (1996, 2000, 2004, 2008, 2012)
AND gender = 'Men' AND medal = 'Gold'
AND event = 'Discus Throw'
;
```

![](img/6.png)

---

- Current and Last champion

```sql
WITH discus_gold AS (
	SELECT
		year, 
		country AS Champion
	FROM summer_medals
	WHERE
	AND year IN (1996, 2000, 2004, 2008, 2012)
	AND gender = 'Men' AND medal = 'Gold'
	AND event = 'Discus Throw'
)
SELECT
	year, 
	champion,
	LAG(champion, 1) OVER (ORDER BY year ASC) AS last_champion
FROM discus_gold
ORDER BY year ASC
;
```

![](img/7.png)

---

- Current and Last champion :: using partition - One Column

```sql
WITH discus_gold AS (
	SELECT
		year, 
		event, 
		country AS champion
	FROM summer_medals
	WHERE 1=1
	AND year IN (2004, 2008, 2012)
	AND gender = 'Men' AND Medal = 'Gold'
	AND event IN ('Discus Throw','Triple Jump')
	AND gender = 'Men'
)
SELECT
	year, 
	event, 
	champion,
	LAG(champion) OVER (PARTITION BY event ORDER BY event ASC, year ASC) AS last_champion
FROM discus_gold
ORDER BY event ASC, year asc
;
```

![](img/8.png)

---

- **More Complex Partitioning**

![](img/9.png)


```sql
WITH country_gold AS (
	SELECT
	DISTINCT year, country, event
	FROM summer_medals
	WHERE
	year IN (2008, 2012)
	AND country IN ('CHN','JPN')
	AND gender = 'Women' AND medal = 'Gold'
)
SELECT
	year, 
	country, 
	event,
	ROW_NUMBER() OVER (PARTITION BY year, country)
FROM country_gold
;
```

![](img/10.png)


---

## Fetching

---



---

## ranking

---


---

## paging

---


---

## Aggregate window functions

---


---

## frames

---
























































