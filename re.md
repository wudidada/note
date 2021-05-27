# re: 正则表达式模块

## 常用语法

### (?aiLmsux)

| 字母 | 同义 | 作用                |
| ---- | ---- | ------------------- |
| a    | re.A | ASCII-only matching |
| i    | re.I | ignore case         |
| L    | re.L | locale dependent    |
| m    | re.M | multi-line          |
| s    | re.S | dot matches all     |
| u    | re.U | Unicode matching    |
| x    | re.X | verbose             |

### (?:...)

只匹配括号里的正则表达式但不提取

### (?P<name>...)

对匹配结果赋予名字

```
(?P<quote>['"]).*?(?P=quote)	# 匹配一队单引号或双引号
```

### (?P=name)

结合上面的语句，引用赋名的匹配结果

### (?=...)	*lookahead assertion*

前置断言 ，只用来匹配，不消耗字符串，会向前看是否匹配

```
Isaac (?=Asimov)		# 当Isaac 后有Asimov时匹配上Isaac 
```

### (?!...)	*negative lookahead assertion*

与前置断言相反

```
Isaac (?=Asimov)		# 当Isaac 没有Asimov时匹配上Isaac 
```

### (?<=...)	*positive lookbehind assertion*

只用来匹配，不消耗字符串，向后看是否匹配

**只能匹配固定长度，如`abc`或者`a|b`,不能用`a*``a{3,4}`等**

```
import re
m = re.search('(?<=abc)def', 'abcdef')		# 推荐使用search()而非match()
m.group(0)
'def'
```

### (?<!...)	*negative lookbehind assertion*

与*positive lookbehind assertion*相反，向后看是否不匹配

### (?(id/name)yes-pattern|no-pattern)

若id/name指定的匹配项存在，则匹配yes-pattern，否则匹配no-pattern

```
(<)?(\w+@\w+(?:\.\w+)+)(?(1)>|$)	# 匹配'<user@host.com>'及'user@host.com' 
									#不匹配'<user@host.com' nor 'user@host.com>'
```

### \A	文本开头

`^`匹配行首，`\A`匹配整个文本（字符串）首

### \Z	文本结尾

### \b	单词边界

## 常用方法

### re.compile(pattern, flags=0)

```python
prog = re.compile(pattern)
result = prog.match(string)

# 等价于
result = re.match(pattern, string)
```

flags见[表格](###(?aiLmsux))

### re.search(pattern, string, flags=0)

找到首次匹配的结果，匹配上返回match对象，否则`None`

### re.match(pattern, string, flags=0)

与[search](###re.search(pattern, string, flags=0))类似，从开始进行匹配。即使加上`MULTILINE`选项，也没有效果

### re.fullmatch(pattern, string, flags=0)

只有整个字符串匹配时返回match对象

### re.split(pattern, string, maxsplit=0, flags=0)

根据正则分割字符串，返回字符串列表。若正则使用括号，则括号中内容也会被分割出来，成为列表中一员。

`maxsplit`非零的话，最多只分割`maxsplit`项，剩下的字符串直接作为下一项。

当使用`*`或者`()`进行分割时，匹配列表中经常会出现空字符串

```python
re.split(r'\b', 'Words, words, words.')
['', 'Words', ', ', 'words', ', ', 'words', '.']

re.split(r'\W*', '...words...')
['', '', 'w', 'o', 'r', 'd', 's', '', '']

re.split(r'(\W*)', '...words...')
['', '...', '', '', 'w', '', 'o', '', 'r', '', 'd', '', 's', '...', '', '', '']
```

### re.findall(pattern, string, flags=0)

返回所有匹配上的字符串列表，若匹配的每一项有多个`group`，则返回字符串`tuple`列表

### re.finditer(pattern, string, flags=0)

与findall类似，返回的是`iterator`，类型为match对象，而非字符串

### re.sub(pattern, repl, string, count=0, flags=0)

repl可以是字符串也可以是方法

### re.subn(pattern, repl, string, count=0, flags=0)

与[sub](###re.sub(pattern, repl, string, count=0, flags=0))相同，返回结果为(新字符串，替换次数)

### re.escape(pattern)

用正则字符串生成转义后的正则

### re.purge()

清除缓存

## 正则表达式对象	Pattern

Pattern对象可以使用上述的种种方法

## 匹配结果对象	Match

### Match.group([group1, ...])

```shell
>>> m = re.match(r"(\w+) (\w+)", "Isaac Newton, physicist")
>>> m.group(0)       # The entire match
'Isaac Newton'
>>> m.group(1)       # The first parenthesized subgroup.
'Isaac'
>>> m.group(2)       # The second parenthesized subgroup.
'Newton'
>>> m.group(1, 2)    # Multiple arguments give us a tuple.
('Isaac', 'Newton')
```

group可以命令

```shell
>>> m = re.match(r"(?P<first_name>\w+) (?P<last_name>\w+)", "Malcolm Reynolds")
>>> m.group('first_name')
'Malcolm'
>>> m.group('last_name')
'Reynolds'
```

group访问快捷方式

```
m[0]等价于m.group(0)
```

### Match.groups(default=None)

```shell
>>> m = re.match(r"(\d+)\.(\d+)", "24.1632")
>>> m.groups()
('24', '1632')

>>> m = re.match(r"(\d+)\.?(\d+)?", "24")
>>> m.groups()      # Second group defaults to None.
('24', None)
>>> m.groups('0')   # Now, the second group defaults to '0'.
('24', '0')
```

### Match.groupdict(default=None)

```sh
>>> m = re.match(r"(?P<first_name>\w+) (?P<last_name>\w+)", "Malcolm Reynolds")
>>> m.groupdict()
{'first_name': 'Malcolm', 'last_name': 'Reynolds'}
```

