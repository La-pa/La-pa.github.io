---
title: Python小技巧
tags: [Python]
categories: 技术博客
date: 2023-11-7 21:51:00
---

### 给程序加上进度条

```Python
from tqdm import tqdm,trange
import  time

for i in trange(100, colour="green", desc="testing"):
    time.sleep(0.1)
```

### 当函数还未实现的时候进行报错

```python

def func1():
    pass

def func2():
    ...


def func3():
    raise NotImplementedError("还未实现")


func3()
```



### 交换两个数

```python
a = 1
b = 2

a, b = b, a
print(f"a:{a}, b:{b}")
```

###  yield操作

```python
def fibonacci(n):
    a, b= 0, 1
    for _ in range(n):
        yield a
        a, b = b, a + b

for i in fibonacci(10):
    print(i)

```

### 函数多值返回

```Python
def user_info():
    name = 'Abhay'
    age = 26
    address = 'Bangalore'
    return name, age, address

Name , Age , Address = user_info()
print(Name, Age, Address) 
```

### 格式化大数字

```python
num = 24_5645_6987
num2 = 3_456_986_784 # 魔幻的写法，魔幻的世界
sum = num + num2
print(f"{sum:,}") # 5,913,443,771
```



### 文件操作

```python
with open("a.txt", "r", encoding="utf-8") as f:
    print(f.read())
```

```python
finally:
    if f:
        f.close()
```

这个with语句和前面的try … finally结构是一样的，但是代码更佳简洁，并且不必调用`f.close()`方法。

### 内联条件

根据特定条件去执行任务时：如果只有一条执行语句，则可以在一行中完成，这样更容易阅读。

```Python
x =5
if x==5: print("x equals 5")  # 一行代码
for i in range(x+5): print(i) # 一行代码
```

**三元运算**

```Python
condition = True
x = 5 if condition else 10 # one line do it
```

### 列表推导式		

与其创建一个空列表，然后在列表中添加元素，不如使用**列表推导式**来创建一个空列表并同时将元素添加到列表中。

语法：`new_list = [expression for item in iterable (if conditional)]`

例如：求偶数的平方

```Python
squares = [i*i for i in range(10) if i%2==0]
print(squares) # [0, 4, 16, 36, 64]
```

### enumerate枚举迭代

数一数 Python 的最佳特性，那么枚举enumerate一定是名列前茅的。 它与循环相似，不同的是，同时提供循环对象和索引值。

语法：`for index,value in enumerate(iterable): print(index,value)`

通过一个例子来更好地理解它，我们需要将列表中所有值为偶数的位置替换为Even，将所有值为奇数的位置替换为Odd：

```python 
lst = [13,2,3,4,90]
for index,value in enumerate(lst):
    if value%2==0:
        lst[index] = "Even"
    else:
        lst[index] = "Odd"
print(lst) # ['Odd', 'Even', 'Odd', 'Even', 'Even']
```

```python 
lst = [13,2,3,4,90]
lst = ['Even' if value %2 == 0 else 'Odd' for index,value in enumerate(lst) ]
print(lst) # ['Odd', 'Even', 'Odd', 'Even', 'Even']
```

### 将列表中的所有元素作为参数传递给函数

我们可以使用 * 号，提取列表中所有的元素

```python3
def sum_of_elements(*arg):
    total = 0
    for i in arg:
        total += i

    return total

result = sum_of_elements(*[1, 2, 3, 4])
print(result)  # 10
```

### 合并字典

```python
first_dictionary = {'name': 'Fan', 'location': 'Guangzhou'}
second_dictionary = {'name': 'Fan', 'surname': 'Xiao', 'location': 'Guangdong, Guangzhou'}

result = first_dictionary | second_dictionary
```



```python
dictionary_one = {"a": 1, "b": 2}
dictionary_two = {"c": 3, "d": 4}

merged = {**dictionary_one, **dictionary_two}

print(merged)  # {'a': 1, 'b': 2, 'c': 3, 'd': 4}
```

### 计算函数运行的时间

```Python
# 计算函数运行的时间
def calc(func: callable) -> None: # callable表示能够被调用的函数
    start_time = time.time()  # 程序开始时间
    func() # function()   运行的程序
    end_time = time.time()  # 程序结束时间
    run_time = end_time - start_time  # 程序的运行时间，单位为秒
    print(run_time)
```
