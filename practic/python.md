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

### os.sep

当前系统文件的路径分隔符(windows '\', linux '/')

### os.popen()
通过os执行需要创建文件的命令时，不要忘记穿第二个参数
>os.popen(command[, mode[, bufsize]])  

* command -- 使用的命令。

* mode -- 模式权限可以是 'r'(默认) 或 'w'。

* bufsize -- 指明了文件需要的缓冲大小：0意味着无缓冲；1意味着行缓冲；其它正值表示使用参数大小的缓冲（大概值，以字节为单位）。负的bufsize意味着使用系统的默认值，一般来说，对于tty设备，它是行缓冲；对于其它文件，它是全缓冲。如果没有改参数，使用系统的默认值。
