---
layout: "post"
title: "Numpy Cheet Sheet"
date: "2018-08-12 22:17"
tags:
 - Finance
---

NumPy is the library that gives Python its ability to work with data at speed. Originally, launched in 1995 as 'Numeric,' NumPy is the foundation on which
many important Python data science libraries are built, including Pandas, SciPy and scikit-learn.

* TOC
{:toc}

## Key and Imports

In this cheat sheet, we use the following shorthand:

`arr` | A NumPy Array object

You'll also need to import numpy to get started:


```python
import numpy as np
```

## Importing/exporting

`np.loadtxt('file.txt')` | From a text file  
`np.genfromtxt('file.csv',delimiter=',')` | From a CSV file  
`np.savetxt('file.txt',arr,delimiter=' ')` | Writes to a text file  
`np.savetxt('file.csv',arr,delimiter=',')` | Writes to a CSV file

## Creating Arrays
`np.empty((1, 2))` | create an empty `1`x`2` array. The value at each position is uninitialized (random value depending on the memory location).
`np.array([1,2,3])` | One dimensional array. Keyword argument `dtype` converts elements into specified type.
`np.array([(1,2,3),(4,5,6)])` | Two dimensional array  
`np.zeros(3)` | 1D array of length `3` all values `0`  
`np.ones((3,4))` | `3`x`4` array with all values `1`  
`np.eye(5)` | `5`x`5` array of `0` with `1` on diagonal (Identity matrix)  
`np.linspace(0,100,6)` | Array of `6` evenly divided values from `0` to `100`  
`np.arange(0,10,3)` | Array of values from `0` to less than `10` with step `3` (eg `[0,3,6,9]`)  
`np.full((2,3),8)` | `2`x`3` array with all values `8`  
`np.random.rand(4,5)` | `4`x`5` array of random floats between `0`-`1`  
`np.random.rand(6,7)*100` | `6`x`7` array of random floats between `0`-`100`  
`np.random.randint(5,size=(2,3))` | `2`x`3` array with random ints between `0`-`4`

## Inspecting Properties

`arr.size` | Returns number of elements in `arr`  
`arr.shape` | Returns dimensions of `arr` (rows,columns)  
`arr.dtype` | Returns type of elements in `arr`  
`arr.astype(dtype)` | Convert `arr` elements to type `dtype`  
`arr.tolist()` | Convert `arr` to a Python list  
`np.info(np.eye)` | View documentation for `np.eye`
`np.copy(arr)` | Copies `arr` to new memory  
`arr.view(dtype)` | Creates view of `arr` elements with type `dtype`  
`arr.sort()` | Sorts `arr`  
`arr.sort(axis=0)` | Sorts specific axis of `arr`  
`two_d_arr.flatten()` | Flattens 2D array `two_d_arr` to 1D  
`arr.T` | Transposes `arr` (rows become columns and vice versa)  
`arr.reshape(3,4)` | Reshapes `arr` to `3` rows, `4` columns without changing data  
`arr.resize((5,6))` | Changes `arr` shape to `5`x`6` and fills new values with `0`

## Adding/removing Elements

`np.append(arr,values)` | Appends values to end of `arr`  
`np.insert(arr,2,values)` | Inserts values into `arr` before index `2`  
`np.delete(arr,3,axis=0)` | Deletes row on index `3` of `arr`  
`np.delete(arr,4,axis=1)` | Deletes column on index `4` of `arr`

## Combining

[`np.vstack((arr1, arr2))`](http://docs.scipy.org/doc/numpy/reference/generated/numpy.vstack.html) | Vertically stack multiple arrays. Think of it like the second arrays's items being added as new rows to the first array.
[`np.hstack((arr1, arr2))`](http://docs.scipy.org/doc/numpy/reference/generated/numpy.hstack.html) | horizontally stack multiple arrays.
`np.concatenate((arr1,arr2),axis=0)` | Adds `arr2` as rows to the end of `arr1`. It's a general-purpose `vstack`.
`np.concatenate((arr1,arr2),axis=1)` | Adds `arr2` as columns to end of `arr1`. It's a general-purpose `hstack`.
`np.split(arr,3)` | Splits `arr` into `3` sub-arrays  
`np.hsplit(arr,5)` | Splits `arr` horizontally on the `5`th index

## Indexing

`arr[5]` | Returns the element at index `5`  
`arr[2,5]` | Returns the 2D array element on index `[2][5]`  
`arr[1]=4` | Assigns array element on index `1` the value `4`  
`arr[1,3]=10` | Assigns array element on index `[1][3]` the value `10`  
`arr[0:3]` | Returns the elements at indices `0,1,2` (On a 2D array: returns rows `0,1,2`)  
`arr[0:3,4]` | Returns the elements on rows `0,1,2` at column `4`  
`arr[:2]` | Returns the elements at indices `0,1` (On a 2D array: returns rows `0,1`)  
`arr[:,1]` | Returns the elements at index `1` on all rows  
`arr<5` | Returns an array with boolean values  
`(arr1<3) & (arr2>5)` | Returns an array with boolean values  
`~arr` | Inverts a boolean array  
`arr[arr<5]` | Returns array elements smaller than `5`

## Conditional Selecting

NumPy makes it possible to test to see if rows match certain values using
mathematical comparison operations like `<`, `>`, `>=`, `<=`, and `==`. For
example, if we want to see which wines have a quality rating higher than `5`,
we can do this:



    wines[:,11] > 5



    array([False, False, False, ..., True, False, True], dtype=bool)


We get a Boolean array that tells us which of the wines have a quality rating
greater than `5`. We can do something similar with the other operators. For
instance, we can see if any wines have a quality rating equal to `10`:



    wines[:,11] == 10



    array([False, False, False, ..., False, False, False], dtype=bool)


One of the powerful things we can do with a Boolean array and a NumPy array is
select only certain rows or columns in the NumPy array. For example, the below
code will only select rows in `wines` where the quality is over `7`:



    high_quality = wines[:,11] > 7
    wines[high_quality,:][:3,:]



    array([[ 7.90000000e+00, 3.50000000e-01, 4.60000000e-01, 3.60000000e+00, 7.80000000e-02, 1.50000000e+01, 3.70000000e+01, 9.97300000e-01, 3.35000000e+00, 8.60000000e-01, 1.28000000e+01, 8.00000000e+00], [ 1.03000000e+01, 3.20000000e-01, 4.50000000e-01, 6.40000000e+00, 7.30000000e-02, 5.00000000e+00, 1.30000000e+01, 9.97600000e-01, 3.23000000e+00, 8.20000000e-01, 1.26000000e+01, 8.00000000e+00], [ 5.60000000e+00, 8.50000000e-01, 5.00000000e-02, 1.40000000e+00, 4.50000000e-02, 1.20000000e+01, 8.80000000e+01, 9.92400000e-01, 3.56000000e+00, 8.20000000e-01, 1.29000000e+01, 8.00000000e+00]])


We select only the rows where `high_quality` contains a `True` value, and all
of the columns. This subsetting makes it simple to filter arrays for certain
criteria. For example, we can look for wines with a lot of alcohol and high
quality. In order to specify multiple conditions, we have to place each
condition in parentheses, and separate conditions with an ampersand (`&`):



    high_quality_and_alcohol = (wines[:,10] > 10) & (wines[:,11] > 7)
    wines[high_quality_and_alcohol,10:]



    array([[ 12.8, 8. ], [ 12.6, 8. ], [ 12.9, 8. ], [ 13.4, 8. ], [ 11.7, 8. ], [ 11. , 8. ], [ 11. , 8. ], [ 14. , 8. ], [ 12.7, 8. ], [ 12.5, 8. ], [ 11.8, 8. ], [ 13.1, 8. ], [ 11.7, 8. ], [ 14. , 8. ], [ 11.3, 8. ], [ 11.4, 8. ]])


We can combine subsetting and assignment to overwrite certain values in an
array:



    high_quality_and_alcohol = (wines[:,10] > 10) & (wines[:,11] > 7)
    wines[high_quality_and_alcohol,10:] = 20

## Reshaping NumPy Arrays

[`numpy.transpose(arr)`](http://docs.scipy.org/doc/numpy/reference/generated/numpy.transpose.html) | Transpose the array.
[`numpy.ravel(arr)`](http://docs.scipy.org/doc/numpy/reference/generated/numpy.ravel.html) | Turn an array into a one-dimensional representation.
[`numpy.reshape(arr)`](https://docs.scipy.org/doc/numpy-dev/reference/generated/numpy.reshape.html#numpy.reshape) | Reshape an array to a certain shape we specify.

## Scalar Math
If you do any of the basic mathematical operations (`/, *, -, +, ^`) with an array and a value, it will apply the operation to each of the elements in the array.

`np.add(arr,1)` or `arr + 1` | Add `1` to each array element  
`np.subtract(arr,2)` or `arr - 2` | Subtract `2` from each array element  
`np.multiply(arr,3)` or `arr * 3` | Multiply each array element by `3`  
`np.divide(arr,4)` or `arr / 4` | Divide each array element by `4` (returns `np.nan` for division by zero)  
`np.power(arr,5)` or `arr ^ 5` | Raise each array element to the `5`th power

Note that the above operation won't change the wines array -- it will return a new 1-dimensional array where 10 has been added to each element in the quality column of wines.

If we instead did `+=`, we'd modify the array in place.

## Vector Math
All of the common operations (`/, *, -, +, ^`) will work between arrays.

`np.add(arr1,arr2)` | Elementwise add `arr2` to `arr1`  
`np.subtract(arr1,arr2)` | Elementwise subtract `arr2` from `arr1`  
`np.multiply(arr1,arr2)` | Elementwise multiply `arr1` by `arr2`  
`np.divide(arr1,arr2)` | Elementwise divide `arr1` by `arr2`  
`np.power(arr1,arr2)` | Elementwise raise `arr1` raised to the power of `arr2`  
`np.array_equal(arr1,arr2)` | Returns `True` if the arrays have the same elements and shape  
`np.sqrt(arr)` | Square root of each element in the array  
`np.sin(arr)` | Sine of each element in the array  
`np.log(arr)` | Natural log of each element in the array  
`np.abs(arr)` | Absolute value of each element in the array  
`np.ceil(arr)` | Rounds up to the nearest int  
`np.floor(arr)` | Rounds down to the nearest int  
`np.round(arr)` | Rounds to the nearest int

## Statistics

`np.mean(arr,axis=0)` | Returns mean along specific axis  
`arr.sum()` | Returns sum of `arr`  
`arr.min()` | Returns minimum value of `arr`  
`arr.max(axis=0)` | Returns maximum value of specific axis  
`np.var(arr)` | Returns the variance of array  
`np.std(arr,axis=1)` | Returns the standard deviation of specific axis  
`arr.corrcoef()` | Returns correlation coefficient of array

## Acknowledgement
The original post can be found at [dataquest.io](https://www.dataquest.io/blog/numpy-cheat-sheet/).
