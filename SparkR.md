# SparkR #

# Queries

- Dividing the query into chuncks for large datasets. Then rbinding it

```
x1 = map(query, fn_to_get_data) %>%
     reduce(SparkR::rbind) %>%
     SparkR::cache() %>%
     SparkR::fillna(0) %>%
     SparkR::withColumn("new_column", something)

SparkR::clearCache() -- when we finish using it
```

- Function to pass aggregate function to a list of columns

```
sparkr_agg_listargs <- function(spark_df, agg_cols_list) {
  do.call(SparkR::agg, c(spark_df, agg_cols_list))
}

sum_cols = c("column3", "column4")

df %>%
  SparkR::group_by("column1", "column2") %>% 
  sparkr_agg_listargs(lapply(sum_cols, function(x) SparkR::alias(sum(SparkR::column(x)), x))) 
  
  -- In this case the new variable takes the same name of the old variable
 
 ```
 
 - Cast variable's names
  
 ```
df %>%
SparkR::withColumn("newColumnName",SparkR::concat_ws('-',
     SparkR::cast(.$column1,"bigint"),SparkR::cast(.$column2,"string"),SparkR::cast(.$column3,"int")))

-- In this case, we casted the variables because the doubles were returning .0 decimal 
 ```
 
 
 - SelectExpr

 ```
 df %>%
 SparkR::selectExpr("column1", "column2 as COLUMN_2") -- Extremely useful, similar to select in SQL
  ```
