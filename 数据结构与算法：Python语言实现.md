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

##### 模块和import语句

```
  # 1、导入模块
  # 将math引入当前命名空间
  import math
  math.sqrt(2)

  # 将math模块定义的pi和sqrt添加到当前命名空间（可能覆盖同名模块）
  from math import pi,sqrt
  sqrt(2)

  # 2、创建新模块
  # 将相关定义放在.py文件里，再导入

  # 3、直接调用的脚本
  if __name__ == '__main__'

  # 4、现有模块
  array
  collections
  copy
  heapq
  math
  os
  random
  Re
  sys
  time

  # 5、伪随机数生成
  next = (a * current + b) % n

  seed(hashable)              # 基于参数的散列值初始化伪随机数生成器
  random()                    # 在开区间(0.0,1.0)返回一个伪随机浮点数
  random(a,b)                 # 在闭区间[a,b]返回一个伪随机整数
  randrange(start,stop,step)  # 在参数指定的范围内返回一个伪随机整数
  choice(seq)                 # 返回一个伪随机选择的给定序列中的元素
  shuffle(seq)                # 重新排列给定的伪随机序列中的元素
```

#### 第 2 章 面向对象编程

##### 面向对象的目标、原则和模式

* 主体
  面向对象模式中的主体是对象，每个对象都是类的实例

* 类的定义
  通常包括对象包含的数据成员和成员函数

* 面向对象的设计目标
  1. 健壮性
  2. 适应性
  3. 可重用性

* 面向对象的设计原则
  1. 模块化
  2. 抽象化
  3. 封装

* 算法设计模式
  1. 递归
  2. 摊销
  3. 分治法
  4. 去除法（减治法)
  5. 暴力算法
  6. 动态规划

* 软件工程模式
  1. 迭代器
  2. 适配器
  3. 位置
  4. 合成
  5. 模板方法
  6. 定位器
  7. 工厂模式

##### 软件开发

* 软件开发阶段
  1. 设计
  2. 实现
  3. 测试和调试
   
* 伪代码
  1. 自然语句和高级编程结构的混合，用于描述隐藏在数据结构和算法实现之后的主要编程思想
  2. 是为读者设计的，不是为计算机设计的，因此可以交流复杂的思想，而不用担心底层具体细节的实现

* 编码风格
  1. 4个空格缩进
  2. 标识符命名要有意义
  3. 类应该以首字母大写的单数名词作为名字，如Date
  4. 函数名应该是动词，字母小写，多个单词用下划线分隔，如make_payment
  5. 变量名/参数名应该是小写的名词。如price
  6. 用大写字母和下划线隔开单词的标识符代表一个常量值，如MAX_SIZE
  7. 以单下划线开头的标识符意味着它们只在类或模块内部使用，如_secret
  8. 用注释给程序添加说明，解释有歧义或者令人困惑的结构

* 文档（docstring）
  1. 在源码中直接插入文档以提供完整的支持
  2. 任何出现在模块、类、函数主体中的第一个语句的字符串，限定在三引号中"""
  3. 作为模块、功能或者类的声明的一个域进行存储，可以作文档用，并且可以用多种方式检索

  ```
  def scale(data, factor):
  """Multiplyn all entries of numeric data list by the given factor
  data   an instance of any mutable sequence type(such as a list)
         containint numeric elements
  factor a number that serves as the multiplicative factor for scaling
  """
  for j in range(len(data)):
      data[j] *= factor
  ```

* 测试
  1. 应该用有代表性的输入子集来运行程序，至少应该确保类的每个方法都至少被执行一次（方法覆盖）
  2. 在特殊情况的输入下，程序往往会失败，需要仔细确认和测试这些情况
  3. 自动测试：if __name__ == '__main__'调用测试模块
  4. 单元测试：unittest模块将单个测试用例分组到更大的测试套件中，并分析测试结果
  5. 回归测试：通过对所有先前测试的重新执行，来确保对软件的更改不会在先前测试的组件中引入新的错误

* 调试
  1. 最简单的调试可以使用打印语句来跟踪程序执行过程中变量的值，但最终需要删除或注释掉打印语句
  2. 更好的方法是用调试器运行程序，调试器提供的基本功能是在代码中插入断点，程序中止时可以检查变量值

##### 类定义

  ```
  # 1、例子：CreditCard类
  # 调用函数时不需要传入 self 参数，解释器自动将调用对应函数的实例绑定为 self 参数
  class CreditCard:
    """ A consumer credit card. """
    
    # 构造函数：将参数值赋给实例变量，用实例变量创建对象
    # 数据成员名称以下划线开头，表名它被设计为非公有的，类的用户不应该直接访问这样的成员
    # get 函数访问内部变量，set 函数更新内部变量
    def __init__(self, customer, bank, acnt, limit):
      """ Create a new credit card instance. """
      The initial balance is zero.
      customer the name of the customer
      bank     the name of the bank
      acnt     the acount identifier
      limit    credit limit (measured in dollars)
      """

      self._customer = customer
      self._bank = bank
      self._account = acnt
      self._limit = limit
      self._balance = 0
    
    def get_customer(self):
      """ Return name of the customer. """
      return self._customer
      
    def get_bank(self):
      """ Return bank's name. """
      return self._bank
      
    def get_account(self):
      """ Return the card identifying number(typically stored as a string). """
      return self._account
      
    def get_limit(self):
      """ Return current credit limit. """
      return self._limit
      
    def get_balance(self):
      """ Return current balance. """
      return self._balance

    def charge(self, price):
      """ Charge given price to the card. assuming sufficient credit limit.
      Return True if charge was processed; False if charge was denied.
      """
      if price + self._balance > self._limit:
        return False
      else:
        self._balance += price
        return True
    
    def make_payment(self, amount):
      """ Process customer payment that reduces balance. """
      self._balance -= amount

  # 创建类实例
  cc = CreditCard('John Doe', '1st Bank', '5391 0375 9387 5309', 1000)

  # 测试类
  # 提供方法覆盖，每个方法至少被调用一次
  # 用 unitest 模块可以将得到的值与预期结果自动比较，只有当检测到错误时才产生输出
  if __name__ == '__main__':
    wallet = []
    wallet.append(CreditCard('John Bowman', 'California Savings', '5391 0375 9387 5309', 2500))
    wallet.append(CreditCard('John Bowman', 'California Federal', '3485 0399 3395 1954', 3500))
    wallet.append(CreditCard('John Bowman', 'California Finance', '5391 0375 9387 5309', 5000))

    for val in range(1, 17):
      vallet[0].charge(val)
      vallet[1].charge(val*2)
      vallet[2].charge(val*3)
  
    for c in range(3):
      print('Customer =', wallet[c].get_customer())
      print('Bank =', wallet[c].get_bank())
      print('Account =', wallet[c].get_account())
      print('Limit =', wallet[c].get_limit())
      print('Balance =', wallet[c].get_balance())
  
      while wallet[c].get_balance() > 100:
        wallet[c].make_payment(100)
        print('New balance =', wallet[c].get_balance())
      print()

  # 2、运算符重载和 Python 的特殊方法
  # 内置类为许多操作提供了自然语义，比如 a+b 可以调用数值类型语句，也可以连接序列类型
  # 当定义一个新类时，必须考虑是否应该定义类似 a+b 的语句
  # 运算符/非运算符通过调用特殊方法（以__开头）实现

  # 运算符重载
  a.__add__(b)      # a+b

  # 当一个二元操作符应用于两个不同类型的实例中时，根据左操作数的类进行判断
  3*'love me'

  # 非运算符重载
  # 当作用于数据结构时，我们经常定义 __len__() 方法来返回一个结构的大小
  foo.__str__()     # str(foo) 
  foo,__bool__()    # return true of false
  foo.__len__()     # len(foo)

  # 隐式的方法
  __add__()         # 未定义该方法将会导致 a+b 引用异常
  __bool__()        # 具有默认语义，将除了 None 以外的每个对象都返回 True
  __len__()         # 返回容器大小，对于长度为 0 的实例默认返回 False

  __eq__()          # a == b 即两个标识符引用等价值
                    # a is b 即两个标识符是否为同一对象的别名
                    # 如果没有实现该方法，这两种写法是等价的

  __ne__()          # a != b 即返回 not(a==b) 作为结果
  __lt__()          # a < b，但不支持 a <= b 的语义

  # 3、多维向量类（多维空间中的向量坐标）
  # 通过保持内部列表的封装，可以为类中的实例执行所请求的公共接口
  v = Vector(5)     # <0,0,0,0,0>
  v[1] = 23         # <0,23,0,0,0>
  v[-1] = 45        # <0,23,0,0,45>
  print(v[4])       # 45
  u = v+v           # <0,46,0,0,90>
  total = 0
  for entry in v:
    total += entry
  
  # 向量类定义
  class Vector:
    """ Represent a vector in a multidimensional space. """

    def __init__(self, d)
      """ Create d-dimensional vector of zeros. """
      self._coords = [0] * d

    def __len__(self):
      """ Return the dimension of vector. """
      return len(self._coords)

    def __getitem__(self, j):
      """ Return jth coordinate of vector. """
      return self._coords[j]

    def __setitem__(self, j, val):
      """ Set jth coordinate of vector to given value. """
      self._coords[j] = val

    def __add__(self, other):
      """ Return sum of two vectors. """
      if len(self) != len(other):         # relies on __len__ method
        raise ValueError('dimensions must agree.')
      result = Vector(len(self))          # start with vector of zeros
      for j in range(len(self)):
        result[j] = self[j] + other[j]
      return result

    def __eq__(self, other):
      """ Return True if vector has same coordinates as other. """
      return self._coords == other._coords

    def __ne__(self, other):
      """ Return True if vector differs from other. """
      return not self == other            # relies on existing __eq__ defination
    
    def __str__(self):
      """ Produce string representation of vector. """
      return '<' + str(slef._coords)[1:-1] + '>'

    # 4、迭代器
    # 集合的迭代器提供了一个关键功能：如果集合有下一个元素，则返回该元素，否则抛出异常
    # 使用生成器(generator)语法自动生成一个已有值的迭代器
    # 例：一个支持任何序列类型的迭代器类
    class SequenceIterator:
      """ An iterator for any of Python's sequence types. """

      def __init__(self, sequence):
        """ Create an iterator for the given sequence. """
        self._seq = sequence              # keep a reference to the underlying data
        self._k = -1                      # will increment to 0 on first call to next

      def __next__(self):
        """ Return the next element, or else raise an error. """
        self._k += 1                      # advance to next index, begin from 0
        if self._k < len(self._seq):
          return(self._seq[self._k])      # return the data element
        else:
          raise StorIteration()           # there are no more elements

      def __iter__(self):
        """ By convention, an iterator must return itself as an iterator. """
        return self

    # 5、Range 类
    # Python 2：会引起一个数字范围列表的实例化和初始化，对时间和内存造成不必要的浪费
    # Python 3：使用惰性求值策略，有效地表示所需元素范围，而不必在内存中明确储存元素
    # 模拟内置 Range 类，构建时通过参数计算元素个数，并返回指定值
    class Range:
      """ A class that mimic's the built-in range class. """

      def __init__(self, start, stop=None, step=1):
        """ Initialize a Range instance. Sementics is similar to built-in range class. """
        if step == 0:
          raise ValueError('step cannot be 0.')
        
        if stop is None:                  # special case of range(n)
          start, stop = 0, start          # should be treated as if range(0,n)

        # calculate the effective length once
        self._length = max(0, (stop - start + step - 1) // step)

        # need knowledge of start and step (but not stop) to support __getitem__
        self._start = start
        self._step = step

      def __len__(self):
        """ Return number of entries in the range. """
        return self._length

      def __getitem__(self, k):
        """ Return entry at index k (using standard interpretation if negative). """
        if k < 0:
          k += len(self)                   # attempt to convert negative index
        
        if not 0 <= k < self.length:
          raise IndexError('index out of range.')
        
        return self._start + k * self._step
  ```

##### 继承

  ```
  # 在一个分层的方式中，在水平层次上把类似的抽象定义组合在一起，下层组件更加具体，上层组件更加通用
  # 继承：模块化和层次化组织的机制，基于一个现有的类（基类/父类/超类）作为起点定义新的类（子类）
  # 子类可以通过覆盖现有方法实现特化，也可以通过提供全新方法拓展其父类
  # 通过 super() 调用从父类继承的方法

  # 1、拓展 CreditCard 类
  # 特化：当尝试收费由于超过信用卡额度被拒绝时，将会收取费用
  # 拓展：将有一个对未结清余额按月收取利息的机制，即基于构造函数的一个参数年利率（APR）
  # 子类直接访问数据成员 self._balance，是由父类建立的（名字带下划线代表它是一个非公有成员）
  # 保护成员：可以访问子类，但不能访问一般的公有类（名字以一个下划线开头）
  # 私有成员：既不能访问子类，也不能访问公有类（名字以两个下划线开头）
  # 为了保护余额，可以添加一个非公有的方法 _set_balance，子类通过该方法来改变余额而不直接访问数据成员

  class PredatoryCreditCard(CreditCard):
    "" An extension to CreditCard that compunds interest and fees. """

    def __init__(self, customer, bank, acnt, limit, apr):
      """ Create a new predatory credit card instance. 
          The initial balance is zero.
          customer  the name of the customer (e.g. 'John Bowman')
          bank      the name of the bank (e.g. 'California Savings')
          acnt      the caacnt identifier (e.g. '5391 0375 9387 5309')
          limit     creadi limit (measured in dollars)
          apr       annual percentage rate (e.g. 0.0825 for 8.25% APR)
      """
      super().__init__(customer, bank, acnt, limit)
      self._apr = apr

    def charge(self, price):
      """ Charge given price to the card, assuming sufficient credit limit.
          Return True if charge was processed.
          Return False and assess $5 fee if charge is denied.
      """
      success = super().charge(price)   # call inherited method
      if not success:
        self._balance += 5              # assess penalty
      return success                    # caller expects return value

    # extended method
    def process_month(self):
      """ Assess monthly interest on outstanding balance. """
      if self._balance > 0:
        # if positive balance, convert APR to monthly multiplicative factor
        monthly_factor = pow(1 + self._apr, 1/12)
        self._balance *= monthly_factor

  # 2、数列的层次图
  # 为了最大限度地提高代码的可重用性，给出一个由通用基类产生的 Progression 类的分层
  # 该类被设计为其他数列类型的基类，提供尽可能多的公共函数，并由此把子类的负担减至最小
  # 非公有方法 _advance 更新 self._current 域的值，但子类可以重写该方法，以不同方法来计算下一个值
  
  # 例：一个通用数字数列类
  class Progression:
    """ Iterator producing a grneric progression
        Default iterator produces the whole numbers 0,1,2,...
    """

    def __init__(self, start=0):
      """ Initialize current to the first value of the progression. """
      self._current = start

    def _advance(self):
      """ Update self._current to a new value. 
          This shoule be overridden by a subclass to customize progression.
          By convention, if current is set to None, this designates the end of a finite progression.
      """
      self._current += 1
    
    def __next__(self):
      """ Return the next element, or else raise an error. """
      if self._current is None:         # our convention to end a progression
        raise StopIteration()
      else:
        answer = self._current          # record current value to return
        self._advance()                 # advance to prepare for next time
        return answer
      
    def __iter__(self):
      """ By convention, an iterator must return itself as an iterator. """
      return self

    def print_progression(self, n):
      """ Print next n values of the progression. """
      print(' '.join(str(next(self)) for j in range(n)))
  
  # 例：一个等差数列类
  class ArithmeticProgression(Progression):
    """ Iterator producing an arithmetic progression. """

    def __init__(self, increment=1, start=0):
      """ Create a new arithmetic progression.
          increment  the fixed constant to add to each term (default 1)
          start      the first term of the progression (default 0)
      """
      super().__init__(start)           # initialize base class
      self._increment = increment

    def _advance(self):
      """ Update current value by adding the fixed increment. """
      self._current += self._increment

  # 例：一个等比数列类
  class GeometricProgression(Progression):
    """ Iterator producing a geometric progression. """

    def __init__(self, base=2, start=1):
      """ Create a new geometric progression.
          base    the fixed constant multiplied to each term (default 2)
          start   the first term of the progression (default 1)
      """
      super().__init__(start)
      self._base = base

    def _advance(self):
      """ Update current value by multilying it by the base value. """
      self._current *= self._base

  # 例：一个斐波那契数列类
  class FibonacciProgression(Progression):
    """ Iterator producing a generalized Fibonacci progression. """

    def __init__(self, first=0, second=1):
      """ Create a new fibonacci progression.
          first    the first term of the progression (default 0)
          second   the second term of the progression (default 1)
      """
      super().__init__(first)           # start progression at first
      self._prev = second - first       # fictitious value prociding first
    
    def _advance(self):
      """ Update current value by taking sum of previous two. """
      self._prev, self._current = self._current, self._prev + self._current

  # 例：数列类的单元测试
  if __name__ =='__main__':
    print('Default progression:')
    Progression().print.progression(10)

    print('Arithmetic progression with increment 5:')
    ArithmeticProgession(5).print.progression(10)

    print('Arithmetic progression with increment 5 and start 2:')
    ArithmeticProgession(5, 2).print.progression(10)

    print('Geometric progression with default base:')
    GeometricProgession().print.progression(10)

    print('Geometric progression with base 3:')
    GeometricProgession(3).print.progression(10)

    print('Fibonacci progression with default start values:')
    FibonacciProgession().print.progression(10)

    print('Fibonacci progression with values 4 and 6:')
    FibonacciProgession(4, 6).print.progression(10)

  # 3、抽象基类
  # 避免重复代码，可以被需要它的其他类所继承（比如上面的 Progression 类）
  # 这个类的唯一目的是作为继承的基类，不能直接实例化，而具体的类可以被实例化
  # 在静态语言（如 Java 和 C++）中，抽象基类作为一个正式的类型，可以确保一个或多个抽象方法
  # 这就为多态性提供了支持，因为变量可以有一个抽象基类作为其声明的类型，及时它是一个具体子类的实例
  # 在 Python 中没有声明类型，这种多态性不需要一个统一的抽象基类就可以实现
  # 因此 Python 中没有强烈要求定义正式的抽象基类，尽管 abc 模块提供了正式的抽象基类的定义

  # 模板方法模式：一个抽象基类在提供依赖于调用其他抽象行为时的具体行为
  # 只要一个类提供定义了缺失的抽象行为，继承的具体行为也就被定义了
  # 如果一个子类对从基类继承的行为提供自己的实现，那么新的定义就会覆盖之前继承的
  # 例：一个类似于 Collections.Sequence 的抽象基类
  # ABCMeta 类作为 Sequence 类的袁磊，为类定义本身提供一个模板，确保类的构造函数引发异常
  # @abstractmethod 声明不需要在抽象基类中提供实现，由具体子类来实现这个方法（禁止没有重载抽象方法的具体子类实例化）
  from abc import ABCMeta, abstractmethod

  class Sequence(metaclass=ABCMeta):
    """ Our own version of collections.Sequence abstract base class. """

    @abstractmethod
    def __len__(self):
      """ Return the length of the sequence. """

    @abstractmethod
    def __getitem__(self, j):
      """ Return the element at index j of the sequence. """

    def __contains__(self, val):
      """ Return True if val found in the sequence; False otherwise. """
      for j in range(len(self)):
        if self[j] == val:              # found match
          return True
      return False

    def index(self, val):
      """ Return leftmost index at which val is found (or raise an error). """
      for j in range(len(self)):
        if self[j] == val:              # leftmose match
          return j
      raise ValueError('value not in sequence')

    def count(self, val):
      """ Return the number of elements equal to given value. """
      k = 0
      for j in range(len(self)):
        if self[j] == val:
          k += 1
      return k

  # Range 类支持 __len__ 和 __getitem__ 方法，但不支持 count 和 index 方法
  # 将 Sequence 类声明为一个超类，那么它也将继承 count 和 index 方法
  class Range(collections.Sequence):
  ```

##### 命名空间和面向对象

  ```
  # 命名空间：管理特定范围内定义的所有标识符，将每个名称映射到相应的值
  # 第一类对象：函数、类、模块

  # 1、实例命名空间：管理单个对象的特定属性
  # 例如 CreditCard 类的每个实例都包含不同的余额、账号、信用额度等
  # 每个已定义的类都有一个单独的类命名空间，用于管理一个类的所有实例所共享的成员或没有引用任何特定实例的成员
  # 类成员中 self 作为限定词使用，使得 _balance 标识符直接被添加到实例命名空间中
  # 类发生继承后，每个对象仍有单一的实例命名空间

  # 一个类命名空间包含所有直接在类定义体内的声明
  class CreditCard:
    def make_payment(self, amount):
      ...
  
  # 类数据成员：当有一些值（如常量），被一个类的所有实例共享时，就可以用到类级的数据成员
  class PredatoryCreditCare(CreditCard):
    OVERLIMIT_FEE = 5                   # this is a class-level member

    def chanrge(self, price):
      success = super().charge(price)
      if not success:
        self._balance += PredatoryCreditCard.OVERLIMIT_FEE
      return success
  
  # 嵌套类
  # B 类是嵌套类，标识符 B 是进入了 A 类的命名空间相关联的一个新定义的类
  # 允许更高级形式的继承，使外部类的子类重载嵌套类的定义
  class A:                              # the outer class
    class B:                            # the nested class

  # 辅助字典
  # 默认情况下，每个命名空间均代表内置 dict 类的一个实例，即将范围内识别的名称与相关联的对象映射起来
  # 虽然字典结构支持相对有效的名称查找，但是需要的额外内存超出了存储原始数据的内存

  # __slots__ 声明
  # 使用流表示一个类的所有实例，类定义必须提供 _slots_ 的类成员分配给一个固定的字符串序列以服务于变量
  class CreditCard:
    __slots__ = '_customer', '_bank', '_account', '_balance', '_limit'

  # 子类也必须声明该变量
  class PredatoryCreditCard(CreditCard):
    __slots__ = '_apr'

  # 2、名称解析和动态调度
  # 名称解析：用点运算符语法访问现有成员
  # 1、在实例命名空间中搜索
  # 2、否则在该实例所属的类的命名空间中搜索
  # 3、通过继承层次结构向上，检查每一个父类的类命名空间（通常检查超类、超类的超类等等）
  # 4、如果还没有找到该名称，就会引发一个 AttrbuteError 异常

  # 动态调度：在编译时基于变量声明的类型来决定调度函数的版本
  ```

##### 深拷贝和浅拷贝

  ```
  # 创建别名 foo
  foo = bar

  # 拷贝对象的一个副本（比如列表），其内容与原来的序列相同
  # 浅拷贝：显式调用构造函数
  palette = list(warmtones)

  # 深拷贝：新副本引用的对象也是从原始版本中复制过来的
  palette = copy.deepcopy(warmtones)
  ```

#### 第 3 章 算法分析

* 运行时间
* 空间利用

  ##### 执行时间的实验研究

  ```
  # 算法执行花费时间依赖于在作业执行时正运行的其他进程
  from time import time
  start_time = time()                   # record the starting time
  run algorithm
  end_time = time()                     # record the ending time
  elapsed = end_time - start_time       # compute the elspesd time

  # 一个更公正的度量是算法使用的 CPU 周期的数量
  # time 模块的 click() 函数，它的粒度依赖于计算机系统
  # timeit 模块可以自动地做多次重复实验来评估差异
  ```

  局限性：
  * 很难比较两个算法的实验运行时间，除非在相同的硬件和软件环境中执行
  * 只有在有限的一组测试输入下才能完成，忽略了输入的运行时间
  * 为了在实验上执行算法来研究它的执行时间，算法必须完全实现，在算法设计初期进行十分不明智

  目标：开发一种分析算法效率的方法
  * 在软硬件环境独立的情况下，在某种程度上允许评价任意两个算法的相对效率
  * 通过研究不需要实现的高层次算法描述来执行算法
  * 考虑所有可能的输入

  原子操作：
  * 给对象指定一个标识符
  * 确定与这个标识符相关联的对象
  * 执行算术计算
  * 比较两个数的大小
  * 通过索引访问列表的一个元素
  * 调用函数（不包括函数内的执行操作）
  * 从函数返回
  算法执行的原子操作数 t 与算法的真实运行时间成正比

  随着输入函数的变化进行测量操作（n 为输入大小）
  * t = f(n)

  最坏情况输入的研究
  * 最好情况运行时间
  * 最坏情况运行时间
  * 平均情况运行时间在两者之间

  ##### 本书使用的 7 种函数

  ```
  # 1、常数函数
  # 描述了在计算机上需要做的基本操作的步数，例如两个数相加、变量赋值或者比较两个数的大小
  f(n) = c
  g(n) = 1                    # 最基本的常数函数
  f(n) = cg(n)                # 可以改写为这种形式

  # 2、对数函数（计算机存储整数采用二进制，默认底数为 2）
  # 许多算法的常见操作是反复把一个输入分成两半
  x = log n
  log n = log n/log 2         # 不同底数转换
  log 2n = log 2 + log n =  1 + log n
  log n/2 = log n - log 2 = log n - 1
  log n^3 = 3log n
  log 2^n = n log 2 = n * 1 = n
  2^log n = n^log 2 = n^1 = n

  # 3、线性函数
  # 用任何算法处理不在计算机内存中的 n 个对象的最快运行时间，因为已经需要 n 次操作了
  f(n) = n

  # 4、n log n 函数
  # 这个函数的增长速度比线性函数块，比二次函数慢
  # 对 n 个任意数进行排序且运行时间与 n log n 成比例的最快可能算法
  f(n)=  n log n

  # 5、二次函数
  # 许多函数中都有嵌套循环，其中内层循环执行一个线性操作数，外层循环则表示执行线性操作数的次数
  f(n) = n^2

  # 嵌套循环中第一次循环迭代的操作数为1，第二次为2，第三次为3，即总操作数计算如下
  # 但增长的阶数仍然是 n 的平方
  1+2+3+...+(n-2)+(n-1)+n = n(n+1)/2

  # 6、三次函数和其他多项式
  # 多项式求和（略）
  f(n) = n^3

  # 7、指数函数
  # 考虑到对数函数的情况，在算法分析中，指数函数最基本的情况是 b = 2
  f(n) = b^n

  # 2^n-1 是在二进制表示法中使用 n 位可以表示的最大整数
  1+2+4+8+...+2^n-1 = 2^n - 1
  
  # 8、比较增长率
  # 以上 7 中函数的增长率比较如下（从大到小）
  # 指数函数、三次函数、二次函数、n log n函数、线性函数、对数函数、常数函数
  
  # 向下取整和向上取整函数
  ⌈x⌉ = 小于或等于 x 的最大整数
  ⌊x⌋ = 大于或等于 x 的最小整数
  ```

  ##### 渐进分析

  * 在算法分析中，我们重点研究运行时间的增长率，采用宏观方法把运行时间视为输入大小为 n 的函数。
  * 通常只要知道算法的运行时间为按比例增长到 n 就足够了
  
  ```
  # 返回列表最大值的函数
  def find_max(data):
    """ Return the maximum element from a nonempty Python list. """
    biggest = data[0]
    for val in data:
      if val > biggest:
        biggest = val
    return biggest
  ```

  ###### 大 O 符号

  定义：
  * 令 f(n) 和 g(n) 作为正实数映射正整数的函数
  * 如果有实型常量 c>0 和整型常量 n0≥1 满足 f(n)≤cg(n)，当 n≥n0
  * 我们就说 f(n) 是 O(g(n))，或者可以说 f(n) 是 g(n) 的量级，用数学语言表达为 f(n) ∈ g(n)
  * 含义：当给定一个常数因子且在渐进意义上 n 趋于无穷时，函数 f(n) ≤ 函数 g(n)

  相关命题：
  * 最大数算法的运行时间为 O(n)
  * 8n+5 是 O(n)
  * 5n^4 + 3n^3 + 2n^2 + 4n + 1 是 O(n^4)
  * 5n^2 + 3n log n + 2n + 5 是 O(n^2)
  * 20n^3 + 10n log n + 5 是 O(n^2)
  * 3 log n + 2 是 O(log n)
  * 2^(n+2) 是 O(2^n)
  * 2n + 100 log n 是 O(n)

  ###### 大 Ω 符号

  定义：
  * 正如大 O 提供一种渐进说法：一个函数的增长速率“小于或等于”另一个函数
  * 大 Ω 提供了另一种渐进说法：一个函数的增长速率“大于或等于”另一个函数
  * 3n log n - 2n 是 Ω(n log n)

  ###### 大 Θ 符号

  定义：
  * 当给定一个常数因子时，两个函数的增长速率相同
  * 3n log n + 4n + 5 log n 是 Θ(n log n)

  ###### 比较分析

  * 假设有两个算法都能解决同一个问题，算法 A 运行时间 O(n)，算法 B 运行时间 O(n^2)
  * 算法 A 比算法 B 更具有渐进性（虽然当 n 的值比较小时，后者运行时间可能更短）

  ###### 算法分析示例

  ```
  # 1、常量时间操作
  data = list()
  len(data)         # O(1)，每一个列表都包含一个记录当前长度的实例变量，函数的运行时间独立于列表长度
  data[j]           # O(1)，无需迭代列表，而是直接将该索引作为底层数组的偏移量达到的

  # 2、回顾在序列中找最大值算法
  find_max(data)    # O(n)，初始化、赋值和返回语句运行时间都为O(1)，该循环执行 n 次

  # 3、进一步分析找最大值算法
  # 但是在循环的每一次迭代中，只有当前元素比以往所有元素都更大时才会更新当前最大值
  # 如果给出的是随机序列，则第j个元素比前j个元素都更大的的概率是1/j（假定元素唯一）
  # 因此，我们更新最大值（包括初始化）的预期次数是 Hn=nΣj=1 1/j（著名的n调和数）
  # Hn的运行时间是 O(log n)，也就是该算法最大值被更新的预期次数

  # 4、前缀平均值
  # 给出一个包含 n 个数的序列 S，想得到序列 A，满足的条件为：当 j=0..n-1 时，A[j]是 S[0..j] 的平均值
  # 例如，给出一个公共资金的每年收益，投资者往往最关注一年、三年或者五年等的年平均收益
  # 同样，给出一连串的日常网络使用日志，网站管理员可能希望能追踪不同时期的平均使用趋势
  # 以下给出三种能用于解决这些问题的方法

  # 5、二次时间算法
  # 运行时间为 O(n^2)
  def prefix_average1(S):
    """ Return list such that, for all j, A[j] equals average of S[0]...S[j]. """
    n = len(S)
    A = [0] * n                         # O(n) create new list of n zeros
    for j in range(n):                  # O(n)
      total = 0
      for i in range(j+1):              # O(n^2)
        total += S[i]                   # equals to 1+2+3+...+n = n(n-1)/2
      A[j] =  total / (j+1)             # record the average
    return A

  # 运行时间为 O(n^2)
  def prefix_average2(S):
    """ Return list such that, for all j, A[j] equals average of S[0]...S[j]. """
    n = len(S)
    A = [0] * n                         # O(n) create new list of n zeros
    for j in range(n):
      A[j] =  sum(S[0:j+1]) / (j+1)     # record the average

  # 6、线性时间算法
  # 在前两个算法中，对于每一个 j，都要对前缀和重新进行计算，每一个 j 都需要 O(j) 的运行时间，从而导致该算法运行时间变为二次
  # 在以下算法中，动态保存当前的前缀和，只有一个 for 循环，用计数器 j 来约束
  # 运行时间为 O(n)
  def prefix_average3(S):
    """ Return list such that, for all j, A[j] equals average of S[0]...S[j]. """
    n = len(S)                          # O(1)
    A = [0] * n                         # O(n) create new list of n zeros
    total = 0                           # O(1)
    for j in range(n):                  # O(n) loop
      total += S[j]
      A[j] = total / (j+1)
    return A

  # 7、三集不相交
  # 给出三个序列，假定任意序列没有重复值，但不同序列间可以重复，确定三个序列的交集是否为空
  # 假设最初序列每一个长度都为 n，在最坏情况下（序列内的每一个元素都要比较），该函数的运行时间为 O(n^3)
  def disjoint1(A, B, C):
    """ Retrurn True if there is no lement common to all three lists. """
    for a in A:
      for b in B:
        for c in C:
          if a == b == c:
            return False
    return True

  # 在最坏情况下（序列 A 和 B 内的每个元素都相等），该函数的运行时间为 O(n^2)
  # 在最好情况下（序列 A 和 B 内的每个元素都不相等），该函数的运行时间为 O(n)
  # 该函数的总运行时间为 O(n^2)
  def disjoint2(A, B, C):
    """ Retrurn True if there is no lement common to all three lists. """
    for a in A:
      for b in B:
        if a == b:
          for c in C:
            if a == c:
              return False
    return True
  
  # 8、元素唯一性
  # 在最坏情况下，该函数的运行时间按比例增长到 (n-1)+(n-2)+...+2+1
  # 该函数的总运行时间为 O(n^2)
  def unique1(S):
    """ Return True if there are no duplicate elemenets in sequence S. """
    for j in range(len(S)):
      for k in range(j+1, len(S)):
        if S[j] == S[k]:
          return False
    return True
  
  # 9、以排序作为解决问题的工具：解决元素唯一性问题更优的一个算法
  # 通过对序列的元素进行排序，为了确定是否有重复值，需要遍历该排序的序列，查看是否有连续的重复值
  # 对序列排序后，可以保证在最坏情况下，该函数的运行时间为 O(n log n)
  # 该函数的总运行时间为 O(n log n)
  def unique2(S):
    """ Return True if there are no duplicate elemenets in sequence S. """
    temp = sorted(S)                    # create a sorted copy of S
    for j in range(1, len(temp)):       # O(n) loop
      if temp[j-1] == temp[j]:
        return False                    # found duplicate pair
    return True                         # if we reach this, elements were unique
  ```

  ##### 简单的证明技术

  * 找反例
  * 反证法（证明逆否命题）
  * 归纳和循环不变量

#### 第 4 章 递归

在本章中，将从以下四个递归使用例证开始讲解，并给出了每个例证的实现：

* 阶乘函数 n! 是一个经典的数学函数，它有一个固有的递归定义
* 英式标尺具有的递归模式是分形结构的一个简单例子
* 二分查找是最重要的计算机算法之一，在一个拥有数十亿以上条目的数据集中，它能有效定位所需的某个值
* 计算机的文件系统有一个递归结构，在该结构中，目录能够以任意深度嵌套在其他目录上，递归算法被广泛用于探索和管理这些文件结构

  ##### 说明性的例子

  ```
  # 1、阶乘函数
  # 函数中不适用任何显式循环，迭代是通过函数的重复递归调用来实现的，每被调用一次参数就会变小一次，直到停止
  # 递归跟踪：记录递归函数的执行过程，对于每个有效调用都有一个不同的活动记录，包含用来存储函数调用的参数和局部变量的命名空间
  # 如果一个函数的执行导致嵌套函数的调用，前者调用的执行将被挂起，其活动记录将存储源代码中的位置（被调用函数返回后继续执行的控制流）
  # 时间复杂度 O(n)，因为有 n+1 次函数的调用，所以每次调用占的操作次数为 O(1)
  def factorial(n):
    if n == 0:
      return 1
    else:
      return n * factorial(n-1)
  
  # 2、绘制英式标尺
  # 英式标尺模式是分型的一个简单示例，也就是具有在各级放大的自递归结构的形状
  def draw_line(tick_length, tick_label=''):
    """ Draw one lie with given tick length (followed by optional label). """
    line = '-' * tick_length
    if tick_label:
      line += ' ' + tick_label
    print(line)

  def draw_interval(center_length):
    """ Draw tick interval based upon a central tick length. """
    if center_length > 0:
      draw_interval(center_length - 1)
      draw_line(center_length)
      draw_interval(center_length - 1)
  
  def draw_ruler(num_inches, major_length):
    """ Draw English ruler with given number of inches, major tick length. """
    draw_line(major_length, '0')
    for j in range(1, 1 + num_inches):
      draw_interval(major_length - 1)
      draw_line(major_length, str(j))

  # 3、二分查找
  # 用于在有序序列中有效定位目标值，这是最重要的计算机算法之一，也是我们经常顺序存储数据的原因
  # 序列无序：使用循环来检查每一个元素（顺序查找算法），在最坏情况下每个元素都需要检查，时间复杂度 O(n)
  # 序列有序：使用二分查找算法，比较目标值和中间值的大小，重复此过程直到找到目标值，时间复杂度 O(log n)
  def binary_search(data, target, low, high):
    """ 
    Return True if target is found in indicated portion of a Python list. 
    The search only considers the portion from data[low] to data[high] inclusive.
    """
    if low > high:
      return False                      # interval is empty; no match
    else:
      mid = (low + high) // 2
      if target == data[mid]:
        return True                     # found a match
      elif target < data[mid]:
        # recue on the portion left of the middle
        return binary_search(data, target, low, mid-1)
      else:
        # recue on the portion right of the middle
        return binary_search(data, target, mid+1, high)

  # 4、文件系统
  # 现代操作系统用递归的方式来定义文件系统目录（文件夹），操作系统允许嵌套任意深度的目录
  # 计算嵌套在一个特定目录中的所有文件和目录的总磁盘使用情况的伪代码
  Allorithm DiskUsage(path):
    Input: A string designating a path to a file-system entry
    Output: The cumulative disk space used by that entry and nested entries
    total = size(path)                  # {immediate disk space used by the entry}
    if path represents a directory then 
      for each child entry stored within directory path do
        total = total + DiskUsage(child)
    return total

  # Python 的操作系统模块
  os.path.getsize(path)                 # 返回由字符串路径标识的文件或者目录使用的即时空间大小（字节）
  os.path.isdir(path)                   # 如果字符串路径指定的条目是一个目录则返回 True 否则 False
  os.listdir(path)                      # 返回一个字符串列表，它是字符串路径指定目录中所有条目的名称
  os.path.join(path, filename)          # 生成路径字符串和文件名字符串，并使用当前操作系统分隔符分隔

  # Python 实例
  # 时间复杂度 O(n)
  import os
  def disk_usage(path):
    """ Return the number of bytes used by a file/folder and any descendents. """
    total = os.path.getsize(path)
    if os.path.isdir(path):
      for filename in os.listdir(path):
        childpath = os.path.join(path, filename)
        total += disk_usage(childpath)
    print('{0:<7}'.format(total), path)
    return total                        # 完成该路径下的所有路径的递归调用，再返回累计磁盘空间大小
  ```

  ##### 递归的其余例子

  ```
  # 1、线性递归
  # 如果一个递归函数主体的每个调用至多执行一个新的递归调用，就称为线性递归（任何递归追踪均为单一调用序列）
  # 例如二分查找也是线性递归的一个例子，两个分支中均产生递归调用，但在一次执行期间只有其中一个调用被执行

  # 元素序列的递归求和，时间复杂度 O(n)
  def linear_sum(S, n):
    """ Return the sum of the first n numbers of sequence S. """
    if n == 0:
      return 0
    else:
      return linear_sum(S, n-1) + S[n-1]

  # 使用线性递归逆置（翻转）序列，时间复杂度 O(n)
  def reverse(S, start, stop):
    """ Reverse elements in implicit slice S[start:stop]. """
    if start < stop - 1:
      S[start], S[stop-1] = S[stop-1], S[start]
      reverse(S, start+1, stop-1)

  # 用于计算幂的递归算法，时间复杂度 O(n)，递归深度 O(n)，激活记录同时被存储在内存中
  def power(x, n):
    """ Compute the value x**n for integer n. """
    if n == 0:
      return 1
    else:
      return x * power(x, n-1)

  # 使用重复的平方计算幂函数，时间复杂度 O(log n)，递归深度 O(log n)，所用内存 O(log n)
  # 当 n 为奇数时，x^n=x*power(x,n//2)^2；当 n 为偶数时，x^n=power(x,n//2)^2
  def power(x, n):
    """ Compute the value x**n for integer n. """
    if n == 0: 
      return 1
    else:
      partial = power(x, n//2)
      result = partial * partial
      if n % 2 == 1:
        result *= x
      return result

  # 2、二路递归（一个函数执行两个递归调用，例如上例绘制英式标尺）
  # 在有两个或者更多元素的情况下，可以递归地计算前一半元素的总和和后一半元素的综合，然后把两部分加起来

  # 用二路递归计算一个序列的元素之和，空间复杂度 O(log n)
  def binary_sum(S, start, stop):
    """ Return the sum of the numbers in implicit slice S[start:stop]. """
    if start >= stop:
      return 0
    elif start == stop - 1:
      return S[start]
    else:
      mid = (start + stop) // 2
      return binary_sum(S, start, mid) + binary_sum(S, mid, stop)

  # 3、多重递归（一个函数执行两个以上递归调用，例如上例计算文件磁盘空间）
  # 通过枚举和测试所有可能的配置来解决组合谜题
  Algorithm PuzzleSolve(k, S, U):
    Input: An integer k, sequence S, and set U
    Output: An enumeration of all k-length extensions to S using elements in U
      without repetitions
    for each e in U do
      Add e to the end of S
      Remove e from U                   # {e is now being used}
      if k == 1 then 
        Test whether S is a configuration that solves the puzzle
        if S solves the puzzle then 
          return "Solution found: " S
        else
          PuzzleSolve(k-1, S, U)        # {a recursive call}
        Remove e from the end of S 
        Add e back to U                 # {e is now considered as unused}
  ```
  
  ##### 设计递归算法

  一般来说，使用递归的算法通常具有以下形式：
  * 对于基本情况的测试
    首先测试一组基本情况（至少应该有一个）。
    这些基本情况应该被定义，以便每个可能的递归调用链最终会达到一种基本情况，并且每个基本情况不应使用递归。
  * 递归
    如果不是一种基本情况，则执行一个或多个递归调用。
    这个递归步骤可能包括一个测试，该测试决定执行哪几种可能的递归调用。
    应该定义每个可能的递归调用，以便使调用向一种基本情况靠近。

  参数化递归：
  * 考虑可以定义的子问题的不同方式，该子问题与原始问题有着相同的总体结构
  * 一个成功的递归设计有时需要重新定义原来的问题，以便找到看起来相似的子问题（参数化函数的特征码）
  * 创建一个有简介接口的公共函数，然后让它的函数体调用一个非公共的效用函数（含有所希望的递归参数）

  ##### 消除尾递归

  算法设计的递归方法：
  * 主要优点：能够简洁地利用重复结构呈现诸多问题
  * 递归的可用性要基于合适的成本
  * 通过管理递归结构自身的嵌套，而不是依赖于解释器，从而把递归算法转换成非递归算法
  * 更好的情况是，递归的某些形式可以在不适用任何辅助存储空间的情况下被消除（尾递归）

  尾递归：
  * 执行的任何递归调用是在这种情况下的最后操作
  * 通过封闭递归，递归调用的返回值（如果有的话）立即返回
  * 一个尾递归必须是线性递归（因为如果必须立即返回第一个递归调用的结果，将无法进行第二次递归调用）
  * binary_search 和 reverse 函数均是尾递归的例子，虽然其他几个线性递归很想尾递归，但技术上并不如此
  * 在重复循环中，通过封闭函数体并重新分配现存参数以及用新的参数来代替一个递归调用，任何尾递归都可以被非递归地重新实现
  * 即使许多其他线性递归不是正式的尾递归，它们也可以非常有效地使用迭代来表达

  ```
  # 二分查找算法（非递归实现）
  def binary_search_iterative(data, target):
    """ Return True if target is found in the given Python list. """
    low = 0
    high = len(data) - 1
    while low <= high:
      mid = (low + high) // 2
      if target == data[mid]:
        return True
      elif target < data[mid]:
        high = mid - 1                  # only changes this line
      else:
        low = mid + 1
    return False

  # 使用迭代逆置一个序列的元素（非递归实现）
  def reverse_iterative(S):
    """ Reverse elements in sequence S. """
    start, stop = 0, len(S)
    while start < stop - 1:
      S[start], S[stop-1] = S[stop-1], S[start]
      start, stop = start + 1, stop - 1
  ```

#### 第 5 章 基于数组的序列

  ##### Python 序列类型

  Python 的各种“序列类”：
  * 列表类（list）
  * 元组类（tuple）
  * 字符串类（str）
  
  共同点：
  * 每个类都支持用下标访问序列元素，比如 seq[k]
  * 每个类都使用数组这种低层次概念表示序列

  ##### 低层次数组

  计算机体系结构的低层次内容：
  * 计算机主存由位信息组成，一个典型的单元就是一个字节，相当于8位
  * 存储地址：跟踪信息存储在哪个字节，每个存储字节都和一个作为其地址的唯一数字相关联（二进制）
  * 存储地址通常和存储系统的物理设计相协调，通常以顺序方式描述这些数字，例如字节#2157
  * 基于这种设计，主存中的任何字节都能被有效访问，计算机主存：随机存储存储器（Random Access Memory,RAM）
  * 使用渐进分析的符号，认为存储器的任一单个字节被存储或检索的运行时间为 O(1)

  数组：
  * 一般来说，变成语言记录标识符和其关联至所存储的地址之间的联系
  * 一组相关变量能够一个接一个地存储在计算机存储器的一块连续区域内，称为数组
  * 数组中的每个位置称为单元，并用整数索引值描述该数组，单元的开始编号为0
  * 数组的每个单元必须占据相同数量的字节，通过计算 start+cellsize*index 便可得出其正确的内存地址

  引用数组：
  * 例如一个字符串数组，每个元素的长度不一致，如果用最长字符串来为每个元素预留足够的空间，太浪费了
  * Python 使用数组内部存储机制（即对象引用）来表示一列或者元组实例
  * 在最底层，存储的是一块连续的内存地址序列，这些地址指向一组元素序列
  * 虽然单个元素的相对大小可能不同，但每个元素存储地址的位数是固定的
  * 在这种方式下，可以通过索引值以常量时间访问元素列表或元组
  * 也就是说，列表和元组是引用结构，列表仅仅存储返回对象的引用
  * 一个列表实例可能会以多个指向同一个对象的引用作为列表元素，一个对象也可能被多个列表元素所指向

  浅拷贝与深拷贝：
  * 浅拷贝：backup = list(primes) 对原列表复制出一个新列表，新列表即为浅拷贝，和原列表指向同样的元素（列表元素不可变时）
  * 深拷贝：backup = deepcopy(primes) 得到一个具有全新元素的新列表（列表元素可变时）

  列表引用性质：
  * backup = extend(primes) 将一个列表的所有元素添加到另一个列表的末尾
  * 扩展列表的过程不是将元素复制过来，而是将元素的引用复制到末尾

  紧凑数组：
  * 字符串是用字符数组表示的（而不是数组的引用）
  * 更直接的表示方式：紧凑数组，因为数组存储的是位，这些位表示原始数据（在字符串情况下，这些位即是字符）
  * 使用紧凑结构会占用更少的内存，因为在内存引用序列的显示存储上没有开销（原始数据除外）
  * 引用结构通常会将 64 位地址存入数组，无论存储单个元素的对象有多少位
  * 字符串中的每个 Unicode 字节存储在紧凑数组中仅需要两个字节
  * 紧凑结构在高性能计算方面的另一个重要优势是，原始数据在内存中是连续存放的

  ##### 动态数组和摊销

  * 在计算机系统中，创建低层次数组时，必须明确声明数组的大小，以便系统为其存储分配连续的内存
  * 由于系统可能会占用相邻的内存位置去存储其他数据，因此数组大小不能靠扩展内存单元来无限增加
  * 虽然列表在被构建时已经有确定的长度，但该类允许对列表增添元素，依赖一种算法技巧，即动态数组
  * 一个列表通常关联着一个底层数组，并且长度比列表的长度更长，通过利用数组的下一个可用单元，很容易增添列表元素
  * 假如所有预留单元被耗尽，列表会请求一个新的更大的数组，并初始化该数组，使其前面部分能和原来数组一样，旧的数组被回收

  ```
  # 列表长度和底层大小关系
  import sys
  data = []
  for k in range(n):
    a = len(data)
    b = sys.getsizeof(data)
    print('Length: {0:3d}; Size in bytes: {1:4d}'.format(a,b))
    data.append(None)

  # 测量列表类增添操作的摊销话费花费 
  from time import time
  def compute_average(n):
    "" Perform n appends to an empty list and return average time elapsed. """
    data = []
    start = time()
    for k in range(n):
      data.append(None)
    end = tiem()
    return (end - start) / n
  ```

  ##### 使用基于数组的序列

  ```
  # 表示游戏条目
  class GameEntry:
    """ Represents one entry of a list of high scores. """

    def __init__(self, name, score):
      self._name = name
      self._score = score

    def get_name(self):
      return self._name

    def get_score(self):
      return self._score

    def __str__(self):
      return '({0}. {1}).'format(self._name, self._score)

  # 存储高分
  class Sccoreboard:
    """ Fixed-length sequence of high scores in nondecreasing order. ""

    def __init__(self, capacity=10):
      """ Initialize scoreboard with given maximum capacity. All entries are initially None. """

      self._board = [None] * capacity
      self._n = 0

    def __getitem__(self, k):
      """ Return entry at index k. """
      return self._board[k]

    def __str__(self):
      """ Return string representation of the high score list. """
      return '\n'.join(str(self._board[j]) for j in range(self._n))

    def add(self, entry):
      """ Consider adding entry to high scores. """
      score = entry.get_score()

      # Does new entry qualify as a high score?
      # answer is yes if board not full of score is higher than last entry
      good = self._n < len(self._board) or score > self._board[-1].get_score()
      if good:
        if self._n < len(self._board):            # no score drops from list
          self._n += 1                            # so overall number increases
        
        # shift lower scores rightward to make room for new entry
        j = self._n - 1
        while j > 0 and self._board[j-1].get_score() < score:
          self._board[j] = self._board[j-1]       # shift entry from j-1 to j
          j -= 1                                  # and decrement j
        self._board[j] = entry                    # when done, add new entry

  # 插入排序
  def insertion_sort(A):
    """ Sort list of comparable elements into nondecreasing order. """
    for k in range(1, len(A)):
      cur = A[k]                                  # current element to be inserted
      j = k                                       # find correct index j for current
      while j > 0 and A[j-1] > cur:               # element A[j-1] must be after current
        A[j] = A[j-1]
        j -= 1
      A[j] = cur                                  # cur is now in the right place

  # 简单密码技术：凯撒密码（用固定数目后的字母进行替换）
  class CaesarCipher:
    """ Class for doing encryption and decryption using a Caesar cipher. """

    def __init__(self, shift):
      """ Construct Caesar cipher using given integer shift for rotation. """
      encoder = [None] * 26                       # temp array for encryption
      decoder = [None] * 26                       # temp array for decryption
      for k in range(26):
        encoder[k] = chr((k + shift) % 26 + ord('A'))
        decoder[k] = chr((k - shift) % 26 + ord('A'))
      self._forward = ''.join(encoder)
      self._backward = ''.join(decoder)

    def encrypt(self, message):
      """ Return string representing encrypted message. """
      return self._transform(message, self._forward)

    def decrypt(self, secret):
      """ Return decrypted message given encrypted secret. """
      return self._transform(secret, self._backward)

    def transform(self, original, code):   
      """ Utility to perform transformation based on given code string. """
      msg = list(original)
      for k in range(len(msg)):
        if msg[k].isupper():
          j = ord(msg[k]) - ord('A')
          msg[k] = code[j]
      return ''.join(msg)

    if __name__ == '__main__':
      cipher = CaesarCipher(3)
      message = "THE EAGLE IS IN PLAY; MEET AT JOE'S."
      coded = cipher.encrypt(message)
      print('Secret:', coded)
      answer = cipher.decrypt(coded)
      print('Message:', answer)
  ```

  ##### 多维数据集

  ```
  # 实例化二维数组，确保原始列表的每个单元都能指向一个独立的二级列表
  data = [ [0] * c for j in range(r) ]
  ```

#### 第 6 章 栈、队列和双端队列

  ##### 栈

  栈的定义：
  * 由一系列对象组成的一个集合，这些对象的插入和删除操作遵循先进后出的原则（LIFO）
  * 可以在任何时刻向栈中插入一个对象，但只能取得或者删除最后一个插入的对象（栈顶）
  * 浏览器将最近浏览的网址存放在一个栈中，新访问的网址被压入栈顶，用户点击后退按钮就弹出最近访问的地址
  * 文本编辑器的撤销功能就是将通过文本的变化状态保存在一个栈中得以实现的

  栈的抽象数据类型（ADT）：
  * S.push(e)：将元素 e 添加到栈 S 的栈顶
  * S.pop(e)：从栈 S 中移除并且返回栈顶的元素（若栈是空的，将返回报错）
  * S.top()：在不移除栈顶元素的前提下，返回栈顶元素（若栈是空的，将返回报错）
  * S.is_empty()：如果占中不包含任何元素，则返回 True
  * len(S)：返回栈中元素的数量

  ```
  # 用 Python 的 list 类实现一个栈
  # 1、Empty 异常类定义
  class Empty(Exception):
    """ Error attempting to access an element from an empty container. """
    pass

  # 2、栈实现
  class ArrayStack:
    """ LIFO Stack implementation using a Python list as underlying storage. """

    def __init__(self):
      """ Create an empty stack. """
      self.data = []                    # nonpublic list instance

    def __len__(self):
      """ Return the number of elements in the stack. """
      return len(self._data)

    def is_empty(self):
      """ Return True if the stack is empty. """
      return len(self._data) == 0

    def push(self, e):
      """ Add element e to the top of the stack. """
      self._data.append(e)              # new item stored at end of list

    def top(self):
      """ Return (but do not remove) the element at the top of the stack. 
          Raise Empty exception if the stack is empty. 
      """
      if self.is_emppty():
        raise Empty('stack is empty')
      return self._data[-1]             # the last item in the list

    def pop(self):
      """ Remove and return the element from the top of the stack (i.e. LIFO). 
          Raise Empty exception if the stack is empty. 
      """
      if self.empty()"
        raise Empty('Stack is empty')
      return self._data.pop()           # remove last item from list

    # 作为一个栈的替代模型，可能希望构造函数接受一个用于指定堆栈最大容量的参数，并初始化数据成员列表长度
    # 栈的长度不再是列表长度的同义词，并且对栈的 push 和 pop 也不再需要改变列表长度
    # 建议单独维护一个整数作为实例变量，以表示当前栈中元素的个数

    # 3、使用栈实现文件内容的逆置
    # 先替换换行符，再重新插入换行符：是为了处理一种特殊情况，在原始文件的最后一行没有换行符
    def reverse_file(filename):
      """ Overwrite given file with its contents line-by-line reversed. """
      S = ArrayStack()
      original = open(filename)
      for line in original:
        S.push(line.rstrip('\n'))
      original.close()

      output = open(filename,'w')
      while not S.is_empty():
        output.write(S.pop() + '\n')
      output.close()

    # 4、在算术表达式中的分隔符匹配算法
    def is_matched(expr):
      """ Return True if all delimiters are properly match: False otherwise. """
      lefty = '({['
      righty = ')}]'
      S = ArrayStack()
      for c in expr:
        if c in lefty:
          S.push(c)
        elif c in righty:
          if S.is_empty():
            return False
          if righty.index(c) != lefty.index(S.pop()):
            return False
      return S.is_empty()

    # 5、HTML 的标签匹配
    def is_matched_html(raw):
      """ Return True if all HTML tags are properly match; False otherwise. """
      S = ArrayStack()
      j = raw.find('<')
      while j != -1:
        k = raw.find('>', j+1)
        if k == -1:
          return False
        tag = raw[j+1:k]
        if not tag.startswith('/)       # this is opening tag
          S.push(tag)
        else:
          if S.is_empty():
            return False
          if tag[1:] != S.pop()
            return False
        j = raw.find('<', k+1)
      return S.is_empty()
  ```

  ##### 队列

  队列的定义：
  * 由一系列元素组成的集合，这些对象的插入和删除遵循先进先出的原则（FIFO）
  * 元素可以在任何时刻进行插入，但是只有处在队列最前面的元素才能被删除
  * 队列中允许插入的一端称为队尾，允许删除的一端称为队头（排队进入游乐场）

  队列的抽象数据类型（ADT）：
  * Q.enqueue(e)：向队列 Q 的队尾添加一个元素