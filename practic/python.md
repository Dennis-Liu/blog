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

### import parent folder library
```python
sys.path.append('../')
```


### os.sep

当前系统文件的路径分隔符(windows '\', linux '/')

### os.popen()
通过os执行需要创建文件的命令时，不要忘记穿第二个参数
>os.popen(command[, mode[, bufsize]])  

* command -- 使用的命令。

* mode -- 模式权限可以是 'r'(默认) 或 'w'。

* bufsize -- 指明了文件需要的缓冲大小：0意味着无缓冲；1意味着行缓冲；其它正值表示使用参数大小的缓冲（大概值，以字节为单位）。负的bufsize意味着使用系统的默认值，一般来说，对于tty设备，它是行缓冲；对于其它文件，它是全缓冲。如果没有改参数，使用系统的默认值。


### Decorator & Parameter
* *装饰器用法，好处*
* 简化代码，避免重复性代码
* 打印日志 @log
* 检测性能 @performance
* 数据库事务 @transaction
* URL路由 @post('/register')
```python
from functools import wraps
from datetime import datetime
def logs(func):
    @wraps(func) #把原函数所有必要的属性复制到新函数上，与原来简单的装饰器相比不只是增加了新功能，还保留原函数的属性
    def wrapper(*args,**kwargs):
        print("running function: ",func.__name__)
        print(datetime.now())
        print("paramenter1 ",args)
        print("paramenter2 ",kwargs)
        func(*args,**kwargs)
        print(datetime.now())
    return wrapper
@logs
def anything(x,*args,**kwargs):#可变参数，顺序不可变
    """function comments"""
    print(x)
    print(args[0])
    print(kwargs['a'])
if __name__ == '__main__': #单独执行当前文件属性__name__的值是'__main__',别其他文件调用时属性__name__的值是文件名
    anything(1,2,a="3")
    print("function name is {}".format(anything.__name__))
    print("function doc is {}".format(anything.__doc__))
```

```Terminal
[dhc.dennisliu@k8snode3 Einvoice]$ python3 test_decorator.py
running function:  anything
2022-07-04 10:38:35.486871
paramenter1  (1, 2)
paramenter2  {'a': '3'}
1
2
3
2022-07-04 10:38:35.487868
function name is anything
function doc is function comments
```
