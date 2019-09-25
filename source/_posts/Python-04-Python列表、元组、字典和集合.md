---
title: '[Python-04]Python列表、元组、字典和集合'
copyright: true
date: 2019-09-08 15:00:28
categories: Python
tags:
 - Python系列
---

Python系列第四章笔记，查看Python系列所有文章，请点击[💿](http://c.biancheng.net/python/list_tuple_dict/)

<!--more-->

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

![dict()函数创建字典](Python-04-Python列表、元组、字典和集合/dict()函数创建字典.png)

## Pyton访问字典

dict()通过元素的键进行访问值，不能像列表或元组通过下标或切片的形式来访问。

例子:

```
test_dict = (['姓名','LTy'],['性别',True])
test_dict = dict(test_dict)
print(type(test_dict))
print(test_dict['姓名'])
print(test_dict.get("性别"))
```

## Python删除字典

如需手动删除，可以使用del语句。

# Pythton dict字典基本操作(包括添加、修改、删除键值对)

本小节对字典实现常见的操作:

- 向字典中添加新的键值对

- 修改字典中的键值对

- 从字典中删除指定的键值对

- 判断字典中是否存在指定的键值对

## Python字典添加键值对

为字典中添加新的键值对只需要为不存在的`key`赋值。

语法格式:

`dict[key]= value` 

`dict`表示字典名称,`key`代表新建键值对的键,`value`代表新建键值对的值

例子:

```
test_dict = (['姓名','LTy'],['性别',True])
test_dict = dict(test_dict)
test_dict['班级'] = 502
print(test_dict.get("班级"))
```

输出结果:

`502` 

## Python字典修改键值对

这里的修改是指:修改键值对的值。

**Python中如果新添加的键值对的键存在，那么则会覆盖原本的键值对的值** 

例子:

```
test_dict = (['姓名','LTy'],['性别',True])
test_dict = dict(test_dict)
test_dict['班级'] = 502
print(test_dict.get("班级"))
test_dict['班级'] = 999
print(test_dict.get("班级"))
```

输出结果:

```
502
999
```

## Python字典删除键值对

如果要删除字典中的键值对，则可以使用`del`语句，指定要删除的键值对的键。

例子:

```
test_dict = (['姓名','LTy'],['性别',True])
test_dict = dict(test_dict)
test_dict['班级'] = 502
print(test_dict.get("班级"))
del test_dict['班级']
print(test_dict.get("班级"))
```

输出结果:

```
502
None
```

## 判断字典中是否存在指定键值对

如果需要判断字典是否存在指定键值对的键，可以使用`in`或`not in`运算符。

**注意:这里的`in`或`not in`运算符都是基于`key`来判断字典中某个键值对是否存在的.** 

例子:

```
test_dict = (['姓名','LTy'],['性别',True])
test_dict = dict(test_dict)
del test_dict['姓名']
print(test_dict.get("姓名"))
print('姓名' in test_dict)
```

输出结果:

```
None
False
```

# Python dict字典方法完全攻略(全)

想查看`dict`该类包包含哪些方法，可以使用`dir()`方法进行查看。

## Python keys(),values()和items()方法

这三个方法可以获取字典中特定的数据.

`keys()`方法用于返回字典中的所有键;

`values()`用于返回字典中所有键对应的值;

`items()`用于返回字典中所有的键值对;

例子:

```
test_dict = {'语文','数学','英语'}
test_dict = dict.fromkeys(test_dict,100)
print(test_dict.keys())
print(test_dict.values())
print(test_dict.items())
```

输出结果:

```
dict_keys(['英语', '语文', '数学'])
dict_values([100, 100, 100])
dict_items([('英语', 100), ('语文', 100), ('数学', 100)])
```

**注意:`Python2.x`中，上面提到的方法的返回值是列表类型。但在`Python3.x`中，以上方法返回的类型不是序列类型。** 

## Python copy()方法

`copy()`方法用于返回一个具有相同键值对的新字典:

例子:

```
test_dict = {'语文','数学','英语'}
test_dict = dict.fromkeys(test_dict,100)
test_dict2 = test_dict.copy()
print(test_dict,"\n",test_dict2)
```

输出结果:

```
{'英语': 100, '语文': 100, '数学': 100}
{'英语': 100, '语文': 100, '数学': 100}
```

**注意Python的copy()方法涉及到`深拷贝`与`浅拷贝`的关系，当字典A拷贝键值对给字典B后，那么拷贝的数据(字典B内)将会在字典A对键值对进行修改时发生变化，而字典A添加新的键值对，字典B不收影响。** 

## Python update()方法

updae()方法可以使用一个字典所包含的键值对来更新已有的字典。

如果被更新的字典中已存在对应的键值对，那么原键值对值将会被覆盖，否则将会被添加。

例子:

```
test_dict = {'语文','数学','英语'}
test_dict = dict.fromkeys(test_dict,100)
test_dict2 = test_dict.copy()
print(type(test_dict))
test_dict['语文']=100
test_dict2['物理']=200
test_dict2.update(test_dict)
print(test_dict,"\n",test_dict2)
print(test_dict,"\n",test_dict2)
```

输出结果:

```
<class 'dict'>
{'英语': 100, '数学': 100, '语文': 100}
{'英语': 100, '数学': 100, '语文': 100, '物理': 200}
{'英语': 100, '数学': 100, '语文': 100}
{'英语': 100, '数学': 100, '语文': 100, '物理': 200}
```

## Python pop方法

pop()方法可以获取指定key的value，并删除。

例子:

```
test_dict = {'语文','数学','英语'}
test_dict = dict.fromkeys(test_dict,100)
print(test_dict.pop('语文'))
```

输出结果:

```
100
{'英语': 100, '数学': 100}
```

## Python popitem()方法

popitem()方法用于弹出字典中最后一个键值对(这里的最后一个其实是随机的，因为字典是无序序列，所以并不能说的某个键值对的位置取向).

例子:

```
test_dict = {'语文','数学','英语'}
test_dict = dict.fromkeys(test_dict,100)
print(test_dict.popitem())
print(test_dict)
```

输出结果:

```
('数学', 100)
{'英语': 100, '语文': 100}
```

## Python setdefault()方法

setdefault()方法用于根据key来获取value，但有两种场景，第一种是，如果获取的键值对的key存在字典中，则会直接获取改键值对的value，不存在则会为该key先将value设置为默认的value，然后再返回该key对应的value。


**特点:setdefault方法总能返回指定key对应的value;**

例子:

```
test_dict = {'语文','数学','英语'}
test_dict = dict.fromkeys(test_dict,100)
# 要设置的键值对的key不存在.
test_dict.setdefault('物理',120)
print(test_dict)
# 要设置的键值对的key存在.
test_dict.setdefault('语文',200)
print(test_dict)
```

输出结果:

```
{'数学': 100, '英语': 100, '语文': 100, '物理': 120}
{'数学': 100, '英语': 100, '语文': 100, '物理': 120}
```

# Python使用字典格式化字符串

我们知道，在格式化字符串时，如果格式化字符串模板中包含多个变量时，则后面月必须按顺序加上相同个数的变量，如果变量过多，则会十分麻烦，Python提供了用字典格式化字符串的方式来解决。

例子:

```
student = '姓名:%(name)s\t班级:%(class)d\t综合成绩:%(score)f'
student_info = {'name':'LTy','class':604,'score':99.5}
print(student % student_info)
```

输出结果:

```
姓名:LTy        班级:604        综合成绩:99.500000
```

# Python set集合详解

set集合的特点:

- 数据唯一性(必须保证集合中每种数据元素是唯一的)

- 数据不可变(只允许存储不可变的数据类型，例如列表、字典、集合则是不允许存储在集合中的)

- 集合是无序的，所以每次输出元素时，排序顺序可能都不同。

## Python创建set集合

有两种创建`set`集合的方法，分别是使用{}创建和使用set()函数将列表、元组等数据类型转化为集合。

### 使用{}创建

语法格式:

`setname = {element1,element2...elementn}`

setname代表集合的名称。

例子:

```
test_set = {1,2,3,4}
print(test_set)
```

输出结果:

```
{1, 2, 3, 4}
```

### 使用set()函数创建集合

set()函数为Python的内置函数，其可以将字符串、列表、元组以及range对象等可迭代的对象转换成集合。

语法格式:

`setname = set(iteration)` 

iteration就表示可迭代的对象。

```
set1 = set("I Love China")
set2 = set(list(range(1,6)))
set3 = set(tuple(range(1,6)))
set4 = set(range(1,6))
print(set1)
print(set2)
print(set3)
print(set4)
```

输出结果:

```
# 从输出结果看，字符串被打乱了
{'a', 'L', 'h', ' ', 'C', 'e', 'n', 'i', 'I', 'v', 'o'}
{1, 2, 3, 4, 5}
{1, 2, 3, 4, 5}
{1, 2, 3, 4, 5}
```

**注意:如果想要创建空集合，必须使用`set()`函数实现，因为如果只给一对`{}`，Python解释器会将其视为一个空字典。**

## Python访问set集合元素

由于`set`集合是无序的，所以无法通过下标索引进行访问，但是我们可通过遍历`set`集合访问元素。

```
set1 = set("I Love China")

for ele in set1:
    print(ele,end='')
```

输出结果:

```
oCenaL Ihvi
```

## Python删除set集合

想要手动删除`set`集合可以使用`del()`方法。

```
set1 = set("I Love China")
print(set1)
del(set1)
print(set1)
```

输出结果:

```
{'a', 'i', 'e', ' ', 'L', 'C', 'o', 'h', 'I', 'n', 'v'}
Traceback (most recent call last):
  File "test.py", line 4, in <module>
  │ print(set1)
NameError: name 'set1' is not defined
```

# Python set集合基本操作(添加、删除、交集、并集、差集)

`set`集合最常用的场景是添加、删除元素以及在多个集合之间做交集、并集、差集等运算。 

## 向set集合中添加元素

Python为`set`集合提供了`add()`方法，其可以向`set`集合添加元素。

语法格式:

`setname.add(element)` 

setname表示要添加元素的集合，element代表添加的元素。

**注意:element不可为可变数据类型** 

## 从set集合中删除元素

想要从集合中删除某个元素可以使用`remove()`方法

语法格式:

`setname.remove(element)` 

例子:

```
set1 = {1,2,3,4}
print(set1)
set1.remove(1)
print(set1)
```

输出结果:

```
{1, 2, 3, 4}
{2, 3, 4}
```

**注意:如果要删除的元素已经被删除，再次删除则会引发`KeyError`错误**

如果想要在删除失败时不抛出异常，我们可以使用`discard()`方法。

例子:

```
set1 = {1,2,3,4}
print(set1)
set1.remove(1)
print(set1)
set1.discard(1)
print(set1)
set1.remove(1)
print(set1)

```

输出结果:

```
{1, 2, 3, 4}
{2, 3, 4}
{2, 3, 4}
Traceback (most recent call last):
  File "test.py", line 7, in <module>
  │ set1.remove(1)
KeyError: 1
```

`discard()`方法与`remove()`方法的唯一区别在于:**删除失败时是否抛出异常** o

## Python set集合做交集、并集、差集运算

`set`集合最常用的操作也就是进行交集、并集、差集以及对称差集运算了，首先看这张图:

![集合示意图](Python-04-Python列表、元组、字典和集合/Python集合示意图.png) 

图中有两个集合，分别是:

- `set1={1,2,3}`

- `set2={3,4,5}`

下面的表格展示了不同的运算，得到的不同的结果。

![集合运算图](Python-04-Python列表、元组、字典和集合/Python集合运算.png) 

# Python set集合方法详解(全)

如果想要看`set`集合所有的方法，可以使用`dir(set)`进行查看。

Python set方法

| 方法名                        | 语法格式                               | 功能                                                                 |
|-------------------------------|----------------------------------------|----------------------------------------------------------------------|
| add()                         | set1.add()                             | 向set1集合中添加非可变元素                                           |
| clear()                       | set1.clear()                           | 清空set1集合中所有的元素                                             |
| copy()                        | set2=set1.copy()                       | 拷贝set1集合中的元素给set2                                           |
| difference()                  | set3=set1.difference(set2)             | 将set1中有而set2中没有的元素给set3                                   |
| difference_update()           | set1.difference_update(set2)           | 从set1中删除与set2相同的元素                                         |
| discard()                     | set1.discard(element)                  | 删除set1中的element元素                                              |
| intersection()                | set3=set1.intersection(set2)           | 取set1和set2的交集赋值给set3                                         |
| intersection_update()         | set1.intersection_update(set2)         | 取set1和set2的交集并且将值赋值给set1(更新set1的值为set1与set2的交集) |
| isdisjoint()                  | set1.isdisjoint(set2)                  | 判断set1和set2是否没有交集，有交集返回False;没有则返回True           |
| issubset()                    | set1.issubset(set2)                    | 判断set1是否是set2的子集                                             |
| issuperset()                  | set1.issuperset(set2)                  | 判断set2是否是set1的子集                                             |
| pop()                         | a = set1.pop()                         | 取出set1中一个元素并赋值给a                                          |
| remove()                      | set1.remove(element)                   | 移除set1中的element元素                                              |
| symmetric_difference()        | set3=set1.symmetric_difference(set2)   | 取set1和set2中互不相同的元素给set3                                   |
| symmetric_difference_update() | set1.symmetric_difference_update(set2) | 取set1和set2中互不相同的元素并更新set1的值                           |
| union()                       | set3=set1.union(set2)                  | 取set1和set2的并集，赋值给set3                                       |
| update()                      | set1.update(element)                   | 添加列表或集合中的元素到set1                                         |

# Python frozenset(set集合的不可变版本)

前面我们所学习的`set`集合是可变的，而frozenset则是`set`集合的不可变版本，它不具备`set`集合的所有能改变集合本身的方法。

frozenset的主要使用场景:

- 当集合元素不需要改变的时候，使用frozenset替代set将会更加安全。

- 当某些API需要不可变对象作为参数时，必须用到frozenset替代set。


# 深入底层了解Python字典和集合、一眼看穿他们的本质

本小节涉及到未知的知识，推荐去原文看**[查看原文](http://c.biancheng.net/view/5302.html) 

# Python深拷贝和浅拷贝详解

本小节涉及到未知的知识，推荐去原文看[查看原文](http://c.biancheng.net/view/5358.html) 

