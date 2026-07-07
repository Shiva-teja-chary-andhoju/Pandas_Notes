# Pandas Notes

A collection of Pandas concepts, code snippets, and outputs.

## Table of Contents — Topics & Functions

1. **Setup**
   - `import pandas as pd`, `pd.__version__`

2. **Creating a DataFrame**
   - `pd.DataFrame(data=..., columns=...)`

3. **Basic DataFrame Exploration**
   - `df.head(n)`, `df.tail(n)`, `df.shape`, `df.columns`, `df.info()`, `df.describe()`

4. **Renaming Columns**
   - `df.rename(columns={...})`, `inplace=True` parameter

5. **Save/Load CSV**
   - `df.to_csv()`, `index=False` parameter, `pd.read_csv()`

6. **Row & Column Selection**
   - Single/multiple column selection `df[['col']]`
   - `df.loc[]` — label-based indexing (single & multiple conditions)
   - `df.iloc[]` — position-based indexing
   - Slicing differences: `iloc` (stop exclusive) vs `loc` (stop inclusive)

7. **Filtering**
   - Boolean masks `df[df['col'] > x]`
   - Multiple conditions with `&`
   - `df.where(condition, other=...)`

8. **Add / Update / Delete Rows & Columns**
   - Creating columns via assignment
   - Derived columns (`df['Bonus'] = df['Salary'] * 0.2`)
   - `len(df)`
   - Adding a row via `df.loc[len(df)] = [...]`
   - Updating values via `df.loc[row, col] = value`
   - `df.drop()` with `axis=0` (rows) / `axis=1` (columns)

9. **Sorting**
   - `df.sort_values(col, ascending=True/False)`

10. **Working with Dates**
    - `dtype` conversion, `pd.to_datetime()`, `format=` parameter
    - `.dt.year`, `.dt.month`, `.dt.day`, `.dt.day_name()`
    - `pd.Timedelta(days=...)` for date arithmetic

11. **Missing Values**
    - `df.isnull()`, `.sum()` for null counts
    - `df.fillna(value)`
    - `import numpy as np`, `np.nan`

12. **Aggregation & Group By**
    - `df['col'].value_counts()`
    - `df.groupby(col)['col2'].sum()`
    - `df.groupby(col).agg({...})` — multiple aggregations per column

13. **Concatenate & Merge (Joins)**
    - `pd.concat([df1, df2], axis=0/1)` — vertical vs horizontal
    - `pd.merge(df1, df2, how='inner'/'left'/'right'/'outer', on=...)`
    - `left_on` / `right_on` for differently named join keys

14. **Query**
    - `df.query("condition")` — SQL-like filtering

15. **Pivot Tables**
    - `pd.pivot_table(df, values=, index=, aggfunc=)`
    - Multiple `columns=` for cross-tab style pivots
    - Multiple `aggfunc=['sum', 'mean', 'max']`
    - Multiple `index=[...]` (multi-level index)
    - `fill_value=0`

---

## What is Pandas?

Python library used for working with structured data sets (tables). It has functions for analyzing, cleaning, exploring, and manipulating data. Similar to data tables in SQL and Excel.


```python
#pip install ipykernel
#pip install pandas
#save file as .ipynb in vscode

import pandas as pd
```


```python
pd.__version__
```

**Output:**
```
'3.0.3'
```


## What is Dataframe?

It's a structured data constructed with rows and columns, similar to a SQL tables or Excel tables


```python
#1.create dataframe 
#hover over function name to know more about it.
#every thing inside the dataframe function should be in square brackets
df = pd.DataFrame(data = [11,22,33],columns=['col_name'])
print(df)
```

**Output:**
```
   col_name
0        11
1        22
2        33
```


```python
print(type(df))
```

**Output:**
```
<class 'pandas.DataFrame'>
```


```python
testdata = { 
    'Name' : ['shiva','teja','chary','andhoju'],
    'Age' : [25,26,27,28],
    'Salary' : [50000,60000,70000,80000]
}
```


```python
df = pd.DataFrame(data = testdata)
print(df)
```

**Output:**
```
      Name  Age  Salary
0    shiva   25   50000
1     teja   26   60000
2    chary   27   70000
3  andhoju   28   80000
```


```python
#2.basic dataframe understanding
df.head(2) #by default gives top 5
```

**Output:**
```
    Name  Age  Salary
0  shiva   25   50000
1   teja   26   60000
```


```python
df.head()
```

**Output:**
```
      Name  Age  Salary
0    shiva   25   50000
1     teja   26   60000
2    chary   27   70000
3  andhoju   28   80000
```


```python
df.tail(2) #by default gives last 5 rows
```

**Output:**
```
      Name  Age  Salary
2    chary   27   70000
3  andhoju   28   80000
```


```python
df.tail()
```

**Output:**
```
      Name  Age  Salary
0    shiva   25   50000
1     teja   26   60000
2    chary   27   70000
3  andhoju   28   80000
```


```python
# returns a tuple containing the shape of the DataFrame - (rows , columns)
df.shape
```

**Output:**
```
(4, 3)
```


```python
df.columns #list out columns
```

**Output:**
```
Index(['Name', 'Age', 'Salary'], dtype='str')
```


```python
df.rename(columns = {'Salary':'monthly_salary'})   #give in a key:value pair (dictionary)
```

**Output:**
```
      Name  Age  monthly_salary
0    shiva   25           50000
1     teja   26           60000
2    chary   27           70000
3  andhoju   28           80000
```


```python
df
```

**Output:**
```
      Name  Age  Salary
0    shiva   25   50000
1     teja   26   60000
2    chary   27   70000
3  andhoju   28   80000
```


```python
#why name didn't changed? you need to store it another variable after change.
df1 = df.rename(columns = {'Salary':'monthly_salary'})
```


```python
df1
```

**Output:**
```
      Name  Age  monthly_salary
0    shiva   25           50000
1     teja   26           60000
2    chary   27           70000
3  andhoju   28           80000
```


```python
df
```

**Output:**
```
      Name  Age  Salary
0    shiva   25   50000
1     teja   26   60000
2    chary   27   70000
3  andhoju   28   80000
```


```python
#or use inplace in the function, means make changes in that place itself
df.rename(columns = {'Salary':'monthly_salary'},inplace = [True])
```


```python
df
```

**Output:**
```
      Name  Age  monthly_salary
0    shiva   25           50000
1     teja   26           60000
2    chary   27           70000
3  andhoju   28           80000
```


```python
df.info()    # info method prints information about the DataFrame
```

**Output:**
```
<class 'pandas.DataFrame'>
RangeIndex: 4 entries, 0 to 3
Data columns (total 3 columns):
 #   Column          Non-Null Count  Dtype
---  ------          --------------  -----
 0   Name            4 non-null      str  
 1   Age             4 non-null      int64
 2   monthly_salary  4 non-null      int64
dtypes: int64(2), str(1)
memory usage: 228.0 bytes
```


```python
df.describe() # describe method generates descriptive statistics of DataFrame, only for numerical-value columns
```

**Output:**
```
             Age  monthly_salary
count   4.000000        4.000000
mean   26.500000    65000.000000
std     1.290994    12909.944487
min    25.000000    50000.000000
25%    25.750000    57500.000000
50%    26.500000    65000.000000
75%    27.250000    72500.000000
max    28.000000    80000.000000
```


```python
#3. Save and Load data from csv
df.to_csv('test_data.csv')   # save file - export data frame - will be saved in current folder
```


```python
df.to_csv('test_data.csv',index= False) # to remove index at starting 
```


```python
#change the salary from 50000 to 100000 in the file manually and load

load_df = pd.read_csv('test_data.csv') # load file - import dataframe
#different functions for load different file types.
load_df
```

**Output:**
```
      Name  Age  monthly_salary
0    shiva   25           50000
1     teja   26           60000
2    chary   27           70000
3  andhoju   28           80000
```


```python
#4. Rows & Columns - Selection
# select single column
df[['Name']]
```

**Output:**
```
      Name
0    shiva
1     teja
2    chary
3  andhoju
```


```python
df[['Name','Age']] #select multiple columns
```

**Output:**
```
      Name  Age
0    shiva   25
1     teja   26
2    chary   27
3  andhoju   28
```


```python
# select single row || loc - label based index
df.loc[df.Name == 'shiva']
# single condition
```

**Output:**
```
    Name  Age  monthly_salary
0  shiva   25           50000
```


```python
#multiple conditions
df.loc[(df.Name == 'shiva')&(df.monthly_salary >= 50000)]
```

**Output:**
```
    Name  Age  monthly_salary
0  shiva   25           50000
```


```python
# Select Rows || iloc - index-value based
df.iloc[0]
```

**Output:**
```
Name              shiva
Age                  25
monthly_salary    50000
Name: 0, dtype: object
```


```python
df.iloc[0:2]  # [start:stop:step]  start is inclusive and stop is exclusive
```

**Output:**
```
    Name  Age  monthly_salary
0  shiva   25           50000
1   teja   26           60000
```


```python
df.loc[0:2]  # [start:stop:step]  start and stop is inclusive 
```

**Output:**
```
    Name  Age  monthly_salary
0  shiva   25           50000
1   teja   26           60000
2  chary   27           70000
```


```python
#5. Filter Dataframe - Filtering by column values
df
```

**Output:**
```
      Name  Age  monthly_salary
0    shiva   25           50000
1     teja   26           60000
2    chary   27           70000
3  andhoju   28           80000
```


```python
df['Age'] > 26
```

**Output:**
```
0    False
1    False
2     True
3     True
Name: Age, dtype: bool
```


```python
df[df['Age'] > 26] #indexes with True will be shown in output - first df[] for table second df[] for column
```

**Output:**
```
      Name  Age  monthly_salary
2    chary   27           70000
3  andhoju   28           80000
```


```python
df_age_filter = df[df['Age'] > 26] # filter and store dataframe in a new variable
df_age_filter
```

**Output:**
```
      Name  Age  monthly_salary
2    chary   27           70000
3  andhoju   28           80000
```


```python
df_age_salary_filter = df[(df['Age'] > 26) & (df['monthly_salary']>=50000)] # multiple filter conditions
df_age_salary_filter
```

**Output:**
```
      Name  Age  monthly_salary
2    chary   27           70000
3  andhoju   28           80000
```


```python
# where function replace values with NaN in a DataFrame based on a condition

df.where(df['Age'] > 26)
```

**Output:**
```
      Name   Age  monthly_salary
0      NaN   NaN             NaN
1      NaN   NaN             NaN
2    chary  27.0         70000.0
3  andhoju  28.0         80000.0
```


```python
df.where(df['Age'] > 26,other= 'Not Eligible') #replce NaN default
```

**Output:**
```
           Name           Age monthly_salary
0  Not Eligible  Not Eligible   Not Eligible
1  Not Eligible  Not Eligible   Not Eligible
2         chary            27          70000
3       andhoju            28          80000
```


```python
#6 Rows and Columns - Operation (Add, update, delete)
df
```

**Output:**
```
      Name  Age  monthly_salary
0    shiva   25           50000
1     teja   26           60000
2    chary   27           70000
3  andhoju   28           80000
```


```python
df['Team'] = ['CEO','HR','CTO','DA'] # Create a new column
df
```

**Output:**
```
      Name  Age  monthly_salary Team
0    shiva   25           50000  CEO
1     teja   26           60000   HR
2    chary   27           70000  CTO
3  andhoju   28           80000   DA
```


```python
df['Bonus'] = df['monthly_salary'] * 0.2 # Add new columns using existing column/s
df
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus
0    shiva   25           50000  CEO  10000.0
1     teja   26           60000   HR  12000.0
2    chary   27           70000  CTO  14000.0
3  andhoju   28           80000   DA  16000.0
```


```python
len(df) #number of rows in the dataframe 
```

**Output:**
```
4
```


```python
df.loc[len(df)] = ['balram',29,50000,'IT',10000] # Add new row - at the end of dataframe, so we give the dataframe total length
df
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus
0    shiva   25           50000  CEO  10000.0
1     teja   26           60000   HR  12000.0
2    chary   27           70000  CTO  14000.0
3  andhoju   28           80000   DA  16000.0
4   balram   29           50000   IT  10000.0
```


```python
len(df)
```

**Output:**
```
5
```


```python
# update value in dataframe using index-name
df.loc[0,'monthly_salary'] = 100000
df
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus
0    shiva   25          100000  CEO  10000.0
1     teja   26           60000   HR  12000.0
2    chary   27           70000  CTO  14000.0
3  andhoju   28           80000   DA  16000.0
4   balram   29           50000   IT  10000.0
```


```python
# update value in dataframe using column-value
df.loc[df.Name == 'teja','monthly_salary'] = 65000
df
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus
0    shiva   25          100000  CEO  10000.0
1     teja   26           65000   HR  12000.0
2    chary   27           70000  CTO  14000.0
3  andhoju   28           80000   DA  16000.0
4   balram   29           50000   IT  10000.0
```


```python
# delete value - rows and columns
# delete row using column-value filter
# drop the row in the index
# Axis = 0 is row and Axis = 1 is column and by default Axis = 0 in the formula
# df[df.Name == 'balram'] is filter
df.drop(df[df.Name == 'balram'].index) # drop row, Axis = 0
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus
0    shiva   25          100000  CEO  10000.0
1     teja   26           65000   HR  12000.0
2    chary   27           70000  CTO  14000.0
3  andhoju   28           80000   DA  16000.0
```


```python
df[df.Name == 'balram'] 
```

**Output:**
```
     Name  Age  monthly_salary Team    Bonus
4  balram   29           50000   IT  10000.0
```


```python
df.drop('Bonus',axis = 1) #need to change axis = 1 as we are dropping column
```

**Output:**
```
      Name  Age  monthly_salary Team
0    shiva   25          100000  CEO
1     teja   26           65000   HR
2    chary   27           70000  CTO
3  andhoju   28           80000   DA
4   balram   29           50000   IT
```


```python
df
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus
0    shiva   25          100000  CEO  10000.0
1     teja   26           65000   HR  12000.0
2    chary   27           70000  CTO  14000.0
3  andhoju   28           80000   DA  16000.0
4   balram   29           50000   IT  10000.0
```


```python
#we need to either use inplace or assign the output again to df
#df.drop('Bonus',axis = 1,inplace = True)
df = df.drop(df[df.Name == 'balram'].index)
df
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus
0    shiva   25          100000  CEO  10000.0
1     teja   26           65000   HR  12000.0
2    chary   27           70000  CTO  14000.0
3  andhoju   28           80000   DA  16000.0
```


```python
# df.drop(['Bonus', 'Team'], axis=1, inplace=True) # delete multiple columns
```


```python
# delete row using index-name filter
df.drop(1, axis=0)
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus
0    shiva   25          100000  CEO  10000.0
2    chary   27           70000  CTO  14000.0
3  andhoju   28           80000   DA  16000.0
```


```python
df
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus
0    shiva   25          100000  CEO  10000.0
1     teja   26           65000   HR  12000.0
2    chary   27           70000  CTO  14000.0
3  andhoju   28           80000   DA  16000.0
```


```python
# sort values - order values in dataframe by asc or desc order
#ascending order, by default

df.sort_values('monthly_salary') #asc
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus
1     teja   26           65000   HR  12000.0
2    chary   27           70000  CTO  14000.0
3  andhoju   28           80000   DA  16000.0
0    shiva   25          100000  CEO  10000.0
```


```python
df.sort_values('monthly_salary',ascending= False) #desc
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus
0    shiva   25          100000  CEO  10000.0
3  andhoju   28           80000   DA  16000.0
2    chary   27           70000  CTO  14000.0
1     teja   26           65000   HR  12000.0
```


```python
#7 working with date value
# create a date column - date of joining
df['DOJ'] = ['2026-07-01','2026-07-02','2026-07-03','2026-07-04']
df
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus         DOJ
0    shiva   25          100000  CEO  10000.0  2026-07-01
1     teja   26           65000   HR  12000.0  2026-07-02
2    chary   27           70000  CTO  14000.0  2026-07-03
3  andhoju   28           80000   DA  16000.0  2026-07-04
```


```python
df['DOJ'].dtype
```

**Output:**
```
<StringDtype(storage='python', na_value=nan)>
```


```python
df['DOJ'] = pd.to_datetime(df['DOJ']) #conversion to datetime
```


```python
df['DOJ'].dtype
```

**Output:**
```
dtype('<M8[us]')
```


```python
df1 = df
```


```python
# creating a new column with incorrect date format
df1['DOJ2'] = ['01-07-2026','02-07-2026','03-07-2026','04-07-2026']
df1 
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus        DOJ        DOJ2
0    shiva   25          100000  CEO  10000.0 2026-07-01  01-07-2026
1     teja   26           65000   HR  12000.0 2026-07-02  02-07-2026
2    chary   27           70000  CTO  14000.0 2026-07-03  03-07-2026
3  andhoju   28           80000   DA  16000.0 2026-07-04  04-07-2026
```


```python
df1['DOJ2'].dtype
```

**Output:**
```
<StringDtype(storage='python', na_value=nan)>
```


```python
df1['DOJ2'] = pd.to_datetime(df1['DOJ2']) #conversion to datetime
df1
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus        DOJ       DOJ2
0    shiva   25          100000  CEO  10000.0 2026-07-01 2026-01-07
1     teja   26           65000   HR  12000.0 2026-07-02 2026-02-07
2    chary   27           70000  CTO  14000.0 2026-07-03 2026-03-07
3  andhoju   28           80000   DA  16000.0 2026-07-04 2026-04-07
```


```python
df1['DOJ2'].dtype
```

**Output:**
```
dtype('<M8[us]')
```


```python
#use format inside the function to let know which is month,day and year
# change to date-time type using date-format
df1['DOJ2'] = pd.to_datetime(df1['DOJ2'],format = '%d-%m-%Y')
```


```python
df1
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus        DOJ       DOJ2
0    shiva   25          100000  CEO  10000.0 2026-07-01 2026-01-07
1     teja   26           65000   HR  12000.0 2026-07-02 2026-02-07
2    chary   27           70000  CTO  14000.0 2026-07-03 2026-03-07
3  andhoju   28           80000   DA  16000.0 2026-07-04 2026-04-07
```


```python
df
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus        DOJ       DOJ2
0    shiva   25          100000  CEO  10000.0 2026-07-01 2026-01-07
1     teja   26           65000   HR  12000.0 2026-07-02 2026-02-07
2    chary   27           70000  CTO  14000.0 2026-07-03 2026-03-07
3  andhoju   28           80000   DA  16000.0 2026-07-04 2026-04-07
```


```python
df = df.drop('DOJ2',axis = 1)
df
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus        DOJ
0    shiva   25          100000  CEO  10000.0 2026-07-01
1     teja   26           65000   HR  12000.0 2026-07-02
2    chary   27           70000  CTO  14000.0 2026-07-03
3  andhoju   28           80000   DA  16000.0 2026-07-04
```


```python
# extract year, month, week, day
df['DOJ'].dt.year
```

**Output:**
```
0    2026
1    2026
2    2026
3    2026
Name: DOJ, dtype: int32
```


```python
df['DOJ'].dt.month
```

**Output:**
```
0    7
1    7
2    7
3    7
Name: DOJ, dtype: int32
```


```python
df['DOJ'].dt.day
```

**Output:**
```
0    1
1    2
2    3
3    4
Name: DOJ, dtype: int32
```


```python
df['DOJ'].dt.day_name()
```

**Output:**
```
0    Wednesday
1     Thursday
2       Friday
3     Saturday
Name: DOJ, dtype: str
```


```python
df['Month'] = df['DOJ'].dt.month # create new column using month extract function from DOJ column
df
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus        DOJ  Month
0    shiva   25          100000  CEO  10000.0 2026-07-01      7
1     teja   26           65000   HR  12000.0 2026-07-02      7
2    chary   27           70000  CTO  14000.0 2026-07-03      7
3  andhoju   28           80000   DA  16000.0 2026-07-04      7
```


```python
# timedelta adds or subs values from dates
df['DOJ'] + pd.Timedelta(days = 10) # add 10 days to DOJ column
```

**Output:**
```
0   2026-07-11
1   2026-07-12
2   2026-07-13
3   2026-07-14
Name: DOJ, dtype: datetime64[us]
```


```python
#8. Handling Missing Values
df.isnull() #  Detect missing values
```

**Output:**
```
    Name    Age  monthly_salary   Team  Bonus    DOJ  Month
0  False  False           False  False  False  False  False
1  False  False           False  False  False  False  False
2  False  False           False  False  False  False  False
3  False  False           False  False  False  False  False
```


```python
#pip install numpy
import numpy as np
```


```python
df.loc[df.Name == 'teja','monthly_salary'] = np.nan # adding a null value
df
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus        DOJ  Month
0    shiva   25        100000.0  CEO  10000.0 2026-07-01      7
1     teja   26             NaN   HR  12000.0 2026-07-02      7
2    chary   27         70000.0  CTO  14000.0 2026-07-03      7
3  andhoju   28         80000.0   DA  16000.0 2026-07-04      7
```


```python
df.isnull()
```

**Output:**
```
    Name    Age  monthly_salary   Team  Bonus    DOJ  Month
0  False  False           False  False  False  False  False
1  False  False            True  False  False  False  False
2  False  False           False  False  False  False  False
3  False  False           False  False  False  False  False
```


```python
df.isnull().sum() # count of null values by columns
```

**Output:**
```
Name              0
Age               0
monthly_salary    1
Team              0
Bonus             0
DOJ               0
Month             0
dtype: int64
```


```python
df.fillna(0)  # fill null values with 0
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus        DOJ  Month
0    shiva   25        100000.0  CEO  10000.0 2026-07-01      7
1     teja   26             0.0   HR  12000.0 2026-07-02      7
2    chary   27         70000.0  CTO  14000.0 2026-07-03      7
3  andhoju   28         80000.0   DA  16000.0 2026-07-04      7
```


```python
#9. Aggregation and Group By
df['Team'].value_counts()  # frequency of values in month column
```

**Output:**
```
Team
CEO    1
HR     1
CTO    1
DA     1
Name: count, dtype: int64
```


```python
df['Month'].value_counts() 
```

**Output:**
```
Month
7    4
Name: count, dtype: int64
```


```python
df[df['Bonus']>=13000].value_counts()  # frequency of values in AGE column where age=25
```

**Output:**
```
Name     Age  monthly_salary  Team  Bonus    DOJ         Month
chary    27   70000.0         CTO   14000.0  2026-07-03  7        1
andhoju  28   80000.0         DA    16000.0  2026-07-04  7        1
Name: count, dtype: int64
```


```python
# aggregation based on group by
df.groupby('Month')['monthly_salary'].sum() # sum of salary by month
```

**Output:**
```
Month
7    250000.0
Name: monthly_salary, dtype: float64
```


```python
# different aggregation on different columns
df.groupby('Month').agg({'monthly_salary':'sum','Bonus':'sum','Name':'count'})
```

**Output:**
```
       monthly_salary    Bonus  Name
Month                               
7            250000.0  52000.0     4
```


```python
#10. Concatenate and Merge Dataframe (JOINS)
df1 = pd.DataFrame({'ID':[1,2,3],'Name':['A','B','C']})
df1
```

**Output:**
```
   ID Name
0   1    A
1   2    B
2   3    C
```


```python
df2 = pd.DataFrame({'ID':[1,2,2,4],'Score':[88,96,77,79]})
df2
```

**Output:**
```
   ID  Score
0   1     88
1   2     96
2   2     77
3   4     79
```


```python
#What is Concatenate:
#Combine objects along a particular axis.
# Concatenate: vertical / row level / top on top
#Axis = 0 is vertical join, Axis = 1 is horizontal join
#by default outer join
pd.concat([df1,df2],axis=0)
```

**Output:**
```
   ID Name  Score
0   1    A    NaN
1   2    B    NaN
2   3    C    NaN
0   1  NaN   88.0
1   2  NaN   96.0
2   2  NaN   77.0
3   4  NaN   79.0
```


```python
# Concatenate: horizontal / column level / side on side
pd.concat([df1,df2],axis=1)
```

**Output:**
```
    ID Name  ID  Score
0  1.0    A   1     88
1  2.0    B   2     96
2  3.0    C   2     77
3  NaN  NaN   4     79
```


```python
#Merge in Pandas
#Performs join operations similar to relational databases like SQL JOINS
pd.merge(df1,df2,how= 'inner',on='ID') #on when both tables have same named joining column
```

**Output:**
```
   ID Name  Score
0   1    A     88
1   2    B     96
2   2    B     77
```


```python
pd.merge(df1,df2,how= 'left',on='ID')
```

**Output:**
```
   ID Name  Score
0   1    A   88.0
1   2    B   96.0
2   2    B   77.0
3   3    C    NaN
```


```python
pd.merge(df1,df2,how= 'right',on='ID')
```

**Output:**
```
   ID Name  Score
0   1    A     88
1   2    B     96
2   2    B     77
3   4  NaN     79
```


```python
pd.merge(df1,df2,how= 'outer',on='ID')
```

**Output:**
```
   ID Name  Score
0   1    A   88.0
1   2    B   96.0
2   2    B   77.0
3   3    C    NaN
4   4  NaN   79.0
```


```python
pd.merge(df1,df2,how= 'inner',left_on='ID',right_on = 'ID') #left_on, right_on when both tables have different named joining column
```

**Output:**
```
   ID Name  Score
0   1    A     88
1   2    B     96
2   2    B     77
```


```python
#query method for filtering data #similar to SQL
df.query("Age>=26 and monthly_salary>= 60000")
```

**Output:**
```
      Name  Age  monthly_salary Team    Bonus        DOJ  Month
2    chary   27         70000.0  CTO  14000.0 2026-07-03      7
3  andhoju   28         80000.0   DA  16000.0 2026-07-04      7
```


```python

testdata = {
    'Name': ['shiva', 'teja', 'chary', 'andhoju',
             'shiva', 'teja', 'chary', 'andhoju'],
    'Age': [25, 26, 27, 28,
            25, 26, 27, 28],
    'Department': ['IT', 'HR', 'IT', 'Finance',
                   'IT', 'HR', 'Finance', 'Finance'],
    'Salary': [50000, 60000, 70000, 80000,
               55000, 65000, 75000, 85000]
}
```


```python

df = pd.DataFrame(testdata)
df
```

**Output:**
```
      Name  Age Department  Salary
0    shiva   25         IT   50000
1     teja   26         HR   60000
2    chary   27         IT   70000
3  andhoju   28    Finance   80000
4    shiva   25         IT   55000
5     teja   26         HR   65000
6    chary   27    Finance   75000
7  andhoju   28    Finance   85000
```


```python

#Pivot Table - Average Salary by Name
pd.pivot_table(
    df,
    values='Salary',
    index='Name',
    aggfunc='mean'
)
```

**Output:**
```
          Salary
Name            
andhoju  82500.0
chary    72500.0
shiva    52500.0
teja     62500.0
```


```python
'''
pd.pivot_table(
    data=df,
    values='column_to_aggregate',
    index='row_grouping_columns',
    columns='column_grouping_columns',
    aggfunc='sum'  # mean, count, min, max, etc.
)
'''
#Pivot Table - Department vs Name

pd.pivot_table(
    df,
    values='Salary',
    index='Department',
    columns='Name',
    aggfunc='sum',
    fill_value=0
)
```

**Output:**
```
Name        andhoju  chary   shiva    teja
Department                                
Finance      165000  75000       0       0
HR                0      0       0  125000
IT                0  70000  105000       0
```


```python
#Pivot Table - Multiple Aggregations

pd.pivot_table(
    df,
    values='Salary',
    index='Department',
    aggfunc=['sum', 'mean', 'max']
)
```

**Output:**
```
               sum          mean    max
            Salary        Salary Salary
Department                             
Finance     240000  80000.000000  85000
HR          125000  62500.000000  65000
IT          175000  58333.333333  70000
```


```python
#Pivot Table with Multiple Indexes

pd.pivot_table(
    df,
    values='Salary',
    index=['Department', 'Name'],
    aggfunc='sum'
)
```

**Output:**
```
                    Salary
Department Name           
Finance    andhoju  165000
           chary     75000
HR         teja     125000
IT         chary     70000
           shiva    105000
```

