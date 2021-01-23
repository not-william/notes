# NumPy

## Basic NumPy

### The numpy ndarray

The ndarray is the central data structure in NumPy. It has a _shape_ and _dtype_. 

**Creating ndarrays**

```python
arr1 = np.array([4, 2, 7, 5]) # we can pass a sequence to the array method

arr2 = np.array([1, 5, 3.6, 5], [3, 2.8, 3.1, 0]) # nested sequences will be converted into multi-dimensional arrays

arr1.dtype
>> dtype('int64') # numpy assings a default data type if none was specified

arr2.dtype
>> dtype('float64')
```

Here's a list of array-creating methods:

| Method          | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| `array`         | Convert input as a sequence or nested sequences into an array, by infering a dtype or explicitly specifying one. Copies data by default. |
| `asarray`       | Convert input to ndarray, but doesn't copy the input if already an ndarray |
| `zeros`         | Creates array of zeros of the given shape                    |
| `zeros_like`    | Creates array of zeros in same shape as ndarray given        |
| `ones`          | As above but with ones                                       |
| `ones_like`     | As above but with ones                                       |
| `empty`         | Assigns memory to an array of the given shape, but doesn't populate the entries |
| `empty_like`    | As before                                                    |
| `full`          | Creates an array of given shape and fills with the value specified |
| `full_like`     | Like before                                                  |
| `arange`        | Similar to the Python `range` function but creates an ndarray |
| `eye, identity` | Creates a square N x N identity matrix                       |

**Data Types**

ndarrays have a dtype, meaning that every element of the array has to be of that type. 

We can specify this type when creating arrays, and cast arrays to another type:

```python
arr1 = np.array([1, 5, 3.6], dtype=np.float64)
arr2 = arr.astype(np.int64) # if I cast a float to an int, the decimals will be truncated
arr2
>> array([1, 5, 3], dtype=int64)
```

**Arithmetic**

Addition and subtraction `+`, `-`, multiplication `*`, division `/`, exponentiation `**`, all work element-wise, and with scalars propogate to every element in an array.

Comparisons between arrays of the same size yield boolean arrays.

Boolean arrays can be passed for indexing - the length must be the same as the array axis it's indexing. Be careful, since this is not checked and will not result in an error if the lengths are different.

Selecting data from an array by Boolean indexing *always* creates a copy of the data, even if the returned array is unchanged.

That being said, *assignment* does work. For example:

```python
arr = np.random.randn(7, 4)
arr
>>> array([[ 0.86934327, -0.26117668, -0.01767656,  1.46971868],
           [ 0.31634369,  0.34974885, -2.11869426,  0.94427843],])
arr[arr<0] = 0
arr
>>> array([[0.86934327, 0.        , 0.        , 1.46971868],
           [0.31634369, 0.34974885, 0.        , 0.94427843]])
```



Operations on Boolean arrays:

| Operation | Description           |
| --------- | --------------------- |
| ~         | Element-wise negation |
| &         | Element-wise AND      |
| \|        | Element-wise OR       |

**Indexing and Slicing**

You index a 2D array by rows then columns, like a matrix. For higher dimensional arrays, the dimentions go in order of highest to lowest.

All regular indexing and slices are _views_ meaning that they reference the original object and data **is not copied**.

Actually, when you index you 'flatten' that dimension, whereas slicing [i:i+1] doesn't flatten it. See the book...

**Fancy Indexing**

Fancy indexing returns a one-dimensional array of elements corresponding to each tuple of indices passed to it.

This behaviour is kind of different to what I expected, which was the return the rectangular section described by the indices passed. Here is one way to get that:

```python
arr[[1, 5, 7, 2]][:, [0, 3, 1, 2]]
```



**Transposing arrays and swapping axes**

Arrays have the `T` attribute and also the `transpose` method.

For two dimensional matrices,

```python
arr = np.arange(15).reshape((3, 5))
arr
>>> array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14]])
arr.T
>>> array([[ 0,  5, 10],
       [ 1,  6, 11],
       [ 2,  7, 12],
       [ 3,  8, 13],
       [ 4,  9, 14]])
```

In higher dimensions, the `.transpose()` method accepts a tuple of axis numbers to permute the axes.

Both methods return a view on the data without making a copy.

Transposing with `.T` is a special case of swapping axes. We can use the `.swapaxes` method like this:

```python
arr = np.arange(16).reshape(2, 2, 4)
arr
>>> array([[[ 0,  1,  2,  3],
            [ 4,  5,  6,  7]],
           [[ 8,  9, 10, 11],
            [12, 13, 14, 15]]])
arr.swapaxes(1, 2)
>>> array([[[ 0,  4],
        [ 1,  5],
        [ 2,  6],
        [ 3,  7]],

       [[ 8, 12],
        [ 9, 13],
        [10, 14],
        [11, 15]]])
```

## Universal Functions

Ufuncs are functions that operate element-wise on numpy arrays. Functions that operate on a single array are *binary funcs*. For example, `np.exp` exponentiates each element in the given array. Ufuncs accept second "out" argument, which can be used to do operations in-place (`np.exp(arr, arr)`). Binary ufuncs accept two arrays.

 ## Array oriented programming

**Example: Plotting points in 2D**

Numpy can be used for accomplashing many tasks that would otherwise require explicit for loops. In the example below, we calculate values of a function for a 2D array of points by writing the sample equation as we would for a single variable.

```python
points = np.arange(-5, 5, 0.01)
xs, ys = np.meshgrid(points, points)
z = np.sqrt(xs**2 + ys**2)
```

### Expressing conditional logic as array operations

The numpy `where` function is a vectorised ternary operator, and it's very useful. It's usage is the following, for example

```python
xarr = np.array([1.1, 1.2, 1.3, 1.4, 1.5])
yarr = np.array([2.1, 2.2, 2.3, 2.4, 2.5])
result = np.where(cond, xarr, yarr)
```

The second and third arguments to `np.where` don't need to be arrays — they can be scalars as well.

### Mathematical and Statistical Methods

These aggregations or reductions are computed along an axis of a numpy array, and are accessed as methods. E.g. `arr.mean(), arr.std(), arr.sum()` . They accept an argument `axis` that dictates which axis the statistic is applied to. Some methods, such as `cumsum`, return an array of the same dimension but with the aggregated statistics in place of the original.

One useful method is `argmax`:

```python
arr = np.random.randn(10, 3)
arr
>>> array([[ 1.54332432,  0.53561416, -1.02432248],
       [ 1.43047791,  0.14327361,  1.59669535],
       [-0.64902694,  0.24191395,  0.24556259],
       [ 0.93030665, -0.73749359,  0.57190822],
       [-0.36390062,  0.26879633, -0.55379076],
       [-0.05742503, -0.66287889, -1.72674911],
       [-0.72013513,  0.37141885, -0.53454988],
       [-1.195886  , -0.2151384 , -0.6109359 ],
       [ 1.71092255, -1.27553902, -0.28536061],
       [-1.13710365,  0.73035844,  0.7742045 ]])
arr.argmax(axis=1)
>>> array([0, 2, 2, 0, 1, 0, 1, 1, 0, 2])
```

### Methods for Boolean Arrays

Boolean values are coerced into 1 or 0 for the preceeding methods, so that `.sum` can be used to count true values.

Use the methods `any` and `all` to return whether there is at least one true value of if every value is true in the array respectively.

### Sorting

Like stardard python lists, numpy arrays can be sorted inplace with the `.sort` method. An axis can be passed that will sort values along that axis respectively. A quick and dirty way to compute quantiles of an array is to sort it and select the value at a particular rank.

### Unique and other set logic

Numpy gives a built-in function for returning a *sorted* unique array.

```python
names = np.array(["Will", "Tom", "Eliza", "Will", "Tom"])
np.unique(names)
>>>  array(['Eliza', 'Tom', 'Will'], dtype='<U5'
# compared to plain python equivalent
sorted(set(names))
>>> ['Eliza', 'Tom', 'Will']
```

Another function, `np.in1d` tests membership of elements of an array in another array:

```python
values = [3, 5, 5, 1, 3, 5, 2]
np.in1d(values, [1, 3])
>>> array([ True, False, False,  True,  True, False, False])
```

Here is a list of array set operations.

| Method            | Description                                                  |
| ----------------- | ------------------------------------------------------------ |
| `unique(x)`       | Compute the sorted, unique elements in `x`                   |
| `intersect1d`     | Compute the sorted, common elements of `x` and `y`           |
| `union1d(x, y)`   | Computed the sorted union of elements                        |
| `in1d`            | Compute a Boolean array indicated if each element of `x` is in `y` |
| `setdiff1d(x, y)` | Set difference, elements of `x` that are not in `y`          |
| `setxor1d(x, y)`  | Symettric difference, elements that are in `x` or `y` but not in both |

## File input and output

**To save**

`np.save` saves an array uncompressed in a binary format, adding the extension `.npy` if it's not already there.

```python 
arr = np.arange(10)
np.save("some_array", arr)
np.load("some_array.npy", arr)
>>> array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

You can save multiple arrays to an uncompressed archive by using `savez` and passing the arrays as keyword arguments:

```python
np.savez("array_archive.npz", a = arr1, b = arr2)
```

Load these in with `np.load`, getting back a dict-like object:

```python
archive = np.load("array_archive.npz")
archive["b"]
>>> array([0, 1, 2, 3, 4])
```

If your array compresses well, consider using `np.savez_compressed` instead.

### Linear Algebra

**Multiplying matrices**

Use the array method `x.dot(y)`, the numpy function `np.dot(x, y)`, or the `@` infix operator `x @ y`. They're equivalent.

**Other functions**

Numpy implements standard matrix decompositions and other things like inverses and determinants under the hood using the same industry-standard libraries used in other languages liek MATLAB and R. See the book for a list.

### Pseudorandom Number Generation

The `numpy.random` module has a several functions for random number generation.

| Function    | Description                                      |
| ----------- | ------------------------------------------------ |
| seed        | Seed the random number generator                 |
| permutation | Return a random permutation of a sequence        |
| shuffle     | Randomly permute a sequence in-place             |
| rand        | Draw a sample from a uniform distribution        |
| uniform     | Draw a sample from a [0,1] uniform distribution  |
| randn       | Draw samples from a standard normal distribution |
| normal      | Draw samples from a normal distribution          |
| beta        | Draw samples from a beta distribution            |

The data generation in `numpy.random` uses a global random seed. The avoid global state, use `numpy.random.RandomState` to create a random number generator isolated from others:

```python
rng = np.random.RandomState(1234)
rng.randn(10)
>>> array([ 0.47143516, -1.19097569,  1.43270697, -0.3126519 , -0.72058873,
        0.88716294,  0.85958841, -0.6365235 ,  0.01569637, -2.24268495])
```

