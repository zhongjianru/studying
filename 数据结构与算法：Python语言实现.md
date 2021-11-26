第 1 章 Python入门
第 2 章 面向对象编程
第 3 章 算法分析
第 4 章 递归
第 5 章 基于数组的序列
第 6 章 栈、队列和双端队列
第 7 章 链表
第 8 章 树
第 9 章 优先级队列
第 10 章 映射、哈希表和跳跃表
第 11 章 搜索树
第 12 章 排序与选择
第 13 章 文本处理
第 14 章 图算法
第 15 章 内存管理和B树

#### 前言

高效数据结构的设计与分析，长期以来一直被认为是计算领域的一个重要主题，同时也是计算机科学与计算机工程本科教学中的核心课程。本书介绍数据结构和算法，包括其设计、分析和实现，可在初级数据结构或中级算法导论课程中使用。我们随后会更详细地讨论如何在这些课程中使用本书。

为了提高软件开发的健壮性和可重用性，我们在本书中采取一致的面向对象的视角。面向对象方法的一个主要思想是数据应该被封装，然后提供访问和修改它们的方法。我们不能简单地将数据看作字节和地址的集合，数据对象是抽象数据类型（Abstract Data Type，ADT）的实例，其中包括可在这种类型的数据对象上执行的操作方法的集合。我们强调的是对于一个特定的 ADT 可能有几种不同的实现策略，并探讨这些选择的优点和缺点。我们几乎为书中的所有数据结构和算法都提供了完整的 Python 实现，并介绍了将这些实现组织为
可重用的组件所需的重要的面向对象设计模式。

通过阅读本书，读者可以：

* 对常见数据集合的抽象有一定了解（如栈、队列、表、树、图）。
* 理解生成常用数据结构的高效实现的算法策略。
* 通过理论方法和实验方法分析算法的性能，并了解竞争策略之间的权衡。
* 明智地利用编程语言库中已有的数据结构和算法。
* 拥有大多数基础数据结构和算法的具体实现经验。
* 应用数据结构和算法来解决复杂的问题。

为了达到最后一个目标，我们在书中提供了数据结构的很多应用实例，包括∶文本处理系统，结构化格式（如HTML）的标签匹配，简单的密码技术，文字频率分析，自动几何布局，霍夫曼编码，DNA序列比对，以及搜索引擎索引。

本书提供一系列丰富的在线资源，可访问以下网站获取：[在线资源](www.wiley.com/college/goodrich)

#### 第 1 章 Python入门

##### Python 概述

* 一种解释语言
* 依赖缩进（一般一条语句写在一行，利用 \ 可以将一条命令写在多行）

##### Python 对象

赋值语句

* 等号左边作为标识符（名称）与等号右边表示的对象相关联

标识符

* 大小写敏感，不能以字母开头，不能使用保留字作为标识符
* 每个标识符与其所引用的对象的内存地址隐式相关联
* 可以分配给一个名为 None 的特殊对象（与 Java 或 C++ 中空引用的目的相似）
* 动态类型语言，标识符的数据类型不需要实现声明（与 Java 或 C++ 不同）
* 标识符可以与任何类型的对象相关联，并可以重新分配给另一个对象
* 虽然标识符没有被声明为确切的类型，但它所引用的对象有一个明确的类型
* 可以通过向现有对象指定第二个标识符来建立别名，一旦建立了别名，两个名称都可用来访问底层对象
  1. 使用一个别名而通过另一个别名改变对象
  2. 如果对象的一个别名被复制语句重新赋予了新的值，并不影响已存在的对象

内置类

* 如果类的每个对象在实例化时有一个固定的值，并且在随后的操作中不会被改变，那么就是不可变的类
  1. 不可变类：bool、int、float、tuple、str、frozenset
  2. 可变类：list、set、dict

* 所有的类都支持传统构造函数形式创建基于一个或多个现有值的实例，传统构造函数如 bool()、int()、float() 等

* bool
  1. 就数字而言，如果为零则为 False 否则为 True
  2. 对于序列和其他容器类型（如字符串和列表），如果是空为 False 非空则为 True
  3. 也就是说，可以使用非布尔类型的值作为控制结构的条件

* int
  1. 不像 Java 和 C++ 支持不同精度的不同整数类型（int、short、long），Python 会根据其整数的大小自动选择内部表示的方式
  2. 可以使用二进制（以0b开头）、八进制（以0o开头）或者十六进制（以0x开头）来表示一个整型值
  3. 构造函数 int() 可以用于构造基于另一类型值的整数值，例如 int(f) 得到浮点数 f 的整数部分，还可以把进制作为第二个可选参数传入，如果传入的参数无法解析为整数，将会产生 ValueError 异常

* float
  1. Python 中唯一的浮点类型，使用固定精度表示，更像是 double 型
  2. 可以使用小数或者科学计数法表示，如 2. 或者 6.022e23

* 序列类型：list、tuple、str，代表许多值的集合，集合中值的顺序很重要

* list
  1. 表示任意对象的序列，列表的元素可以是任意对象（包括 None 对象）
  2. 列表是基于数组的序列，采用**零索引**，因此一个长度为 n 的列表包含索引号从 0 到 n-1 的对象
  3. 具备随着需求动态扩展和收缩存储容量的能力
  4. 使用字符 [] 作为列表的分隔符，[] 本身表示一个空列表
  5. list() 构造函数默认产生一个空列表，list('hello') 产生一个单个字符的列表（h、e、l、l、o）

* tuple
  1. list 的一个不可变的版本，可以看做列表类一种简化的内部表示
  2. 使用 () 表示元组，() 代表一个空的元组。为了表示只有一个元素的元组，该元素之后必须有一个逗号并在圆括号之内，例如 (17,)

* str
  1. 文本字符**不可变**的序列，字符可以看做长度为 1 的字符串
  2. 可以用单引号或者双引号括起来（在序列中使用另一个引号字符作为一个实际字符，如 "Don't worry"），反斜杠也可以实现这个目的（如 'C:\\Python\\' 实际上要表达的是一个反斜杠作为路径），其他常用的转义字符有 \n（换行符）和 \t（制表符）
  3. 在字符串的收尾使用连续三个单引号或者双引号，使得换行符可以在字符串中自然出现，而不用使用转义字符（三引号表示段落，可以直接使用换行）

* set
  1. 许多元素的集合，集合中没有重复的元素，而且元素之间没有内在联系，如 set('hello') 产生集合 {'h','e','l','o'}
  2. 与列表相反，使用集合的主要优点是有一个高度优化的方法来检查特定元素是否包含在集合内（基于散列表）
  3. 没有特定顺序
  4. 只有不可变类型的实例才能被添加到集合
  5. frozenset 是集合类型的一种不可变的形式
  6. 使用 {} 作为集合的分隔符，但是 {} 并不表示一个空集合，而是一个空字典，构造函数 set() 返回一个空集合

* dict
  1. 表示字典或者映射，即从一组不同的键中找到对应的值
  2. 字典也使用 {} 表示，因为在 Python 中字典类型是早于集合类型出现的，{} 表示一个空字典，非空字典用逗号分隔一系列的键值对表示，例如 {'ga':'Irish','de':'German'}
  3. 构造函数 dict() 接受一个现有的映射作为参数，也可以接受一系列键值对作为参数，例如 dict(pairs) 中 pairs=[('ga','Irish'),('de'.'German')]

##### 表达式、运算符和优先级

* 逻辑运算符
  1. not 逻辑非
  2. and 逻辑与（短路保护）
  3. or  逻辑或（短路保护）

* 相等运算符
  1. is     同一实体
  2. is not 不同实体
  3. ==     等价
  4. !=     不等价

* 比较运算符
  1. <   小于
  2. <=  小于等于
  3. \>  大于
  4. \>= 大于等于

* 算术运算符
  1. \+  加
  2. \-  减
  3. \*  乘
  4. /   除
  5. //  整数除法
  6. %   模运算符

* 位运算符
  1. ~   取反（前缀一元运算符）
  2. &   按位与
  3. |   按位或
  4. ^   按位异或
  5. <<  左移位，用零填充
  6. \>> 右移位，按符号位填充

* 序列运算符
  1. s[j]                 # 索引下表为j的元素
  2. s[start:stop]        # 切片操作得到索引为[start,stop)的序列
  3. s[start:stop:step]   # 切片操作，步长为step
  4. s + t                # 序列的连接
  5. k * s                # 序列s连接k次
  6. val in s             # 检查元素val在序列s中
  7. val not in s         # 检查元素val不在序列s中

* 序列类型支持
  1. s==t  相等（每一个元素对应相等）
  2. s!=t  不相等
  3. s< t  字典序的小于
  4. s<=t  字典序的小于或等于
  5. s> t  字典序的大于
  6. s>=t  字典序的大于或等于

* 集合运算符
  1.  key in s            # 检查key是s的成员
  2.  key not in s        # 检查key不是s的成员
  3.  s1==s2              # s1等价于s2
  4.  s1!=s2              # s1不等价于s2
  5.  s1<=s2              # s1是s2的子集
  6.  s1< s2              # s1是s2的真子集
  7.  s1>=s2              # s1是s2的超集
  8.  s1> s2              # s1是s2的真超集（s1不等于s2）
  9.  s1 | s2             # s1与s2的并集
  10. s1 & s2             # s1与s2的交集
  11. s1 - s2             # s1与s2的差集
  12. s1 ^ s2             # 对称差分（该集合中的元素在s1和s2的其中之一）

* 字典运算符
  1. d[key]               # 给定键key所关联的值
  2. d[key]==value        # 设置（或充重置）与给定的键相关联的值
  3. del d[key]           # 从字典中删除键及其关联的值
  4. key in d             # 检查key是d的成员
  5. key not in d         # 检查key不是d的成员
  6. d1==d2               # d1等价于d2
  7. d1!=d2               # d1不等价于d2

* 拓展赋值运算符
  1. count+=5

* 复合表达式和运算符优先级（从高到低排序）
  1.  expr.member                       # 成员访问
  2.  expr(...) expr[...]               # 函数/方法调用，容器下标/切片
  3.  **                                # 幂运算
  4.  +expr -expr ~expr                 # 一元运算符
  5.  * / // %                          # 乘法，除法
  6.  + -                               # 加法，减法
  7.  << >>                             # 按位移位
  8.  &                                 # 按位与
  9.  ^                                 # 按位异或
  10. |                                 # 按位或
  11. is(not) == != < <= > >= (not)in   # 比较，包含
  12. not expr                          # 逻辑非
  13. and                               # 逻辑与
  14. or                                # 逻辑或
  15. val1 if cond else val2            # 条件判断
  16. = += -= *=                        # 赋值

* 多级赋值
  1. x=y=0                              # 将最右边的值赋给指定的多个标识符
  2. x<=x+y<=10                         # 不需要计算两遍中间值

##### 控制流程

```
  # 控制结构：冒号用于标识代码块的开始，缩进级别或嵌套结构指定代码块
  
  # 1、条件语句
  if first_condition:
    first_body
  elif second_condition:
    second_body
  elif third_condition:
    third_body
  else:
    fourth_body
  
  # 以下两种写法等价
  if response:
  if response != '':
  
  # 机器人控制器
  if door_is_closed:
    if door_is_locked:
      unlock_door()
    open_door()
  advance()
  
  # 2、while循环
  while condition:
    body
  
  # 3、for循环
  for element in iterable:
    body
  
  # 基于索引的for循环(找出列表中最大元素的索引)
  big_index = 0
  for j in range(len(data)):
    if data[j] > data[big_index]:
      big_index = j
  
  # 4、break和continue
  # break：终止内层循环
  # continue：停止当前迭代
  found = False
  for item in data:
    if item == target:
      found = True
      break
```

##### 函数

```
  1、信息传递
  # 函数：给定目标值出现的次数
  def count(data,target):
    n = 0
    for item in data:
      if item == target:
        n += 1
    return n
  
  # 函数：测试序列中是否有一个这样的值
  def contains(data,target):
    for item in data:
      if item == target:
        return True
    return False
  
  # 函数调用
  # data作为grades的别名，target作为字符串'A'的别名
  # data和target是count函数定义的局部范围内的形式参数
  prizes = count(grades,'A')
  
  # 默认参数
  # 实参优先传给前面没有默认值的参数
  # 如果一个参数有默认值，它后面的参数也必须有默认值
  def foo(a,b=15,c=7)
  
  # 函数：计算学生平均绩点（可以定制可选参数）
  def compute_gpa(grades,
                  points={'A+':4.0, 'A':4.0,'A-':3.67,
                          'B+':3.33,'B':3.0,'B-':2.67,
                          'C' :2.0, 'C':1.67,'D+':1.33,
                          'D' :1.0, 'F':0.0
                  })
    num_courses=0
    total_points=0
    for g in grades:
      if g in points:
        num_course+=1
        total_points+=points[g]
    return total_points/num_courses
  
  # 函数：range(start,stop,step)，但range(n)中n作为stop值，违反前面的原则
  def range(start,stop=None,step=1):
    if stop is None:
      stop=start
      start=0
    ...
  
  # 2、常见的内置函数
  abs(x)
  all(iterable)
  any(iterable)
  chr(integer)
  divmod(x,y)
  hash(obj)
  id(obj)
  input(prompt)
  isintance(obj,cls)
  iter(iterable)
  len(iterable)
  map(f,iter1,iter2,...)
  max(iterable/a,b,c,...)
  min(iterable/a,b,c,...)
  next(iterable)
  open(filename,mode)
  ord(char)
  pow(x,y)
  print(obj1,obj2,...)
  range(stop/start,stop/start,stop,step)
  reversed(sequence)
  round(x/x,k)
  sorted(iterable)
  sum(iterable)
  type(obj)
```

##### 简单的输入和输出

```
  # 1、控制台输入输出
  print()
  # 以空格分隔，末尾有一个换行符（sep参数指定分隔符，end参数指定结尾字符串）
  print(a,b,c,sep=':')

  input()
  # 可以指定提示信息，等待用户输入任意字符，直到按下返回键
  year=int(input('In what year were you born?'))
  reply=input('Enter x and y, sepaated by space:')
  pieces=reply.split()
  x=float(pieces[0])
  y=float(pieces[1])

  2、文件操作
  fp=open('sample.txt')   # 默认以mode='r'只读方式打开文件（w/r初始位置=0，a初始位置=文件末尾）
  fp.read(k)              # 返回可读文件中接下来的k个字符（不指定参数则返回所有内容）
  fp.readline()           # 从文件中读取一行内容，并以此作为一个字符串返回
  fp.readlines()          # 将文件的每行内容作为一个字符串存入列表中并返回该列表
  for line in fp          # 遍历文件的每一行
  fp.tell()               # 返回当前位置偏离开始处的字节数
  fp.write(string)        # 在可写文件的当前位置将string的内容写入（如果需要换行符则需手动写入）
  wp.writelines(seq)      # 在可写文件的当前位置写入给定序列的每个字符串
  print(...,file=fp)      # 将print函数的输出重定向给文件（输出文件内容）
```

##### 异常处理

```
  # 抛出异常
  raise ValueError('x cannot be negative)

  # 处理异常
  try:
    fp = open('sample.txt)
  except IOError as e:
    print('Unable to open the file:', e)

  # 处理多种异常
  age = -1
  while age <= 0:
    try:
      age = int(input('Enter age in years: '))
      if age <= 0:
        print('Your age must be positive')
    # 1、输出异常信息
    except(ValueError, EOFError):
      print('Invalid response')
    
    # 2、不处理异常
    except(ValueError, EOFError):
      pass

    # 3、分别输出异常信息并抛出异常
    except ValueError:
      print('That is an invalid age specification')
    except EOFError:
      print('There was an unexpected error reading input.')
      raise
    except:  # 其他异常
      pass
    finally:  # 总是被执行，通常用于清理工作，比如关闭一个文件
      print('done.')
```

##### 迭代器和生成器

```
  # 1、迭代器
  # 许多类型的对象都可迭代，比如基本的容器类型（列表、元组和集合）
  # 字符串可以生成字符的迭代，字典可以生成键的迭代，文件可以生成行的迭代
  # 迭代器：一个对象，通过一系列值来管理迭代
  next(i)         # i定义为一个迭代器对象，访问后续元素，如果没有后续元素则抛出异常
  iter(obj)       # 对象obj是可迭代的，产生一个迭代器

  # list的实例是可迭代的，但它本身不是一个迭代器，无法调用next(data)
  data = [1,2,4,8]
  i = iter(data)  # 产生list_iterator类的一个实例
  next(i)

  # 基于同一个可迭代对象可以创建多个迭代器，同时每个迭代器维护自身演进状态
  # 迭代器不储存列表元素，而是保存原始列表的当前索引，该索引指向下一个元素（如果原始列表在迭代完成之前被修改，迭代器将报告修改后的内容）

  # 隐式迭代序列值函数和类：无需立刻构建数据结构来存储所有的值
  # 返回可迭代的range对象，只有访问时才会生成值（懒惰计算法）
  range(1000000)
  
  # 2、生成器
  # 创建迭代器最方便的技术是使用生成器
  # 例1：确定一个正整数的所有因子
  def factors(n):
    results = []
    for k in range(1,n+1):
      if n % k == 0:
        results.append(k)
    return results
  
  # 例2：使用生成器实现
  def factors(n):
    for k in range(1,n+1):
      if n % k == 0:
        yield k  # yield this factor as next result

  # 生成器可以依赖不同构造中的多个yield语句，以及由控制的自然流决定的生成序列
  # 例3：在计算整数n的因子时，通过测试值达到这个数的平方根，同时指出每个k相关联的因子
  def factors(n):
    k = 1
    while k * k < n:
      if n % k == 0:
        yield k
        yield n//k
      k += 1
    if k * k == n:
      yield k

  # 3、懒惰计算
  # 只计算需要的数，并且整个系列的数不需要一次性全部驻留在内存中
  # 事实上，一个生成器可以有效地产生数值的无限序列
  # 例4：斐波那契数列
  def fibonacci():
    a = 0
    b = 1
    while True:
      yield a
      future = a + b
      a = b
      b = future
```

##### Python 的其他便利特性

```
  # 1、条件表达式
  expr1 if condition else expr2

  # 2、解析语法
  # expression 和 condition 都取决于 value，if 子句是可选的
  [ expression for value in iterable if condiion ]

  # 等价于：
  result = []
  for value in iterable:
    if condition:
      result.append(expression)

  # 例1：计算数的平方
  squares = []
  for k in range(1,n+1):
    squares.append(k*k)

  # 使用列表解析
  squares = [ k*k for k in range(1,n+1) ]

  # 例2：求整数n的因子
  factors = [ k for k in range(1,n+1) if n % k == 0 ]

  # Python 支持类似的集合、生成器或字典的解析语法
  [ k*k for k in range(1,n+1) ]     # 列表解析
  { k*k for k in range(1,n+1) }     # 集合解析
  ( k*k for k in range(1,n+1) )     # 生成器解析
  { k: k*k for k in range(1,n+1) }  # 字典解析

  # 当结果不需要存储在内存中时，生成器语法特别有优势
  # 例3：计算前n个数的平方和（将列表作为参数使用）
  total = sum(k * k for k in range(1,n+1))

  # 3、序列类型的打包和解包
  # 元组的自动打包：即使没有提供封闭的圆括号，也会被视为元组
  data = 2,4,6,8

  # 自动返回单个对象：即元组(x,y)
  return x,y

  # 自动解包：右边可以是任意迭代类型，只要左边变量数等于右边迭代元素数
  a,b,c,d = range(7,11)

  # 用来解包一个函数返回的元组
  quotient, remainder = divmod(a,b)

  # 遍历迭代序列时，相当于
  for x,y in [ (7,2),(5,8),(6,4) ]

  # 遍历由字典类的item()返回的键值对，相当于
  for k,v in mapping.items()

  # 4、同时分配：自动打包和解包结合起来
  # 将右边自动打包成一个元组，然后自动解包，将元素分配给左边的标识符
  x,y,z = 6,2,5

  # 同时分配技术先计算右侧再计算左侧
  # 在执行交换时，代表右边打包值的未命名元素相当于隐式的临时变量
  j,k = k,j
  # 相当于
  temp = j
  j = k
  k = temp

  # 例4：用同时分配技术生成斐波那契数列
  def fibonacci():
    a,b = 0,1
    while True:
      yield a
      a,b = b,a+b
```

##### 作用域和命名空间