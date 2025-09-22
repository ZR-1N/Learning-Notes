# List

## list

list里面可以存放不同类型的数据
`lst=[]`，创建了一个空列表
`[]`，列表字面量用`[]`表示，里面的元素用逗号`,`分隔
用`print`直接输出整个列表的时候，也会输出两端的`[]`

列表和字符串都属于容器中的有序容器
在Python中有一些针对所有序列类型容器的操作，它们对于列表和字符串都是一样的

## 连接

- 加号可以用来连接两个序列容器成为一个新的打的容器
- 和数字的加法一样，对两个序列容器做加法，不会改变那两个容器本身

## 重复

- 一个序列容器乘以一个整数，可以形成一个新的重复若干遍的容器
- 同样这个运算也不会改变原本的容器内容

```python
lst=[2,3]
print(lst*3)
print([2,3]*3)
```

## 存在

- `in`和`not in`判断数据是否在序列中
- 当需要判断多个元素的组合时，字符串和列表有所不同

## 比较关系

- 判断是否相等或大小相同
- 比较是从左到右逐个元素（字符）比较的，一旦在某个位置上的元素已经可以比较出结果，就不再比较下去了
- 当两个序列不等长时，比较到多出来的那个位置为止，此时短的元素值以0计算
- 对于字符串的字符，是按照他们的Unicode编码进行比较

```python
# 情况1：前缀相同，长度决定
print('12' < '120')     # True  - 前缀'12'相同，短的<长的 ✓
print([1,2] < [1,2,0])  # True  - 同上 ✓ 无论多出的元素正负如何，只与长度有关

# 情况2：字典序在前缀就分出胜负
print('12' > '11')      # True  - 位置1：'2'>'1' ✓
print('12' < '13')      # True  - 位置1：'2'<'3' ✓
print([1,2] > [1,1])    # True  - 位置1：2>1 ✓

# 情况3：混合情况
print('120' < '13')     # True  - 位置1：'2'<'3'（不看后面） ✓
print([1,2,0] < [1,3])  # True  - 位置1：2<3（忽略后面的0） ✓
```

## 索引

下标不能越界，但是可以为负

## 序列的函数

- Python有一些全局的函数，用来对序列做一些基础运算
- `len`:给出容器中元素的个数
- `min()`和`max()`计算序列中最小和最大的元素
- `sum()`计算序列中元素之和

## range

`range`用来产生一个数列的迭代器

`list`可以把这个迭代器转成列表

```python
r = range(1,7)
print(r)
print(type(r))
t = list(r)
print(t)
print(list(range(10)))
print(list(range(10,0,-1)))
```

## 遍历数列

- 如果只是需要遍历`range()`产生的数列，并不需要转成列表
- 对一个容器使用`range()`产生其所有下标的数列再遍历是错误的

```python
x = 'Zhejiang'
for i in range(len(x)):
  print(x[i])
'''多余的索引计算：Python的 for item in container 底层是直接调用迭代器，效率最高
重复的内存访问：my_list[i] 需要每次都重新计算内存地址
额外的 len() 调用：虽然 len() 很快，但仍然是多余开销'''
'''优先直接迭代：这是最快、最安全的方式
需要索引时用 enumerate()：简洁且高效
避免 range(len())：除非有特殊需求'''
# 直接迭代元素
my_list = ['apple', 'banana', 'cherry']
for item in my_list:
    print(item)
#需要索引时用 enumerate()
my_list = ['apple', 'banana', 'cherry']
for i, item in enumerate(my_list):#enumerate同时获得索引和元素值
    print(f"Index {i}: {item}")
'''Index 0: apple
Index 1: banana
Index 2: cherry'''
```

## 输出列表上的值

```python
print(*prime, sep=',')
#*把列表打开传递给print()
```



# 在数列上的计算

## range

`range()`可以用来产生一个数列的迭代器
`list()`可以把这个迭代器转成列表、

`range(b,e,s)`

- b表示数列的起点
- e表示数列的终点，不包含在数列中
- 等差数列的步长，s<0是表示递减
- 只有一个参数的`range(n)`相当于`range(0,n)`



## 质数

## 蒙特卡洛模拟

蒙特卡洛方法，又称随机抽样或统计试验方法。
传统的经验方法由于不能逼近真实的物理过程，很难得到满意的结果
而蒙特卡洛方法由于能够真实地模拟实际物理过程，故解决问题和实际非常符合，可以得到很圆满的结果
这也是以概率和统计理论方法为基础的一种计算方法，是使用随机数（或更常见的伪随机数）来解决更多问题的方法

### 模拟步骤

- 构造或描述**概率过程**：正确描述和模拟这个概率过程。对于本来不是随机性质的确定性问题，比如计算定积分，就必须事先构造一个人为的概率过程，它的某些参量正好是所要求问题的解，即要**将不具有随机性质的问题转化为随机性质的问题**
- 实现**从已知概率分布抽样**：最简单、最基本、最重要的一个概率分布是(0,1)上的均匀分布（或称矩形分布）。随机数就是具有这种均匀分布的随机变量。随机数序列就是具有这种分布的总体的一个简单子样，也就是一个具有这种分布的相互独立的随机变数序列
- 建立**各种估计量**：确定一个随机变量，作为所要求的问题的解，我们称它为无偏估计。建立各种估计量，相当于对模拟实验的结果进行考察和登记，从中得到问题的解

### 估计Π

- 利用圆与其外接正方形面积之比为Π/4的关系，通过产生大量均匀分布的二维点，计算落在单位圆和单位正方形的数量之比再乘以4便得到Π的近似值，样本点也多，计算出的数据将会越接近真实的Π

### 随机数

- 产生随机数有多种不同的方法。这些方法被称为随机数发生器。随机数最重要的特征是：它所产生的后面的那个数与前面的那个数毫无关系
- 在实际应用中往往使用伪随机数就足够了。这些数列是“似乎”随机的数，实际上它们是通过一个固定的，可以重复的计算方法产生的。计算机或计算器产生的随机数具有很长的周期性。它们不真正的随机，因为它们实际上是可以计算出来的，但是它们具有类似于随机数的统计特征。这样的发生器叫做伪随机数发生器。

```python
# 随机函数库
import random
random.random()# 生成一个[0.0,1.0)区间的浮点随机数
random.uniform(a,b)# 拓展到自定义区间(a,b)
random.randint(a, b)# 生成一个[a,b]区间内的随机整数
random.randrange(a,b+1)# 开区间randrange(start=0,end,step)
random.choice(lst)# 从非空序列（如列表、元组、字符串）中随机选择一个元素
random.shuffle(lst)# in-place 随机打乱可变序列（如列表，mutable）的元素顺序
# 使用 Fisher-Yates 算法，确保均匀打乱。不返回新列表，而是修改原对象。如果需要新列表，可先复制：shuffled = lst[:]; random.shuffle(shuffled)。不支持不可变序列如元组。
random.seed(x)# 设置随机数生成器的种子，使后续随机调用可重现（相同种子产生相同序列）
# 无种子时，使用当前时间作为默认种子，导致不可重现。用于调试或测试：设置种子后，多次运行结果一致。种子后影响所有 random 函数。多次调用会覆盖前一个种子。
```

## 迭代法

### 牛顿迭代法

迭代法也称辗转法，是一种不断用变量的旧值递推新值的过程

- 牛顿迭代法（Newton’s method）又称为牛顿-拉夫逊（拉弗森）方法（Newton-Raphson method），它是牛顿在17世纪提出的一种**在实数域和复数域上近似求解方程**的方法。其最大优点是在方程的单根附近具有平方收敛，而且该法还可以用来求方程的重根、复根
- 为了找到函数f(x)=0的根，给定一个初始估计x0，画出函数f在x0处的切线，用切线来近似函数f，求出其与x轴的交点，作为f的根。但是由于f是弯曲的，该交点可能不是精确解，因此，这个步骤需要迭代进行
- 在每一步求出$x_i$以后，代入f可以知道$|f(x_i)-0|$，即与精确解的误差

### 牛顿迭代法求x的平方根

求解a的平方根，可以看作是求方程$x^2-a=0$的解

- 循环结束的条件不是输入的数据，而是计算的结果（误差）
- 浮点数不能直接用==比较相等，而应该用`abs(x-y)<eps(ε)`

### Bisection Method求方程根

二分法求解

```python
f = input()# 输入函数表达式，如 "2*x**2 + 3*x - 2"
left, right = map(float, input().split())# 输入区间端点，如 "-1 1"
x = left
fl = eval(f)
x = right
fr = eval(f)#eval()计算 2*x**2+3*x-2
while fl*fr<0 and abs(right-left)>1e-3:
  mid = (left+right)/2
  x = mid
  fm = eval(f)
  print(fm)
  if abs(fm) < 1e-3:
    print("root is "+str(mid))
    break
  x = left
  fl = eval(f)
  x = right
  fr = eval(f)
  if fm*fl <0:
    right = mid
  else:
    left = mid
else:
  print("no root")

```

# 使用列表的计算

## basic operations

### 创建空列表

`t=[]` ;`t=list()`

- `list()`可以将其他类型转换成列表，比如把字符串转换成列表`t=list('Hello')`

### 赋值

- 元素赋值可以通过下标修改容器中某个元素的值
- 但是，**字符串**是**不可修改**的数据类型，所以不能修改字符串中的某个字符（'str' object does not support item assignment）
- **列表变量是列表的管理者而非所有者，列表变量之间的赋值，是管理权的共享，而非列表内容的赋值**
- 用切片可以复制整个列表
- 列表的切片还可以被赋值。当列表的切片被赋值时，切片所表达的那部分元素被新的列表的全部元素所替换

```python
# 列表变量赋值
s1 = [1,2,3]
s2 = s1
s2[0] = 4
print(s1)
print(s2)## the same
# 复制列表
s1 = [1,2,3]
s2 = s1[:]
# 切片赋值
name = [1,2,3,4]
name[2:] = [5,6]
name
# 而且这种替换的元素数量还可以不一致
name = [1,2,3,4]
name[2:] = [5,6,7]
print(name)
name[2:] = [9,10]
print(name)
# 还可以给切片赋值空列表
```

### List function

- del: 删除列表的元素`del list[index]`
- append():在列表后面增加一个元素
- extend():把另一个列表的内容添加到列表的后面（append和extend添加一个列表的时候是不同的）
- insert():插入到指定位置之前（负数Index同理）（指定位置不存在时，加到最后）
- remove():删除第一次出现的那个元素
- pop():弹出指定位置的元素，不指定则弹出最后一个
- reverse():反转
- index():查找元素第一次出现的位置(**如果查找的元素在列表中不存在，则报错；in和index都可用来查找，表现（找不到时）和结果不同；字符串也有index函数，但是find函数只有字符串有，list无find函数**)
- copy():复制列表

```python
>>> s=[1,2,3,4]
>>> s.insert(3,2)
>>> s
[1, 2, 3, 2, 4]
>>> s.insert(100,5)
>>> s
[1, 2, 3, 2, 4, 5]
>>> s.insert(-2,6)//添加到-2之前，即-3
>>> s
[1, 2, 3, 2, 6, 4, 5]
>>> s.insert(-100,7)
>>> s
[7, 1, 2, 3, 2, 6, 4, 5]//插到反向的最后，即0
```

### 列表和字符串之间的操作

- 字符串`split()`：根据单个字符或字符串分隔字符串为列表，默认为空格
- 如果`split()`函数不带参数，就是以空格来分隔字符串

```python
date = '3/11//2018'
a = date.split('/')
print(a)
name = 'John Johnson'
a = name.split()
print(a)
#多个符号？
#列表会输出空元素['3', '11', '', '2018']，字符串无影响
```

- 字符串`join()`：用字符串做分隔符将列表中的元素组成一个字符串

```python
a = ['hello','good','boy','wii']
print(' '.join(a))
print(':'.join(a))
```

- 复制列表

```python
a=[1,2,3,4]
b=a
print(id(a),id(b))
b[2]=5
print(a)
c=a.copy()
print(id(a),id(c))
c[2]=6
print(a)
# id给出一个变量的编号（类似地址），区分列表变量的赋值和copy复制
```

- 判断列表是否为空

```python
t = []
print(t==[])
print(len(t)==0)
print(not t)#0、空表、空字符串被当作是False，其他值都是True
```



## 读入实验数据

### 常见的数据来源:

- 仪器产生的数据：文本或二进制形态，有说明格式
- 网络上采集到的数据：由采集程序产生
- 手工录入的程序：Excel表格中的数据、Word中数据

### 题目中常见的数据

- 一行数据，空格（或逗号）分隔
- 一行数据，已经是Python的表或元组的形态了
- 多行数据
  - 先告诉你有几个数据
  - 或用特殊数值/符号表示结束
  - 每行一个数据或每行若干个数据不一定

### 一行数据

- 直接`list(map(int, input().split()))`,不同的分隔符号在split中用不同的参数
- 如果明确知道有浮点数，int换成float

### 一行数据带列表格式

- 用`eval(input())`
- `eval()`会计算字符串中的表达式，产生计算结果，但是成本比int()和float()高
- 三种适合用`eval()`的情况：数据中带有十六进制或八进制的字面量形态、数据中带有四则运算、数据是列表或元组的格式

```python
# eval() 可以执行任意 Python 表达式
expression = "2 + 3 * 4"
result = eval(expression)
print(result)  # 输出: 14
# 低成本 - 直接类型转换
num1 = int("123")      # 只转换类型
num2 = float("3.14")   # 只转换类型
# 高成本 - 完整表达式解析
num3 = eval("int('123')")      # 解析字符串 → 创建 int 对象 → 转换
num4 = eval("float('3.14')")   # 解析字符串 → 创建 float 对象 → 转换

# 1.十六进制或八进制字面量
# 普通十进制 - 用 int() 即可
hex_str = "0x1A"    # 十六进制
oct_str = "0o52"    # 八进制
# ❌ 不适合用 eval() - 成本高
result1 = eval(hex_str)  # 0x1A = 26
result2 = eval(oct_str)  # 0o52 = 42
# ✅ 更高效的方式
result1 = int(hex_str, 16)
result2 = int(oct_str, 8)
print(f"十六进制: {result1}, 八进制: {result2}")
# 输出: 十六进制: 26, 八进制: 42

# 2.带有四则运算的表达式
# 简单计算 - 用 eval() 合适
calc_expressions = [
    "2 + 3 * 4 - 1",
    "(10 + 5) / 2",
    "100 * (1 - 0.2)"
]
for expr in calc_expressions:
    # ✅ 适合用 eval()
    result = eval(expr)
    print(f"{expr} = {result}")

# 3.列表或元组格式的数据
# 字符串形式的列表/元组
list_str = "[1, 2, 3, 4, 5]"#文件内容Python读取后是字符串格式
tuple_str = "(10, 20, 30, 'hello', True)"
# ❌ 不能用 int() 或 float()
# int(list_str)    # ValueError
# float(tuple_str) # ValueError
# ✅ 必须用 eval()
my_list = eval(list_str)
my_tuple = eval(tuple_str)
print(f"列表: {my_list}, 类型: {type(my_list)}")
print(f"元组: {my_tuple}, 类型: {type(my_tuple)}")

# 输出:
# 列表: [1, 2, 3, 4, 5], 类型: <class 'list'>
# 元组: (10, 20, 30, 'hello', True), 类型: <class 'tuple'>
```

```python
# 实际使用：配置文件
# 对比：不用 eval() 和用 eval()

# ❌ 不用 eval() - 写死的值，缺乏灵活性
HARD_CODED_CONFIG = {
    "items": [1, 2, 3, 4, 5],                    # 写死，无法动态修改
    "prices": {"apple": 1.2, "banana": 0.5},     # 写死，无法动态计算
    "total": 110.0,                              # 写死，无法动态计算
    "hex_color": 16738091,                       # 写死，不直观
}

# ✅ 用 eval() - 灵活、可读、动态
DYNAMIC_CONFIG = {
    # 可以轻松修改数字，保持格式
    "items": eval("[1, 2, 3, 4, 5, 6]"),        # 随时添加元素
    
    # 字典字面量，更直观
    "prices": eval("{'apple': 1.2, 'banana': 0.5, 'orange': 0.8}"),  # 易于扩展
    
    # 动态计算
    "total": eval("100 * 1.1"),                  # 税率变动时只需改这里
    
    # 十六进制颜色值更直观
    "hex_color": eval("0xFF5733"),              # 颜色设计师熟悉的格式
}

# 实际使用场景：读取外部配置
def load_config_from_file(config_file):
    """从外部文件加载配置"""
    with open(config_file, 'r') as f:
        config_str = f.read()
    
    # 假设文件内容是：
    # items = [1, 2, 3, 4, 5]
    # prices = {'apple': 1.2, 'banana': 0.5}
    # total = 100 * 1.1
    
    # 逐行解析并用 eval()
    local_vars = {}
    exec(config_str, local_vars)
    
    return local_vars

# 模拟配置文件内容
config_content = """
items = [1, 2, 3, 4, 5]
prices = {'apple': 1.2, 'banana': 0.5, 'orange': 0.8}
total = 100 * 1.1
hex_color = 0xFF5733
"""

# 写入临时文件
with open('temp_config.py', 'w') as f:
    f.write(config_content)

# 加载配置
loaded_config = load_config_from_file('temp_config.py')
print("从文件加载的配置：")
for key, value in loaded_config.items():
    print(f"{key}: {value}")
```

