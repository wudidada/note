# subprocess

## 使用

推荐使用run()来处理所有其能应付的任务，更为复杂的使用场景，可以使用Popen接口。

### subprocess.run(*args*, ***, *stdin=None*, *input=None*, *stdout=None*, *stderr=None*, *capture_output=False*, *shell=False*, *cwd=None*, *timeout=None*, *check=False*, *encoding=None*, *errors=None*, *text=None*, *env=None*, *universal_newlines=None*, ***other_popen_kwargs*)

根据参数调用命令，等到任务结束，返回 [`CompletedProcess`](https://docs.python.org/3/library/subprocess.html#subprocess.CompletedProcess)实例。

如果*capture_output*，会捕获stdout和stderr，如果想将二者合在一起，应该使用stdout=PIPE以及stderr=STDOUT，而非*capture_output*

如果*check*而进程结束状态非0，会抛出异常。

如果定义了*encoding*或者*errors*，或者*text*为真，stdin,stdout和stderr对应的文件对象会以指定的编码在文本模式下打开。

### 常用参数

#### args

所有调用都必须有args，args为字符串或者参数序列。参数序列会更好。如果是字符串，要么*shell*为True，要么字符串只简单的包含命令而不带参数。

#### stdin, stdout, stderr

标准输入、标准输出、标准错误文件句柄。可用的值为PIPE，DEVNULL，现有的文件描述符，现有的文件对象，None。

PIPE表明子进程新建一个管道。

*stderr*可以为STDOUT

#### encoding, errors, text

指定的话则以文本模式打开上面的文件

#### shell

如果*shell*为True，命令通过shell执行。