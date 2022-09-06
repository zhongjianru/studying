### 第1章 Spark 与大数据
### 第2章 Spark 基础
### 第3章 Spark 工作机制
### 第4章 Spark 内核讲解
### 第5章 Spark 与数据仓库
### 第6章 Spark 流式计算
### 第7章 Spark 图计算
### 第8章 Spark MLlib
### 附录  Scala 语言参考

### 第1章 Spark 与大数据

数据只是工具，最终还是要用于创造价值，大数据只是一种新的实践。

#### 大数据的发展及现状

大数据的特征：
* 大量（volume）
* 多样（variety）
* 快速（velocity）
* 价值（value）

大数据时代所面临的的问题：
* 数据量大，硬盘读写速度慢，还需要在这些数据上做复杂的分析工作
* 如何存储和计算大数据（分布式存储和分布式计算）
* 随着数据和机器的增加，处理数据的技术也变得越来越复杂，成本也高

大数据解决方案：
* 云计算服务商（如Amazon AWS、阿里云、腾讯云和 UCloud）
* 复杂性降低：不需要考虑容错性、并发等问题，在100台机器上编程和在1000台机器上编程没有太大区别
* 成本降低：IaaS（Infrastructure as a service，基础设施即服务）支持按天付费，而且可以动态按需扩容
* 在云计算服务商的帮助下，如今一个小创业公司都能够快速开发和部署大数据应用

数据存储的类型和时间范围：
* 一个字节价值 / 存储一个字节花费，如果比值大于 1，就值得存储更多数据
* 存储单条数据的价值可能不大，拥有更多数据时，就有价值了

如何对数据进行分析：
* Spark 的很多库就是为了解决不同场景下的分析任务而存在的
* MLlib库：解决各种机器学习问题，包括分类问题、回归问题、聚类等
* GraphX库：分析社交网络等应用

大数据交易：
* 数据本身已经成为一种可以交易的商品
* 促进大数据发展行动刚要剔除，要建设公共数据资源开放的统一开放平台，逐步实现信用、交通、医疗等民生保障服务相关领域的政数据集向社会开放
* 培育 10 家国际领先的大数据核心龙头企业，500 家大数据应用、服务和产品制造企业

谷歌的大数据解决方案：
* Hadoop：包括 MapReduce 与分布式文件系统（HDFS），已成为数据处理的事实标准
* 但 MapReduce 不是唯一的大数据分析范式，有一些场景不适用
* 比如处理网状的数据结构时，要求能处理顶点和边的增加和减少操作，并在所有节点进行运算，典型的场景就是在搜索引擎中链接地图计算和社交网络分析
* 谷歌也建设了基于图的计算系统 Pregel，允许连通的节点之间互相交换信息
* 如今图计算库 GraphX 已经是 Spark 主要的库之一

Hadoop 生态系统：
* Hadoop 是谷歌大数据解决方案的开源实现，使用 Java 语言开发，核心主要是 HDFS 和 MapReduce 两部分
* 在此基础上，经过业界巨头雅虎等，以及开源界其他力量的努力，建设了很多其他的重要系统

Hive：
* 在 HDFS 和 MapReduce 上提供一个类似于 SQL 风格的抽象层，非常容易上手
* 可以用数据库、表的概念来管理数据，使用 SQL 来访问、计算，不需要写 MapReduce 程序，SQL 语法非常类似于关系型数据库

HBase：
* 基于 Hadoop 的非关系型数据库
* 具备分布式、可拓展的特点
* 支持在几十亿行、数百万列的一张达标上进行实时、随机的读写访问

ZooKeeper：
* 提供分布式应用程序协调服务的系统
* 谷歌 Chubby 一个开源的实现
* Hadoop 和 HBase 的重要组件
* 比如 Spark 为了保证高可用，同时运行多台 Master 节点，但只有一台是活跃的，其他都处于热备状态
* 通过 ZooKeeper 可以协调选择出当前活跃的节点，当这个活跃节点异常时，再从剩下的热备节点中重新选择一台活跃节点

Hadoop 不适用场景：
* Hadoop 是一个批处理系统，不擅长实时计算，如果需要实时或者准实时的分析，可以使用 Storm（Twitter）、S4（雅虎）、Akka 等系统
* Hadoop 也不擅长复杂数据结构计算，比如前面提到的图计算，可以利用的开源系统有 GraphLab 和 Spark 的 GraphX 库

Hadoop 2.0：
* Hadoop YARN 将资源调度从 MapReduce 范式中分离出来
* YARN 已经成为通用的资源管理系统，可以为上层应用提供统一的资源管理和调度
* Spark 支持部署在 YARN 管理的集群上

Hadoop 应用门槛：
* 在工业界大规模应用 Hadoop 生态的系统，还要面临部署、排错、升级等问题
* 为解决这些问题，降低应用门槛，可以使用 Hadoop 商用解决方案的提供商的产品

MpaReduce 局限性：
* 机器学习算法通常需要对同一个数据集合进行多次迭代计算，而 MapReduce 中每次迭代都会涉及 HDFS 读写
* 以及缺乏一个常驻的 MapReduce 任务，因此每次迭代都需要初始化新的 MapReduce 任务
* 这是 MapReduce 就显得效率不高了，基于 MapReduce 之上的 Hive、Pig 等技术也存在类似问题

#### Spark 应时而生

Spark 的起源：
* 迭代算法
* 交互式查询

Spark 的发展：
* 刚出现时，常常被概括为内存计算
* 在典型应用中，Spark 读取 HDFS 中的文件，加载到内存，在内存中使用弹性分布式数据级来组织数据
* RDD 即弹性分布式数据级（Resilient Distributed Dataset）是 Spark 的核心数据结构
* RDD 可以重用，支持重复的访问，在机器学习的各个迭代中都会驻留内存，显著提升性能
* 即使是必须使用磁盘进行复杂计算的场景，Spark 也常常比 Hadoop MapReduce 更高效

Spark 的特点：
* Spark 是一个通用计算框架，包含了特定场景下的计算库：Streaming，SQL，MLlib，Graphx 等
* 除了支持常见的 MapReduce 范式，还能支持图计算、流式计算等复杂计算场景，很大程度上弥补了 Hadoop 的不足
* 此外，Spark 的接口丰富，提供了 Python、Java、Scala 等接口，文档清晰，为初学者提供了便利的上手条件
* Spark 在容错方面也有自己的特色，容错机制记录创建 RDD的一系列变换序列，如何从其他 RDD变换过来、如何重建某一个数据的信息
* 由于 Spark 只允许进行粗粒度的 RDD 转换，所以其容错机制相对高效，Spark 也支持检查点（checkpoint）的容错机制
* Spark 自带调度器，同时能够运行在 Hadoop YARN 集群、Apache Mesos 上，可以很方便地和现有集群进行融合
* Spark 的输入支持本地存储、Hadoop 的 HDFS，以及其他支持 Hadoop 接口的系统：S3、Hive、HBase 等
* Spark 还有一个优点，当 RDD 的大小超过集群的所有内存时，可以优雅地进行降级支持，存储在磁盘
* 从成本的角度出发，由于 Spark 能适应多种应用场景，公司不需要部署多套大数据处理系统，可大大减低学习、维护、部署、支持等成本

Spark 的未来发展：
* Spark 的 contributor 呈指数级增长
* Spark 峰会上，各大机构共分享了近百个精彩纷呈的报告
* Spark 主要贡献者包括 Databricks、英特尔、雅虎、加州大学伯克利分校
* Spark 1.4 中发布了 SparkR，引入了更友好的 R 语言支持，进一步扩展了 Spark 数据分析的应用场景
* 从 Spark 官方提供的用户列表可以看到，国内的 BAT 都在用 Spark
* Spark 已得到广泛认可，成为一种优秀的大数据处理技术平台


### 第2章 Spark 基础

#### Spark 本地单机模式体验

Spark 单机模式和集群比较：
* Spark 提供了本地单机部署的体验模式
* 与真实集群相比，最大差异只是计算资源较少，但用来学习或体验足够了
* 考虑到 Spark 真实的部署环境绝大部分都在 Linux 下，而且 Linux 强大的 shell 功能操作起来更方便

本地单机部署的步骤：
* 安装 Linux 虚拟机（Ubuntu 操作系统）
* 安装 JDK（Spark 的运行依赖 JVM，Spark 使用 Scala 语言开发，像 Java 一样运行在 JVM 上，完美融合了面向对象编程与函数式编程）
* 下载 Spark 预编译包
* 本地体验 Spark

```
// 进入 Scala 交互式编程模式：--master 指定服务器地址，local 表示运行在本地，[2] 为可选项，表示启动两个工作线程
> ./bin/spark-shell --master local[2]

// 启动后，可以看到欢迎界面输出，同时 Master 会启动一个 HTTP Web 服务，方便查看 Spark 的工作状态
// 浏览器输入 <LocalIP>:4040 访问 web 页面

// 1、Hello, world
object HelloWorld {
    def main(args: Array[String]) {
        println("Hello, world!")
    }
}

// 输出结果
defined module HelloWorld
scala> HelloWorld.main(null)
Hello, world!

// 2、从本地文本文件创建一个 RDD
scala> val textFile = sc.textFile("README.md")

// 3、RDD 支持的方法，不同于普通数据集，主要分为两类：
// 转换(Transformation)：生成新的 RDD
// 动作(Action)：返回一个非 RDD 类型的值，视方法而不同

// 动作操作：
scala> textFile.count()  // 返回 RDD 元素的个数
scala> textFile.first()  // 取 RDD 的第一个元素

// 转换操作：
scala> val linesWithSpark = textFile.filter(line => line.contains("Spark"))
scala> linesWithSpark.count()  // 统计文本中包含 Spark 的行数是多少

// 把两次调用合并起来写
scala> textFile.filter(line => line.contains("Spark")).count()
```

#### 高可用 Spark 分布式集群部署

单机模式特点：
* 磁盘、内存、CPU 资源有限
* 单点，无法满足企业对可用性方面的高要求

集群特点：
* 高可用
* 全集群没有单点，任何一台机器宕机时，集群依然可以正常工作

集群总览：
* 核心：Spark
* 高可用性：ZooKeeper 进行主备切换
* 存储：Hadoop HDFS
* 拓展性：Hadoop YARN

  Spark
HDFS YARN
ZooKeeper

ZooKeeper 集群：
* 集群的机器数量为 2n+1 台，可以抵御 n 台机器宕机的风险
* 所有节点都是相同的角色，但运行中会自动选择一个作为 leader
* 负责以下集群的主备切换：
* Spark 集群的 Master 节点的主备切换
* Hadoop 集群中 HDFS 文件系统的 NameNode 节点主备切换
* YARN 中的 ResourceManager 主备切换

   zk1
   / \
zk2---zk3

Spark 集群：
* 分为两类节点：Master 节点和 Slave 节点
* Master 节点：负责集群的资源管理，只要有一台集群就可以工作，但是一般至少两台，其中一台作为主节点，其他热备
* 当主 Master 节点异常下线时，其他节点通过 ZooKeeper 集群竞争选出新的主节点
* Slave 节点：用于执行计算任务，机器数量可以是一台，也可以是几十上千台

ZooKeeper集群--Master（主）--Slave 1
             \Master（备） \Slave 2
                          \Slave N

Hadoop 集群：
* 存储和计算分离，形成两个相对独立的子集群：HDFS 和 YARN，MapReduce 依附于 YARN 来运行
* HadoopHDFS：分布式文件系统，为集群提供大文件的存储能力
* NameNode 管理所有的文件信息，一主一备，通过 ZooKeeper 来实现容灾切换，QJM 负责记录 NameNode 的流水日志
* DataNode 担负集群的存储负载，用于数据存储

               /  NameNode（主）--DataNode 1
              /  /   |   \      \DataNode 2
ZooKeeper集群  QJM1-QJM2-QJM3    \DataNode N
              \  \   |   /
               \  NameNode（备）

Hadoop YARN：
* 为MapReduce 计算提供调度服务，而且也可以为其他符合 YARN 接口要求的集群提供，这也是 Spark 可以借助 YARN 来做资源调度的前提
* 有两类节点：ResourceManager 和 NodeManager，其结构和 Spark 集群非常类似
* ResourceManager：一主多备
* NadeManager：一般和 DataNode 部署在一起，实现高效的数据访问

ZooKeeper集群--ResourceManager（主）--NodeManager 1
             \ResourceManager（备） \NodeManager 2
                                   \NodeManager N

集群机器的型号选择：
* 集群各节点的工作性质不同，对硬件的消耗也不同，所以合理地搭配可以让资源得到更充分的利用
* 离据越紧，移动的开销越小
* 在大数据计算的大部分场景下，移动数据的开销都会大于计算的开销
* 所以计算节点和存储节点一般都会混合部署

-----------------------------------------------------------
集群          |    节点        | 主要工作            | 资源消耗
ZooKeeper    |ZooKeeper      |多个节点竞选 leader   | 不明显
             |               |存储配置信息          | 不明显
Spark        |Master         |进行调度管理          | 消耗少量内存
             |Slave          |进行任务计算          | 消耗 CPU，内存越多越好
Hadoop HDFS  |NameNode       |文件元数据存储、访问   | 内存越大支持的文件数量越多
             |DataNode       |数据存储             | 磁盘越大存的越多
             |QJM            |文件元数据日志存储     | 少量存储
Hadoop YARN  |ResourceManager|调度管理             | 少量内存
             |NodeManager    |Hadoop MapReduce计算|CPU、内存，但Spark程序不需要

#### Spark 编程指南

RDD 计算过程：
* 在 Driver 节点上序列化代码
* 传送至各个计算节点，RDD 在各个节点上进行计算
* 在计算节点上反序列化
* 执行

```
// 1、进入交互式编程环境
> ./bin/spark-shell

// 输出：
> Spark context availabel as sc

// WorkCount
scala> val textFile = sc.textFile("README.md")
scala> val wordCounts = textFile.flatMap(line => line.split(" "))
                                .map(word => (word, 1))
                                .reduceByKey((a, b) => a + b)
scala> wordCounts.collect()

// 2、RDD 创建
// 从 Driver 程序的数据集生成 RDD
// 受到所在节点的资源限制，不适合处理特别大的数据，特别是接近或超过本机资源的数据
scala> val data = Seq(1, 2, 3, 4, 5)    // 从 Seq 对象生成 RDD
scala> val data = Array(1, 2, 3, 4, 5)  // 从 Array 对象生成 RDD
scala> val data = List(1, 2, 3, 4, 5)   // 从 List 对象生成 RDD
scala> val rdd = sc.parallelize(data)

// 从外部数据集生成 RDD
scala> val textFile = sc.textFile("README.md")

// 3、RDD 操作
// 转换：一个 RDD 经过计算后生成新的 RDD，比如上面的 flatMap、map、reduceByKey
// 动作：结果返回到 Driver 程序中，一般意味着 RDD 计算的结束，比如上面的 collect
// 所有的转换都是 lazy 模式，不会马上进行计算，而是先记录下计算方式，仅在动作被触发需要返回结果时才会启动计算
// 这样的好处是更高效，不需要将每次转换的非常大的结果返回给 Driver 程序，只需要返回最终结果
// 但是可以对 RDD 进行持久化或 cache 操作，这样也会触发转换进行真正的计算，再次访问时不需要重复计算

// WordCount
scala> val textFile = sc.textFile("README.md")
scala> val words = textFile.flatMap(line => line.split(" "))
scala> val wordPairs = words.map(word => (word, 1))
scala> val wordCounts = wordPairs.reduceByKey((a, b) => a + b)
scala> wordCounts.collect()  // 只有这里会马上执行，前面都没有

// 传递函数参数
// 匿名函数：适用于小片段的代码，比如上面的 WordCount
// 传递 object 对象中的静态方法

// flatMap 等价形式
object MyFunctions {
    def lineSplit(line: String): Array[String] = {
        line.split(" ")
    }
}
val words = textFile.flatMap(MyFunctions.linesplit)
scala> val words = textFile.flatMap(line => line.split(" "))  // 两者等价

// map 等价形式
import org.apache.spark.rdd.RDD
class MyClass extends java.io.Serializable {
    def wordMap(word: String): (String, Int) = { (word, 1) }
}
val myclass = new MyClass()
val wordPairs = word.map(myclass.wordMap)
scala> val wordPairs = words.map(word => (word, 1))  // 两者等价

// 变量的作用域
// 计数正确
var counter = 0
val data = Seq(1, 2, 3)
data.foreach(x => counter += x)
println("Counter value: " + counter)

// 将 data 转换成 RDD，再来重新计数，计数结果为 0
// 因为计算都不在本地，每个 RDD 操作都会被转换成 Job 分发至集群的执行器进程上运行，与 Driver 属于不同进程
// 在执行过程中，只是计算节点上的计数器会自增，而 Driver 程序中的变量则不会发生变化
var counter = 0
val data = Seq(1, 2, 3)
var rdd = sc.parallelize(data)
rdd.foreach(x => counter += x)  // 该匿名函数先被序列化，然后传送至计算节点，反序列化之后再运行
println("Counter value: " + counter)

// 4、以键值对为参数，比如上面的 reduceByKey

// 5、RDD 的转换
map(func)  // 对源 RDD 的每个元素调用 func，生成新的元素，构成新 RDD 并返回
flatMap(func)  // 与 map 类似，但每个输入的 RDD 成员可以产生 0 个或多个输出成员
filter(func)  // 对 RDD 用 func 进行过滤，返回新 RDD
mapPartitions(func)  // 与 map 类似，但是作用于 RDD 的整个分区
mapParttionsWithIndex(func)
sample(withReplacement, fraction, seed)  // 抽样
union(otherDataset)  // 不去重地合并两个 RDD，要求元素类型一致
distinct([numTasks])  // 去重
groupByKey([numTasks])  // 聚合
reduceByKey(func, [numTasks])  // 对键值对进行聚合
sortByKey([ascending], [numTasks])  // 排序
join(otherDataset, [numTasks])  // 关联

// 6、RDD 的动作：在转换后执行，输出不再是 RDD，也就意味着输出不是分布式了，而是回送至 Driver 程序
reduce(func)  // reduce
collect()  // 将 RDD读取至 Driver 程序，类型是 Array，一般要求 RDD 不能太大
count()  // 返回 RDD 的成员数量
first()  // 返回 RDD 的第一个成员，等价于 take(1)
take(n)  // 返回 RDD 前 n 个成员
saveAsTextFile(path)  // 将 RDD 转换为文本内容并保存至路径下
saveAsSequenceFile(path)
countByKey()  // 对 key 技术，返回 (K, Int)
foreach(func)  // 对 Rdd 每个成员执行 func，没有返回值
```

#### 打包和提交

```
// 1、将 Spark 程序代码保存到文件中
/* WordCount.scala */
import org.apache.spark.SparkContext
import org.apache.spark.SparkContext._
import org.apache.spark.SparkConf
object WordCount {
    def main(args: Array[string]) {
        val conf = new SparkConf().setAppName("WordCount")
        val sc = new SParkContext(conf)
        val textFile = sc.textFile("README.md")
        val words = textFile.flatMap(line => line.split(" "))
        val wordPairs = word.map(word => (word, 1))
        val wordCounts = wordPairs.reduceByKey((a, b) => a + b)
        println("wordCounts: ")
        wordCounts.collect().foreach(println)
    }
}

// 2、sbt
// 编写sbt配置文件
// 编译、链接、打包
$ bin/sbt package

// 3、提交
$ ./bin/spark-submit \
>    --class <main-class> \
>    --master <master-url> \
>    --deploy-mode <deploy-mode> \
>    --conf <key>=<value> \
>    <application-jar> \
>    [application-arguments]

// 例如
$ ./bin/spark-submit \
>    --class org.apache.spark.examples.SparkPi \
>    --master local[2] \
> lib/spark-examples-1.4.1-hadoop2.6.0.jar \
> 10
```

### 第3章 Spark 工作机制

#### 调度管理