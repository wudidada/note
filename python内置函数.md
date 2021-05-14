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