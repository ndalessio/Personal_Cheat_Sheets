# R #

### Basics

- ```typeof(df)```
- ```subset(df, v == xxx)```
- ```nrow(df)```
- ```ncol(df)```
- ```dim(df)```
- ```table(df$v)```
- ```dplyr::count(df, v, sort = TRUE) -- Value counts```
- ```df %>% group_by(v) %>% summarise(n = n()) %>% mutate(Freq = n/sum(n)) -- Frequency by tidyverse```

### Exploration

- ```quantile(df$v, na.rm = T, probs = c(0.0001, 0.001, 0.01, 0.05, 0.1, 0.15, 0.2, 0.3, 0.90, 0.95, 0.99, 0.999))```

### Dates 

- ```yesterday <- Sys.Date() - days(1)```
- ```start_current_month = as.Date(paste0(substr(Sys.Date(),1,7),"-01"))```
- ```start_current_month - months(1) -- start previous month```
- ```format(date,"%d/%m/%Y")```
- ```format(as.Date(df$date), "%b-%Y") -- "March 1993"```
- ```as.Date(as.POSIXct(date_variable, tz=''))```
- ```df$date_variable = as.POSIXct(paste(date_variable,'00:00:01'),tz='')```
- ```week_ending = ceiling_date(date, unit='weeks')-days(1) --Week ending```
- ```as.numeric(difftime(as.Date(df$date1), as.Date(df$date2), units='mins')) -- Difference in minutes between 2 timestamps```
- ```previous_saturday <- floor_date(Sys.Date(), "week") - days(1) -- lubridate``` 

- The data in the R data frame is with the timezone CEST. When writing to excel, excel automatically changes the timezone to GMT which causes the time shift in excel. One workaround is by changing the timezone in R to GMT without changing the time data, by using force_tz.
```df$date = as.POSIXct(strptime(df$date, "%Y-%M-%d %H:%M:%S"))

Sys.setenv(TZ="")    

df$date = force_tz(df$date,tzone="GMT")

Now can be saved in excel- ```

### Duplicates

- ```df[duplicated(df$v),] -- Find duplicates in a specific variable```
- ```unique(df[, c("v1", "v2")]) -- Uniques combinations```

### Missing data

- ```newdata <- na.omit(df) -- drop NA```
- ```df[is.na(df)] <- 0 -- Replace all NA```
- ```df$v[is.na(df$v)] <- 0 -- Replace NA in specific column```
- ```names(which(colSums(is.na(df))>0)) -- Returns columns names qhere are NA```
- ```df[rowSums(is.na(df)) > 0,] -- Returns rows with NA```
- ```df[!complete.cases(df),] -- Returns rows with NA```
- ```df[is.na(df$v),] -- Returns row but checking NA in specific columns```

### Drop columns
- ```df <- df[, !(colnames(df) %in% c("v1","v2))] ```
- ```df <- df[, c("foo","bar"):= NULL]```
- ```df %>% drop_na(column)```

### Column names
- ```colnames(df)[colnames(df) == 'col_old_name'] <- 'cold_new_name'```
- ```names(df) <-c(cold_new_name_1','cold_new_name_2') -- In this case, assuming there are only two columns```
- ```df <- setcolorder(summary_games, c("v2", "v1", "v3")) -- data.table package```
- ```select(v1, v2, everything()) -- re order v1, v2, andd then the rest```

### Rolling average
- ```mutate(V_Rolling_Avg = lag(round(zoo::rollmean(Vs, k = 2, fill = NA, align ="right"), 2), 1, default = NA)) -- lag in this case "lowers" it one row```

### Dummy variables
- ```dummies <- dcast(df, v1 ~ v2, fun.aggregate = length) -- Reshape2 packege```

### Format

- ```format(as.numeric(number), big.mark=",")```

### data.table

https://cran.r-project.org/web/packages/data.table/vignettes/datatable-intro.html

- ```f = data.table(df)```
- ```df[ ,total := sum(gross), by=.(v1,v2)]```
- ```df[ ,big_numbers := ifelse(total >= 1000, 1, 0), by=.(v1,v2)]```
- ```df[, .(n = .N, n_big_numbers = sum(big_numbers)), by=c("v1", "v2")]``` 
- ```df[total >= 1000, .(UnN = uniqueN(outlet_id))] -- How many outlets have sold > 1000```
- ```unique(df[,c("v1", "v2")])```
- ```df[total > 1000, .(number_cases = length(unique(v1, v2))), by = v3][number_cases > 0][order(-number_cases)]```

- ```df[ , (cols) := lapply(.SD, nafill, fill=0), .SDcols = cols]```

### Completing cases

Useful for time series 

```
%>% complete(date = seq.Date(as.Date(from), as.Date(to), by="day"), fill = list(v1 = 0, v2 = 0)) -- tidyr::complete()
all_dates = seq(min(dft$week_ending),max(df$week_ending), 7)
all = expand.grid(outlet = unique(df$outlet), week_ending=all_dates)
df = all %>% left_join(df)
```

### Pivots
- ```df %>% spread(v1, v2, fill=0) -- tidyr```

### Merge

- ```merge(df1,df2[ ,c(1,2)],all.x=TRUE, by="v1") -- merging just some columns from df2```

### Excel

- ```filename = paste0("name", from, "_", to,".xlsx")```

```write.xlsx(df1, file=filename, sheetName="name1", row.names=FALSE)```

```write.xlsx(df2, file=filename, sheetName="name2", append=TRUE, row.names=FALSE)```

### Running queries

- ```paste0("SELECT * FROM table WHERE date_variable BETWEEN '",from,"' AND '",to,"'")```
- ```sprintf("SELECT *  WHERE date_variable BETWEEN  '%s' AND '%s' GROUP BY 1;", from, to)```

### Patterns

- ```grepl(pattern, x, ignore.case = FALSE, perl = FALSE,fixed = FALSE, useBytes = FALSE)-- returns TRUE or FALSE```
```as.data.frame(df[grepl("some_repetitive_text",df$column),]) -- keeps rows from column that has the repetitive text``` 
```df$column = gsub("A lot of text to remove = ","",df$column) -- removes a lot of text and then leaves the rest```

- Substract
```substr(df$column,1,20) -- keeps spaces to 1 to 20```


### Others

- Terminate R session

https://stat.ethz.ch/R-manual/R-devel/library/base/html/quit.html

```q(save = "no", status = 0, runLast = TRUE)```
- ```df[9,] = c("Total", colSums(df[,2:6]), NA) -- Adds a row with a Total per column, except the last one that leaves NA```
- ```df[9,] = c("Total", colSums(df[,2:6]), NA)```
- ```df[9,7] = (as.numeric(df[9,"column_name"]) - as.numeric(df[9,"column_name_2"]))/as.numeric(df[9,"column_name"] * 100 ```

- case_when (should go from the most specific to more general)

```
 mutate (
    type = case_when(
      height > 200 | mass > 200 ~ "large",
      species == "Droid"        ~ "robot",
      TRUE                      ~  "other"
    )
    
mutate(flag = case_when(
        (condition1) & (condition2) ~ 1, 
        TRUE ~ 0)
        )
        
if_else(to < from, true = from, false = to) 
```
