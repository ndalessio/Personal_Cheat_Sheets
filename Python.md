# Python #

### Installing packages


- ```py -3 -m pip install -- I have found this commands useful sometimes```
- ```conda install -c conda-forge scikit-surprise```
- Installing specific versions

```pip / pipenv install teradatasql==17.10.0.3```

### Pickle 

- ```df.to_pickle('filename.pkl') -- to save models for example```
- ```df = pd.read_pickle('filename.pkl ') ```

### Subset

- ```df.query("v1 < 0")```
- Find those rows that are not in another dataframe

```df1[~df1.isin(df2)].dropna()```

### Na

- ```df = pd.concat([i.append({"v1": np.nan}, ignore_index=True) for _, i in s])```

### Duplicates

- ```df[df.duplicated(["v1", "v2", "v3"])]```
- ```df[df.v1.duplicated(keep=False)].sort_values(by="v1")```

### Inf

- Get those rows with Inf

```df_inf = df[(df == np.inf).any(axis=1)]```

- Drop rows with Inf

```df.replace([np.inf, -np.inf], np.nan, inplace=True) \ .dropna(subset=["col1", "col2"], how="all")```

- ```len(df[df.variable.isin([np.inf])].variable2.unique()) ```
- ```df[df.variable.isin([np.inf])]```

## Data transformation

### Dates

- Grouping by week

```groupby(['V1', pd.Grouper(key='date_variable', freq="W")])```

- Number of months between two dates

```
df['nb_months'] = ((df.date2 - df.date1)/np.timedelta64(1, 'M'))
df['nb_months'] = df['nb_months'].astype(int)
```

### EA

- Removing outliers

``` df = df[df.between(df.quantile(.05), df.quantile(.95))]```

- Removing outliers for specific cols

```
cols = df.columns.difference(["col_wr_dont_need_1", "col_wr_dont_need_2"]) 
df = df[~(df[cols] > df[cols].quantile(.99)).any(axis=1)]
```

- IQR

```
Q1 = df[cols].quantile(0.25)
Q3 = df[cols].quantile(0.75)
IQR = Q3 - Q1
df = df[~((df[cols] < (Q1 - 1.5 * IQR)) |(df[cols] > (Q3 + 1.5 * IQR))).any(axis=1)
```

### Random

-Assign to a group randomly

```
df["rand"] = np.random.randint(0, 100, df.shape[0])
df["v1"] = np.where((df["c1"].isna()) & (df["rand"] >= 0) & (df["rand"] < 40), "something", df["v1"])
```

### Apply and transform

- https://stackoverflow.com/questions/27517425/apply-vs-transform-on-a-group-object

- Division of two columns with groupby

https://stackoverflow.com/questions/42046885/pandas-division-of-two-columns-with-groupby/42451038

```
def divide_two_cols(df):
    df['v3'] = df['v1'] / float(df_sub['v2'].sum())
    return df
    
df.groupby('variable').apply(divide_two_cols)  
df.groupby('state').apply(divide_two_cols)
```

- ```df["new_variable"] = df.groupby([pd.Grouper(key="date", freq="W-SUN"), "v1", "v2"])["v3"].transform('sum')```

- Cumsum how many different x per group 

```df['count'] = df.groupby('id')['x'].apply(lambda x: (~pd.Series(x).duplicated()).cumsum())``` 

-  Modify a variable with conditions

```
conditions = [
    (df[""] < df[""]),
    (df[""] > df[""]),
    (df[""] == df[""])
    ]
    
values = [a, b, c] 

data["new_variable"] = np.select(conditions, values)
```

- np.where inside another np.where
 
https://stackoverflow.com/questions/53597236/pandas-groupby-inside-np-where

```df["ok"] = np.where(df["date"] >= '2021-01-30', np.where(df.groupby(["v1", "v2"])["v3"].transform('sum') > 0, 1, 0), np.nan)```

### Cumsum

- https://stackoverflow.com/questions/39741136/in-python-pandas-using-cumsum-with-groupby-and-reset-of-cumsum-when-value-is-0
- https://stackoverflow.com/questions/61590055/pandas-cumsum-conditional-reset 
- https://stackoverflow.com/questions/47741626/cumsum-within-group-and-reset-on-condition-in-pandas -- Groupby cumsum, reset with 1 
- Cumsum reset

```
a = df["expending"] == 0
df['attrition'] = a.cumsum()-a.cumsum().where(~a).ffill().fillna(0).astype(int)
```

### Rolling

- https://stackoverflow.com/questions/13996302/python-rolling-functions-for-groupby-object

- Basic rolling example

```
df = pd.DataFrame({'month': [1,2,3,4,5,6,7,8,9],'expense': [100,120,90,253.5,55,100,250,150,200]})

df["moving_average"] = df["expense"].rolling(2).mean()

v = df["month"].values[-1]+1
row =[v, np.nan, np.nan]
df.loc[len(df2)] = row

df["moving_average"] = df["moving_average"].shift(1)
df["expense/before"] = round(df["expense"] / df["expense"].shift(1),2)
df["desviation_ma"] = df["expense"] / df["moving_average"]
```

### Pivot Tables, Merge, Stack

- https://towardsdatascience.com/reshape-pandas-dataframe-with-pivot-table-in-python-tutorial-and-visualization-2248c2012a31

- https://www.youtube.com/watch?v=kJsiiPK5sxs&ab_channel=DataTalks

- Unstack

```
df = df.unstack()
df.columns = ["_".join(col).strip() for col in df.columns.values]
```

- Removing ugly index when pivot

```
df = df.pivot_table(index=["v1", "v2"], columns="v3", values="v4").reset_index()
df = df.rename_axis(None, axis=1).reset_index(drop=True) -- this removes it
```

- Flatten pivot table 

https://stackoverflow.com/questions/39273441/flatten-pandas-pivot-table

```
df = df.pivot_table(index=["v1", "v2"], columns="v3", values="v4")
df.columns = df.columns.to_series().str.join('_')
df = df.columns.reset_index()
```

## Merge

- Delete a group if condition is true

```
c = df[(df_date == "2021-09-22") & (df.v1.isna())]
df = (df.merge(c, how='left', indicator=True).query('_merge == "left_only"')[df.columns])
```

### Shift

- https://stackoverflow.com/questions/36339679/how-can-i-get-a-previous-row-from-where-the-condition-is-met-in-data-frame-in-pa

### Others

- Exporting Jupyter notebook as PDF

```
pip install -U notebook-as-pdf

# We also need an additional setup for Chromium. It is used to perform the HTML to PDF conversion. Just run the following code in your code prompt.

Pyppeteer-install

jupyter-nbconvert --to PDFviaHTML example.ipynb
```

- https://stackoverflow.com/questions/51884792/pandas-insert-blank-row-for-each-group-in-pandas
- https://chrisalbon.com/code/python/data_wrangling/pandas_make_new_columns_using_functions/ 


