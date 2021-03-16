# Pandas
- Indexing and selecting data: <https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html>
- Using iloc, loc, & ix: <https://www.shanelynn.ie/select-pandas-dataframe-rows-and-columns-using-iloc-loc-and-ix/>
- Iterating Dataframes
  - iterrows: <https://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.DataFrame.iterrows.html>
  - itertuples: <https://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.DataFrame.itertuples.html>
  - iteritems: <https://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.DataFrame.iteritems.html>


## Create Dataframe
```python
data = {'col1': [1, 2], 'col2': [3, 4]}
df = pd.DataFrame(data=data)
```

## Load and Save

### Save to CSV
```python
df.to_csv(<path_or_buffer>)
```

Without row names (index):

```python
df.to_csv(<path_or_buffer>, index=False)
```

### Load from CSV
```python
df = pd.read_csv(
        <path_or_buffer>,
        sep=';',
        encoding='us-ascii',
        usecols=<col_list>,
        nrows=<number_of_rows_to_read>,
        low_memory=False,
        )
```

## Delete Data

### Delete Column inline
```python
df.drop('<column_name>', axis=1, inplace=True)
```

## Modify Data

### Sort Data
```python
# low to high values
df.sort_values('<column_name>', inplace=True)

# high to low values
df.sort_values('<column_name>', ascending=False, inplace=True)
```

## Combine Data

### Stack two Dataframes
Never forget to `ignore_index` or you have duplicate index values and
bad things might happen later\!

```python
df = pd.concat([df_01, df_02], ignore_index=True)
```

## Display Settings
Examples for display settings:
```python
pd.set_option('display.max_rows', None)
pd.set_option('display.max_columns', None)
pd.set_option('display.max_colwidth', None)
```

--------------------------------------------------------------------

## OLD CONTENT

Remove all duplicates and keep first (inplace).
```python
df.drop_duplicates(inplace=True, keep='first')
```

Only consider certain columns for identifying duplicates and keep first
(inplace).
```python
df.drop_duplicates(<list_of_cols>, inplace=True, keep='first')
```

Remove rows on Condition
```python
df.drop(df[df[<col_name>] == <condition>].index , inplace=True)
```

### Filter nan Values
`nan == nan` is always `false`. That is why we can not use `==` to
check for `nan`-values. Use `pd.isnull(obj : scalar or array-like)`
instead or `isnull()`. Examples:
```python
df.loc[pd.isnull(df['col'])]
df[df['col'].isnull()]
```

Rename Columns
```python
df.rename(columns={'A': 'x'}, inplace=True)
```
