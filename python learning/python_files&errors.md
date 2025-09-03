# 文件和异常

## 从文件中读取数据

### 读取整个文件

```python
with open('filename.txt') as file_object:
    contents = file_object.read()
   print(contents)
```

函数`open()`打开文件，`open()`接受一个参数：要打开的文件名
python在当前执行的文件所在的目录中查找指定的文件

关键词`with`在不再需要访问文件后将其关闭
也可以使用`open()`&`close()`来实现
但是如果程序存在bug导致`close()`未执行，文件将不会关闭，可能导致数据丢失或受损
使用`with`可以让python自己去关闭，更为妥当

使用方法`read()`来读取文件内容

相比于原始文件，该输出会在末尾多一个空行（`read()`到达文件末尾的时候会返回一个空字符串，显示出来就是一个空行，可以用`rstrip()`来解决）

**要让python打开不与程序文件位于同一个目录的文件，需要提供文件路径**

```python
with open('D:\pycharm\pyc\data\data_for_test.txt') as file_object:
    contents=file_object.read()
print(contents.rstrip())
```

以上代码是不合适的，\是转义符，会产生错误，有以下解决方案

- （推荐）在字符串前面加上r，即`with open(r'D:\pycharm\pyc\data\data_for_test.txt') as file_object:`
- 使用斜杠/
- 使用双反斜杠`\\`(每个反斜杠都进行转义)

### 逐行读取

要以每次一行的方式检查文件，可对文件对象使用for循环：

```python
filename='filename.txt'#常用处理方法
with open(filename) as file_object:
    for line in file_object:
        print(line)
```

结果导致每一行后面都会出现一个空行，处理方法即strip()

### 创建一个包含文件各行内容的列表

使用关键词`with`时，`open()`返回的文件对象只在`with`代码块内可用。如果要在with代码块外访问文件内容，可以使用一个列表存储文件内容。

```python
filename='filename.txt'

with open(filename) as file_object:
    lines=file_object.readlines()
    
for line in lines:
    print(line.strip())
```

`readline()`从文件中读取每一行，并将其存储在一个列表中，再赋值给lines

**注意**：读取文本文件时，Python将其中的所有文本都解读为字符串。如果读取的是数，并要将其作为数值使用，就必须使用函数int()将其转换为整数或者使用float()将其转化为浮点数。

## 写入文件

## 异常

## 存储数据