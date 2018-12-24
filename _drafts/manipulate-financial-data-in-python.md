---
layout: "post"
title: "Manipulate Financial Data in Python"
date: "2018-12-20 22:36"
---

# Reading in a CSV file
You can read in the contents of a CSV (comma-separated values) file into a Pandas dataframe using: `df = [pd.read_csv(<filename>)](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html)`. This creates a dataframe with default index. To specify which column should be treated as the index, specify the optional parameter `index_col=<column_name>`.

Selecting rows from a dataframe:
* First 5 rows: `df.head()`
* Last 5 rows: `df.tail()`. Similarly, last n rows: `df.tail(n)`.
* Slicing: `df.ix[start:end]` in which `end` is exclusive. `df[start:end]` will work too, but it is less Pythonic and robust.

Selecting a column: `df[<column_name>]` <column_name> is a string. To select multiple columns, use `df[[col1, col2, ...]]`. It can be combined with slicing by rows: `df.ix[start:end, [col1, col2, ...]]`.

For more information regarding slicing data, refer to [pandas documentation on "Indexing and Selecting Data"](http://pandas.pydata.org/pandas-docs/stable/indexing.html).

## Building DataFrame
* Create a date series: `pd.date_range(start_date, end_date)`, returns a list of `DateTimeIndex`.
* Create a dataframe with a non-default index: `pd.DataFrame(index=dates)`. If `index` parameter is not specified, the DataFrame will have a default index 0, 1, 2, ...
* Join dataframes on indexes: `[df1.join(df2)](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.join.html)`. By default `df.join()` performs a left join, and different ways of joins can be specified using `how` parameter.
* `df.dropna()` will drop rows with `NaN` values. `df.dropna(subset='COLNAME')` removes rows with NaN values in specified column.
* [Rename a column](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.rename.html): `df.rename(columns=<old_name_to_new_name_dict>)`.

An example:

```python
def get_data(symbols, dates):
    """Read stock data (adjusted close) for given symbols from CSV files."""
    df = pd.DataFrame(index=dates)
    if 'SPY' not in symbols:  # add SPY for reference, if absent
        symbols.insert(0, 'SPY')

    for symbol in symbols:
        df_temp = pd.read_csv(symbol_to_path(symbol), index_col='Date',
                parse_dates=True, usecols=['Date', 'Adj Close'], na_values=['nan'])
        df_temp = df_temp.rename(columns={'Adj Close': symbol})
        df = df.join(df_temp)
        if symbol == 'SPY':  # drop dates SPY did not trade
            df = df.dropna(subset=["SPY"])

    return df
```

Normalization: `df / df.ix[0, :]`.

## Plot DataFrame
To plot, call `df.plot()`. You will need to `import matplotlib.pyplot as plt` and call `plt.show()` to display the output. Common customizations to plots are:
* Title and font size: pass `title=str` and `fontsize=int` keyword parameters to `df.plot()` function.
* Axis label: use `set_xlabel(str)` and `set_ylabel(str)` methods on the return value of `df.plot()`.

An example:
```python
import matplotlib.pyplot as plt

def plot_data(df, title="Stock prices"):
    """Plot stock prices with a custom title and meaningful axis labels."""
    ax = df.plot(title=title, fontsize=12)
    ax.set_xlabel("Date")
    ax.set_ylabel("Price")
    plt.show()
```

# NumPy
NumPy is a wrapper around numeric library in C and Fortran. Pandas DataFrame wraps NumPy `ndarray` (n-dimensional array type) by adding indexes and labels. The underlying ndarray can be retrieved by `df.values`.

## Indexing and Slicing
* indexing: `nd[row, col]`, where `row` and `col` starts at 0.
* slicing: `nd[row_start:row_end, col_start:col_end]`, where `row_end` and `col_end` is exclusive.

Just like Python, the start and/or index can be omitted. For example, the colon itself represents all rows or columns, so `nd[:, 3]` returns the 4th column. The index -1 represents the last index. So for example, `nd[:, -1]` returns the last column.

For more information, refer to [NumPy documentation](https://docs.scipy.org/doc/numpy/reference/arrays.indexing.html).

## Array Creation
[Reference](https://docs.scipy.org/doc/numpy/user/basics.creation.html)

In general, numerical data arranged in an array-like structure in Python can be converted to arrays through the use of the `np.array()` function. The most obvious examples are lists and tuples. For example,

```python
# note mix of tuple and lists, and types
np.array([[1,2.0],[0,0],(1+1j,3.)])
```

NumPy also provides functions to create arrays with initial values:
* `np.empty(shape)`: create an empty array. The value at each position is uninitialized (random value depending on the memory location).
* `np.ones(shape)`: create an array with all 1s.
* Random values: `numpy.random` package provides functions:
    ** `random(shape)` and `rand(rows, cols)` that generates ndarrays filled with random values distributed in `[0.0, 1.0)`. Note that `random()` takes a tuple and `rand()` takes multiple parameters, one for each dimension.
    ** `normal(shape)` uses normal distribution.
    ** `randint(start, end, shape)` generates integers from uniform distribution.

They all take a positional parameter, shape, which is a number for one-dimensional array, or a tuple for multi-dimensional array. The default data type is `float64`, you can specify any [data type](https://docs.scipy.org/doc/numpy/user/basics.types.html) using the `dtype` parameter.
