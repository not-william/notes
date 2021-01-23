## Series

### Creation
```python
# From list
obj = pd.Series([3, 9, -4, 5])
# Out:
# 0    3
# 1    9
# 2   -4
# 3    5
# dtype: int64

# From a list specifying an index
obj2 = pd.Series([3, 9, -4, 5], index=['b', 'c', 'a', 'd'])
# Out:
# b    3
# c    9
# a   -4
# d    5
# dtype: int64

# From dictionary
sdata = {"London": 9.0, "Paris": 2.2, "Mumbai": 18.4, "Tokyo": 9.3}
obj3 = pd.Series(sdata)
# Out:
# London     9.0
# Paris      2.2
# Mumbai    18.4
# Tokyo      9.3
# dtype: float64

# From dictionary, specifying key order
cities = ["Mumbai", "London", "Paris", "Shanghai"]
obj4 = pd.Series(sdata, index=cities) 
# Out:
# Mumbai    18.4
# London     9.0
# Paris      2.2
# Shanghai   Nan  (Shanghai not in dictionary)
# dtype: float64
# (Note Tokyo is missing, as it's not in `cities`)

# Checking inclusion
'a' in obj2
# Out:
# True
```

### Indexing and selecting data
```python
# Select single values
obj2['a']
# Out:
# -4

# Select set of values
obj2[['c', 'a', 'd']]
# Out:
# 9
# -4
# 5

# Use Numpy functions and Numpy-like operations and the index-value link is preserved
obj2[obj2>0]
# Out:
# b    3
# c    9
# d    5
# dtype: int64

obj2 * 2
# Out:
# b     6
# c    18
# a    -8
# d    10
# dtype: int64

np.exp(obj2)
# Out:
# b      20.085537
# c    8103.083928
# a       0.018316
# d     148.413159
# dtype: float64

# Series automatically aligns by index for arithmetic
obj3 + obj4
# Out:
# London      18.0
# Mumbai      36.8
# Paris        4.4
# Shanghai     NaN
# Tokyo        NaN
# dtype: float64
```

### Dealing with missing values
```python
pd.isnull(obj4) # or obj4.isnull()
# Mumbai      False
# London      False
# Paris       False
# Shanghai     True
# dtype: bool

pd.notnull(obj4) # or obj4.notnull()
Mumbai       True
London       True
Paris        True
Shanghai    False
dtype: bool
```