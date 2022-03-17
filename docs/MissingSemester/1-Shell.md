## Shell

### 平常使用遇到的小问题

#### 执行 `sudo sth.sh` 时，command not found

```bash
$ ./sth.sh
-bash: ./sth.sh: Permission denied
$ sudo ./sth.sh
sudo: command not found
```

解决方案：

首先执行 `chmod +x ./sth.sh`，再使用 `sudo` 运行

```bash
$ chmod +x ./sth.sh
$ sudo ./sth.sh
```

### 单文件

#### 引入

`date` 打印当前日期时间

```bash
date
```

`echo` 打印后面的参数

```bash
$ echo hello
hello
$ echo $SHELL
/bin/bash
$ echo Hello\ World
Hello World
$ echo Hello World
Hello World
$ echo "Hello World"
Hello World
$ echo 'Hello World'
Hello World
```

使用空格来分割参数，所以如果不使用转义符就敲空格，得到的参数并不是一个整体，而是两个参数。

#### 环境变量

存有用户信息、home目录、path等

```bash
echo $PATH
```

运行文件时，会遍历 path 中的命令来运行。

如果想知道运行的命令的目录在哪儿

```bash
$ which echo
/usr/bin/echo
```

打印当前工作目录（Print Working Directory）

```bash
pwd
```

目录中的文件（List）

```bash
ls
ls ..
```

退回上一目录

```bash
cd -
```

#### `ls`

使用 `ls --help` 可以查看许多命令

查看文件信息详情

```bash
$ ls -l
total 98080
-rw-r--r-- 1 ohg ohg 66709754 Jul 22 00:13 Miniconda3-latest-Linux-x86_64.sh
-rw-r--r-- 1 ohg ohg 33719965 Oct  8 22:51 R-4.1.1.tar.gz
-rw-r--r-- 1 ohg ohg        0 Oct 20 14:54 fish
drwxr-xr-x 1 ohg ohg     4096 Dec 31 12:46 githubPy
drwxr-xr-x 1 ohg ohg     4096 Sep 24 16:31 miniconda3
```

第一个字母若是 `d`，代表 directory；后三组字母代表权限 permission: read, write, execute

对文件而言，读指读取文件的内容，写指更改文件内容，执行指执行文件

对目录而言，读指读取目录内的文件名，写指可以对目录内的文件进行删除或重命名，执行指是否有权限进入这个目录，若想要访问一个文件，则需要对其目录树上的所有目录都有执行权限，这样才可以访问到要访问的文件。

#### `mv` `cp` `rm` `rmdir` `mkdir`

`mv` 重命名/剪切

`cp` 复制

`rm` 删除文件

`rmdir` 删除空目录

`mkdir` 建立新目录，如果 `mkdir My Document` 则会建立 2 个新目录，所以我们应该对空格使用转义符或外面加引号。

一些尝试：

```bash
$ mkdir TouhcFish
$ mv ./TouhcFish ./TouchFish
$ ls
Miniconda3-latest-Linux-x86_64.sh  R-4.1.1.tar.gz  TouchFish  fish  githubPy  miniconda3
$ mv ./fish ./TouchFish/
$ ls
Miniconda3-latest-Linux-x86_64.sh  R-4.1.1.tar.gz  TouchFish  githubPy  miniconda3
$ ls TouchFish
fish
$ touch fish2
$ mv ./fish2 ./TouchFish
$ ls TouchFish
fish  fish2
$ mv ./TouchFish ./TouchFish/TouchFishsh
mv: cannot move './TouchFish' to a subdirectory of itself, './TouchFish/TouchFishsh'
$ mv ./TouchFish ./Touch4Fish/TouchFishsh
mv: cannot move './TouchFish' to './Touch4Fish/TouchFishsh': No such file or directory
$ mv ./TouchFish ./Touch
$ ls
Miniconda3-latest-Linux-x86_64.sh  R-4.1.1.tar.gz  Touch  githubPy  miniconda3
$ ls Touch
fish  fish2
$ rmdir Touch
rmdir: failed to remove 'Touch': Directory not empty
$ rm -r Touch
$ ls
Miniconda3-latest-Linux-x86_64.sh  R-4.1.1.tar.gz  githubPy  miniconda3
```

#### `man`

`man command` 可以获得更加详实的帮助。

#### 清屏

Ctrl L

`clear`

本质是终端向后翻了一页，往前还是能找到刚刚的内容。

### 流

默认的输入输出流，是屏幕。

```bash
$ echo hello > hello.txt
```

将输出保存到 `hello.txt` 中。

要想查看文本文档的内容，使用 `cat` 命令。

```bash
$ cat hello.txt
hello
```

也可以把 `hello.txt` 中的内容丢给 `cat`，作为它的输入流。

```bash
$ cat < hello.txt
hello
```

使用 `cat` 还可以实现 `cp` 的作用。

```bash
$ cat < hello.txt > hello2.txt
```

如果使用 `>>`，则是起到 append 的作用，把新的输出放在文档末尾。

```bash
$ cat < hello.txt >> hello2.txt
$ cat hello2.txt
hello
hello
```

Pipe：`|` 可以将左边的输出作为右边的输入。

```bash
$ ls -l ./ > hey.txt
$ cat hey.txt
total 98080
-rw-r--r-- 1 ohg ohg 66709754 Jul 22 00:13 Miniconda3-latest-Linux-x86_64.sh
-rw-r--r-- 1 ohg ohg 33719965 Oct  8 22:51 R-4.1.1.tar.gz
drwxr-xr-x 1 ohg ohg     4096 Dec 31 12:46 githubPy
-rw-r--r-- 1 ohg ohg        0 Jan 17 16:01 hey.txt
drwxr-xr-x 1 ohg ohg     4096 Sep 24 16:31 miniconda3
$ tail -n2 hey.txt
-rw-r--r-- 1 ohg ohg        0 Jan 17 16:01 hey.txt
drwxr-xr-x 1 ohg ohg     4096 Sep 24 16:31 miniconda3
$ tail -n2 < hey.txt
-rw-r--r-- 1 ohg ohg        0 Jan 17 16:01 hey.txt
drwxr-xr-x 1 ohg ohg     4096 Sep 24 16:31 miniconda3
$ ls -l ./ | tail -n2
-rw-r--r-- 1 ohg ohg      304 Jan 17 16:01 hey.txt
drwxr-xr-x 1 ohg ohg     4096 Sep 24 16:31 miniconda3
$ ls -l ./ | tail -n1 > ls.txt
```

 ### root

对于一些被保护的文件，如果想要使用 `echo` 再加流的重定向去更改他们，只用 `sudo` 是拿不到最高权限的。

```bash
$ sudo echo 7e:59:c2:91:84:7f > address
```

这样的 `sudo` 只修饰的 `echo`，却对流的另一边没有任何作用。

这里，我们需要使用 `tee` 命令结合管道，而不只是流的重定向。

```bash
$ touch fish
$ tee fish
600
600
exit
exit
^C
$ cat fish
600
exit
$ echo 500 | tee fish
500
$ cat fish
500
```

第二行命令中重复的部分里，前一个是通过键盘输入的，后一个是 `tee` 命令输出的。

所以最开始的命令使用 `tee` 命令之后，就不会有 Permission Denied 了。

```bash
$ echo 7e:59:c2:91:84:7f | sudo tee address
```

