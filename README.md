# Pandas Notes

A collection of Pandas concepts, code snippets, and outputs.
-👉 [Open in NBViewer](https://nbviewer.org/github/Shoju/Pandas_Notes/blob/main/pandas.ipynb)
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


