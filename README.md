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
### Plotting with Pandas

```
%matplotlib inline

# Histograms
df.hist() # display all histograms for all variables
df.hist(figsize=(8,8));
df['col_name'].hist(); # to generate a histogram for a specific column

# Plots
df['col_name'].plot(kind='hist'); # to generate a histogram using the plot function
df['col_name'].value_counts().plot(kind='bar') # this counts up the number of each of the categories then creates a bar plot

# Pie chart
df['col_name'].value_counts().plot(kind='pie', figsize=(8,8));

# See relationships between all variables
pd.plotting.scatter_matrix(df, figsize=(15,15));

# Scatter plot
df.plot(x='var1', y='var2', kind='scatter');

# Box plot
df['col_name'].plot(kind='box');
```

### Indexing 

```
# Splitting data by category

# splitting males and females
df_f = df[df['gender'] == 'F']
df_m = df[df['gender'] == 'M']

# using pandas query
df_f = df.query('gender == "F"')
df_m = df.query('gender == "M"')
```

``` 
# Splitting numeric data
df_h = df[df['radius'] > 13.375]

# using pandas query
df_h = df.query('radius > 13.375')
```

### Plotting overlapping histograms 
```
import matplotlib.pyplot as plt
% matplotlib inline

fig, ax = plt.subplots(figsize =(8,6))
ax.hist(df_b['area_mean'], alpha=0.5, label='benign')
ax.hist(df_m['area_mean'], alpha=0.5, label='malignant')
ax.set_title('Distributions of Benign and Malignant Tumor Areas')
ax.set_xlabel('Area')
ax.set_ylabel('Count')
ax.legend(loc='upper right')
plt.show();
```

### Appending data (joining together two or more data frames)

1. Make sure all of the column names are the same
2. Create a column for the variable that the data frames are separated on
3. Join the tables

```
# We will be joining the red wine and white wine data sets

# Rename columns
df=df.rename(columns = {'old_name' : 'new_name'})
red_df.rename(columns={'total_sulfur-dioxide':'total_sulfur_dioxide'}, inplace=True)

# Create colour columns using NumPy's `repeat()` function
color_red = np.repeat('red', 1599) # create color array for red dataframe
color_white = np.repeat('white', 4898) # create color array for white dataframe
red_df['color'] = color_red # add column using the array you created
white_df['color'] = color_white # add column

# Append dataframes
wine_df = red_df.append(white_df) 
# view dataframe to check for success
wine_df.head()
```

### Pandas Groupby
```
df.groupby('col1').mean() # this gives the means for all columns and groups by each category in col1
df.groupby(['col1', 'col2'], as_index=False)['col3'].mean() # this gives the means for only col3 and groups by col1 and col2

### Creating visualisations 
```
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
% matplotlib inline

wine_df = pd.read_csv('winequality.csv')
wine_df.head()

colors = ['red', 'white'] # create list of colours
wine_df.groupby('color')['quality'].mean().plot(kind='bar', title='Average Wine Quality by Color', color=colors, alpha=.7);
plt.xlabel('Colors', fontsize=18)
plt.ylabel('Quality', fontsize=18)

colors = ['red', 'white']
color_means =wine_df.groupby('color')['quality'].mean()
color_means.plot(kind='bar', title='Average Wine Quality by Color', color=colors, alpha=.7);
plt.xlabel('Colors', fontsize=18)
plt.ylabel('Quality', fontsize=18)
```

https://seaborn.pydata.org/

https://seaborn.pydata.org/examples/index.html
