# Pandas notes

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

#### this displays a concise summary of the dataframe,
# including the number of non-null values in each column
    df.info()

#### this returns the number of unique values in each column
    df.nunique()

#### this returns useful descriptive statistics for each column of data
    df.describe()

# this returns the first few lines in our dataframe
# by default, it returns the first five
    df.head()

# although, you can specify however many rows you'd like returned
    df.head(20)

# same thing applies to `.tail()` which returns the last few rows
    df.tail(2)
