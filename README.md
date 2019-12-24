# Python Notes

- [Python Notes](#python-notes)
  - [First Class Functions](#first-class-functions)
    - [Assigning function to varbiale](#assigning-function-to-varbiale)
    - [Passing functions as arguments to other functions](#passing-functions-as-arguments-to-other-functions)
    - [Returning functions from other functions](#returning-functions-from-other-functions)
  - [Closures](#closures)
  - [Decorators](#decorators)
  - [Generators](#generators)
  - [Mutable vs Immutable types](#mutable-vs-immutable-types)
  - [Function Default Lists](#function-default-lists)
  - [Making Python Beautiful](#making-python-beautiful)
    - [Looping over two collections](#looping-over-two-collections)
    - [Counting with dictionaries](#counting-with-dictionaries)
    - [Grouping with dictionaries](#grouping-with-dictionaries)
    - [Clarify function calls with keyword arguments](#clarify-function-calls-with-keyword-arguments)
    - [Simultaneus state updates](#simultaneus-state-updates)
    - [Concatenating strings](#concatenating-strings)
    - [Updating sequences](#updating-sequences)

## First Class Functions

### Assigning function to varbiale

``` python
def hello():
    return ('hi')

# Assign function to variable but don't execute it
a = hello

# Display address in memory
print(a)

# Execute assigned function - works same as doing hello()
print(a())

# Execute function and assign value to variable
b = hello()

print(b)
```

```python
>>> <function hello at 0x7fd3ca1ab268>
>>> hi
>>> hi
```

### Passing functions as arguments to other functions

```python
# Create a function
def square(x):
    return x * x

def cube(x):
    return x * x * x

# Create my_map function that takes some other function as first argument.
# The second argument is list that will become arguments for the other function
def my_map(func, arg_list):
    result = []
    for i in arg_list:
        # Here we are executing passed function
        # while iterating through arg_list
        result.append(func(i))
    return result

# Using square function without () and pass it as argument to our my_map
# function. Also providing list of arguments to be used with square:
squares = my_map(square, [1,2,3,4,5])

# Same my_map function but using cube function as argument
cubes = my_map(cube, [1,2,3,4,5])

# Output:
>>> [1, 4, 9, 16, 25]
>>> [1, 8, 27, 64, 125]
```

### Returning functions from other functions

```python
def html_tag(tag):

    def wrap_text(msg):
        print('<{0}>{1}</{0}>'.format(tag, msg))
    # This return wrap_text _function_ as variable
    return wrap_text

# This assigns html_tag function to h1_tag with the html tag to create and
# _returns_ wrap_text function
h1_tag = html_tag('h1')

# this _executes_ wrap_text function assigned to h1_tag with msg
h1_tag('Test Heading!')

# Output:
>>> <h1>Test Heading!</h1>

```

Also remember that assigning class to variable without () is acctualy creating reference to that class insted of class instance

```python
>>> x = myClass
>>> x
<class '__main__.myClass'>

>>> y = myClass()
>>> y
<__main__.myClass object at 0x7ff0dfc3c4a8>
```

## Closures

A closure is an inner function that remebers and has access to variables in the local scope in which it was created. Even after the outer function has finished executing.

```python
# We create a function
def outer_func():

    # We assign value to our variable
    message = 'Hi'

    # We define inner_func and print the message variable
    def inner_func():
        print(message)

    # We return inner_func - note there is no () for execution
    return inner_func

# my_func is equal to returned value of outer_func (inner_funct)
my_func = outer_func()

my_func()
my_func()
my_func()

# executing my_func is remembering message variable name!
>>> Hi
>>> Hi
>>> Hi
```

Functions can also take parameters and remember them when executed

```python
# We pass a variable to create tag from
def html_tag(tag):

    # This inner function also accepts a parameter that prints between tags
    def wrap_text(msg):
        print('<' + tag + '>' + msg + '<' + tag + '>')

    # Returning inner function wrap_text without ()
    return wrap_text

# Assigning and executing html_tag with h1 as parameter
print_h1 = html_tag('h1')

# Executing print_h1 with msg. Note that it behaves just like calling
# wrap_text() but it remembers the tag variable value!
print_h1('Test Headline!')

>>> <h1>Test Headline!<h1>
```

Another slightly more difficult example

```python
import logging
logging.basicConfig(filename='example.log', level=logging.INFO)

def logger(func):
    def log_func(*args):
        logging.info(
            'Running "{}" with arguments {}'.format(func.__name__, args))
        print(func(*args))
    return log_func

def add(x, y):
    return x+y

def sub(x, y):
    return x-y

add_logger = logger(add)
sub_logger = logger(sub)

add_logger(3, 3)
add_logger(4, 5)

sub_logger(10, 5)
sub_logger(20, 10)
```

## Decorators

Decorator is a function that takes another function as an argument and adds some kind of functionality and then returns another function. All of this without altering source code of original function you passed in.

## Generators

Difference between a generator and a regular loop is that a regular loop first processes all the data and returns all the data at once. Imagine having one billion records. Looping over that data will take you a lot of time. Now try looping that same thing with generator. It _yeilds_ (returns) one piece of the data at a time which you can work with.

``` python
# This simple function take an argument (a list) and appends to another list
# with square roots. Then It returns the resulting list.

def square_numbers(nums):
    result = []
    for num in nums:
        result.append(i*i)
    return result

my_nums = square_numbers([1,2,3,4,5])

print(my_nums)

# The side effect of this is that the function will only return the result if
# all the elements of the arguments are processed. Consider how long it could
# take if the list had milion+ elements.

# Let's take a look at a generator.
def square_numbers(nums)
    for i in nums:
        yield(i*i)

my_nums = square_numbers([1,2,3,4,5])

# This looks simpler. Doesn't it? However if we:

print(my_nums)

>>> <generator object square_numbers at 0x7f485827e570>

# In order to print element we must use next():

print(next(my_nums))
>>> 1
print(next(my_nums))
>>> 4
print(next(my_nums))
>>> 9

# Also you can print all the elements from list:

for num in my_nums:
    print(num)

>>> 16
>>> 25

# Wait. What? Why only 2 values?
# That is beacause the generators are exhaustive. Every time you use up element
# from that list it will only give you the next() value. That is why it is so
# memory efficient and fast.

# If you try to run next after exhausting the list you'll get an error:

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

## Mutable vs Immutable types

mutable = can be mutated/changed
immutable = can't be mutated/changed

| Class | Immutable? |
|:---------:|:---:|
|    bool   | yes |
|    int    | yes |
|   float   | yes |
|    list   | no  |
|    tuple  | yes |
|    str    | yes |
|    set    | no  |
| frozenset | yes |
|    dict   | no  |

Strings cannot be changed, they are **immutable**. You can only copy and modify to another object.

```python
>>> x = 'tomasz'
>>> x[0] = 'T'

# Output:
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
```

Lists are **mutable** and you can change their values

```python
>>> mylist = [0, 1, 2, 3]
>>> mylist[0] = 3
>>> mylist

# Output:
>>> [3, 1, 2, 3]
```

## Function Default Lists

Default list is created only **once** during function creation and not during its call

```python
def addToList(val, list=[]):
    list.append(val)
    return list

list1 = addToList(1)
list2 = addToList(123, [])
list3 = addToList('a')

print("list1 = %s" % list1)
print("list2 = %s" % list2)
print("list3 = %s" % list3)

# Output
>>> list1 = [1, 'a']
>>> list2 = [123]
>>> list3 = [1, 'a']
```

***

Entire list replacment

```python
def fun(l):
    l=[3,2,3,4]

l = [1,2,3,4]
fun(l)
print(l)

# Output:
>>> [1, 2, 3, 4]
```

Single element replacement

```python
def fun(l):
    l[0] = 3

l = [1,2,3,4]
fun(l)
print(l)

# Output:
>>> [3, 2, 3, 4]
```

## Making Python Beautiful

### Looping over two collections

``` python
names = ['John', 'Ben', 'Adam']
colors = ['red', 'green', 'blue']

# No, just no
n = min(len(names), len(colors))
for i in range(n):
    print(names[i], '-->', colors[i])

# Python 3.x way
for name, color in zip(names, colors):
    print(name, '-->', color)
```

### Counting with dictionaries

``` python
# Not bad way
colors = ['red', 'green', 'red', 'blue', 'green', 'red']

d = {}
for color in colors:
    if color not in d:
        d[color] = 0
    d[color] += 1

>>> {'red': 3, 'green': 2, 'blue': 1}

# Better way
d = {}
for color in colors:
    d[color] = d.get(color, 0) + 1

>>> {'red': 3, 'green': 2, 'blue': 1}


# Uber way but you need to know about defaultdicts, ints without arguments,
# collections and factory functions. On top of that it doesn't return dict but
# defaultdict. Not beginner friendly.
d = defaultdict(int)
for color in colors:
    d[color] += 1

>>> defaultdict(<class 'int'>, {'red': 3, 'green': 2, 'blue': 1})
```

### Grouping with dictionaries

``` python
names = ['raymond', 'rachel', 'matthew', 'roger',
         'betty', 'melissa', 'judith', 'charlie']

# Not bad way
d = {}
for name in names:
    key = len(name)
    if key not in d:
        d[key] = []
    d[key].append(name)

>>> {7: ['raymond', 'matthew', 'melissa', 'charlie'], 6: ['rachel', 'judith'],
     5: ['roger', 'betty']}


# A bit better way, still not that beautiful
d = {}
for name in names:
    key = len(name)
    d.setdefault(key, []).append(name)

>>> {7: ['raymond', 'matthew', 'melissa', 'charlie'], 6: ['rachel', 'judith'],
     5: ['roger', 'betty']}


# Best way (but need to have the knowlege what is going on)
d = defaultdict(list)
for name in names:
    key = len(name)
    d[key].append(name)

>>> defaultdict(<class 'list'>, {7: ['raymond', 'matthew', 'melissa',
                'charlie'], 6: ['rachel', 'judith'], 5: ['roger', 'betty']})
```

### Clarify function calls with keyword arguments

``` python
# This rises the questions. What is False, 20 or True?
twitter_search('@obama', False, 20, True)

# Better way - collections.namedtuple()
twitter_search('@obama', retweets=False, numtweets=20, popular=True)
```

### Simultaneus state updates

``` python
# Don't do this.. No need for creating temporary variables to hold values
tmp_x = x + dx * t
tmp_y = y + dy * t
tmp_dx = influence(m, x, y, dx, dy, partial='x')
tmp_dy = influence(m, x, y, dx, dy, partial='y')
x = tmp_x
y = tmp_y
dx = tmp_dx
dy = tmp_dy

# Do this instead. From the docs 'while evaluating assignment, the right-hand
# side is evaluated before left-hand side
x, y, dx, dy = ( x + dx * t,
                 y + dy * t,
                 tmp_dx = influence(m, x, y, dx, dy, partial='x'),
                 tmp_dy = influence(m, x, y, dx, dy, partial='y'))
```

### Concatenating strings

``` python
names = ['raymond', 'rachel', 'matthew', 'roger',
         'betty', 'melissa', 'judith', 'charlie']

# Not fun way
s = names[0]
for name in names[1:]:
    s += ', ' + name
print(s)

# The only way
print(', '.join(names))
```

### Updating sequences

``` python
names = ['raymond', 'rachel', 'matthew', 'roger',
         'betty', 'melissa', 'judith', 'charlie']

# If you do this many times - you are doing it wrong
del names[0]
names.pop(0)
names.insert(0, 'mark')

# Instead use proper data structure. Deque is a list-like sequence optimized
# for data accesses near its endpoints
from collections import deque

names = deque(['raymond', 'rachel', 'matthew', 'roger',
               'betty', 'melissa', 'judith', 'charlie'])

del names[0]
names.popleft()
names.appendleft('mark')
```
