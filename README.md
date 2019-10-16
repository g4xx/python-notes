# Python Notes

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

# Using square function without () and pass it as argument to our my_map function
# Also providing list of arguments to be used with square
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

# This assigns html_tag function to h1_tag with the html tag to create and _returns_ wrap_text function
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

## Mutable vs Immutable types

mutable = can be muated/changed
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