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

