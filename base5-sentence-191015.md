
# 条件、循环及其他语句

## 再谈 print 和 import
- 对很多应用程序来说,使用模块 logging 来写入日志比使用 print 更合适,详情请参阅第19章


```python
# print 可用于打印一个表达式,这个表达式要么是字符串,要么将自动转换为字符串。但实际上,你可同时打印多个表达式,条件是用逗号分隔它们:
print('Age:', 42)
```

    Age: 42



```python
# 在参数之间插入了一个空格字符。在你要合并文本和变量值,而又不想使用字符串格式设置功能时,这种行为很有帮助
name = 'Gumby'
salutation = 'Mr.'
greeting = 'Hello,'
print(greeting, salutation, name) #（问候语、敬礼、姓名）
```

    Hello, Mr. Gumby



```python
# 如果字符串变量 greeting 不包含逗号,如何在结果中添加呢?
print(greeting, ',', salutation, name) # no
```

    Hello, , Mr. Gumby



```python
print(greeting + ',', salutation, name) # yes
```

    Hello,, Mr. Gumby



```python
# 可自定义分隔符
print("I", "wish", "to", "register", "a", "complaint", sep="_") # 我希望你登记一个投诉
```

    I_wish_to_register_a_complaint



```python
# 自定义结束字符串,以替换默认的换行符。例如,如果将结束字符串指定为空字符串,以后就可继续打印到当前行
print('Hello,', end='')
print('world!')
```

    Hello,world!



```python
'''
import modulename
from modulename import function
from modulename import function1,function2,function3,...
from modulename import *
仅当你确定要导入模块中的一切时,采用使用最后一种方式。但如果有两个模块,它们都包含函数 open ,该如何办呢?你可使用第一种方式导入这两个模块,
并像下面这样调用函数:
module1.open(...)
module2.open(...)
但还有一种办法:在语句末尾添加 as 子句并指定别名。下面是一个导入整个模块并给它指定别名的例子:
'''
import math as foobar
foobar.sqrt(4)
```




    2.0




```python
# 导入特定函数并给它指定别名

# 对于前面的函数 open ,可像下面这样导入它们
# from module1 import open as open1
# from module2 import open as open2

# 有些模块(如 os.path )组成了层次结构(一个模块位于另一个模块中)。有关模块结构的详细信息,请参阅10.1.4节

from math import sqrt as foobar
foobar(4)
```




    2.0



## 赋值魔法
- 即便是不起眼的赋值语句也蕴藏着一些使用窍门

### 序列解包


```python
# 同时(并行)给多个变量赋值
x, y, z = 1, 2, 3
print(x, y, z)
```

    1 2 3



```python
# 还可交换多个变量的值
# 这在使用返回元组(或其他序列或可迭代对象)的函数或方法时很有用
x, y = y, x
print(x, y, z)
```

    2 1 3



```python
'''
假设要从字典中随便获取(或删除)一个键-值对,可使用方法 popitem ,它随便获取一个键-值对并以元组的方式返回。接下来,可直接将返回的元组解包到
两个变量中。
'''
scoundrel = {'name': 'Robin', 'girlfriend': 'Marion'}
key, value = scoundrel.popitem()
key
```




    'girlfriend'




```python
value
```




    'Marion'




```python
'''
这让函数能够返回被打包成元组的多个值,然后通过一条赋值语句轻松地访问这些值。要解包的序列包含的元素个数必须与你在等号左边列出的目标个数相同,
否则Python将引发异常。
'''
x, y, z = 1, 2 # 没有足够的值来解包（需要3，得到2）
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-16-37e55ee07fc5> in <module>()
          3 否则Python将引发异常。
          4 '''
    ----> 5 x, y, z = 1, 2
    

    ValueError: not enough values to unpack (expected 3, got 2)



```python
x, y, z = 1, 2, 3, 4 # 要解包的值太多（应为3）
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-17-fa60cb8a6b9a> in <module>()
    ----> 1 x, y, z = 1, 2, 3, 4
    

    ValueError: too many values to unpack (expected 3)



```python
# 可使用星号运算符( * )来收集多余的值,这样无需确保值和变量的个数相同
a, b, *rest = [1, 2, 3, 4]
rest
```




    [3, 4]




```python
# 还可将带星号的变量放在其他位置
name = "Albus Percival Wulfric Brian Dumbledore" # 阿不思·珀西瓦尔·伍尔弗里克·布赖恩·邓布利多
first, *middle, last = name.split()
middle
```




    ['Percival', 'Wulfric', 'Brian']




```python
# 赋值语句的右边可以是任何类型的序列,但带星号的变量最终包含的总是一个列表。在变量和值的个数相同时亦如此
a, *b, c = "abc"
a, b, c
```




    ('a', ['b'], 'c')



### 链式赋值


```python
# 链式赋值是一种快捷方式,用于将多个变量关联到同一个值
# x = y = somefunction()
# 上述代码与下面的代码等价
# y = somefunction()
# x = y
'''
请注意,这两条语句可能与下面的语句不等价:
x = somefunction()
y = somefunction()
有关这方面的详细信息,请参阅5.4.6节介绍相同运算符( is )的部分。
'''
```

### 增强赋值


```python
# 适用于所有标准运算符,如 * 、 / 、 % 等
# 通过使用增强赋值,可让代码更紧凑、更简洁,同时在很多情况下的可读性更强
a = 1
a += 1
a
```




    2




```python
a *= 2
a
```




    4




```python
# 增强赋值也可用于其他数据类型(只要使用的双目运算符可用于这些数据类型)
fnord = 'foo'
fnord += 'bar' # 弗诺德 福巴尔
fnord *= 2
fnord
```




    'foobarfoobar'



## 代码块:缩进的乐趣
- 代码块是一组语句,可在满足条件时执行( if 语句),可执行多次(循环),等等
- 标准(也是更佳的)做法是只使用空格(而不使用制表符)来缩进,且每级缩进4个空格
- 在同一个代码块中,各行代码的缩进量必须相同


```python
'''
this is a line
this is another line:
    this is another block
    continuing the same block
    the last line of this block
phew, there we escaped the inner block 嘿，我们从里面的块逃走了
在Python中,使用冒号( : )指出接下来是一个代码块,并将该代码块中的每行代码都缩进相同的程度。发现缩进量与之前相同时,你就知道当前代码块到此结束了
'''
```

## 条件和条件语句
### 布尔值的用武之地
- 用作布尔表达式(如用作 if 语句中的条件)时,下面的值都将被解释器视为假:False None 0 "" () [] {}
- 标准值 False 和 None 、各种类型(包括浮点数、复数等)的数值0、空序列(如空字符串、空元组和空列表)以及空映射(如空字典)都被视为假,而其他各种值都被视为真 1 ,包括特殊值 True


```python
# 实际上, True 和 False 不过是0和1的别名,虽然看起来不同,但作用是相同的
True
```




    True




```python
False
```




    False




```python
True == 1
```




    True




```python
False == 0
```




    True




```python
True + False + 9
```




    10




```python
# 布尔值 True 和 False 属于类型 bool ,而 bool 与 list 、 str 和 tuple 一样,可用来转换其他的值
# 鉴于任何值都可用作布尔值,因此你几乎不需要显式地进行转换(Python会自动转换)
bool('I think, therefore I am')
```




    True




```python
bool(9)
```




    True




```python
bool('')
```




    False




```python
bool(0)
```




    False



### 有条件地执行和 if 语句


```python
name = input('What is your name? ')
if name.endswith('Gumby'):
#print('Hello, Mr. Gumby') # expected an indented block 需要缩进的块
    print('Hello, Mr. Gumby')
```

    What is your name? Gumby
    Hello, Mr. Gumby


### else子句


```python
# 之所以叫子句是因为 else 不是独立的语句,而是 if 语句的一部分
name = input('What is your name?')
if name.endswith('Gumby'):
    print('Hello, Mr. Gumby')
else:
    print('Hello, stranger')
```

    What is your name?Peter
    Hello, stranger



```python
# 还有一个与 if 语句很像的“亲戚”,它就是条件表达式——C语言中三目运算符的Python版本
#如果条件(紧跟在 if 后面)为真,表达式的结果为提供的第一个值(这里为 "friend" ),否则为第二个值(这里为 "stranger" )
status = "friend" if name.endswith("Gumby") else "stranger"
status
```




    'stranger'



### elif子句


```python
# 要检查多个条件,可使用 elif,也就是包含条件的 else 子句
num = int(input('Enter a number: '))
if num > 0:
    print('The number is positive') # 正的
elif num < 0:
    print('The number is negative') # 负数
else:
    print('The number is zero')
```

    Enter a number: 3
    The number is positive


### 代码块嵌套


```python
name = input('What is your name? ')
if name.endswith('Gumby'):
    if name.startswith('Mr.'):
        print('Hello, Mr. Gumby')
    elif name.startswith('Mrs.'):
        print('Hello, Mrs. Gumby')
    else:
        print('Hello, Gumby')
else:
    print('Hello, stranger')
```

    What is your name? Mr.Gumby
    Hello, Mr. Gumby


### 更复杂的条件
- 不要将 is 用于数和字符串等不可变的基本值


```python
# 1.比较运算符
"foo" == "foo"
```




    True




```python
"foo" == "bar"
```




    False




```python
"foo" = "foo" # can't assign to literal 无法指定文字
```


      File "<ipython-input-46-0f4e4fdda35a>", line 1
        "foo" = "foo"
                     ^
    SyntaxError: can't assign to literal




```python
a = b = [1,2,3]
a == b
```




    True




```python
c = [1,2,3]
a == c
```




    True




```python
a is b
```




    True




```python
a is c # 两个列表虽然相等,但并非同一个对象
```




    False




```python
# 再来
x = [1,2,3]
y = [2,4]
x is not y
```




    True




```python
del x[2] # x=[1,2]
y[1] = 1
y.reverse() # y=[1,2]
```


```python
x == y
```




    True




```python
x is y # 这两个列表相等但不相同
```




    False




```python
# in :成员资格运算符与其他比较运算符一样,它也可用于条件表达式中
name = input('What is your name?')
if 's' in name:
    print('Your name contains the letter "s".')
else:
    print('Your name does not contain the letter "s".')
```

    What is your name?smith
    Your name contains the letter "s".



```python
# 字符串和序列的比较
# 字符串是根据字符的字母排列顺序进行比较的
"alpha" < "beta"
```




    True




```python
# 虽然基于的是字母排列顺序,但字母都是Unicode字符,它们是按码点排列的
chr(128584)
```




    '🙈'




```python
chr(128585)
```




    '🙉'




```python
chr(128586)
```




    '🙊'




```python
"🙈🙉🙊" < "🙈🙊🙉"
```




    True




```python
ord("🙉")
```




    128585




```python
# 这种方法既合理又一致,但可能与你排序的方式相反。例如,涉及大写字母时,排列顺序就可能与你想要的不同
"a" < "B"
```




    False




```python
# 一个诀窍是忽略大小写。为此可使用字符串方法 lower
"a".lower() < "B".lower()
```




    True




```python
'FnOrD'.lower() == 'Fnord'.lower()
```




    True




```python
# 其他序列的比较方式与此相同,但这些序列包含的元素可能不是字符,而是其他类型的值
[1, 2] < [2, 1]
```




    True




```python
# 如果序列的元素为其他序列,将根据同样的规则对这些元素进行比较
[2, [1, 4]] < [2, [1, 5]]
```




    True




```python
# 2. 布尔运算符
# 假设你要编写一个程序,让它读取一个数,并检查这个数是否位于1~10(含)

number = int(input('Enter a number between 1 and 10: '))
if number <= 10:
    if number >= 1:
        print('Great!')
    else:
        print('Wrong!')
else:
    print('Wrong!') # 这有点笨拙,输入了 print('Wrong!') 两次。重复劳动可不是好事
```

    Enter a number between 1 and 10: 8
    Great!



```python
number = int(input('Enter a number between 1 and 10: '))
#if number <= 10 and number >= 1:
if 1 <= number <= 10:
    print('Great!')
else:
    print('Wrong!')
```

    Enter a number between 1 and 10: 15
    Wrong!



```python
# 运算符 and 是一个布尔运算符。它接受两个真值,并在这两个值都为真时返回真,否则返回假
# 还有另外两个布尔运算符: or 和 not 。通过使用这三个运算符,能以任何方式组合真值
if ((cash > price) or customer_has_good_credit) and not out_of_stock:
    give_goods()
```


```python
# 布尔运算符有个有趣的特征:只做必要的计算
#name = input('Please enter your name: ') or '<unknown>'
# 如果没有输入名字,上述 or 表达式的结果将为 '<unknown>'
```

### 断言


```python
'''
if 语句有一个很有用的“亲戚”,其工作原理类似于下面的伪代码:
if not condition:
    crash program
为何要编写类似于这样的代码呢?因为让程序在错误条件出现时立即崩溃胜过以后再崩溃。基本上,你可要求某些条件得到
满足(如核实函数参数满足要求或为初始测试和调试提供帮助),为此可在语句中使用关键字 assert 。
'''
age = 10
assert 0 < age < 100
```


```python
age = -1
assert 0 < age < 100
```


    ---------------------------------------------------------------------------

    AssertionError                            Traceback (most recent call last)

    <ipython-input-77-3315f3eccfe8> in <module>()
          1 age = -1
    ----> 2 assert 0 < age < 100
    

    AssertionError: 



```python
# 如果知道必须满足特定条件,程序才能正确地运行,可在程序中添加 assert 语句充当检查点,这很有帮助。
# 还可在条件后面添加一个字符串,对断言做出说明
age = -1
assert 0 < age < 100, 'The age must be realistic' # 年龄必须现实
```


    ---------------------------------------------------------------------------

    AssertionError                            Traceback (most recent call last)

    <ipython-input-81-c0e577d3fae0> in <module>()
          2 # 还可在条件后面添加一个字符串,对断言做出说明
          3 age = -1
    ----> 4 assert 0 < age < 100, 'The age must be realistic'
    

    AssertionError: The age must be realistic


## 循环


```python
'''没有循环：
send mail
wait one month send mail
wait one month send mail
wait one month send mail
(... and so on)

循环伪代码：
while we aren't stopped:
    send mail
    wait one month

没有循环：
print(1)
print(2)
print(3)
...
print(99)
print(100)
# 循环：
a = 1
while a <= 100:
    print(a)
    a += 1
'''
name = ''
while not name:
    name = input('Please enter your name: ')
print('Hello, {}!'.format(name))
```

    Please enter your name: Mary
    Hello, Mary!



```python
'''
while 语句非常灵活,可用于在条件为真时反复执行代码块。这在通常情况下很好,但有时候你可能想根据需要进行定制。
一种这样的需求是为序列(或其他可迭代对象)中每个元素执行代码块。
可迭代对象是可使用 for 循环进行遍历的对象。第9章将详细介绍可迭代对象和 11迭代器。就目前而言,只需将可迭代
对象视为序列即可'''
words = ['this', 'is', 'an', 'ex', 'parrot']
for word in words:
    print(word)
```

    this
    is
    an
    ex
    parrot



```python
# 鉴于迭代(也就是遍历)特定范围内的数是一种常见的任务,Python提供了一个创建范围的内置函数。
list(range(0, 10))
```




    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]




```python
range(10)
```




    range(0, 10)




```python
'''
# 相比前面使用的 while 循环,这些代码要紧凑得多
# 只要能够使用 for 循环,就不要使用 while 循环
for number in range(1,101):
    print(number)'''
```

### 迭代字典


```python

```
