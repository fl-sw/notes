# 包和模块

## 导入模块的写法
导入文件时，可以import这个文件的路径，然后在使用其中的函数时，用全路径加上函数的名字
```python
    import article.rwLog
    article.rwLog.readLog()

    import article.Common.commonTool
    article.Common.commonTool.importTool()
```
上面的写法，在面对较长的路径时，调用的写法很冗长，就可以先用from找到路径，再用import来导入。
这样一来，即使路径的嵌套层级很深也可以很方便的调用。
```python
    from article.Common import commonTool
    commonTool.importTool()
    
    from article import rwLog
    rwLog.readLog()
```

## 包的放置路径
```
>>> import sys
>>> print(sys.path)
```
上面这句可以打印出所有的路径。
```
[
    '',
    'C:\\Users\\30885\\AppData\\Local\\Programs\\Python\\Python38\\python38.zip',
    'C:\\Users\\30885\\AppData\\Local\\Programs\\Python\\Python38\\DLLs',
    'C:\\Users\\30885\\AppData\\Local\\Programs\\Python\\Python38\\lib',
    'C:\\Users\\30885\\AppData\\Local\\Programs\\Python\\Python38',
    'C:\\Users\\30885\\AppData\\Local\\Programs\\Python\\Python38\\lib\\site-packages'
]
```
1.优先再当前目录找  
2.再在内置模块目录中找  
3.外部模块目录site-packages  
4.从前往后，一旦找到了就不去后面的路径找了
___
可以向path中添加路径：
```
import sys
sys.path.append()
```

## 模块的特性
## 内置变量`__name__`
每个模块中都有一个`__name__`变量。依据这个变量的值，可以做单元测试，可以辨识模块的入口函数
* 当作为主文件被执行时，`__name__`就等于`__main__`
* 当作为被导入的文件时，`__name__`等于`模块的名字`  

***python3不区分模块和文件夹，但是python2会区分。  
在python2的文件夹中如果存在`__init__.py`文件，这个文件夹才会被当作一个包，进而扫描里面的文件并且可被导入。***

## 常用模块
### sys
```python
sys.argv    # 中以列表的形式存放着执行参数
```
### random
```python
import random
val = random.random()  #andom float:  0.0 <= x < 1.0
val = random.randint(4, 6)  # 随机整数
val = random.uniform(1.3, 2.1)  # 随机小数
val = random.choice(("apple", "banana", "pear"))  # 随机选择一个
val = random.sample(["apple", "banana", "pear", "melon"], 2)  # 随机采样2个
```

### hashlib
```python
import hashlib
obj = hashlib.md5("JiaYan".encode("utf-8"))  # 加盐
obj.update("admin".encode("utf-8"))
res = obj.hexdigest()
print(res)
```

### json
```python
import json
data_info = {'code': 257, 'category': ['apple', 'banana', 'pear', 'melon']}
str_json = json.dumps(data_info, ensure_ascii=False)
print(str_json)
data_info = json.loads(str_json)
print(data_info)
```

### time datetime
```python
import time
from datetime import datetime
v1 = time.time()
print(v1)
v2 = datetime.now().strftime("%y=%m=%d~~%H/%M/%S")
print(v2)
```
按日期时间分隔文件写日志:
```python
while True:
    name = input("name:")
    password = input("password:")
    strDateTime = datetime.now().strftime("%Y-%m-%d-%H-%M-%S") #这里按秒
    fileName = "{}.txt".format(strDateTime)
    with open(file=fileName, mode="a", encoding="utf-8") as file:
        file.write("name:{}\npswd:{}".format(name, password))
```

### os
创建文件夹，当文件夹不存在时
```python
logFolderName = datetime.now().strftime("%Y-%m-%d")
if not os.path.exists(logFolderName):
    os.mkdir(logFolderName)
```
判断是否是文件夹
```python
print(os.path.isdir(logFolderName))
print(os.path.isdir(logFileName))
```
删除文件，删除文件夹
```python
os.remove(logFileName)
import shutil
shutil.rmtree(logFolderName)
```
递归打印文件夹中的内容
```python
def test5(folderName):
    itemList = os.listdir(folderName)
    for i in itemList:
        if os.path.isdir(folderName + "\\" + i):
            test5(folderName + "\\" + i)
        else:
            print(i)
```
递归打印文件夹中的内容,也可以用os的walk函数：
```python
def test6(folderName):
    for tempPath, tempFolderName, tempFileName in os.walk(folderName):
        # walk函数返回一个元组，第一项是当前路径，第二项是当前路径下的文件夹，第三项是当前路径下的文件
        # ('D:\2022-12-28', ['newDir'], ['22-04-42.txt', '22-04-44.txt', '22-04-46.txt'])
        tempTuple = tempFolderName + tempFileName  # 不管是文件夹还是文件，拼接到一个元组中
        for i in tempTuple:
            print(os.path.join(tempPath, i))
# output:
# D:\2022-12-28\newDir
# D:\2022-12-28\22-04-42.txt
# D:\2022-12-28\22-04-44.txt
# D:\2022-12-28\22-04-46.txt
# D:\2022-12-28\newDir\a.txt
# D:\2022-12-28\newDir\b.txt
```
### shutil
```python
import shutil
shutil.rmtree(logFolderName)    # 删除文件夹
shutil.move(oldName, newName)   # 文件重命名/文件夹重命名
shutil.copytree(orifolderName, destFolderName)  # 拷贝文件夹
shutil.copy(oriFileName, destFileName)          # 拷贝文件
shutil.make_archive("20221228", "zip", "./", "2022-12-28")  # 压缩文件夹
shutil.unpack_archive("20221228.zip", "./unpacked/", format="zip")  # 解压缩
```
### congfigparser
```python
import configparser
configPS = configparser.ConfigParser()
baseDir = os.path.dirname(os.path.abspath(__file__))
filePath = os.path.join(baseDir, "setting.ini")
configPS.read(filePath, encoding="utf-8")
# 读取所有节点
print(configPS.sections())  # ['main', 'conveyor', 'LCU']
# 获取某个节点下所有的键
for item in configPS.options("main"):
    print(item)
# 获取某个节点下所有的键值对
for k, v in configPS.items("main"):
    print(k, v)
# 获取节点.键对应的值
print(configPS.get("main", "language"))
# 判断是否存在某个节点
print(configPS.has_section("lcu"))  # 是区分大小写的
# 判断是否存在某个键
print(configPS.has_option("LCU", "host"))
# 添加节点和键值
if not configPS.has_section("weigher"):
    configPS.add_section("weigher")
    if not configPS.has_option("weigher", "host"):
        configPS.set("weigher", "host", "7.7.7.11")
    if not configPS.has_option("weigher", "port"):
        configPS.set("weigher", "port", "20301")
# 删除某个键值
if configPS.has_option("weigher", "port"):
    configPS.remove_option("weigher", "port")
# 修改某个键值
configPS.set("LCU", "port", "20301")
# 将内存里的数据更新到文件
with open(file=filePath, mode="w", encoding="utf-8") as f:
    configPS.write(f)
```
### xml
```python
dataDict = {}
tree = ElementTree.parse("nitf.xml")
rootNode = tree.getroot()
for child in rootNode:
    dataDict[child.tag] = child.text
```

### re
```python
text = """This module’s encoders and decoders preserve input and output order by default.appter,apptor,fl12345saf,
    fl12asgadse
    talsfndjgapple
    email:flfox@gmail.com邮箱
    tell:18945674321
    address:tokyo ootaku 1-2-3-606"""
    res = re.findall(r"appt[oe]", text)  # 方括号里的字母表示接下来匹配一个o或者e
    res = re.findall(r"appt[a-z]", text)  # 方括号里也可以写范围,[a-z][0-9]都可以
    res = re.findall(r"t\d{5}", text)  # \d代表数字，后面再跟上花括号5代表5个数字
    res = re.findall(r"t\d{2,}", text)  # 花括号里面也可以写范围，{2,}代表两个以上，{2,7}代表2个以上7个以下
    res = re.findall(r"1[358]\d{9}", text)  # 电话号码，以1开头，第二位是3或5或8，再跟9位数字
    res = re.findall(r"\d+-\d+-\d+-\d+", text)  # +号代表一个或多个，*号代表零个或多个，?号代表零个或一个
    res = re.findall(r"p\w+e", text)  # \w代表字母，数字，下划线，或者汉字
    res = re.findall(r"m.+?s", text)  # .代表除换行外的任意字符，如果要配置.这个符号的话，可以加\进行转义。默认是贪婪匹配，再代表数量的+号后面加一个?就可以取消贪婪匹配
    res = re.findall(r"\w+@\w+\.\w+", text, re.A)  # 文本@文本.文本，匹配邮箱。后面的re.A代表只匹配ASCII字符
    res = re.findall(r"\w+@(\w+)\.\w+", text, re.A)  # 用圆括号分组，先匹配到邮箱，然后再匹配邮箱地址的域名主体
    res = re.findall(r"(\w+@(\w+)\.\w+)", text, re.A)  # 如果也想要匹配到整个邮箱的话，再在外面加个圆括号
    res = re.findall(r"(fl(\d{5}|\d+\w+e))", text, re.A)  # 圆括号里面用|表示或的意思（fl\d{5}或者fl\d+\w+e）。匹配到的结果是子组，若也想要整体的结果，还需要在最外面加上圆括号
    res = re.findall(r"^fl.*m$", text, re.A)  # 开始结束符，要求text这个字符串以O开始以O结束
    res = re.findall(r"[a-zA-Z0-9-_]+@[a-zA-Z0-9-_]+\.[a-zA-Z0-9-_]+", text)  # 邮箱
    re.findall() #在text中寻找符合正则表达式的文本片段
    re.match() #从text的开头进行匹配，返回匹配到的第一个结果
    re.search() # 从前到后进行匹配，返回匹配到的第一个结果
    res = re.split(r"\w{2,3}put", text)  # 根据匹配到的片段对text进行切割，片段会被舍弃
```

## 输入输出
```python
name = input('please input your name:')
print('hello, %s' % name)
```
### input()和raw_input()的区别：
* input()和raw_input()两者都可以接收字符串
* raw_input()将所有的输入都看做字符串
* python2中的input()接收一个合法的python表达式，也就是说输入一个字符串的时候要用引号括起来
* python2中`input() = eval(raw_input())` (*eval()执行一个字符串表达式，并返回表达式的值。比如->eval('2 + 2')会返回4*) 因为使用了eval()所以会有代码注入的风险
* 在python3中移除了input()而将raw_input()更名为input()，只接收字符串，移除了代码注入的风险

### print函数
```python
print("www", "flsw", "com", sep=".", end=";", file=myFile)
objects -- 表示输出的对象。输出多个对象时，需要用 , （逗号）分隔。
sep -- 用来间隔多个对象。默认是空格
end -- 用来设定以什么结尾。默认值是换行符 \n，下面的例子中换成了分号。
file -- 要写入的文件对象。
```
比如：
```python
myFile = open("./flsw.txt", 'w', encoding='utf-8')
print("www", "flsw", "com", sep=".", file=myFile)
```
打开文件并写入->www.flsw.com;

#### 格式化输出
```
%s      字符串采用str()的显示
%r      字符串(repr())的显示
%c      单个字符
%b      二进制整数
%d      十进制整数
%i      十进制整数
%o      八进制整数
%x      十六进制整数
%e      指数（基底写e）
%E      指数（基底写E）
%f,%F   浮点数
%g      指数(e)或浮点数(根据显示长度)
%G      指数(E)或浮点数(根据显示长度)
%%      字符%

最小字段宽度：输出字符串占位数。如果是*（星号），则宽度会从值元组中读出。
```

比如：
```python
PI = 3.1415926
charWidth = 6           # 总共占位数
decimalPrecision = 2    # 保留小数点后位数
print("PI=%*.*f" % (charWidth, decimalPrecision, PI))
# 指定占位数和小数点位数时，可以用*代替变量，具体的值要按顺序放在元组中
```
打印->`PI=  3.14` #前面有两个空格，总共占6个字符位，小数点后保留两位。小数点也占一个字符位

```python
print('%-10.3f' % PI)  # 左对齐，还是10个字符，但空格显示在右边
print('%+f' % PI)      # 显示正负号  #+3.141593 浮点数的默认精度为6位小数。
print('%010.3f' % PI)  # 字段宽度为10，精度为3，不足处用0填充空白
```

## 数据类型
### list
```python
s = ['python', 'java', ['asp', 'php'], 'scheme']
```
* len()函数可以获得list元素的个数  
* 可以用-1做索引，直接获取最后一个元素：s[-1]
* 追加元素到末尾用append()
* 插入到指定位置用insert(1, 'Jack')，后面的元素依次向后顺移
* 删除末尾的元素用pop(),删除指定位置可以pop(1),后面的元素会依次向前顺移

### tuple
```python
classmates = ('Michael', 'Bob', 'Tracy')
```
* tuple和list非常类似，但是tuple一旦初始化就不能修改
* tuple没有append()，insert()这样的方法。其他获取元素的方法和list是一样的，可以正常地使用classmates[0]，classmates[-1]，但不能赋值成另外的元素。
* 定义一个空的tuple，可以写成t = ()
* 只有1个元素的tuple定义时必须加一个逗号,t = (5,)
* 可变的tuplet ，t = ('a', 'b', ['A', 'B'])

### dictionary
```python
d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
```
* 取值方式：d['Adam'] = 67
* 通过in判断key是否存在`'Thomas' in d`会返回False
* 通过get()方法判断key是否存在，如果key不存在，可以返回None，或者自己指定的value`d.get('Thomas', -1)`返回-1
* 要删除一个key，用pop(key)方法

### set
```python
 s = set([1, 1, 2, 2, 3, 3])    #重复的元素会自动被过滤
```
* set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key
* 通过add(key)方法可以添加元素到set中，可以重复添加，但不会有效果
* 通过remove(key)方法可以删除元素

## 逻辑控制语句
### 条件判断
```python
if
    pass
elif
    pass
else
    pass
```
### 循环
```python
for i in ...
while
break
continue
```

## 函数


### 内置函数
```python
abs(x)          # 返回绝对值
all()           # all() 函数用于判断给定的可迭代参数 iterable 中的所有元素是否都为 TRUE，如果是返回 True，否则返回 False。
any()           # 用于判断给定的可迭代参数 iterable 是否全部为 False，则返回 False，如果有一个为 True，则返回 True。元素除了是 0、空、FALSE 外都算 TRUE
str() 函数将对象转化为适于人阅读的形式。
bin() 返回一个整数 int 或者长整数 long int 的二进制表示。
bool() 函数用于将给定参数转换为布尔类型，如果没有参数，返回 False。
bytearray() 方法返回一个新字节数组。这个数组里的元素是可变的，并且每个元素的值范围: 0 <= x < 256。
callable() 函数用于检查一个对象是否是可调用的。如果返回 True，object 仍然可能调用失败；但如果返回 False，调用对象 object 绝对不会成功。
chr() 用一个范围在 range（256）内的（就是0～255）整数作参数，返回一个对应的字符。
classmethod 修饰符对应的函数不需要实例化，不需要 self 参数，但第一个参数需要是表示自身类的 cls 参数，可以来调用类的属性，类的方法，实例化对象等。static化。
cmp(x,y) 函数用于比较2个对象，如果 x < y 返回 -1, 如果 x == y 返回 0, 如果 x > y 返回 1。
compile() 函数将一个字符串编译为字节代码。
complex() 函数用于创建一个值为 real + imag * j 的复数或者转化一个字符串或数为复数。如果第一个参数为字符串，则不需要指定第二个参数。
delattr 函数用于删除属性。
dict() 函数用于创建一个字典。
dir() 函数不带参数时，返回当前范围内的变量、方法和定义的类型列表；带参数时，返回参数的属性、方法列表。
python divmod() 函数把除数和余数运算结果结合起来，返回一个包含商和余数的元组(a // b, a % b)。
enumerate() 函数用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标，一般用在 for 循环当中。
eval() 函数用来执行一个字符串表达式，并返回表达式的值。
execfile() 函数可以用来执行一个文件。
file() 函数用于创建一个 file 对象，它有一个别名叫 open()，更形象一些，它们是内置函数。参数是以字符串的形式传递的。
filter() 函数用于过滤序列，过滤掉不符合条件的元素，返回由符合条件元素组成的新列表。
float() 函数用于将整数和字符串转换成浮点数。
str.format() 格式化字符串的函数，它增强了字符串格式化的功能。
frozenset() 返回一个冻结的集合，冻结后集合不能再添加或删除任何元素。
getattr() 函数用于返回一个对象属性值。
globals() 函数会以字典类型返回当前位置的全部全局变量。
hasattr() 函数用于判断对象是否包含对应的属性。
hash() 用于获取取一个对象（字符串或者数值等）的哈希值。
help() 函数用于查看函数或模块用途的详细说明。
hex() 函数用于将10进制整数转换成16进制，以字符串形式表示。
id() 函数返回对象的唯一标识符，标识符是一个整数。CPython 中 id() 函数用于获取对象的内存地址。
input() 函数接受一个标准输入数据，返回为 string 类型。
int() 函数用于将一个字符串或数字转换为整型。
isinstance() 函数来判断一个对象是否是一个已知的类型，类似 type()。
isinstance() 与 type() 区别：type() 不会认为子类是一种父类类型，不考虑继承关系。isinstance() 会认为子类是一种父类类型，考虑继承关系。
issubclass() 方法用于判断参数 class 是否是类型参数 classinfo 的子类。 如果class 是 classinfo 的子类则返回 True，否则返回 False。
iter() 函数用来生成迭代器。
len() 方法返回对象（字符、列表、元组等）长度或项目个数。
list() 方法用于将元组转换为列表。元组与列表是非常类似的，区别在于元组的元素值不能修改，元组是放在括号中，列表是放于方括号中。
locals() 函数会以字典类型返回当前位置的全部局部变量。
map() 会根据提供的函数对指定序列做映射。第一个参数 function 以参数序列中的每一个元素调用 function 函数，返回包含每次 function 函数返回值的新列表。
max() 方法返回给定参数的最大值，参数可以为序列。
memoryview() 函数返回给定参数的内存查看对象(memory view)。所谓内存查看对象，是指对支持缓冲区协议的数据进行包装，在不需要复制对象基础上允许Python代码访问。
min() 方法返回给定参数的最小值，参数可以为序列。
next() 返回迭代器的下一个项目。next() 函数要和生成迭代器的 iter() 函数一起使用。
oct() 函数将一个整数转换成 8 进制字符串。
open() 函数用于打开一个文件，创建一个 file 对象，相关的方法才可以调用它进行读写。
ord() 函数是 chr() 函数（对于8位的ASCII字符串）或 unichr() 函数（对于Unicode对象）的配对函数，它以一个字符（长度为1的字符串）作为参数，返回对应的 ASCII 数值，或者 Unicode 数值
pow() 方法返回 xy（x 的 y 次方） 的值。
print() 方法用于打印输出，最常见的一个函数。
property() 函数的作用是在新式类中返回属性值。
range() 函数可创建一个整数列表，一般用在 for 循环中。
raw_input() 用来获取控制台的输入。raw_input() 将所有输入作为字符串看待，返回字符串类型。
reduce() 函数会对参数序列中元素进行累积。
reload() 用于重新载入之前载入的模块。
repr() 函数将对象转化为供解释器读取的形式。返回一个对象的 string 格式。
reverse() 函数用于反向列表中元素。该方法没有返回值，但是会对列表的元素进行反向排序。
round() 方法返回浮点数x的四舍五入值。
set() 函数创建一个无序不重复元素集，可进行关系测试，删除重复数据，还可以计算交集、差集、并集等。
setattr() 函数对应函数 getattr()，用于设置属性值，该属性不一定是存在的。
slice() 函数实现切片对象，主要用在切片操作函数里的参数传递。
sorted() 函数对所有可迭代的对象进行排序操作。sort 与 sorted 区别：sort 是应用在 list 上的方法，sorted 可以对所有可迭代的对象进行排序操作。list 的 sort 方法返回的是对已经存在的列表进行操作，无返回值，而内建函数 sorted 方法返回的是一个新的 list，而不是在原来的基础上进行的操作。
staticmethod() 返回函数的静态方法。
str() 函数将对象转化为适于人阅读的形式。
sum() 方法对序列进行求和计算。
super() 函数是用于调用父类(超类)的一个方法。
tuple() 函数将列表转换为元组。
type() 函数如果你只有第一个参数则返回对象的类型，三个参数返回新的类型对象(相当于定义新类型)。
vars() 函数返回对象object的属性和属性值的字典对象。
xrange() 函数用法与 range 完全相同，所不同的是生成的不是一个数组，而是一个生成器。
zip() 函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的列表。
__import__() 函数用于动态加载类和函数。如果一个模块经常变化就可以使用 __import__() 来动态载入。
```


## 函数参数
对于任意函数，都可以通过类似`func(*args, **kw)`的形式调用它，无论它的参数是如何定义的。

### 参数组合
* 在Python中定义函数，可以用必选参数、默认参数、可变参数、关键字参数和命名关键字参数，这5种参数都可以组合使用。
* 参数定义的顺序必须是：__必选参数、默认参数、可变参数、命名关键字参数和关键字参数。__

### 必选参数
```python
def func(x, n):
    pass
func(5, 3)
```

### 默认参数
```python
def func(x, n=2):
    pass
func(5)
```
* 必选参数在前，默认参数在后，否则Python的解释器会报错
* 有多个默认参数时，也可以不按顺序提供部分默认参数，但需要把参数名写上。
* 在Python中默认参数是一个指针时，要注意多次调用实际上操作的仍然是同一块内存。

### 可变参数
```python
def func(*nums)
```
定义可变参数和定义一个list或tuple参数相比，仅仅在参数前面加了一个*号。
在函数内部，参数nums接收到的是一个tuple，因此，函数代码完全不变。但是，调用该函数时，可以传入任意个参数，包括0个参数
*nums表示把nums这个list的所有元素作为可变参数传进去

### 命名关键字参数
```python

```
### 关键字参数
```python

```



```python

```