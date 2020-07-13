---
title: GNU Emacs Lisp编程入门》读书笔记
copyright: true
date: 2019-12-21 16:17:25
bucategories: Lisp
tags:
 - EmacsLisp
---

借了本Elisp的书，不厚，200多页，大概一周(两周(一個月))就能看完吧!

<!--more-->
    
# 第一章 列表处理 #

列表是Lisp的基础。

## Lisp列表 ##
### 介绍 ###

简单的Lisp列表书写形式:

``` emacs-lisp
'(rose violet daisy buttercup) ;; => (rose violet daisy buttercup)
```

这个简单的列表中的四个元素时四种不同花的名称。**元素之间用空格隔开，并且被括号包住。**

另有一种在elisp中常用到的写法:

 
```emacs-lisp
(+ 2 2) ;; => 4
```

这种写法是Lisp的一个特性————**数据和代码都用相同的方式表示**。

**列表还允许嵌套列表，也就是说列表的元素也可以是列表**。

### Lisp原子 ###


原子意味着"不可再分"。例如刚刚列表内的'2'或者是'rose'等等，这些都是原子。

而列表却不是原子，因为**列表是由CAR和CDR与CONS构成的**。

技术上说，Lisp的列表有三种组合方式:
  * 括号和括号中由空格分隔的原子
  * 括号和括号中的其他列表
  * 括号和括号中的其他列表和原子

**一个列表可以仅有一个原子或完全没有原子**

无任何原子的列表称作空列表: `()`。你可以把空列表看为原子或列表。

*原子和列表可以被称为**符号表达式**(symbolic expression)，又可称为**S-表达式** *。

Lisp编程几乎都是关于列表中的符号的(且有时是关于数字的).

**双引号中的文本，都是单个原子**:`'(Info "Name:EvanMeek Age:18 Sex:Men**)`

这种被双引号括起来的文本叫做**字符串(string)**。

### 列表中的空格 ###

Lisp中括号的数量无关紧要。下面两个的列表是完全相同的。

括号多
``` emacs-lisp
'(1 2 3 4      5       6) ;; => (1 2 3 4 5 6)
```

括号少
``` emacs-lisp
'(1 2 3 4 5 6) ;; => (1 2 3 4 5 6)
```

适当的在列表中加入一些空格以及换行符可以提高可读性。


### GNU Emacs帮助你输入列表 ###

在Emacs中使用Emacs Lisp模式或Lisp交互模式输入Lisp表达式时可以用`Tab`按键使光标所在行自动缩排到适当的位置。要使某个区域的表达式都自动缩排的快捷键是`C-M-\`

并且Emacs会具有括号匹配的功能，以防你迷失在Lisp的括号海洋中。

有兴趣的可以看下这个知乎提问。<https://www.zhihu.com/question/356026550>

## 运行一个程序 ##

如果你想运行一段Lisp程序，那么计算机可能会做三种事:
* 只返回列表本身;
* 提示出错信息;
* 将列表中第一个符号作为要执行的命令;
大多数情况，我们希望计算机做的是第三件事

前面我们看到过一些列表的前面有一个单引号"'"，它其实是Lisp中的一个引用(quote)。它的作用是告诉Lisp不要做对这个列表做任何操作，只返回列表本身即可。

``` emacs-lisp
'(只是返回本身 即可) ;; => (只是返回本身 即可)
```

而如果列表前没有quote，那么列表的第一个符号就会称为Lisp要执行的命令(函数)，后面的则是函数的参数。

``` emacs-lisp
(+ 1 2 3 4) ;; => 10
```

Emacs中可以将光标放置一对S-表达式后按 `C-x C-e`就会将表达式读入至Lisp解释器中，进行解释，将结果输出至回显区，英文叫 `mini buffer`。

也可以对原子(没有被括号括起来)求值

## 产生错误消息 ##

编写Lisp代码时难免遇到错误，而Lisp解释器会在程序出错时输出报错信息。与其说是报错信息，不如说是有助的信息(书上这么说)。

下面，我们对一个没有引用并且第一个元素不是一个有意义的符号的列表进行求值。

``` emacs-lisp
(Just Error!) ;; => Symbol's function definition is void: Just
```
这个输出结果就像是出错了一样，它告诉我们Just符号命令没有定义。也就是说Lisp解释器视图将`(Just Error!)`当成类似`(+ 1 2)`这种列表进行求值，但由于后者的第一个元素"+"是有意义的(已定义)，不会有任何问题，而我们编写的列表中第一个元素"Just"是没有任何意义(未定义)的，Lisp解释器不知道怎么办了，只好告诉我们一些有用的信息。


 ##

我们已经讨论过一些符号或函数，例如"+"。就以"+"为例子，当我们对`(+ 2 2)`表达式求值时，计算机并不是执行的"+"这个命令，而是执行其对应的指令。我们甚至可以随意改变，例如我把"+"改为"Plus"。





## Lisp解释器 ##

下面，简单解释下Lisp解释器在对一个列表求值时做了些什么。
* 首先，检查列表前是否有单引号(quote)，如果有则直接返回这个列表的输出形式。
* 若没有单引号则查看列表的第一个元素，是否有相对应的函数定义，如果有则执行对应的指令。
* 若没有则提示错误消息。
以上就是Lisp解释器的工作方式。但都比较简单，下面将会介绍几种比较复杂的工作方式。

* 第一种。Lisp解释器还可以对符号求值(并不是只能对列表)，但这个符号前不能有单引号和被括号括起。
* 第二种，Lisp解释器在遇到一些特殊函数时。这些特殊的函数以特殊的方式运行着，这些特殊函数通常被称为特殊表(special form)。见名知意，它用于一些特殊的工作，例如定义函数之类的。
* 第三种，Lisp解释器在遇到一些不是特殊表，而是列表的一部分时。这可能意味着是一个嵌套列表，Lisp解释器首先查看列表中是否有另外一个列表，如果有则先解释内部列表，如果内部列表仍然具有一个列表，那么就再次解释更深的列表层级中的列表，最终以此返回结果。

否则Lisp解释器将会从左往右依次执行。

### 字节编译 ###

Lisp解释器还可以将Lisp代码编译成字节。这样的好处是可以让程序的执行速度更快，缺点是可读性几乎为零。

被编译成字节码后的源码叫做elc，可以通过命令`(byte-compile-file)`进行编译。


## 求值 ##

前面我们所得到的一些结果，都是由Lisp解释器求值得到的。求值也就是Lisp解释器处理表达式时进行的操作。

解释器对表达式求值时几乎总是会返回值，如果没有返回值，通常是会提示一些错误信息。

解释器对表达式求值不仅会返回值，还可能会有一些附加效果，可能是移动光标或拷贝文件、刷新Buffer之类的动作。

### 对一个内部列表求值

上一节我们讨论过嵌套列表，这一小节解释了为什么内层列表总是首先被求值。首先给出答案:因为内层列表的返回值要被外层列表所使用。

下面通过一个例子了解这个求值的过程。

``` emacs-lisp
(+ 1 (+ 2 3))
```

我们知道使用快捷键`C-x C-e`可以对一个表达式求值，那么我们将光标放置`(+ 2 3)`表达式之后，然后按下这个快捷键，看看会发生什么。很显然，我们得到返回值`5`，那么再将光标放置整个表达式之后，按下快捷键得到返回值`6`。这就很好的解释了Lisp解释器对嵌套列表的求值过程。

我们可以通过快捷键`C-h k`然后键入`C-x C-e`查看这个快捷键所对应的符号(命令\函数)，也就是`eval-last-sexp`。 这个函数的作用是**对最近的一个表达式进行求值，并且将结果打印至输出区域**。





## 变量 ##

其他语言中，我们经常将某值赋值给一个变量，但你有见过可以将一个函数赋值给一个变量的语言吗(Python)?

**在Lisp中，我们可以把值赋值给符号，并且这个符号的值可以使Lisp中任意表达式(符号、数字、列表、字符串)**，并且符号的值是可变的，我们把一个有值的符号称为变量。

前面提到的将一个函数定义赋值给一个符号就是通过Lisp中变量的特性。**Lisp的符号可以同时具有一个函数定义和一个值。**我们可以把这种概念想象成具有多个抽屉的柜子。

例如变量`fill-column`是一个有值的符号，对它求值可以获取自动换行的字符数。

任何值都可以赋给符号，这个操作是:变量与一个值进行绑定。

`fill-column`只是一个很普通的值而已，如果我们对其求值时加上括号，将会发生一些错误。

``` emacs-lisp
(fill-column) ;; => Symbol's function definition is void: fill-column
```
这个错误被打印在回显区，很容易可以理解为什么会出现这个错误。因为Lisp解释器将这个列表读入后试图寻找第一个元素与之相联系的函数定义，让而fill-column只是一个值为数字的变量罢了，所以就会引发这个错误。

### 符号无值时的错误信息 ###

下面我对一个没有赋值的符号进行求值。
``` emacs-lisp
(+ 2 2) ;; => 4
```
很显然，如果是在这个表达式右括号后执行`eval-last-sexp`将不会有任何问题，但我们现在将光标移动至`+`后，执行`eval-last-sexp`，会引发这个报错提示:`Symbol's value as variable void: +`。

这是因为Lisp解释器将无值的符号当成一个变量处理了，而Lisp解释器却没有找到任何关于`+`的变量，只找到了函数定义，因为就报错了。

## 参量 ##

参量对应着`argument`，也就是其他语言中参数的意思。例如`(+ 2 3**`这个列表的参数就是`2`和`3`，而这个`+`则是函数，`+`允许由多个参量。

**不同的函数需要不同数目的参量，有些函数甚至不需要参量。**

### 参量的数据类型 ###

函数所接收的参量也是有数据类型的限制的，例如`+`函数就规定其参量的值必须是数字。

有些函数允许有任意数量个参量，例如`concat`函数，它将任意多个字符串类型的参量合并为一个字符串。

``` emacs-lisp
(concat "My name is:" "EvanMeek") ;; => "My name is: EvanMeek"
```

**请注意，`concat`函数的参量类型需要字符串并不意味着参量就必须写上字符串，这里指的是参量的值必须是字符串，请不要混淆。**

例如我们可以在`concat`的参量中写上`substring`函数。

``` emacs-lisp
(concat "Nice to meet " (substring "fuck you" 5 8))
```
上面的`concat`是一个参量必须都为字符串类型且任意数量的函数，而`substring`是一个可变数量参量且有多种参量类型的函数。
`substring`是可以对字符串这种单原子抽取出子字符串的，而这种操作可以取个好玩的名称`原子分裂机`。





### 作为变量和列表的值的参量 ###

> 上一小节提到————有些函数允许有任意数量个参量，且参量数据类型不同，我们可以理解为有些函数的参量可以是任意任意数量个符号。

这一节，我们将谈谈函数的参量还支持那些。

函数参量还支持列表和变量，当参量为变量时，Lisp解释器就会寻找是否有有值的变量，如果有就返回值，例如:

``` emacs-lisp
;; 结果可能不同，取决于你的Emacs配置。
(+ 2 fill-column) ;; => 82
```

并且参量还可以是一个列表，例如:

``` emacs-lisp
(+ 2 (+ 3 (+ 4))) ;; =>  9
```


### 数目可变的参量 ###
前面提到过的函数已经具有这个规则了，参量的数目可变，例如我们已经知道的`+`函数:

``` emacs-lisp
(+) ;; => 0
(+ 2) ;; => 2
```
又或是`\*`函数:

``` emacs-lisp
(*) ;; => 1
(* 2) ;; => 2
```

所以它们都具有一个特性————参量数量可变。

``` emacs-lisp
(* 1 2 3) ;; => 66
(+ 1 2 3) ;; => 6
```



### 用一个错误类型的数据对象作为参量 ###

试想下，如果我们对函数提供了错误类型的参量会发生什么？

``` emacs-lisp
(+ 2 'hello) ;; => Wrong type argument: number-or-marker-p,hello
```
我们知道`+`函数的参量必须是数字，而我们传入的参量2的`'hello`被Lisp解释器操作时会试图将`2`和`'hello`的返回值相加，但是`'hello`的返回值不是一个数字，所以就会引发这个错误。

让我们来试着解读下Lisp解释器提示的错误信息。首先Lisp解释器明确的告诉了我们————Wrong type argument(参量类型错误)，再是一段我们看不懂的符号`number-or-marker-p`这个符号意味着:Lisp解释器提示我们`+`函数的参量所需的数据类型，`number`也就是数字，而`marker`是一个标记(Elisp的一个特性，缓冲区的位置是由标记决定的，我们可以通过快捷键C-@C-SPC命令设置标记)， 而`p`则是Lisp的一个历史问题(早期Lisp程序员中用"p"替代"predicate"用于表示是否为真)，类似的有`zerop`用于判断参量值是否为零。最后的`hello`则是我们传给`+`函数出错的参量的值。

### message函数 ###
`message`函数用于给用户发送消息。这些消息是被打印在回显区的，它的第一个参量要求为一个`FORMAT STRING`(可格式化字符串)。

``` emacs-lisp
(message "Should you sleep now?") ;; => Should you sleep now?
```
双引号中的文本被打印在回显区(我们看到的是表达式的返回值，而打印只是一个附加效果)，这显得没什么新奇的，我们直接写一个字符串原子并且对其求值貌似也有同样的结果，下面将会介绍格式化字符串。

``` emacs-lisp
(message "The name of this buffer is: %s" (buffer-name)) ;; => "The name of this buffer is: 《GNU-Emacs-Lisp编程入门》读书笔记.md"
```
这输出了我当前buffer的名称。重点在于第一个参量中存在一段特殊的子串`%s`，`message`函数的第二个参量的值将用于它，这个`%s`代表了接收一段字符传，类似的还有`%d`用于接收一个十进制数。

``` emacs-lisp
(message "The value of fill-column is %d" fill-column) ;; => "The value of fill-column is 80"
```

这些特殊子串都支持变量、列表或符号，例如：

``` emacs-lisp
(message "我想吃%s，各来%d斤!" (concat "苹果、" "梨子、" "葡萄") (+ 1 1)) ;; => "我想吃苹果、梨子、葡萄，各来2斤!"
```

从第二个参量开始将会一次取代第一个参量中特殊子串的位置。
## 给一个变量赋值 ##

这一小节学习为一个变量赋值，常用的操作有`set`、`setq`、`let`，在这节主要将`set`和`setq`。并且描述这两个函数是如何工作的。

### 使用set函数 ###

我们要为`flowers`符号附上一个列表值，那么可以这么写 
```emacs-lisp
(set 'flowers '(菊花 百合 玫瑰)) ;; => (菊花 百合 玫瑰)
```
这意味着我们将列表`菊花 百合...`绑定给了`flowers`符号。

两个参量的引用是为了让其不要求值，所以你可以知道`set`函数的第一个参量最好要加上引用，这大概是很常用的搭配。

### 使用setq函数 ###

`setq`函数与`set`函数很相似，它们只有一个区别，就是`setq`函数会自动给第一个参量加上引用，例如用`setq`完成上一小节的例子:

``` emacs-lisp
(setq flowers '(菊花 百合 玫瑰)) ;; => (菊花 百合 玫瑰)
```

并且它们俩都是可以为多个变量绑定多个值的，例如:

``` emacs-lisp
(setq 学生 '(学号 地址 姓名 电话)
      老师 '(教师号 地址 姓名 电话))
学生 ;; => (学号 地址 姓名 电话)
老师 ;; => (教师号 地址 姓名 电话)
```

其中参量二绑定给参量一，参量四绑定给参量三，以此类推。




### 计数 ###

就是用`setq`函数完成了一个递增的操作。

首先生成一个初始化器，再生成一个递增器，再进行输出数值。

``` emacs-lisp
(setq count 0) ;; 初始器
(setq count (1+ count)) ;; 递增器
count ;;输出
```

## 小结 ##
记录一下，加油努力，毕竟书中都说了，我已经跨过了最陡峭的那段山路。

  * Lisp程序由表达式组成，表达式是列表或单个原子。
  * 列表由0个或更多的原子或内部列表组成，原子或列表之间由空格隔开，并由括号括起来，列表可为空。
  * 原子是多字符的符号(如fill-column)，单字符的符号(如+号)，双引号之间的字符串或者数字。
  * 对数字求值就是其本身
  * 对双引号之间的字符串求值也是其本身
  * 当对一个符号求值时，将返回它的值。
  * 当对一个列表求值时，Lisp解释器查看列表首符号绑定在其上的函数定义，并执行其指令。
  * 单引号告诉Lisp解释器返回后续表达式的书写形式，而不是像没有单引号时那样对其求值。
  * 参量是传递给函数的信息。除了作为列表的第一个元素的函数之外，通过对列表的其余元素求值来计算函数的参量。
  * 当对一个函数求值时总是返回一个值(除非得到错误信息)。另外，它也可以完成一些被称作附带效果的操作。在许多情况下，一个函数的主要目的是产生一个附带效果。
  
  **最后，希望这篇博客可以记录好第一章所讲。**

## 练习 ##

一个一个来，题目写在博客内吧...

* 通过对一个不在括号内的适当符号求值，产生一个错误信息。

解答:

``` emacs-lisp
一个不存在的变量 ;; => Symbol's value as variable is void: 一个不存在的变量 
```

* 通过对一个在括号内的适当符号求值，产生一个错误信息。

解答:

``` emacs-lisp
(又一个不存在的符号) ;; => Symbol's function definition is void: 又一个不存在的符号
```

* 创建一个每次增加2而不是1的计数器。

解答:

``` emacs-lisp
(setq count 0) ;; 初始器 
(setq count (+ 2 count)) ;; 递增器
count ;;输出
```

* 写一个表达式，当对它求值时，它在回显区输出一条消息。

解答:

``` emacs-lisp
(message "这段消息是要输出至%s%s的" '回显 "区")
```

# 第二章 求值实践 #

这一章主要讲了关于buffer的一些知识，讲了一些关于buffer的函数，并且讲解了一些buffer的注意事项。

## 缓冲区名 ##

讲了两个函数，分别是`(buffer-name)`以及`(buffer-file-name)`。

首先得清楚buffer和文件的概念，buffer的中文翻译叫做缓冲区，它其实是将文件中的内容拷贝到Emacs中，所以在我们修改buffer时，文件并不会改动，只有当我们进行保存修改时才会。而文件是保存在磁盘上的信息，人们常常在修改buffer时说成"我在修改xx文件"，这严格意义上讲是错误的，但人们都知道其实我只是在修改buffer，而不是那个文件，只不过说成文件罢了，所以各位小伙伴不要误解。

计算机没有人类那么聪明，所以大家在写代码时要分清楚这两个的不同。

``` emacs-lisp (buffer-name) ;; => "《GNU-Emacs-Lisp编程入门》读书笔记.md"
(buffer-file-name) ;; =>"/home/evanmeek/Documents/Blog/source/_posts/《GNU-Emacs-Lisp编程入门》读书笔记.md"
```
**表达式`(buffer-name)`用于获取当前buffer的名称而`(buffer-file-name)`用于获取当前buffer所对应文件的完整路径**，还要说明一点，并不是所有的buffer都有所对应的文件，例如`\*scratch\*`buffer就没有所对应的文件。

## 获得缓冲区 ##

如果想获得缓冲区本身，那么可以使用`current-buffer`。

``` emacs-lisp
(buffer-name) ;; => "《GNU-Emacs-Lisp编程入门》读书笔记.md"
(current-buffer) ;; => #<buffer 《GNU-Emacs-Lisp编程入门》读书笔记.md>
```
我们发现这两个函数的返回值挺相似的，但实际上他们却完全不同，`buffer-name`获取到的只是一个名称罢了，而`current-buffer`函数获取到的是一个名称所指向的对象或实体。举个生活上的例子:你对小明说:"小明，帮我拿个苹果"，然后小明就拿给了你"苹"和"果"字，显然这不是你真正想要的，你想要的是叫苹果的那个实体，可以吃的，有维生素的水果。经过这个例子，希望大家都能理解。 

## 切换缓冲区 ##

先介绍一个函数————`other-buffer`，这个函数用于发挥最近常打开过的buffer对象。例如最近常在`\*scratch\*`与`test.el`之间切换buffer，那么当前buffer为`\*scratch\*`时调用`(other-buffer)`时将会返回`test.el`的buffer对象。

再介绍一个函数————`switch-to-buffer`，这个函数接收一个`Buffer`对象的参量，可以切换当前buffer为`Buffer`参量。

``` emacs-lisp
(buffer-name) ;; => "《GNU-Emacs-Lisp编程入门》读书笔记.md"
(other-buffer) ;; => #<buffer *Backtrace*>
;; 根据最近最常打开过的buffer对象切换buffer
(switch-to-buffer (other-buffer))
(buffer-name) ;; => "*Backtrace*"
```
如果想要回到上一个buffer可以键入键序列`C-x b RET`，RET是回车。

前面几次提到`调用(call)`这个概念，实际上Lisp解释器对一个列表的首元素为一个函数进行处理时，就是在调用那个函数。

## 缓冲区大小和位点的定位 ##

先为大家介绍下四个函数:
  * `(point)`  
  ;; 获取当前光标在当前buffer的位点。
  * `(buffer-size)`
  ;; 获取当前buffer的字符数(包括空格)
  * `(point-min)`
  ;; 获取当前光标中位点的最小可能值。默认是1,除非设置了变窄,毕竟默认是从第一个字符开始
  * `(point-max)`
  ;; 获取当前光标中位点的最大可能值。默认是最后一个字符的point,除非设置了增宽。
  
``` emacs-lisp
;; 获取当前buffer的字符数(包括空格)
(buffer-size) ;; => 10636
;; 获取当前光标在当前buffer的位点。
(point) ;; => 10525
;; 获取当前光标中位点的最小可能值。默认是1,除非设置了变窄,毕竟默认是从第一个字符开始
(point-min) ;; => 1

;; 获取当前光标中位点的最大可能值。默认是最后一个字符的point,除非设置了增宽。
(point-max) ;; => 10670
```

## 练习 ##

找一个文件，对它进行操作，将光标移动到缓冲区的中间部分。找出它的缓冲区名、文件名、长度、和你在文件中的位置。

* 缓冲区名

``` emacs-lisp
(buffer-name) ;; => "《GNU-Emacs-Lisp编程入门》读书笔记.md"
```

* 文件名

``` emacs-lisp
(buffer-file-name) ;; => "/home/evanmeek/Documents/Blog/source/_posts/《GNU-Emacs-Lisp编程入门》读书笔记.md"
```

* 长度

``` emacs-lisp
(buffer-size) ;; => 11005
```

* 在文件中的位置

``` emacs-lisp
(point) ;; => 11033
```

# 第三章 如何编写函数定义 #
    
本章内容较多，不止于函数定义，还介绍了一些常用的函数。

## defun 特殊表 ##

`defun`不以通常的方式对它的参量求值，因为它是特殊表。

这节将简单描述函数定义的过程。

一个函数定义在`defun`之后最多可有5个部分:
* 符号名，这个符号指向了这个函数的定义。
* 一个列表，包含了要传给函数的参量。若没有任何参量要传递，那么可以写空列表。
* 这个函数的文档，由双引号括住，此选项为可选。
* 使当前函数成为一个交互式函数的列表，此选项为可选。交互式函数将会在后的小节讲到。
* 函数的主题，也就是一系列的命令。
下面是一个包含了五个部分的函数模板:

``` emacs-lisp
(defun funciton-name (arguments...)
  "optional-documentation"
  (interactive argument-passing-info) ;; optional
  body...)
```

我们通过这个模板写一个简单的函数吧，这个函数的功能呢是让其参量乘以7。

``` emacs-lisp
(defun multiply-by-seven (number)
  "使number(参量)乘以7"
  (* number 7))
```

这个函数的函数名就是`multiply-by-seven`，而函数名后面的列表也就是函数的参量，当我们调用函数时，传递给函数的参量的值就会被绑定到这个`number`上。并且`number`也可以为其他的名称，这取决于写代码的人，你可以改为`multiplicand`，这可能更符合函数定义的意思。并且这里的`number`的范围是仅在函数定义内才有效的，如果我们在函数定义外对`number`求值，可能会返回个错误信息。所以说，参量列表内的参量名可以是任意的，只要不与参量列表内的其他参量相同即可。这就比如：你班上的外号叫李大头，那么在这个班里的李大头代表的人就是你，而如果是在学校中，也有人的名字叫李大头，这时这个李大头就不代表你了。

跟随参量列表后的叫做函数文档，它的作用是当我们或任何人使用`C-h f`并键入函数名时所查看的函数帮助文档中的文本。这里还要注意一点，有些函数例如`apropos`在查看函数文档时会只显示文档的第一行，所以我们要在第一行就尽量写清楚这个函数的作用。通过`C-h f`查看函数文档将会弹出一个`\*Help\*`Buffer。

而这个例子的第三行就是函数的主体(往往比例子里多很多**，它是一个列表，功能是将`number`乘以7。

函数定义好了，但我们不要直接尝试调用，我先告诉你们调用的方式吧，**把函数名作为一个列表的首元素，其余的作为参量**最后eval即可。
``` emacs-lisp
(multiply-by-seven 7)
```
但可千万不要急着求值，因为肯定会报错。这主要是因为你只是写好了函数定义，并没有把函数定义给安装上。


## 安装函数定义 ##

将光标移动到我们写好的函数定义之后，eval一下即可安装好函数。安装完成后的函数就被包含在Emacs之中了，直到退出Emacs之前。如果已经尝试安装函数定义的童鞋应该已经发现了，在我们eval函数定义后，会在回显区显示函数名，这就代表我们成功安装了这个函数。

### 改变函数定义  ###

想要修改已经安装好的函数定义很简单，我们只需要重新安装一遍即可，例如我们想要将`multiply-by-seven`函数改为用`+`来实现，这样做:

``` emacs-lisp
(defun multiply-by-seven (number)
  "使number(参量)加7次"
  (+ number number number number number number number)) ;; => multiply-by-seven

(multiply-by-seven 7) ;; => 49
```
所以编写Emacs Lisp代码时的流程通常是:**写函数，装函数，测函数，改函数，装函数**。

## 使函数成为交互式函数 ##

交互式函数的特点是可以通过`M-x`调用，还可以通过键序列调用，例如常用的移动光标`C-n`就是一个交互式函数，并且交互式函数是不会自动将返回值输出至回显区的，因为让一个函数称为交互式函数通常被认为是想得到函数的附加效果，而不是返回值。

想要使一个函数称为交互式函数，可以在函数定义时，将`defun`特殊表开头的列表的第四个元素写成一个`interactive`特殊表开头的列表。例如:

``` emacs-lisp
(defun multiply-by-seven-interactive (number)
  "打印 number(参量) 乘以 7
这是个交互式函数"
  (interactive "p")
  (message "7 * %d = %d" number (* number 7))) ;; => multiply-by-seven-interactive
```

安装好这个函数后，可以有三种方式调用它:
* 对表达式`(multiply-by-seven-interactive 7**`求值
* 键入`C-u`然后输入一个数字作为函数的参量，键入`M-x`然后键入函数名最后键入`RET`。
* 还有一种暂时没搞成功，等学到16章的第7节就知道了。

**交互式函数不会自动将返回值打印至回显区** 并且当使用第二种调用方式时，没有在`C-u`之后键入数字的话将会以默认值4来替代。

### 交互的multiply-by-seven函数 ###

我们为`multiply-by-seven`函数定义了交互式版本，但没有解释其中的`(interactive "p")`列表是个什么东西，这里就说一下。

首先`(interactive "p")`中的`p`代表将接收一个前缀参量，这个前缀参量也就是我们通过`C-u 数字 M-x 函数名`这种方式调用时的`数字`了，例如这个数字是`5`那么就会把`5`传递给`number`参量，然后在`message`函数中的`(* 7 5)`求值，最终由于函数是一个交互式函数，所以函数不会自动将返回值输出到回显区中，而`message`函数的返回值原本是带双引号的，由于是交互式函数，我们要的只是函数带来的附加效果，而不是返回值。

### interactive函数的不同选项 ###

前面已经解释过`interactive`函数的`p`选项，其实`interactive`特殊表有20多个预定义的选项，我们可以结合多个选项使得将信息正确交互地选送给函数。

下面介绍两个选项，首先是`r`，**它可以让Emacs将位点所在区域的开始值和结束值作为函数的两个参量**:

``` emacs-lisp
(interactive "r") ;; => (13945 13978)
```

再则是`B`选项，**它可告诉Emacs用缓冲区的名字作为函数的参量**，它会让Emacs在小缓冲区提示用户输入缓冲区名字，并将跟在`B`后面的字符串作为提示，而且Emacs还会进行函数名的补全（按TAB）。

``` emacs-lisp
;; 会出来一个交互式的窗口
(interactive "B请输入Buffer名称:") ;; => ("test.el")
```

下面再看一个例子，可以获取任意Buffer所对应的文件路径以及位点的开始及结束点。

``` emacs-lisp
(defun get-buffer_name-point_start-point_end (buffer start end)
  "获取buffer所对应的文件以及位点的开始结束值"
  (interactive "B请输入Buffer名称: \nr")
  (message "Buffer-file:%s\npoint-start:%d\npoint-end:%d"(buffer-file-name (get-buffer buffer)) start end))
```

通过`interactive`的`B`选项获取任意buffer的名称，再通过`r`选项后去位点的开始和结束值。最后在输出时通过`get-buffer`获取Buffer对象，从而达到目的。

**`interactive`是支持控制字符的，例如上个例子中用到的`\n`。并且`interactive`还可以无参量，这样的话跟`mark-whole-buffer`函数差不多。如果`interactive`的选项不能满足你的要求，那么你可以将参量传递给`nteractive`做为一个列表。**

### 永久地安装代码 ###

前面我们写了那么多的函数，在我们没有关闭Emacs之前这些函数都是可以被我们随意求值的，但是当我们关闭Emacs之后再打开就必须重新安装函数才能求值了。所以有几种方式可以永久地安装函数或其他的代码。

* 如果代码作为个人使用，可以把函数定义的代码放到`.emacs`初始化文件中，当启动Emacs时，将会自动对`.emacs`文件中的代码求值。
* 如果代码很多，可以将函数定义存放至多个文件，然后使用`load`函数使得emacs对单独存放函数的文件求值。
* 如果是当前计算机其他用户需要使用的代码，可以将代码放入`site-init.el`文件中，这样使得所有的用户都可以使用你的代码。
* 如果你想让你的作为全世界的人使用，那么你可以将代码放到互联网上，或着给自由软件基金会发送一份拷贝，它有可能被加入下一个发行版本中。

**Emacs在过去的年代里成长的道路————奉献。**


## let函数 ##


`let`函数与`defun`函数一样，都是EmacsLisp中的特殊表。

`let`函数将一个符号附着到或者绑定到一个值上。这些被绑定的值是只能在`let`函数之内使用，就像说`defun`特殊表举的例子，两个同名的事物在不同的场景下代表了不同的意思，例如你的房子和你朋友的房子都是房子，你对你朋友说，你要粉刷房子，在你听来是要粉刷你家的房子，但你朋友有可能会认为你要粉刷他的房子。

而`let`函数就避免了这种问题，在`let`函数中绑定的变量都只能在`let`函数中访问，这就是局部变量的概念。

`let`函数跟`setq`以及`set`函数有个很大的区别就在于:

* `let`函数绑定的变量可有任意个 `setq`和`set`只能有一个
* `let`函数可以执行一个或多个列表。
* `let`函数绑定的变量都是局部的。

### let表达式的各个部分 ###

`let`表达式分为三个部分，第一个就是`let`符号，第二个是变量列表，第三个是主体。

**第二个部分的变量列表是由一个列表组成的，这个列表内可以是任意多个符号或任意多个由两个元素组成且第一个元素必定是符号的列表。***

**第三个部分是`let`表达式主体，由任意多个列表组成。**

`let`表达式的模板看起来像:

``` emacs-lisp
(let varlist body...)
```

其中变量列表的符号是由`let`特殊表赋初始化值的变量.符号本身的初始值是nil.而作为两元素列表的首元素的每一个符号被绑定到对第二个元素求值后的返回值.

所以一个简单的变量列表看起来可能是这样的:`(apple (pear 10))`.在这个例子中,`apple`的值是nil,因为我们没有为其赋予初始值.而`pear`的值为10.

那么当变量是由两个元素的列表组成,就可以是下面这样的模板:

``` emacs-lisp
(let ((variable value) (variable value) variable...)
  (body1)
  (body2)...)
```











### let表达式例子 ###

现在需要创建一些关于水果的栗子,不同的水果有不同的数量.

``` emacs-lisp
(let (apple (pear 10) (banana '4斤) watermelon)
  (message "苹果数量:%S\t梨子数量:%d\t香蕉数量:%s\t西瓜数量:%S" apple pear banana watermelon)) ;; => "苹果数量:nil	梨子数量:10	香蕉数量:4斤	西瓜数量:nil"
```

我们创建了两个初始值为nil的变量`apple`和`watermelon`,以及一个绑定到10的变量pear,还有一个绑定到`4斤`的变量banana.随后在`let`主体中将创建的变量都用`message`函数打印在回显区.

### let语句中的未初始化变量 ###

在`let`表达式的变量列表中,允许不给予变量初始化值,那么Lisp解释器将会为其默认绑定到nil值上.

并且,如果没有给予初始值,那么它可以称作:`作为独立的原子出现`.




## if特殊表 ##

`if`函数是除了`defun`/`let`特殊表之外的一个特殊表,它用于让计算机做一些判断的工作.

首先来看`if`函数的模板:

``` emacs-lisp
(if if-part
    then)
```
其工作方式是：首先测试`if-part`的返回值是否不为nil，如果不为nil，那么就执行`then`的表达式。

**通常会把then部分放在if-part的下一行，这样更有利于阅读。**

下面做一个简单的栗子:

``` emacs-lisp
(if (> 5 4)
    (message "5 比 4 大!")) ;; => "5 比 4 大!"
```
其中`>`函数测试它的第一个参量和第二个参量进行比较大小，如果第一个参量大于第二个参量则返回“真”，否则返回nil。

可往往在日常编码中，`if-part`的值不能直接确定，它可能是一个表达式，又可能是一个函数调用。

例如测试参量可能被绑定在函数定义的参量上:

``` emacs-lisp
(defun type-of-animal (animal-name)
  "根据animal-name打印信息到回显区
如果animal-name是符号'fierce则返回'tiger"
  (if (equal animal-name 'fierce)
      (message "It's a tiger")))
(type-of-animal 'fierce) ;; => "It’s a tiger"
```

**type-of-animal函数详解**

首先`type-of-animal`是包含了两个特殊表模板，分别是`defun`和`if`。`type-of-animal`是函数名，紧跟着函数名的是函数参量列表，然后跟着的是函数的文档。最后函数的主体内是一个if函数，if函数的的一个参量是一个列表，这个列表的首元素的符号是一个函数`equal`，在Lisp中，`equal`函数用于比较参量一和参量二是否相等，如果相等则输出`if`函数的第二个参量，也是一个列表，这个列表的首元素是符号且是函数`message`，将`It's a tiger`打印至回显区。

## if-then-else表达式 ##


其实`if-then-else`表达式就是在`if`特殊表之上加了一个参量，其他没什么不同，新增的参量是当参量一的值为nil时，对参量三求值。

它的函数模板可以写成这样:

``` emacs-lisp
(if true-or-false-test
    action-to-carry-out-if-the-test-returns-true
  action-to-carry-out-if-the-test-returns-false)
```

例如:

``` emacs-lisp
(if (> 4 5)
  (message "5 大于 4!")
(message "4 小于 5")) ;; => "4 小于 5"
```

通过这个例子，我们发现当`if`函数的第一参量值为nil时就会对第三参量求值。并且第三参量的缩进也要少于第二参量。

那我们现在将`type-of-animal`函数改造成:

```emacs-lisp
(defun type-of-animal (animal-name) ;second version
  "根据animal-name打印信息到回显区
如果animal-name是符号'fierce则输出'It's tiger!否则输出Tt not fierce"
  (if (equal animal-name 'fierce)
      (message "It's tiger")
    (message "It not fierce")))
(type-of-animal 'fierce) ;; =>"It’s tiger"
(type-of-animal 'notfierce)  ;; => "It not fierce"
```

## Lisp中的真与假 ##




Lisp中所有除了nil的都是真，nil也就是我们常说的假，nil有多种意思，一种就是nil代表假，另一种则是代表空列表，但往往都把空列表写成`()`，而当Lisp解释器对表达式求值时，只要值不是nil或空列表那么都是真，不管得到的值为一个数字或字符串或列表，它都代表真。

``` emacs-lisp
(if nil
    'true
  'false) ;; => false
(if t
    'true
  'false) ;; => true
```

真也有另外一种表示方式，在表达式求值后没什么可返回时就会返回`t`，例如:

``` emacs-lisp
(> 5 4) ;; => t
```

## save-excursion函数 ##

各位还记得什么是位点(point)，什么是标记(mark)吗?

Emacs是一个文本编辑器，主要还是与文本打交道，那么`save-excursion函数`就是与文本打交道的一个经典函数。

先让我们来回顾一下位点与标记:

* 位点(point)：及当前buffer中光标所在字符位置，例如当光标置于buffer的开头，那么point应该是1，若置于buffer的末尾，那么point就是当前buffer的字符数了。

* 标记(mark)：标记是buffer的另外一个位置，其中一个就是位点。在buffer中可以自定义很多标记，这样可以方便我们跳转至某个位点。设置标记的函数是`(set-mark-command)`，它绑定了一个键序列`C-SPC`，如果此时想要快速跳转至某个标记点，就可以使用命令`(exehange-point-and-mark`快速回到标记处，它绑定了一个键序列`C-x C-x`，并且还会将当初的位点设置一个标记，方便来回跳转。

**位点与标记之间的缓冲区叫做现域(region)**，有很多条命令是专用于操作现域的，例如:`center-region|count-lines-region|kill-region|print-region`

前面将了很多关于位点与标记的知识，下面正式介绍`save-excursion`函数，这个函数的作用主要是将位点和标记的当前位置保存，然后当其他会影响位点或标记的函数执行完后，再将标记的位点复原。 

`save-excursion`函数模板:

``` emacs-lisp
(save-excursion
  body...)
```
**注意，`save-excursion`函数的函数体允许有多条表达式，但是只返回最后一条表达式的值。**





## 回顾 ##

这一章学的东西还是挺多的，下面我就把书中的所有文字一字不差的抄下来。

先说几个提到但是没有过多解释的函数:

* eval-last-sexp
对光标所处的位点前的最后一个符号表达式求值。如果这个函数激活时没有参量，那么将返回值输出在回显区，否则将打印在当前缓冲区中。

* defun
定义函数。这个特殊表最多可有五个部分：函数名、传送给函数的参量的模板、文档、一个可选的交互函数声明以及函数体。

* interactiv
向解释器声明这个函数可以被交互的使用，并且这个特殊表还可以用一个字符串，分成单个或多个部分，依次传送信息至这个函数的参量。 

* let
声明在`let`表达式主体中使用的变量列表并且给它们赋初始值，初始值要么是nil，要么是一个指定的值，然后对`let`表达式主体的其他表达式求值并返回最后一个表达式的值。

* save-excursion
对这个函数主体求值前，记录位点和标记的值以及当前缓冲区。求值后恢复位点和标记。

* if
对函数的第一个参量求值，如果值为真，则对第二个参量求值，否则对第三个参量求值。

* equal、eq
测试两个对象的结构或内容是否相等用equal，测试两个对象是否完全相当用eq

* < > <= >=
用于判断第一个参量的值是否大于/小于/大于或等于/小于或等于第二个参量的值，如果是则返回真，否则返回nil

* message
用于往回显区内打印消息

* setq set
setq 用于将第一个参量的值绑定到第二个参量的值，而第一个参量的值由`setq`自动加上引用。
set 用于将第一个参量的值绑定到第二个参量的值，但是不会自动为第一个参量加上引用。

* buffer-name
这个函数用于获取一个缓冲区的名字

* buffer-file-name
这个函数用于获取缓冲区所对饮的名字

* current-buffer
这个函数用于获取当前缓冲区的buffer对象

* other-buffer
返回最近选择过的缓冲区

* switch-to-buffer
选择一个缓冲区

* set-buffer
设置当前缓冲区为某一个缓冲区

* buffer-size
返回当前缓冲区的字符数

* point
返回当前光标所对应缓冲区的位置

* point-min
返回当前缓冲区最小的字符数

* point-max
返回当前缓冲区最大的字符数





## 练习 ##

* 编写一个非交互的函数，这个函数将其第一个参量(是一个数)的值翻倍。然后使这个函数成为一个交互函数

非交互:

``` emacs-lisp
(defun double(number)
  "使number翻倍"
  (setq number (* number 2)))

(double 10) ;; => 20
```

交互式:

``` emacs-lisp
(defun double(number)
  "使number翻倍"
  (interactive "n请输入要翻倍的数字: ")
  (setq number (* number 2))
  (message "翻倍后:%d" number))
```

* 编写一个函数，测试`fill-column`的当前值是否大于传送给函数参量的值，如果是则打印适当的信息

``` emacs-lisp
(defun is-bigger-to-fill-column (number)
  "判断number是否比fill-column大"
  (if (> number fill-column)
      (message "%d比fill-column要大" number)
    (message "%d比fill-column要小" number)))

(is-bigger-to-fill-column 79) ;; => "79比fill-column要小"
(is-bigger-to-fill-column 81) ;; => "81比fill-column要大"
```


# 第四章 与缓冲区有关的函数 #

本章将会介绍大量的函数定义。

## 查找更多的信息 ##

可以使用`C-h f`查看一个函数所对应的文档，使用`C-h v`查看一个变量所对应的文档，如果要在源代码文件中查看函数定义，可以使用函数`find-tags`，跳到相应的位置。

Lisp代码可以分为多个模块或包，如果想要查看某个模块的帮助，可以键入`C-h p`。

## 简化的beginning-of-buffer函数定义 ##

对于`beginning-of-buffer`函数你们可能已经使用过了，其绑定的键序列是`M-<`。其作用是将当前光标移动至buffer的起始处。

下面我们将自己实现一个简单的`beginning-of-buffer`函数。

让我们来看看我们需要做什么事:

1. 首先这个函数得是个交互式函数，以便我们能通过键序列调用或用`M-x`调用。
2. 其次我们需要记录个位点为标记
3. 最后我们再跳转到buffer起始处

相比与真正的`beginning-of-buffer`函数定义，没有考虑一些复杂的选项，但是我们先完成这个简化版本吧!

``` emacs-lisp
(defun simple-beginning-of-buffer ()
  "移动光标至buffer开始处"
  (interactive)
  (push-mark)
  (goto-char (point-min)))
```

这个`defun`函数包含了5个部分:

1. 首先是这个函数的函数名————`simple-beginning-of-buffer`
2. 再就是函数的文档
3. 随后是交互式表达式
4. 然后记录位点为标记
5. 最后跳转至buffer的起始处 

由于这个函数是无参量的，所以交互式表达式内也不用写任何字符串，而`push-mark`函数默认将`point`加入到标记中，最后通过`goto-char`跳转至`point-min`的位置。如果想要回到原来的位置可以使用`C-x C-x`。

既然已经写了一个`simple-beginning-of-buffer`那我们也可以写一个`simple-end-of-buffer`吧!

``` emacs-lisp
(defun simple-end-of-buffer()
  "移动光标至buffer结束处"
  (interactive)
  (push-mark)
  (goto-char (point-max)))
```

最后则是，如果遇到不了解的函数，可以将光标放置函数之上，键入键序列`C-h f RET`即可。 

## mark-whole-buffer函数定义 ##

`mark-whole-buffer` 不比`simple-end-of-buffer`复杂多少。

这次我们来看一个函数的完整定义

这是书中的定义:

``` emacs-lisp
(defun mark-whole-buffer ()
  "Put point at beginning and mark at end of buffer."
  (interactive)
  (push-mark (point))
  (push-mark (point-max))
  (goto-char (point-min)))
```

书中讲不知道为啥函数体内的第一条表达式`push-mark`内还需要写`(point)`，但我现在再去看，这个函数已经改变了，并且也改掉了这个冗余的代码。

``` emacs-lisp
(defun mark-whole-buffer ()
  "Put point at beginning and mark at end of buffer.
If narrowing is in effect, only uses the accessible part of the buffer.
You probably should not use this function in Lisp programs;
it is usually a mistake for a Lisp function to use any subroutine
that uses or sets the mark."
  (declare (interactive-only t))
  (interactive)
  (push-mark)
  (push-mark (point-max) nil t)
  ;; This is really `point-min' in most cases, but if we're in the
  ;; minibuffer, this is at the end of the prompt.
  (goto-char (minibuffer-prompt-end)))
```

并且还改了一些代码，先不看`declare`，我们发现变化的有`(push-mark (point-max))`，最新的`mark-whole-buffer`函数中函数体第二个表达式多了两个参量，第二个参量代表如果值不为nil则显示`Mark set`。第三个参量代表如果在瞬时标记模式下，值不为nil则激活。

并且最后一条表达式也修改过了，原本是`point-min`但改为`minibuffer-prompt-end`，其区别在于:

`minibuffer-prompt-end会在返回buffer位置时输出信息至minibuffer`

在是如果当前buffer不是minibuffer那么就返回`point-min`




## append-to-buffer函数定义 ##

由于我看的这本书年代久远(2001)，现在是2019年，整整过去18年，Emacs也已经发生了巨大的变化。

所以我将先试着记录书中所讲的`append-to-buffer`函数再对Emacs 26.3版本的`append-to-buffer`函数进行讲解，希望对看到这篇博客的同仁们提供一些微薄的帮助。

> 这个计划已经鸽了，我发现用我现在所学还是很难解释清楚26.3版本的append-to-buffer函数的工作方式。所以后面的26.版 append-to-buffer会在以后完成，十分抱歉。

首先`append-to-buffer`函数的作用是将指定buffer区域(region)中的文本追加到当前Buffer的point前。

### 旧版append-to-buffer ###

先让我们看看书中的`append-to-buffer`函数定义:

``` emacs-lisp
(defun append-to-buffer (buffer start end)
  "Append to specified buffer the text of the region.
It is inserted into that buffer before its point.
When alling from a program, give three arguments:
a buffer or the name of one, and two character numbers
specifying the portion of the current buffer to be copied."
  (interactive "BAppend to buffer: \nr")
  (let ((oldbuf (current-buffer)))
    (save-excursion
      (set-buffer (get-buffer-create buffer))
      (insert-buffer-substring oldbuf start end))))
```

通过阅读这个函数的文档就能很清晰的了解这个函数的工作方式。

略过函数名和文档，我们直接来看`interactive`。`interative`中的`B`代表让用户选择一个Buffer(可能不存在)并将buffer名称传给函数参量一，其次是一些友好的文本`Append to buffer: `，紧跟其后的`\n`用于控制换行，最后的`r`获取当前区域(point和mark)，并将其传给函数的参量二和参量三。 

随后是一个`let`特殊表，在`let`的变量列表中，定义了一个`oldbuf`，其绑定的值是`(current-buffer)`的返回值，也就是当前Buffer对象。在`let`的表达式体中有一个`save-excursion`函数。

`save-excursion`函数体中有两条表达式，第一条表达式`(set-buffer)`用于将当前缓冲区变换到另外一个缓冲区，而其参量又是一个函数`get-buffer-create`，这个函数的作用是获取指定BUFFER对象或名(称为BUFFER-OR-NAME)，如果这个BUFFER-OR-NAME不存在，将会自动创建。

`save-excursion`函数体的第二条表达式是一个`(insert-buffer-substring)`函数这个函数将参量一从参量二到参量三的区域的字符串插入到当前Buffer的point之前。




### 26.3版append-to-buffer ###

由于本人水平有限，如有错误，欢迎提出issue.

先让我们看函数定义原型:

``` emacs-lisp
(defun append-to-buffer (buffer start end)
  "Append to specified BUFFER the text of the region.
The text is inserted into that buffer before its point.
BUFFER can be a buffer or the name of a buffer; this
function will create BUFFER if it doesn't already exist.

When calling from a program, give three arguments:
BUFFER (or buffer name), START and END.
START and END specify the portion of the current buffer to be copied."
  (interactive
   (list (read-buffer "Append to buffer: " (other-buffer (current-buffer) t))
	 (region-beginning) (region-end)))
  (let* ((oldbuf (current-buffer))
         (append-to (get-buffer-create buffer))
         (windows (get-buffer-window-list append-to t t))
         point)
    (save-excursion
      (with-current-buffer append-to
        (setq point (point))
        (barf-if-buffer-read-only)
        (insert-buffer-substring oldbuf start end)
        (dolist (window windows)
          (when (= (window-point window) point)
            (set-window-point window (point))))))))
```

我们发现函数参量没有变化，并且注释也没有大的变化，只是函数体内发生了较大的变化。

首先是`interactive`函数参量发生了变化，变成了一个`list`函数，回想一下，`list`函数是用于构造一个列表的函数，这个`list`函数的第一个参量是`(read-buffer)`函数。

`read-buffer`函数可以通过buffer名称读取buffer中的字符串并返回。其第一个参量是用于提示用户的友好信息，并且特意指出这个提示信息必须是由冒号和空格结尾且被双引号包围的字符串。第二个参量用于也就是返回值，它的默认值为列表




## 回顾 ##

惯例抄书

* describe-function\describe-variable
打印一个函数或一个变量的文档。通常将其绑定到`C-h f`和`C-h v`

* find-tag
找到存放某个函数或变量的源代码的文件，并切换到这个缓冲区，将位点(光标)置于相应函数或这变量的开始处。习惯上将其绑定到`M--`。

* save-excursion
保存位点和标记的位置，并在对`save-excursion`参量求值之后恢复这些值。它也保存当前缓冲区并返回到该缓冲区。

* push-mark
在指定位置设置一个标记，并在标记环中记录原来标记的值。标记是缓冲区中的一个位置，即使由一些文本被从缓冲区删除或者增加到缓冲区，标记仍将保持它的相对位置。

* goto-char
将位点设置为由参量指定的位置。参量值可以是一个数，也可以是一个标记，甚至可以是一个返回一个位置数字的表达式(point-min)

* insert-buffer-substring
将来自一个缓冲区（这是被作为一个参量而传递给函数的）的文本域拷贝到当前缓冲区

* mark-whole-buffer
将整个缓冲区标记为一个域。一般将这个函数绑定到`C-x h`。

* set-buffer
将Emacs的注意力转移到另一个缓冲区，但是不该便显示的窗口。 

* get-buffer-create\get-buffer
寻找一个已指定名字的缓冲区，或当指定名字的缓冲区不存在时就创建它。如果指定名称的缓冲区不存在，get-buffer函数就返回nil。

## 练习  ##

* 编写自己的`simplified-end-of-buffer`函数定义，然后测试它是否能工作。

``` emacs-lisp
(defun simple-end-of-buffer()
  "移动光标至buffer结束处"
  (interactive)
  (push-mark)
  (goto-char (point-max)))
```

* 用if和get-buffer编写一个函数，这个函数要打印一个说明某个缓冲区是否存在的消息 

``` emacs-lisp
(defun is-exist-buffer (buffer-or-name)
  "判断buffer-or-name是否为一个已存在的buffer"
  (if (bufferp (get-buffer buffer-or-name))
      (message "%s存在!" buffer-or-name)
    (message "此缓冲区不存在!")))
    
(is-exist-buffer "test.el") ;; => "test.el存在!"
```

* 用find-tag找到copy-to-buffer函数的源代码

不会...

# 第五章 更复杂的函数

本章我们将在已学内容的基础之上学习更复杂的函数，例如有使用两次`save-excursion`的
`copy-to-buffer`函数，以及一个在`interactive`中使用`*`和`or`函数的函数。

## copy-to-buffer函数的定义

`copy-to-buffer`函数与前面学过的`append-to-buffer`的定义很类似。

`copy-to-buffer`是替换指定`BUFFER`的内容，而`append-to-buffer`是在指定`BUFFER`中追加内容。

首先来看看`copy-to-buffer`的函数定义:

``` emacs-lisp
(defun copy-to-buffer-t (BUFFER START END)
  "docutments..."
  (interactive "BCopy to buffer: \nr")
  (let ((oldbuf (get-buffer-create BUFFER)))
    (save-excursion
      (set-buffer BUFFER)
      (save-excursion
        (insert-buffer-substring oldbuf START END)))))
```

略过函数名与文档不看，可以看到这个函数被定义为交互式函数，并且这个函数的参量要求
是一个BUFFER对象，以及两个表示位置的数字。这三个参量都由`interactive`所解决，其
使得让用户选择一个BUFFER，然后获取当前`Buffer`的`point`和`mark`，作为`START`和
`END`。

随后是一个`let`表达式，其在它的`varlist`部分将`BUFFER`参量通过
`get-buffer-create`函数获取了其对象(就算`BUFFER`参量的值不存在也会创建一个)，并
且将这个BUFFER对象赋值给`oldbuf`。在`let`表达式的`BODY`部分，出现了一个
`save-excursion`函数，这个函数用于记录当前`point`和`mark`的位置，然后在其参量求
值完毕后恢复记录的位置，这个参量也就是`set-buffer`，这个函数用于改变当前BUFFER为
  参量`BUFFER`。随后又是一个`save-excursion`函数，其参量
  `insert-buffer-substring`函数我们也了解过，其用于将当前BUFFER的START到END之间
  的区间插入到`oldbuf`内。

## insert-buffer函数的定义

前面我们用过`append-to-buffer`以及`copy-to-buffer`，他们都是将当前`BUFFER`的内容
拷贝或追加到某一个`BUFFER`中，而`insert-buffer`可以将当前`BUFFER`的内容，拷贝至
一个已存在的`BUFFER`当中。

先来看看其函数定义:

``` emacs-lisp
(defun insert-buffer-t (buffer)
  "docutmens.."
  (interactive "*bInsert buffer: ")
  (or (bufferp buffer)
      (setq buffer (get-buffer buffer)))
  (let (start end newmark)
    (save-excursion
      (save-excursion
        (set-buffer buffer)
        (setq start (point-min) end (point-max)))
      (insert-buffer-substring buffer start end)
      (setq newmark (point)))
    (push-mark newmark)))
```

先简单整理一下这个函数都包含了什么东西:

- 一个参量
- 函数文档
- 定义交互式函数
- `interactive`参量说明有\*和 `b`
- 以及一个`or`函数，这个函数内有两个参量
- 第一个参量是`bufferp`函数
- 第二个参量是`setq`函数，`setq`的第二个参量是`get-buffer`函数。
- 随后是一个`let`表达式
- `let`表达式中先是初始化了三个空变量
- 一个外层`save-excursion`函数
- 一个内层`save-excursion`函数
- 其第一个参量是一个`set-buffer`函数
- 第二个参量是`setq`函数
- 外层`save-excursion`的第二个参量是一个`insert-buffer-substring`函数
- 第三个参量是一个`setq`函数。
- 最后是一个`push-mark`函数。 

### insert-buffer函数中的交互表达式

让我们先从`interactive`表达式说起，首先起表达式说明分为三个部分：

1. `*`代表只读缓冲区，这个说明会在当`b`说明返回的buffer是一个只读缓冲区时在回显
   区提示错误。也就是说当这个`insert-buffer`函数当在一个只读缓冲区被调用时，将不
   被允许。
2. `b`代表要求是一个存在的缓冲区或者是缓冲区名，它与`B`说明不同。

3. `Insert buffer: `是友好的提示。

**提示:\*控制符无需后接一个换行符来分割不同的参量。**

### insert-buffer函数体

主要有两个部分，分别是`or`函数和`let`函数。

先让我们来看看`or`函数，其第一个参量是一个`bufferp`函数，这个函数用于当其参量是
是一个已存在的buffer或buffer的名称才会返回`non-nil`的值，也就是真/非假，其第二个
参量是一个`setq`函数，里面有一个`get-buffer`函数，这个函数是用于获取一个已存在的
buffer对象根据buffer对象或buffer的名称，并将buffer绑定到`get-buffer`的值之上。

其实这个函数的意思是，要让`buffer`参量确定是一个已存在的buffer，我们可以用`if`函
数重写一遍。

### 用if表达式编写insert-buffer函数

我们的需求是，必须确保`buffer`的值是一个已存在的buffer或buffer的名称。

``` emacs-lisp
(if (not (bufferp buffer)
         (setq buffer (get-buffer buffer))))
```

就可以这样写，当`bufferp`函数的值为nil时，那么就会尝试获取`buffer`的对象并且保存，
否则则报错。


### insert-buffer函数中的let表达式

在我们确保`buffer`参量是一个非只读缓冲区后，可以开始写拷贝内容的代码了。

首先我们初始化三个空变量: start end newmark

  `let`表达式体中有一个外层`save-excursion`函数，其记录了我们的`point`和`mark`，
其第一个参量又是一个`save-excursion`函数，我们称为内层`save-excursion`。内层
`save-excursion`主要做了两件事，首先是将Emacs的注意力转移到`buffer`之上，随后为
`start`以及`end`附上`buffer`的`point-min`和`point-max`的值，并且由于内层
`save-excursion`已经求值完毕，那么会恢复在求值过程中可能变动的位点和标记的值。随
后外层`save-excursion`函数的第二个参量是将`buffer`参量的内容插入到当前buffer从
`start`到`end`结束的内容，这个`start`和`end`也就是`buffer`参量所有的内容。随后又
把`newmark`变量绑定到值`point`之上，最后将`newmark`记录为`标记`。

## beginning-of-buffer函数的完整定义

前面我们尝试写了`beginning-of-buffer`函数的部分定义，它是一个无参量的函数，那么
这次我们来写一个有参量的`beginning-of-buffer`函数。

这个带参量的`beginning-of-buffer`函数可以指定在当前缓冲区的几分之几标记位置。

那么现来说下需求:`beginning-of-buffer`函数接受一个可选参量，这个参量的返回是1-10
之间，这个参量作为标记点的位置。

### 可选参量

在需求里我们提到: 

> `beginning-of-buffer`函数接受一个可选参量

除非特别声明，否则Lisp会认为函数的参量是必须在函数被调用时传递一个值给该参量的。
如果不传入参量，则该函数就会出错:`Wrong number of arguments`。

而如果需要使一个或多个参量变为可选参数只需要在参量前加上`&optional`关键字，例如:

``` emacs-lisp
(defun beginning-of-defun-function-t (&optional arg1 arg2)
  "documentation"
  (interactive "P")
  (push-mark)
  (goto-char
   ;; if-there-is-an-argument
   ;;   xxxx
   ;; else-go-to
   (point-min)))
```

对比前面的`simple-beginning-to-buffer`函数，好像唯一多的地方是在`goto-char`函数
的第一个参量，变成了`if`特殊表。这个`if`函数判断由`interactive`的`P`参量获得到的前
缀参量是否为一个非nil值，如果是则执行`if then`部分，否则执行跟
`simple-beginning-to-buffer`一样的操作。

### 带参量的beginning-of-buffer函数 

`goto-char`函数中有一个`if`表达式，这个表达式做了很多关于算术的操作。 

 ``` emacs-lisp
(defun beginning-of-defun-function-t (&optional arg)
  "documentation"
  (interactive "P")
  (push-mark)
  (goto-char
   (if (> (buffer-size) 10000)
       (/ (prefix-numeric-value arg) 10)
     (/
      (+ 10
         (*
          (buffer-size) (prefix-numeric-value arg))) 10))
   (point-min)))
 ```
 
 一眼看上去有些复杂，其实我们通过函数模板来揭开其中的奥秘，十分简单。
 
我们看将`if`函数的结构看成这样:

``` emacs-lisp
(if (buffer-is-large
     divide-buffer-size-by-10-and-multiply-by-arg)
    else-use-alternate-calculation)
```

要吃饭了

这里的`if`函数用于检查缓冲区的大小，这是因为书中使用的是第18版本的Elisp，其使用
了不大于800w的数字来描述缓冲区的大小，如果有在某次求值中遇到更大的缓冲区，Emacs
就会试图用更大的数来描述它(通常叫做overflow溢出)。

那么现在就有两种情况了，分别是超大缓冲区和正常的缓冲区，这两种不同数量级的缓冲区
有不同的处理方式，先来看看那超大缓冲区的处理方式

当`(if (> (buffer-size) 10000))`时，执行`if`表达式的`then`部分，这部分内有一个
`prefix-numeric-value`用于将从`(interactive "P")`读入的参数转为一个数字，也就是
把`arg`的值转为数字，并且将其除以10，這樣可以讓產生的數比緩衝區中相應的比例多只
多一個字符。 



### 完整的beginning-of-buffer函數

``` emacs-lisp
(defun beginning-of-buffer-t (&optional arg)
  "Move point to the beginning of the buffer;
leave mark at previous position.
Wwith arg N, put point N/10 of the way from the true beginning.
Don't use this in Lisp programs!
\(goto-char (point-min)) is faster
and does not set the mark."
  (interactive "P")
  (push-mark)
  (goto-char
   (if arg
       (if (> (buffer-size) 10000)
           ;; Avoid overflow for large buffer sizes!
           (* (prefix-numeric-value arg)
              (/ (buffer-size) 10))
         (/ (+ 10 (* (buffer-size)
                     (prefix-numeric-value arg)))
            10))
     (point-min))) (if arg (forward-line 1)))
```


## 回顧

慣例抄書

* or

逐一對每一個參量求值，並返回第一個非空值（不是nil)。如果所有參量的值都是nil，則
返回nil。

* and

逐一對每一個參量求值，如果有任意一個參量的值爲nil，則返回nil，並且隨後的參量不會
進行求值。

* &optional

指定函數定義時參量爲可選項，如果有任意參量前有&optional則表明當前且隨後的參量都
是可選參量。

* prefix-numberic-value

將由 `(interactive "P")`產生的尚未加工的前綴參量轉換成一個數值。

* erase-buffer

刪除當前緩衝區的所有內容 

* bufferp

如果參量是一個buffer對象則返回真，否則返回 nil。






## &optional 參量練習

題目

> 編寫一個帶可選參量的交互參量，這個函數要測試函數被調用時是否有參量（其值是一個
> 數），這個數是否大於或小於fill-column的值，並將結果以一個消息的形式給出。然而，
> 如果不帶參量調用這個函數時，則使用56作爲默認值。

解答

``` emacs-lisp
(defun bigger-fill-column (&optional arg)
  "判斷是否大於fill-column
ARG爲可選參量，默認值爲56"
  (interactive "P")
  (if arg
      (if (> arg fill-column)
          (message "%d大於fill-column的值" arg)
        (message "%d小於fill-column的值" arg))
    (if (< 56 fill-column)
        (message "fill-column的值大於於56"))))
```

# 第六章 變窄和增寬

變窄(narrowing)是Emacs的一個特性，它的作用是使得當前buffer在開啓變窄後將變窄範圍
之外的內容屏蔽。

上面提到的屏蔽不僅有視覺效果而且還對EmacsLisp解釋器有效。

## save-restriction特殊表

前面我們學習過`save-excursion`函數，其作用是記錄當前位點和標記的位置，而對於變窄，
Emacs提供了一個`save-restriction`函數，其作用是記錄當前變窄的標記位置，當其表達
式內參量求值完成後再恢復記錄過的變窄的標記位置，這麼做可以讓當遇到取消變窄的表達
式後可以恢復。

其函數模板見:

``` emacs-lisp
(save-restriction body...)
```

**注意:**

如果同時連續使用`save-excursion`函數和`save-restriction`函數時必須將
`save-excursion`函數放在`save-restriction`之前，類似於:

``` emacs-lisp
(save-excursion
  (save-restriction))
```

如果位置反過來則會出現`save-excursion`無法記錄變窄範圍之外的標記。


 



## what-line函數

這個函數是一個典型的同時使用`save-restriction`和`save-excursion`的例子，其作用是
返回當前光標所在當前buffer的行數。

先看看它的實現:

``` emacs-lisp
(defun what-line ()
  "Print the current line number (in the buffer) of point."
  (interactive)
  (save-restriction
  (widen)
  (save-excursion
    (beginning-of-line)
    (message "Line %d"
    (1+ (count-lines 1 (point)))))))
```

首先看到`widen`函數，這個函數可以取消變窄開啓，但是它被`save-restriction`包住，
這意味着就算取消了變窄開啓，`save-restriction`也可以保證變窄範圍不會變化.

再看到`beginning-of-line`，這個函數會移動point到當前行的首個字符.明顯這個函數改
變了`point`值，不過由於`save-excursion`的出現，這些改變位點和標記的操作都將會被
恢復。

前面所做的類似`widen`和`beginning-of-line`都可以說是爲了後面真正求出行數的計算做
的準備。


現在再讓我們看向`(message)`函數，裏面只是簡單的做了一個友好提示以及格式化，隨後
讓我們注意`(1+ (count-lines 1 (point)))`，`count-lines`的作用是計算其第一個參量
到第二個參量之間的總行數，至於`1+`是爲了處理一些疑難雜症。

## 練習：變窄

題目

> 編寫一個函數，這個函數在即使設置了變窄開啓而使緩衝區的前一半不可見的情況下也能
> 顯示出當前緩衝區的頭60個字符。要在顯示完成之後恢復位點、標記和變窄開啓等相關設
> 置。對於這個練習題，要使用`save-restriction`、`widen`、`goto-char`、
> `point-min`、`buffer-substring`、`message` 和其他函數，真可以算得上是一個大雜
> 燴。

解題

``` emacs-lisp
(defun print-60-char ()
  "輸出當前buffer前60個字符
變窄範圍之外也算"
  (interactive)
  (save-restriction
    (widen)
    (save-excursion
      (message "%s"
               (buffer-substring
                (point-min)
                60)))))
```

# 第七章 基本函數: car、cdr、cons

本章學習car、cdr、cons等函數，這三個函數都是Lisp中非常基礎的函數。 

`car`

> Contents of the address part of the Register.

`cdr`

> Contents of the Decrement part of the Register.

`cons`

> construct
    
## car和cdr函數 

我們見過類似這樣的列表:

``` emacs-lisp
'(cat dog tiger lion)
```

而這個列表的car也就是`cat`，car函數用於獲取列表的第一部分(也就是第一個符號)。

這個列表的cdr是`(dog tiger lion)`，cdr函數用於獲取列表的第二部分(除了首符號之外
的符號)。

**注意:**

- 這裏由cdr獲取的部分是一個列表，而`car`獲取的部分只是一個符號罷了。

- `car`和`cdr`函數都是非破壞性的，也就是說他們不會對操作對象有任何修改操作，只是
  讀取。








## cons函數

`cons`函數用於構造新列表，例如想要爲`(cat dog tiger lion)`中添加一個`panda`可以
這麼寫:

``` emacs-lisp
(cons 'panda '(cat dog tiger lion)) ;; => (panda cat dog tiger lion)
```

**注意:**

`cons`函數不能憑空構造一個新列表，**其必須有一個待插入新元素的列表**。

``` emacs-lisp
(cons 'panda ()) 
(cons 'water '(milk nongfu_spring)) ;; => (water milk nongfu_spring)
```

### 查詢列表的長度: length函數

`length`函數可以獲取列表的長度，也就是元素的個數。

``` emacs-lisp
(length '(a b c d)) ;; => 4
```

也可以查詢空列表的長度:

``` emacs-lisp
(length ()) ;; => 0
```

但是此函數必須有一個列表對象的參量:

``` emacs-lisp
(length ) ;; => eval: Wrong number of arguments: length, 0
```




## nthcdr函數

`nthcdr`函數於`cdr`函數是有聯繫的，試想下如果你想要通過`cdr`函數獲取列表`(cat
dog lion)`的最後一個元素，你可以這麼做:

``` emacs-lisp
(cdr (cdr '(cat dog lion))) ;; => (lion)
```

但假如這個列表的長度有10000，你想獲取最後一個元素還是用這種方式將會十分麻煩，這
時候就可以使用`nthcdr`函數了，它可以重複`cdr`函數的操作。

``` emacs-lisp
;; 返回移出前五個元素的列表
(nthcdr 5 '(car dog lion tiger panda fish)) ;; => fish
;; 返回移出前三個元素的列表
(nthcdr 3 '(car dog lion tiger panda fish)) ;; => (tiger panda fish)
;; 留下全部列表
(nthcdr 0 '(car dog lion tiger panda fish)) ;; => (car dog lion tiger panda fish)
```

**注意:**

當`nthcdr`第一個參量大於等於列表長度時將永遠得到`nil`值。

`nthcdr`函數也是不據破壞性的。


## setcar函數

`setcar`函數有些類似`cons`函數，只不過其會改變列表的值，也就是說具有破壞性的。

`setcar`將某個值替換某個列表的第一部分。

``` emacs-lisp
(setq animal '(cat dog lion riger panda fish)) ;; => cat dog lion riger panda fish

(setcar animal 'cow) ;; => cow

animal ;; => (cow dog lion riger panda fish)
```

於`setcar`類似的還有一個`setcdr`函數，其作用是將列表的第二個參量設置爲第一個參量
的第二部分。

``` emacs-lisp
(setq animal '(cat dog lion riger panda fish)) ;; => cat dog lion riger panda fish

(setcdr animal '(a b c d)) ;; => (a b c d)

animal ;; => (cat a b c d)
```






## 練習

題目

> 通過對幾個cons表達式求值，來構建一個四元素的，有關鳥的列表。試一試，當你對列表
> 本身使用cons函數時會發生什麼？用一種魚取代這個列表的第一個元素。用其他魚的列表
> 取代這個列表的其餘部分。

解題

``` emacs-lisp

;; 鳥列表
(setq fishes
      (cons 'sparrow
            (cons 'owl
                  (cons 'kingfisher
                        (cons 'ostrich ())))))

;; 魚取代鳥列表第一部分
(setcar fishes '多寶魚)
;; 魚列表取代鳥列表第二部分 
(setcdr fishes (cons '(甲魚 中華鱘) ()))

fishes
```

# 第八章 剪切和存儲文本

> 本人又可以用回简体中文啦！ 而且词库还增加了！可以打字更快了呢！

首先需要先引入一个概念————在Emacs中有一个kill环，这个环记录了kill的数据，在这里
的kill不代表“杀死”，而是clip的意思。

## zap-to-char函数

书中讲了zap-to-char函数的两个版本，分别是第19版和第18版。 

先让我们来看啊可能`zap-to-char`函数的第`19版本`。

``` emacs-lisp
(defun zap-to-char (arg char) ;; version 19 implementation
  "Kill up to and including ARG'th occurrence of CHAR.
Goes backward if ARG is negative; error if CHAR not found."
  (interactive "*p\ncZap to char: ")
  (kill-region (point)
               (progn
                 (search-forward
                 (char-to-string char) nil nil arg))
               (point)))
```

简单的描述下这个函数做了什么:

这个函数的功能是将当前位点到指定字符之间的字符串剪切到kill环内，其ARG的作用是指
定指定字符的次数(可能会有多个指定字符，所以可以选择是哪一个)。

例如有这么一段文字:`Thus,if the cursor were are begining of this sentence...`

我们的光标处于`if the`的`t`之上，当我们通过传递前缀参量`2`给`zap-to-char`函数，
并且指定`zap-to-char`函数的`CHAR`参量为`s`就会发现`the cursor were are beginning
of this`被截取了，并且通过`C-y(Yark)`可以将剪切掉的文本找回。

### interactive 表达式

让我们看看`interactive`表达式:

``` emacs-lisp
(interactive "*p\ncZap to char: ")
```

双引号部分的作用:

-r `*`代表只读缓冲区，也就是说当我们尝试对一个只读缓冲区使用`zap-to-char`函数将会
  获得一个报错信息。
- `p`代表这个函数可以传递一个数值类型的前缀参量。
- `\n` 换行
- `c`代表这个函数会提示输入一个字符类型的值。
- `Zap to char: `一个友好的提示，冒号后面的空格只是让格式更好看罢了。

**通过分析我们大概能知道，这个`interactive`表达式可以做到 监测是否为只读缓冲区，并
且支持通过前缀参量传递数值类型的值，在使用这个函数时还会提示输入一个字符。**

### zap-to-char函数体

函数体内通过几行简短的代码完成将当前位点到指定字符之间的文本添加到kill环内并删除
的操作。

跳过`interactive`表达式，可以看到所有的代码都被一个`kill-region`函数给包裹起来了，
其第一个参量是`(point)`，也就是当前的位点位置，第二个参量是`progn`函数，要讲解这
个函数的作用先让我们看看`search-forward`函数。

### search-forward函数

或许你曾经使用过`search-forward`函数，它接收四个参量:
- 第一个参量是要查找的字符
- 第二个参量(可选)是指定查找范围，可以绑定当前Buffer的一个位置.
- 第三个参量(可选)是当查找不到时，可以指定一个返回值，如果为t，则再出错时只返回
  一个nil，如果既不为nil也不为t，则使搜索改变，并且返回一个nil，如果为nil则返回
  出错信息。
- 第四个参量(可选)是重复计数值————待查找字符出现的次数，如果值为负数则从后查找。默认值为1。

**注意:**

找到某个字符后位点也会随之改变。

### progn函数

`progn`函数很简单，它的作用就是将任意多个参量进行一一求值，并且返回最后一个参量
的值。

在这个函数内将`kill-region`所需的第二个参量获取出来了。

首先使用`search-forward`函数将位点改变到要查找的字符上，然后再对`(point)`求值，
利用自身特性返回最后一个参量的值，从而让`kill-region`函数正常工作。

### 总结zap-to-char函数

了解了`search-forward`以及`progn`函数后我们大致能了解`zap-to-char`函数的工作方式
了。

其中`kill-region`作用是将某个区域的字符添加到kill环，并且删除，其接收两个参量，
第一个参量作为区域的开始，第二个参量作为区域的结束，区域开始很容易获取，通过对
`(point)`求值可以得到，而区域的结束也就是我们要查找的字符的位置，首先通过
`progn`函数将多条函数放在一个函数内，其中里面的`search-forward`将位点改变为待查
找的字符上，由于`search-forward`函数的第一个参量必须要是一个字符串类型的值，所以
我们将通过`interactive`函数获取的字符由`char-to-string`转化为字符类型，其次由于
位点改变后我们再对`(point)`求值，可以获取区域结束的位置了。


### 第18版本的zap-to-char函数

> 个人感觉18版本的zap-to-char很复杂，但是也很精妙的体现出Emacs开发者的智慧。

**第18版本的zap-to-char函数跟第19版本的区别在于，第18版本的剪切区域是去除待查找字
符本身的。**

先让我们看看代码:

``` emacs-lisp
(defun zap-to-char-18 (arg char)
  (interactive "*p\ncZap to char: ")
  (kill-region (point)
               (if (search-forward
                    (char-to-string char) ;; target
                    nil ;; bind buffer position
                    t ;; return 
                    arg) ;; repetition count
                   (progn
                     (goto-char
                      (if (> arg 0)
                          (1- (point))
                        (1+ (point))))
                     (point))
                 (if (> arg 0) ;; else-part
                     (point-max)
                   (point-min)))))
```

18版本的`zap-to-char`看起来更加复杂，并且功能也更多。

首先，两个版本的主要作用基本相同，都是为了将当前位点到指定字符的位置之间的区域剪
切，18版本相比较于19版本，多了几个特性:

- 不会剪切指定字符(忽略指定的字符，其他的剪切)
- 当指定字符不存在将会改变位点为buffer的位点最小值或最大值。 

下面让我们来分析下吧!

看到`interactive`表达式，并没有任何改变，这里可以忽略。而`kill-region`的第一个参
量仍然是当前位点，后面则有巨大的改变。第二个参量被一个`if`表达式包裹住，这个
`if`表达式的CONS部分是一个`search-forward`函数，其作用是判断指定字符是否存在，如
果存在则执行THEN部分的`progn`表达式。

### progn表达式主体

`progn`函数的表达式主体内有两个参量，根据`progn`函数的特性，我们知道，只有最后一
个参量会被返回，先看看第一个参量，  是一个`goto-char`函数，让我们回顾下18版本的
特性，其在剪切时会忽略指定的字符，那么这个`goto-char`函数就是做了这么件事，当ARG
大于0时(往前搜索时字符出现的次数)，那么就将位点往后挪一，反之加一。最后由于改变
了位点， 并且`progn`函数的第二个参量是`(point)`又将改变后的位点返回，也就成了
`kill-region`函数的结束区域了。

最后是`if`表达式的ELSE部分，其作用是当为查找到指定字符时，将会根据ARG的值来判断
剪切到最后还是最前。





## kill-region函数

在前面的`zap-to-char`函数使用了`kill-region`函数，现在让我们来康康这个函数的内部
实现吧！

``` emacs-lisp
(defun kill-region-19 (begin end)
  "Kill between point and mark.
The text is deleted but saved in the kill ring."
  (interactive "*r")
  (copy-region-as-kill begin end)
  (delete-region begin end))
```

**注意: 看注释**





## delete-region函数:接触C
  

`kill-region`函数的`delete-region`函数是一个由C语言宏编写的函数，让我们看看它的
第一部分:

``` c++
DEFUN ("delete-region", Fdelete_region, Sdelete_region, 2, 2, "r",
       doc: /* Delete the text between START and END.
If called interactively, delete the region between point and mark.
This command deletes buffer text without modifying the kill ring.  */)
  (Lisp_Object start, Lisp_Object end)
{
  validate_region (&start, &end);
  del_range (XINT (start), XINT (end));
  return Qnil;
}
```

这里不过多解释(书中没讲，本人也不太了解)

这个宏可以分为7个部分分别是:

- Lisp 中的函数名，也就是被双引号括住的"delete-region"。
- C语言中的函数名，也就是`Fdelete_region`。
- C常数结构名，`Sdelete_region`。
- 第四和第五部分是指明函数参量数目的最小和最大值。
- 第六部分就像interactive函数的说明表达式一样， 这里是"r"，代表函数的两个参量是
  某个缓冲区中某个区域的开始和结束。
- 第七部分是文档字符串，跟Emacs Lisp的函数文档不同在于换行时必须显式的写出`\n`。

再后面就是真正的函数参量，并且每个参量所对应的数据类型都有一段解释。

下面则是函数的主体了，其中`validate-region`函数用于检查参量的值的类型，而
`del-range`函数则是用于删除区域内字符的函数。
  
## 用defvar初始化变量 

`defvar`用于初始化变量，想必有些小伙伴会想到`setq`，`defvar`与其的区别在于:

- `defvar`只能为无值的变量赋值。
- `defvar`如果在为一个有值的变量赋值时将不会覆盖。
- 由`defvar`设置的变量可以给予变量文档。

``` emacs-lisp
(defvar box nil
  "Just a test box")
box ;; => nil
(defvar box 20
  "Just a test box")
  box ;; = > nil
```

不是说由 `defvar`定义的变量就不能够重新赋值了，其实是可以的，但是必须在变量文档
前加上`*`，并且由`setq`变量重新赋值。

``` emacs-lisp

(defvar t-box nil
  "*Just a test box")
t-box ;; => nil
(defvar t-box 20
  "*Just a test box")
t-box ;; => nil
(setq t-box 20)
t-box ;; => 20
```

## copy-region-as-kill函数 

> 这一小节我鸽了好久，原因是内容有点多，我懒得写。。

> 好了，让我们继续写吧(刚刚看了欧阳娜娜的Vlog，感觉真好)！

先让我们看看这个函数的源码:

``` emacs-lisp
(defun copy-region-as-kill (begin end)
  "Save the region as if killed, but don't kill it."
  (interactive "r")
  (if (eq last-command 'kill-region)
      ;; then-part: Combine newly copied text with previously copied text
      (kill-append (buffer-substring begin end) (< end begin))

``    ;; else-part: Add newly copied text as new element
``    ;; to the kill ring and shorten the kill ring if necessary
    (setq kill-ring
          (cons (buffer-substring begin end) kill-ring))
    (if (> (length kill-ring) kill-ring-max)
        (setcdr (nthcdr (1- kill-ring-max) kill-ring) nil)))
  (setq this-command 'kill-region)
  (setq kill-ring-yank-pointer kill-ring))
```

由于书上的内容我是前几天看的，而我每次写笔记又是直接根据源码来解读的，所以跟书上
有些表达不同（甚至有错误），希望各位能理解（有没有人看都是个问题..)。

根据这个函数的文档可以知道，这个函数的作用是:如果某个区域被剪切过了那么就将其保
存，但是并不再一次剪切。

`copy-region-as-kill`函数接收两个参量，它们分别代表了两个位置，通过这两个位置可
以确定一个区域。

### copy-region-as-kill函数体

把视野转向`interactive`函数，其表达式只有一个`r`，它的作用是：代表这个函数接收一
个`region`，也就是把`point`和`mark`作为这个函数的两个参量以数值类型传递过去。

随后让我们看到`if`特殊表，它几乎涵盖了整个函数体，下面我们来看看其`CONS`部分的作
用。`CONS`部分如下:

``` emacs-lisp
(eq last-command 'kill-region)
```

这里有一个我们没有遇到过的函数————`eq`，其作用跟`equal`函数类似，用于比较，但
`eq`函数比较的是其参量的对象是否相同，然而`eqaul`函数用于判断其参量的内容或结构
是否相同。

这里就是将`last-command`与`kill-region`进行比较，其中`last-command`是一个变量，
其作用是记录最后一个执行过的命令。在这个例子中就是将 Emacs最后一次执行的命令与
`kill-region`进行对象比对，测试其是否为同一个对象。换种说法就是`if`特殊表判断最
后一次执行的命令是否为`kill-region`。

如果是`kill-region`那么会发生什么呢？先让我们回顾前一章所学的`kill-region`，其作
用是将某个`region`添加到`kill环`中，并将其删除，从而实现剪切的操作。并且由于其通
常只被`kill-region`函数所使用，所以对于`kill-region`进行单独适配。

让我们看向`THEN`部分吧！

``` emacs-lisp
(kill-append (buffer-substring begin end) (< end begin))
```

整个`THEN`部分只有一个`kill-append`函数， 通过这个函数名我们就大概能猜到其作用是
将数据`追加`到`kill环`的操作。

首先，`kill-append`函数接收两个参量，先说第一个，其要求第一个参量是一个
`STRING`类型的数值，而这里就可以使用`buffer-substrin`函数获得要追加的字符串。第
二个参量用于判断将`STRING` 放置于`kill环`中原数据之前还是之后，这里就是将`end`与
`begin`进行比较，如果结束位置小于开始位置则代表用户想要从缓冲区的开始往后剪切，
否则是从缓冲区后往前进行剪切。


下面我们看`ELSE`部分，这个部分是当最后一个执行的命令不是`kill-region`时触发的，
其作用让我们潸潸道来，首先是用`setq`函数更新`kill-ring`中的数据:


``` emacs-lisp
(setq kill-ring
      (cons (buffer-substring begin end) kill-ring))
```

其为`kill-ring`重新赋值，值为通过`buffer-substring`函数获取的字符串，并将其通过
`cons`函数将字符串设置为`kill-ring`的car部分。

随后是一个`if`表达式，这个表达式的作用是防止`kill环`的长度超过`kill-ring-max`的
值，如果当前`kill-ring`的长度超过`kill-ring-max`，那么就将`kill-ring`最后一个元
素去除。去除的方式是通过`setcdr`以及`nthcdr`函数，首先通过`nthcdr`函数获取`kill
环 `中最后一个元素，再通过`setcdr`函数将其设置为`nil`。

到此，涵盖整个函数体的`if`表达式就执行完了，后面的则一些后续的工作，例如设置
`this-command`值为`kill-region`函数。

最后的:

``` emacs-lisp
(setq kill-ring-yank-pointer kill-ring)
```

中的`kill-ring-yank-pointer`也只不过是`kill-ring`的一个全局变量的复制品罢了。






## 回顾

* cdr,car
car 返回一个列表的第一个元素，cdr返回从列表第二个元素开始到最后一个元素的列表。

* cons
这个函数用于将其第一个参量插入至第二个参量从而构造一个新列表。

* nthcdr
获取第一个参量个第二个参量的cdr部分。

* setcdr、setcar
setcar用于设置列表的car部分，setcdr用于设置列表的car部分。

* progn
这个函数可以依次对其多个参量求值并最终返回最后一个参量的值。

* save-restriction
这个函数用于记录当前缓冲区变窄是否设置，如果设置那么其后续的参量求值后都将恢
复记录过的变窄设置。

* search-forward
这个函数用于查找字符串，如果查找就将当前位点改变为查找到的字符串的位点。
并且其还自带四个参量:
  * 要查找的字符串
  * 查找的限制范围(region)
  * 如果查找失败的处理方式，如果为nil则返回错误信息。
  * 重复查找多次
  
* kill-region
此函数将`region`复制进`kill环`并 将其在buffer中删除。

* delete-region
将某个`region`之间的数据全部删除。

* copy-region-as-kill
将某个`region`之间的数据复制进`kill环`。

 
## 查找练习

* 编写一个查找字符串的交互函数。如果找到需要的字符串，在其后设置位点并显示这样的
  一条消息:"Found!"
  
``` emacs-lisp
(defun search-forward-t (str)
  (interactive "MSearch String:")
  (let ((isfound nil))
    (if (search-forward str nil nil nil)
        (message "Found!"))))
```

* 编写一个函数，这个函数在回显区打印kill环的三个元素，如果kill环没有第三个元素，
  则打印一条适当的消息。

``` emacs-lisp
(defun print-kill-ring-three ()
  (if (nthcdr 3 kill-ring)
      (nthcdr 3 kill-ring)
    (message "kill-ring not have 3'th cdr")))
```

* 在第19.29版中，copy-region-as-kill函数不再设置this-command变量。这种变化的后果
  是什么？要采取什么相应的变化，才能达到相同的效果? 
 
答: 不会

# 第九章 列表是如何实现的

本章讲述的是列表的原理而不是实现，也不知道书中为何要这么起标题。

列表是由一系列的成对指针构成的，这每对指针的第一个指针要么指向一个原子，要么指向
另一个列表，而第二个指针要么指向下一个系列列表要么指向nil，nil也就代表这整个列表
系列中最后一个列表。 

指针其实就是一个指向电子地址的玩意，由此可得知列表就是一堆指向电子地址的指针。

那一个简单的列表举例

``` emacs-lisp
'(rose violet buttercup)
```

这个列表中有三个元素，每个元素也就是我们所说的一个成对的指针，如图:

``` 

 ___ ___       ___ ___       ___ ___
|___|___| --> |___|___| --> |___|___| --> nil
  |             |             |
  |             |             |
  -->rose       --> violet    --> buttercup

```

上图每个方框都代表计算机中的某个地址，例如，第一个方框中保存的就是指向"rose"的地
址，你也可以直接理解为保存的就是"rose"的地址，计算机可以直接通过地址来访问"rose"
这个数据。而第二个方框要么指向下一个元素要么指向nil，这里是指向的下一个元素的地
址，这个地址也就是第二个成对方框的地址，而这第二个元素的第一个方框中保存的是
"violet"的地址，跟第一个元素的第一个方框一样，有不同的在于第三个元素的第二个方框，
它指向的是一个nil，代表这个元素是这个列表最后的元素，也就是用于标记结束。

如果我们通过`setq`函数将这个整个列表赋值给一变量这个图会怎么表示呢?

``` emacs-lisp
(setq bouquet '(rose violet buttercup))
```

我们知道`setq`的第二个参量是一个列表，所以只需要将这个列表的第一个元素的地址赋值
给`bouquet`，这样就可以通过`bouquet`访问这个花(上面的这个列表可以称为花列表，因
为它们都是花的名称)列表了。

用图表示:

``` 
bouquet
      |
      |
      |     ___ ___       ___ ___       ___ ___
      -->  |___|___| --> |___|___| --> |___|___| --> nil
              |             |             |
              |             |             |
              -->rose       --> violet    --> buttercup

```

前面说过Lisp中除了列表就是原子，像前面这个花列表的每个元素也可以称为一个列表或是
cons，因为是成对的结构，我们可以理解为`car`和`cdr`的关系，如果用图可以这样表示:

``` 
bouquet
      |
      |     ________ _______       ________ ________       _________ ________
      -->  | car    | cdr   |     |  car   | cdr    |     | car     | cdr    |
           | rose   | 0     ----> | violet | 0      ----> | butter- | 0      |
           |        |       |     |        |        |     | cup     |        |
            -------   ------       -------- --------       --------- --------

```


对于函数来说，我们可以将它的结构想象成一个抽屉，通过下图来将想象的抽屉画出来:

``` 

      抽屉箱子              抽屉内容
 ___________________
|                   |
| symbol name       | --> bouquet
|                   |
 -------------------
|                   |
| symbol definition | --> [none]
|                   |
 -------------------
|                   |
| variable value    | --> (rose violet buttercup)
|                   |
 -------------------
|                   |
| property list     | --> [not describe here]
|                   |
 -------------------
|/                 \|
```

ELisp把符号定义、变量值、符号名放在单独的抽屉内，每个抽屉内存储的是其指向的真正
数据的地址，这样做的好处是例如在改变变量值时其他的符号不会有任何改变。

其实还有一个属性列表，书中没有讲，有兴趣的可以点下这个连接[Property List With GNU-EMACS Manual](https://www.gnu.org/software/emacs/manual/html_node/elisp/Property-Lists.html)

符号讲完，我们再继续讨论，如果将一个列表的cdr部分赋值给一个变量，那么用图怎么表
示它的结构呢？先看看如下代码:

``` emacs-lisp
(setq flowers (cdr bouquet))
```

其实十分简单，也只不过是通过`cdr`函数取出`bouquet`变量所指向的花列表的第二部分，
然后将其地址赋值给`flowers`变量。

``` 
bouquet
      |        flowers
      |              |
      |     ___ ___  |    ___ ___       ___ ___
      |    |   |   | --> |   |   |     |   |   | 
       --> |___|___| --> |___|___| --> |___|___| --> nil
              |             |             |
              -->rose       --> violet    --> buttercup

```

## 带点偶对 

带点偶对(dotted pair)又被称为cons原胞(cons cell)，它代表一个成对的地址框。

我们都知道`cons`函数会把第一个参量作为新元素插入到第二个列表类型的参量中作为
`car`部分，那如果插入一个新值其他与其他元素有关系的符号会不会有影响呢？请看代码
和图:

``` emacs-lisp
(setq bouquet (cons 'lily bouquet))
```

上段代码可以得到下图:

``` 
bouquet
      |                    flowers
      |                          |
      |    ___ ___      ___ ___  |    ___ ___       ___ ___
      |   |   |   |    |   |   | --> |   |   |     |   |   | 
       -->|___|___|--> |___|___| --> |___|___| --> |___|___| --> nil
              |             |             |
              --> lily      -->rose       --> violet    --> buttercup

```

如果用`eq`函数将`flowers`与`bouquet`的`nthcdr` 2 部分进行比较会是怎样呢?

``` emacs-lisp
(eq flowers (nthcdr 2 bouquet))
```

我们发现`flowers`的值并没有变化，这完全是因为Lisp中，想要得到一个列表的`cdr`，只
要得到地址系列中下一个cons原胞的地址即可；要得到一个列表的`car`只需要的得到这个
列表的第一个元素的地址；而要插入一个新元素，只不过是往列表中添加了新的cons原胞罢
了。

经过上面的演示，可以得到一个列表最后一个cons原胞的最后一个地址指向的是。

## 练习

将符号flowers设置为violet和buttercup两个元素组成的列表啊。 往这个列表中增加两种
新的花名，并将这个列表赋值给more-flowers变量。将flowers的car设置为一种鱼的名字。
看一看more-flowers列表现在的内容是什么?

``` emacs-lisp
(setq flowers '(violet buttercup)) ;; ==> (violet buttercup)
(setq more-flowers (cons 'chrysanthemum (cons 'carnation flowers)))  ;; ==> (chrysanthemum carnation violet buttercup)
(setcar flowers 'shark) ;; ==> shark
more-flowers ;; ==> (chrysanthemum carnation shark buttercup)
```

# 第十章 找回文本

> 女朋友闹分手，很好(都是我的错)，我又有时间学习了.

前面在讲剪切和存储文本时讲到被kill的数据将会添加到kill环中，并且随时可以将其取出。
本章将会学习`yank`及相关命令。

先对`kill-ring`求值，看看里面有些什么吧，如果数据过多可以将`kill-ring`设置为nil，
但是注意一旦清除就无法找回。

现在你的`kill环`就是一个空列表了，那么下面你可以通过`kill-region`或
`copy-region-as-kill`添加一些数据到`kill环`中。

那么现在你可以通过`yank`命令取出最后一个放入`kill环`的数据。`yank`命令通常被绑定
在`C-y`键上，如果你再接上按键`M-y`就持续循环找回数据，例如`kill环`列表的求值结果
是:

``` emacs-lisp
kill-ring ;; ==> ("some text" "a different piece of text " " yet more text" )
```

可以按一次`C-y`找回`some text`，然后再按任意次`M-y`键，能一次循环找回`kill环`中
的文本，如果找到`kill环`最后一个时将会重置顺序，从第一个开始找。`M-y`键绑定的是
`yank-pop`命令。

能从`kill环`找回文本的函数目前我只知道三个，分别是`yank`、`yank-pop`、
`rotate-yank-pointer`。这三个函数都使用了`kill-ring-yank-pointer`变量。

## kill-ring-yank-pointer变量 

`kill-ring-yank-pointer`和`kill-ring`都是一个变量，只不过
`kill-ring-yank-pointer` 可以通过被绑定相应的值来指向某些东西。

因此，如果`kill环`的值为:

```
("some text" "a different piece of text" " yet more text" )
```

并且`kill-ring-yank-pointer`是指向其中第二个元素，那么`kill-ring-yank-pointer`的
值就是:

``` 
("a different piece of text " " yet more text")
```

为啥会出现这样的结果呢？因为计算机不会同时为`kill-ring-yank-pointer`和
`kill-ring`拷贝一份相同的数据，而是存储一个地址。

``` emacs-lisp
kill-ring   kill-ring-yank-pointer
        |             |
        |    ___ ___  |    ___ ___       ___ ___
        |   |   |   | --> |   |   |     |   |   |
        --> |___|___| --> |___|___| --> |___|___| --> nil
              |             |             |
              |             |             |
              |             |             |
              |             |             --> "yet more text"
              |             --> "a different piece of text"
              |--> "some text"
```

其实变量`kill-ring`和`kill-ring-yank-pointer`都是指针，但是好像大家在提到
`kill-ring`时都像是特指它的组成部分，也就是说提到它就像是提到`kill环`列表，而不
是说它是一个指向列表的指针。但是`kill-ring-yank-pointer`大家一看就会觉得是一个指
向列表的指针。

其实取出`kill环`中具体哪个元素取决于`rotate-yank-pointer`函数，这个函数将会在附
录B提供详解。
  
## 练习

  * 使用C-h v (describe-variable) 命令，看一看你的kill环的值。为你的kill环增加几
    个元素，再看一看它的值。使用M-y(yank-pop)命令在kill环中移动。你的kill环到底
    有几个元素？用kill-ring-max得到这个值。你的kill环是否已满，或者你能否可以继
    续往其中保存更多的文本块？
    
    ``` emacs-lisp
    ;; 增加元素
    (setq kill-ring (cons 'test kill-ring))
    ;; kill环中移动
    (counsel-yank-pop)
    ;; kill环的元素总量
    (length kill-ring)
    ;; kill环的元素数量最大值
    kill-ring-max
    ;; kill环了后是否还能继续保存文本？
    ;; 不能 
    ```
  * 使用nthcdr和car函数构建一系列表达式分别来返回一个列表的第一个元素，第二个元素，
    第三个元素，等。
    
    ``` emacs-lisp
    ;; 第一个元素
    (car '(a b c))
    ;; 第二个元素
    (nthcdr 1 '(a b c))
    ```

# 第十一章 循环和递归

Emacs Lisp 有 `while`循环和使用递归(recursion)来对任意多个表达式重复求值。

## while

`while`特殊表会对其第一个参量求值，如果值不为false/nil，则执行循环体内部表达式，
执行完毕后会再次测试第一个参量的求值结果是否为false/nil，如果值为flase/nil则不执
行循环体内部表达式，并且退出循环，不再对一个参量求值。

## while循环和列表 

我们使用列表作为`while`循环的控制语句部分，因为如果一个列表为nil(空列表)，那么
`while`将不会再重复执行，例如有这么一个列表。

``` emacs-lisp
(setq lst ())
```

然后这个将这个包含了一个空列表的变量`lst`作为`while`循环控制语句部分，`while`将
不会重复执行一次，因为`while`重复执行的条件是，当对控制语句部分求值时值不为nil才
会执行循环体内部的代码。

而如果我们有一个有数据的列表会怎样呢:

``` emacs-lisp
(setq lst'(a b c d e))
```

就像这样，如果我们直接将其作为`while`循环的控制将会使得`while`循环永远的执行，因
为`lst`的值永远都是非`nil`的。

为了让`while`循环不要永远的执行，我们必须尝试改变`lst`的值，让其每次都有变化，直
到循环体内达到我们想要的效果，就可以将`lst`的值设置为`nil`了。

下面编写一个函数，可以让任何函数列表参量的元素都被打印到`MESSAGE`缓冲区，并
且当所有的元素被打印过一次后，使得`while`循环结束。

## 一个例子: print-elements-of-list

前面一直都没有提到`while`语句的结构，这里简单说下:

``` emacs-lisp
(while true-or-false-test
    body...)
```

其中`true-or-false-test`为`while`语句的控制语句部分，`body`为循环体，可以有多个
表达式。

而在这里我们的代码是:

``` emacs-lisp
(defun print-elements-of-list (lst)
  "打印LST列表的每个元素至MESSAGE缓冲区"
  (while lst
    (message "%s" (car lst))
    (setq lst (cdr lst))))
(print-elements-of-list lst)
```

我们可以通过函数的结构来分析这个函数:

``` emacs-lisp
(defun fun-name (fun-argument)
    "fun-doc"
    (while true-or-false-test
        body..))
```

这个函数接收一个参量，随后是函数的文档，文档内清楚的写了函数的功能，随后就是
`while`表达式了，这个表达式的控制语句部分是将参量`lst`进行求值，根据`while`表达
式的特性我们直到只有控制语句部分的值为`nil`循环体才会停止循环，让我们看看循环体
部分，循环体部分有一个`message`函数用于打印`lst`参量第一个元素的值，随后是一个
`setq`这个函数将`lst`重新赋值了，其值就是将原本列表的值重新赋值为原本的cdr部分，
随后`while`再一次判断`lst`的值是否为nil，如果不是继续执行，直到`lst`的值为nil为
止，由于循环体的副作用，`lst`的每个元素都被打印出来了。

## 使用增量计数器的循环

多说不宜，让我们赶快写个案例来使用一下 `while`循环吧!

下面有一个场景，数石头:

``` 
*
**
***
****
*****
```

每个星号就代表一个石头，现在这个三角形一共有五行石头，如果一个一个数数可以很快的
数出一共有15个石头，但是假如现在有100行，那么再一个溢恶个去数数确实显得比较慢了，
所以我们在这里就可以使用循环来帮助我们完成一些重复性的工作。

那么我们就要设计一个函数，这个函数可以根据用户录入的行数计算出石头的总量。那么首
先根据"需要一个用户录入的行数"可以预想，这个函数需要一个参量当作用户给定的行数，
下面我们跳过函数文档直接来写`while`部分，首先是`while`函数的控制语句部分，这个控
制语句决定了我们是否要继续执行函数体内部的代码，那内部的代码我们暂时未知，但是我
们可以预想，这个控制语句部分的值不能永远是非`nil`的值，如果是则会一直循环，永远
无法退出，那这个控制语句该怎么写呢？我们目前有一个参量可用，是由用户录入的总行数，
那么是不是当我们计算到的当前行小于或等于总行数时才可以执行循环体，但是我们发现，
现在没有定义表示当前行的变量，所以我们在写`while`循环体时就必须加一个计数器，这
个计数器可以用`let`函数来生成，但是我们是否还差一个变量呢？我们好像还需要一个可
以存储每次计算后石头的总量的变量，所以我们还需要使用`let`函数定义一个石头总量的
变量。那么就有一段这样的代码:

``` emacs-lisp
(defun calculation-triangle (row-total)
  "计算石头总量"
  (let (
        (current-row 1) ;; 当前行
        (stone-total 0) ;; 当前石头总量
        )
    (while (<= current-row row-total)
      body...)
    ...))
```

为什么要把`current-row`设置为`1`呢？因为我们肯定是从第一行开始计算，所以默认值就
为1，至于`stone-total`很简单，因为一开始没有计算的时候，我们的石头总量为0.

那么下面我们只需要写`while`语句的循环体部分即可，也就是我们真正计算的地方，试想
一下，要如何计算呢？其实很简单，我们知道`current-row`当前行行数就等于那一行的石
头个数，那么我们只需要每次执行循环体时将`stone-total`的值加上`current-row`的值，
即可做到累加石头个数，但这样还不够，因为为了让循环体循环次数得到我们想要的结果，
是需要将`current-row`在每次计算后加一，这样也代表当前行计算完成，需要转移到下一
行。那么这样`while`语句的循环体内的代码是这样的:

``` emacs-lisp
(while (<= current-row row-total)
  (setq stone-total (+ stone-total current-row))
  (setq current-row (1+ current-row)))
```

那么现在一切都计算完成，我们还需要做一步，那就是将`stone-total`返回，不然我们的
计算等于白做了，最终的函数实现代码是:

``` emacs-lisp
(defun calculation-triangle (row-total)
  "计算石头总量"
  (let (
        (current-row 1) ;; 当前行
        (stone-total 0) ;; 当前石头总量
        )
    (while (<= current-row row-total)
      (setq stone-total (+ stone-total current-row))
      (setq current-row (1+ current-row)))
    stone-total)) ;; => calculation-triangle

(calculation-triangle 100) ;; => 5050
(calculation-triangle 5) ;; => 15
```
 


## 使用减量计数器的循环

有增那也有减，这个例子我们使用减的方式，将任意行数的石头总量计算出来，规则不变，
还是相同100行的石头有100个石头，99行的石头有99个石头，那么下面就让我们来分析一下
吧.

首先是我们同样需要用户录入石头的总行数，其次我们仍然需要两个变量，分别用于存储计
算得到的石头个数，以及当前行数。那么我们也就大概知道每次循环体内部需要做的事就是
将石头总数加上当前行以及将当前行减去1。

``` emacs-lisp
(defun calculation-triangle-sub (row-total)
  "计算石头总量"
  (let ((current-row row-total)
        (stone-total 0))
    (while (> current-row 0)
      (setq stone-total (+ current-row stone-total))
      (setq current-row  (1- current-row)))
    stone-total)) ;; => calculation-triangle-sub

(calculation-triangle-sub 5) ;; => 15
```

这段代码跟增量计数器有两点不同，分别是初始化`current-row`的值以及对
`current-row`进行修改时有变化，先让我们来看为什么把`current-row`的初始值设置为
`row-total`，因为我们是要使用减量计数器来计算，所以我们需要直接从最大石头个数的
行数开始算，直到行数小于1为止，而要做到“直到小于1为止”那么则需要通过`while`函数
的控制语句以及修改每次`current-row`时做的事了，我们先是通过将`current-now`与
`0`进行比较，当`current-now`小于0时，也就代表了所有行数的石头都累加完成，至于是
如何从最大石头个数的行开始计算是通过`(setq current-now (1- current-now))`实现的。






## 递归

递归是一种调用函数的方式，但是由于其特殊性，它可以做到类似循环的操作，**递归就是
一种自身调用自身的函数**，与`while`循环类似，如果不为递归提供一个停止自身调用的
机制，那么递归将会陷入死循环，永不止境的调用自身。

一个递归函数的常用组合:

* 一个真假测试，决定函数是否要继续调用自身，称作**do-again-test**。
* 函数名。
* 一个表达式，它在函数被重复求值正确的次数之后使条件表达式返回“假”值，称作
**next-step-expression**。

根据以上这三个部分可以为递归函数画一个模板:

``` emacs-lisp
(defun name-of-recursive-function (argument-list)
  "docutmentation..."
  body...
  (if do-agaion-test
      (name-of-recursive-function next-step-expression)))
```

在每一次执行时至少要让`next-step-expression`的值发生变化，这样做是为了让当函数不
必再递归时使得`do-again-test`返回假。

`do-again-test`是真假测试表达式，也可以称作`停止条件`。

### 使用列表的递归函数

下面我们将前面用`while`循环做的将一个列表的元素一一打印出来，用递归函数来实现一
次。

书中是使用的`print`将列表的元素一一输出，我这边直接用`message`函数，这样方便查看：

``` emacs-lisp
(defun print-element-by-recursion (lst)
  "用递归的方式打印一个列表的元素"
  (message "%s" (car lst)) ;; body
  (if lst ;; do-again-test
      (print-element-by-recursion ;; recursive call
       (cdr lst)))) ;; next-step-expression
```

这个递归函数首先打印了`lst`的`car`部分，也就是第一个元素，然后经由
`do-again-test`进行判断，如果`lst`不为`nil`那么则调用自身，形成递归，但是在
`recursion-call`时传递的参量是原本`lst`的`cdr`部分，也就是从第二个元素到最后一个
元素的列表，然后依次递归，每递归一次`lst`的长度就会减少，直到其只剩下`nil`，
`do-again-test`控制得以不再递归。

最终得到的结果可以在`*MESSAGE*`缓冲区查看:

``` 
a
b
c
d
e
nil
```

### 用递归算法替代计数器

前面我们写过计算石头个数的例子，其实它是一个数列求和，我们知道最大行数，也就是最
大个数，也知道最小个数为一，那么我们可以通过一个公式求出其总和，这个公式就是:`((1
+ n) * n) / 2`，头加尾，乘以数量，除以2.那么我们原本的计算石头个数例子就可以通过
这个公式来计算:

``` emacs-lisp
(defun sum-stone (number)
  (/ (* number (1+ number)) 2))
(sum-stone 5)
```

虽然这样算也比较方便，但是其实这是用一个公式计算得到的，遇到其他不可用公式但是需
要用计数器的地方，我们就可以使用递归了。

先来看看递归版本:

``` emacs-lisp
(defun sum-stone-recursive (row-total)
  (if (= row-total 1)
      1
    (+ row-total (sum-stone-recursive (1- row-total)))))

(sum-stone-recursive 5) ;; => 15
```

为了解释这个函数如何工作，我们将会一次对这个函数传递1、2、3来分析其运作过程。

首先其参量的值1，当遇到`if`判断时`row-total`等于1，那么就直接返回1，不会递归，得
到我们想要的答案1。

再就是参量的值为2的情况，当遇到`if`判断时`row-total`不等于1，那么进入`ELSE`部分，
这里将`row-total`与`sum-stone-recursive`的返回值进行相加，此时的`row-total`的值
为2，然后当递归调用时，传递的参量是`row-total`减1，此时`row-total`的值为1，当遇
到`if`判断时返回值为1，那么现在程序运行到递归前将`row-total`与
`sum-stone-recursive`相加时了，这表达式得到的结果是3，因为是2+1，那么这个递归函
数就停止递归了，得到我们下更要的答案3。

最后是参量值尾3的情况，这一切跟参量为2时情况差不多。




### 使用cond的递归例子

`cond`是一个类似`if`特殊表的特殊表，其跟`if`特殊表不同的是，其没有ELSE部分,其函
数模板是这样的:

``` emacs-lisp
(cond
 ((first-t-or-f-test first-consequent)
  (second-t-or-f-test second-consequent)
  (third-t-or-f-test third-consequent)
  ...))
```

Lisp解释器首先计算`t-or-f-test`部分，如果为t，那么对`consequent`部分求值，如果为
nil，则直接忽略，继续计算下一个`t-or-f-test`部分，如果`cond`内的所有表达式的值都
为nil，那么`cond`的返回值为nil。

下面可以通过`cond`特殊表来修改下`sum-stone-recursive`函数，可以为其添加几种情况
的处理方式：

``` emacs-lisp
(defun sum-stone-recursive (row-total)
  (cond
   ((= row-total 0) 0)
   ((= row-total 1) 1)
   ((> row-total 1) (+ row-total
                       (sum-stone-recursive
                        (1- row-total))))))
```

## 有关循环表达式的练习 

* 编写一个与calculation-triangle函数类似的函数，在这个函数中，每一行的值等于所
在行的平方。使用while循环来编写这个函数。

``` emacs-lisp
(defun demo-01 (row-total)
  (let ((current-row 1)
        (stone-total 0))
    (while (<= current-row row-total)
      (setq stone-total (+ stone-total (* 2 current-row)))
      (setq current-row (1+ current-row)))
    (1- stone-total)))

(demo-01 2) ;; => 5
```

* 编写一个与`sum-stone`函数相似的函数，求那些数的积，而不是和。

``` emacs-lisp
(defun demo-02 (number)
  (let ((sum-number 1)
        (count-number 1))
    (while (<= count-number number)
      (setq sum-number (* sum-number count-number))
      (setq count-number (1+ count-number)))
    sum-number))

(demo-02 3) ;; => 6
```

* 用递归的方法重新编写上面两个函数，然后用`cond`表达式重新编写这两个函数。

``` emacs-lisp
(defun demo-03-1 (number)
  "demo-01的递归版本"
  (if (= number 1)
      1
    (+ (* 2 number) (demo-03-1 (1- number)))))

(demo-03-1 2) ;; => 5

(defun demo-03-2 (number)
  "demo-02的递归版本"
  (if (= number 1)
      1
    (* number (demo-03-2 (1- number)))))

(demo-03-2 3) ;; => 6

(defun demo-03-3 (number)
  "demo-01的cond递归版本"
  (cond
    ((= number 0) 0)
    ((= number 1) 1)
    ((> number 1) (+ (* 2 number) (demo-03-3 (1- number))))))

(demo-03-3 2)

(defun demo-03-4 (number)
  "demo-02的cond递归版本"
  (cond
    ((= number 0) 0)
    ((= number 1) 1)
    ((> number 1) (* number (demo-03-4 (1- number))))))

(demo-03-4 3)
```

* 为Textinfo模式编写一个函数，这个函数再每一个以"@dfn"开始的段落创建一个索引入口。

  《Texinfo: GNU文档格式》中关于"标记函数和命令等" 的一节。)
  
  不会。。。

# 第十二章 正则表达式查询

> 这一章鸽了几天，主要是因为我这两天有点沉迷王者荣耀，并且还对正则表达式不是很了解，
> 学校决定三月九日开始上网课，为了到时不要每天忙死，还是先早点学习完这些要学的。

本章要求你了解过正则表达式的使用方式。这里有两个链接，可供快速入门。

[30分钟入门正则表达式](https://deerchao.cn/tutorials/regex/regex.htm)

[35分钟入门正则表达式-视频](https://www.bilibili.com/video/av18182693?from=search&seid=2331088568154680512)

## 查询sentence-end的正则表达式

`sentence-end`是一个用于标记句子是否结束的正则表达式，它的值是
`[.?!][]\"'”’)}]*\\($\\|    \\|  \\)[]*`，如果你不会正则表达式你将会对此感到非常
的疑惑，没关系，让我们一一解读它。

由于这个符号是用于标记句子结束的，所以我们知道.?!这一段针对的是句子末尾如果存
在.?!就代表它们结束了，那么第一个子表达式就的到了解释:

- `[.?!]`
代表句子末尾存在这三个字符都代表句子结束了。

再让我们看看后面的`[]\"'”’)}]*`，首先第一个`[`和最后一个`]`代表了子表达式的范围，
随后是一个`]`，这个字符可能被一些句子作为结尾的标识。再后是一个`\"`，因为有些句子已
右双引号结尾代表句子的结束，而前面的`\`是Emacs中对双引号的转义标记，让这个本来在
Emacs中为特殊字符的字符转换为普通字符。随后的`'`右单引号，`”`中文右双引号，`’`中
文右单引号，`)`右括号，`}`右花括号都是一些句子的结尾标识，所以这里要对其进行匹配。

解答完第一个子表达式，再让我们看看`*`，这是一个特殊字符，代表前面一个字符可以出
现任意次。

- `*`
前匹配字符可以出现任意次。

而后面的`\\`说明让Emacs将`(`标记为一个正则表达式特殊字符，其作用是代表了一个分组
的开始，而后面的`\\(`代表了分组的结束，分组的内容通过拆分可以得到`$`，`\\|`，
`&emsp;&emsp;&emsp;&emsp;`，`\\|`,`&emsp;&emsp;`几个表达式，下面一一进行解答:

- `$`
代表从一个句子的末尾开始匹配后面的模式

- `\\|`
由`\`进行转义，使得`|`称为特殊字符，这是代表了一个分支。

- `&emsp;&emsp;&emsp;&emsp;`
代表了四个空格，也就是`TAB`制表符

- `\\|`
与前面同义

- `&emsp;&emsp;`
代表两个空格，有些句子的分隔会用两个空格来区分。

## re-search-forward函数

`search-forward`的另一个版本，这个将第一个参量改为接收一个正则表达式字符串，其他
三个参量与其相同。

- 第一个参量
  要查找的正则表达式字符串。
- 第二个参量
  可选参量，用于限制查询的范围。
- 第三个参量
  可选参量，用于在查询失败时的响应。
- 第四个参量
  可选参量，用于查询计数。
  
## forward-sentence函数

`forward-sentence`是一个将光标移动到下一个句子之前的命令，我们通过这个例子理解在
EmacsLisp中使用正则表达式查询的工作方式。

``` emacs-lisp
(defun forward-sentence-t (&optional arg)
  "向前移动至下一个sentence-end的位置
具体细节可以看官方的文档"
  (interactive "p")
  (or arg (setq arg 1)) ;; 如果arg为nil，那么就将其设置为1
  (while (< arg 0) ;; 如果arg小于0
    (let ((par-beg (save-excursion
                     (start-of-paragraph-text)
                     (point))))
      (if (re-search-forward
           (concat sentence-end "[^ \t\n]") par-beg t)
          (goto-char (1- (match-end 0)))
        (goto-char par-beg)))
    (setq arg (1+ arg)))
  (while (> arg 0)
    (let ((par-end (save-excursion
                     (end-of-paragraph-text)
                     (point))))
      (if (re-search-forward sentence-end par-end t)
          (skip-chars-backward " \t\n")
        (goto-char par-end))
      (setq arg (1- arg)))))
```









