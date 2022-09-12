# SQL #

- Yesterday

```SELECT column_date FROM table WHERE column_date = CURRENT_DATE() -1```

- Substract 10 days

```SELECT DATE_SUB("2022-06-15", INTERVAL 10 DAY);```

- Date format

```DATE_FORMAT(date_variable, '%Y-%m')```

- https://www.mysqltutorial.org/mysql-date/#:~:text=MySQL%20uses%20yyyy%2Dmm%2Ddd,date%20the%20way%20you%20want.

- Creating variables with coalesce

```COALESCE(CASE WHEN variable LIKE 'finish_with%' THEN 1 end ,0) AS new_variable```


### DbConn

- ```rm(dbConn)```
