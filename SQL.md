# SQL #

- Yesterday

```SELECT column_date FROM table WHERE column_date = CURRENT_DATE() -1```

- Substract 10 days

```SELECT DATE_SUB("2022-06-15", INTERVAL 10 DAY);```

- Crreating variables with coalesce
- 
```COALESCE(CASE WHEN variable LIKE 'finish_with%' THEN 1 end ,0) AS new_variable```

### DbConn

- ```rm(dbConn)```
