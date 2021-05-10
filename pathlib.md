# pathlib - 面向对象的文件系统路径

## 基础使用

### 导入主类

```python
from pathlib imoprt Path
```

### 列出子目录

```python
p = Path('.')			# Path会根据系统实例化为WindowsPath或PosixPath
[x for x in p.iterdir() if x.is_dir()]
```

### 列出当前目录数下的所有Python源文件

```
list(p.glog('**/*.py'))
```

### 目录树中移动

```python
>>> p = Path('/etc')
>>> q = p / 'init.d' / 'reboot'
>>> q
PosixPath('/etc/init.d/reboot')
>>> q.resolve()
PosixPath('/etc/rc.d/init.d/halt')
```

### 查询路径属性

```python
>>> q.exists()
True
>>> q.is_dir()
False
```

### 打开一个文件

```python
with q.open() as f:
	f.readline()
```

## 纯路径

### PurePath(*pathsegments)

实例化为`PurePosixPath`或者`PureWindowsPath`

#### 初始化

```python
>>> PurePath('foo', 'some/path', 'bar')
PurePosixPath('foo/some/path/bar')
>>> PurePath(Path('foo'), Path('bar'))
PurePosixPath('foo/bar')
```

给出一些绝对路径，最后一个将被当作锚

```python
>>> PurePath('/etc', '/usr', 'lib64')
PurePosixPath('/usr/lib64')
>>> PureWindowsPath('c:/Windows', 'd:bar')
PureWindowsPath('d:bar')
```

### 通用性质

#### 可比较

```python
>>> PurePosixPath('foo') == PurePosixPath('FOO')
False
>>> PureWindowsPath('foo') == PureWindowsPath('FOO')
True
>>> PureWindowsPath('C:') < PureWindowsPath('d:')
True
```

### 运算符

斜杠 `/` 操作符有助于创建子路径，就像 [`os.path.join()`](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.join) 一样

```python
>>> p = PurePath('/etc')
>>> p
PurePosixPath('/etc')
>>> p / 'init.d' / 'apache2'
PurePosixPath('/etc/init.d/apache2')
>>> q = PurePath('bin')
>>> '/usr' / q
PurePosixPath('/usr/bin')
```

文件对象可用于任何接受 [`os.PathLike`](https://docs.python.org/zh-cn/3/library/os.html#os.PathLike) 接口实现的地方

```python
>>> import os
>>> p = PurePath('/etc')
>>> os.fspath(p)
'/etc'
```

### 访问个别部分

#### PurePath.parts

```python
>>> p = PurePath('/usr/bin/python3')
>>> p.parts
('/', 'usr', 'bin', 'python3')
```

### 方法和特征属性

#### PurePath.drive

#### PurePath.root

#### PurePath.anchor

dirve和root的联合

#### PurePath.parents

```python
>>> p = PureWindowsPath('c:/foo/bar/setup.py')
>>> p.parents[0]
PureWindowsPath('c:/foo/bar')
>>> p.parents[1]
PureWindowsPath('c:/foo')
>>> p.parents[2]
PureWindowsPath('c:/')
```

#### PurePath.parent

父路径，不能超过anchor或空路径

#### PurePath.name	完整文件名

文件名，若为目录则是空字符串

#### PurePath.suffix	文件后缀(最后一个)

文件扩展名

```python
>>> PurePosixPath('my/library/setup.py').suffix
'.py'
>>> PurePosixPath('my/library.tar.gz').suffix
'.gz'
>>> PurePosixPath('my/library').suffix
''
```

#### PurePath.suffixes	所有后缀

扩展名列表

```python
>>> PurePosixPath('my/library.tar.gar').suffixes
['.tar', '.gar']
>>> PurePosixPath('my/library.tar.gz').suffixes
['.tar', '.gz']
>>> PurePosixPath('my/library').suffixes
[]
```

#### PurePath.stem	文件名(不含后缀)

```python
>>> PurePosixPath('my/library.tar.gz').stem
'library.tar'
>>> PurePosixPath('my/library.tar').stem
'library'
>>> PurePosixPath('my/library').stem
'library'
```

#### PurePath.as_posix()

```python
>>> p = PureWindowsPath('c:\\windows')
>>> str(p)
'c:\\windows'
>>> p.as_posix()
'c:/windows'
```

#### PurePath.is_absolute()

是否为绝对路径

#### PurePath.joinpath(*other)	拼接路径

#### PurePath.match(pattern)	是否与通配符匹配

pattern是相当的，则路径可以是绝对路径或相对路径，且匹配从右侧完成

```python
>>> PurePath('a/b.py').match('*.py')
True
>>> PurePath('/a/b/c.py').match('b/*.py')
True
>>> PurePath('/a/b/c.py').match('a/*.py')
False
```

#### PurePath.relative_to(*other)	计算相对路径

若不可计算，抛出`ValueError`

#### PurePath.with_name(name)	替换完整文件名

原路径没有name，抛出`ValueError`

```python
>>> p = PureWindowsPath('c:/Downloads/pathlib.tar.gz')
>>> p.with_name('setup.py')
PureWindowsPath('c:/Downloads/setup.py')
```

#### PurePath.with_stem(stem)	替换不带后缀文件名

原路径没有name，抛出`ValueError`

```python
>>> p = PureWindowsPath('c:/Downloads/draft.txt')
>>> p.with_stem('final')
PureWindowsPath('c:/Downloads/final.txt')
>>> p = PureWindowsPath('c:/Downloads/pathlib.tar.gz')
>>> p.with_stem('lib')
PureWindowsPath('c:/Downloads/lib.gz')
```

*3.9 新版功能*

#### PurePath.with_suffix(suffix)	替换后缀

若suffix为空字符串，则原有后缀被移除

```python
>>> p = PureWindowsPath('c:/Downloads/pathlib.tar.gz')
>>> p.with_suffix('.bz2')
PureWindowsPath('c:/Downloads/pathlib.tar.bz2')
>>> p = PureWindowsPath('README')
>>> p.with_suffix('.txt')
PureWindowsPath('README.txt')
>>> p = PureWindowsPath('README.txt')
>>> p.with_suffix('')
PureWindowsPath('README')
```

## 具体路径

### 类

#### Path

PurePath的子类，根据系统实例化为PosixPath或WindowsPath

#### PosixPath

#### WindowsPath

###  方法

#### Path.cwd()	当前目录的路径对象

```python
>>> Path.cwd()
PosixPath('/home/antoine/pathlib')
```

#### Path.home()

#### Path.stat()	路径信息

返回os.stat_result对象

```python
>>> p = Path('setup.py')
>>> p.stat().st_size
956
>>> p.stat().st_mtime
1327883547.852554
```

#### Path.chmod(mode)	改变文件模式及权限

```python
>>> p = Path('setup.py')
>>> p.stat().st_mode
33277
>>> p.chmod(0o444)
>>> p.stat().st_mode
33060
```

#### Path.exists()	路径是否存在

#### Path.expanduser()	展开~和~user构造

#### Path.glob(pattern)	根据通配符匹配文件

```python
>>> sorted(Path('.').glob('*.py'))
[PosixPath('pathlib.py'), PosixPath('setup.py'), PosixPath('test_pathlib.py')]
>>> sorted(Path('.').glob('*/*.py'))
[PosixPath('docs/conf.py')]
```

`**`模式表示此递归匹配此目录及所有子目录

```python
>>> sorted(Path('.').glob('**/*.py'))
[PosixPath('build/lib/pathlib.py'),
 PosixPath('docs/conf.py'),
 PosixPath('pathlib.py'),
 PosixPath('setup.py'),
 PosixPath('test_pathlib.py')]
```

#### Path.rglob(pattern)	根据通配符匹配文件

相当于在glob的pattern上加上了`**/`

#### Path.group()

#### Path.is_dir()	是否为目录

#### Path.is_file()	是否为文件

#### Path.is_mount()

是否是挂载点

*3.7 新版功能*

#### Path.is_symlink()	是否为符号链接

#### Path.itemdir()	产生目录下的对象的路径

子条目以任意顺序生成，若有文件在迭代器创建后移除或添加，是否包含该文件并没有规定。

#### Path.mkdir(mode=0o777, parent=False, exist_ok=False)	创建目录

parent为`Ture`，则找不到的父目录会被创建，以默认权限创建，不考虑mode参数

exist_ok为`Ture`，则目标存在时不抛出错误，否则抛出`FileExistError`

*3.5版更改：加入exist_ok*

#### Path.open(*mode='r'*, *buffering=-1*, *encoding=None*, *errors=None*, *newline=None*)	打开文件

```python
>>> p = Path('setup.py')
>>> with p.open() as f:
...     f.readline()
...
'#!/usr/bin/env python3\n'
```

#### Path.resolve(*strict=False*)	路径绝对化

`..`路径也会被消除(只有这一种方法能做到)

```python
>>> p = Path('docs/../setup.py')
>>> p.resolve()
PosixPath('/home/antoine/pathlib/setup.py')
```

strict为`Ture`，则路径不存在时抛出错误

*3.6新版功能：加入strict参数(3.6之前相当于为True)*

## os模块对应

| os 和 os.path                                                | pathlib                                                      |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`os.path.abspath()`](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.abspath) | [`Path.resolve()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.resolve) |
| [`os.chmod()`](https://docs.python.org/zh-cn/3/library/os.html#os.chmod) | [`Path.chmod()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.chmod) |
| [`os.mkdir()`](https://docs.python.org/zh-cn/3/library/os.html#os.mkdir) | [`Path.mkdir()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.mkdir) |
| [`os.makedirs()`](https://docs.python.org/zh-cn/3/library/os.html#os.makedirs) | [`Path.mkdir()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.mkdir) |
| [`os.rename()`](https://docs.python.org/zh-cn/3/library/os.html#os.rename) | [`Path.rename()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.rename) |
| [`os.replace()`](https://docs.python.org/zh-cn/3/library/os.html#os.replace) | [`Path.replace()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.replace) |
| [`os.rmdir()`](https://docs.python.org/zh-cn/3/library/os.html#os.rmdir) | [`Path.rmdir()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.rmdir) |
| [`os.remove()`](https://docs.python.org/zh-cn/3/library/os.html#os.remove), [`os.unlink()`](https://docs.python.org/zh-cn/3/library/os.html#os.unlink) | [`Path.unlink()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.unlink) |
| [`os.getcwd()`](https://docs.python.org/zh-cn/3/library/os.html#os.getcwd) | [`Path.cwd()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.cwd) |
| [`os.path.exists()`](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.exists) | [`Path.exists()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.exists) |
| [`os.path.expanduser()`](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.expanduser) | [`Path.expanduser()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.expanduser) 和 [`Path.home()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.home) |
| [`os.listdir()`](https://docs.python.org/zh-cn/3/library/os.html#os.listdir) | [`Path.iterdir()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.iterdir) |
| [`os.path.isdir()`](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.isdir) | [`Path.is_dir()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.is_dir) |
| [`os.path.isfile()`](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.isfile) | [`Path.is_file()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.is_file) |
| [`os.path.islink()`](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.islink) | [`Path.is_symlink()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.is_symlink) |
| [`os.link()`](https://docs.python.org/zh-cn/3/library/os.html#os.link) | [`Path.link_to()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.link_to) |
| [`os.symlink()`](https://docs.python.org/zh-cn/3/library/os.html#os.symlink) | [`Path.symlink_to()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.symlink_to) |
| [`os.readlink()`](https://docs.python.org/zh-cn/3/library/os.html#os.readlink) | [`Path.readlink()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.readlink) |
| [`os.stat()`](https://docs.python.org/zh-cn/3/library/os.html#os.stat) | [`Path.stat()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.stat), [`Path.owner()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.owner), [`Path.group()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.group) |
| [`os.path.isabs()`](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.isabs) | [`PurePath.is_absolute()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.PurePath.is_absolute) |
| [`os.path.join()`](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.join) | [`PurePath.joinpath()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.PurePath.joinpath) |
| [`os.path.basename()`](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.basename) | [`PurePath.name`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.PurePath.name) |
| [`os.path.dirname()`](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.dirname) | [`PurePath.parent`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.PurePath.parent) |
| [`os.path.samefile()`](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.samefile) | [`Path.samefile()`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.Path.samefile) |
| [`os.path.splitext()`](https://docs.python.org/zh-cn/3/library/os.path.html#os.path.splitext) | [`PurePath.suffix`](https://docs.python.org/zh-cn/3/library/pathlib.html#pathlib.PurePath.suffix) |

