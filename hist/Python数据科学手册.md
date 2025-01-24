### 第2章 Numpy 入门

##### Python 数据类型

Python 的用户往往被其易用性所吸引，其中一个易用之处就在于动态输入。静态类型的语言（如 C 或 Java）往往需要明确地声明每一个变量的数据类型，而动态类型的语言（例如 Python）可以跳过这个特殊规定，类型是动态推断的，这意味着可以将任何类型的数据指定给任何变量。

标准的 Python 实现是用 C 语言编写的。这意味着每一个 Python 对象都是一个聪明的伪 C 语言结构体，该结构体不仅包含其值，还有其他信息。这意味着与 C 语言这样的编译语言中的整型相比，在 Python 中存储一个整型会有一些开销。

两者的差异在于，C 语言整型本质上是对应某个内存位置的标签，里面存储的字节会编码成整型。而 Python 的整型其实是一个指针，指向包含这个 Python 对象所有信息的某个内存位置，其中包括可以转换成整型的字节。由于 Python 的整型结构体里面还包含了大量额外的信息，所以 Python 可以自由、动态地编码。但是，Python 类型中的这些额外信息也会成为负担，在多个对象组合的结构体中尤其明显。

```
# Python 列表
L1 = list(range(10)) # 数字列表
L2 = [str(c) for c in L1] # 对应的字符串列表
L3 = [True, "2", 3.0, 4] # 异构列表
[type(item) for item in L3] # 输出异构列表的类型 [bool, str, float, int]
```

但是想拥有这种灵活性也是要付出一定代价的：为了获得这些灵活的类型，列表中的每一项必须包含各自的类型信息、引用计数和其他信息；也就是说，每一项都是一个完整的 Python 对象。来看一个特殊的例子，如果列表中的所有变量都是同一类型的，那么很多信息都会显得多余——将数据存储在固定类型的数组中应该会更高效。动态类型的列表和固定类型的（NumPy 式）数组间的区别如图 2-2 所示。

![2-2](https://upload-images.jianshu.io/upload_images/4311027-562bc37de3fbc2c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在实现层面，数组基本上包含一个指向连续数据块的指针。另一方面，Python 列表包含一个指向指针块的指针，这其中的每一个指针对应一个完整的 Python 对象（如前面看到的 Python 整型）。另外，列表的优势是灵活，因为每个列表元素是一个包含数据和类型信息的完整结构体，而且列表可以用任意类型的数据填充。固定类型的 NumPy 式数组缺乏这种灵活性，但是能更有效地存储和操作数据。


##### 创建数组

```
# 使用 Python 内置的 array 创建统一类型的密集数组
# 输出A：array('i', [0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
import array
L = list(range(10))
A = array.array('i', L) # 'i' 是一个数据类型码，表示数据为整型

# 使用 numpy 从 Python 列表创建数组
import numpy as np
np.array([1, 4, 2, 5, 3]) # 创建整型数组

# 不同于 Python 列表，NumPy 要求数组必须包含同一类型的数据。如果类型不匹配，NumPy 将会向上转换（如果可行）
np.array([3.14, 4, 2, 3]) # 这里整型被转换为浮点型

# 如果希望明确设置数组的数据类型，可以用 dtype 关键字
np.array([1, 2, 3, 4], dtype='float32')

# 不同于 Python 列表，NumPy 数组可以被指定为多维的
# 输出：array([[2, 3, 4], [4, 5, 6], [6, 7, 8]])
np.array([range(i, i + 3) for i in [2, 4, 6]]) # 嵌套列表构成的多维数组，内层的列表被当作二维数组的行
```

面对大型数组的时候，用 NumPy 内置的方法从头创建数组是一种更高效的方法。

```
np.zeros(10, dtype=int) # 创建一个长度为10的数组，数组的值都是0
np.ones((3,5), dtype=float) # 创建一个3×5的浮点型数组，数组的值都是1
np.full((3,5), 3.14) # 创建一个3×5的浮点型数组，数组的值都是3.14
np.arange(0, 20, 2) # 创建一个3×5的浮点型数组，数组的值是一个线性序列（从0开始，到20结束，步长为2，和内置的range()函数类似）
np.linspace(0, 1, 5) # 创建一个5个元素的数组，这5个数均匀地分配到0~1
np.random.random((3, 3)) # 创建一个3×3的、在0~1均匀分布的随机数组成的数组
np.random.normal(0, 1, (3, 3)) # 创建一个3×3的、均值为0、方差为1的正态分布的随机数数组
np.random.randint(0, 10, (3, 3)) # 创建一个3×3的、[0, 10)区间的随机整型数组
np.eye(3) # 创建一个3×3的单位矩阵
np.empty(3) # 创建一个由3个整型数组成的未初始化的数组，数组的值是内存空间中的任意值
```


##### Numpy数据类型

```
np.zeros(10, dtype='int16')
np.zeros(10, dtype=np.int16) # 使用numpy对象指定数据类型
```

 | 数据类型 | 描述 |
 | ------- | ---- | 
 | bool_   | 布尔值（真、True 或假、False），用一个字节存储
 | int_    | 默认整型（类似于 C 语言中的 long，通常情况下是 int64 或 int32）
 | intc    | 同 C 语言的 int 相同（通常是 int32 或 int64）
 | intp    | 用作索引的整型（和 C 语言的 ssize_t 相同，通常情况下是 int32 或 int64）
 | int8    | 字节（byte，范围从–128 到 127）
 | int16   |  整型（范围从–32768 到 32767）
 | int32   |  整型（范围从–2147483648 到 2147483647）
 | int64   |  整型（范围从–9223372036854775808 到 9223372036854775807）
 | uint8   |  无符号整型（范围从 0 到 255）
 | uint16  | 无符号整型（范围从 0 到 65535）
 | uint32  | 无符号整型（范围从 0 到 4294967295）
 | uint64  | 无符号整型（范围从 0 到 18446744073709551615）
 | float_  | float64 的简化形式
 | float16 | 半精度浮点型：符号比特位，5 比特位指数（exponent），10 比特位尾数（mantissa）
 | float32 | 单精度浮点型：符号比特位，8 比特位指数，23 比特位尾数
 | float64 | 双精度浮点型：符号比特位，11 比特位指数，52 比特位尾数
 | complex_   | complex128 的简化形式
 | complex64  | 复数，由两个 32 位浮点数表示
 | complex128 | 复数，由两个 64 位浮点数表示


##### NumPy数组基础

```
import numpy as np
np.random.seed(0) # 设置随机数种子，以确保每次程序执行时都可以生成同样的随机数组
x1 = np.random.randint(10, size=6) # 一维数组
x2 = np.random.randint(10, size=(3, 4)) # 二维数组
x3 = np.random.randint(10, size=(3, 4, 5)) # 三维数组

# 数组属性
print("x3 ndim: ", x3.ndim) # nidm（数组的维度）
print("x3 shape:", x3.shape) # shape（数组每个维度的大小）
print("x3 size: ", x3.size) # size（数组的总大小）
print("dtype:", x3.dtype) # dtype（数据类型）
print("itemsize:", x3.itemsize, "bytes") # itemsize（每个数组元素字节大小）
print("nbytes:", x3.nbytes, "bytes") # nbytes（数组总字节大小）

# 数组索引
print(x1) # array([5, 0, 3, 3, 7, 9])
print(x1[0]) # 5
print(x1[-1]) # 倒数第一个元素

print(x2)
# array([[3, 5, 2, 4],
#        [7, 6, 8, 8],
#        [1, 6, 7, 7]])

print(x2[0,0]) # 在多维数组中，可以用逗号分隔的索引元组获取元素
# 和 Python 列表不同，NumPy 数组是固定类型的。这意味着当你试图将一个浮点值插入一个整型数组时，浮点值会被截短成整型
x1[0] = 3.14159 # 这将被截短
```


##### 数组切片：获取子数组

```
# 默认值 start=0、stop= 维度的大小（size of dimension）和 step=1
x[start:stop:step]

# 一维子数组
x = np.arange(10) # array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
x[:5] # 前五个元素
x[5:] # 索引五之后的元素
x[4:7] # 中间的子数组
x[::2] # 每隔一个元素
x[1::2] # 每隔一个元素，从索引1开始
x[::-1] # 所有元素，逆序的
x[5::-2] # 从索引5开始每隔一个元素逆序

# 多维子数组
print(x2)
# array([[12, 5, 2, 4],
#        [ 7, 6, 8, 8],
#        [ 1, 6, 7, 7]])

x2[:2, :3] # 两行，三列
x2[:3, ::2] # 所有行，每隔一列
x2[::-1, ::-1] # 逆序

# 获取数组的行和列
x2[:, 0] # x2的第一列
x2[0, :] # x2的第一行
x2[0] # 相当于x2[0, :]
```

NumPy 数组切片和 Python 列表切片的不同之处：Numpy 数组切片返回的是数组数据的视图，而不是数值数据的副本。在 Python 列表中，切片是值的副本。

它意味着在处理非常大的数据集时，可以获取或处理这些数据集的片段，而不用复制底层的数据缓存。

```
# 非副本视图的子数组
x2_sub = x2[:2, :2] 
# 现在如果修改这个子数组，将会看到原始数组也被修改了！
x2_sub[0, 0] = 99

# 创建数组的副本
x2_sub_copy = x2[:2, :2].copy()
# 如果修改这个子数组，原始的数组不会被改变
x2_sub_copy[0, 0] = 42
```


##### 数组变形

```
# 将数字 1~9 放入一个 3×3 的矩阵中
# 原始数组的大小必须和变形后数组的大小一致
# reshape 方法将会用到原始数组的一个非副本视图
grid = np.arange(1, 10).reshape((3, 3)) 

x = np.array([1, 2, 3])
# 通过变形获得的行向量
x.reshape((1, 3)) 
# 通过 newaxis 获得的行向量
x[np.newaxis, :]
# 以上变换结果均为 array([[1, 2, 3]])
```


##### 数组拼接

```
# 一维数组拼接
x = np.array([1, 2, 3])
y = np.array([3, 2, 1])
z = [99, 99, 99]
np.concatenate([x, y,z]) # array([1, 2, 3, 3, 2, 1, 99, 99, 99])

# 二维数组拼接
grid = np.array([[1, 2, 3], 
                 [4, 5, 6]])
np.concatenate([grid, grid]) # 沿着第一个轴拼接
# array([[1, 2, 3],
#        [4, 5, 6],
#        [1, 2, 3],
#        [4, 5, 6]])

np.concatenate([grid, grid], axis=1) # 沿着第二个轴拼接（从0开始索引）
# array([[1, 2, 3, 1, 2, 3],
#        [4, 5, 6, 4, 5, 6]])

# 沿着固定维度处理数组时，使用 np.vstack（垂直栈）和 np.hstack（水平栈）函数会更简洁
x = np.array([1, 2, 3])
y = np.array([[99],
              [99]])
grid = np.array([[9, 8, 7],
                 [6, 5, 4]])

np.vstack([x, grid]) # 垂直栈数组
# array([[1, 2, 3],
#        [9, 8, 7],
#        [6, 5, 4]])

np.hstack([grid, y]) # 水平栈数组
# array([[ 9, 8, 7, 99],
#        [ 6, 5, 4, 99]])
        
np.dstack # 沿着第三个维度拼接数组
```


##### 数组分裂

```
# split
x = [1, 2, 3, 99, 99, 3, 2, 1]
x1, x2, x3 = np.split(x, [3, 5]) # 分裂点位置，N 分裂点会得到 N + 1 个子数组
print(x1, x2, x3) # [1 2 3] [99 99] [3 2 1]

# vsplit
grid = np.arange(16).reshape((4, 4))
# array([[ 0, 1, 2, 3],
#        [ 4, 5, 6, 7],
#        [ 8, 9, 10, 11],
#        [12, 13, 14, 15]])

upper, lower = np.vsplit(grid, [2])
print(upper)
# [[0 1 2 3]
#  [4 5 6 7]]
print(lower)
# [[ 8 9 10 11]
# [12 13 14 15]]

# hsplit
left, right = np.hsplit(grid, [2])
print(left)
# [[ 0 1]
#  [ 4 5]
#  [ 8 9]
#  [12 13]]
print(right)
# [[ 2 3]
#  [ 6 7]
#  [10 11]
#  [14 15]]

# dsplit 沿着第三个维度分裂数组
```


##### NumPy数组的计算：通用函数

NumPy 提供了一个简单灵活的接口来优化数据数组的计算。NumPy 数组的计算有时非常快，有时也非常慢。使 NumPy 变快的关键是利用向量化操作，通常在 NumPy 的通用函数（ufunc）中实现。NumPy 通用函数可以提高数组元素的重复计算的效率。

NumPy 为很多类型的操作提供了非常方便的、静态类型的、可编译程序的接口，也被称作向量操作。你可以通过简单地对数组执行操作来实现，这里对数组的操作将会被用于数组中的每一个元素。这种向量方法被用于将循环推送至 NumPy 之下的编译层，这样会取得更快的执行效率。

通过通用函数用向量的方式进行计算几乎总比用 Python 循环实现的计算更加有效，尤其是当数组很大时。只要你看到 Python 脚本中有这样的循环，就应该考虑能否用向量方式替换这个循环。

```
# 求倒数
import numpy as np
np.random.seed(0)
big_array = np.random.randint(1, 100, size=1000000)

def compute_reciprocals(values):
    output = np.empty(len(values))
    for i in range(len(values)):
        output[i] = 1.0 / values[i]
        return output

# 1、使用循环，2.91s
print(compute_reciprocals(big_array))

# 2、使用通用函数，4.6ms
print(1.0 / big_array) 

```

NumPy 中的向量操作是通过通用函数实现的。通用函数的主要目的是对 NumPy 数组中的值执行更快的重复操作。通用函数也可以对数组进行运算。

```
np.arange(5) / np.arange(1, 6) # 一维数组
x = np.arange(9).reshape((3, 3))
2 ** x # 多维数组
```

通用函数有两种存在形式：
一元通用函数（unary ufunc）：对单个输入操作
二元通用函数（binary ufunc）：对两个输入操作

```
# 数组运算
# 1、标准加减乘除
x = np.arange(4)
print("x =", x)
print("x + 5 =", x + 5)
print("x - 5 =", x - 5)
print("x * 2 =", x * 2)
print("x / 2 =", x / 2)
print("x // 2 =", x // 2) # 地板除法运算

# 2、一元通用函数
print("-x = ", -x) # 逻辑非
print("x ** 2 = ", x ** 2) # 指数运算
print("x % 2 = ", x % 2) # 模运算
```

以下运算符是借助对应的通用函数实现的，直接使用运算符即可。

 | 运算符 | 对应的通用函数   | 描述 | 
 | ----- | -------------   | ---- | 
 | +     | np.add          | 加法运算（即 1 + 1 = 2）
 | -     | np.subtract     | 减法运算（即 3 - 2 = 1）
 | -     | np.negative     | 负数运算（ 即 -2）
 | *     | np.multiply     | 乘法运算（即 2 \* 3 = 6）
 | /     | np.divide       | 除法运算（即 3 / 2 = 1.5）
 | //    | np.floor_divide | 地板除法运算（floor division，即 3 // 2 = 1）
 | **    | np.power        | 指数运算（即 2 ** 3 = 8）
 | %     | np.mod          | 模 / 余数（ 即 9 % 4 = 1）

```
# 绝对值
np.abs(x) # 或者 np.absolute(x)

# 三角函数
theta = np.linspace(0, np.pi, 3) # 定义一个角度数组
print("theta = ", theta)
print("sin(theta) = ", np.sin(theta))
print("cos(theta) = ", np.cos(theta))
print("tan(theta) = ", np.tan(theta))

# 逆三角函数
x = [-1, 0, 1]
print("x = ", x)
print("arcsin(x) = ", np.arcsin(x))
print("arccos(x) = ", np.arccos(x))
print("arctan(x) = ", np.arctan(x))

# 指数运算
x = [1, 2, 3]
print("x =", x)
print("e^x =", np.exp(x))
print("2^x =", np.exp2(x))
print("3^x =", np.power(3, x))

# 对数运算
print("x =", x)
print("ln(x) =", np.log(x))
print("log2(x) =", np.log2(x))
print("log10(x) =", np.log10(x))

# 提高精度
x = [0, 0.001, 0.01, 0.1]
print("exp(x) - 1 =", np.expm1(x))
print("log(1 + x) =", np.log1p(x))
```

更多函数请浏览 [NumPy 文档](http://www.numpy.org/)。


##### 通用函数高级特性

```
# 1、指定输出：所有的通用函数都可以通过 out 参数来指定计算结果的存放位置，有效节约内存
x = np.arange(5)
y = np.empty(5)
np.multiply(x, 10, out=y)

# 这个特性也可以被用作数组视图，例如可以将计算结果写入指定数组的每隔一个元素的位置
y = np.zeros(10)
np.power(2, x, out=y[::2])

# 对比：创建临时数组，并将值复制到y数组中，计算量增大
y[::2] = 2 ** x 

# 2、聚合：多个值经过运算后返回一个值
x = np.arange(1, 6) # [ 0, 10, 20, 30, 40 ]
np.add.reduce(x) # 返回数组中所有元素的和
np.multiply.reduce(x) # 返回数组中所有元素的乘积

# 存储每次计算的中间结果
np.add.accumulate(x) # array([ 1, 3, 6, 10, 15])
np.multiply.accumulate(x) # array([ 1, 2, 6, 24, 120])

# 3、外积：获得两个不同输入数组所有元素对的函数运算结果
x = np.arange(1, 6)
np.multiply.outer(x, x)
# array([[ 1, 2, 3, 4, 5],
#        [ 2, 4, 6, 8, 10],
#        [ 3, 6, 9, 12, 15],
#        [ 4, 8, 12, 16, 20],
#        [ 5, 10, 15, 20, 25]])
```


##### 聚合：最小值、最大值和其他值

当你面对大量的数据时，第一个步骤通常都是计算相关数据的概括统计值。最常用的概括统计值可能是均值和标准差，这两个值能让你分别概括出数据集中的“经典”值，但是其他一些形式的聚合也是非常有用的（如求和、乘积、中位数、最小值和最大值、分位数，等等）。

当你操作 NumPy 数组时，确保你执行的是 NumPy 版本的聚合。

```
# 单维度聚合
big_array = np.random.rand(1000000)
print(min(big_array), max(big_array), np.sum(big_array))
print(big_array.min(), big_array.max(), big_array.sum()) # 数组对象直接调用方法，效果一样

# 多维度聚合
M = np.random.random((3, 4))
M.sum() # 对整个数组的聚合结果
# axis 指定数组将会被折叠的维度，二维数组的维度为x和y
M.min(axis=0) # 第一个轴被折叠，返回每一列的最小值
M.max(axis=1) # 第二个轴被折叠，返回每一行的最大值
```

大多数的聚合都有对 NaN 值的安全处理策略（NaNsafe），即计算时忽略所有的缺失值，这些缺失值即特殊的 IEEE 浮点型 NaN 值。

 | 函数名称       | NaN安全版本      | 描述 | 
 | -------       | -----------      | --- |
 | np.sum        | np.nansum        | 计算元素的和
 | np.prod       | np.nanprod       | 计算元素的积
 | np.mean       | np.nanmean       | 计算元素的平均值
 | np.std        | np.nanstd        | 计算元素的标准差
 | np.var        | np.nanvar        | 计算元素的方差
 | np.min        | np.nanmin        | 找出最小值
 | np.max        | np.nanmax        | 找出最大值
 | np.argmin     | np.nanargmin     | 找出最小值的索引
 | np.argmax     | np.nanargmax     | 找出最大值的索引
 | np.median     | np.nanmedian     | 计算元素的中位数
 | np.percentile | np.nanpercentile | 计算基于元素排序的统计值
 | np.any        | N/A              | 验证任何一个元素是否为真
 | np.all        |  N/A             | 验证所有元素是否为真

```
# 示例：美国总统的身高是多少
import pandas as pd
data = pd.read_csv('data/president_heights.csv')
heights = np.array(data['height(cm)'])
print(heights)

# 计算概括统计值
print("Mean height: ", heights.mean())
print("Standard deviation:", heights.std())
print("Minimum height: ", heights.min())
print("Maximum height: ", heights.max())

# 计算分位数
print("25th percentile: ", np.percentile(heights, 25))
print("Median: ", np.median(heights))
print("75th percentile: ", np.percentile(heights, 75))

# 可视化
%matplotlib inline
import matplotlib.pyplot as plt
import seaborn; seaborn.set() # 设置绘图风格
plt.hist(heights) # 直方图
plt.title('Height Distribution of US Presidents')
plt.xlabel('height (cm)')
plt.ylabel('number');
```


##### 数组的计算：广播

前面介绍了 NumPy 如何通过通用函数的向量化操作来减少缓慢的 Python 循环，另外一种向量化操作的方法是利用 NumPy 的广播功能。广播可以简单理解为用于不同大小数组的二进制通用函数（加、减、乘等）的一组规则。

```
a = np.array([0, 1, 2])
b = np.array([5, 5, 5])

# 对于同样大小的数组，二进制操作是对相应元素逐个计算
print(a + b) # array([5, 6, 7])

# 广播允许这些二进制操作可以用于不同大小的数组，例如数组和标量相加（相当于一个零维数组）
# 相当于将数值 5 扩展或重复至数组 [5, 5, 5]，然后执行加法
print(a + 5) # array([5, 6, 7])

M = np.ones((3, 3))
# 这个一维数组就被扩展或者广播了，沿着第二个维度扩展，扩展到匹配 M 数组的形状
# M = [[1, 1, 1], [1, 1, 1], [1, 1, 1]]
# a = [[0, 1, 2], [0, 1, 2], [0, 1, 2]]
print(M + a)

# 两个数组同时广播，最终结果为一个 3*3 数组
a = np.arange(3) # [0 1 2]
b = np.arange(3)[:, np.newaxis] # [[0], [1], [2]]
print(a + b)
```

![2-4](https://upload-images.jianshu.io/upload_images/4311027-eaf610fd1e4e2981.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

广播遵循以下规则：

规则 1：如果两个数组的维度数不相同，那么小维度数组的形状将会在最左边补 1。
规则 2：如果两个数组的形状在任何一个维度上都不匹配，那么数组的形状会沿着维度为 1 的维度扩展以匹配另外一个数组的形状。
规则 3：如果两个数组的形状在任何一个维度上都不匹配并且没有任何一个维度等于 1，那么会引发异常。

```
# 广播示例1
# 根据规则1，数组 a 的维度数更小，所以在其左边补1；根据规则2，第一个维度不匹配，因此扩展这个维度以匹配数组
M = np.ones((2, 3)) # M.shape = (2, 3) -> (2, 3) -> (2, 3)
a = np.arange(3)    # a.shape = (3,  ) -> (1, 3) -> (2, 3)

# 广播示例2
# 根据规则1用 1 将 b 的形状补全，根据规则2更新这两个数组的维度来相互匹配
a = np.arange(3).reshape((3, 1)) # a.shape = (3, 1) -> (3, 1) -> (3, 3)
b = np.arange(3)                 # b.shape = (3,  ) -> (1, 3) -> (3, 3)

# 广播示例3：两个数组不兼容
M = np.ones((3, 2)) # M.shape = (3, 2) -> (3, 2) -> (3, 2)
a = np.arange(3)    # a.shape = (3,  ) -> (1, 3) -> (3, 3)
M + a[:, np.newaxis] # 通过变形数组来实现
```


##### 广播的实际应用

```
# 1、数组的归一化
np.random.random((10, 3))
Xmean = X.mean(0) # 沿着第一个维度聚合，计算第一个特征的均值
X_centered = X - Xmean # 从 X 数组的元素中减去这个均值实现归一化

# 2、基于二维函数显示图像
x = np.linspace(0, 5, 50) 
y = np.linspace(0, 5, 50)[:, np.newaxis] # x和y表示0~5区间50个步长的序列
# 定义一个函数 z = f (x, y)，可以用广播沿着数值区间计算该函数
z = np.sin(x) ** 10 + np.cos(10 + y * x) * np.cos(x)

import matplotlib.pyplot as plt
plt.imshow(z, origin='lower', extent=[0, 5, 0, 5], cmap='viridis')
plt.colorbar();
```


##### 比较、掩码和布尔逻辑

当你想基于某些准则来抽取、修改、计数或对一个数组中的值进行其他操作时，掩码就可以派上用场了。例如你可能希望统计数组中有多少值大于某一个给定值，或者删除所有超出某些门限值的异常点。在 NumPy 中，布尔掩码通常是完成这类任务的最高效方式。

```
# 示例：统计下雨天数
import numpy as np
import pandas as pd
# 利用 Pandas 抽取 2014 年西雅图市的日降水统计数据，放入一个 NumPy 数组
# 数组包含 365 个值，降水量的单位是英寸
rainfall = pd.read_csv('data/Seattle2014.csv')['PRCP'].values
inches = rainfall / 254 # 1/10mm -> inches
inches.shape

%matplotlib inline
import matplotlib.pyplot as plt
import seaborn; seaborn.set() # 设置绘图风格

# 上面的直方图无法回答西雅图一年中下雨天数，平均降水量，有多少天的降水量超过了半英寸
# 可以对全部数据使用循环，当数据落在区间时计数器便加 1，但浪费时间且非常低效
# NumPy 的通用函数可以用来替代循环，以快速实现数组的逐元素（element-wise）运算
# 同样，我们也可以用其他通用函数实现数组的逐元素比较

print("Number days without rain: ", np.sum(inches == 0))
print("Number days with rain: ", np.sum(inches != 0))
print("Days with more than 0.5 inches:", np.sum(inches > 0.5))
print("Rainy days with < 0.1 inches :", np.sum((inches > 0) & (inches < 0.2)))

# 为所有下雨天创建一个掩码
rainy = (inches > 0)
# 构建一个包含整个夏季日期的掩码（6月21日是第172天）
summer = (np.arange(365) - 172 < 90) & (np.arange(365) - 172 > 0)
print("Median precip on rainy days in 2014 (inches): ", np.median(inches[rainy]))
print("Median precip on summer days in 2014 (inches): ", np.median(inches[summer]))
print("Maximum precip on summer days in 2014 (inches): ", np.max(inches[summer]))
print("Median precip on non-summer rainy days (inches):", np.median(inches[rainy & ~summer]))
```

以下运算符是借助对应的通用函数实现的，直接使用运算符即可。

 | 运算符 | 对应的通用函数 |
 | ------ | ------------ | 
 | ==     | np.equal
 | !=     | np.not_equal
 | <      | np.less
 | <=     | np.less_equal
 | >      | np.greater
 | >=     | np.greater_equal
 | &      | np.bitwise_and
 |\|      | np.bitwise_or
 | ^      | np.bitwise_xor
 | ~      | np.bitwise_not

```
# 1、比较操作：和通用函数类似，可以用于任意形状、大小的数组
x = np.array([1, 2, 3, 4, 5])
x <= 3 # array([True, True, True, False, False], dtype=bool)
x >= 3 # array([False, False, True, True, True], dtype=bool)
x != 3 # array([ True, True, False, True, True], dtype=bool)
x == 3 # array([False, False, True, False, False], dtype=bool)
(2 * x) == (x ** 2) # array([False, True, False, False, False], dtype=bool)

# 2、布尔数组操作
x = np.array([[5, 0, 3, 3],
              [7, 9, 3, 5],
              [2, 4, 7, 6]])
np.count_nonzero(x < 6) # 统计布尔数组中 True 记录的个数
np.sum(x < 6, axis=1) # 另一种实现方式，好处是这个求和也可以沿着行或列进行
np.all(x < 8, axis=1) # 是否每行的所有值都小于8？array([ True, False, True], dtype=bool)
np.all(x == 6) # 是否所有值都等于6？

# 3、布尔运算符
np.sum((inches > 0.5) & (inches < 1)) # 降水量在 0.5 英寸~1 英寸间

# 4、将布尔数组作为掩码，通过该掩码选择数据的子数据集
x[x < 5] # 这些值是掩码数组对应位置为 True 的值，array([0, 3, 3, 3, 2, 4])

# 5、使用关键字 and/or 与使用逻辑操作运算符 &/|
# and 和 or 对整个对象执行单个布尔运算，所有非零的整数都会被当作是 True
bool(42), bool(0) # (True, False)
# & 和 | 对一个对象的内容（单个比特或字节）执行多个布尔运算（and/or）
bin(42) # '0b101010'
bin(59) # '0b111011'
bin(42 & 59) # '0b101010'
bin(42 | 59) # '0b111011'

# 布尔数组被当作是由比特字符组成的，可以逐个比较
A = np.array([1, 0, 1, 0, 1, 0], dtype=bool)
B = np.array([1, 1, 1, 0, 1, 1], dtype=bool)
A | B # array([ True, True, True, False, True, True], dtype=bool)
A or B # 计算整个数组的真或假，报错

# 对给定数组进行逻辑运算时，也应该使用 | 或 &，而不是 or 或 and
x = np.arange(10)
(x > 4) & (x < 8) # array([False, False, ..., True, True, False, False], dtype=bool)
(x > 4) and (x < 8) # 报错
```


##### 索引

```
rand = np.random.RandomState(42)
x = rand.randint(100, size=10)
print(x) # [51 92 14 71 60 20 82 86 74 74]

# 1、通过简单索引（数组下标）获得3个元素
[x[3], x[7], x[2]] 

# 2、通过索引列表获得3个元素：结果的形状与索引列表的形状一致，而不是与被索引列表的形状一致
ind = [3, 7, 4]
x[ind] 

X = np.arange(12).reshape((3, 4))
# array([[ 0, 1, 2, 3],
#        [ 4, 5, 6, 7],
#        [ 8, 9, 10, 11]])

row = np.array([0, 1, 2])
col = np.array([2, 1, 3])
X[row, col]
# X[0, 2]，X[1, 1]，X[2, 3]：array([ 2, 5, 11])

# 索引值的配对遵循广播的规则，因此会得到一个二维的结果
X[row[:, np.newaxis], col]
# array([[ 2, 1, 3],
#        [ 6, 5, 7],
#        [10, 9, 11]])

# 每一行的值都与每一列的向量配对，就像广播的运算
X[row[:, np.newaxis] * col]
# array([[0, 0, 0],
#        [2, 1, 3],
#        [4, 2, 6]])

# 3、组合索引
# 索引列表+简单索引
X[2, [2, 0, 1]] 
# array([10, 8, 9])

# 索引列表+切片
X[1:, [2, 0, 1]] 
# array([[ 6, 4, 5],
#        [10, 8, 9]])

# 索引列表+掩码
mask = np.array([1, 0, 1, 0], dtype=bool)
X[row[:, np.newaxis], mask] 
# array([[ 0, 2],
#        [ 4, 6],
#        [ 8, 10]])

# 4、使用索引列表修改值
x = np.arange(10)
i = np.array([2, 1, 8, 4])
x[i] = 99

# 实现累加
i = [2, 3, 3, 4, 4, 4]
x[i] += 1 # 不能实现，发生赋值

x = np.zeros(10)
np.add.at(x, i, 1) # 执行就地操作，对数组x的索引i的值加1
# [ 0. 0. 1. 2. 3. 0. 0. 0. 0. 0.]

# 示例：选择随机点
# 通常用于快速分割数据，即需要分割训练 / 测试数据集以验证统计模型，以及在解答统计问题时的抽样方法
中使用
mean = [0, 0]
cov = [[1, 2],
       [2, 5]]
X = rand.multivariate_normal(mean, cov, 100) # 二维正态分布的点组成的数组
X.shape # (100, 2)

# 散点图
%matplotlib inline
import matplotlib.pyplot as plt
import seaborn; seaborn.set() # 设置绘图风格
plt.scatter(X[:, 0], X[:, 1]);

indices = np.random.choice(X.shape[0], 20, replace=False) # 随机选取 20 个值，作为索引列表
selection = X[indices] # 利用索引列表取出数组值
plt.scatter(X[:, 0], X[:, 1], alpha=0.3)
# 将选中的点在图上用大圆圈标示出来
plt.scatter(selection[:, 0], selection[:, 1], facecolor='none', edgecolor='b', s=200);

# 示例：数据区间划分
np.random.seed(42)
x = np.random.randn(100)
bins = np.linspace(-5, 5, 20) # 手动计算直方图
counts = np.zeros_like(bins)
i = np.searchsorted(bins, x) 
np.add.at(counts, i, 1) # 为每个区间加上1
plt.plot(bins, counts, linestyle='steps'); # 画出结果

plt.hist(x, bins, histtype='step'); # 等价于以上代码，且数据量大时执行效率更高
np.histogram(x, bins)
np.searchsorted(bins, x), 1)
```

算法效率并不是一个简单的问题。一个对大数据集非常有效的算法并不总是小数据集的最佳选择，反之同理。但是自己编写这个算法的好处是可以理解这些基本方法。你可以利用这些编写好的模块去扩展，以实现一些有意思的自定义操作。将 Python 有效地用于数据密集型应用中的关键是，当应用场景合适时知道使用现成函数，当需要执行更多指定的操作时也知道如何利用更低级的功能来实现。


##### 数组的排序

```
# 1、选择排序：简洁，但是对于大数组来说太慢了
# 对于一个包含 N 个值的数组来说，它需要做 N 个循环，每个循环中执行~ N 次比较，以找到交换值，O(n²)
import numpy as np
def selection_sort(x):
    for i in range(len(x)):
        swap = i + np.argmin(x[i:])
        (x[i], x[swap]) = (x[swap], x[i])
    return x

x = np.array([2, 1, 4, 3, 5])
selection_sort(x)

# 2、全部排序
# np.sort：默认为快速排序，O(NlogN)，也可以选择归并排序和堆排序
# Python 有内置的 sort 和 sorted 函数可以对列表进行排序，但是 NumPy 的 np.sort 函数实际上效率更高
x = np.array([2, 1, 4, 3, 5])
np.sort(x) # 返回排好序的数组，不修改原始输入数组
x.sort() # 用排好序的数组替代原始数组
i = np.argsort(x) # 返回原始数组排好序的索引值！[1 0 3 2 4]
x[i] # 排好序的有序数组！array([1, 2, 3, 4, 5])

# 沿着行或列排序
# 将行或列当作独立的数组，任何行或列的值之间的关系将会丢失
rand = np.random.RandomState(42)
X = rand.randint(0, 10, (4, 6))
print(X)
# array([[6 3 7 4 6 9]
#        [2 6 7 4 3 7]
#        [7 2 5 4 1 7]
#        [5 1 4 0 9 5]])

np.sort(X, axis=0) # 对X的每一列排序
# array([[2, 1, 4, 0, 1, 5],
#        [5, 2, 5, 4, 3, 7],
#        [6, 3, 7, 4, 6, 7],
#        [7, 6, 7, 4, 9, 9]])

np.sort(X, axis=1) # 对X每一行排序
# array([[3, 4, 6, 6, 7, 9],
#        [2, 3, 4, 6, 7, 7],
#        [1, 2, 4, 5, 7, 7],
#        [0, 1, 4, 5, 5, 9]])

# 3、部分排序：分隔
# np.partition：找到数组中第 K 小的值，用该值进行分隔，在这两个分隔区间中，元素都是任意排列的
x = np.array([7, 2, 3, 1, 6, 5, 4])
np.partition(x, 3) # 前三个值是数组中最小的三个值，剩下的位置是原始数组剩下的值
# array([2, 1, 3, 4, 6, 5, 7]) 

np.partition(X, 2, axis=1) # 沿着多维数组任意的轴进行分隔
# 该数组每一行的前两个元素是该行最小的两个值，每行的其他值分布在剩下的位置
# array([[3, 4, 6, 7, 6, 9],
#        [2, 3, 4, 7, 6, 7],
#        [1, 2, 4, 5, 7, 7],
#        [0, 1, 4, 5, 9, 5]])

i = np.argpartition(X) # 分隔的索引值

# 示例：K个最近邻
X = rand.rand(10, 2) # 有 10 个随机点的集合

%matplotlib inline
import matplotlib.pyplot as plt
import seaborn; seaborn.set() # 设置画图风格
plt.scatter(X[:, 0], X[:, 1], s=100); # 散点图

# 计算两两数据点对间的距离的平方，（X1-Y1)²+(X2-Y2)²
dist_sq = np.sum((X[:,np.newaxis,:] - X[np.newaxis,:,:]) ** 2, axis=-1) 

# 计算每个点的最近邻，因为要排除本身，所以axis为1
nearest = np.argsort(dist_sq, axis=1)

# 计算 k 个最近邻，分隔每一行，最小的 k + 1 的平方距离将排在最前面，其他更长的距离排在其他位置
K = 2
nearest_partition = np.argpartition(dist_sq, K + 1, axis=1)

# 将邻节点网络可视化，每个点和其最近的两个最近邻连接
plt.scatter(X[:, 0], X[:, 1], s=100)
K = 2 # 将每个点与它的两个最近邻连接
for i in range(X.shape[0]):
    for j in nearest_partition[i, :K+1]: # 画一条从X[i]到X[j]的线段
        plt.plot(*zip(X[j], X[i]), color='black') # 用zip方法实现
```

![2-11](https://upload-images.jianshu.io/upload_images/4311027-54cd2c7e43afac94.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### 结构化数据：NumPy的结构化数组

NumPy 的结构化数组和记录数组，它们为复合的、异构的数据提供了非常有效的存储，通常也可以用 Pandas 的 DataFrame 来实现，并且 Pandas 有时更好用。

```
name = ['Alice', 'Bob', 'Cathy', 'Doug']
age = [25, 45, 37, 19]
weight = [55.0, 85.5, 68.0, 61.5]

# 1、使用复合数据结构的结构化数组
data = np.zeros(4, dtype={'names':('name', 'age', 'weight'), 'formats':('U10', 'i4', 'f8')})
print(data.dtype) 
# [('name', '<U10'), ('age', '<i4'), ('weight', '<f8')]

# 2、将列表数据放入数组中，按顺序匹配
data['name'] = name
data['age'] = age
data['weight'] = weight
print(data)
# [('Alice', 25, 55.0) ('Bob', 45, 85.5) ('Cathy', 37, 68.0) ('Doug', 19, 61.5)]

# 获取所有名字
data['name']
# array(['Alice', 'Bob', 'Cathy', 'Doug'], dtype='<U10')

# 获取数据第一行
data[0]
# ('Alice', 25, 55.0)

# 获取最后一行的名字
data[-1]['name']
# 'Doug'

# 获取年龄小于30岁的人的名字（布尔掩码）
data[data['age'] < 30]['name']
# array(['Alice', 'Doug'], dtype='<U10')
```

dtype格式，每一位的含义：

1、 <（低字节序 / >（高字节序），表示字节（bytes）类型的数据在内存中存放顺序的习惯用法
2、数据的类型：字符、字节、整型、浮点型，等等
3、该对象的字节大小

NumPy的数据类型：

 | NumPy数据类型符号 | 描述  | 示例 | 
 | ---------------- | ---- | ---- | 
 | 'b'              | 字节型 | np.dtype('b')
 | 'i'              | 有符号整型 | np.dtype('i4') == np.int32
 | 'u'              | 无符号整型 | np.dtype('u1') == np.uint8
 | 'f'              | 浮点型 | np.dtype('f8') == np.int64
 | 'c'              | 复数浮点型 | np.dtype('c16') == np.complex128
 | 'S'、'a'         | 字符串 | np.dtype('S5')
 | 'U'              | Unicode 编码字符串 | np.dtype('U') == np.str_
 | 'V'              | 原生数据，raw data（空，void） |  np.dtype('V') == np.void


### 第3章　Pandas 数据处理

NumPy 和它的 ndarray 对象，为 Python 多维数组提供了高效的存储和处理方法。Pandas 是在 NumPy 基础上建立的新程序库，提供了一种高效的 DataFrame 数据结构。DataFrame 本质上是一种带行标签和列标签、支持相同类型数据和缺失值的多维数组。建立在 NumPy 数组结构上的 Pandas，尤其是它的 Series 和 DataFrame 对象，为数据科学家们处理那些消耗大量时间的“数据清理”（data munging）任务提供了捷径。

##### Pandas 对象简介

如果从底层视角观察 Pandas 对象，可以把它们看成增强版的 NumPy 结构化数组，行列都不再只是简单的整数索引，还可以带上标签。Pandas 的三个基本数据结构：Series、DataFrame 和 Index。

```
import numpy as np
import pandas as pd

# Series 对象：带索引数据构成的一维数组
# 与 NumPy 数组间的本质差异其实是索引：
# NumPy 数组通过隐式定义的整数索引获取数值
# Pandas 的 Series 对象用一种显式定义的索引与数值关联，索引不仅可以是数字，还可以是字符串等其他类型

1、Serise 是通用的 NumPy 数组
data = [0.25, 0.5, 0.75, 1.0]
index = ['a', 'b', 'c', 'd']
data = pd.Series(data, index=index)
# 0 0.25
# 1 0.50
# 2 0.75
# 3 1.00
# dtype: float64

data.values # 获取值
data.index  # 获取索引，类型为 pd.Index 的类数组对象 RangeIndex(start=0, stop=4, step=1)
data[1:3] # 通过中括号索引获取值
data['b'] # 通过字符串索引获取值

# 2、Series 是特殊的 Python 字典
population_dict = {'California': 38332521,
                   'Texas': 26448193,
                   'New York': 19651127,
                   'Florida': 19552860,
                   'Illinois': 12882135}
population = pd.Series(population_dict) # 使用 Python 字典创建 Series 对象，默认索引为第一列

population['California'] # 获取值
population['California':'Illinois'] # 还支持数组形式的操作，比如切片

# data 可以是列表或 NumPy 数组，这时 index 默认值为整数序列
pd.Series([2, 4, 6])
# data 也可以是一个标量，创建 Series 对象时会重复填充到每个索引上
pd.Series(5, index=[100, 200, 300])
# data 还可以是一个字典，index 默认是排序的字典键
pd.Series({2:'a', 1:'b', 3:'c'})
# 每一种形式都可以通过显式指定索引筛选需要的结果
pd.Series({2:'a', 1:'b', 3:'c'}, index=[3, 2]) # 筛选出c和a

# DataFrame 对象：既有灵活的行索引，又有灵活列名的二维数组

states.index
# Index(['California', 'Florida', 'Illinois', 'New York', 'Texas'], dtype='object')
states.columns
# Index(['area', 'population'], dtype='object')
states['area']
# 每个州的面积

# 1、通过单个 Series 对象创建 DataFrame
pd.DataFrame(population, columns=['population'])

# 2、通过字典列表创建 DataFrame
data = [{'a': i, 'b': 2 * i}
        for i in range(3)]
pd.DataFrame(data)

# 这里相当于用 b 做笛卡尔积，因为 b 没有相等的时候，所以 a 和 c 都有缺失，缺失值用 NaN 表示
pd.DataFrame([{'a': 1, 'b': 2}, {'b': 3, 'c': 4}])
#   a   b   c
# 0 1.0 2   NaN
# 1 NaN 3   4.0

# 3、通过 Series 对象字典创建 DataFrame
area_dict = {'California': 423967, 'Texas': 695662, 'New York': 141297, 'Florida': 170312, 'Illinois': 149995}
area = pd.Series(area_dict)
# 结合上面的 population 和 area 两个 Series，创建 DataFrame
states = pd.DataFrame({'population': population, 'area': area}) 
#            area   population
# California 423967 38332521
# Florida    170312 19552860
# Illinois   149995 12882135
# New York   141297 19651127
# Texas      695662 26448193

# 4、通过 NumPy 二维数组创建 DataFrame
pd.DataFrame(np.random.rand(3, 2), columns=['foo', 'bar'], index=['a', 'b', 'c'])
#   foo      bar
# a 0.865257 0.213169
# b 0.442759 0.108267
# c 0.047110 0.905718

# 5、通过 NumPy 结构化数组创建 DataFrame
A = np.zeros(3, dtype=[('A', 'i8'), ('B', 'f8')])
pd.DataFrame(A)
#   A B
# 0 0 0.0
# 1 0 0.0
# 2 0 0.0

# Index 对象：不可变数组或有序集合
ind = pd.Index([2, 3, 5, 7, 11])
# Int64Index([2, 3, 5, 7, 11], dtype='int64')

# 1、将Index看作不可变数组：像数组一样进行操作，但是不能修改它的值
ind[1]
ind[::2]
print(ind.size, ind.shape, ind.ndim, ind.dtype)

# 2、将Index看作有序集合：对集合进行并、交、差
# 这些操作还可以通过调用对象方法来实现，例如 indA.intersection(indB)
indA = pd.Index([1, 3, 5, 7, 9])
indB = pd.Index([2, 3, 5, 7, 11])
indA & indB # 交集 Int64Index([3, 5, 7], dtype='int64')
indA | indB # 并集 Int64Index([1, 2, 3, 5, 7, 9, 11], dtype='int64')
indA ^ indB # 异或 Int64Index([1, 2, 9, 11], dtype='int64')
```


##### 数据取值与选择

```
# 1、Series 对象
# 1.1、将 Series 看作 Python 字典：键值对的映射
data = pd.Series([0.25, 0.5, 0.75, 1.0], index=['a', 'b', 'c', 'd'])
data['b']
'a' in data # 检测键 True
data.keys() # 列出索引
list(data.items()) # 列出键值
data['e'] = 1.25 # 增加新的索引值扩展 Series

# 1.2、将 Series 看作 Numpy 一维数组
data['a':'c'] # 切片（使用索引名称）：显式索引，结果包含最后一个索引
data[1:3]     # 切片（使用索引位置）：隐式索引，结果不包含最后一个索引，从0开始
data[1]       # 取值：显式索引

data[(data > 0.3) & (data < 0.8)] # 掩码
data[['a', 'e']] # 索引列表

# 2、索引器：loc、iloc 和 ix（使用索引位置取值，取值范围均为左闭右开！）
# 由于整数索引很容易造成混淆，所以 Pandas 提供了一些索引器（indexer）属性来作为取值的方法
# 它们不是 Series 对象的函数方法，而是暴露切片接口的属性
# Python 代码的设计原则之一是“显式优于隐式”

# 2.1、loc 属性：取值和切片都是显式的，从1开始
data.loc[1]   # 第1行
data.loc[1:3] # 第1-2行（左闭右开）

# 2.2、iloc 属性：取值和切片都是 Python 形式的隐式索引，从0开始
data.iloc[1]   # 第2行
data.iloc[1:3] # 第2-3行（左闭右开）

# 2.3、ix 属性：实现混合效果
data.ix[:3, :'pop'] # 第0-2行，第1-pop列

# 3、DataFrame 对象
# 3.1、将 DataFrame 看作 Python 字典：键值对的映射
data['area'] # 字典形式取值：建议使用
data.area # 属性形式取值：如果列名不是纯字符串，或者列名与 DataFrame 的方法同名，那么就不能用属性索引
data.area is data['area'] # True
data['density'] = data['pop'] / data['area'] # 增加一列

# 3.2、将 DataFrame 看作二维数组：可以把许多数组操作方式用在 DataFrame 上
data.values # 按行查看数组数据
data.T # 行列转置
data.values[0] # 获取一行数据
data['area']   # 获取一列数据：向 DataFrame 传递单个列索引

data.iloc[:3, :2] # 3行2列
data.loc[:'Illinois', :'pop'] # 截至到指定行列名的数值
data.ix[:3, :'pop'] # 第0-2行，第1-pop列
data.loc[data.density > 100, ['pop', 'density']] # 组合使用掩码与索引列表

# 以上任何一种取值方法都可以用于调整数据，这一点和 NumPy 的常用方法是相同的
data.iloc[0, 2] = 90

# 3.3、其他取值方法
# 如果对单个标签取值就选择列，而对多个标签用切片就选择行
data['Florida':'Illinois'] # 指定的两行，所有列（缺省）
data[1:3] # 不用索引值，而直接用行数来实现，从0开始的第1-2行
data[data.density > 100] # 掩码操作直接对行进行过滤
```


##### Pandas 数值运算方法

NumPy 的基本能力之一是快速对每个元素进行运算，既包括基本算术运算（加、减、乘、除），也包括更复杂的运算（三角函数、指数函数和对数函数等）。Pandas 继承了 NumPy 的功能，其中通用函数是关键。

但是 Pandas 也实现了一些高效技巧：对于一元运算（像函数与三角函数），这些通用函数将在输出结果中保留索引和列标签；而对于二元运算（如加法和乘法），Pandas 在传递通用函数时会自动对齐索引进行计算。这就意味着，保存数据内容与组合不同来源的数据（两处在 NumPy 数组中都容易出错的地方）变成了 Pandas 的杀手锏。

```
# 1、通用函数：保留索引
# 因为 Pandas 是建立在 NumPy 基础之上的，所以 NumPy 的通用函数同样适用于 Pandas 的 Series 和 DataFrame 对象

rng = np.random.RandomState(42)
ser = pd.Series(rng.randint(0, 10, 4)) # 0-10之间任意取4个整数
# 0 6
# 1 3
# 2 7
# 3 4
# dtype: int64

np.exp(ser) # 对 Series 使用 Numpy 通用函数，结果是一个保留索引的 Series

df = pd.DataFrame(rng.randint(0, 10, (3, 4)), columns=['A', 'B', 'C', 'D']) # 0-10，3行4列
#   A B C D
# 0 6 9 2 6
# 1 7 4 3 7
# 2 7 2 5 4

np.sin(df * np.pi / 4) # 对 DataFrame 使用 Numpy 通用函数，结果是一个保留索引的 DataFrame

# 2、通用函数：索引对齐
# 当在两个 Series 或 DataFrame 对象上进行二元计算时，Pandas 会在计算过程中对齐两个对象的索引！
# 实际上就是对索引的全外连接

# 2.1、Series 索引对齐
area = pd.Series({'Alaska': 1723337, 'Texas': 695662, 'California': 423967}, name='area')
population = pd.Series({'California': 38332521, 'Texas': 26448193, 'New York': 19651127}, name='population')
population / area 
# 结果数组的索引是两个输入数组索引的并集，缺失位置的数据会用 NaN 填充
# Alaska     NaN
# California 90.413926
# New York   NaN
# Texas      38.018740

A = pd.Series([2, 4, 6], index=[0, 1, 2])
B = pd.Series([1, 3, 5], index=[1, 2, 3])
A + B
# 此处重复的索引是1和2，索引0和3的值运算结果为NaN
# 0 NaN
# 1 5.0
# 2 9.0
# 3 NaN

# 等价于 A + B，可以设置参数自定义 A 或 B 缺失的数据
A.add(B, fill_value=0)

# 2.2、DataFrame 索引对齐
A = pd.DataFrame(rng.randint(0, 20, (2, 2)), columns=list('AB'))
#   A B
# 0 1 11
# 1 5 1

B = pd.DataFrame(rng.randint(0, 10, (3, 3)), columns=list('BAC'))
#   B A C
# 0 4 0 9
# 1 5 8 0
# 2 9 2 6

A + B
# 行索引对齐（0行对0行），对应的列索引做运算（A列+A列）
#   A    B    C
# 0 1.0  15.0 NaN
# 1 13.0 6.0  NaN
# 2 NaN  NaN  NaN

# 用 A 中所有值的均值来填充缺失值（计算 A 的均值需要用 stack 将二维数组压缩成一维数组）
fill = A.stack().mean()
A.add(B, fill_value=fill)

# 2.3、通用函数：DataFrame 与 Series 的运算
A = rng.randint(10, size=(3, 4))
# array([[3, 8, 2, 4],
#        [2, 6, 4, 8],
#        [6, 1, 3, 8]])

# 二维数组减自身的一行数据会按行计算，也就是用每一行的值减去第一行的对应值
A - A[0]
# array([[ 0, 0, 0, 0],
#        [-1, -2, 2, 4],
#        [ 3, -7, 1, 4]])

# 在 Pandas 里默认也是按行运算的
df = pd.DataFrame(A, columns=list('QRST'))
#   Q R S T
# 0 3 8 2 4
# 1 2 6 4 8
# 2 6 1 3 8

df - df.iloc[0]
#    Q  R  S  T
# 0  0  0  0  0
# 1 -1 -2  2  4
# 2  3 -7  1  4

# 如果想按列计算，那么就需要通过 axis 参数设置
df.subtract(df['R'], axis=0)
#    Q  R  S  T
# 0 -5  0 -6  4
# 1 -4  0 -2  2
# 2  5  0  2  7

# DataFrame / Series 的运算结果的索引都会自动对齐
halfrow = df.iloc[0, ::2] # 每2列取1列
#   Q S
# 0 3 2

df - halfrow
# df中的每一行都减掉halfrow中的0行，对应列相减，halfrow中没有的列运算结果为NaN
# 相当于将halfrow复制成与df相同的形状（行数），再进行运算
#      Q    R    S    T
# 0  0.0  NaN  0.0  NaN
# 1 -1.0  NaN  2.0  NaN
# 2  3.0  NaN  1.0  NaN

# 这些行列索引的保留与对齐方法说明 Pandas 在运算时会一直保存这些数据内容
# 避免在处理数据类型有差异和 / 或维度不一致的 NumPy 数组时可能遇到的问题
```

 | Python运算符 | Pandas方法 |
 | ----------- | ---------- |
 | +           | add()
 | -           | sub()、subtract()
 | *           | mul()、multiply()
 | /           | truediv()、div()、divide()
 | //          | floordiv()
 | %           | mod()
 | **          | pow()


##### 处理缺失值

缺失值主要有三种形式：null、NaN 或 NA。

```
# 1、None：Python 对象类型的缺失值（Python 单体对象）
# 不能作为任何 NumPy / Pandas 数组类型的缺失值，只能用于 'object' 数组类型（即由 Python 对象构成的数组）
# 在进行常见的快速操作时，这种类型比其他原生类型数组要消耗更多的资源
vals1 = np.array([1, None, 3, 4])
# array([1, None, 3, 4], dtype=object)

# 如果你对一个包含 None 的数组进行累计操作，通常会出现类型错误
vals1.sum()
# TypeError：在 Python 中没有定义整数与 None 之间的加法运算

# 2、NaN：数值类型的缺失值（特殊浮点数）
# NumPy 会为这个数组选择一个原生浮点类型，和 object 类型数组不同，这个数组会被编译成 C 代码从而实现快速操作
# NaN 会将与它接触过的数据同化，无论和 NaN 进行何种操作，最终结果都是 NaN
1 + np.nan
# NaN

# 不会抛出异常，但是并非有效的
vals2.sum(), vals2.min(), vals2.max()
# (nan, nan, nan) 

# NumPy 也提供了一些特殊的累计函数，它们可以忽略缺失值的影响
np.nansum(vals2), np.nanmin(vals2), np.nanmax(vals2)
# (8.0, 1.0, 4.0)

# 3、Pandas 中 NaN 与 None 的差异
# 虽然 NaN 与 None 各有各的用处，但是 Pandas 把它们看成是可以等价交换的，在适当的时候会将两者进行替换
pd.Series([1, np.nan, 2, None])
# 0 1.0
# 1 NaN
# 2 2.0
# 3 NaN
# dtype: float64

# Pandas 会将没有标签值的数据类型自动转换为 NA
x = pd.Series(range(2), dtype=int)
# 0 0
# 1 1
# dtype: int64
x[0] = None
# 0 NaN
# 1 1.0
# dtype: float64

# 3、处理缺失值
# 3.1 发现缺失值
data = pd.Series([1, np.nan, 'hello', None])
data.isnull()
data.notnull()
# 0 False
# 1 True
# 2 False
# 3 True
# dtype: bool

# 布尔类型掩码数组可以直接作为 Series 或 DataFrame 的索引使用
data[data.notnull()]
# 0 1
# 2 hello
# dtype: object

# 3.2 剔除缺失值
# 剔除 Series 中的缺失值
data.dropna()

# 剔除 DataFrame 中的缺失值，默认会剔除任何包含缺失值的整行数据
df.dropna() 
df.dropna(axis='columns') 
df.dropna(axis=1) # 二者等价，剔除任何包含缺失值的整列数据

# 默认设置 how='any'（只要有缺失值就剔除整行或整列）
# 可以设置 how='all'（只会剔除全部是缺失值的行或列）
df.dropna(axis='columns', how='all') 

# thresh 参数设置行或列中非缺失值的最小数量
df.dropna(axis='rows', thresh=3)

# 3.3 填充缺失值
data = pd.Series([1, np.nan, 2, None, 3], index=list('abcde'))

# 用值填充 Series
data.fillna(0) 

# 用缺失值前面的有效值来从前往后填充 Series（forward-fill）
data.fillna(method='ffill')

# 用缺失值后面的有效值来从后往前填充 Series（back-fill）
data.fillna(method='bfill')

# DataFrame 的操作方法与 Series 类似，只是在填充时需要设置坐标轴参数 axis（axis=1填充列，axis=0填充行）
df.fillna(method='ffill', axis=1)
```

 | 类型 | 缺失值转换规则 | NA标签值 | 
 | --- | ------------ | ------- | 
 | floating 浮点型  | 无变化            | np.nan
 | object  对象类型 | 无变化            | None 或 np.nan
 | integer 整数类型 | 强制转换为 float64 | np.nan
 | boolean 布尔类型 | 强制转换为 object  | None 或 np.nan 

需要注意的是，Pandas 中字符串类型的数据通常是用 object 类型存储的。


##### 层级索引

到目前为止，我们接触的都是一维数据和二维数据，用 Pandas 的 Series 和 DataFrame 对象就可以存储。但我们也经常会遇到存储多维数据的需求，数据索引超过一两个键。因此，Pandas 提供了 Panel 和 Panel4D 对象解决三维数据与四维数据。

而在实践中，更直观的形式是通过层级索引（hierarchical indexing，也被称为多级索引，multi-indexing）配合多个有不同等级（level）的一级索引一起使用，这样就可以将高维数组转换成类似一维 Series 和二维DataFrame 对象的形式。

```
 # 1、多级索引 Series
 # 假设你想要分析美国各州在两个不同年份的数据
 index = [('California', 2000), 
          ('California', 2010),
          ('New York', 2000), 
          ('New York', 2010),
          ('Texas', 2000), 
          ('Texas', 2010)
]
populations = [33871648, 
               37253956,
               18976457, 
               19378102,
               20851820, 
               25145561
]

# 1.1 普通方法：用一个 Python 元组来表示索引
pop = pd.Series(populations, index=index)
pop[('California', 2010):('Texas', 2000)]
pop[[i for i in pop.index if i[1] == 2010]] # 切片很不方便

# 1.2 更优方法：Pandas 多级索引
# MultiIndex 里面有一个 levels 属性表示索引的等级
# 这样做可以将州名和年份作为每个数据点的不同标签
index = pd.MultiIndex.from_tuples(index)
# MultiIndex(levels=[['California', 'New York', 'Texas'], [2000, 2010]],
#            labels=[[0, 0, 1, 1, 2, 2], [0, 1, 0, 1, 0, 1]])
```

211


