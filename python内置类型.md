# Build-in Types

## 真值测试

任何对象都能测试是否为真，用来条件判断或者布尔运算

默认情况下，一个对象会被判断为真，除非`__bool__()`方法返回False或者`__len__()`方法返回0。

被认为`False`的几种情况：

- 几种常量：`None`和`False`
- 数值中的0：`0`，`0.0`， `0j`, `Decimal(0)`, `Fraction(0, 1)`
- 空的序列或者集合： `''`, `()`, `[]`, `{}`, `set()`, `range(0)`

返回布尔结果的操作和内置函数总是返回 `0` 或者 `False` 以及`1` 或者 `True` 

## 布尔运算 — and,or,not

and和or是短路运算，后面的运算可能不会进行

not的优先级比非布尔运算低

## 比较

一共八种比较运算，他们的优先级相同，可以被链式组合

如`x < y <= z`等价于 `x < y and y <= z`

| Operation | Meaning                 |
| :-------- | :---------------------- |
| `<`       | strictly less than      |
| `<=`      | less than or equal      |
| `>`       | strictly greater than   |
| `>=`      | greater than or equal   |
| `==`      | equal                   |
| `!=`      | not equal               |
| `is`      | object identity         |
| `is not`  | negated object identity |

`==`操作符总是有效，但是对于一些类型等价于`is`。而 `<`, `<=`, `>` 和 `>=`等只在有意义时有效，否则会抛出异常。 

不相同的实例通常会被比较为不相等，除非类定义了`__eq__()`方法。

类的实例不能排序，除非类定义了[`__lt__()`](https://docs.python.org/3/reference/datamodel.html#object.__lt__), [`__le__()`](https://docs.python.org/3/reference/datamodel.html#object.__le__), [`__gt__()`](https://docs.python.org/3/reference/datamodel.html#object.__gt__), 和 [`__ge__()`](https://docs.python.org/3/reference/datamodel.html#object.__ge__)。通常， [`__lt__()`](https://docs.python.org/3/reference/datamodel.html#object.__lt__) and [`__eq__()`](https://docs.python.org/3/reference/datamodel.html#object.__eq__) 就足够了。

另外两个优先级相同的操作符`in`和`not in`，被 [iterable](https://docs.python.org/3/glossary.html#term-iterable) 对象和实现了 [`__contains__()`](https://docs.python.org/3/reference/datamodel.html#object.__contains__) 方法的类型支持。

## 数值类型 — int, float, complex

布尔类型为int的子类型，integers精度不受限，float通常继承自C语言的double。关于float的精度和表示信息可用命令 [`sys.float_info`](https://docs.python.org/3/library/sys.html#sys.float_info)。

整值会产生integer，带小数点或者指数会产生float，数值后加上j或者J会产生复数的虚部。

python支持混合运算，不同类型的数运算时较窄的类型会被转换为另一类型。

所有数值类型支持一下操作符（除了复数）

|                   |                                                              |        |                                                              |
| :---------------- | :----------------------------------------------------------- | :----- | :----------------------------------------------------------- |
| Operation         | Result                                                       | Notes  | Full documentation                                           |
| `x + y`           | sum of *x* and *y*                                           |        |                                                              |
| `x - y`           | difference of *x* and *y*                                    |        |                                                              |
| `x * y`           | product of *x* and *y*                                       |        |                                                              |
| `x / y`           | quotient of *x* and *y*                                      |        |                                                              |
| `x // y`          | floored quotient of *x* and *y*                              | (1)    |                                                              |
| `x % y`           | remainder of `x / y`                                         | (2)    |                                                              |
| `-x`              | *x* negated                                                  |        |                                                              |
| `+x`              | *x* unchanged                                                |        |                                                              |
| `abs(x)`          | absolute value or magnitude of *x*                           |        | [`abs()`](https://docs.python.org/3/library/functions.html#abs) |
| `int(x)`          | *x* converted to integer                                     | (3)(6) | [`int()`](https://docs.python.org/3/library/functions.html#int) |
| `float(x)`        | *x* converted to floating point                              | (4)(6) | [`float()`](https://docs.python.org/3/library/functions.html#float) |
| `complex(re, im)` | a complex number with real part *re*, imaginary part *im*. *im* defaults to zero. | (6)    | [`complex()`](https://docs.python.org/3/library/functions.html#complex) |
| `c.conjugate()`   | conjugate of the complex number *c*                          |        |                                                              |
| `divmod(x, y)`    | the pair `(x // y, x % y)`                                   | (2)    | [`divmod()`](https://docs.python.org/3/library/functions.html#divmod) |
| `pow(x, y)`       | *x* to the power *y*                                         | (5)    | [`pow()`](https://docs.python.org/3/library/functions.html#pow) |
| `x ** y`          | *x* to the power *y*                                         | (5)    |                                                              |

 [`numbers.Real`](https://docs.python.org/3/library/numbers.html#numbers.Real)  类型（int、float）包含以下操作：

| Operation                                                    | Result                                                       |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`math.trunc(x)`](https://docs.python.org/3/library/math.html#math.trunc) | *x* truncated to [`Integral`](https://docs.python.org/3/library/numbers.html#numbers.Integral) |
| [`round(x[, n\])`](https://docs.python.org/3/library/functions.html#round) | *x* rounded to *n* digits, rounding half to even. If *n* is omitted, it defaults to 0. |
| [`math.floor(x)`](https://docs.python.org/3/library/math.html#math.floor) | the greatest [`Integral`](https://docs.python.org/3/library/numbers.html#numbers.Integral) <= *x* |
| [`math.ceil(x)`](https://docs.python.org/3/library/math.html#math.ceil) | the least [`Integral`](https://docs.python.org/3/library/numbers.html#numbers.Integral) >= *x* |

### 整数位运算

|           |                                       |        |
| :-------- | :------------------------------------ | :----- |
| Operation | Result                                | Notes  |
| `x | y`   | bitwise *or* of *x* and *y*           | (4)    |
| `x ^ y`   | bitwise *exclusive or* of *x* and *y* | (4)    |
| `x & y`   | bitwise *and* of *x* and *y*          | (4)    |
| `x << n`  | *x* shifted left by *n* bits          | (1)(2) |
| `x >> n`  | *x* shifted right by *n* bits         | (1)(3) |
| `~x`      | the bits of *x* inverted              |        |

### 整数的一些方法

整数实现了 [`numbers.Integral`](https://docs.python.org/3/library/numbers.html#numbers.Integral)抽象类，提供了一些额外的方法。

#### int.bit_length()

二进制比特数，为0则返回0

#### int.to_bytes(*length*, *byteorder*, *, *signed=False*)

byteorder可以是`little`或者`big`

big是较大的位数在左端（平时习惯的那样）

#### *classmethod* int.from_bytes(*bytes*, *byteorder*, ***, *signed=False*)

二进制转整数

#### int.as_integer_ratio()

返回分子、分母

### 浮点数的一些方法

浮点数实现了 [`numbers.Real`](https://docs.python.org/3/library/numbers.html#numbers.Real) 抽象类

#### float.as_integer_ratio()

返回分子、分母

#### float.is_integer()

是否只有整数部分

#### float.hex()

返回浮点数的16进制字符串，浮点数和16进制之间转换不会有精度损失

#### *classmethod* float.fromhex(*s*)

从16进制字符串转为浮点型

### 数值类型的哈希

## 迭代器类型

容器对象需要定义一个方法以支持迭代器

#### `container.__iter__`()

返回迭代器对象

迭代器对象需要有下面两个方法，组成了迭代器协议

#### `iterator.__iter__`()

返回迭代器对象本身

#### `iterator.__next__`()

返回容器的下一个对象，没有时抛出[`StopIteration`](https://docs.python.org/3/library/exceptions.html#StopIteration)

### 生成器类型

生成器为实现迭代器协议提供了便利。通过yield返回值，不用定义iter和next方法。

## 序列类型 — list, tuple, range

### 通用序列操作

| Operation              | Result                                                       | Notes  |
| :--------------------- | :----------------------------------------------------------- | :----- |
| `x in s`               | `True` if an item of *s* is equal to *x*, else `False`       | (1)    |
| `x not in s`           | `False` if an item of *s* is equal to *x*, else `True`       | (1)    |
| `s + t`                | the concatenation of *s* and *t*                             | (6)(7) |
| `s * n` or `n * s`     | equivalent to adding *s* to itself *n* times                 | (2)(7) |
| `s[i]`                 | *i*th item of *s*, origin 0                                  | (3)    |
| `s[i:j]`               | slice of *s* from *i* to *j*                                 | (3)(4) |
| `s[i:j:k]`             | slice of *s* from *i* to *j* with step *k*                   | (3)(5) |
| `len(s)`               | length of *s*                                                |        |
| `min(s)`               | smallest item of *s*                                         |        |
| `max(s)`               | largest item of *s*                                          |        |
| `s.index(x[, i[, j]])` | index of the first occurrence of *x* in *s* (at or after index *i* and before index *j*) | (8)    |
| `s.count(x)`           | total number of occurrences of *x* in *s*                    |        |

按照优先级升序排序，in和not in优先级相同，+和*优先级相同

相同类型的序列支持排序。特别的，tuple和list比较时会一一对照比较。

1. 一些特殊的序列也支持in，not in操作
2. *的时候序列的元素不是深拷贝
3. i或j为负数时，实际上为len(s)+i。i省略或者为`None`，则为0；j省略或者为`None`，则为len(s)。i大于等于j，结果为空。
4. 连接不可变序列总是会生成新的序列，这意味着重复连接一个序列会使时间复杂度倍增。要实现线性复杂度，必须使用下面的方法：
   - str使用str.join()或者写到io.StringIO
   - bytes使用bytes.join()或者[`io.BytesIO`](https://docs.python.org/3/library/io.html#io.BytesIO)
   - tuple则extend为list
   - 其余的参考相关文档

### 不可变序列类型

不可变序列独有的方法只有hash()

这使得不可变序列可以作为dict的key，存储在set和forzenset中

### 可变序列类型

| Operation                 | Result                                                       | Notes |
| :------------------------ | :----------------------------------------------------------- | :---- |
| `s[i] = x`                | item *i* of *s* is replaced by *x*                           |       |
| `s[i:j] = t`              | slice of *s* from *i* to *j* is replaced by the contents of the iterable *t* |       |
| `del s[i:j]`              | same as `s[i:j] = []`                                        |       |
| `s[i:j:k] = t`            | the elements of `s[i:j:k]` are replaced by those of *t*      | (1)   |
| `del s[i:j:k]`            | removes the elements of `s[i:j:k]` from the list             |       |
| `s.append(x)`             | appends *x* to the end of the sequence (same as `s[len(s):len(s)] = [x]`) |       |
| `s.clear()`               | removes all items from *s* (same as `del s[:]`)              | (5)   |
| `s.copy()`                | creates a shallow copy of *s* (same as `s[:]`)               | (5)   |
| `s.extend(t)` or `s += t` | extends *s* with the contents of *t* (for the most part the same as `s[len(s):len(s)] = t`) |       |
| `s *= n`                  | updates *s* with its contents repeated *n* times             | (6)   |
| `s.insert(i, x)`          | inserts *x* into *s* at the index given by *i* (same as `s[i:i] = [x]`) |       |
| `s.pop([i])`              | retrieves the item at *i* and also removes it from *s*       | (2)   |
| `s.remove(x)`             | remove the first item from *s* where `s[i]` is equal to *x*  | (3)   |
| `s.reverse()`             | reverses the items of *s* in place                           | (4)   |

### Lists

可变序列，通常存储同类型元素。数列除了以上方法外，还提供了以下方法

#### sort(*, *key=None*, *reverse=False*)

原地排序，只用<进行比较，不返回序列。排序稳定。

### Tuples

不可变序列，通常用来存储不同类型的元素

一般情况下定义tuple时括号不是必须的，有逗号即可

### Ranges

range表示数字的不可变序列

#### *class* range(*stop*)

#### *class* range(*start*, *stop*[, *step*])

省略step，step为1；省略start，start为0

range的优势在于只占用固定大小的内存

## 文本序列类型 — str

str是用unicode表示的不可变序列，可以用单引号、双引号、三个单双引号定义

单个表达式中的之后空格符隔开的多个字符串会隐含地被转换为一个字符串，如 `("spam " "eggs") == "spam eggs"`。

### *class* str(*object=''*)

### *class* str(*object=b''*, *encoding='utf-8'*, *errors='strict'*)

如果不指定编码以及错误，返回 [`object.__str__()`](https://docs.python.org/3/reference/datamodel.html#object.__str__)，没有则返回 [`repr(object)`](https://docs.python.org/3/library/functions.html#repr)。

如果至少提供了一个，则object应该为字节流类型。为bytes，则 `str(bytes, encoding, errors)` 等价于 [`bytes.decode(encoding, errors)`](https://docs.python.org/3/library/stdtypes.html#bytes.decode)。

### 字符串方法

字符串支持通用的序列方法

字符串支持两种格式化，一种 [Format String Syntax](https://docs.python.org/3/library/string.html#formatstrings) 更为灵活，另一种基于Cprintf格式，效率更高。

#### str.capitalize()

返回首字母大写、剩余字母小写的复制字符串。

#### str.casefold()

类似于小写化，但试图将不同语言小写话。

#### str.center(*width*[, *fillchar*])

将字符串居中并填充，若字符串长度大于等于width，则返回原字符串

#### str.count(*sub*[, *start*[, *end*]])

sub在字符串中出现的次数，start,end会被用来切片

#### str.encode(*encoding="utf-8"*, *errors="strict"*)

返回字节，`strict`下有错误会抛出`UnicodeError`。

#### str.endswith(*suffix*[, *start*[, *end*]])

是否以suffix结尾，suffix可以为tuple

#### str.expandtabs(*tabsize=8*)

返回替换tab后的结果

#### str.find(*sub*[, *start*[, *end*]])

返回sub的位置，-1则不存在

如果只想判断是否含有sub，使用`in`

```python
>>> 'Py' in 'Python'
True
```

#### str.rfind(*sub*[, *start*[, *end*]])

返回最大下标

#### str.format(*\*args, **kwargs*)

格式化字符串

#### str.format_map(*mapping*)

与 `str.format(**mapping)`相似，区别在于直接使用mapping而不拷贝。

#### str.index(*sub*[, *start*[, *end*]])

类似find，没找到时抛出ValueError

#### str.rindex(*sub*[, *start*[, *end*]])

最大下标

#### str.isalnum()

所有的字符是否为数字或者字母

#### str.isalpha()

是否全部为字母，字母为Unicode中定义为Letter的字符，即那些具有 "Lm"、"Lt"、"Lu"、"Ll" 或 "Lo" 之一的通用类别属性的字符。

#### str.isascii()

如果字符串为空或者所有字符都是ASCII，返回`True`。

*3.7 新版功能.*

#### str.isdecimal()

所有字符都是十进制字符

#### str.isdigit()

所有字符都是数字

#### str.isidentifier()

字符串是有效的标识符，keyword.iskeyword()可以检测是否为保留标识符

```python
>>> from keyword import iskeyword

>>> 'hello'.isidentifier(), iskeyword('hello')
True, False
>>> 'def'.isidentifier(), iskeyword('def')
True, True
```

#### str.islower()

至少有一个区分大小写字符且均为小写返回`True`

#### str.isnumeric()

均为数值字符

#### str.isprintable()

均为可打印字符或字符串为空返回`True`。不可打印字符在Unicode中定义为"Other"或者"Separator"（空格例外）。

#### str.isspace()

只有空白字符

#### str.istitle()

是否为标题

#### str.isupper()

至少有一个区分大小写字母且所有区分大小写的全为大写

#### str.join(*iterable*)

拼接iterable中的字符串

#### str.ljust(*width*[, *fillchar*])

字符串扩充后左对齐，如果字符串长度大于等于width，返回原字符串

#### str.rjust(*width*[, *fillchar*])

右对齐

#### str.lower()

返回小写

#### *static* str.maketrans(*x*[, *y*[, *z*]])

返回转换表，提供给str.translate()使用

#### str.partition(*sep*)

返回一个三个元素的元组，分隔符前，分隔符，分隔符后。

若不包含分隔符，则返回字符串，空，空

#### str.rpartision(*sep*)

以最后出现的分隔符位置进行分割

#### str.removeprefix(*prefix*, */*)

注意与str.lstrip()差异

```python
>>> 'TestHook'.removeprefix('Test')
'Hook'
>>> 'BaseTestCase'.removeprefix('Test')
'BaseTestCase'
```

*New in version 3.9.*

#### str.removesuffix(*suffix*, */*)

*New in version 3.9.*

#### str.replace(*old*, *new*[, *count*])

替换字串，count为最多替换次数

#### str.split(*sep=None*, *maxsplit=-1*)

返回根据分隔符分割后的字符串列表，若传入maxsplit，则最多返回maxsplit+1个元素

如果不指定分隔符，则根据空格分割

```python
>>> '1,2,,3,'.split(',')
['1', '2', '', '3', '']
>>> '   1   2   3   '.split()
['1', '2', '3']
```

#### str.rsplit(*sep=None*, *maxsplit=-1*)

与split类似，返回顺序也是自左至右，只是maxsplit时略有不同

#### str.splitlines([*keepends*])

根据通用分隔符分行，可以保留换行符

和split稍有不同

```python
>>> "".splitlines()
[]
>>> ''.split('\n')
['']
```

#### str.startswith(*prefix*[, *start*[, *end*]])

是否以某字符串开头

#### str.strip([*chars*])

指定chars则清除左右两侧chars中字符，否则清除空格符

#### str.lstrip([*chars*])

左侧strip，chars不指定或者为`None`，则清楚空格；若指定chars，则清楚所有chars中的字符

与str.removeprefix()不同

```python
>>> '   spacious   '.lstrip()
'spacious   '
>>> 'www.example.com'.lstrip('cmowz.')
'example.com'
```

#### str.rstrip([*chars*])

与lstrip()类似

#### str.swapcase()

大小写反转

#### str.title()

返回标题版本

#### str.translate(*table*)

根据转换表转换字符串

#### str.upper()

转换成大写

#### str.zfill(*width*)

```python
>>> "42".zfill(5)
'00042'
>>> "-42".zfill(5)
'-0042'
```

### printf风格格式化

使用操作符`%`， `format % values` 。

## 字节序列类型 — bytes, bytearray, memoryview

字节操作的主要类型是bytes和bytearray，memoryview可以无需复制访问这些类型

### Bytes类型

不可变序列，由于主要的字节协议只支持ascii编码，有一些方法只在兼容ascii的情况下适用。

#### *class* bytes([*source*[, *encoding*[, *errors*]]])

定义字符bytes和str类似，再前面加`b`即可。

- 填充0的字节	`bytes(10)`
- 从整数的迭代器  `bytes(range(20))`
- 从支持buffer协议的二进制数据复制  `bytes(obj)`

#### *classmethod* fromhex(*string*)

从16进制字符串构造字节序列，每个字节必须两个16进制数，空格会被忽略

```python
>>> bytes.fromhex('2Ef0 F1f2  ')
b'.\xf0\xf1\xf2'
```

#### hex([*sep*[, *bytes_per_sep*]])

fromhex的逆运算

### Bytearray类型

可变字节序列，方法与Bytes类似

### Bytes和Bytearray操作

支持str类型的大多数操作

### MemoryView类型

## Set类型 — set, frozenset

不同哈希值对象的无序集合。不支持下标、切片或者一些其他的序列操作

构造方式都是一样的

### *class* set([*iterable*])

### *class* frozenset([*iterable*])

- 使用括号 `{'jack', 'sjoerd'}`
- 使用表达式 `{c for c in 'abracadabra' if c not in 'abc'}`
- 使用类型名 `set()`, `set('foobar')`, `set(['a', 'b', 'foo'])`

二者都支持一下操作

#### len(s)

#### x in s

#### x not in s

#### isdisjoint(*other*)

和另一个数据没有共同元素，则返回`True`

#### issubset(*other*)

#### set <= other

是否是子集，即所有元素都在另一个数据中

#### set < other

set <= other and set != other

#### issuperset(*other*)

#### set >= other

是否为父集，即另一个数据中所有元素都在该set中

#### set > other

不等于

#### union(**others*)

#### set | other | ...

返回并集

#### intersection(**others*)

#### set & other & ...

返回交集

#### difference(**others*)

#### set - other - ...

返回只在该set中有的元素

#### symmetric_difference(*other*)

#### set ^ other

返回不共有的元素集合

#### copy()

浅复制

用方法名进行运算时，参数可以是任意迭代器类型。而使用运算符运算时，只支持set

set和frozenset运算时，结果类型为首个参数的类型

以下为set特有而frozenset没有的方法

#### update(**others*)

#### set |= other | ...

更新set，添加元素进set

#### intersection_update(**others*)

#### set &= other & ...

更新set，只保留others中已有元素

#### difference_update(**others*)

#### set -= other | ...

只保留set中特有元素

#### symmetric_difference_update(*other*)

#### set ^= other

只保留不共用元素

#### add(*elem*)

添加元素

#### remove(*elem*)

移除元素，若不存在抛出`KeyError`

#### discard(*elem*)

移除元素，不存在不跑出异常

#### pop()

移除并返回任意元素，为空时抛出异常

#### clear()

移除所有元素

## 映射类型 — dict

将可哈希的值映射到任意对象。

### *class* `dict`(***kwarg*)[¶](https://docs.python.org/3/library/stdtypes.html#dict)

### *class* `dict`(*mapping*, ***kwarg*)

### *class* `dict`(*iterable*, ***kwarg*)

- 使用冒号分开`{'jack': 4098, 'sjoerd': 4127}`
- 使用表达式 `{}`, `{x: x ** 2 for x in range(10)}`
- 使用构造器 `dict()`, `dict([('foo', 100), ('bar', 200)])`, `dict(foo=100, bar=200)`

传入参数时，参数必须可迭代，且迭代值也是可迭代的，包含两个元素，一个为键，一个为值

#### list(d)

返回所有key组成的list

#### len(d)

键值对数目

#### d[key]

返回对应key的值，如key不存在抛出异常

可以通过定义`__missing__(key)`改变缺失key时处理方式

```python
>>> class Counter(dict):
...     def __missing__(self, key):
...         return 0
>>> c = Counter()
>>> c['red']
0
>>> c['red'] += 1
>>> c['red']
1
```

#### d[key] = value

设置值

#### del d[key]

删除key对应的值，不存在则抛出异常

#### key in d

key是否存在

#### key not in d

如上

#### iter(d)

等价于iter(d.keys())

#### clear()

清除所有元素

#### copy()

浅拷贝

#### *classmethod* fromkeys(*iterable*[, *value*])

以迭代器中值为key，value的值为value，未传入则为`None`

由于所有键的值一样，value应该为不可变数据

#### get(*key*[, *default*])

default默认为`None`，不会抛出`KeyError`

#### items()

返回(key, value)构成的view

#### keys()

返回key构成的view

#### pop(*key*[, *default*])

若key存在，移除并返回对应的值；否则返回default。key不存在且default未传入，抛出异常

#### popitem()

移除并返回`(key, value)`，后进先出顺序。

*Changed in version 3.7:* 保证后进先出顺序

#### reversed(d)

返回dict的keys的逆序

#### setdefalut(*key*[, *default*])

key存在，返回值；否则插入key，value为default并返回defalut。default默认为`None`

#### update([*other*])

更新内容。可以是有key,value组成的迭代器或者另一个dict。返回`None`

#### values()

返回values的view，dict.values()之间总是不相等

```python
>>> d = {'a': 1}
>>> d.values() == d.values()
False
```

#### d | other

合并并返回dict，必须都是dict

*New in version 3.9.*

#### d |= other

New in version 3.9.

dict所有元素值相等时相等，其他运算会抛出异常

dict保留插入时顺序，更新值不影响顺序，删除重新插入则放入末尾

*Changed in version 3.7:* 保证按照插入顺序

### view类型

keys()，values()，items()返回，dict改变时，view也会改变

#### len(views)

长度

#### iter(dictview)

创建(value, key)元组可以使用zip， `pairs = zip(d.values(), d.keys())`

在遍历时加入或删除元素会抛出异常

#### x in dictview

x是否为key

#### reversed(dictview)

逆序

## 上下文管理器类型

with语句支持上下文这一概念。语句被执行前进入，执行完成后退出

### contextmanager.\__enter__()

返回值绑定到as后的值

### contextmanager.\__exit__(*exc_type*, *exc_val*, *exc_tb*)

退出管理器并返回布尔值表示是否出现异常

## 其他内置类型

### 模块

模块唯一的特殊操作是`m.name`，import不是对模块的操作，无需模块存在

每个模块都有的特殊值是`__dict__`。包含模块的符号表。

### 类和实例

### 函数

函数的唯一操作是调用

### 方法

方法是用属性表示法来调用的函数。两种形式：内置函数和类实例方法

### 代码对象

### 类型对象

可通过type()获取

### 空对象

`None`

### 省略符对象

