# Pandas
old content:
<https://github.com/PhilipMay/eniak/blob/master/py/pandas.rst>

## Create Dataframe
``` python
data = {'col1': [1, 2], 'col2': [3, 4]}
df = pd.DataFrame(data=data)
```

## Load and Save

### Save to CSV
``` python
df.to_csv(<path_or_buffer>)
```

Without row names (index):

``` python
df.to_csv(<path_or_buffer>, index=False)
```

### Load from CSV
``` python
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
``` python
df.drop('<column_name>', axis=1, inplace=True)
```

## Modify Data

### Sort Data
``` python
# low to high values
df.sort_values('<column_name>', inplace=True)

# high to low values
df.sort_values('<column_name>', ascending=False, inplace=True)
```

## Combine Data

### Stack two Dataframes
Never forget to `ignore_index` or you have duplicate index values and
bad things might happen later\!

``` python
df = pd.concat([df_01, df_02], ignore_index=True)
```

## Display Settings
Examples for display settings:

``` python
pd.set_option('display.max_rows', None)
pd.set_option('display.max_columns', None)
pd.set_option('display.max_colwidth', None)
```
