# SQL #

## Date thinghys

- Yesterday

```SELECT column_date FROM table WHERE column_date = CURRENT_DATE() -1```

- Week ending (saturday)

```to_date(column_datetime) + 5 - WEEKDAY(to_date(column_datetime)) as Week_Ending```

- Substract 10 days

```SELECT DATE_SUB("2022-06-15", INTERVAL 10 DAY);```

- Date format

```DATE_FORMAT(date_variable, '%Y-%m')```

```date_format(date_variable,'yyyy-MM')```

- https://www.mysqltutorial.org/mysql-date/#:~:text=MySQL%20uses%20yyyy%2Dmm%2Ddd,date%20the%20way%20you%20want.

- Current month

- ``` (select concat_ws('-',year(current_date()),month(current_date()), '01')) ```


## Others

- Desviation

``` variable - LAG(variable) OVER(ORDER BY t1.some_variable) ```

``` ((variable - LAG(variable) OVER(ORDER BY t1.some_variable)) / LAG(variable) OVER(ORDER BY t1.some_variable)) * 100 as Variance_On_Previous_month ```

- Creating variables with coalesce

```COALESCE(CASE WHEN variable LIKE 'finish_with%' THEN 1 end ,0) AS new_variable```

- Force index

```
  SELECT column1, column2 FROM table FORCE_INDEX(column1);
```

- '&' and 'AND' work different in SQL 

Right way:

```
SUM(IF(column1=0  and column2=0 and column3 COLLATE utf8_unicode_ci NOT IN ('A','B'),1,0)) column_count,

-- Also 'COLLATE utf8_unicode_ci variable', in this case I had to change it in column3



### DbConn

- ```rm(dbConn)```
