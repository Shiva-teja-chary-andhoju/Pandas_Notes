# Pandas Notes

Pandas Essentials: Quick Reference.
-👉 [View in NBViewer](https://nbviewer.org/github/Shiva-teja-chary-andhoju/Pandas_Notes/blob/main/pandas.ipynb)

- [Pandas Official Documentation & User Guide](https://pandas.pydata.org/docs/index.html)


## Table of Contents

1. [What is Pandas?](#1-what-is-pandas)
2. [Create Series / DataFrame](#2-create-series--dataframe)
3. [Basic DataFrame Understanding](#3-basic-dataframe-understanding)
4. [Import & Export Data (I/O)](#4-import--export-data-io)
5. [Data Types & Conversion](#5-data-types--conversion)
6. [Rows & Columns - Selection](#6-rows--columns---selection)
7. [Filter DataFrame](#7-filter-dataframe)
8. [Rows and Columns - Operations (Add, Update, Delete)](#8-rows-and-columns---operations-add-update-delete)
9. [Sorting](#9-sorting)
10. [Renaming & Resetting Index](#10-renaming--resetting-index)
11. [String Operations (.str)](#11-string-operations-str)
12. [Working with Date Values](#12-working-with-date-values)
13. [Handling Missing Values](#13-handling-missing-values)
14. [Duplicates](#14-duplicates)
15. [Apply, Map & Lambda Functions](#15-apply-map--lambda-functions)
16. [Aggregation and Group By](#16-aggregation-and-group-by)
17. [Concatenate and Merge DataFrame (Joins)](#17-concatenate-and-merge-dataframe-joins)
18. [Reshaping - Pivot, Melt, Stack/Unstack, Crosstab](#18-reshaping---pivot-melt-stackunstack-crosstab)
19. [Query Method](#19-query-method)
20. [MultiIndex (Hierarchical Indexing)](#20-multiindex-hierarchical-indexing)
21. [Window Functions (Rolling, Expanding, EWM)](#21-window-functions-rolling-expanding-ewm)
22. [Categorical Data](#22-categorical-data)
23. [Copy vs View & Chained Assignment](#23-copy-vs-view--chained-assignment)
24. [Options & Display Settings](#24-options--display-settings)
25. [Plotting Basics](#25-plotting-basics)
26. [Performance Tips](#26-performance-tips)

---

## 1. What is Pandas?

A Python library for working with structured (tabular) data — reading, cleaning, transforming, analyzing. Built on top of NumPy.

| Structure | Description |
|---|---|
| `Series` | 1-D labeled array (a single column) |
| `DataFrame` | 2-D labeled table (rows + columns) |
| `Index` | The labels for rows (or columns) |

```python
import pandas as pd
import numpy as np
pd.__version__
```

---

## 2. Create Series / DataFrame

| Function | Description |
|---|---|
| `pd.Series(data, index=[...])` | Create a 1-D labeled array |
| `pd.DataFrame(data, columns=[...])` | Create a DataFrame from list/dict/array |
| `pd.DataFrame.from_dict(d)` | Build from a dict (rows or columns orientation) |
| `pd.DataFrame.from_records(rows)` | Build from a list of tuples/records |

```python
s = pd.Series([10, 20, 30], index=['a', 'b', 'c'])

df = pd.DataFrame(data=[11, 22, 33], columns=['col_name'])

testdata = {
    'Name': ['shiva', 'teja', 'chary', 'andhoju'],
    'Age': [25, 26, 27, 28],
    'Salary': [50000, 60000, 70000, 80000]
}
df = pd.DataFrame(data=testdata)

pd.DataFrame.from_dict({'a': [1, 2], 'b': [3, 4]})
pd.DataFrame.from_records([(1, 'x'), (2, 'y')], columns=['id', 'val'])
```

---

## 3. Basic DataFrame Understanding

| Function | Description |
|---|---|
| `.head(n)` / `.tail(n)` | First/last `n` rows (default 5) |
| `.shape` | Tuple of (rows, columns) |
| `.columns` / `.index` | Column labels / row labels |
| `.dtypes` | Data type of each column |
| `.info()` | Summary: dtypes, non-null counts, memory |
| `.describe()` | Descriptive stats for numeric columns |
| `.describe(include='all')` | Stats for all columns (incl. object/categorical) |
| `.nunique()` | Count of unique values per column |
| `.memory_usage(deep=True)` | Memory used by each column |
| `.sample(n)` | Random `n` rows |

```python
df.head(2)
df.tail(2)
df.shape
df.columns
df.dtypes
df.info()
df.describe()
df.describe(include='all')
df.nunique()
df.sample(3)
```

---

## 4. Import & Export Data (I/O)

| Function | Description |
|---|---|
| `pd.read_csv(path)` | Load from CSV |
| `.to_csv(path, index=False)` | Save to CSV |
| `pd.read_excel(path, sheet_name=)` | Load from Excel |
| `.to_excel(path, sheet_name=)` | Save to Excel |
| `pd.read_json(path)` | Load from JSON |
| `.to_json(path)` | Save to JSON |
| `pd.read_parquet(path)` | Load from Parquet (columnar, fast) |
| `.to_parquet(path)` | Save to Parquet |
| `pd.read_sql(query, conn)` | Load from a SQL query/table |
| `.to_sql(table, conn)` | Write to a SQL table |
| `pd.read_clipboard()` | Load whatever is copied to clipboard |

```python
df = pd.read_csv('data.csv')
df.to_csv('out.csv', index=False)

df = pd.read_excel('data.xlsx', sheet_name='Sheet1')
df.to_excel('out.xlsx', index=False)

df = pd.read_json('data.json')
df.to_parquet('data.parquet')

# Only some rows/columns
pd.read_csv('data.csv', usecols=['Name', 'Age'], nrows=100)
```

---

## 5. Data Types & Conversion

| Function | Description |
|---|---|
| `.astype('type')` | Convert column(s) to a given dtype |
| `pd.to_numeric(col, errors='coerce')` | Convert to numeric, invalid → NaN |
| `pd.to_datetime(col)` | Convert to datetime |
| `.convert_dtypes()` | Auto-infer best nullable dtypes |
| `.select_dtypes(include=/exclude=)` | Select columns by dtype |

```python
df['Age'] = df['Age'].astype('int32')
df['Salary'] = pd.to_numeric(df['Salary'], errors='coerce')

df.select_dtypes(include='number')
df.select_dtypes(exclude='object')
```

---

## 6. Rows & Columns - Selection

| Function | Description |
|---|---|
| `df['col']` | Select a single column (Series) |
| `df[['col']]` | Select a single column (DataFrame) |
| `df[['col1', 'col2']]` | Select multiple columns |
| `df.loc[condition]` | Label-based selection (supports conditions) |
| `df.iloc[start:stop]` | Position-based selection (stop exclusive) |
| `df.loc[start:stop]` | Label-based slicing (stop inclusive) |
| `df.at[row, col]` | Fast scalar access by label |
| `df.iat[row, col]` | Fast scalar access by position |

```python
df['Name']
df[['Name']]
df[['Name', 'Age']]

df.loc[df.Name == 'shiva']
df.loc[(df.Name == 'shiva') & (df.Salary >= 50000)]

df.iloc[0]
df.iloc[0:2]     # start inclusive, stop exclusive
df.loc[0:2]      # start and stop inclusive

df.at[0, 'Name']
df.iat[0, 1]
```

---

## 7. Filter DataFrame

| Function | Description |
|---|---|
| `df[condition]` | Filter rows where condition is True |
| `.isin([...])` | Keep rows where value is in a list |
| `.between(a, b)` | Keep rows where value falls in a range |
| `.where(condition, other=value)` | Replace non-matching values (default NaN) |
| `.mask(condition, other=value)` | Opposite of `.where` — replace matching values |

```python
df[df['Age'] > 26]
df[(df['Age'] > 26) & (df['Salary'] >= 50000)]

df[df['Name'].isin(['shiva', 'teja'])]
df[df['Age'].between(25, 27)]

df.where(df['Age'] > 26)
df.where(df['Age'] > 26, other='Not Eligible')
df.mask(df['Age'] > 26, other='Too Old')
```

---

## 8. Rows and Columns - Operations (Add, Update, Delete)

| Function | Description |
|---|---|
| `df['col'] = [...]` | Add/create a new column |
| `df.insert(loc, 'col', values)` | Insert a column at a specific position |
| `df.loc[len(df)] = [...]` | Add a new row at the end |
| `df.loc[row, 'col'] = value` | Update a value by index |
| `df.loc[condition, 'col'] = value` | Update a value by column condition |
| `df.drop(index)` | Drop row(s) by index (`axis=0`, default) |
| `df.drop('col', axis=1)` | Drop column(s) (`axis=1`) |
| `df.pop('col')` | Remove and return a column |

```python
df['Team'] = ['CEO', 'HR', 'CTO', 'DA']
df['Bonus'] = df['Salary'] * 0.2
df.insert(1, 'Dept', 'General')

df.loc[len(df)] = ['balram', 29, 50000, 'IT', 10000]

df.loc[0, 'Salary'] = 100000
df.loc[df.Name == 'teja', 'Salary'] = 65000

df.drop(df[df.Name == 'balram'].index)
df.drop(1, axis=0)
df.drop('Bonus', axis=1)
# df.drop(['Bonus', 'Team'], axis=1, inplace=True)
```

---

## 9. Sorting

| Function | Description |
|---|---|
| `.sort_values('col')` | Sort by column, ascending (default) |
| `.sort_values('col', ascending=False)` | Sort descending |
| `.sort_values(['c1', 'c2'])` | Sort by multiple columns |
| `.sort_index()` | Sort by row index |
| `.nlargest(n, 'col')` / `.nsmallest(n, 'col')` | Top/bottom `n` rows by value |
| `.rank()` | Rank values (like SQL `RANK()`) |

```python
df.sort_values('Salary')
df.sort_values('Salary', ascending=False)
df.sort_values(['Team', 'Salary'], ascending=[True, False])
df.sort_index()

df.nlargest(3, 'Salary')
df.nsmallest(3, 'Salary')
df['Salary'].rank(ascending=False)
```

---

## 10. Renaming & Resetting Index

| Function | Description |
|---|---|
| `.rename(columns={...})` | Rename columns (need `inplace=True` or reassign) |
| `.set_index('col')` | Make a column the index |
| `.reset_index()` | Turn the index back into a column |
| `.reset_index(drop=True)` | Reset index without keeping old one as a column |

```python
df1 = df.rename(columns={'Salary': 'monthly_salary'})
df.rename(columns={'Salary': 'monthly_salary'}, inplace=True)

df.set_index('Name')
df.reset_index()
df.reset_index(drop=True)
```

---

## 11. String Operations (.str)

Use `.str` accessor on a text column — same idea as Python string methods, vectorized.

| Function | Description |
|---|---|
| `.str.lower()` / `.str.upper()` | Change case |
| `.str.strip()` | Remove leading/trailing whitespace |
| `.str.replace(a, b)` | Replace substring |
| `.str.contains('x')` | Boolean mask — does string contain `x` |
| `.str.startswith()` / `.str.endswith()` | Boolean mask |
| `.str.split(delim)` | Split into a list |
| `.str.len()` | Length of each string |
| `.str.cat(sep=)` | Concatenate strings |
| `.str.extract(regex)` | Extract regex group into new column(s) |

```python
df['Name'] = df['Name'].str.strip().str.title()
df['Email_domain'] = df['Email'].str.split('@').str[1]

df[df['Name'].str.contains('sh', case=False)]
df['Name'].str.len()
df['Name'].str.extract(r'(\w+)\s(\w+)')  # first, last name into 2 columns
```

---

## 12. Working with Date Values

| Function | Description |
|---|---|
| `pd.to_datetime(col)` | Convert column to datetime |
| `pd.to_datetime(col, format='...')` | Convert with an explicit date format |
| `.dt.year / .dt.month / .dt.day` | Extract date parts |
| `.dt.day_name()` | Name of the weekday |
| `pd.Timedelta(days=n)` | Add/subtract a duration from dates |
| `pd.date_range(start, end, freq=)` | Generate a sequence of dates |
| `.dt.to_period('M')` | Convert to a period (e.g. month) |

```python
df['DOJ'] = pd.to_datetime(df['DOJ'])
df['DOJ2'] = pd.to_datetime(df['DOJ2'], format='%d-%m-%Y')

df['DOJ'].dt.year
df['DOJ'].dt.month
df['DOJ'].dt.day
df['DOJ'].dt.day_name()

df['Month'] = df['DOJ'].dt.month
df['DOJ'] + pd.Timedelta(days=10)

pd.date_range('2024-01-01', '2024-01-10', freq='D')
```

---

## 13. Handling Missing Values

| Function | Description |
|---|---|
| `.isnull()` / `.isna()` | Detect missing values (boolean) |
| `.isnull().sum()` | Count of nulls per column |
| `.notnull()` | Opposite of `.isnull()` |
| `.fillna(value)` | Fill nulls with a given value |
| `.fillna(method='ffill')` | Forward-fill (carry last valid value) |
| `.dropna()` | Drop rows with any null |
| `.dropna(subset=['col'])` | Drop rows where a specific column is null |
| `.interpolate()` | Fill nulls using interpolation |

```python
df.isnull()
df.isnull().sum()
df.fillna(0)
df['Age'] = df['Age'].fillna(df['Age'].mean())
df.fillna(method='ffill')

df.dropna()
df.dropna(subset=['Salary'])
df['Salary'].interpolate()
```

---

## 14. Duplicates

| Function | Description |
|---|---|
| `.duplicated()` | Boolean mask — is the row a duplicate |
| `.duplicated(subset=['col'])` | Check duplicates on specific columns |
| `.drop_duplicates()` | Remove duplicate rows |
| `.drop_duplicates(keep='last')` | Keep last occurrence instead of first |

```python
df.duplicated()
df.duplicated(subset=['Name'])
df.drop_duplicates()
df.drop_duplicates(subset=['Name'], keep='last')
```

---

## 15. Apply, Map & Lambda Functions

| Function | Description |
|---|---|
| `.apply(func)` | Apply a function along rows/columns of a DataFrame |
| `Series.apply(func)` | Apply a function element-wise on a Series |
| `Series.map(func_or_dict)` | Element-wise mapping (Series only) |
| `.applymap(func)` (deprecated, use `.map`) | Element-wise on entire DataFrame |
| `lambda` | Anonymous inline function, commonly used with apply/map |

```python
df['Salary_k'] = df['Salary'].apply(lambda x: x / 1000)
df['Grade'] = df['Age'].map({25: 'A', 26: 'B', 27: 'C'})

df['Bonus_Category'] = df.apply(
    lambda row: 'High' if row['Salary'] > 60000 else 'Low', axis=1
)

df[['Age', 'Salary']].map(lambda x: x * 2)  # element-wise on whole df
```

---

## 16. Aggregation and Group By

| Function | Description |
|---|---|
| `.value_counts()` | Frequency of unique values |
| `.groupby('col')['col2'].sum()` | Aggregate one column by group |
| `.groupby('col').agg({...})` | Different aggregations per column |
| `.groupby('col').agg(['sum', 'mean'])` | Multiple aggregations at once |
| `.groupby(['c1', 'c2'])` | Group by multiple columns |
| `.groupby('col').transform(func)` | Aggregate but keep original row shape |
| `.groupby('col').filter(func)` | Keep only groups matching a condition |

```python
df['Team'].value_counts()

df.groupby('Month')['Salary'].sum()
df.groupby('Month').agg({'Salary': 'sum', 'Bonus': 'sum', 'Name': 'count'})
df.groupby('Month')['Salary'].agg(['sum', 'mean', 'max'])

df.groupby(['Team', 'Month'])['Salary'].sum()

df['Team_avg'] = df.groupby('Team')['Salary'].transform('mean')
df.groupby('Team').filter(lambda g: g['Salary'].mean() > 55000)
```

---

## 17. Concatenate and Merge DataFrame (Joins)

| Function | Description |
|---|---|
| `pd.concat([df1, df2], axis=0)` | Vertical concat (row-wise, stacked) |
| `pd.concat([df1, df2], axis=1)` | Horizontal concat (column-wise, side by side) |
| `pd.merge(df1, df2, how=, on=)` | SQL-style join (`inner`, `left`, `right`, `outer`) |
| `pd.merge(..., left_on=, right_on=)` | Merge when join columns have different names |
| `.join(df2)` | Join on index (shortcut for merge) |

```python
pd.concat([df1, df2], axis=0)
pd.concat([df1, df2], axis=1)

pd.merge(df1, df2, how='inner', on='ID')
pd.merge(df1, df2, how='left', on='ID')
pd.merge(df1, df2, how='right', on='ID')
pd.merge(df1, df2, how='outer', on='ID')
pd.merge(df1, df2, how='inner', left_on='ID', right_on='EmpID')

df1.join(df2, how='left')
```

---

## 18. Reshaping - Pivot, Melt, Stack/Unstack, Crosstab

| Function | Description |
|---|---|
| `pd.pivot_table(df, values=, index=, aggfunc=)` | Aggregate values grouped by index |
| `pd.pivot_table(..., columns=, fill_value=)` | Cross-tab style pivot with row + column grouping |
| `df.pivot(index=, columns=, values=)` | Reshape without aggregation (no duplicates allowed) |
| `pd.melt(df, id_vars=, value_vars=)` | Wide → long format |
| `.stack()` | Columns → innermost row index level |
| `.unstack()` | Row index level → columns |
| `pd.crosstab(df.a, df.b)` | Frequency cross-tabulation of two columns |

```python
# Average Salary by Name
pd.pivot_table(df, values='Salary', index='Name', aggfunc='mean')

# Department vs Name
pd.pivot_table(df, values='Salary', index='Department', columns='Name', aggfunc='sum', fill_value=0)

# Multiple aggregations / indexes
pd.pivot_table(df, values='Salary', index='Department', aggfunc=['sum', 'mean', 'max'])
pd.pivot_table(df, values='Salary', index=['Department', 'Name'], aggfunc='sum')

df.pivot(index='Name', columns='Month', values='Salary')

pd.melt(df, id_vars=['Name'], value_vars=['Jan', 'Feb'], var_name='Month', value_name='Salary')

df.set_index(['Name', 'Month'])['Salary'].unstack()

pd.crosstab(df['Team'], df['Month'])
```

---

## 19. Query Method

| Function | Description |
|---|---|
| `.query("expression")` | Filter rows using a SQL-like expression |
| `.query("col in @list")` | Reference a Python variable with `@` |

```python
df.query("Age >= 26 and Salary >= 60000")

min_salary = 60000
df.query("Salary >= @min_salary")
```

---

## 20. MultiIndex (Hierarchical Indexing)

| Function | Description |
|---|---|
| `df.set_index(['c1', 'c2'])` | Create a MultiIndex from two+ columns |
| `df.loc[(v1, v2)]` | Select using a tuple of index values |
| `.xs('value', level='c1')` | Cross-section — select at one level |
| `.swaplevel()` | Swap the order of index levels |
| `.droplevel('c1')` | Remove a level from the index |

```python
df2 = df.set_index(['Team', 'Name'])
df2.loc[('CEO', 'shiva')]
df2.xs('CEO', level='Team')
df2.swaplevel()
df2.droplevel('Team')
```

---

## 21. Window Functions (Rolling, Expanding, EWM)

| Function | Description |
|---|---|
| `.rolling(n).mean()` | Moving average over a fixed window of `n` rows |
| `.rolling(n).sum()` | Rolling sum |
| `.expanding().sum()` | Cumulative sum from the start up to each row |
| `.ewm(span=n).mean()` | Exponentially weighted moving average |
| `.cumsum()` / `.cummax()` / `.cummin()` | Running cumulative sum/max/min |

```python
df['Salary'].rolling(3).mean()
df['Salary'].rolling(3).sum()
df['Salary'].expanding().sum()
df['Salary'].ewm(span=3).mean()

df['Salary'].cumsum()
df['Salary'].cummax()
```

---

## 22. Categorical Data

| Function | Description |
|---|---|
| `.astype('category')` | Convert a column to categorical dtype (saves memory) |
| `pd.Categorical(values, categories=, ordered=True)` | Create an ordered categorical |
| `.cat.codes` | Underlying integer codes for categories |
| `.cat.categories` | List of category labels |
| `.cat.reorder_categories([...])` | Change category order |

```python
df['Team'] = df['Team'].astype('category')

grade = pd.Categorical(['Low', 'High', 'Medium'],
                        categories=['Low', 'Medium', 'High'], ordered=True)

df['Team'].cat.codes
df['Team'].cat.categories
```

---

## 23. Copy vs View & Chained Assignment

| Concept | Description |
|---|---|
| View | A slice that shares memory with the original — modifying it may affect the original |
| Copy | An independent object — `.copy()` guarantees no link back to original |
| Chained assignment | `df[df.a > 1]['b'] = x` — unreliable, may silently fail (`SettingWithCopyWarning`) |

```python
df_copy = df.copy()          # safe, independent copy

# Avoid chained assignment:
# df[df['Age'] > 26]['Salary'] = 0        # BAD - may not update original
df.loc[df['Age'] > 26, 'Salary'] = 0       # GOOD - single .loc call
```

---

## 24. Options & Display Settings

| Function | Description |
|---|---|
| `pd.set_option('display.max_rows', n)` | Change max rows shown |
| `pd.set_option('display.max_columns', n)` | Change max columns shown |
| `pd.reset_option('all')` | Reset all display options to default |
| `pd.get_option('display.max_rows')` | Check current setting |

```python
pd.set_option('display.max_rows', 100)
pd.set_option('display.max_columns', None)
pd.reset_option('all')
```

---

## 25. Plotting Basics

Pandas plotting is a thin wrapper around Matplotlib — needs `matplotlib` installed.

| Function | Description |
|---|---|
| `.plot()` | Default line plot |
| `.plot(kind='bar')` | Bar chart |
| `.plot(kind='hist')` | Histogram |
| `.plot(kind='box')` | Box plot |
| `.plot(kind='scatter', x=, y=)` | Scatter plot |

```python
df['Salary'].plot(kind='hist')
df.groupby('Team')['Salary'].sum().plot(kind='bar')
df.plot(kind='scatter', x='Age', y='Salary')
```

---

## 26. Performance Tips

| Tip | Description |
|---|---|
| Prefer vectorized ops | `df['a'] + df['b']` instead of looping row by row |
| Avoid `.apply(axis=1)` when possible | Row-wise apply is slow — vectorize or use `.map`/`.str` instead |
| Use `.query()` / `.eval()` | Faster and more memory-efficient for large filters/expressions |
| Use appropriate dtypes | `category` for low-cardinality text, smaller int/float types |
| Read only needed columns | `pd.read_csv(path, usecols=[...])` |
| Use `.itertuples()` over `.iterrows()` | Much faster when a loop is unavoidable |
| Chain with method chaining | Combine steps to avoid intermediate copies |

```python
df.eval('Bonus2 = Salary * 0.1', inplace=True)

for row in df.itertuples():
    pass   # faster than df.iterrows()

(df
 .query("Age >= 26")
 .assign(Salary_k=lambda d: d['Salary'] / 1000)
 .sort_values('Salary_k', ascending=False)
)
```

