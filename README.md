# Pandas notes

* this is a work in progress

#### Import pandas 
    import pandas as pd

#### Read CSV
```
pd.read_csv('student_scores.csv', sep = ",", header = 0)
# header specifies which row is the column labels
# sep specifies the delimiter, by default this is ","
```

#### To specify your own column labels 
```
labels = ['id', 'name', 'attendance', 'hw', 'test1', 'project1', 'test2', 'project2', 'final']
df = pd.read_csv('student_scores.csv', names=labels)
df.head()
```
#### write to csv
```
df_powerplant.to_csv('powerplant_data_edited.csv', index=False)    
# index is set to false to ignore the index (so an extra column is not created)
```

#### this returns a tuple of the dimensions of the dataframe
    df.shape
    
#### this returns the datatypes of the columns
    df.dtypes

#### this displays a concise summary of the dataframe, including the number of non-null values in each column
    df.info()

#### this returns the number of unique values in each column
    df.nunique()

#### this returns useful descriptive statistics for each column of data
    df.describe()

#### this returns the first few lines in our dataframe, by default, it returns the first five
    df.head()

#### although, you can specify however many rows you'd like returned
    df.head(20)

#### same thing applies to `.tail()` which returns the last few rows
    df.tail(2)

#### View the index number and label for each column
```
for i, v in enumerate(df.columns):
    print(i, v)
```

#### We can select data using loc and iloc, which you can read more about [here](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html). `loc` uses labels of rows or columns to select data, while `iloc` uses the index numbers. We'll use these to index the dataframe below.
```
# select all the columns from 'id' to the last mean column
df_means = df.loc[:,'id':'fractal_dimension_mean']
df_means.head()
```

```
# repeat the step above using index numbers
df_means = df.iloc[:,:12]
df_means.head()
```

## Cleaning

### Issues to look out for:

1. Missing data
2. Duplicates 
3. Incorrect datatypes

Solutions to issues

1. Missing data 

- either we can remove the columns with missing data 
- or for small datasets, we can fill the missing values with the mean, see code below.
```
mean = df['view_duration'].mean()
df['view_duration'] = df['view_duration'].fillna(mean)
```

OR 

```
mean = df['column_name'].mean()
df['column_name'].fillna(mean, inplace = True)
```

2. Duplicates
# count the number of duplicates
    sum(df.duplicated())
# drop duplicates
    df.drop_duplicates(inplace=True)
    
3. Incorrect data types
```
# get data types
df.info()
# convert to datetime data type
df['timestamp'] = pd.to_datetime(df['timestamp'])
```
