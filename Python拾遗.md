### Python拾遗

#### 1. 我早已忘了的`del`

`del`用于删除对象或元素。如果定义了`a = 1`，使用`del a`后，`a`将不再存在。也可以用`del`语句删除列表、字典里的元素（不能删除元组里的元素，因为元组是不可变的）。

```python
zoo = ['python', 'panda', 'turtle']
del zoo[1]
# ['python', 'turtle']
address_book = {
    'Ross': 'ross@friends.com',
    'Joey': 'joeydoing@freinds.com',
    'Monica': 'mon@freinds.com'
}
del address_book['Joey']
```

#### 2. 类中的术语

类中的变量叫做**字段（Field）**，函数叫**方法（Method）**，字段与方法通称类的**属性（Attribute）**

#### 3. 类变量与对象变量，类方法与成员方法

类变量是直接在类中定义的变量，而对象变量是通过`self.member`这种方式定义的。正常方法都是成员方法，而如果要使一个方法为类方法时，需要使用**装饰器（Decorator）** `@classmethod`或`@staticmethod`将其标记为类方法。

```python
class Robot:
    population = 1;
    
    def __init__(self, name):
        self.name = name
        print("Inintializing {}".format(self.name))
        Robot.population += 1
        
    def say_hi(self):
        print("Hi, my name is {}".format(self.name))
        
    @classmethod
    def how_many(cls):
        print("We have {:d} robots".format(cls.population))
```

上述示例中，`population`是类变量，调用时也是使用`Robot.population`，而`self.name`则是对象成员，在类中其他函数调用时，也是使用`self.name`这样的形式。`say_hi`是成员方法，而`hoe_many`是类方法。

#### 4. 输入输出

##### 4.1.  输入

```python
text = input('enter your text here:')
```

这里的`text`的类型为`str`，如果想输入一个`int`类型的，应该做类型转换：

```python
num = int(input('enter a number here:'))
```

##### 4.2. 输出

`print`语句可以用`format`方法：

```python
print("My name is {}, I'm {} years old".format('Zemin', 92))
# format中的name和age位置是可以随意调换的
print("My name is {name}, I'm {age} years old".format(name= 'Zemin', age = 92))
# name和age为在之前定义的变量
print("My name is {0}, I'm {1} years old".format(name, age))
```

**转换描述：**在替换域中关联描述（下表或关键字）之后可以跟一个冒号":"和一个转换描述，无关联描述时应写冒号后根转换描述。常用的转换描述如下：

+ 描述对齐方式：`<, >, ^`，分别表示替换内容在特定范围内居左、居右、居中。对齐字符钱可以有一个填充字符，默认填充字符是空格。无对齐描述时，字符串居左对齐，数值居右对齐（这跟Excel是一样的）。

+ 描述本替换域最小宽度的整数。

+ 表示转换类型的字符：

  - `s`表示字符串
  - `d`要求生成十进制的整数表示
  - `f`和`F`要求生成浮点数形式
  - `e`和`E`要求生成科学计数法形式

  在表示浮点转换字符`f`和`F`前可有一个圆点和一个整数，说明浮点数表示中的小数位数，默认为6位。

举几个例子：

- `{1:->10s}`               字符串形式，第一个实参，宽度10，右对齐，填充字符是`-`
- `{price: 10.2f}`     域名`price`，宽度10，浮点形式，小数点后2位
- `:<<10d`                     十进制整数形式，宽度10， 左对齐，填充字符是`<`



`print`语句还可以通过`end`参数使得输出不换行：

```python
print("My name is {}, I'm {} years old".format('Zemin', 92), end = '')
```



这里我要记录一个我写的程序。

`描述`：检查一个输入的字符串是否是回文，忽略其中的标点、空格与大小写。例如，“Rise to vote, sir.”是一段回文文本。题目出自`A Byte of Python-输入与输出`。

`思路`：使用正则表达式

```python
import re

def strip_text(text):
    pattern = re.compile(r'[\s\,\.\;]+')
    after_strip = pattern.split(text)
    text = ''
    for s in after_strip:
        text += s
    return text

def reverse(text):
    striped_text = strip_text(text)
    print(striped_text[::-1])
    return striped_text[: : -1]

def is_palindrome(text):
    striped_text = strip_text(text)
    return re.match(striped_text, reverse(text), re.I)

something = input("Enter text: ")
if is_palindrome(something):
    print("Yes, it is a palindrome")
else:
    print("No, it is not a palindrome")
```

#### 5. 读写文件

- 读文件：

  ```python
  filename = 'fuck.txt'
  with open(filename) as file_obj:
      contents = file_obj.read()
      # do other things
      
  # 或者这样
  file_obj2 = open(filename)
  # do other things
  ```

  逐行读取：

  ```python
  # 方法1
  filename = 'fuck.txt'
  with open(filename) as file_obj:
      lines = file_obj.readlines()
      for line in lines:
          print(line.rstrip())
          
  # 方法2
  # 方法1
  filename = 'fuck.txt'
  with open(filename) as file_obj:
      for line in file_obj: #for line in file_obj.readline():
          print(line.rstrip())
  ```

- 写文件

  ```python
  filename = 'fuck_again.txt'
  with open(filename, 'w') as file_obj:
      file_obj.write("I like Python.")
  ```

  <font color = red>以写模式打开的文件，打开后文件为空，打开已有文件时清空其内容</font>

#### 6. 字典

用于字典的键必须是不变的对象。因此，数、字符串、元素为不变对象的元组都可以作为键，而表`[1, 2]`、元组`([1], [2])`等则不可以。

字典的迭代方法：

+ `dict.keys()`           得到`dict`的所有键
+ `dict.values()`       得到`dict`的所有值
+ `dict.items()`         得到`dict`的所有键—值关联，形式为`(k, v)`

几种典型的模式：

+ ```python
  for k in dict.keys():
      # do something on k
      print(dict[k])
  ```

  也可以直接写成：

  ```python
  for k in dict:
      # do somethin on k
  ```

  这种情况下解释器自动调用`keys()`。

+ ```python
  for v in dict.values():
      # do something ~
  ```

+ ```python
  for k, v in dict.items():
      # do something ~
  ```

**函数定义的双星号形参**

如果函数调用中出现未匹配的关键字实参，解释器就把这些实参做成一个字典， 关联于双星号形参。

```python
def print_data(x, **dict):
    print(x)
    for k, v in dict.items():
        print( "{:6}{}".format(str(k) + ": ", v) )
        
print_data("PKU", math = 100, bio = 200, sci = 300)
```

#### 7. 函数

函数的形参为可变对象时，通过形参修改可变对象也会导致实参的改变，也就是说形参和实参这时是共享对象的，如下例子：

```python
def fuck(a, b):
    b = [8, 9]
    a[1] = 6
    return a, b

m = [1, 2, 3]
n = [4, 5]
s = fuck(m, n)
print(m)
print(n)
print(s)
# outputs:
# [1, 6, 3]
# [4, 5]
# ([1, 6, 3], [8, 9])
```

注意在函数中对`b`重新赋值并不会导致`n`的改变，因为这实际上是将`b`重新绑定到另一个对象上去了。

但是如果函数的对象为不可变对象（数值、字符串、元组）时，函数的形参和实参在初始时也是共享同一个对象的，但对形参的“改变”（这里的“改变”当然不是修改对象中的元素，因为形参是不可变的，应该指的是重新绑定其他对象）并不会对实参有影响，如下例子：

```python
def fuck(a, b):
    a = 2
    b = 'xyz'
    return a, b

m = 1
n = "abc"
fxxk = fuck(m, n)
print(fxxk)
print(m, n)
# outputs:
# (2, 'xyz')
# 1 abc
```

