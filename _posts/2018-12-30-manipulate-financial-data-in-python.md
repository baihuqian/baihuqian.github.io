---
layout: post
title: Manipulate Financial Data in Python
date: '2018-12-30 20:22'
tags:
 - Finance
---

This is the notes for mini course 1 of [Machine Learning for Trading](https://classroom.udacity.com/courses/ud501), offered by Udacity.

* TOC
{:toc}

# Reading in a CSV file
You can read in the contents of a CSV (comma-separated values) file into a Pandas dataframe using: [`df = pd.read_csv(<filename>)`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html). This creates a dataframe with default index. To specify which column should be treated as the index, specify the optional parameter `index_col=<column_name>`.

Selecting rows from a dataframe:
* First 5 rows: `df.head()`
* Last 5 rows: `df.tail()`. Similarly, last n rows: `df.tail(n)`.
* Slicing: `df[start:end]` in which `end` is exclusive.

Selecting a column: `df[<column_name>]` `<column_name>` is a string. To select multiple columns, use `df[[col1, col2, ...]]`. It can be combined with slicing by rows: `df.ix[start:end, [col1, col2, ...]]`.

For more information regarding slicing data, refer to [pandas documentation on "Indexing and Selecting Data"](http://pandas.pydata.org/pandas-docs/stable/indexing.html).

## Building DataFrame
* Create a date series: `pd.date_range(start_date, end_date)`, returns a list of `DateTimeIndex`.
* Create a dataframe with a non-default index: `pd.DataFrame(index=dates)`. If `index` parameter is not specified, the DataFrame will have a default index 0, 1, 2, ...
* Join dataframes on indexes: `[df1.join(df2)](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.join.html)`. By default `df.join()` performs a left join, and different ways of joins can be specified using `how` parameter.
* `df.dropna()` will drop rows with `NaN` values. `df.dropna(subset='COLNAME')` removes rows with NaN values in specified column.
* [Rename a column](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.rename.html): `df.rename(columns=<old_name_to_new_name_dict>)`.
* Copy from existing DataFrame: `df.copy()`

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

Plot a vertical line: `plt.axvline(value, color='r', linestyle='dashed', linewidth=2)`

# NumPy
NumPy is a wrapper around numeric library in C and Fortran. Pandas DataFrame wraps NumPy `ndarray` (n-dimensional array type) by adding indexes and labels. The underlying ndarray can be retrieved by `df.values`.

## Indexing and Slicing
* indexing: `nd[row, col]`, where `row` and `col` starts at 0.
* slicing: `nd[row_start:row_end, col_start:col_end]`. Slice `m:n:t` speficies a range `[m, n)` in step `t`.

Just like Python, the start and/or index can be omitted. For example, the colon itself represents all rows or columns, so `nd[:, 3]` returns the 4th column. The index -1 represents the last index. So for example, `nd[:, -1]` returns the last column.

For more information, refer to [NumPy documentation](https://docs.scipy.org/doc/numpy/reference/arrays.indexing.html).

### Integer Array Indexing

You can index an array with another integer array of indices.

```python
a = np.random.rand(5)
indices = np.array([1, 2, 1])
a[indices] # returns the array of elements at each index
```

### Boolean Value Indexing
You can index an array with a boolean array, and only those positions are selected where there is a `True`. You can create a corresponding boolean array from boolean expressions:

```python
a = np.random.rand(5)
mean = a.mean()
a[a < mean] # elements less than the mean
a[a < mean] = mean # replace elements smaller than mean with mean
```

## Assignment
* `nd[row, col] = 2` assigns `2` to a single position in the array.
* `nd[row, :] = 2` assigns `2` to the entire row.
* `nd[row, :2] = [0, 1]` performs element-wise assignment, but make sure the size matches.

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

## Array Attributes
* `ndarray.shape`: a tuple of the shape of the ndarray. `shape[0]` is number of rows, and `shape[1]` is number of columns.
* `ndarray.ndim`: number of dimensions (i.e. `len(ndarray.shape)`).
* `ndarray.size`: total number of elements in the array.
* `ndarray.dtype`: the data type of the array.

## Array Operations
Mathmatical functions: [reference](https://docs.scipy.org/doc/numpy/reference/routines.math.html)

* `ndarray.sum()` sum of elements, along rows, columns, or all. To sum over rows (i.e. of each column), pass the keyword parameter `axis=0`. To sum over columns (i.e. of each row), pass the keyword parameter `axis=1`.
* `ndarray.min()` minimum along axis or all, same as `sum()`.
* `ndarray.max()` maximum along axis or all, same as `sum()`.
* `ndarray.mean()` mean along axis or all, same as `sum()`.

Arithmetic operations are applied element-wise, and return a new array. For example, `a + 2` returns a new array with every element in `a` incremented by 2.
* `numpy.add`: Element-wise addition, same as `+` operator
* `numpy.subtract`: Element-wise subtraction, same as `-`
* `numpy.multiply`: Element-wise multiplication, same as `*`
* `numpy.divide`: Element-wise division, same as `/`
* `numpy.dot`: Dot product (1D arrays), matrix multiplication (2D)

# Statistical Analysis
DataFrame provides many global statistics, such as `mean()`, `median()`, `std()`, `sum()`, `prod()` (the product), etc. They all support specifying axis, by default `0` along the index (i.e. stats of each column), `1` along the column (i.e. stats of each index). These statistics can also be computed on all rolling (time window) basis.

DataFrame and Series provides a `rolling()` method that takes parameters of the window to create a calling object, which stats methods such as `mean()`, `max()` are defined.

Bollinger Bands is the rolling range $$2\sigma$$ above and below the rolling mean. For example, a 20-day Bollinger Band can be computed as follows:

```python
# df is the Series of prices
window = 20
rm = df.rolling(window=window).mean()
rstd = df.rolling(window=window).std()
upper_band = rm + rstd * 2
lower_band = rm - rstd * 2

# Plot raw SPY values, rolling mean and Bollinger Bands
ax = df.plot(title="Bollinger Bands", label='SPY')
rm.plot(label='Rolling mean', ax=ax)
upper_band.plot(label='upper band', ax=ax) # Plot onto the same Axes object
lower_band.plot(label='lower band', ax=ax)

# Add axis labels and legend
ax.set_xlabel("Date")
ax.set_ylabel("Price")
ax.legend(loc='upper left')
plt.show()
```

Daily return is the percentage increased between today's price and previous day's price. It can be computed as follows:
```python
# df is the DataFrame of prices
daily_returns = df.copy() # Make a copy of the data to preserve the size

# When making element-wise operations, pandas will match rows by index, but we don't want that.
# df.values returns the underlying ndarray, erasing the index information.
daily_returns[1:] = df[1:] / df[:-1].values - 1
# Or
daily_returns = (df / df.shift(1)) - 1

# There is no daily return at index 0, so set it to 0.
daily_returns.iloc[0] = 0
```

Cumulative return is the percentage increased between today's price and the price at the beginning of the period.

```python
df / df.iloc[0] - 1 #iloc gets the 1st row by index
```

# Incomplete Data
Financial data are not pristine. Stocks are not listed before IPO, no longer listed after merger, or sometimes they stopped being traded. In a time series, NaN can take place anywhere. To address NaN and not leaking information about the future, we should:
1. Fill forward. For any given NaN, if there is a known value before it, fill it with the closest known value before it;
2. Fill backward. This is to fill NaN with no known previous values, at the beginning of the time series.

`df.fillna()` method can fill NaN. `method` keyword parameter allows forward filling (`ffill` or `pad`) or backward filling (`bfill` or `backfill`), and `inplace` keyword, when set to `True`, can modify the DataFrame in place without creating a new one.

# Histograms and Scatter Plots
Daily returns are not interesting when examined as a time series. However, histograms and scatter plots are two useful way to further examine it.

## Histograms
we can use `df.hist()` to plot a histogram. `bins` parameter specifies how many bins to use for the histogram, and the default is 10. If the DataFrame contains multiple columns, `df.hist()` will plot one subgraph for each column. To plot multiple histograms onto the same graph, call `df[symbol].hist()` on each symbol. This is useful to compare the histograms of multiple stocks.

Once we plot the histogram of daily returns of a stock, there are several statistics of interest:
1. mean: `df.mean()`;
2. standard deviation: `df.std()`;
3. [kurtosis](https://en.wikipedia.org/wiki/Kurtosis) (measurement of fat tail): a positive value means fatter tail than normal distribution. `df.kurtosis()`

## Scatter Plots
Scatter plots are useful to identify correlations between daily returns of different stocks, or between stocks and the market. If a line is fitted to the data points on a scatter plot, the slope is $$\beta$$, and the vertical distance from the origin is the $$\alpha$$.

By passing in `kind='scatter'` parameter to `df.plot()`, we can get a scatter plot. Note that if the DataFrame contains more than 2 columns, you can use keyword parameter `x` and `y` to specify two columns.

To fit a line between two columns, we can use `beta, alpha = numpy.ployfit(x, y, degree)` function with `degree=1`. To plot a line,

```python
beta, alpha = numpy.ployfit(daily_returns['SPY'], daily_returns['XOM'], 1)
# plot.plot() takes two series.
# the 3rd parameter indicates we want a line plot.
plt.plot(daily_returns['SPY'], beta * daily_returns['SPY'] + alpha, '-', color='r')
plt.show()
```

Note that $$\beta$$ is not the same as correlation. Higher correlation is indicated by the tighter correlations between the fitted line and the points. You can also compute the correlation among each column of the DataFrame using `df.corr()` method. It computes the Pearson correlation.

# Portfolio Statistics
You can compute the value of your portfolio as follows:

```python
start_val = 1000000 # start at $1 MM
symbollist = ['SPY', 'XOM', 'GOOG', 'GLD']
start_date = '2009-01-01'
end_date = '2012-12-31'
allocs = [0.4, 0.4, 0.1, 0.1]

# Read stock data from portfolio
idx = pd.date_range(start_date, end_date)
df_data = get_data(symbollist, idx)

# Normalize stock price
normed = df_data / df_data.iloc[0]

# Normalized price for each allocation
alloced = normed * allocs

# Position for each allocation
pos_vals = alloced * start_val

# Sum across columns (axis=1) to get the portfolio value
port_val = pos_vals.sum(axis=1)
```
We can calculate the daily returns from the portfolio. Note that because the first value of daily returns is always 0, we would like to exclude that. You can do so by `daily_rets = daily_rets[1:]`. Some useful statistics are:
* Cumulative return: `port_val[-1] / port_val[0] - 1`;
* average daily return: `daily_rets.mean()`;
* standard deviation of daily return: `daily_rets.std()`;
* Sharpe ratio.

Sharpe ratio quantifies risk adjusted return. Sharpe ratio is $$S = \frac{E[R_p-R_f]}{std[R_p-R_f]}$$, where $$R_p$$ is the return of the portfolio, $$R_f$$ is the risk-free rate. This is the *ex ante* form, meaning the forward-looking form. When computing Sharpe Ratio using historical data, the expectation is just the mean.

Risk-free rate can be
* LIBOR;
* 3-month T-bill;

which are usually in annual rate. To convert from annual rate to daily rate, the traditional short cut is $$\sqrt[252]{1+R_f} - 1$$. Mostly we can treat the daily return of the risk-free rate as a constant, thus Sharpe Ratio becomes $$S = \frac{E[R_p]-R_f}{std[R_p]}$$.

Note, Sharpe Ratio can vary widely depending on the sample frequency. Traditionally, Sharpe ratio is an annual measure, so it must be normalized from other sampling frequency. The normalization factor is the square root of the number of samples per year. So the normalization factor is $$\sqrt{252}$$ for daily data, $$\sqrt{52}$$ for weekly data, and $$\sqrt{12}$$ for monthly data. Note that the normalization factor has nothing to do with number of samples.

```python
risk_free_rate = 0.0
norm = sqrt(252) # Normalization factor
mean = daily_returns.mean()
std = daily_returns.std()
sharpe_ratio = norm * (mean - risk_free_rate) / std
```

# Optimizers
We can use [SciPy to minimize an objective function](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.minimize.html).

```python
import scipy.optimize as spo
# The objective function
def f(X):
    return (X - 1.5) ** 2 + 0.5

Xguess = 2.0 # Provide an initial guess
min_result = spo.minimize(f, Xguess, method='SLSQP', options={'disp': True})
print("X = {}, Y = {}".format(min_result.x, min_result.fun))
```

From a given data set, we can build a parameterized model that fits the data. We need to define an error function for which the minimizer can minimize. For example, to fit a line (i.e. first-order polynomial) to a data set, we can:

```python
# Define the error function
def error(line, data):
    err = np.sum((data[:, 1] - (line[0] * data[:, 0] + line[1])) ** 2)
    return err

def fit_line(data, error_func):
    # initial guess
    l = np.float32([0, np.mean(data[:, 1])]) # a horizontal line

    # minimize
    result = spo.minimize(error_func, l, args=(data,), method = "SLSQP", options={'disp': True})
    return result.x
```

Portfolio optimization aims to find the best allocation from a given set of assets and time period, using some criteria such as
* Cumulative return;
* Minimum volatility;
* Sharpe Ratio.

Cumulative return and volatility are easy to optimize, just allocate all investments into the asset with the best cumulative return or minimum volatility. Thus, we will optimize by Sharpe Ratio.

So the optimization problem becomes: given an allocation $$X$$, find the optimal value that minimizes $$-S$$ (negation of Sharpe Ratio). To improve the performance of the optimizer, we can provide ranges and constraints. In this case, we have:
* Range: $$0\leq x_i\leq 1$$;
* Constraint: properties of $$X$$ that must be `True`. In this case, $$\sum_i x_i = 1.0$$.

For `spo.minimize()`, range can be expressed by a sequence of `(min, max)` pairs for each element in `x`. `None` is used to specify no bound. Constraints are more complicated, see the documentation.

```python
# Initial guess
allocs = np.asarray([1.0 / len(syms) for _ in syms])

# Optimization function: negation of Sharpe Ratio
def optimization_func(alloc):
    daily_returns = compute_daily_returns(portfolio_value(alloc, normed_prices))
    return -1 * sharpe_ratio(daily_returns)

# Each allocation must be 0.0 <= x <= 1.0
bounds = [(0.0, 1.0) for _ in syms]

# Sum of the allocation must equal to 1.0
# The lambda function must return 0 when True
constraint = {'type': 'eq', 'fun': lambda alloc: 1.0 - alloc.sum()}
result = spo.minimize(optimization_func, allocs, method='SLSQP', options={'disp': True}, bounds=bounds, constraints=constraint)
allocs = result.x

```
