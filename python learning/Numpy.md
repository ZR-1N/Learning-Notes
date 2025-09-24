# Numpy

**Numpy包括**

- N维数组对象
- 精密广播功能函数
- 集成C/C++和Fortran代码的工具
- 线性代数、傅里叶变换、随机数功能

## 数组

- NumPy 的主要对象是 **N 维数组对象 ndarray**，它是一系列**同类型数据**的集合，以 0 下标为开始进行集合中元素的索引，ndarray 中的每个元素在内存中都有相同存储大小的区域
- NumPy中数组的**维度**称为**轴axis**
- 与列表最大的区别，在于NumPy数组以C语言数组的形式组织，并且其中的整数也以C语言的形式组织
- ndarray对象的内容可以通过**索引或切片**来访问和**修改**

**N维数组即张量Tensor，维度称为轴**

```python
import numpy as np#引入numpy

arr=np.array([1,2,3,4,5])
print(arr)
#array([1,2,3,4,5])
print(type(arr))
#numpy.ndarray
arr.dtype
#dtype('int64')
#array的成员变量dtype表示元素的数据类型
#整数默认是int64,但是可以指定为其他类型
arr2 = arr.astype(np.int32)
arr2.dtype
#dtype('int32')
arr3 = np.array([1,2,3,4], dtype=np.int32)
arr3
#array([1, 2, 3, 4], dtype=int32)
#如果初始化时字面量数组中的元素类型不一致，有整数也有浮点数，则取浮点数
arr3 = np.array([1,2.0,3,4])
arr3.dtype
#dtype('float64')
```

## arange()和linspace()

- `np.arange()`用来产生一个数组
- 与`range()`不同，`arange()`产生的是数组，而`range()`是一个迭代器
- `arange()`的区间和步长都可以是浮点数

```python
arr3 =np.arange(10)
arr3
#array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])(repr_notebook) or [0 1 2 3 4 5 6 7 8 9](str_vscode,repr())
a=np.arange(0.2,1.3,0.1)
```

- `np.linspace()`则给出的是区间内数据的个数，而不是步长，以此对区间做等分（浮点数类型，故整数x会显示为x.）
- 另外，它的区间是左右闭区间。除非使用参数`endpoint=False`

## 其他产生数组的方法

- 通常，数组的元素最初是未知的，但它的大小是已知的。因此，NumPy提供了几个函数来创建具有初始占位符内容的数组。这就减少了数组增长的必要，因为数组增长的操作花费很大。
- `zeros()`、`ones()`产生0或1填充的数组**注意参数！**ones(shape)，故ones((3,4))/ones((3,))(tuple)
- 默认情况下，这样创建的数组的dtype是`float64`类型的
- `eye()`产生单位矩阵
- `zeros_like`用一个数组产生新数组，使用旧数组的尺寸和类型，但是其中的元素值都是0
- `ones_like`类似，其中元素值都为1
- `empty()`产生一个空的数组，其中的元素值都是任意的,然后可以用`fill()`填充

```python
np.zeros((3,4))
np.zeros(shape=[3,4])
np.ones(shape=[3,4], dtype=float)
k = np.eye(4)
np.zeros_like(k)
np.ones_like(k)
a=np.empty(10)
a.fill(10)
```

## 随机数组

- numpy的random模块可以生成满足各种分布的随机数序列

```python
# [0,1)
np.random.random_sample(5)
# [0,5)
5*np.random.random_sample(10)
# [0,10)
np.random.randint(0,10,size=10)
# 正态分布(标准正态分布：均值loc默认=0，标准差scale默认=1)
np.random.normal(loc=0.0, scale=1.0, size=None)
# N(3,0.1) N(均值, 标准差)
np.random.normal(3,0.1,size=10)
# seed
np.random.seed(2022)
np.random.random_sample(2)
# 这些函数功能相同
print(np.random.random_sample(2))
print(np.random.random(2))
print(np.random.rand(2))
```

## `ndarray`的属性

- **ndarray.ndim** - 数组的轴（维度）的个数。在Python中，维度的数量被称为**rank**。
- **ndarray.shape** - 数组的**形状**（维度）。这是一个整数的元组，表示每个维度中数组的大小。对于有 *n* 行和 *m* 列的矩阵，`shape` 将是 `(n,m)`。因此，`shape` 元组的长度就是rank或维度的个数 `ndim`。
- **ndarray.size** - 数组**元素的总数**。这等于 `shape` 的元素的乘积。
- **ndarray.dtype** - 一个描述数组中**元素类型**的对象。可以使用标准的Python类型创建或指定dtype。另外NumPy提供它自己的类型。例如numpy.int32、numpy.int16和numpy.float64。
- **ndarray.itemsize** - 数组中**每个元素的字节大小**。例如，元素为 `float64` 类型的数组的 `itemsize` 为8（=64/8），而 `complex32` 类型的数组的 `itemsize` 为4（=32/8）。它等于 `ndarray.dtype.itemsize` 。
- **ndarray.data** - 该缓冲区包含数组的实际元素。通常，我们不需要使用此属性，因为我们将使用**索引访问数组中的元素**。
- 3D空间中的点的坐标`[1, 2, 1]`具有一个轴。该轴有3个元素，所以我们说它的长度为3
- 在下面的例子中，数组有2个轴。第一轴的长度为2，第二轴的长度为3

```python
[[ 1., 0., 0.],
 [ 0., 1., 2.]]#2*3
```

关于1D，**数据本身是标量序列**

- [1, 2, 1] 只是 **3个数值的序列**
- 没有任何"嵌套"结构，所以是 1D
- **轴数 = 嵌套层数 + 1**

所以其只有一个轴，长度为3

```python
a = np.array([[ 0, 1, 2, 3],
           [10,11,12,13]])
a.ndim
a = np.zeros((3,4))
a.shape#shape是数组的属性，表示其张量的形状（在每一维上的尺寸
a.ndim
a.dtype.name
a.itemsize
a.size
```

## 改变形状

- `reshape()`可以改变数组的形状，数组实际存储为**线性数据**，所以改变形状就是**改变下标组织方式**（总量不变）
- `ravel()`形成**扁平化**的数组
- `T`**转置**数组
- 这三个函数都是**生成新的数组**，而不会修改原来的数组

```python
a = np.arange(120).reshape(2,3,4,5)
print(a.shape)
a.ravel()  # returns the array, flattened(linear)展平数组
a.T# returns the array, transposed
```

## 数组的运算

- 两个形状相同（维数和每一维的大小相同）的数组可以做对应位置元素的算法运算
- 如`+、-、*、/、%`

```python
a = np.array( [20,30,40,50] )
b = np.arange( 4 )
c = a-b
```

## 复合运算

- 某些操作（例如`+=`和`*=*`）会更直接更改被操作的矩阵数组而不会创建新矩阵数组

```python
a = np.ones((2,3), dtype=int)
a *= 3
b = np.random.random((2,3))
b += a
a += b                # b is not automatically converted to integer type
#可以考虑
a +=b.astype('int32')
```

## 自动类型转换

- 当使用不同类型的数组进行操作时，结果数组的类型对于更一般或更精确的数组（称之为向上转换的行为）

int32->float64->complex128

```python
arr=np.exp(c*1j)#复指数函数

import numpy as np

# 欧拉公式：e^(iθ) = cos(θ) + i*sin(θ)
# 其中 1j 是 NumPy 中的虚数单位（√-1）

# 示例：θ = π/4 (45度)
theta = np.pi / 4
result = np.exp(1j * theta)
print(f"e^(iπ/4) = {result}")
print(f"验证: cos(π/4) + i*sin(π/4) = {np.cos(theta)} + {np.sin(theta)}i")

# 输出：
# e^(iπ/4) = (0.70710678+0.70710678j)
# 验证: cos(π/4) + i*sin(π/4) = 0.7071067811865475 + 0.7071067811865475i

# 假设 c 是一个角度数组（弧度）
c = np.array([1.15, 2.3, 0.3, 3.14])  # 示例角度值

print("角度数组 c:", c)
print("c * 1j:", c * 1j)
d = np.exp(c * 1j)
print("复指数结果 d:", d)
'''角度数组 c: [1.15 2.3  0.3  3.14]
	c * 1j: [1.15 +0.j 2.3  +0.j 0.3  +0.j 3.14 +0.j]
	复指数结果 d: [ 0.40808206+0.91294525j -0.74805753-0.66363388j
  	0.95507364+0.29636858j -0.99233547+0.12357312j]'''
```

## 数组和标量计算

```python
a = np.linspace(1,10,20)
a=a+1
a=a*5
a<35#返回[ True,  True,  True,  True,  True,  True,  True,  True,  True,True,  True, False, False, False, False, False, False, False,False, False]
```

可以理解为用`1`产生了一个和`a`等长的数组，其中的元素都是1，然后两个数组对应的元素相加，结果还是一个数组，乘法同理

- 可以利用这个性质，配合`ones()`来产生任意数的数组

## 通函数

- Numpy提供熟悉的数学函数，例如sin,cos,和exp。在Numpy中，这些被称为“通函数”(`ufunc`)。在Numpy中，这些函数在数组上按元素进行运算，产生一个数组作为输出

```python
B=np.arange(3)
np.exp(B)
np.sqrt(B)
np.log(price)#默认为ln
```



## 约减

- reduce:约减，将多个数据按照某种规则合并成一个或几个数据
- 如数组的累加操作
- 类似的，mean,max,min都是约减操作。如果不指定约减的方向，它们都是约减为一个数
- `sum()`、`min()`、`max()`、`median()`、`mean()`、`average()`、`std()`和`var()`函数可以求和、最小值、最大值、中位数、平均数、加权平均数、标准差和方差

- 既有np的这些函数，也有数组的这些函数，**功能相同，用法不同**

```python
# 推荐：根据场景选择
import numpy as np

arr = np.random.rand(3, 4)

# 1. 简单计算：任选其一
print("简单求和:", np.sum(arr))  # 或 arr.sum()

# 2. 链式操作：用数组方法
result = (arr.T  # 转置
          .sum()  # 总和
          .round(2))  # 保留2位小数
print("链式结果:", result)

# 3. 复杂统计：用 NumPy 函数
medians = np.median(arr, axis=0, keepdims=True)
print("中位数（保持维度）:", medians.shape)

# 4. 与其他操作结合：用数组方法
normalized = ((arr - arr.mean(axis=0, keepdims=True)) /  # 中心化
              arr.std(axis=0, keepdims=True))  # 标准化
print("标准化后形状:", normalized.shape)
```

## Numpy中的“轴”方向

- mean,max,min都是约减操作，如果不指定约减的方向，它们都是约减为一个数。指定axis后，就会按照指定的轴来做约减。`axis=0`表示第一个维度

- 轴也可以是个向量，`[0,1]`表示先在列上垂直约减，再在行上水平约减

- 另一种理解方式是括号层次：

  `[[[1,1,1],[2,2,2]],[[3,3,3],[4,4,4]]]`有三层括号，其维度由外而内依次为[0,1,2]。

  axis等于几，就是把那层括号去掉

```python
a = np.ones((2,3))
b.cumsum(axis=1)# cumulative sum along each row
```

## 数组的切片

- 和list的切片一样
- 列表的切片做右值时是复制
- 而数组的切片是引用
- Numpy的数组切片做右值时也是引用，是为了提高效率，尽量避免数据拷贝操作

## 多维的下标和切片

- 和列表不同，多维数组的下标形式更像数学的形式
- 对于二维数组，可以传入两个数字来索引
- 只给出一个维度索引，表达的是一行
- 多维也支持切片，每一维都支持切片的规则，包括负值的索引
- 当提供的索引少于轴的数量时，缺失的索引被认为是完整的切片`:`

- `a[i]` 方括号中的表达式 `i` 被视为后面紧跟着多个 `:` ，用于表示剩余轴。NumPy也允许你使用三个点写为 `b[i,...]`
- 三个点（ `...` ）表示产生完整索引元组所需的冒号。例如，如果 `x` 是rank为5的数组（即，它具有5个轴），则：
- `x[1,2,...]` 相当于 `x[1,2,:,:,:]`，
- `x[...,3]` 等效于 `x[:,:,:,:,3]`
- `x[4,...,5,:]` 等效于 `x[4,:,:,5,:]`。

## 遍历

- 对多维数组进行遍历（**lterating**）是相对于第一个轴完成的：

```python
def f(x,y):
     return 10*x+y
b = np.fromfunction(f,(5,4),dtype=int)
for row in b:
     print(row)
```

## 在扁平化数组上遍历

- 想要对数组中的每个元素执行操作，可以使用`flat`属性，该属性是数组的所有元素的迭代器

```python
for element in b.flat:
	print(element,end=' ')
```

## 乘法

- 对应元素乘

`np.multiply(a,b)`和`a*b`一样，都是对应位置元素的乘法，并不是矩阵乘法

```python
A = np.array([[1,1],
             [0,1]])
B=np.array([[2,0],
           [3,4]])
A*B  # elementwise product
```

- 点乘
- `np.dot(a,b)`，或写做`a@b`则是对a和b做矩阵点乘，要求a的列数和b的行数相同

```python
A @ B # matrix product
A.dot(B)
```

## 矩阵matrix

- 使用`np.matrix()`将多维列表转换成matrix类型，或使用`np.mat()`生成矩阵，则所使用的矩阵做$*$乘法就是对应元素乘（*,@,.dot()效果一致）

- 对一个N阶方阵A，如果存在另一个n阶方阵B，它们满足：AB=BA=E（E为单位矩阵），那么两矩阵互为逆矩阵，其中`A.I`表示A的逆矩阵
- `A.T`表示转置矩阵

## 堆叠和拆分

### 将不同数组堆叠在一起

- 几个数组可以沿不同的轴堆叠在一起，例如：

```python
a = np.floor(10*np.random.random((2,2)))
b = np.floor(10*np.random.random((2,2)))
np.vstack((a,b))
np.hstack((a,b))
```

- 该函数将`column_stack`1D数组作为列堆叠到2D数组中，它仅相当于`hstack`2D数组

  ```python
  from numpy import newaxis
  np.column_stack((a,b))     # with 2D arrays
  a = np.array([4.,2.])
  b = np.array([3.,8.])
  np.column_stack((a,b))     # returns a 2D array
  np.hstack((a,b))           # the result is different
  a[:,newaxis]               # this allows to have a 2D columns vector
  np.column_stack((a[:,newaxis],b[:,newaxis]))
  np.hstack((a[:,newaxis],b[:,newaxis]))   # the result is the same
  ```

- 另一方面，该函数`ma.row_stack`等效`vstack`于任何输入数组。通常，对于具有两个以上维度的数组，`hstack`沿其第二轴`vstack`堆叠，沿其第一轴堆叠，并`concatenate`允许可选参数给出连接应发生的轴的编号

### 将一个数组拆分成几个较小的数组

- 使用`hsplit`，可以沿数组的水平轴拆分数组，方法是指定要返回的形状相等的数组的数量，或者指定应该在其之后进行分割的列

```python
a = np.floor(10*np.random.random((2,12)))
np.hsplit(a,3)   # Split a into 3
np.hsplit(a,(3,4))   # Split a after the third and the fourth column
`vsplit`沿垂直轴分割，并`array_split`允许指定要分割的轴
```

## 拷贝和视图

当计数和操作数组时，有时会将**数据复制到新数组**中，有时不会，共三种情况

### 简单分配不会复制数组对象或其数据

```python
a = np.arange(12)
b=a # no new object is created
print(b is a) #True a and b are two names for the same ndarray object
b.shape=3,4 # change the shape of a
#Python将可变对象作为引用传递，因此函数调用不会复制，可以用id check(id is a unique identifier of an object)
#使用 == 而非 is：id(a) == f(a) 总是 True，这才是验证引用传递的有效方式。
def f(x):
     return (id(x))                          
id(a) == f(a)#True
id(a) is f(a)#False
```

### 视图/浅拷贝

- 不同的数组对象可以**共享相同的数据**。该`view`方法创建一个查看相同数据的新数组对象

```python
c = a.view()
c is a
c.base is a # c is a view of the data owned by a
c.flags.owndata
c.shape = 2,6# a's shape doesn't change
a.shape#(3,4)
c[0,4] =1234# a's data changes
s = a[ : , 1:3]     # spaces added for clarity; could also be written "s = a[:,1:3]"
s[:] = 10           # s[:] is a view of s. Note the difference between s=10 and s[:]=10
```

- 切片数组会返回一个视图
- **深拷贝**：`copy`方法生成数组及其数据的完整副本
- 有时，如果不再需要原始数组，则应**在切片后调用`copy`**。例如，假设中间结果a很大，最终结果b只包含a的一小部分，那么**在切片构造b时应该做一个深拷贝**

```python
d=a.copy()# a new array object with new data is created
d is a
d.base is a # d doesn't share anything with a
d[0,0]=9999
a= np.arange(int(1e8))
b= a[:100].copy()
del a # the memory of a can be released
```

- 如果改为使用`b=a[:100]`，则`a`由`b`引用，并且即使执行`del a`也会在内存中持久存在

## 花式切片

- 切片只能支持连续或者等间隔的切片操作，要想实现**任意操作**的操作，需要使用**花式索引**

### 使用索引数组进行索引

- 当索引数组`a`时多维的，单个索引数组指的是第一个维度`a`。
- 还可以**为多个维度提供索引**。每个维度的索引数组**必须具有相同的形状**

- 也可以**按顺序（比如元组）**放入i,j，然后使用**元组**进行索引（不能用列表）
- 但是不能通过放入i,j放入数组来实现这一点，因为这个**数组将被解释为索引a的第一个维度**

```python
a=np.arange(12)**2 # the first 12 square numbers
i=np.array([1,1,3,8,5])# an array of indices
a[i]# the elements of a at the position i
j=np.array([[3,4],[9,7]])# a bidimensional array
a[j]# the same shape as j

# 通过使用调色板将标签图像转换为彩色图像来显示此行为
palette = np.array([0,0,0],#black
                  [255,0,0],#red
                  [0,255,0],#green
                  [0,255,255],#blue
                  [255,255,255])#white
image =np.array([[0,1,2,0],#each value corresponds to a color in the palette
                [0,3,4,0]])
palette[image]# the(2,4,3) color image

a=np.arange(12).reshape(3,4)
i = np.array( [ [0,1],                        # indices for the first dim of a
                [1,2] ] )
j = np.array( [ [2,1],                        # indices for the second dim
                [3,3] ] )
a[i,j] #i and j must have equal shape
#array([[2,5],[7,11]])
a[i,2]
a[:j]#i.e.,a[ : , j]
l=(i,j)
a[l]#equivalent to a[i,j]
#s= np.array([i,j])
#a[s]# not what we want
a[tuple(s)]#same as a [i,j]
```

- 使用数组索引的另一个常见用法是搜索与时间相关的系列的最大值

```python
time = np.linspace(20,145,5)#time scale
data = np.sin(np.arange(20).reshape(5,4))# 4 time-dependent series
ind = data.argmax(axis=0)# index of the maxima for each series
#
time_max = time[ind]# times corresponding to the maxima
data_max = data[ind,range(data.shape[1])]# =>data[ind[0],0],data[ind[1],1]...
np.all(data_max == data.max(axis=0))
```

- 还可以使用数组索引作为分配给的目标
- 但是，当索引列表包含重复时，分配会多次完成，留下最后一个值

```python
a = np.arange(5)
a[[1,3,4]]=0
np.array([0,0,2,0,0])
a= np.arange(5)
a[[0,0,2]]=[1,2,3]
#这是合理的，但请注意是否要使用Python的+=构造，因为它可能不会按预期执行
a = np.arange(5)
a[[0,0,2]]+=1
#即使0在索引列表中出现两次，第0个元素也只增加一次，因为是选中副本，而不是直接引用a的视图
# 可以使用Numpy的np.add.at函数，它专门用于处理重复索引的累加操作
np.add.at(a, [0, 0, 2], 1)
```

## 使用布尔数组进行索引

- 当使用（整数）索引数组时，提供了要选择的索引列表
- 使用布尔索引需要明确地选择想要的数组中的哪些项目以及哪些不需要的项目
- 可以使用与原始数组具有相同形状的布尔数组

```python
a=np.arange(12).reshape(3,4)
b=a>4
b# b is a boolean with a's shape
array([[False, False, False, False],
       [False,  True,  True,  True],
       [ True,  True,  True,  True]])
a[b] # 1d array with the selected elements
#此属性在分配中非常有用
a[b]=0
```

- 实例如下：使用布尔索引生成Mandelbrot集的图像

```python
import numpy as np
import matplotlib.pyplot as plt
def mandelbrot( h,w, maxit=20 ):
    """Returns an image of the Mandelbrot fractal of size (h,w)."""
    y,x = np.ogrid[ -1.4:1.4:h*1j, -2:0.8:w*1j ]
    c = x+y*1j
    z = c
    divtime = maxit + np.zeros(z.shape, dtype=int)

    for i in range(maxit):
        z = z**2 + c
        diverge = z*np.conj(z) > 2**2            # who is diverging
        div_now = diverge & (divtime==maxit)  # who is diverging now
        divtime[div_now] = i                  # note when
        z[diverge] = 2                        # avoid diverging too much

    return divtime
plt.imshow(mandelbrot(400,400))
plt.show()
```

- 使用布尔索引的第二种方法更类似整数索引；对于数组的每个维度，给出一个1D布尔数组，**选择想要的切片**

```python
a=np.arange(12).reshape(3,4)
b1 = np.array([False,True,True])             # first dim selection
b2 = np.array([True,False,True,False])       # second dim selection
a[b1,:]                                   # selecting rows
a[:,b2]# selecting columns
a[b1,b2]# 逐元素配对(True和True的索引配对)，而非笛卡尔积，结果为4,10
```

- 注意1D布尔数组的长度必须要与要切片的尺寸（或轴）的长度一致

## 爱因斯坦求和

- Einstein Notation/Summation Convention
- 点乘公式$s=∑iv_iw_i$，表示两个向量对应的元素相乘后求和。爱因斯坦的写法是$s=v_iw^i$
  - 这里，$v_i$表示行向量中的元素，而$w^i$表示列向量中的元素。**下标表示列，上标表示行**。（行向量中的元素的行相同，列不同）
- 类似的，矩阵A中第m行，第n列的元素，以前标记为$A_{mn}$，现在标记为$A_n^m$。

- 内积inner product： $u⋅v=u_jv^j$
- 向量乘以矩阵matrix-vector multiplication： 矩阵A和向量v的乘积向量u可表示为$u_i=A^i_jv^j$。
- 矩阵乘法matrix multiplication： 两个矩阵$A_{i×j}$和$B_{j×k}$，二者的乘法可表示为$C^i_k$=$A^i_jB^j_k$。
- 矩阵的迹trace： 对角线元素之和。一个矩阵A，如果其上标和下标相同，则它的迹t为t=$A_i^i$
- 外积outer product： M维向量a和N维余向量b的外积是一个M×N的矩阵A，A=ab，则可以表示为$A^i_j=a^ib_j$

## `einsum()`

`result = einsum('ijk, ijl->kl', a, b)`

- 这里a是一个三维矩阵，因为格式字符串里有`ijk`三个字符，而b也是三维矩阵。结果是二维矩阵，因为只有`kl`两个字符

```python
#例如：向量求和
a = np.arange(10)
s = np.einsum('i->',a)
s#45
#i->表示一个一维张量转换为标量，方法是求和
#例如：按行求和
a = np.arange(20).reshape(4,5)
scol = np.einsum('ij->j', a)
scol#array([30, 34, 38, 42, 46])
# ij-j表示i行j列的矩阵变成了j列的向量，所以是按列求和
#例如：按行求和
np.einsum('ab->a',a)
# 三维张量到二维的约减求和
a = np.arange(60).reshape(3,4,5)
np.einsum('ijk->jk', a)
#相当于
a.sum(axis=0)
#dot乘
a = np.arange(20).reshape(4,5)
b = np.arange(20).reshape(5,4)
np.einsum('ij,jk->ik', a, b)
#a是i行j列的，b是j行k列的，结果是i行k列的，能形成这样的运算，只有dot乘
#元素乘
np.einsum('ij,ij->ij', a, b.T)
#内积：
np.einsum('ij,ij->', a, b.T)
#取对角线
a = np.arange(16).reshape(4,4)
np.einsum('ii->i', a)  # 相当于`np.diag(a)`
#求迹trace
np.einsum('ii->', a)
#转置
np.einsum('ij->ji', a)
```
