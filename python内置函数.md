# Build-in Functions

## abs(*x*)

返回数值(integer、float)或者实现了`__abs__()`方法的对象

## all(*iterable*)

全部为`Ture`或者为空时返回`Ture`

## any(*iterable*)

任何一个为`Ture`返回`True`，参数为空时返回`False`

## ascii(*object*)

类似repr()，返回能表示对象输出的字符串，对于非ascii字符进行转义。

## bin(*x*)

整数转换成二进制字符串，`0b`开头。

若不是整数，则`__index__()`方法需要返回整数。

## *class* bool([*x*])

bool是int的子类，而`True`和`False`是bool的实例。

## breakpoint(*\*args, \*kws*)

调用[`sys.breakpointhook()`](https://docs.python.org/3/library/sys.html#sys.breakpointhook)

*3.7 加入*

## *class* bytearray([*source*[, *encoding*[, *errors*]]])

返回字节序列，序列中的数介于0与256之间，可变

## *class* bytes([*source*[, *encoding*[, *errors*]]])

返回不可变的字节序列

## callable(*object*)

是否能调用，class是可调用的(创建实例)，如果class有`__call__()`方法则实例也是可调用的

## chr(*i*)

数值转对应的unicode值

```python
>>> chr(97)
'a'
```

## @classmethod

把方法转为类方法

类方法接受class作为隐含的第一个参数

```python
class C:
    @classmethod
    def f(cls, arg1, arg2, ...): ...
```

类方法既可以使用类名调用(`C.f()`)，也可以使用实例调用(`C().f()`)

与C++以及Java的静态方法不同

## compile(*source*, *filename*, *mode*, *flags=0*, *dont_inherit=False*, *optimize=-1*)

## *class* complex([*real*[, *imag*]])

数或字符串转为复数

对于对象`x`，`complex(x)` 对应 `x.__complex__()`。 如果 `__complex__()`没定义那么调用 [`__float__()`](https://docs.python.org/3/reference/datamodel.html#object.__float__)。 If `__float__()` 没定义那么调用 [`__index__()`](https://docs.python.org/3/reference/datamodel.html#object.__index__)。

## delattr(*object*, *name*)

删除对象的某个值域

 `delattr(x, 'foobar')` 等价于 `del x.foobar`

## getattr(*object*, *name*[, *default*])

获取类的属性值，若属性不存在返回default，未提供default抛出[`AttributeError`](https://docs.python.org/3/library/exceptions.html#AttributeError)

## setattr(*object*, *name*, *value*)

## hasattr(*object*, *name*)

含有返回`True`，不含返回`False`，通过调用`getattr(object, name)`实现

## *class* dict(***kwarg*)

## *class* dict(*mapping*, ***kwarg*)

## *class* dict(*iterable*, ***kwarg*)

## dir([*object*])

列出类或包的属性，本来是为了在交互环境中使用。

## divmod(*a*, *b*)

返回除法的商及余数

## enumerate(*iterable*, *start=0*)

```python
>>> seasons = ['Spring', 'Summer', 'Fall', 'Winter']
>>> list(enumerate(seasons))
[(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
>>> list(enumerate(seasons, start=1))
[(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
```

## eval(*expression*[, *globals*[, *locals*]])

如果不指定globals和locals，执行eval()的当前环境将传入

```python
>>> x = 1
>>> eval('x+1')
2
```

## exec(*object*[, *globals*[, *locals*]])

`object`可以是字符串或者代码类

- globals()和locals()方法可以获取当前信息
- 如果不想影响locals可以传入一个locals

## filter(*function*, *iterable*)

根据function产生iterator

```python
# 等价于
# function!=None
(item for item in iterable if function(item))
# function==None
(item for item in iterable if item)
```

## *class* float([*x*])

```
sign           ::=  "+" | "-"
infinity       ::=  "Infinity" | "inf"
nan            ::=  "nan"
numeric_value  ::=  floatnumber | infinity | nan
numeric_string ::=  [sign] numeric_value
```

传入字符串，字符串首尾空白字符会被清除，且大小写不敏感

如果x是类，那么调用`x.__float__()`，不存在则调用`[`__index__()`](https://docs.python.org/3/reference/datamodel.html#object.__index__)`

## format(*value*[, *format_spec*])

*format_spec*默认为空字符串，通常等价于str(value)

转化为`type(value).__format__(value, format_spec)`

## *class* frozenset([*iterable*])

返回[`frozenset`](https://docs.python.org/3/library/stdtypes.html#frozenset)对象，不可变的set

## globals()

返回表示当前全局变量的dict

## locals()

字典。不应该修改其中的值，不一定会改变变量的值。

## hash(*object*)

返回对象的哈希值，哈希值为整数

## help([*object*])

交互界面使用，显示帮助页

## hex(*x*)

整数化为小写字母表示的十六进制。如果`x`不是整数，`__index__()`方法必须返回整数。

想要用大写字母表示，要使用其他方式。

```python
>>> '%#x' % 255, '%x' % 255, '%X' % 255
('0xff', 'ff', 'FF')
>>> format(255, '#x'), format(255, 'x'), format(255, 'X')
('0xff', 'ff', 'FF')
>>> f'{255:#x}', f'{255:x}', f'{255:X}'
('0xff', 'ff', 'FF')
```

## oct(*x*)

与hex类似，化为八进制

## id(*object*)

返回对象的id，一个保证特异且不变的整数。两个生命周期不重叠的对象可能有相同的id。

**CPython实现细节：**对象在内存中的地址

## input([*prompt*])

## *class* int([*x*])

## *class* int(*x*, *base=10*)

x为整数或者字符串。`x.__int__()`到`x.__index__()`到`x.__trunc__()`

如果x不是数字或者传入了`base`，那么`x`必须是字符串，`bytes`或者`bytearray`。

## isinstance(*object*, *classinfo*)

对象是否是类或类的子类的实例，`classinfo`可以是tuple

## issubclass(*class*, *classinfo*)

类是否是另一个类的子类(一个类也被看做是自己的子类)。

`classinfo`可以是tuple

## iter(*object*[, *sentinel*])

如果没有`sentinel`，对象必须是集合对象，含有`__iter__()`；或者序列对象，含有`__getitem__()`。

如果有第二个参数，那么会不断调用类的`__next__()`方法，知道返回`sentinel`值。

读取固定宽度的二进制数据：

```python
from functools import partial
with open('mydata.db', 'rb') as f:
    for block in iter(partial(f.read, 64), b''):
        process_block(block)
```

## len(*s*)

参数可以是序列(string, bytes, tuple, list, or range)，也可以是集合(dictionary, set, or frozen set)。

## *class* list([*iterable*])

可变序列

## map(*function*, *iterable*, *...*)

不断将遍历对象传入函数，yield结果。如果有多个遍历对象，那么都将作为参数传入函数。当最短的遍历对象耗尽时结束。

## max(*iterable*, *[, *key*, *default*])

## max(*arg1*, *arg2*, **args*[, *key*])

如果只传入一个值，那么必须是可遍历的。

key参数指定排序时使用的关键字，default指定对象为空时的返回值。

## min(*iterable*, *[, *key*, *default*])

## min(*arg1*, *arg2*, **args*[, *key*])

与max类似

## next(*iterator*[, *default*])

通过调用对象的`__next__()`方法获取下一个值，如果指定了default，那么当对象耗尽时返回default；否则抛出 [`StopIteration`](https://docs.python.org/3/library/exceptions.html#StopIteration) 

## *class* object

返回一个对象。object是所有的基类。包含所有类共有的方法。

object不包含`__dict__`，所以不能对object的实例的属性赋值。

## open(*file*, *mode='r'*, *buffering=-1*, *encoding=None*, *errors=None*, *newline=None*, *closefd=True*, *opener=None*)

| Character | Meaning                                                      |
| :-------- | :----------------------------------------------------------- |
| `'r'`     | open for reading (default)                                   |
| `'w'`     | open for writing, truncating the file first                  |
| `'x'`     | open for exclusive creation, failing if the file already exists |
| `'a'`     | open for writing, appending to the end of the file if it exists |
| `'b'`     | binary mode                                                  |
| `'t'`     | text mode (default)                                          |
| `'+'`     | open for updating (reading and writing)                      |

编码见[`codecs`](https://docs.python.org/3/library/codecs.html#module-codecs)

errors是一个字符串，指示发生错误时如何处理。(只在text模式下生效)

newline控制 [通用换行符](https://docs.python.org/3/glossary.html#term-universal-newlines) ，默认为通用模式。

- 读取模式下：如果为`None`, `'\n'`, `'\r'`, or `'\r\n'`都被处理为`\n`。如果为`''`，同样为通用模式，只是原始的换行符将被返回。否则以指定的换行符换行。
- 写入模式下：如果为`None`，`\n`会被转化为系统默认换行符（os.linesep）。如果为`''`或`\n`，将不做转换。

如果closefd为`True`且打开的是文件描述符，那么文件关闭后文件描述符依然打开。如果传入的是文件名，那么将报错。

可以传入自定义的opener。文件对象的文件描述符将通过opener打开，opener必须返回一个打开文件描述符。

返回的 [文件对象](https://docs.python.org/3/glossary.html#term-file-object) 类型取决于打开模式。如果是文本模式，那么将返回[`io.TextIOBase`](https://docs.python.org/3/library/io.html#io.TextIOBase) 的子类。如果是二进制模式，那么将返回[`io.BufferedIOBase`](https://docs.python.org/3/library/io.html#io.BufferedIOBase)的子类。

## ord(*c*)

输入字符，返回对应的unicode值

## chr(*c*)

## pow(*base*, *exp*[, *mod*])

pow(base, exp)等价于base**exp

pow(base, exp, mod)比 `pow(base, exp) % mod`)效率更高

## print(**objects*, *sep=' '*, *end='\n'*, *file=sys.stdout*, *flush=False*)

## *class* property(*fget=None*, *fset=None*, *fdel=None*, *doc=None*)

返回一个属性值

```python
class C:
    def __init__(self):
        self._x = None

    def getx(self):
        return self._x

    def setx(self, value):
        self._x = value

    def delx(self):
        del self._x

    x = property(getx, setx, delx, "I'm the 'x' property.")
```

等价于

```python
class C:
    def __init__(self):
        self._x = None

    @property
    def x(self):
        """I'm the 'x' property."""
        return self._x

    @x.setter
    def x(self, value):
        self._x = value

    @x.deleter
    def x(self):
        del self._x
```

## *class* range(*stop*)

## *class* range(*start*, *stop*[, *step*])

返回不可变序列 [`range`](https://docs.python.org/3/library/stdtypes.html#range)

## repr(*object*)

返回代表对象的打印值的字符串。可以通过`__repr__()`方法控制。

## reversed(*seq*)

返回翻转的 [iterator](https://docs.python.org/3/glossary.html#term-iterator)。

seq必须含有`__reversed__()`方法，或者支持序列协议（`__len__()`方法以及`__getitem__()`方法，其参数从零开始）

## round(*number*[, *ndigits*])

对数值进行舍入。

ndigits被忽略或者传入`None`将返回整数，如果舍入的距离一样，将向偶数方舍入。

## *class* set[*iterable*]

返回set对象

## *class* slice(*stop*)

## *class* slice(*start*, *stop*[, *step*])

返回slice对象

## sorted(*iterable*, *, *key=None*, *reverse=False*)

返回新的排序列表

key指定一个方法用来从元素中提取比较值，默认直接用元素本身比较。

排序是稳定的

## @staticmethod

将方法转变为静态方法，和Java的静态方法类似

与classmethod不同，不能访问类中的变量

```python
class C:
    @staticmethod
    def f(arg1, arg2, ...): ...
```

## *class* str(*object=''*)

## *class* str(*object=b''*, *encoding='utf-8'*, *errors='strict'*)

返回对象的字符串版本

## sum(*iterable*, */*, *start=0*)

求和，通常为数值

其他情况下，连接字符串序列的最快方式为 `''.join(sequence)`。

连接一串iterables，使用 [`itertools.chain()`](https://docs.python.org/3/library/itertools.html#itertools.chain)。

## super([*type*[, *object-or-type*]])

返回代理对象，可以用来调用父类或者兄弟类的方法。

在类中，可以无需传入参数

```python
class C(B):
    def method(self, arg):
        super().method(arg)    # This does the same thing as:
                               # super(C, self).method(arg)
```

## *class* tuple([*iterable*])

不可变序列类型

## *class* type(*object*)

## *class* type(*name*, *bases*, *dict*, ***kwds*)

如果是一个参数，返回对象的类型

如果是三个参数，返回一个新的[对象类型](https://docs.python.org/3/library/stdtypes.html#bltin-type-objects)

```python
# 二者返回相同的类型
class X:
    a = 1

X = type('X', (), dict(a=1))
```

## vars([*object*])

返回模块、类、实例或者其他类型的`__dict__`

没有参数时，vars()作用和locals()类似。而locals()只能读取，对字典的更新将被忽略。

## zip(**iterables*)

整合iterables中的元素，返回一个tuple的iterator 。最短的iterable耗尽时结束。

```python
def zip(*iterables):
    # zip('ABCD', 'xy') --> Ax By
    sentinel = object()
    iterators = [iter(it) for it in iterables]
    while iterators:
        result = []
        for it in iterators:
            elem = next(it, sentinel)
            if elem is sentinel:
                return
            result.append(elem)
        yield tuple(result)
```

如果需要那些较长iterator中的元素，使用[`itertools.zip_longest()`](https://docs.python.org/3/library/itertools.html#itertools.zip_longest)

zip结合`*`操作符使用可以unzip数组。

```python
>>> x = [1, 2, 3]
>>> y = [4, 5, 6]
>>> zipped = zip(x, y)
>>> list(zipped)
[(1, 4), (2, 5), (3, 6)]
>>> x2, y2 = zip(*zip(x, y))
>>> x == list(x2) and y == list(y2)
True
```

## `__import__`(*name*, *globals=None*, *locals=None*, *fromlist=()*, *level=0*)

import语句调用该方法，不推荐直接使用该方法，可以使用[`importlib.import_module()`](https://docs.python.org/3/library/importlib.html#importlib.import_module)


