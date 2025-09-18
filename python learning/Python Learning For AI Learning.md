# Python Learning For AI Learning

## 除法&取模

/表示普通除法

//表示整数除法（**向下取模**）

取余%

**余数的符号与除数符号一致，与被除符号无关**

## 赋值

python支持**连续赋值**和**同步赋值**

```python
a=b=6
a, b=4, 5
```

交换变量的值可以选择：1.使用中间变量来过渡；2.使用同步赋值`a,b=b,a`

## 输入

```python
a,b=map(int,input().split())
```

在算法竞赛中，`a, b = map(int, input().split())` 是标准写法
`split()`:这是字符串方法，默认以空格为分隔符，将字符串拆分成一个列表
`map(function, iterable)` 是一个 Python 内置函数，将 function 应用到 iterable 的每个元素。

等价写法（更verbose的写法）

```python
line = input()          # 读取 "48 18"
numbers = line.split()  # 分割成 ["48", "18"]
a = int(numbers[0])     # 转换为整数 48
b = int(numbers[1])     # 转换为整数 18
```

## 链式比较

形如`a op1 b op2 c`,这里`op1`、`op2`是做比较的关系运算符，Python的计算规则是：`a op1 b and b op2 c`

