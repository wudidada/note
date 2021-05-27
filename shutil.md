# shutil 高阶文件操作

## 目录和文件操作

### shutil.copyfile(*src*, *dst*, *, *follow_symlinks=True*)

复制内容，不复制权限和信息

### shutil.copymode(*src*, *dst*, *, *follow_symlinks=True*)

复制权限、owner、group

### shutil.copystat(*src*, *dst*, *, *follow_symlinks=True*)

复制权限、owner、group，最后访问时间、最后修改时间以及flags(?)

### shutil.copy(*src*, *dst*, *, *follow_symlinks=True*)

```python
if os.path.isdir(dst):
    dst = os.path.join(dst, os.path.basename(src))
copyfile(src, dst, follow_symlinks=follow_symlinks)
copymode(src, dst, follow_symlinks=follow_symlinks)
```

### shutil.copy2(*src*, *dst*, *, *follow_symlinks=True*)

```python
if os.path.isdir(dst):
    dst = os.path.join(dst, os.path.basename(src))
copyfile(src, dst, follow_symlinks=follow_symlinks)
copystat(src, dst, follow_symlinks=follow_symlinks)
```

### shutil.ignore_patterns(**patterns*)

可以生成传入[copytree](###shutil.copytree(*src*, *dst*, *symlinks=False*, *ignore=None*, *copy_function=copy2*, *ignore_dangling_symlinks=False*, *dirs_exist_ok=False*))的ignore函数

```python
copytree(source, destination, ignore=ignore_patterns('*.pyc', 'tmp*'))
```

### shutil.copytree(*src*, *dst*, *symlinks=False*, *ignore=None*, *copy_function=copy2*, *ignore_dangling_symlinks=False*, *dirs_exist_ok=False*)

递归复制目录

### shutil.rmtree(*path*, *ignore_errors=False*, *onerror=None*)

递归删除目录

onerror接受三个参数

- function，抛出errror的函数
- path，传入到function的path
- execinfo，sys.exc_info()中的错误信息

### shutil.move(*src*, *dst*, *copy_function=copy2*)

移动目录或者文件

### shutil.chown(*path*, *user=None*, *group=None*)

修改owner及group

### shutil.which(*cmd*, *mode=os.F_OK | os.X_OK*, *path=None*)

```python
>>> shutil.which("python")
'C:\\Python33\\python.EXE'
```

## 实例

### 删除只读文件

```python
import os, stat
import shutil

def remove_readonly(func, path, _):
    "Clear the readonly bit and reattempt the removal"
    os.chmod(path, stat.S_IWRITE)
    func(path)

shutil.rmtree(directory, onerror=remove_readonly)
```

### 复制目录时忽略某些文件

```python
from shutil import copytree, ignore_patterns

copytree(source, destination, ignore=ignore_patterns('*.pyc', 'tmp*'))
```

### 复制目录时记录复制的目录路径

```python
from shutil import copytree
import logging

def _logpath(path, names):
    logging.info('Working in %s', path)
    return []   # nothing will be ignored

copytree(source, destination, ignore=_logpath)
```

