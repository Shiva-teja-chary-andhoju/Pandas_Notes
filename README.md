# Pandas Notes

Pandas Essentials: Quick Reference.
-👉 [View in NBViewer](https://nbviewer.org/github/Shiva-teja-chary-andhoju/Pandas_Notes/blob/main/pandas.ipynb)

- [Pandas Official Documentation & User Guide](https://pandas.pydata.org/docs/index.html)


## Table of Contents

1. [What is Pandas?](#1-what-is-pandas)
2. [Create DataFrame](#2-create-dataframe)
3. [Basic DataFrame Understanding](#3-basic-dataframe-understanding)
4. [Save and Load Data from CSV](#4-save-and-load-data-from-csv)
5. [Rows & Columns - Selection](#5-rows--columns---selection)
6. [Filter DataFrame](#6-filter-dataframe)
7. [Rows and Columns - Operations (Add, Update, Delete)](#7-rows-and-columns---operations-add-update-delete)
8. [Sort Values](#8-sort-values)
9. [Working with Date Values](#9-working-with-date-values)
10. [Handling Missing Values](#10-handling-missing-values)
11. [Aggregation and Group By](#11-aggregation-and-group-by)
12. [Concatenate and Merge DataFrame (Joins)](#12-concatenate-and-merge-dataframe-joins)
13. [Query Method](#13-query-method)
14. [Pivot Tables](#14-pivot-tables)

---

## 1. What is Pandas?

A Python library for working with structured data sets (tables). It has functions for analyzing, cleaning, exploring, and manipulating data — similar to data tables in SQL and Excel.

A **DataFrame** is structured data built from rows and columns, similar to a SQL table or Excel sheet.

```python
import pandas as pd
pd.__version__
```

---

## 2. Create DataFrame

| Function | Description |
|---|---|
| `pd.DataFrame(data, columns=[...])` | Create a DataFrame from a list or dict |

```python
df = pd.DataFrame(data=[11, 22, 33], columns=['col_name'])

testdata = {
    'Name': ['shiva', 'teja', 'chary', 'andhoju'],
    'Age': [25, 26, 27, 28],
    'Salary': [50000, 60000, 70000, 80000]
}
df = pd.DataFrame(data=testdata)
```

---

## 3. Basic DataFrame Understanding

| Function | Description |
|---|---|
| `.head(n)` | First `n` rows (default 5) |
| `.tail(n)` | Last `n` rows (default 5) |
| `.shape` | Tuple of (rows, columns) |
| `.columns` | List of column names |
| `.rename(columns={...})` | Rename columns (returns new df unless `inplace=True`) |
| `.info()` | Summary info about the DataFrame |
| `.describe()` | Descriptive statistics for numeric columns |

```python
df.head(2)
df.tail(2)
df.shape
df.columns

df1 = df.rename(columns={'Salary': 'monthly_salary'})     # need to assign, else original is unchanged
df.rename(columns={'Salary': 'monthly_salary'}, inplace=True)  # or use inplace

df.info()
df.describe()
```

---

## 4. Save and Load Data from CSV

| Function | Description |
|---|---|
| `.to_csv(path)` | Export DataFrame to a CSV file |
| `.to_csv(path, index=False)` | Export without writing the index column |
| `pd.read_csv(path)` | Load a DataFrame from CSV |

```python
df.to_csv('test_data.csv')
df.to_csv('test_data.csv', index=False)

load_df = pd.read_csv('test_data.csv')
```

---

## 5. Rows & Columns - Selection

| Function | Description |
|---|---|
| `df[['col']]` | Select a single column |
| `df[['col1', 'col2']]` | Select multiple columns |
| `df.loc[condition]` | Label-based selection (supports conditions) |
| `df.iloc[start:stop]` | Index-position based selection (stop exclusive) |
| `df.loc[start:stop]` | Label-based slicing (stop inclusive) |

```python
df[['Name']]
df[['Name', 'Age']]

df.loc[df.Name == 'shiva']                                  # single condition
df.loc[(df.Name == 'shiva') & (df.monthly_salary >= 50000)]  # multiple conditions

df.iloc[0]
df.iloc[0:2]   # start inclusive, stop exclusive
df.loc[0:2]    # start and stop inclusive
```

---

## 6. Filter DataFrame

| Function | Description |
|---|---|
| `df[condition]` | Filter rows where condition is True |
| `df.where(condition, other=value)` | Replace non-matching values (default NaN) |

```python
df['Age'] > 26                     # boolean mask
df[df['Age'] > 26]                 # filtered rows
df[(df['Age'] > 26) & (df['monthly_salary'] >= 50000)]  # multiple conditions

df.where(df['Age'] > 26)
df.where(df['Age'] > 26, other='Not Eligible')
```

---

## 7. Rows and Columns - Operations (Add, Update, Delete)

| Function | Description |
|---|---|
| `df['col'] = [...]` | Add/create a new column |
| `df.loc[len(df)] = [...]` | Add a new row at the end |
| `df.loc[row, 'col'] = value` | Update a value by index |
| `df.loc[condition, 'col'] = value` | Update a value by column condition |
| `df.drop(index)` | Drop row(s) by index (`axis=0`, default) |
| `df.drop('col', axis=1)` | Drop column(s) (`axis=1`) |

```python
df['Team'] = ['CEO', 'HR', 'CTO', 'DA']            # new column
df['Bonus'] = df['monthly_salary'] * 0.2           # derived column
len(df)                                            # row count

df.loc[len(df)] = ['balram', 29, 50000, 'IT', 10000]  # add row

df.loc[0, 'monthly_salary'] = 100000                       # update by index
df.loc[df.Name == 'teja', 'monthly_salary'] = 65000        # update by condition

df.drop(df[df.Name == 'balram'].index)   # drop row by filter
df.drop(1, axis=0)                       # drop row by index
df.drop('Bonus', axis=1)                 # drop column
# df.drop(['Bonus', 'Team'], axis=1, inplace=True)  # drop multiple columns
```

---

## 8. Sort Values

| Function | Description |
|---|---|
| `.sort_values('col')` | Sort ascending (default) |
| `.sort_values('col', ascending=False)` | Sort descending |

```python
df.sort_values('monthly_salary')
df.sort_values('monthly_salary', ascending=False)
```

---

## 9. Working with Date Values

| Function | Description |
|---|---|
| `pd.to_datetime(col)` | Convert column to datetime |
| `pd.to_datetime(col, format='...')` | Convert with an explicit date format |
| `.dt.year / .dt.month / .dt.day` | Extract date parts |
| `.dt.day_name()` | Name of the weekday |
| `pd.Timedelta(days=n)` | Add/subtract a duration from dates |

```python
df['DOJ'] = pd.to_datetime(df['DOJ'])
df['DOJ2'] = pd.to_datetime(df['DOJ2'], format='%d-%m-%Y')  # specify day-month-year format

df['DOJ'].dt.year
df['DOJ'].dt.month
df['DOJ'].dt.day
df['DOJ'].dt.day_name()

df['Month'] = df['DOJ'].dt.month     # new column from extracted part
df['DOJ'] + pd.Timedelta(days=10)    # add 10 days
```

---

## 10. Handling Missing Values

| Function | Description |
|---|---|
| `.isnull()` | Detect missing values (boolean DataFrame) |
| `.isnull().sum()` | Count of null values per column |
| `.fillna(value)` | Fill null values with a given value |

```python
df.isnull()
df.isnull().sum()
df.fillna(0)
```

---

## 11. Aggregation and Group By

| Function | Description |
|---|---|
| `.value_counts()` | Frequency of unique values |
| `.groupby('col')['col2'].sum()` | Aggregate one column by group |
| `.groupby('col').agg({...})` | Different aggregations per column |

```python
df['Team'].value_counts()

df.groupby('Month')['monthly_salary'].sum()
df.groupby('Month').agg({'monthly_salary': 'sum', 'Bonus': 'sum', 'Name': 'count'})
```

---

## 12. Concatenate and Merge DataFrame (Joins)

| Function | Description |
|---|---|
| `pd.concat([df1, df2], axis=0)` | Vertical concat (row-wise, stacked) |
| `pd.concat([df1, df2], axis=1)` | Horizontal concat (column-wise, side by side) |
| `pd.merge(df1, df2, how=..., on=...)` | SQL-style join (`inner`, `left`, `right`, `outer`) |
| `pd.merge(..., left_on=, right_on=)` | Merge when join columns have different names |

```python
pd.concat([df1, df2], axis=0)   # default outer join
pd.concat([df1, df2], axis=1)

pd.merge(df1, df2, how='inner', on='ID')
pd.merge(df1, df2, how='left', on='ID')
pd.merge(df1, df2, how='right', on='ID')
pd.merge(df1, df2, how='outer', on='ID')
pd.merge(df1, df2, how='inner', left_on='ID', right_on='ID')
```

---

## 13. Query Method

| Function | Description |
|---|---|
| `.query("expression")` | Filter rows using a SQL-like expression |

```python
df.query("Age >= 26 and monthly_salary >= 60000")
```

---

## 14. Pivot Tables

| Function | Description |
|---|---|
| `pd.pivot_table(df, values=, index=, aggfunc=)` | Aggregate values grouped by index |
| `pd.pivot_table(..., columns=, fill_value=)` | Cross-tab style pivot with row and column grouping |
| `aggfunc=['sum', 'mean', 'max']` | Multiple aggregations at once |
| `index=['col1', 'col2']` | Multiple index levels |

```python
# Average Salary by Name
pd.pivot_table(df, values='Salary', index='Name', aggfunc='mean')

# Department vs Name
pd.pivot_table(df, values='Salary', index='Department', columns='Name', aggfunc='sum', fill_value=0)

# Multiple aggregations
pd.pivot_table(df, values='Salary', index='Department', aggfunc=['sum', 'mean', 'max'])

# Multiple indexes
pd.pivot_table(df, values='Salary', index=['Department', 'Name'], aggfunc='sum')
```
