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