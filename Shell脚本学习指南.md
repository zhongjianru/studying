#### 前言
#### 第一章 背景知识
#### 第二章 入门
#### 第三章 查找与替换
#### 第四章 文本处理工具
#### 第五章 管道的神奇魔力
#### 第六章 变量、判断、重复动作
#### 第七章 输入/输出、文件与命令执行
#### 第八章 产生脚本
#### 第九章 awk的惊人表现
#### 第十章 文件处理
#### 第十一章 拓展实例：合并用户数据库
#### 第十二章 拼写检查
#### 第十三章 进程
#### 第十四章 Shell可以执行议题与拓展
#### 第十五章 安全的Shell脚本：起点
#### 附录A 编写手册页
#### 附录B 文件与文件系统
#### 附录C 重要的UNIX命令


#### 前言

读者应该了解以下背景知识：
* 如何登陆 UNIX 系统
* 如何在命令行上执行程序
* 如何做一个简单的命令管道，与使用简单的输出 / 入重定向，例如 < 和 >
* 如何以 & 将程序放到后台执行
* 如何建立与编辑文件
* 如何使用 chmod 将脚本设为可执行权限


#### 第一章 背景知识

##### UNIX 简史

UNIX 最初是由贝尔电话实验室的计算机科学研究中心开发的，第一版诞生于 1970 年。

由于 UNIX 是在面向研究的环境下开发的，因而没有必须生产或者销售成品的盈利压力，这使其具有下列优势：

* 系统由用户自行开发
* 研究人员可以不受拘束地进行实验，必要时也可以任意变换程序
* 务实的设计模式：程序会执行你所赋予的任务，但不会跟你对话，也不会问一堆“你确定吗？”之类的问题
* 不断追求优雅：简单就是力量

当然，自由同样也带来了一些缺点，当 UNIX 流传至开发环境以外的地方，这些问题也逐一浮现：

* 工具程序之间存在许多不一致的地方：例如同样的选项字母，在不同的程序之间有着完全不一样的定义，或者是相同的工作却需要制定不同的选项字母
* 诸多工具程序具有缺陷：例如输入行的长度，或是可打开的文件个数，等等（现行的系统多半已经修正这些缺陷）
* 有时程序并未经过彻底测试，这使得它们在执行时一不小心就会遭到破坏
* 系统的文档尽管大致上内容完备，但通常极其简单，用户在学习时很难找到所需要的信息

本书之所以将重点放在文本（而非二进制）数据的处理与运用上，是由于 UNIX 早期的发展都源自于对文本处理的强烈需求，不过除此之外还有另外的重要理由。

最后，对常用标准工具组与选项的需求终于明朗化，POSIX 标准即为最后的结果。现在的商用 UNIX 系统，以及可以免费使用的同类型产品，都兼容POSIX。这样一来，学习 UNIX 变得更容易，编写可移植的 Shell 脚本也成为可能。除 POSIX 之外，还有其他标准，例如 XPG4/XPG5，其更广为人知的名称为 UNIX98 标准。XPG5 很大程度上把 POSIX 纳入为一个子集，同样深具影响力。

##### 软件工具的原则

* 一次做好一件事
  产生出更小、更专用于特定功能的程序

* 处理文本行，不要处理二进制数据
  文本行是 UNIX 的通用格式，使用文本文件更有助于任何自定义工具和现存的 UNIX 程序之间的结合

* 使用正则表达式
  正则表达式是很强的文本处理机制，可适度简化编写命令的脚本的工作

* 默认使用标准输入 / 输出
  程序默认会从标准输入读取数据，将数据写到标准输出，至于错误信息则会传送到标准错误输出，以这样的范式编写程序，恶意轻松地让它们成为数据过滤器，例如，组成部分的规模越大，越需要复杂的管道（pipeline）或脚本来处理

* 避免喋喋不休
  软件工具的执行过程不该像在聊天，不要将“开始处理”、“即将完成”或者“处理完成”这类信息放进程序的标准输出。如果每个工具都会产生这些消息并送往管道，那么整个屏幕画面就会布满一堆无用的过程信息。在工具程序的世界里，没有消息就是好消息。
  当用户键入 rm somefile 时，UNIX 的设计人员会认为用户知道自己在做什么，然后毫无疑问的删除掉这个文件。如果你真觉得这样不好，rm 的 -i 选项可以强制给你提示以确认。一直以来，是否需要提示是个争议的话题，值得用户深思。

* 输出格式必须与可接受的输入格式一致
  容易将一个程序的执行结果交给另一个程序处理

* 让工具去做困难的部分
  虽然 UNIX 程序并非完全符合你的需求，但是现有的工具或许已经可以为你完成 90% 的工作，如果有需要，你可以编写一个功能特定的小型程序来完成接下来的工作。与每次都从头开始来解决问题相比，这样可以省去许多工作。

* 构建特定工具前，先想想
  在动手编写一个能够解决问题的程序前，请先停下来想几分钟，你所要做的事，是否有其他人也需要做？这个特殊的工作是否有可能是某个一般问题的一个特例？如果是的话，请针对一般问题来编写程序。


#### 第二章 入门

当需要计算机帮助你做些什么时，最好用对工具。你不会用文字编辑器来做支票簿的核对，也不会用计算器来写策划方案。同理，当你需要程序语言协助完成工作时，不用的程序用于不同的需求。

Shell 脚本最常用于系统管理工作，或是用于结合现有的程序以完成小型的、特定的工作。一旦你找出完成工作的方法，可以把用到的命令串在一起，放进一个独立的程序或脚本里，此后只要直接执行该程序便能完成工作。此外，如果你写的程序很有用，其他人可以利用该程序作为一个黑盒来使用，它是一个可以完成工作的程序，但我们不必知道它是如何完成的。

##### 脚本编程语言与编译型语言的差异

* 编译型语言
  Fortran、Ada、Pascal、C、C++ 或 Java
  从源代码转换成目标代码（编译），便能直接通过计算机来执行
  好处：高效
  缺点：多半运作于底层，所处理的是字节、整数、浮点数或是其他机器层级的对象

* 脚本编程语言
  awk、Perl、Python、Ruby 和 Shell
  解释型代码，由解释器读入程序代码，并将其转换成内部的形式，再执行。
  解释器本身是一般的编译型程序。
  好处：多半运行在比编译型语言还高的层级，能够轻易处理文本与目录之间的对象
  缺点：效率通常不如编译型语言

因为 Shell 是各 UNIX 系统之间通用的功能，并且经过 POSIX 的标准化，所以 Shell 脚本只要用心写一次，即可应用到很多系统上。

之所以要使用 Shell 脚本是基于：

* 简单性
  简洁地表达复杂的操作
* 可移植性
  使用 POSIX 定义的功能，可以做到脚本无需修改就可以在不同的程序上执行
* 开发容易
  可以在短时间内完成一个功能强大又好用的脚本

##### 一个简单的脚本

* 获取系统登录用户
  ```
  $ who
  ```

* 计算登录用户个数
  ```
  $ who | wc -l
  # 将 who 的输出通过管道 | 成为 wc 的输入（wc-字数计算）
  ```

* 将管道转变成独立命令
  
  ```
  $ cat $ nusers            # 建立文件，使用 cat 复制终端的输入
  #! bin/sh                 # 使用该目录下的 shell 来执行程序
  who | wc -l               # 程序内容
  ^D                        # end-of-file
  $ chmod +x nusers         # 让文件拥有执行权限
  $ ./nusers                # 执行测试
  6                         # 输出结果
  ```

* 特别注意

  1. #! 这行尽量不要超过64个字符
  2. 在某些系统上，命令行部分包含了命令的完整路径名称。不过有些系统却不是这样，命令行的部分会原封不动地传给程序。因此，脚本是否具有可移植性取决于是否有完整的路径名称。
  3. 不要在选项（option）之后放置空格，因为空格也会跟着选项一起传递给被引用的程序
  4. 需要知道解释器的完整路径，这样可以规避可移植性问题，因为不同的厂商可能将同样的东西放在不同的路径
  5. 一些旧系统上，内核不具备解释 #! 能力，有些 Shell 会自行处理，它们对符号与解释器名称之间是否可以有空格，可能有不同的解释。

##### Shell 的基本元素

* 命令与参数
  ```
  $ cd work; ls -lt whizprog.c  # 短选项：命令 -参数（多个参数可合并）
  $ cd whizprog-1.1
  $ patch --verbose --backup -p1 < /tmp/whizprog-1.1-1.2-patch  # 长选项：已被纳入 POSIX 标准
  # ; 可用于分割同一行里的多条命令
  # & 在后台执行前面的命令（不用等到该命令完成即可执行下一个命令）
  ```

* Shell 基本命令
  1. 内建命令：例如 cd、read 等
  2. Shell 函数
  3. 外部命令

* 变量
  1. 以字母或下划线开头，后面可以接任意长度的字母、数字或下划线
  2. 变量名称和保存的字符串值长度没有限制
  3. 变量赋值方式：变量名称=新值（中间没有空格）
  4. 变量取出方式：变量a=$变量b
  ```
  $ first=isaac; middle=bashevis; last=singer  # 单行可进行多次赋值
  $ fullname="isaac bashevis singer"  # 值中包含空格时需使用引号
  $ oldname=$fullname  # 此处不需要引号
  ```

* 简单的 echo 输出
  1. echo 将各个参数打印到标准输出，参数之间以一个空格隔开，并以换行符结束
  2. UNIX 各版本间互不相同的行为模式使得 echo 的可移植性变得很困难
  3. 比较复杂的输出，推荐使用 printf
  ```
  $ echo "Now is the time for all good man"
  ```

* 华丽的 printf 输出
  1. printf 不像 echo 自动提供一个换行符号，需要显示指定换行符 \n
  2. 格式声明：以 % 开头，用来控制相应参数的输出
  ```
  $ printf "The first program always prints '%s, %s!'\n" Hello world
  ```

* 基本的 I/O 重定向
  
  标准输入 / 输出可能是软件设计原则里最重要的概念了：程序应该有标准输入（数据的来源端）、标准输出（数据的目的端）和标准错误输出（报告问题的地方）。程序不必关心输入输出背后是什么设备，当程序启动时，可以预期的是标准输出入都已打开，并且已经准备好供其使用。

  许多 UNIX 程序都遵循这一设计原则。默认的情况下，它们会读取`标准输入`、写入`标准输出`，并将错误信息传递到`标准错误输出`，这类程序常叫做`过滤器`。默认的标准输入、标准输出以及标准错误输出都是终端，在你登陆时，UNIX 就已经安排好了。

* 重定向与管道
  
  通过与终端交互，或者在 Shell 脚本里面设置，重新安排从哪里输入或者输出到哪里。

  ```
  # program < file 改变标准输入
  $ tr -d '\r' < dos-file.txt

  # program $ file 改变标准输出（目标文件不存在时会新建一个，目标文件已存在时会被覆盖）
  $ tr -d '\r' < dos-file.txt $ UNIX-file.txt

  # 可以把 < 和 $ 想象成数据的漏斗：数据会从大的一端进入，从小的一端出来

  # program >$ file 附加到文件
  $ for f in dos-file*.txt
  $ do
  $     tr -d '\r' < $f >$ big-UNIX-file.txt
  $ done

  # program1 | program2 建立管道（前一个的标准输出作为后一个的标准输入）
  $ tr -d '\r' < dos-file.txt | sort $ UNIX-file.txt

  # tr [options] source-char-list replace-char-list
  # options: -c -C -d -s
  ```

* 特殊文件
 
  ```
  # 1、/dev/null（位桶）：可以作为命令的退出状态
  # 当程序将数据写到此文件时，会认为它已经成功完成写入数据的操作，但实际上什么事都没做
  $ if grep pattern myfile $ /dev/null
  $ then ..  # 找到模式时
  $ else ..  # 找不到模式时
  $ fi

  # 2、/dev/tty（自动重定向到终端，再与程序结合）
  # 在程序必须读取人工输入（例如密码）时特别有用，此外用它来产生错误信息也很方便
  $ printf "Enter new password:"  # 提示输入
  $ stty -echo                    # 关闭自动打印输入字符的功能
  $ read pass < /dev/tty          # 读取密码（一般输入两次密码以确认修改）
  $ stty echo                     # 别忘了打开自动打印输入字符的功能
  ```

##### 基本命令查找

  ```
  # Shell 会沿着查找路径 $PATH 来寻找命令
  # $PATH 是一个以冒号分隔的目录列表，可以在列表指定的目录下找到所要执行的命令
  $ echo $PATH
  $ /bin:/usr/bin:usr/X11R6/bin:/usr/local/bin

  # bin 目录用来保存可执行文件
  # 要让修改永久生效，在 .profile 文件中把你的 bin 目录加入 $PATH，每次登陆 Shell 都会读取该文件
  $ cd                      # 切换到 home 目录
  $ mkdir bin               # 建立个人 bin 目录
  $ mv nusers bin           # 将脚本放入该目录
  $ PATH=$PATH:$HOME/bin    # 将个人 bin 目录附加到 PATH
  $ nusers                  # Shell 有找到并执行它
  ```

##### 访问 Shell 脚本的参数

  ```
  # 查找 betsy 用户的命令
  $ who | grep betsy

  # 查找特定用户的脚本，脚本的第一个参数就是我们要找的用户名称
  $ cat $ finduser          # 建立新文件
  #! /bin/sh
  who | grep $1             # $1 表示第一个参数
  ^D                        # 以 End-of-file 结尾
  $ chmod +x finduser       # 设置执行权限
  $ ./finduser betsy        # 测试：寻找 betsy
  $ mv finduser $HOME/bin   # 将这个文件存进自己的 bin 目录
  ```

##### 简单的执行跟踪

  ```
  $ sh -x nusers            # 打开执行跟踪功能
  + who                     # Shell 会显示每个被执行到的命令
  + wc -l

  $ set -x                  # 打开执行跟踪功能
  $ set +x                  # 关闭执行跟踪功能
  ```

##### 国际化与本地化

编写软件给全世界的人使用，是一项艰难的条件。整个工作可以分为两部分：

* 国际化
  软件无需在修改或重新编译程序代码，就可以给特定的用户群使用。

* 本地化
  让特地的用户群得以使用软件，可能需要翻译软件文件和输出文字，以及货币、日期、数字、时间、单位换算等格式。


#### 第三章 查找与替换

* 如果需要从输入的数据文件中取出特定的文本行，主要工具为 grep 程序。有三种不同的变体，通过不同的选项，分别提供这三种行为模式。
  1. grep
  2. egrep
  3. fgrep

* POSIX 正则表达式分为两种：
  1. 基本正则表达式（BRE）
  2. 扩展正则表达式（ERE）

##### 查找文本

  ```
  # 简单的 grep
  $ who | grep austen       # 查找用户 austen

  # 正则表达式匹配范例
  # 匹配原则：从最左边开始，拓展至最长（longest leftmost)
  $ tolstoy                 # 位于一行上任何位置
  $ ^tolstoy                # 在开头
  $ tolstoy$                # 在结尾
  $ ^tolstoy$               # 正好包含，没有其他任何字符
  $ [Tt]olstoy              # T 或 t
  $ tol.toy                 # . 表示一个字符
  $ tol.*toy                # .* 表示 0 或多个字符

  # 字符集(需要在方括号表达式内使用)
  $ [[:alnum:]]             # 字母和数字
  $ [[:alpha:]]             # 字母
  $ [[:balnk:]]             # 空格 space 与定位符 tab
  $ [[:cntrl:]]             # 控制字符
  $ [[:digit:]]             # 数字
  $ [[:graph:]]             # 非空格字符 nonspace
  $ [[:lower:]]             # 小写字母
  $ [[:print:]]             # 可显示字符
  $ [[:punct:]]             # 标点符号
  $ [[:space:]]             # 空白字符 whitespace
  $ [[:upper:]]             # 大写字母
  $ [[:sdigit:]]            # 十六进制数字

  # 匹配一般字符
  $ a                       # 匹配字符 a 本身
  $ \                       # \ 转义字符
  $ .                       # . 任一字符
  $ c[aeiouy]t              # [] 方括号表达式：匹配方括号内任一字符
  $ [0-9]                   # 所有数字
  $ [0-9a-fA-F]             # 所有十六进制数字

  # 排序元素 [..]
  $ [ab[.ch.]de]            # 匹配字符 a、b、d、e 或者成对的 ch

  # 等价字符集
  $ [a[=e=]iouy]            # 所有小写英文字母元音，以及字母 è 和 é 等

  # 后向引用：在寻找重复字以及匹配引号时特别好用
  # 第一步：将子表达式包围在 \( 和 \) 里面，单个模式里可包括最多 9 个子表达式，且可为嵌套结构
  # 第二步：在同一模式之后使用 \digit，即匹配于第 n 个先前方括号内子表达式匹配成功的字符（1 ≤ n ≤ 9）
  $ \(ab\)\(cd\)[def]*\2\1              # abcd 开头且 cdab 结尾
  $ \(why\).*\1                         # 一行里重现两个 why
  $ \([[:alpha:]_][[:alnum:]_]*\)=\1;   # 简易 C/C++ 赋值语句
  $ \(["']\).*\1                        # 以单引号或者双引号括起来的字，例如 'foo' 或 "bar"

  # 单个表达式匹配多字符
  # 1、修饰符
  $ ab*c                    # 0个或多个
  $ ab+c                    # 1个或多个
  # 2、区间表达式
  $ a\{n\}                  # n个a
  $ a\{n,\}                 # 至少n个a
  $ a\{n,m\}                # n-m个a

  # 文本匹配锚点
  # 1、脱字符号：^，匹配开头
  # 2、货币符号：$，匹配结尾
  $ ^$                      # 同时使用，用来匹配空字符串或行列
  $ foo.c | grep -v '^$'    # 删除空行（v选项用来显示所有不匹配于模式的行）

  # 运算符优先级（从高到低）
  # 1、[..] [==] [::]       # 用于字符排序的方括号符号
  # 2、\metacharacter       # 转义的 meta 字符
  # 3、[]                   # 方括号表达式
  # 4、\(\) \digit          # 子表达式与向后引用
  # 5、* \{ \}              # 前置单个字符重现的正则表达式
  # 6、无符号                # 连续
  # 7、^ $                  # 锚点

  # 在文本文件里进行替换：使用 sed（流编辑器）
  # 1、用正则表达式查找并修改文本
  $ sed 's/:.*//' /etc/passwd |           # 删除第一个冒号之后的所有东西
  $  sort -u                              # 排序列表并删除重复部分
  # 2、插入命令
  $ find /home/tolstoy -type d -print  |  # 寻找所有目录
  $  sed 's;/home/tolstoy/;/home/lt/;' |  # 修改名称；注意这里使用分号作为定界符
  $   sed 's/^/mkdir /'                |  # 插入 mkdir 命令
  $    sh -x                           |  # 以 Shell 跟踪模式执行
  ```

##### 字段处理
  
  ```
  # 在文本文件下，一行表示一条记录，在一行内用来分隔字符的方法：
  # 1、用空格 space 或者制表符 tab（字段内容最好不要有空格）
  # 2、使用特定的定界符，例如冒号（定界符最好也不要称为数据内容）
  # 例子：etc/passwd，一行表示系统里的一个用户，每个字段都以冒号隔开
  # 字段依次为：用户名称、加密后的密码、用户id、用户组id、用户姓名、根目录、登陆的Shell
  # tolstoy:x:2076:10:Leo Tolstoy:/home/tolstoy:/bin/bash

  # 选定字段：cut 命令（-d设置定界符，-f设置字段范围，最后指定文件路径）
  $ cut -d : -f 1,5 /etc/passwd           # 显示系统上每个用户的登陆名称及其姓名（即第1、5个字段）
  # cut -d : -f 6   /etc/passwd           # 取出根目录
  # ls -l | cut -c 1-10                   # 取出ls -l命令输出结果中的文件权限字段

  # 将多个文件结合在一起：join 命令
  # 现在有两个文件：一个是业务员销售业绩sales，一个是业务员目标业绩quotas，都有两个字段：业务员和对应数值
  #! /bin/sh
  # merge-sales.sh
  # 删除注释并排序数据文件
  $ sed '/^#/d' quotas | sort $ quotas.sorted
  $ sed '/^#/d' sales  | sort $ sales.sorted
  # 以第一个键值（业务员名字）作结合，将结果产生至标准输出
  $ join quotas.sorted sales.sorted
  # 删除缓存文件
  $ rm quotas.sorted sales.sorted

  # 重新编排字段：awk
  # 1、基本架构：pattern { action }
  # 2、字段：awk 读取输入记录，并自动将各个记录切分为字段（每条记录的字段数目储存到内建变量NF）
  $ awk '{ print $1 }'                    # 打印第一个字段（未指定pattern）
  $ awk '{ print $2, $5 }'                # 打印第二个与第五个字段（未指定pattern）
  $ awk '{ print $1, $NF }'               # 打印第一个与最后一个字段（未指定pattern）
  $ awk 'NF $ 0 { print $0 }'             # 打印非空行（指定pattern与action），编号0表示整条记录
  $ awk 'NF $ 0'                          # 未指定action则默认为打印
  # 3、设置字段分隔符
  $ awk -F: '{ print $1, $5 }' /etc/passwd  # 设置分隔符为冒号，并输出第1、5个字段（登陆名称和用户姓名）
  # 4、打印行
  $ awk -F : '{ print "User", $1, "is really", $5 }' /etc/passwd
  $ awk -F : '{ printf "User %s is really %s\n", $1, $5}' /etc/passwd
  # 5、起始与清除模式
  $ awk 'BEGIN { FS = ":" ; OFS = "**" }
  $ { print $1, $5 }' /etc/passwd
  ```


#### 第四章 文本处理工具

##### 排序文本

  ```
  # 行的排序
  $ LC_ALL=C sort french-english          # 以严格的字节顺序排序文件

  # 以字段排序
  $ sort -t: -k1,1 /etc/passwd            # 以冒号隔开的第一个字符（用户名称）排序
  $ sort -t: -k3nr /etc/passwd            # 以字段三中的数值类型反向排序
  $ sort -t: -k4n -k3n /etc/passwd        # 先以字段四数值排序，再以字段三数值排序
  $ sort -t: -k4n -u /etc/passwd          # 以唯一的字段四数值排序（相同排序时只输出一条）

  # 文本块排序（先将文本块进行合并，再排序）
  # 1、在地址数据文件里的管道
  $ cat my-friends |
  # 2、转换地址为单行（gsub全局性替换）
  $  awk -v RS="" '{ gsub{"\n", "^Z"}; print }' |
  # 3、排序地址数据，忽略大小写
  $   sort -f |
  # 4、恢复行结构
  $    awk -v ORS="\n\n" '{ gsub{"^Z" ,"\n"}; print }' |
  # 5、删除标记行
  $     grep -v '# SORTKEY'
  ```

##### 删除重复

  ```
  # uniq 命令
  $ sort latin-numbers | uniq             # 显示唯一的、排序后的记录
  $ sort latin-numbers | uniq -c          # 计数唯一的、排序后的记录
  $ sort latin-numbers | uniq -d          # 仅显示重复的记录
  $ sort latin-numbers | uniq -u          # 仅显示未重复的记录
  ```

##### 重新格式化段落

  ```
  # fmt 命令
  # 1、重新格式化 20 个字典单词
  $ sed -n -e 9991,10010p /usr/dict/words | fmt
  # 2、重新将 10 个单词格式化为短的行
  $ sed -n -e 9995,10004p /usr/dict/words | fmt -w 30
  # 3、仅重新格式化长的行（长度大于10）
  $ fmt -s -w 10 << CONTENT_OF_TEXT
  ```

##### 计算行数、字数以及字符数

  ```
  # wc 命令
  $ echo Hello World | wc                 # 字符计数报告
  $ wc /etc/passwd /etc/group             # 计算两个文件里的数据
  ```

##### 打印

  ```
  # lp 命令（打印）
  # 1、Berkeley 风格
  $ lpr -Plcb102 sample.ps                # 将 PostScript 文件传给打印队列lcb102
  $ lpq -Plcb102                          # 查看该打印队列的状态
  $ lprm -Plcb102 81352                   # 停止该进程

  # 2、System V 风格
  $ lp -d lcb102 sample.ps                # 将 PostScript 文件传给打印队列lcb102
  $ lpstat -t lcb102                      # 查看该打印队列的状态
  # cancel lcb102-81352                   # 停止该进程

  # pr 命令（将文本文件编页，供打印用）
  # 1、保证每页页标题都起始于新的一页
  $ pr -f -160 -o10 -w65 file(s) | lp
  # 2、将 26 个单词格式化为 5 栏
  $ sed -n -e 19000,19025p /usr/dict/words | pr -c5 -t
  ```

##### 提取开头或结尾数行

  ```
  # 提取开头n行：head -n n
  # 提取结尾n行：tail -n n
  $ tail -n 25 -f /var/log/messages       # 观察不断成长的系统信息日志（f选项不会中断）
  ```


#### 第五章 管道的神奇魔力

##### 从结构化文本文件中提取数据

  ```
  #! /bin/sh
  # 过滤 /etc/passwd 这类格式的输入流，并从此数据衍生出办公室名录

  # 1、设置文件权限
  $ umask 077                             # 限制临时性文件只有我们能够访问

  # 2、定义变量来表示临时文件
  $ PERSON=/tmp/pd.key.person.$$          # 具有唯一性的临时文件名
  $ OFFICE=/tmp/pd.key.office.$$
  $ TELEPHONE=/tmp/pd.key.telephone.$$
  $ USER=/tmp/pd.key.user.$$

  # 3、工作终止
  $ trap "exit 1"                         # 无论是正常或异常终止，都要删除临时文件
  $ trap "rm -f $PERSON $OFFICE $ TELEPHONE $USER"

  # 4、读取标准输入放入临时文件，并进行处理
  $ awk -F: '{ print $1 ":" $5 }' > $USER
  $ sed -e 's=/.*==' \
  $     -e 's=^\([^:]*\}:\{.*\} \([^]*\)=\1:\3, \2=' <$USER | sort >$PERSON

  # 5、重新格式化输出，配合制表符 tab 分隔每个字段
  $ join -t: $PERSON $OFFICE |
  $     join -t: - $ TELEPHONE |
  $         cut -d: -f 2- |
  $             sort -t: -k1,1 -k2,2 -k3,3
  $                 awk -F: '{ printf{"%-39s\t%s\t%s\n", $1, $2, $3} }'
  ```

##### 针对 Web 的结构型数据

  ```
  <TABLE>
      <TR>
          <TD> Jones, Adrian W. </TD>
          <TD> 555-0123 </TD>
          <TD> OSD211 </D>
      </TR>
  </TABLE>
  ```

##### 文字解谜好帮手

  ```
  #! /bin/sh
  # 通过一对单词列表，进行类似 egrep(1) 的模式匹配
  # 语法：puzzle-help egrep-pattern [word-list-files]

  $ FILES="
  $         /usr/dict/words
  $         /usr/share/dict/words
            ...
  $       "
  $ pattern="$1"
  $ egrep -h -i "$pattern" $FILES 2> /dev/null | sort -u -f
  # -h 最后结果不要显示文件名，-i 忽略字母大小写，2> /dev/null 丢弃标准错误信息的输出

  # 以 b 开头，中间 5 个字符，加上 x 或 z，再加 3 个字符
  $ puzzle-help '^b.....[xz]...$' | fmt
  # 每行有 6 个辅音字母的单词
  $ puzzle-help '[^aeiouy]{6}' /usr/dict/words
  ```

##### 单词列表

  ```
  #! /bin/sh
  # 从标准输入读取文本流，再输出出现频率最高的前 n 个单词的列表（默认为25）
  # 附上出现频率的计数，按照这个计数从大到小排列
  # 输出到标准输出
  # 语法：wf [n]

  $ tr -cs A-Za-z\' '\n' |                # 将非字母字符转换为换行符
  $   tr A-Z a-z |                        # 所有大小字母转换为小写
  $     sort |                            # 从小到大排序单词
  $       uniq -c |                       # 去除重复，并显示其计数
  $         sort -k1,1nr -k2 |            # 计数从大到小排序后，再按照单词从小到大排序
  $           sed ${1:-25}q               # 显示前 n 行（默认为25）

  # 用法：
  # 1、使用 pr 重新格式化输出《哈姆雷特》，以每行 4 列显示
  $ wf 12 < hamlet | pr -c4 -t w80
  # 2、计算去除重复字后有多少单词出现在此剧中（需要一个足够大的值）
  $ wf 999999 < hamlet | wc -l
  # 3、最不常出现的字有哪些（仅显示一部分）
  $ wf 999999 < hamlet | tail -n 12`| pr -c4 -t -w80
  # 4、有几个单词是仅出现一次的
  $ wf 999999 < hamlet | grep -c '^ *1.'
  # 5、有几个单词是经常出现的核心单词
  $ wf 999999 < hamlet | awk '$1 >= 5' | wc-l
  ```


#### 第六章 变量、判断、重复动作

##### 变量与算术

  ```
  # 1、变量赋值与环境
  # 给变量赋值
  $ hours_per_day=24
  $ seconds_per_hour=3600
  $ days_per_week=7

  # 将变量设为只读模式
  $ readonly hours_per_day seconds_per_hour days_per_week

  # 将新变量添加到环境中
  $ PATH=$PATH:/usr/local/bin
  $ export PATH

  # 可以将赋值和命令结合到一起
  $ readonly hours_per_day=24 seconds_per_hour=3600 days_per_week=7
  $ export PATH=$PATH:/usr/local/bin

  # 显示当前环境中的变量
  $ export -p

  # 变量可以添加到程序环境中，但是对 Shell 或者接下来的命令不会一直有效
  $ PATH=$PATH:/usr/local/bin awk '...' file1 file2

  # 删除环境变量 / 改变环境变量值
  $ env -i PATH=$PATH HOME=$HOME LC_ALL=C awk '...' file1 file2

  # 从执行的 Shell 中删除变量与函数
  $ unset -v full_name                  # 删除变量
  $ unset -f who_is_on                  # 删除函数

  # 2、参数展开
  # 2.1、展开运算符
  $ reminder="Time to go to the dentist!"
  $ sleep 120
  $ echo $reminder
  $ echo _${reminder}_

  # 默认情况下未定义变量会展开为空字符串，这个时候就可能会导致灾难发生
  $ rm -fr /$MYPROGRAM

  # 2.2、替换运算符
  # 冒号是可选的，如果没有冒号，则“存在且非空”改为“存在”（即用于值测试）
  $ {varname:-word}                     # 如果varname存在且非空，则返回其值，否则返回word
  $ {varname:=word}                     # 如果varname存在且非空，则返回其值，否则赋值为word并返回
  $ {varname:+word}                     # 如果varname存在且非空，则返回word，否则返回null
  $ {varname:?message}                  # 如果varname存在且非空，则返回其值，否则显示message并退出
  
  # 2.3、模式匹配运算符
  $ {variable#pattern}                  # 如果模式匹配于变量开头，则删除匹配的最短部分，并返回剩下部分
  $ {variable##pattern}                 # 如果模式匹配于变量开头，则删除匹配的最长部分，并返回剩下部分
  $ {variable&pattern}                  # 如果模式匹配于变量结尾，则删除匹配的最短部分，并返回剩下部分
  $ {variable&&pattern}                 # 如果模式匹配于变量结尾，则删除匹配的最长部分，并返回剩下部分

  # 2.4、位置参数
  $ echo first arg is $1                # 用正整数表示
  $ echo tenth arg is ${10}             # 大于9时应该用花括号括起来
  $ filename=${1:-/dev/tty}             # 将值测试与模式匹配运算符应用到位置参数

  # 对参数的访问
  $ $#                                  # 参数总数
  $ $*/$&                               # 一次性表示所有的命令行参数
  $ "$*"                                # 将所有参数视为单个字符串，等同于"$1 $2 ..."
  $ "$@"                                # 将所有参数视为单独字符串，等同于"$1" "$2"

  # 修改参数
  $ set -- hello "hi there" greetings   # 设置位置参数（一共三个参数，但是有四个单词，--没有给选项）
  $ echo there are $# total arguments   # 显示计数值

  # 循环处理每一个参数
  $ for i in $*                         # 1、在没有双引号的情况下，$*和$@是一样的（输出：四行，每个单词一行）
  $ for i in "$*"                       # 2、加了双引号，表示一个字符串（输出：四个单词一行）
  $ for i in "$@"                       # 3、加了双引号，保留真正的参数值（输出：三行，每个参数一行）
  > do echo i is $1
  > done
  $ shift                               # 截去第一个参数
  $ echo there are now $# arguments     # 现在第一个参数已经消失了

  # 2.5、特殊变量
  $ #                                   # 目前进程的参数个数
  $ @                                   # 目前进程的命令行参数，置于双引号内时会展开为个别的参数
  $ *                                   # 目前进程的命令行参数，置于双引号内时会展开为一单独参数
  $ -                                   # 在引用时给予 Shell 的选项
  $ ?                                   # 前一命令的退出状态
  $ $                                   # Shell 进程的进程编号（process ID）
  $ 0                                   # Shell 程序的名称
  $ !                                   # 最近一个后台命令的进程编号
  $ ENV                                 # 环境
  $ HOME                                # 根目录（登陆）
  $ IFS                                 # 内部作为分隔符的字段列表，一般为空格、制表符和换行
  $ LANG                                # 当前 locale 的默认名称，其他的 LC_* 变量会覆盖其值
  $ LC_ALL                              # 当前 locale 的名称，会覆盖其他 LANG 与其他 LC_* 变量
  $ LC_COLLATE                          # 用来排序字符的当前 locale 名称
  $ LC_CTYPE                            # 在模式匹配期间，用来确定字符类别的当前 locale 名称
  $ LC_MESSAGES                         # 输出信息的当前语言的名称
  $ LINENO                              # 刚执行过的行在脚本或函数内的行编号
  $ NLSPATH                             # 在 $LC_MESSAGES(XSI) 所给定的信息语言里，信息目录的位置
  $ PATH                                # 命令的查询路径
  $ PPID                                # 输出信息的当前语言的名称
  $ PS1                                 # 主要的命令提示字符串，默认为"$"
  $ PS2                                 # 行继续的提示字符串，默认为"> "
  $ PS4                                 # 以 sex -x 设置的执行跟踪的提示字符串，默认为"+ "
  $ PWD                                 # 当前工作目录

  # 3、算术展开（默认顺序为从左到右）
  $ ++ --                               # 增加 / 减少，可前置也可放在末尾
  $ + - ! ~                             # 一元的正号与负号；逻辑与位的取反
  $ * / %                               # 乘法、除法、余数
  $ + -                                 # 加法和减法
  $ << >>                               # 向左位移、向右位移
  $ < <= > >=                           # 比较
  $ == !=                               # 相等、不等
  $ &                                   # 位的 AND
  $ ^                                   # 位的 Exclusive OR
  $ |                                   # 位的 OR
  $ &&                                  # 逻辑的 AND
  $ ||                                  # 逻辑的 OR
  $ ?:                                  # 条件表达式
  $ = += -+ *= /= %= &= ^= <<= >>= |=   # 赋值运算符

  # 用法
  $ echo $((3 && 4))                    # 真
  $ $((x += 2))                         # 为 x 增加 2，并将结果存储到 x
  $ echo $((i++))                       # 先返回原值，再执行自增
  $ echo $((++i))                       # 先执行自增，再返回值
  ```

##### 退出状态

  ```
  # 1、退出状态值
  # 内置变量 ? 返回最近一次执行程序的退出状态（除 0 外其余状态都为失败）
  $ echo $?                             # 输出退出状态
  $ exit 42                             # 脚本会立即退出，并且返回状态值 42

  # 2、if-elif-else-fi 语句
  # 使用程序的退出状态，最简单的方法就是使用 if 语句
  $ if grep pattern myfile > /dev/null
  $ then ...
  $ else ...
  $ fi

  # 3、逻辑的 NOT、AND 与 OR
  $ if ! grep pattern myfile > /dev/null
  $ if grep pattern1 myfile && grep pattern2 myfile
  $ if grep pattern1 myfile || grep pattern2 myfile

  # 4、test 命令
  # 产生可使用的退出状态，不是一般输出
  $ if test "$str1" = "$str2"             # 测试两个字符串是否相等
  $ if [ -f "$file" ] && ! [ -w "$file" ] # file 存在且为一般文件，但不可写入
  ```

##### case 语句
 
  ```
  $ case $1 in                            # 判断命令选项
  $ -f)
  $   ...
  $   ;;
  $ -d | --directory
  $   ...
  $   ;;
  $ *)
  $   echo $1: uknown option >&2
  $   exit
  $ esac
  ```

##### 循环

  ```
  # 1、for 循环
  $ for i in atlbrochure8.xml
  $ do
  $   echo $i
  $   mv $i $i.old
  $   sed 's/Atlanta/&, the capital of the South/' < $i.old > $i
  $ done

  # 2、while 与 until 循环
  # while：只要 condition 成功退出，while 会继续循环
  $ pattern=...
  $ while [ -n "$string” ]
  $ do
  $   string=${string%pattern}
  $ done

  # until：只要 condition 未成功结束，untiil 则执行循环
  # 等待某个用户登陆（每30秒确认一次）
  $ printf "Enter username: "
  $ read user
  $ until who | grep "$user" > /dev/null
  $ do
  $   sleep 30
  $ done

  # 也可以将管道放入到 while 循环中，用来重复处理每一行的输入
  $ 产生数据 |
  $ while read name rank serial_no
  $ do
  $   ..
  $ done

  # 3、break 与 continue
  # 等待某个用户登陆（每30秒确认一次）
  $ printf "Enter username: "
  $ read user
  $ while true
  $ do
  $   if who | grep "$user" > /dev/null
  $   then 
  $     break
  $   fi
  $   sleep 30
  $ done

  # break 和 contine 都接受可选的数值参数，用来指出要中断或继续多少个被包含的循环
  # 如果循环计数需要的是一个在运行时刻被计算的表达式，可以使用 $((...)) 表示
  $ while condition1                    # 外部循环
  $ do ...
  $   while condition2                  # 内部循环
  $     do ...
  $       break 2                       # 中断外部循环
  $     done
  $ done
  $ ...                                 # 在中断之后，继续执行这里的程序

  # 4、shift 与选项的处理
  # 用来处理命令行参数的时候，一次向左位移一位或更多位
  $ file=  verbose=  quiet=  long=      # 将变量设置为空值
  $ while [ $# -gt 0 ]                  # 执行循环，直到没有参数为止
  $ do
  $   case $1 in                        # 检查第一个参数
  $   -f) file=$2                       # 移位退出 "-f"，使得结尾的 shift 得到在 $2 的值
  $       shift
  $       ;;
  $   -v) verbose=true
  $       quiet=
  $       ;;
  $   -q) quiet=true
  $       verbose=true
  $       ;;
  $   -l) long=true
  $       ;;
  $   --) shift                         # 传统上，以 -- 结束选项
  $       break
  $       ;;
  $   -*) echo $0: $1: unregnized option >&2
  $       ;;
  $   *)  break                         # 无选项参数，跳出循环
  $       ;;
  $   esac
  $   shift                             # 设置下一个重复
  $ done

  # getopets 可以简化选项处理
  $ file=  verbose=  quiet=  long=      # 将变量设置为空值
  $ while getopts :f:val opt            # 第一个冒号是处理错误的方式
  $ do
  $   case $opt in
  $   f)   file=$OPTARG
  $        ;;
  $   v)   verbose=true
  $        quiet=
  $        ;;
  $   q)   quiet=true
  $        verbose=true
  $        ;;
  $   l)   long=true
  $        ;;
  $   '?') echo "$0: invalid option -$OPTARG" >&2
  $        echo "Usage: $0 [-f file] [-vql] [files ...]" >&2
  $        exit 1
  $        ;;
  $   esac
  $ done
  $ shift $((OPTIND - 1))               # 删除选项，留下参数
  ```

##### 函数

  ```
  # wait_for_user --- 等待用户登陆
  # 
  # 语法：wait_for_user user [ sleeptime ]
  $ wait_for_user () {
  $   until who |grep "$1" > /dev/null
  $   do
  $     sleep ${2:-30}
  $   done
  $   return 0
  $ }

  # 引用方法
  $ wait_for+user tolstoy               # 等待用户 tolstoy，每 30 秒检查一次（默认）
  $ wait_for+user tolstoy 60            # 等待用户 tolstoy，每 60 秒检查一次
  ```

#### 第七章 输入/输出、文件与命令执行

##### 标准输入、标准输出与标准错误输出

默认情况下，它们会读取标准输入、写入标准输出，并将错误信息传递到标准错误输出，这样的程序称为过滤器。

##### 使用 read 读取行

  ```
  # 读入一个变量
  $ x=abc ; printf "x is now '%s'. Enter new value: " $x ; read x
  # 读入多个变量
  $ read name rank serno
  # 如果输入单词多于变量，最后剩下的单词全部被指定给最后一个变量
  # 理想的行为应该转义这个法则：使用 read 搭配单一变量，将整行输入读取到该变量中
  # 当给定 -r 选项时，read 不会将结尾的反斜杠视为特殊字符（read 默认反斜杠为继续读取下一行）
  $ read -r name rank serno
  $ tolstoy \                               # 只读取两个变量

  # 复制目录树
  # 这个脚本并不完美，特别是它无法保留原始目录的所有权与使用权限
  $ find /home/tolstoy -type d -print    |  # 寻找所有目录
  $   sed 's;/home/tolstoy/;/home/lt/;'  |  # 更改名称（使用分号定界符）
  $     while read newdir                   # 读取新的目录名
  $     do
  $       mkdir $newdir                     # 新建目录
  $     done
  ```

##### 关于重定向

  ```
  # 1、读取与写入操作
  # < 以只读模式打开文件，而 > 以只写模式打开文件
  $ program <> file

  # 2、文件描述符处理
  # 文件描述符 0、1、2 分别对应标准输入、标准输出以及标准错误输出
  $ make 1> results 2> ERRS
  # 舍弃错误信息
  $ make 1> results 2> /dev/null
  # 将输出和错误信息送给相同的文件
  $ make > results 2>&1                     # 将 2 送到 1 的位置（中间不能有任何空格）
  # 注意顺序，从左到右处理重定向
  $ make 2>&1 > results
  # 在文件描述符重定向之前会处理管道
  $ make 2>&1 | ...

  # 3、改变 I/O 设置
  $ exec 2> /tmp/$0.log                     # 重定向标准错误输出
  $ exec 3< /some/file                      # 打开新文件描述符 3
  $ read name rank serno <&3                # 从该文件读取

  # 如果希望取消（undo）标准错误输出的重定向，可以先把它复制到一个新文件以存储描述符
  $ exec 5>&2                               # 把原来的标准错误输出保存到文件描述符 5 上
  $ exec 2> /tmp/$0.log                     # 重定向标准错误输出
  $ ...                                     # 执行各种操作……
  $ exec 2>&5                               # 将原始文件复制到文件描述符 2
  $ exec 5>&-                               # 关闭文件描述符 5，因为不再需要了
  ```

##### printf 的完整介绍

##### 波浪号展开与通配符

  ```
  # 1、波浪号展开
  # ~ 表示 当前用户的根目录，可以避免在程序里把路径直接编码
  $ read user
  $ vi ~$user/.profile

  # 2、使用通配符
  $ ?                                       # 任何的单一字符
  $ *                                       # 任何的字符字符串
  $ [set]                                   # 任何在 set 里的字符
  $ [!set]                                  # 任何不在 set 里的字符
  $ [a-c]                                   # a b c
  $ [a-z]                                   # 小写字母
  $ [!0-9]                                  # 非数字字符
  $ [0-9!]                                  # 数字或感叹号
  $ [a-zA-Z0-9_-]                           # 字母、数字、下划线或破折号

  $ echo .*                                 # 显示隐藏文件
  $ ls -la                                  # 列出隐藏文件
  ```

##### 命令替换

  ```
  # 1、使用反引号
  $ for i in `cd /old/code/dir ; echo *.c`
  $ do
  $   diff -c cd /old/code/dir/$i $i | more
  $ done

  # 需要小心地转移反斜杠字符和双引号
  $ echo outer `echo inner1 \`echo inner2\` inner1` outer
  $ echo "outer +`echo inner -\`echo \"nested quote\" here\`- inner`+ outer"

  # 2、将命令括在 $(...) 里面
  $ echo outer $(echo inner1 $(echo inner2) inner1) outer
  $ echo "outer +$(echo inner -$(echo "nested quote" here)- inner)+ outer"

  # 3、简易数学：expr
  # 优先级从小到大
  $ e1 | e2                                 # 
  $ e1 & e2                                 # = != < <= > >=
  $ e1 = e2                                 # = -
  $ e1 + e2                                 # * / 5
  $ e1 * e2                                 # 
  $ e1 : e2
  $ ( expression )
  $ integer
  $ string
  ```

##### 引用

  ```
  # 1、反斜杠转义
  $ echo here is a real star: \* and a real question mark: \?

  # 2、单引号
  $ echo 'here are some metachararcters: * ? [abc] ` $ \'
  # 混用单引号与双引号
  $ echo 'He said, "How'\''s tricks?"'

  # 3、双引号
  $ x="I am x"
  $ echo "\$x is \"$x\". Here is some output: '$(echo Hello World)'"
  ```

##### 执行顺序与 eval

##### 内建命令

#### 第八章 产生脚本

#### 第九章 awk 的惊人表现

#### 第十章 文件处理

##### 列出文件

  ```
  # 列出文件
  $ echo /bin/*sh                         # 显示 bin 下的 sh 脚本
  $ ls /bin/*sh | cat                     # 在输出管道里显示

  # 列出长文件
  $ ls -l /bin/*sh

  # 列出文件的 meta 数据
  ```

##### 使用 touch 更新修改时间

  ```
  # 改变文件的最后修改时间
  $ touch -t 197607040000.00 US-bicentennial

  # 复制参照文件的时间戳
  $ touch -r US-bicentennial birthday     # 把时间戳复制到新的 birthday 文件
  ```

##### 临时性文件的建立和使用

  ```
  # /tmp 和 /var/tmp 目录存储临时文件
  $ df /tmp                               # 显示 /tmp 下的磁盘剩余空间

  # 创建临时文件
  $ TMPFILE=`mktemp /tmp/myprog.XXXXXXXXXX` || exit 1
  $ ls -l $TMPFILE
  ```

##### 寻找文件

  ```
  # 1、快速寻找文件
  $ locate gcc-3.3.tar
  $ locate gcc-3.3.tar | fgrep .tar.gz
  $ locate '*gcc-3.3*.tar*'

  # 2、寻找命令存储位置
  $ type gcc                              # gcc 命令位置 

  # 3、查找文件
  $ find .                                # 寻找所有文件
  $ find | LC_ALL=C sort                  # 以传统顺序排序 find 的输出结果
  $ find -ls                              # 寻找文件，并使用 ls 风格的输出结果
  $ find -ls | sort -kll                  # 寻找文件，并以文件名排序
  $ find 'o*'                             # 寻找此目录下以 o 开头的文件
  $ find sub                              # 在 sub 目录下寻找文件
  $ find -prune                           # 不要在此目录下寻找
  $ find * -prune                         # 寻找此目录下的文件
  $ ls -d *                               # 列出文件，但没有目录内容
  $ find . -size +0 -a -size -10          # 寻找文件大小块小于 10 的非空文件
  $ find . -size 0 -o -atime +365         # 寻找空文件，或过去一年都未读取过的文件
  ```

##### 执行命令：xargs

##### 文件系统的空间信息

  ```
  # 1、df 命令：文件单行摘要
  $ df -k
  $ df -h
  $ df -lk /dev/sda6 /var

  # 2、du 命令：磁盘用量
  $ du -s /var/log /var/spool /var/tmp
  ```

##### 比较文件

  ```
  1、cmp 与 diff
  $ cp /bin/ls /tmp                       # 创建副本
  $ cmp /bin/ls /tmp/ls                   # 比较原始文件与副本

  $ echo Test 1 > test.1
  $ echo Test 2 > test.2
  $ diff test.[12]                        # 比较两个相似文件有何不同

  # 2、patch
  $ diff -c test.[12] > test.dif          # 将相异处的相关内文存储到文件
  $ patch < test.dif                      # 用 dif 文件修补 test.1 文件

  # 3、文件校验和匹配
  $ md5sum /bin.l?

  # 4、数字签名验证
  $ ls -l coreutils-5.0.tar*
  $ gpg coreutils-5.0.tar.gz.sig          # 尝试验证此签名
  ```

#### 第十三章 进程

  ```
  # 显示开机至今的时间、用户数和平均负载
  $ uptime

  # 查看进程状态（PID-进程ID/PPID-父进程ID）
  $ ps -efl
  
  # 显示前列资源消耗情况
  $ top

  # 删除进程（ABRT-中断/HUP-搁置/KILL-删除/TERM-终结）
  $ kill -HUP 25094

  # 捕捉进程信号
  $ trap

  # 进程追踪
  $ PS1='traced-sh$ ' strace -e trace=process /bin/sh
  $ traced-sh$ pwd
  $ traced-sh$ /bin/pwd
  $ traced-sh$ exit

  # 查看进程帐
  $ acctcom -a
  $ acctcom -a | cut -d ' ' -f 1 | sort | uniq - c | sort -klnr -k2 |head -n 10

  # 延迟的进程调度
  $ sleep 3600
  $ at now + 10 minutes
  $ batch < command-file
  $ crontab -l

  # /proc 文件系统（每个执行中的进程都会拥有一个子目录）
  $ ls -l /proc/16521
  $ cat -v /proc/16521/cmdline
  ```
