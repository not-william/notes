### Python language basics, IPython, and Jupyter Notebooks

## IPython and Jupyter notebooks

**Kernel features**

* Tab completion - namespace, file paths, even in strings
* `?` -- introspect, `??` -- display source code
* `?` one more usage - to search namespace in conjuction with wildcard `*` - e.g. to search top level numpy namespace for methods containing load, type `np.*load*?`
* `%run` magic function - execute a python script as if in command line. Run in a seperate namespace and variables made available in kernel
* `%load` magic function - paste the code of the script into the current cell block
* `%paste` takes whatever is in the clipboard and executes it
* `%cpaste` puts whatever is in the clipboard into a prompt - press Ctrl-C to break out of the prompt

**Keyboard shortcuts**

Same as Emacs keybindings

| Keyboard shortcut | Description                            |
|-------------------|----------------------------------------|
| Ctrl-P            | Search backwards in command history    |
| Ctrl-N            | Search forward in command history      |
| Ctrl-R            | Readline-style reverse history search  |
| Ctrl-Shift-V      | Paste text from clipboard              |
| Ctrl-C            | Interrupt currently executing code     |
| Ctrl-A            | Move cursor to beggining of line       |
| Ctrl-E            | Move cursor to end of line             |
| Ctrl-K            | Delete text from cursor to end of line |
| Ctrl-U            | Discard all text on current line       |
| Ctrl-F            | Move cursor forward one character      |
| Ctrl-B            | Move cursor backward one character     |
| Ctrl-L            | Clear screen                           |

**Matplotlib Integration**

Use the `%matplotlib` magic function to configure integration with Jupyter notebooks and the IPython shell.

* In IPython, use `%matplotlib` so you can create multiple plots without interfering with the console session
* In Jupyter, use `%matlotlib inline` 



## Python Language Basics

### Language semantics

Intented block in python are indicated by a semicolon `:`, and the block following it must have a uniform number of spaces or tabs indenting it. Semicolons can be use to seperate statements on a single line.

**Everything is an object**

Numbers, strings, functions... they are all objects, with a _type_ and internal data.

**Variables and argument passing**

In python, everytime you create an _assignment_, you are creating a _reference_ to the object of the righthand side of the equals sign. E.g. suppose you have `a = [1, 2, 3]; b = a` . In some languages (PHP!) this would cause the data `[1, 2, 3]` to be copies. In python, `a` and `b` actually now refer to the same object. To make them refer to different objects use the `copy` method.

When you pass arguments to a function, new local variables are created that _reference the original object without any copying_. 

```python
def append_element(some_list, element)
    some_list.append(element)
  
data = [1, 2, 3]
append_element(data, 4)

print(data)
>> [1, 2, 3, 4]
```

If you bind a new object to a variable inside a function, that change will not be reflected in the parent scope.

**Dynamic references, strong types**

In contrast to many compiled languages, object _references_ in python have no type associated to them - it's the object they refer to that has a type.

However, python is a typed language. For example running `'5' + 5` would raise an error - whereas in some languages an implicit conversion would take place.

Knowing the type of an object is important, and it's useful to be able to write functions that can deal with variables having different types. That's where the `isinstance` function comes in handy:

```python
a = 5
isinstance(a, int)
>> True
```

`isinstance` can accept a tuple of types if you want to check if the object type belongs to those present in the tuple:

```python
a = 5; b=4.5
isinstance(a, (int, float))
>> True

isinstance(b, (int, float))
>> True
```



**Attributes and methods**

Objects in python typically have both _attributes_ (other python objects that are stored "inside" the object) and _methods_ (functions associated with an object that have access to its internal data).

Attributes and methods can be accessed via the `getattr` function:

```python
getattr(a, 'split')
>> <function str.split>
```

In other languages, accessing objects by name is reffered to as "reflection". The `getattr`, `hasattr` and `setattr` functions can be very efffective for writing generic, reusable code.



**Iterables in python**

An iterable in python is anything which can be looped over in a for loop. You can pass any iterable to the `iterator` built in function and get an iterator object. That object can be passed to `next`, and everytime it is, the next element is returned.

**Imports**

* `import some_module` - like running all the code in `some_module.py`, except under the namespace `some_module`
* `from some_module import load_table` - only runs the code for object `load_table`  
* Use the `as` keyword to give imported objects different names

**Binary operators**

As you might expect - here are some things I didn't know:

| Operation    | Description                                            |
| ------------ | ------------------------------------------------------ |
| `a // b`     | floor division, dropping any remainder                 |
| `a ^ b`      | exclusive OR: true is `a` or `b` is true but but both  |
| `a is b`     | true if `a` and `b` reference the same python object   |
| `a is not b` | true if `a` and `b` reference different python objects |

**Mutable and immutable objects**

Most python objects (lists, dicts, NumPy arrays, and most user defined typed), are *mutable*. This means the values they contain can be modified.

Tuples and strings are *immutable*.



### Scalar types

| Type    | Description                                                  |
| ------- | ------------------------------------------------------------ |
| `None`  | Python's "null" value - only one instance of teh `None` object exists |
| `str`   | Holds Unicode (UTF-8) encoded strings                        |
| `bytes` | Raw ASCII bytes                                              |
| `float` | Double-precision (640bi) floating-point number               |
| `bool`  | `True` or `False` value                                      |
| `int`   | Abitrary precision signed integer                            |

**Strings**

For multiline strings, use `'''` or `"""`:

```python
my_string = """
This is a
Long string
"""
```

The string `my_string` actually contains 4 lines of text, since the line breaks after `"""` and `string` are included in the string. We can count the new Lin characters with the `count` method on `my_string`:

```python
my_string.count('\n')
>> 3
```

Many python objects can be converted to a string using the `str` function.

Strings are a squence of unicode characters and can therefore be treated like other sequences:

```python
s = 'python'
list(s)
>> ['p', 'y', 't', 'h', 'o', 'n']

s[:3]
>> pyt
```

The syntax `s[:3]` is called _slicing_ and is implemented for many python sequences.

The backslack `\` is an escape character.

**Bytes and unicode**

Python strings are Unicode by default. To use another encoding scheme, use the `.encode()` method.

**Type casting**

The `str`, `bool`, `int` and `float` types are also functions that can be used to cast values to those types. The boolean value of any number (`float` or `int`) is `True` if and only if it's non-zero.

**None**

`None` is Python's null value. `None` is not only a reserved keyword but also a unique instance of `NoneType`.

**Dates and times**

The built-in python `datetime` module provides `datetime`, `date` and `time` types.

* `datetime` - both the date and the time
* `date`  - just the date
* `time` - just the time!

```python
from datetime import datetime, date, time

# Defining a new datetime
dt = datetime(2020, 2, 22, 14, 51, 20)

# Getting specific elements from the datetime object
dt.day
>> 22
dt.second
>> 20

# Getting date and time objects from the datetime
dt.date()
>> datetime.date(2020, 2, 22)

dt.time()
>> datetime.time(14, 51, 20)

# format as a string:
dt.strftime('%Y-%m-%d %H:%M')
>> '2020-02-22 14:51'

# convert (parse) string to datetime:
datetime.strptime('20210818', '%Y%m%d')
>> datetime.datetime(2021, 8, 18, 0, 0)
```

When you are aggregating or grouping timeseries data, it may be useful to replcae the time elements with a 0:

```python
dt.replace(minute=0, second=0)
>> datetime.datetime(2020, 2, 22, 0, 0)
```

The difference of two `datetime` objects produces a `datetime.timedelta` type:

```python
dt2 = datetime(2020, 3, 1, 0, 0)
delta = dt2 - dt1

delta
>> datetime.timedelta(days=7, seconds=32920) # this means 7 days and 32920 seconds
```

Adding a `timedelta` to a `datetime` produces a new shifted `datetime`.

| Type | Description                                                  |
| ---- | ------------------------------------------------------------ |
| %Y   | Four-digit year                                              |
| %y   | Two-digit year                                               |
| %m   | Two-digit month                                              |
| %d   | Two-digit day                                                |
| %H   | Hour (24 hour clock)                                         |
| %I   | Hour (12 hour clock)                                         |
| %M   | Two-digit minute                                             |
| %S   | Second                                                       |
| %w   | weekday as an integer                                        |
| %U   | Week number of the year; Sunday is considered the first day of the week, and days before the first Sunday are considered "week 0" |
| %W   | Week number of the year; Monday is considered the first day of the week |
| %z   | UTC time zone offset as +HHMM or -HHMM                       |
| %F   | Shortcut for %Y-%m-%d                                        |
| %D   | Shortcut for $m/$d/$y                                        |

### Control Flow

It's possible to chain comparisons:

```python
4 > 3 > 2 > 1
>> True
```

**for loops**

You can advance a for loop to the next iteration by using `continue`.

You can break out of a for loop altogether by using `break`. The `break` keyword only exits the inner-most loop.

If the elemets of a collection are sequences (tuples or lists, say), they can be _unpacked_ into variables:

```python
for a, b, c in iterator:
	# do something
```

**pass**

`pass` means do nothing. It's required in blocks where nothing happens.

**range**

The `range` functions returns an iterator the yields a sequence of evenly spaced integers.

```python
range(10)
>> range(0, 10)

list(range(5, 30, 3))
>> [5, 8, 11, 14, 17, 20, 23, 26, 29, 32, 35, 38, 41, 44, 47]
```

While you can use `list` to store all the integers generated by `range`, sometimes it's useful to keep the iterator. For example here we sum all numbers from 0 to 99,999 that are multiples of 3 or 5:

```python
sum = 0
for i in range(100000):
	if i % 3 or i % 5:
		sum += i
```

While the range generated can be arbitrarily large, the memory use at any given time may be very small.

**Ternary expressions**

```python
value = true_expr if condition else false_expr
```



## Built in Data Structures, Functions, and Files

### Data Sctructures and Sequences

**Tuple**

A tuple is a fixed-length, immutable sequence of Python objects.

```python
tup = 4, 5, 6
tup
>> (4, 5, 6)

tup2 = 4, 5, (6, 7)
tup2
>> (4, 5, (6, 7))

# Any sequence of iterator can be converted to a tuple:
tup3 = tuple('string')
tup3
>> ('s', 't', 'r', 'i', 'n', 'g')
```

Once a tuple is created it's not possible to modify which _object_ is stored in each slot. However, if the object is mutable, it's possible to modify it in-place:

```python
tup = 'foo', [1, 2], False
tup[1].append(3)
tup
>> ('foo', [1, 2, 3], False)
```

We can concatenate tuples using `+` and we can multiply tuples by an integer to create copies of it appended to one another:

```pyt
('foo', 'bar') * 4
>> ('foo', 'bar', 'foo', 'bar', 'foo', 'bar', 'foo', 'bar')
```

Note that this does not copy the objects, only the rederences to them.

**Unpacking tuples**

```python
tup = (4, 5, 6)
a, b, c = tup
b
>> 5

# Even sequences with nested tuples can be unpacked:
tup = 4, 5, (6, 7)
a, b, (c, d) = tup
d
>> 7

# we can easily swap variable names this way:
a, b = b, a
a
>> 7
```

We can also 'pluck' the first few elements using the syntax  `*rest`

```python
tup = 3, 4, 5, 6, 7, 8
a, b, *rest = tup
b
>> 4
rest
>> [5, 6, 7, 8]
```

Useful tuple method: `count`, counts the number of occurances of a given object

```python
tup = 1, 2, 2, 3, 4, 2, 5
tup.count(2)
>> 3
```



#### List

Cool way to check if items are unique:

```python
len(feature_names) == len(set(feature_names)) # also just a cool way to get the number of unique items!
```

Use the `append` method to add an item to the end of the list. Use the `insert` method to insert an item at the specified point in the list.

The inverse operation to `insert` is `pop`, which removes the element at the given position and returns it.

Elements can be removed with `remove`, which located the first instance of the given value and removes it.

```python
my_list = ['foo', 'bar', 'baz', 'foo']
my_list.insert(4, 'dwarf')

my_list
>> ['foo', 'bar', 'baz', 'dwarf', 'foo']

my_list.pop(1)
>> 'bar'
my_list
>> ['foo', 'baz', 'dwarf', 'foo']

my_list.remove('foo')
my_list
>> ['baz', 'dwarf', 'foo']


```

**Concatenating and combining lists**

Either use the concatenator operation `+` or the `extend` method. The `extend` method is faster.

**Sorting**

You can sort a list in-place by calling the `sort` function:

```python
a = [7, 2, 5, 1, 3]
a.sort()
a
>> [1, 2, 4, 5, 7]
```

Sort has the ability to pass in a _sort key_ - e.g.

```python
b = ['hat', 'table', 'he', 'saw']
b.sort(key=len)
b
>> ['he', 'hat', 'saw', 'table']
```

**Binary search and maintaining a sorted list**

The built in `bisect` module implements binary search and insertion into a sorted list. `bisect.bisect` finds the location where as element should be inserted to keep it sorted, while `bisect.insert` actually does this. Note that this function doesn't check whether a list is sorted as it would be computationally expensive.

**Slicing**

After a second colon, we have a step operator:

```python
seq = [6, 2, 8, 4, 6, 6, 9, 3, 0]
seq[::2]
>> [6, 8, 6, 9, 0]
seq[::-1]
>> [0, 3, 9, 6, 6, 4, 8, 2, 6]
```

#### Built-in sequence functions

**enumerate**

Used when looping through a sequence and wish the keep track of the index:

```python
for i, value in enumerate(collection):
	# do something with value
```

**sorted**

The `sorted` function returns a new sorted list from the elements of any sequence (not in-place). Also accepts a sort key.

**zip**

`zip` pairs up elements from two or more sequences.

```python
seq1 = ['foo', 'bar', 'baz']
seq2 = ['one', 'two', 'three']
zipped = zip(seq1, seq2)
list(zipped)
>> [('foo', 'one'), ('bar', 'two'), ('baz', 'three')]
```

Given a "zipped" sequence, `zip` can be applied to "unzip" the sequence:

```python
players = [('roger', 'federer'), ('novak', 'djokovic'), ('rafael', 'nadal')]

first_names, last_names = zip(*players)

first_names
>> ('roger', 'novak', 'rafael**'
last_names
>> ('federer', 'djokovic', 'nadal')
```

**reversed**

`reversed` iterated over the elements of a sequence in reverse order.

#### dict

It's a flexibly sized collection of _key-value_ pairs, where the keys and values are python objects. Known as a _hash map_ or _associative array_ in other languages.

You can check if a dict contains a key using the same syntax using the `in` keyword.

Keys and their values can be removed using the `del` keyword, or the `pop` method, which returns the deleted value.

The `keys` and `values` methods give you iterators of the dicts keys and values. While the key-value pairs are not in any particular order, these functions put the keys and values in the same order.

You can merge one dict into another using the `update` method:

```python
d1 = {'a': 'foo', 'b': 'bar'}
d1.update({'a': 'updated', 'c': 'baz'})
d1
>> {'a': 'updated', 'b': 'bar', 'c': 'baz'}
```

**Creating dicts from sequences**

It's common to end up with two lists which you want to pair up into a dict. Since a dict is essentially a collection of 2-tuples, it accepts a list of 2-tuples:

```python
mapping = dict(zip(range(5), reversed(range(5))))
mapping
>> {0: 5, 1: 4, 2: 3, 3: 2, 4: 1, 5: 0}
```

**Default values**

It's very common to have logic like this:

```python
if key in some_dict:
  value = some_dict[key]
else:
  value = default_value
```

Because of this, the dict methods `get` and `pop` can take a default value to be returned, so that the above if-else block can be writted as

```python
value = some_dict.get(key, default_value)
```

If the key doesn't exist, `get` returns `None` by default where `pop` gives an exception.

With setting values, a common task is for values in dicts to be other collections, like lists. For this the `setdefault` method sets the value of the given key _if not already set_, and return the defualt object. For example

```python
for word in words:
	letter = word[0]
	by_letter.setdefault(letter, []).append(word)
```

The built-in `collection` class has a useful class, `defaultdict`, that makes this even easier. You pass a type or function for generating the default value for each slot:

```python
from collections import defaultdict
by_letter = defaultdict(list)
for word in words
	by_letter[word[0]].append(word)
```

**Valid dict key types**

Values can be anything, but keys generally have to be immutable, e.g. a string, int or float, or tuple, where all the elements of the tuple are also immutable. The technical condiction is _hashability_. You can check if an item is hashable by using the `hash` function.

#### Set

A set is an unordered collection of unique elements. Kind of like a dict, but keys only, no values. Create one using the set function or using a _set literal_:

```python
set([2, 2, 2, 1, 3])
>> {1, 2, 3}
{2, 2, 2, 1, 3}
>> {1, 2, 3}
```

Sets support mathematical set operations union, intersection, difference, and symettric difference.

| Function                           | Alternative syntax | Description                                                  |
| ---------------------------------- | ------------------ | ------------------------------------------------------------ |
| `a.add(x)`                         |                    | Add element `x` to the set `a`                               |
| `a.clear()`                        |                    | Resets a set to empty                                        |
| `a.remove(x)`                      |                    | Remove element `x` from the set `a`                          |
| `a.pop()`                          |                    | Removes an arbitrary element from the set, raising `KeyError` if the set is empty |
| `a.union(b)`                       | `a | b`            | Set union                                                    |
| `a.update(b)`                      | `a |= b`           | As above but in-place on a                                   |
| `a.intersection(b)`                | `a & b`            | Set intersection                                             |
| `a.intersection_update(b)`         | `a &= b`           | In-place intersection                                        |
| `a.difference(b)`                  | `a - b`            | Set difference                                               |
| `a.difference_update(b)`           | `a -= b`           | Set difference in-place                                      |
| `a.symmetric_difference(b)`        | `a ^ b`            | Symmetric difference (elements in a or b but not both)       |
| `a.symmetric_difference_update(b)` | `a ^= b`           | In-place symettric difference (set a to the symettric difference) |
| `a.issubset(b)`                    |                    |                                                              |
| `a.superset(b)`                    |                    |                                                              |
| `a.isdisjoint(b)`                  |                    |                                                              |

#### List, Set and Dict Comprehensions

Syntax:

```python
[expr for val in collection if condition]
```

Also for dicts and sets. For example a dict comprehension to create a loopup map:

```python
cities = ['london', 'chicago', 'berlin', 'prague', 'shanghai']
loc_mapping = {value: key for key, value in enumerate(cities)}
loc_mapping
>> {'london': 0, 'chicago': 1, 'berlin': 2, 'prague': 3, 'shanghai': 4}
```

Also have nested list comprehensions, e.g.

```python
some_tuples = [(1, 2, 3), (4, 5, 6), (7, 8, 9)]
flattened = [x for tup in some_tuples for x in tup] # for keywords in order of the loops
flattened = [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### Functions

Functions have a local namespace. Variables can be assigned in the global namespace using the `global` keyword declaration. 

**Map**

The built-in `map` function applies a function to a sequence of some kind:

```python
def square(x):
	return x ** 2
my_list = [3, 6, 7]
map(square, my_list)
>> [9, 36, 49]
```

#### **Currying**

Partial functions.

#### Generators

Having a consistent way to make all types of sequences iterable is achieved through the _iterator protocol_.

Basically, there's a next item.

You can make your own generators using the `yield` construct:

```python
def squares(n):
	for i in range(n + 1):
		yield i ** 2

gen = squares(10)
for x in gen:
  print(x, end=' ')
>> 1, 4, 9, 16, 25, 36, 49, 64, 81, 100
```

You can also use generator comprehentions:

```python
gen = (x ** 2 for x in range(10)) # equivalent to above.
```

**itertools module**

This is a handy module with convinience methods for iterators.

For example, the `groupby` method, which groups consecutive items in the sequence by some key function. 

```python
places = ['Tokyo', 'Tohoku', 'Nara', 'Nagasaki', 'Sendai', 'Nikko']
def first_letter(x: string):
	return x[0]
for first_letter, places in itertools.groupby(places, first_letter):
	print(first_letter, list(places)) # places is a generator
>> T ['Tokyo', 'Tohoku']
   N ['Nara', 'Nagasaki']
   S ['Sendai']
   N ['Nikko']
```

Here's a table with some useful methods of itertools

| Function                      | Description                                                  |
| ----------------------------- | ------------------------------------------------------------ |
| `combinations(iterable, k)`   | Generates a sequence of k-tuples of combinations of elements in the iterable, without replacement. Also see `combinations_with_replacement`. |
| `permutations(iterable, k)`   | Genreates a sequence of k-tuples of _permutations_, i.e., order matters in the tuples. There will be $ \frac{n!}{(n-k)!}$ permutations. |
| `groupby(iterable, function)` | Lists consecutive items in the iterator by return value of function given. |
| `product(*iterable)`          | Gives the Cartesian product of the input iterator as tuples, similar to a nested `for` loop. |

### Errors and Exception Handling

Functions can throw errors that have different types; here's not to handle them with try/except blocks:

```python
my_string = 'something'
tuple(my_string)
>> ValueError Exception
try:
	tuple(my_string)
except:
	my_string
>> 'something'

try:
	tuple((1, 3))
except ValueError: # only surpress ValueErrors
  my_string
>> TypeError Exception
```

You can execute a block only if the `try` part was executed with an `else` block, and any code regardless in the `finally` block:

```python
try:

except:

else:

finally:
  
```

### Files and the Operating System

You can open a file using the `open` function:

```python
path = 'book.txt'
f = open(path) # opened in read only mode 'r' by default
```

We can iterate over the lines of the file directly on `f`:

```python
for line in f:
	pass
```

Lines come with end-of-line markers intact, so you'll often want to do

```python
lines = [x.rstrip() for x in open(path)]
lines
>> ['Hello, this is the first line',
    'This is the second line']
```

When you use `open` to acess files, you need to `close` the handler after you're done, to free up resources from the operating system. You can get around this using a `wuth` clause, which automatically does this when you exit the block:

```python
with open(path) as f:
  pass # do something with f
```

If you pass the `'w'` marker you will create a blank file (overwriting one if it already exists - careful!)

```python
open(path, 'w')
```

Alternatively the `'x'` marker creates a new file but fails if a file already exsits.

**read, tell and seek**

* `read` returns the next n number of _characters_ in a file from the current position. What constitues a character is determined by the file's encoding (e.g. UTF-8). We can switch to byte mode by appending `'b'` mode argument. `read` advances the handler's position by the number of bytes it took to read n characters. Note that this may be more than n.
* `tell` gives the current *byte* position.
* `seek` moves the position to the given one.

Here's a table of the file modes:

| Mode | Description                                                  |
| ---- | ------------------------------------------------------------ |
| `r`  | Read-only                                                    |
| `w`  | Write-only: creates a new file (erasing data for any file with the same name) |
| `x`  | Write-only: creates a new file, but fails if the file already exists |
| `a`  | Append to existing file (creates the file if it doesn't already exist) |
| `r+` | Read and write                                               |
| `b`  | Add to mode for binary file (i.e. `rb` or `wb`)              |
| `t`  | Text mode (default if not specified)                         |

**Writing to files**

Having opened a file in write mode, we can write to the file using the `write` or `writelines` methods.

```python
# code to remove blank lines
with open('temp.txt', 'w') as f:
	f.writelines(x for x in open(path) if len(x) > 1)
```

Here's a table of commonly used methods.

| Method                | Description                                                  |
| --------------------- | ------------------------------------------------------------ |
| `read([size])`        | Return data from file as a string, eith optional size argument |
| `readlines([size])`   | Return list of lines in the file, with optional size argument |
| `write(str)`          | Write passed string to file                                  |
| `writelines(strings)` | Writes the passed sequence of strings to file                |
| `close()`             | Closes the handle                                            |
| `flush()`             | Flush the internal I/O buffer to disk                        |
| `seek(pos)`           | Move to indicated position                                   |
| `tell()`              | Returns the current file position                            |
| `closed`              | `True` if the file is closed                                 |

Text mode, comined with the `encoding` option of `open`, gives us a nice way to convert between encodings:

```python
with open('original.txt') as source:
  with open('new.txt', 'xt', encoding='iso-8859-1') as sink:
    sink.write(source.read())
```

<img src=""



