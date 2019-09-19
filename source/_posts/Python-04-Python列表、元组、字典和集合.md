---
title: '[Python-04]Python列表、元组、字典和集合'
copyright: true
date: 2019-09-08 15:00:28
categories: Python
tags:
 - Python系列
---

# 什么是序列，Python序列详解（包括序列类型和常用操作）

序列：一块可存放多个且连续的内存空间，并且这些值有顺序，可通过索引进行访问。

常见的序列有: 字符串、列表、元组、集合和字典。

这些常见的序列，除了集合和字典不支持索引、切片、相加和相乘的操作，其余的都可以。

## 序列索引

索引就是一个序列中每个元素的编号。第一个元素的索引是0，也就是说想要访问某个序列的第一个元素，那么它的索引就是0，而想要访问最后一个元素，那么它的索引就是序列长度-1

索引还分为正索引值和负索引值，它们的区别仅仅在于访问方式不同。

__注意:__

- 正索引值的起始位置是0，结束位置是序列长度-1

- 负索引值的起始位置是-1,结束位置是-(序列长度-1)

例子:

__根据索引访问序列元素__

~~~
str = "Hello World"
print("str 的第一个字符是:%s，最后一个字符是:%s" % (str[0],str[len(str)-1]))
~~~

输出结果:
~~~
str 的第一个字符是:H，最后一个字符是:d
~~~

## 序列切片

刚刚我们通过索引值进行访问序列元素，那么序列切片也是可以做到的，它可以访问某个范围内的元素。

语法格式:

~~~
sname[start:end:step]
~~~

- sname:序列名称

- start:切片开始的索引位置（包括该位置），此参数可不指定，默认为0。

- end:切片结束的索引位置(不包括该位置)，此参数可不指定，默认为序列的长度。

- step:切片的范围，也就是每次取元素时，要隔多少个位置，此参数可不指定，可以直接忽略最后一个冒号

例子:

~~~
str = "Hello World"
# 获取整个字符串
print(str[:])
# 从索引4开始，一直到最后一个，没隔2个字符取一次。
print(str[4::2])
~~~

输出结果:

~~~
Hello World
oWrd
~~~

## 序列相加

序列可以使用`+`运算符，进行相加操作，他会将两个序列进行连接，但不会去除重复的元素。

例子:

~~~
str1="hello"
str2="world"
print(str1+str2)
~~~

输出结果:`helloworld`

## 序列相乘

使用`*`运算符，可以将序列的元素进行重复。

例子:

~~~
str1 = "hello\t"
print(str1*3)
~~~

输出结果:`hello	hello	hello	`

__tips:可以使用序列相乘，创建指定长度空列表__

~~~
test_list = [None]*5
~~~

## 检查元素是否包含在序列中

使用`in`关键字可以检查序列中的元素是否存在。 

语法格式:

~~~
value in sequence
~~~

例子:

~~~
str1="Hello"
print('o' in str1)
~~~

输出结果: `True`

__tips:使用`not in`关键字可以检查是否不存在__


## 和序列相关的内置函数

Python提供了几个用于操作序列的内置函数，可以很方便的操作序列。

![Python序列内置函数](Python-04-Python列表、元组、字典和集合/Python序列内置函数.png)

# Python list列表详解

Python提供了一种数据结构————`list`(列表)

__列表可以存储多个不同数据类型的元素。__


## Python创建列表

创建列表分为两种方式

使用`=`运算符创建列表

语法格式:

~~~
listname = [element1,element2...elementn]
~~~

listname: 列表的名称

element1: 列表的元素

例子:

~~~
# 创建一个列表
test_list1 =["one",1,True,1.0]
print(test_list1)
# 创建一个空列表
empty_list = []
print(empty_list)
~~~

输出结果:

~~~
['one', 1, True, 1.0]
[]
~~~

__使用list()函数创建列表__

例子:

~~~
str1="HelloWorld"
test_list = list(str1)
print(test_list)
~~~

输出结果:

~~~
['H', 'e', 'l', 'l', 'o', 'W', 'o', 'r', 'l', 'd']
~~~

## 访问列表元素

两种方式：通过索引访问和通过切片访问。

## 删除列表

不常用，因为Python具有垃圾回收机制，有些不需要再使用的列表将会自动回收。

使用`del`关键字进行删除

语法格式:

~~~
del listname
~~~

__注意:删除后的列表不能再次使用__

# Python list列表添加元素的3种方法

## Python append()方法添加元素

`append()`方法在列表的末尾追加元素。

语法格式:

~~~
listname.append(obj)
~~~

listname代表要添加元素的列表;obj代表要添加到列表末尾的数据。

obj可以是单个元素，也可以是其他序列。

例子:

~~~
# 追加单个元素
list1 = [0,1,2,3]
print(list1)
list1.append(4)
print(list1)
# 追加一个列表
list2 = [5,6,7,8]
print(list2)
list1.append(list2)
print(list1)
~~~

输出结果:
~~~
[0, 1, 2, 3]
[0, 1, 2, 3, 4]
[5, 6, 7, 8]
[0, 1, 2, 3, 4, [5, 6, 7, 8]]
~~~

__注意:使用append()函数时，如果是传递的单个数据，将会直接追加到列表后，但是如果传入的是个列表（序列），那么则会追加一个列表形式的元素。__

想要访问刚刚追加的列表元素的其中一个可以这样：

~~~
list1 = [0,1,2]
list2 = [3,4,5]
list1.append(list2)
print(list1[3][2])
~~~

输出结果:`5`

## Python extend()方法添加元素

刚刚我们使用`append()`函数追加了个列表元素，但是并没有像添加单个字符一样作为一个整体添加，而我们只需要使用`extend()`方法就可以将列表以整体的方式添加进去。

例子:

~~~
list1 = [0,1,2,3]
print(list1)
list2 = [5,6,7,8]
print(list2)
list1.extend(list2)
print(list1)
~~~

输出结果:

~~~
[0, 1, 2, 3]
[5, 6, 7, 8]
[0, 1, 2, 3, 5, 6, 7, 8]
~~~

## Python insert()方法插入元素

需要指定插入列表元素的位置时，可以使用insert()方法。

语法格式:

~~~
listname.insert(index,obj)
~~~

index: 将obj插入到listname列表的索引

例子:

~~~
test_list1 = list(range(1,11))
print(test_list1)

print(len(test_list1))
test_list1.insert(len(test_list1),11)
print(test_list1)
~~~

输出结果:

~~~
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
10
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
~~~

## Pyhton list列表删除元素(3种方法)

> del删除

__del语句在Python中可以删除变量、列表的元素__

~~~
test_list = list(range(1, 11))
print(test_list)
del test_list[1::2]
print(test_list)
~~~

输出结果:

~~~
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
[1, 3, 5, 7, 9]
~~~

> 根据元素值进行删除

可以使用remove()方法来删除列表元素。

删除第一个被查找到的元素。

__注意:remove()方法不是根据索引来删除元素的，而是查找元素本身，再进行删除，所以如果找不到元素，则会报错__

例子:

~~~
test_list = ['test', 30, 'test2', 10, 30]
test_list.remove('test')
test_list.remove(30)
print(test_list)
~~~

输出结果:

~~~
['test2', 10, 30]
~~~

> 删除列表所有元素

使用clear()方法可以删除列表的所有元素。

例子:

~~~
test_list = ['test', 30, 'test2', 10, 30]
test_list.clear()
print(test_list)
~~~

输出结果:

~~~
[]
~~~

# Python list列表修改元素

修改列表元素，可以通过列表索引获取元素进行赋值。

~~~
testlist = list(range(1,10))
print(testlist)
testlist[len(testlist)-1] = 100
print(testlist)
~~~

输出结果:

~~~
[1, 2, 3, 4, 5, 6, 7, 8, 9]
[1, 2, 3, 4, 5, 6, 7, 8, 100]

[Process exited 0]
~~~

使用slice语法对列表部分进行赋值。

slice语法，不要求新赋值的元素个数与原来的元素个数相等。也就是说使用slice剩余法既可以为列表增加元素，也可以为列表删除元素。

~~~
b_list = list(range(1,5))
print(b_list)

b_list[1:3] = ['a','b']
print(b_list)
~~~

输出结果:

~~~
[1, 2, 3, 4]
[1, 'a', 'b', 4]
~~~

# Pyhthon list常用方法(count、index、pop、reverse和sort)快速攻略

Pyhton为list提供了一些常用的方法。

我们使用dir(list)方法可以看到列表包含的所有方法。

```
print(dir(list))
```

输出结果:

```
  6 _', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__',
  5 '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setat
  4 tr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'c
  3 lear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse'
  2 , 'sort']
```

**注意，方法名包含双下划线的不推荐使用** 

## count()方法

count()方法用于统计列表中某个元素出现的次数

语法格式

`listname.count(obj)` 

listname:列表名称

obj:表示判断是否存在的元素

例子:

```
a_list = [2]*5
print(a_list)
print("2出现%d次" % a_list.count(2))
```

```
[2, 2, 2, 2, 2]
2出现5次
```

## index()用法

index()方法用于定位某元素在列表的索引位置，如果该元素没有出现，则会引发ValueError错误。 

语法格式:

`listname.index(obj,start,end)`

index()方法可以传入start,end参数，用于指定在列表的某范围内搜索元素

例子:

```
test_list = list(range(1,10))
print(test_list)
print(test_list.index(9))
```

输出结果:

```
[1, 2, 3, 4, 5, 6, 7, 8, 9]
8
```
## pop用法

pop()方法会溢出列表中指定索引处的元素，如果没有传入参数，则会移除列表中最后一个元素。

语法格式:

`listname.pop(index)` 

例子:

```
test_list = list(range(1,10))
print(test_list)
# 默认移除最后一个元素
test_list.pop()
print(test_list)
# 移除第一次元素
print(test_list.pop(0))
print(test_list)
```

输出结果:

```

[1, 2, 3, 4, 5, 6, 7, 8, 9]
[1, 2, 3, 4, 5, 6, 7, 8]
1
[2, 3, 4, 5, 6, 7, 8]
```

## reverse()方法

reverse()方法会将列表中所有元素反向存放。

语法格式;

`listname.reverse()`

例子:

```
test_list = list(range(1,11))
print(test_list)
test_list.reverse()
print(test_list)
```

输出结果:

```
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```

## sort()用法

sort()方法用于对列表元素进行排序。

语法格式:

`listname.sort(key=None,reserse=Fale)`

- key参数用于指定从每个元素中提取一个用于比较的键。

- reverse参数用于设置是否需要逆序，默认为False也就是从小打到排序，否则反之。

例子:

```
a_list = [123,213,1,325,1,51,213,5132,4156]
print(a_list)
# 对列表进行排序
a_list.sort()
print(a_list)
# 逆序排序
a_list.sort(reverse=True)
print(a_list)
```

输出结果:

```
[123, 213, 1, 325, 1, 51, 213, 5132, 4156]
[1, 1, 51, 123, 213, 213, 325, 4156, 5132]
[5132, 4156, 325, 213, 213, 123, 51, 1, 1]
```

# Python range()快速初始化数字列表

Python的range()函数能够生成一系列的数字。

```
for value in range(1,5):
  print(value)
```

输出结果:

```
1
2
3
4
```

range()方法是指定从第一个值开始，生成连续的数字，直到指定的第二值为止(不包括第二个值)

range()方法可以指定步长，也就是说，每次连续增加时所增加的数值。

# Python tuple元组详解

与列表类似，元组也可以存储任何Python的数据类型，但元组是不可变的，一旦赋值则不可修改。

Python中，使用`()`小括号包住的一列数据，被称为元组，而各数据之间又由`,`逗号隔开。

元组的数据类型是:`tuple`

```
test_tuple = tuple(range(1,11))
print(type(test_tuple))
```
输出结果:

```
<class 'tuple'>

[Process exited 0]
```

## Python创建元组

### = 运算符直接创建元组

可以使用赋值运算符`=`将一个元组赋值给变量。

语法格式:

```
tuplename = (element1,element2...elementn)
```

其中tuplename表示要创建的元组名,element代表元素。

例子:

```
num_tuple = tuple(range(1,11))
str_tuple = ("Just A Test",)
object_tuple = ("a",1,2.22,num_tuple,list(range(1,5)))

print(num_tuple)
print(str_tuple)
print(object_tuple)
```

输出结果:

```
(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
Just A Test
('a', 1, 2.22, (1, 2, 3, 4, 5, 6, 7, 8, 9, 10), [1, 2, 3, 4])
```

**注意** :创建元组时，如果元组内只有一个元素，那么必须要在元素后加上一个逗号，否则Python解释器将会一位是字符串。

```
test_str = ("Just a Test")
str_tuple = ("Jest a Test",)
print(type(test_str))
print(type(str_tuple))
```

输出结果:

```
<class 'str'>
<class 'tuple'>
```

### 使用tuple()函数创建元组

Python提供了tuple()函数创建元组，我们可以将一些常见的对象，转化成元组。

语法格式:

`tuple(iterable: Iterable[_T_co]=...)`

例子:

```
test_list = list(range(1,21,2))
test_tuple = tuple(test_list)

print(type(test_list))
print(type(test_tuple))
```

输出结果:

```
<class 'list'>
<class 'tuple'>
```

## Python访问元组元素

可以通过元素的下标获取或通过切片操作获取。

## Python修改元组元素

虽然前面提到元组是不可变序列，但还是可以通过一些手段进行修改元组内的数据。

### 重新赋值

```
test_tuple = tuple(range(1,6))
print(test_tuple)
test_tuple = tuple(range(1,20))
print(test_tuple)
```

输出结果:

```
(1, 2, 3, 4, 5)
(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19)
```

### 连接多个元组

```
test_tuple = tuple(range(1,6))
print(test_tuple)
test_tuple2 = tuple(range(6,11))
test_tuple += test_tuple2
print(test_tuple)
```

输出结果:

```
(1, 2, 3, 4, 5)
(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```

## Python删除元组

若创建的元组不再使用可以使用`del`语句将其删除。

# Python元组和列表的区别

本节讨论元组和列表的区别:

| 区别                 | 元组 | 列表 |
|----------------------|------|------|
| 元素是否允许任意修改 | ❌   | ⭕   |
| 是否能作为map的key   | ⭕   | ❌   |
| 内存占用小           | ⭕   | ❌   |

# Python列表和元组的底层实现

这里就不记录笔记了，总的来说就是列表是一个长度可变且连续的内存空间，当列表的元素满了后，将会申请更多的内存空间，再将原本的内存空间拷贝过去。而元组则是一个空间带下固定且连续的内存空间。Python对元组进行了优化，例如为了避免系统老是释放和申请内存空间这一繁琐且耗时的操作，所以每次释放元组时其实是将其放入一个缓存中，如果下次还需要申请相同数据的元组，则可以直接从缓存中拿到。

具体介绍看这里[💿Python列表和元组的底层实现](http://c.biancheng.net/view/5360.html) 

# Python dict字典

dict字典属于可变序列，但是无序的序列，其内容是键值对的形式保存的。

键值对也可以称为映射，所以字典的值与键是互相对应的关系。

Python字典特征

| 特征                           | 解释                                                                                        |
|--------------------------------|---------------------------------------------------------------------------------------------|
| 通过键读取元素                 | 通过字典中的键来获取指定项，而不是通过索引获取                                              |
| 字典是任意数据类型的无序集合   | 和列表、元组不同，索引值可以通过数字获取对应的元素，而字典中的元素是无序的。                |
| 字典是可变的，并且可以任意嵌套 | 字典可以在原处增长或缩短，并且它支持任意深度的嵌套，即字典存储的值也可以是列表或其他字典   |
| 字典中的键必须是唯一的         | 字典中不允许出现相同的键，否则只会保留最后一个键值对                                        |
| 字典中的键必须是不可变的       | 字典中的值是不可变的，所以只能使用数字、或元组、字符串等，不能使用可变，例如可变序列：列表 |

**Python中字典的数据类型为`dict`** 

## Python创建字典

### 花括号语法创建字典

字典中每个元素都包含两个部分，分别是键和值。因此在创建字典元素时，需要在键和值之间以冒号`:`分隔，相邻元素之间使用逗号分隔，所有元素放下大括号`{}`间。

语法格式:

`dictname = {'key1':'value',"key2":"value"}`

其中`dictname` 代表字典名，`key:value` 表示各个元素的键值对。

使用花括号创建字典例子:

```
test_dict = {"语文":80,"数学":12}
print(test_dict)

```

输出结果

`{'语文': 80, '数学': 12}`

### 通过fromkey()方法创建字典

Python中使用dict字典类型提供的fromkeys()方法创建所有键值为空的字典。

语法格式

`dictname=dict.fromkeys(list,value=None)` 

list参数表示字典中键的列表，value参数默认为None，表示所有键对应的值。

例子:

```
knowledge = {'语文','数学','英语'}
scores = dict.fromkeys(knowledge,100)
print(scores)
```

输出结果:

```
{'语文': 100, '英语': 100, '数学': 100}
```

### 通过dict()映射函数创建字典

dict()函数常用创建字典方法


