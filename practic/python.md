### difference
Returns the difference between two iterables.  
Use list comprehension to only keep values not contained in b
```python
def difference(a, b):
    return [item for item in a if item not in b]
```

```python
difference([1, 2, 3], [1, 2, 4]) # [3]
```

### is_upper_case
Checks if a string is upper case.

Convert the given string to upper case, using str.upper() method and compare it to the original.

```python
def is_upper_case(string):
    return string == string.upper()
```

```python
is_upper_case('ABC') # True
is_upper_case('a3@$') # False
is_upper_case('aB4') # False
```

### capitalize_every_word
Capitalizes the first letter of every word in a string.

Uses str.title to capitalize the first letter of every word in the string.

```python
def capitalize_every_word(string):
    return string.title()
```

```python
capitalize_every_word('hello world!') # 'Hello World!'
```