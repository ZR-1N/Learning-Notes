# numpy

## 矩阵

- 在数学中，矩阵（Matrix）是一个**按照长方阵列排列的复数或实数集合** ，最早来自于方程组的系数及常数所构成的方阵。 
- 一个m×n的矩阵是一个由m行（row）n列（column）元素排列成的矩形阵列。矩阵里的元素可以是数字、符号或数学式

- 大小相同（行数列数都相同）的矩阵之间可以相互加减的，具体是对每个位置上的元素做加减法
- 矩阵的乘法则较为复杂。两个矩阵可以相乘，当且仅当**第一个矩阵的列数等于第二个矩阵的行数**。矩阵的乘法满足结合律和分配律，但不满足交换律
- 矩阵的一个重要用途是**解线性方程组**。线性方程组中未知量的系数可以排成一个矩阵，**加上常数项，则称为增广矩阵**。另一个重要用途是表示**线性变换**，即是诸如f(x)=4x之类的线性函数的推广
- 矩阵的**特征值和特征向量**可以揭示**线性变换的深层特性**
- 矩阵是高等代数学中的常见工具，也常见于统计分析等应用数学学科中
- 矩阵的运算是数值分析领域的重要问题。将**矩阵分解为简单矩阵的组合**可以在理论和实际应用上简化矩阵的运算

## Python的矩阵

- Python的NumPy,SciPy,SymPy和Pandas库都提供了对矩阵的支持
- 列表的元素可以是列表，因此可以构造出一个长的像矩阵的二维列表来表达矩阵

```python
m=[
[1,2,3],
[4,5,6],
[7,8,9]
]
m
```

## 输入矩阵

- 程序中除了矩阵字面量，更多的是需要从外部输入矩阵、保存计算的（中间）结果

- 通常矩阵的输入数据会给出**行数、列数**，然后按照行排列，**每一行的元素之间用空格分隔**

```python
m,n = map(int, input().split())#获取矩阵维度；map用来处理数据，转成int
mat= []
for _ in range(m):#不需要使用循环变量，所以用_
    mat.append(list(map(int, input().split())))
```

## 创建矩阵

- 程序中除了需要从外部输入矩阵，还需要保存中间结果
- 往往需要创建一个空的（零）矩阵，以方便之后保存计算的结果

```python
m, n = 3, 4 #3行4列的矩阵
mat = []
for row in range(m):
    mat.append([])
    for col in range(n):
        mat[row].append(0)
mat
# 用列表推导式表达
m, n = 3, 4
mat = [[0]*n for _ in range(m)]
mat
def matzero(m, n):
    return [[0]*n for _ in range(m)]#深拷贝，每次创建新行
# 产生随机数矩阵
import random
m, n = 3, 4
mat = [[random.randint(1,100) for _ in range(n)] for _ in range(m)]
mat
def matran(m, n):
    return [[random.randint(1,10) for _ in range(n)] for _ in range(m)]
# 产生连续数矩阵
m, n = 3, 4
mat = [[r*n+c for c in range(n)] for r in range(m)]
mat
```

## 矩阵的形状

- `len(mat)`给出矩阵的行数
- `len(mat[0])`给出矩阵的列数

## 遍历矩阵

```python
def matprt(mat):
    for row in range(len(mat)):
#         for col in range(len(mat[row])):
#             print(mat[row][col], \
#                   end=',' if col<len(mat[row])-1 else '')
        print(*mat[row], sep=',')#*拆为单独参数，sep默认为空格
#         print()
matprt(mat)
```

## 矩阵复制

- 二维矩阵表面上是列表的元素为另一个列表，实际上**列表的元素是另一个列表的管理者**，**而非列表本身**
- 采用切片复制列表时，复制的是管理关系；**Python中，变量是对象的引用而非对象本身**
- Python列表的`copy()`和切片复制是一样的
- 只有**逐个元素的复制**才能实现真的复制

## 矩阵的转置

一个m*n的矩阵A的转置是一个n\*m的矩阵，记为$A^T$，其中第i个行向量是原矩阵A的第i个列向量;
或者说，转置矩阵$A^T$第i行第j列的元素是原矩阵A第j行第i列的元素

$(\mathbf {A} ^{\mathrm {T} })_{i,j}=\mathbf {A} _{j,i}$

```python
# 法一：按行处理
matt=[]
for col in range(len(mat[0])):
    matt.append([])
    for row in range(len(mat)):
        matt[col].append(mat[row][col])
matprt(mat)
print()
matprt(matt)
#法二：逐元素copy
matt = matzero(len(mat[0]), len(mat))
for col in range(len(mat[0])):
    for row in range(len(mat)):
        matt[col][row]= mat[row][col]
matprt(mat)
print()
matprt(matt)
#法三：嵌套列表推导式
matt = [[mat[row][col] for row in range(len(mat))] for col in range(len(mat[0]))]
matprt(matt)
def mattrans(mat):
    return [[mat[row][col] for row in range(len(mat))] for col in range(len(mat[0]))]
```

## 矩阵加法

```python
A = matran(3,4)
B = matran(3,4)
matprt(A)
print()
matprt(B)
print()
mats = matzero(3,4)
for row in range(3):
    for col in range(4):
        mats[row][col] = A[row][col]+B[row][col]
matprt(mats)
#######################################################################################################
mats = [[A[row][col]+B[row][col] for col in range(len(A[0]))] for row in range(len(A))]
matprt(mats)
def matplus(A, B):
    return [[A[row][col]+B[row][col] for col in range(len(A[0]))] for row in range(len(A))]
```

## 矩阵的数乘

```python
c=2
matm = [[c*x for x in r] for r in mat]
matprt(mat)
print()
matprt(matm)
```

## 矩阵的对应元素乘

同加法&数乘

## 矩阵乘法

- 两个矩阵的乘法仅当第一个矩阵A的列数(column)和另一个矩阵B的行数(row)相等时才能定义
- 如A是m×n矩阵和B 是n×p的矩阵，它们的乘积AB是一个**m×p矩阵**

## 寻找鞍点

- 一个矩阵元素的“鞍点”是指**该位置上的元素值在该行上最大、在该列上最小**。
- 本题要求编写程序，求一个给定的n阶方阵的鞍点。