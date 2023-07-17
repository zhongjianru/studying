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
// 在序列化时，Spark 会将 Job 运行所依赖的变量、方法（称为闭包）全部打包在一起序列化，相当于它们的一份副本
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

##### 集群概述及名词解释

集群模式下的 Spark 程序运行：
            -------------工作节点--执行器+缓存
Driver 程序  --集群管理器/\
SparkContext-------------工作节点--执行器+缓存

Driver 程序：
* 用户编写的 Spark 程序
* 每个 Driver 程序包含一个代表集群环境的 SparkContext 对象并与之连接
* 程序的执行从 Driver 程序开始，中间会调用 RDD 操作，通过集群资源管理器来执行调度
* 操作一般在 Worker 节点上执行，所有操作执行结束后回到 Driver 程序中并结束

SparkContext 对象：
* 每个驱动程序里都有一个，担负着与集群沟通的职责
* SparkContext 对象联系集群管理器（dluster manager）分配 CPU 和内存等资源
* 集群管理器在工作节点（Worker Node）上启动一个执行器（专属于本驱动程序）
* SparkContext 分发粪污（Task）至各执行器执行

集群管理器：
* Standalone 模式：资源管理器是 Master 节点
* Hadoop YARN 模式：资源管理器是 YARN 集群（非常适合多个集群同时部署的场景，是目前最主流的资源管理系统）
* Apache Mesos：资源管理器是 Mesos 集群（高仿谷歌内部的资源管理系统 Borg 实现的）

涉及名词解释：
* Allocation：分配
* App/Application/Spark程序：泛指用户编写的运行在 Spark 上的程序（不仅是 Scala 语言，其他支持的语言也是）
* 节点/Worker节点：集群上的计算节点，一般对应一台物理机，尽在测试时会在一台物理机器上启动多个节点
* Worker：每个节点上会启动一个进程，负责管理本节点，运行 jps 命令可以看到 Worker 进程在运行
* core：Spark 标识 CPU 资源的方式，对应一个或多个物理 CPU 核心，每个 Task 运行时至少需要一个 core
* 执行器：每个 Spark 程序在每个节点上启动的一个进程，专属于一个 Spark 程序，负责在该节点上启动的一个 Task
* Job：一次 RDD Action 对应一个 Job，会提交至资源管理器调度执行
* Stage：Job 在执行过程中被分为多个阶段，介于 Job 和 Task 之间，是按 Shuffle 分隔的 Task 集合
* Task：在执行器执行的最小单元。比如 RDD Tranformation 操作时对 RDD 内每个分区的计算都会对应一个 Task

##### Spark 程序之间的调度

* 静态分配：Spark 程序启动时即一次性分配所有资源，运行过程中固定不变，直到程序退出
* 动态分配：运行过程中不断调整分配的资源，可以按需增加或减少（比静态分配复杂得多，需要在实践中不断调试）

静态资源分配：
* 所有的集群管理器都支持静态分配
* 每个 Spark 程序都分配一个最大可用的资源数量，而且在程序运行的整个过程中都持有它
* Standalone 模式：按照先进先出（FIFO）的顺序执行程序
* Mesos：限制每个进程的最大 core 使用量和内存的使用
* YARN：限制最多分配的执行器的数量

动态资源分配：
* 分配给 Spark 程序的资源可以增加或减少，意味着程序可能会在不适用资源的时候将资源还给集群，需要时再从集群申请
* 动态资源分配的粒度是执行器，即增加或减少执行器，意味着为 Spark 程序服务的节点数据量的增加或减少
* 这个特性当前只有在 YARN 模式下才可以使用

资源分配策略：
请求策略：
* 开启动态资源分配之后，当 Spark 程序有 Task 初遇排队待调度时，会请求额外的执行器资源
* 说明现有的执行器不足以让所有尚未执行完的 Task 的并发达到饱和
* 实际请求的触发是在当排队 Task 已经等待了 Timeout 秒后，只要还有排队的 Task，就每隔 Timeout 秒请求一次
* Spark 会一致循环请求执行器资源，每一轮请求的执行器总数量会呈指数增长，1、2、4、6、8 以此推类
* 指数增长策略的原因：刚开始时申请的执行器应该谨慎一些，如果程序真的需要放多执行器，应该可以及时获取到

移除策略：
* 当 Spark 程序的某个执行器处于空闲状态的时间超过 Timeout 秒，之后就会被移除
* 在大部分情况下，这个条件与请求的条件是互斥的，即当有 Task 在等待执行时，是没有空闲执行器的

执行器的优雅退出：
* Spark 需要一种在执行器优雅退出之前保存执行器状态的机制
* 这个需求对于 shuffle 特别重要
* 在 shuffle 期间，Spark 执行器首先将 map 结果写入本地磁盘，作为一个服务来运行，其他执行器请求访问这些数据时进行响应
* 但是对于哪些执行时间远超过同伴的 Task 来说，动态分配可能会在 shuffle 结束之前移除这个执行器，这样就需要无谓的重新计算
* 保存 shuffle 文件的方法是使用一个外部的 shuffle 服务，指向一个长期运行的进程，在集群的每个节点上都会独立运行
* Spark 执行器会从这些服务获取 shuffle 文件，而不是其他执行器，意味着执行器写的任何 shuffle 状态在执行器退出之后还可以保留

##### Spark 程序内部的调度

* 不同线程提交的 Job 可以并行执行，Spark 调度器是线程安全的，可以支持这种需要同时处理多个请求的服务型应用
* 默认情况下：Spark 调度器以 FIFO 的方式运行 Job，每个 Job 分成多个 Stage（比如 map 和reduce 阶段）
* 在公平共享方式下：Spark 采用循环的方式为不同 Job 之间的 Task 分配资源，这样所有的 Job 可以获取差不多相同的资源
* 这就意味着，当有长时间的 Job 在运行时，短时间的 Job 在提交之后也可以马上运行，而不用等长时间 Job 结束，特别适合多用户场景

公平调度池：
* 支持对多个 Job 进行分组，这个分组称为调度池，每个调度池可以设置不同的调度选项
* 可以为不同的用户设置不同的调度池，然后让各个资源池平等地共享资源，而不是按 Job 来共享资源
* 不做设置的话，新提供的 Job 会自动进入默认调度池，也可以指定 Job 进入哪个调度池

调度池的默认行为：
* 默认情况下，所有调度池平均共享集群的资源
* 但在每个调度池内部，各个 Job 是按照 FIFO 的顺序来执行的

```
// Standalone 模式属性
spark.cores.max
spark.deploy,defaultCores
spark.executor.memory

// Mesos 属性
spark.mesos.coarse=true
spark.cores.max
spark.executor.memory

// YARN 属性
--num-executors
--executor-memory
--executor-cores

// 动态共享 CPU 核心资源
spark.executor
spark.mesos.coarse=true

// 动态资源分配
spark.dynamicAllocation.enabled=true
spark.shuffle.service.enabled=true

// 公平共享资源调度
spark.scheduler.mode=FAIR

// 设置调度池（按线程设置，可以让线程下的所有 Job 都在同一个用户下）
spark.scheduler.pool=pool1

// 清空当前线程的调度池设置
spark.scheduler.pool=null

// 调度池配置文件恩建
<?xml version="1.0"?>
<allocations>
  <pool name="production">
    <schedulingMode>FAIR</schedulingMode>
    <weight>1</weight>
    <minShare>2</minShare>
  </pool>
  <pool name="test">
    <schedulingMode>FIFO</schedulingMode>
    <weight>2</weight>
    <minShare>3</minShare>
  </pool>
</allocations>
```

#### 内存管理

相比 Hadoop MapReduce 来说，Spark 具有巨大的性能优势：
* Spark 对于内存的充分利用
* 提供的缓存机制

RDD 持久化：
* 持久化早起被称为缓存，但缓存一般指将内容放在内存中
* 在内存不够时可以用磁盘顶上去，也可以选择不使用内存，而是仅仅保留到磁盘中
* 如果一个 RDD 布置一次备用到，就可以持久化它，这样可以大幅提升程序的性能，避免重复计算
* 这也是 Spark 刚出现时被称为内存计算的原因

共享变量原因：
* Spark 程序的大部分操作都是 RDD 操作，通过传入函数给 RDD 操作函数来计算
* 这些函数在不同的节点上并发执行，内部的变量有不同的作用于，不能互相访问

共享变量分类：
* 广播变量：只读（创建之后再更新值是没有意义的）
* 计数器：只能增加，只有 Driver 程序可以读计算器变量（RDD 操作中读取是无意义的）

```
// 持久化
RDD.persisi()

// 删除持久化
RDD.unpersist()

// 创建广播变量
scala> val broadcastVar = sc.broadcast(Array(1, 2, 3))
scala> broadcastVar.value  // 输出值

// 创建计数器
scala> val accum = sc.accumulator(0, "My Accumulator")
scala> sc.parallelize(Array(1, 2, 3, 4)).foreach(x => accum += x)
scala> accum.value  // 输出值
```

#### 容错机制

分布式系统通常在一个机器集群上运行，同时运行的几百台机器中出问题的概率很大，所以容错设计是分布式系统的一个重要能力。

容错体系概述：
* MapReduce 将计算转换为一个有向无环图（DAG）的任务集合，这样可以通过重复执行 DAG 里的一部分任务来完成容错恢复
* 但是由于主要的数据存储在 HDFS 中，容错过程需要在网络上进行数据复制，从而增加了大量的消耗
* 所以，分布式编程中经常需要做检查点，即将某个时机的中间数据写到存储（通常是 HDFS）中

Spark 容错体系：
* RDD 也是一个 DAG，每一个 RDD 都会记住创建该数据集需要哪些操作，跟踪记录 RDD 的继承关系（lineage）
* 创建 RDD 的操作是相对粗粒度的变换，即单一的操作应用于许多数据元素，不需要存储真正的数据（比通过网络复制数据更高效）
* 当一个 RDD 的某个分区丢失时，RDD 有足够的信息记录其如何通过其他 RDD 进行计算，且只需重新计算该分区

RDD 之间的依赖：
* 窄依赖：父分区对应一个子分区（只需要通过重新计算丢失的那一块数据来恢复，容错成本较小）
* 宽依赖：父分区对应多个子分区（父分区数据只有一部分是需要重算子分区的，其余数据重算造成冗余计算）
* 所以，不同的应用有时候也需要在适当的时机设置数据检查点，RDD 的之独特性使它比常用的共享内存更容易做检查点
* Kafka 和 Flume 这样的数据源，接收到的数据只在数据被预写到日志以后，接收器才会受到确认消息
* 这样，所有的数据要不从日志中恢复，要不由数据源重发，实现了零丢失

Spark Master的容错：
* Standalone 集群模式：通过 ZooKeeper 来完成，即有多个 Master，一个角色是 Active，其余角色是 Standby
* 单点模式：当 Master 进程异常时，重启 Master 进程并从错误中恢复

Slave 节点失效：
* Worker 异常退出：现将自己启动的执行器停止，Driver 需要有响应的程序来重启 Worker 进程
* 执行器异常退出：Driver 会将注册的执行器删除，Worker 受到指令，再次启动执行器
* Driver 异常退出：一般使用检查点重启 Driver，重新构造上下文并重启接收器

#### 监控管理

* Web 界面 / REST API
* Metrics
* 外部系统

#### Spark 程序配置管理

* 环境变量
* 配置文件
* 命令行参数
* 直接在 Spark 程序中指定
* 配置 Spark 日志

不同的配置方式有不同的优先级，可以相互覆盖，而且这些配置属性在 Web 界面中可以直接看到。

### 第4章 Spark 内核讲解

#### Spark 核心数据结构 RDD

RDD 含义：
* 弹性分布式数据集（Resilient Distributed Dataset）

分布存储：
* RDD 里面的成员被水平切割成小的数据块，分散在集群的多个节点上，便于对数据进行并行计算

弹性：
* RDD 分布是弹性的，不是固定不变的
* 操作：有些操作可以被拆分成对各个数据块直接计算，不涉及其他节点，比如 map，但是有些操作比如访问所有数块
* 高可靠性

只读：
* RDD 一旦生成，内容就不能修改了
* 好处是让整个系统的设计相对简单，比如并行计算时不用考虑数据互斥的问题

缓存：
* RDD 可指定缓存在内存中
* 一般计算都是流水式生成和使用 RDD，新的 RDD 生成后，旧的不再使用，并被 Java 虚拟机回收掉
* 但如果后续有多个计算依赖某个 RDD，可以将 RDD 花村在内存中，避免重复计算

计算：
* RDD 可以通过计算重新得到
* RDD 的高可靠性不是通过复制来实现的，而是通过记录足够的计算过程，在需要时（比如因为节点故障导致内容失效）重新计算来恢复的

RDD 核心属性：
* 一个分区列表：每个分区里是 RDD 的部分数据（或称数据块）
* 一个依赖列表：存储依赖的其他 RDD
* 一个计算函数：名为 compute，用于计算 RDD 各分区的值
* 分区器（可选）：用于键值类型的 RDD，比如某个 RD 是按三列来分区
* 计算各分区时优先的位置列表（可选）：比如从 HDFS 上的文件生成 RDD 时，优先选择数据所在的节点，可以避免数据移动带来的开销

```
// 1、分区
// 依赖关系定义在一个 Seq 数据集中，类型是 Dependency
// 有检查点时，这些信息会被重写，指向检查点
private var dependencies_ : Seq[Dependency[_]] = null

// 2、依赖
// 分区定义在 Array 数据中，类型是 Partition，没用 Seq，主要是考虑到随时通过下标来访问或更新分区内容
// 而 dependencies_ 使用 Seq 是因为它的使用场景以便是取第一个成员或遍历
@transient private var partiitons_ : Array[Partition] = null

// 3、计算函数：由子类来实现，对输入的 RDD 分区进行计算
def compute(split: Partition, context: TaskContext): Iterator[T]

// 4、分区器：子类可以重写以指定新的分区方式
// Spark 支持两种分区方式：Hash 和 Range
@transient var partitioner: Option[Partitioner] = None

// 5、优先计算位置：子类可以指定分区优先的位置，让分区尽可能与数据在相同的节点上
protected def getPreferredLocations(split: Partition): Seq[String] = Nil
// RDD 提供统一的调用方法，统一处理检查点问题
final def preferredLocations(split: Partitions): Seq[String] = {
    checkpointRDD.ma(_.getPreferedLocations(split)).getOrElse {
        getPreferredLocations(split)
    }
}

// 6、RDD 的 Transformation
// 每个 Transformation 操作都会生成一个新的 RDD，不同操作也可能范返回相同类型的 RDD，只是计算方法等参数不同
// 比如，map、flapMap、filter 这三个操作都会生成 MapPartitionsRDD 类型的 RDD

// Transformation: map
def map[U: ClassTag](f: T => U): RDD[U] = withScope {
    var cleanF = sc.clean(f)
    new MapPartitionsRDD[U, T]
}

// Transformation: flatMap
def flatMap[U: ClassTag](f: T => TraversableOnce[U]): RDD[U] = withScope {
    val cleanF = sc.clean(f)
    new MapPartitionsRDD[U, T](this, (context, pid, iter) => iter.flatMap(cleanF))
}

// Transformation: filter
def filter(f: T => Boolean): RDD[T] = withScope {
    val cleanF = sc.clean(f)
    new MapPartitionsRDD[T, T](
        this,
        (context, pid, iter) => iter.filter(cleanF),
        preservesPartitioning = true
    )
}
```

RDD 的 Transformation：
* 是指由一个 RDD 生成新 RDD 的过程
* 所有的 RDD Transformation 都只是生成了 RDD 之间的计算关系和计算方法，没有进行真正的计算
* 结合每一个 RDD 的数据和它们之间的依赖关系，每个 RDD 都可以按依赖链追溯它的祖先，这些依赖链接就是 RDD 重建的基础

RDD 依赖关系类型：
* 窄依赖（NarrowDependency）：依赖上级 RDD 的部分分区
* Shuflle 依赖（ShuffleDenpendency）：依赖上级 RDD 的所有分区

窄依赖：
* 使用窄依赖时，可以精确直到依赖的上级 RDD 的分区
* 一般情况下，会选择与自己在同一节点的上级 RDD 分区，这样计算过程都在同一节点进行，没有网络 IO 开销，非常高效
* 常见的 map、flatMap、filter 操作都是这一类

Shuffle 依赖：
* 无法精确定位依赖的上级 RDD 的分区，相当于依赖所有分区
* 比如 reduceByKey 计算，需要对手游的 key 重新排列
* 计算式设计所有节点之间的数据传输，开销巨大
* 以 Shuffle 依赖为分隔，Task 被分为 Stage，方便计算时的管理

检查点：
* 如果依赖链条太长，通过计算来恢复 RDD 的代价就太大了
* 对于依赖链条太长的计算，对中间结果存一份快照，这样就不需要从头开始计算了
* 比如 Spark Streaming 流式计算，程序需要 7*24 小时运行，依赖链接会无穷扩充
* 如果没有检查点机制，容错将完全没有意义

RDD 的 Action：
* 代表计算的结束，调用之后不再生成新的 RDD，结果返回到 Driver 程序
* Transformation 只是建立计算关系，而 Action 才是实际的执行者
* 每个 Action 都会调用 SparkContext 的runJob 方法向集群正式提交请求，所以每个 Action 对应一个 Job
* 
```
// Action 操作是不可以在 RDD Transformation 内部调用的
rdd1.map(x => rdd2.values.count() * x)

// 返回 RDD 中的元素数 RDD
def count(): Long = sc.runJob(this, Utils.getIteratorSize _).sum
```

Shuffle：
* 当对一个 RDD 的某个翻去进行操作，而无法精确直到依赖前一个 RDD 的哪些分区时，变成了依赖前一个 RDD 的所有分区
* 在进行 reduce 操作之前，Spark 可能分布在不同的机器节点上，此时需要先把它们汇聚到一个节点，这个汇聚过程就是 Shuffle
* Shuffle 非常消耗资源，除了会涉及大量网络 IO 操作并使用大量内存外，还会在磁盘上生成大量临时文件，以避免错误恢复时重新计算
* Shuffle 操作的结果启示是一次调度的 Stage 的结果，而一次 Stage 包含许多 Task，缓存下来还是很划算的

#### SparkContext
* Spark 程序最主要的入口，用于与 Spark 集群连接，与 Spark 集群的所有操作都通过 SparkContext 来进行
* 可以在 Spark 集群上创建 RDD、计数器以及广播变量
* 所有的 Spark 程序都必须创建一个 SparkContext 对象（关联现有对象或隐式创建一个对象）
* 每个 JVM 只允许启动一个 SparkCOntext，否则默认会抛出异常，可以用 sc.stop() 方法先停止默认的 sc

SparkContext 功能接口：
* 初始化环境，连接 Spark 集群
* 创建 RDD
* RDD 持久化
* 创建共享变量：包括计数器和广播变量
* stop()：停止
* runJob：提交 RDD Action 操作，这是所有执行调度的入口

DAG 调度：
* 最高层级的调度
* 为每个 Job 回执出一个有向无环图（简称 DAG），跟踪各 Stage 的输出，计算完成 Job 的最短路径，并将 Task 提交给 Task 调度器来执行
* Task 调度器只负责接受 DAG 调度器 的请求，负责 Task 的实际执行调度，所以 DAGScheduler 的初始化必须在 Task 调度器之后

DAG 与Task 分离调度的好处：
* Spark 可以灵活设计自己的 DAG 调度，同时还能与其他资源调度系统结合，比如 YARN、Mesos

```
// 1、初始化 SparkContext 对象时，只需要一个 SparkConf 配置对象作为参数即可
class SparkContext(config: SparkConf)
def this() = this(new SParkConf())

// 直接设置常用属性
def this(
    master: String,  // 集群地址
    appName: String,  // 程序名
    sparkHome: String = null,  // Spark 在机器上的安装目录
    jars: Seq[String] = Nil,  // 给集群添加额外的 Jar 文件集合，可以是本地文件或者是 HDFS 等类型的URL
    environment: Map[String, String] = Map(),  // 环境变量
    perferredodeLocationData: Map[String, Set[SplitInfo]] = Map()) = {
        this(SparkContext.updateConf(new SparkConf(), master, appName, sparkHome, jars, environment))
        this.preferredNodeLocationData = preferredNodeLocationData
    }
)

// 保存配置的 SparkConf 类的定义在相同目录下的 SparkConf.scala 文件中
private val settings = new COncurrentHashMap[String, String]()

// 所有的配置都以键值对的形式保存在 settings 中，比如设置 Master 方法启示就是设置了配置项 spark.master
// 所以，使用配置文件中的配置项，或是参数列表中的 --master选项，或是setMaster() 方法都可以，只是优先级不同
def setMaster(master: String): SparkConf = {
    set("spark.master", master)
}

// 2、初始化操作：除了初始化各类配置、日志之外，还要启动 Task 调度器和 DAG 调度器
// 创建并启动 Task 调度器
val (sched, ts) = SparkContext.createTaskScheduler(this, master)
_schedulerBackend = sched
_taskScheduler = ts
_dagScheduler = new DagScheduler(this)
_heartbeatReceiver.send(TaskSchedulerIsSet)

// 创建 DAG 调度器，并引用之前创建的 Task 调度器之后，再启动 Task 调度器
_taskScheduler.start()
```

#### DAG 调度
SparkContext 在初始化时，创建了 DAG 调度和 Task 调度来负责 RDD Action 操作的执行调度

DAGScheduler：
* 负责 Spark 的最高级别的任务调度，调度的粒度是 Stage
* 为每个 Job 的所有 Stage 计算一个有向无环图，控制它们的并发，并找到一个最佳路径来执行它们
* 具体过程是将 Stage 下的 Task 集提交给 TaskScheduler 对象，由它来提交到集群上去申请资源并完成执行

任务处理过程：
* runJob 调用 summitJob 提交任务
* submitJob 生成新的 JobId，发送消息 JobSubmitted
* DAG 收到该消息，调用 handleJobSubmitted 来处理
* 创建一个 ResultStage，并使用 submitStage 来提交这个 ResultStage

Spark 的执行过程是懒惰（lazy）的：
* 任务提交时，不是按 Job 的先后顺序提交的，而是倒序的
* 每个 Job 最后一个操作是 Action 操作
* DAG 把这最后的操作当做一个 Stage 首先提交，然后逆向逐级递归填补缺少的上级 Stage
* 从而生成一棵实现最后 Action 操作的最短的（因为都是必须的）有向无环图，然后再重头开始计算

Stage：
* 仅对依赖类型是 ShuffleDependency 的 RDD 操作创建 Stage，其他的 RDD 操作并没有创建
* RDD 操作有两类：窄依赖和 Shuffle 依赖
* DAG 在调度时，对于在相同节点上进行的 Task 计算，会合并为一个 Stage
* 各 Stage 之间以 Shuffle 为分界线

生成新的 Stage：
* 依赖类型是 Shuffle 的 Transformation 操作（基于所有的 ByKey 操作都是，比如 reduceByKey 和 groupByKey）
* Action 操作，为了生成默认的 Stage，这样即使没有 Shuffle 类操作，保证至少有一个 Stage

```
private[spark]
class DAGScheduler(
    private[scheduler] val sc: SparkContext,
    private[scheduler] val taskScheduler: TaskScheduler,
    listenerBus: LiveListenerBus,
    mapOutputTracker: MapOutputTrackerMaster,
    blockManagerMaster: BlockManagerMaster,
    env: SparkEnv,
    clock: Clock = new SystemClock()
)
extends Logging {
    ...
}

def runJob[T, U](
    rdd: RDD[T],
    func: (TaskContext, Iterator[T]) => U,
    partitions: Seq[Int],
    callSite: CallSite,
    allowLocal: Boolean,
    resultHandler: (Int, U) => Unit,
    properties: Properties): Unit = {
        val start = System.nanoTime
        val waiter = submitJob(rdd, func, partitions, callSite, allowLocal, resultHandler, properties)
        waiter.awaitResult() match {
            case JobSucceeded =>
                logInfo(
                    "Job %d finished: %s, took %f s".
                    format(waiter.jobId, callSite.shortForm, (system.nanoTiem-start)/1e9))
            case JobFailed(exception: Exception) =>
                logInfo(
                    "Job %d finished: %s, took %f s".
                    format(waiter.jobId, callSite.shortForm, (system.nanoTiem-start)/1e9))
            throw exception
        }
    }
)

private def submitStage(stage: Stage) {
    val jobId = activeJobForStage(stage)
    if(jobId.isDefined) {
        logDebug("submitStage(" + stage + ")")
        if(!waitingStages(stage) && !runningStages(stage) && !failedStages(stage)) {
            val missing = getMissingParentStages(stage).sortBy(_.id)
            logDebug("missing: " + missing)
            if(missing.isEmpty) {
                // 仅在所有缺失的父Stage都提交执行了，才开始提交自己
                logInfo("Submitting " + stage + " (" + stage.rdd + "), which has no missing parents")
                submitMissingTasks(stage, jobId.get)
            }
            else {
                for (parent <- missing) {
                    submitStage(parent)
                }
                waitingStages += stage
            }
        }
    }
    else {
        abortStage(stage, "No active job for stage " + stage.id)
    }
}

// 查找上级 Stage 的过程，启示就是递归向上遍历所有 RDD 依赖列表并生成 Stage 的过程
// 遍历的过程是非递归的层序遍历（不是前序、中序或后序），使用了堆栈来协助遍历，而且保证层序的顺序与 DAG 中的依赖顺序一致
private def getMissingParentStages(stage: Stage): List[Stage] = {
    val missing = new HashSet[Stage]
    val visited = new HashSet[RDD[_]]
    // 这里手工维护一个堆栈，避免递归访问过程中的栈溢出错误
    val waitingForVisit = new Stack[RDD[_]]
    def visit(rdd: RDD[_]) {
        if(!visited(rdd)) {
            visited += rdd
            if(getCacheLocs(rdd).contains(Nil)) {
                for(dep <- rdd.dependencies) {
                    dep match {
                        case shufDep: ShuffleDependency[_, _, _] =>
                            val mapStage = getShuffleMapStage(shufDep, stage.jobId)
                            if(!mapStage.isAvailable) {
                                missing += mapStage
                            }
                        case narrowDep: NarrowDependency[_] =>
                            waitingForVisit.push(narrowDep.rdd)
                    }
                }
            }
        }
    }
    waitingForVisit.push(stage.rdd)
    while(waitingForVisit.nonEmpty) {
        visit(waitingFOrVisit.pop())
    }
    missing.toList
}

// TaskScheduler
// 主要接口包括一个钩子接口（也称 hook，表示定义好之后，不是用户主动调用的）
def postStartHook() {} // 在初始化完成之后和调度启动之前
def start(): Unit  // 启动
def stop(): Unit  // 停止调度
def submitTasks(taskSet: TaskSet): Unit  // 提交 Task 集
def cancelTasks(stageId: Int, interruptThread: Bollean)
```

### 第5章 Spark SQL 与数据仓库

Apache Spark 核心与其他模块之间的关系：
* Spark SQL
* Spark Streaming
* MLlib（机器学习）
* GraphX（图计算）

Spark SQL 的特点：
* 性能非常高，得益于 Spark 的基因
* 使用了基于成本的优化器、列存储、代码生成等技术
* 可以拓展到上千个计算节点以及数小时的计算能力，并且支持自动容错恢复
* 提供了传统的 SQL 交互式查询功能，还支持业界标准的 JDBC/ODBC 访问接口，这样很容易实现分布式数据仓库，以及商业智能计算
* 与 Apache Hive 基本完全兼容，可以像使用 Hive 一样来使用 Spark SQL
* 提供领域 API，并且提供了专门的数据结构抽象 DataFrame，可以让 Spark 程序轻松进行 SQL 操作
* 支持 Scala、Java、Python 和 R 这四种编程语言
* 支持非常多的数据源，包括 Hive、Avro、Parquet、ORC、JSON、JDBC，而且提供了统一的读写接口，使用起来非常方便

```
// 在 Scala 代码中做 SQL 查询
sqlContext = HiveContext(sc)
val df = sqlContext.sql("SELECT * FROM table")
```

#### Spark SQL 基础

使用 Spark SQL 有两种方式：
* 作为分布式 SQL 引擎，此时只需要写 SQL 就可以进行计算，不需要复杂的编码，也是最受欢迎的方式
* 在 Spark 程序中，通过领域 API 的形式来操作数据（被抽象为 DataFrame）

作为分布式引擎，有两种运行方式：
* JDBC/ODBC Server（在正式环境下建议使用）
* 使用 Spark SQL 命令行（仅在本地测试时使用）

JDBC/ODBC 服务的部署结构：
  用户      用户      APP
   |        |         |
BeeLine  BeeLine      |
   |        |         |
Thrift JDBC/ODBC Server -- DB
            |
        Spark 集群

部署步骤：
* 部署 Spark 集群
* 部署一个 DB 用于存储数据库元数据
* 启动一个 JDBC/ODBC Server 作为对外服务的接口

支持的 SQL 语法：
* Spark SQL 在设计上与 Hive 兼容，是一种“开箱即用”的兼容方式，即可以直接替代 Hive 的角色，但内部实现完全不同
* 可以直接使用 Hive 的 meta 数据库，语法上也基本完全兼容 HiveQL，不同的地方非常少

Spark SQL 支持绝大部分的 Hive 特性：
* Hive 查询语句：SELECT/GROUP BY/ORDER BY/CLUSTER BY/SORT BY
* Hive 运算符：关系运算符/数学运算符/逻辑运算符/复杂类型构造器/数学函数/字符串函数/求交/求并/子查询/抽样/解释/表分区/视图
* Hive DDL：建表/CTAS/修改表结构
* Hive 数据类型：支持绝大部分，包括数字类型/字符串类型/二进制类型/布尔类型/日期时间类型/复杂类型
* 窗口函数
* 自定义：用户自定义函数（UDF）/用户自定义聚合函数（UDAF）/用户自定义序列化格式（SerDes）

Spark SQL 不支持：
* Hive 的 bucket 表，使用散列的方式对 Hive 表进行分区
* Hive 比较隐秘的特性：包括 UNION 类型、Unique join
* Hive 的优化方法：大部分都不支持，实现机制完全不一样，一些问题可能在 Spark 的内存计算模式下也不是问题

DataFrame：
* Spark 程序中可以通过编程接口来引用 Spark SQL 模块，编程时使用的数据抽象是 DataFrame
* 具有与 RDD 类似的分布式数据集特点，但增加了列的概念，可以与传统关系型数据库的表对应起来
* 与 R 和 Python 中的数据框（dataframe）在概念上也是相同的
* Spark 程序支持的 Scala、Java、Python、R 这四种编程语言都亏使用 DataFrame

在 Spark 中使用 DataFrame 的过程：
* 初始化环境：一般是创建一个 SQLContext 对象
* 创建一个 DataFrame：可以来源于 RDD 或其他数据源
* 调用 DataFrame 操作：一种领域特定的 API，可以实现所有的 SQL 功能
* 或者通过函数直接执行 SQL 语句

从 RDD 创建 DataFrame：
* 用 Scala 反射，代码简洁，但需要提前直到数据格式
* 程序指定，略复杂，但可以运行时指定

动态从 RDD 创建 DataFrame 的步骤：
* 从原来的 RDD 创建一个新的 RDD，成员是 Row 类型，包含所有列
* 创建一个 StructType 类型的表模式，其结构与 Row 结构相匹配
* 将表模式应用到创建的 RDD 上

```
// 1、创建 SQLContext：Spark SQL 相关的所有函数，都在 SQLContext 或它的子类中
val sc: SparkContext  // sc 是已经存在的 SparkContext 实例
val sqlContext = new org.apache.spark.sql.SQLContext(sc)
val sqlContext = new org.apache.spark.sql.hive.HiveContext(sc)  // Hive

// 2、创建 DataFrame
// 使用反射的方法从 RDD 创建 DataFrame
import sqlContext.implicits._
case class Person(name: String, age: Int)  // 定义一个 case class，参数名即为表的列名
val rdd = sc.textFile("examples/src/main/resources/people.txt").map(_.split(","))  // 从文本创建 RDD
val rddContainingCaseClass = rdd.map(p => Person(p(0), p(1).trim.toInt))  // RDD 包含 case class
val people = rddContainingCaseClass.toDF()  // RDD 被隐式转换成 DataFrame
people.show()  // 将 DataFrame 的内容打印到标准输出

// 使用程序动态从 RDD 创建 DataFrame
val people = sc.textFile("examples/src/main/resources/people.txt")  // 普通 RDD
val schemaString = "name age"  // 字符串格式的表模式
import org.apache.spark.sql.Row;
import org.apache.spark.sql.types.{StructType, StructField, StringType};
val schema =  // 根据字符串格式的表模式创建结构化的表模式，用 StructType 保存
    StructType(
        schemaString.split(" ").map(fieldName => StructField(fieldName, StringType, True))
    )
val rowRDD = people.map(_.split(",")).map(p => Row(p(0), p(1).trim))  // 将普通 RDD 的成员转换成 Row 对象
val peopleDataFrame = sqlContext.createDataFrame(rowRDD, schema)  // 将模式作用到 RDD 上，生成 DataFrame
people.show()  // 将 DataFrame 的内容打印到标准输出

// 从其他数据源生成 DataFrame
val df = sqlContext.read.json("examples/src/main/resoures/people.json")  // 从 JSON 格式的文件创建
df.show()

// 3、DataFrame 操作：通过领域 API 方式访问 DataFrame
var sc: SparkContext
val sqlContext = new org.apache.spark.sql.SQLContext(sc)
var df = sqlContext.read.json("examples/src/main/resources/people.json")
df.show() // equals to select * from people
df.printSchema()  // equals to create table people
df.select("name")  // equals to select name from people
df.select(df("name"), df("age")+1).show()  // equals to select name, age+1 from people
df.filter(df("age") > 21).show  // equals to select * from people where age > 21
df.groupBy("age").count().show()  // equals to select age, count(*) from people group by age

// 4、执行 SQL：将 DataFrame 注册成表，然后使用纯 SQL 语句的方式来访问
df.registerTempTable("people")
val result = sqlContext.sql("SELECTA * FROM people")  // 自带 SQL 解析器

// 也可以换成功能更强大的 HiveQL 解析器
set spark.sql.dialect=hiveql;
val sqlContext = new org.apache.spark.sql.SQLContext(sc)
var df = sqlContext.read.json("examples/src/main/resources/people.json")
df.saveAsTable("people")  // 持久化，将表信息保存在数据库中，即使程序退出后，其他进程还可以访问它

// 5、DataFrame 数据源
// 通过数据源加载数据
// 默认的类型是 Parquet，这是大数据计算中最常用的列式存储格式，也可以用 format 指定其他类型
var df = sqlContext.read.load("examples/src/main/resources/users.parquet")
var df = sqlContext.read.format("json").load("examples/src/main/resources/users.parquet")
df.select("name", "age").write.save("nameAndFavColors.parquet")

// 保存数据：默认模式（如果文件已存在则报错）、追加模式、覆盖写模式、如果文件存在就放弃写
df.selct("name, "age").write.save("nameAndFavColors.parquet")
df.selct("name, "age").write.format("parquet").save("nameAndFavColors.parquet")

// 6、性能调优：使用内存来缓存数据
sqlContext.cacheTable("tableName")
sqlContext.uncacheTable("tableName")
dataFrame.cache()
spark.sql.inMemoryColumnarStorage.compressed=true  // 开启压缩
spark.sql.inMemoryColumnarStorate.batchSize  // 增加缓存快大小可以提升内存利用率，但也会增加内存溢出的风险
```

#### Spark SQL 原理和运行机制

Spark SQL 发展：
* 在 Spark 出现之前，基于 Hadoop MR 的 Hive 一致是开源大数据 SQL 计算的唯一选择，但其性能一致被人诟病
* 基于 Spark 的 SQL 计算框架 Shark：基于 Hive 代码实现，优化和维护很困难，也无法充分利用 Spark 和 Scala 的优势
* 在 Shark 的基础上，Spark SQL 进行重新开发，语法上尽可能保持与 Hive 兼容，重写了执行优化器 Catalyst
* 相较于 Shark，Spark SQL 最大的优势是性能，并且 Catalyst 的拓展性也非常好

Spark SQL 整体框架：
数据源    JDBC/ODBC  Spark SQL CLI  Spark程序/ML
Parquet      |            |             |
Hive     Spark SQL                      |
JSON         |                          |
JDBC     Spark Core（RDD)
AVRO
...

Catalyst 执行过程：
* 分析阶段：分析逻辑树，解决引用
* 逻辑优化
* 物理计划：Catalyst 会生成多个计划，并基于成本进行对比
* 代码生成：将查询编辑成 Java 字节码

#### 应用场景：基于淘宝数据建立电商数据仓库

传统的数据仓库方案弊端：
* 成本高
* 拓展性差

Spark SQL 的特点：
* 分布式计算
* 高可拓展性
* 容错性
* 支持 JDBC/ODBC
完全可以作为分布式数据仓库的核心

数据仓库架构：
* 其他辅助工具、BI工具
* BeeLine交互式
* metaDB、JDBC/ODBC Server
* Spark with Spark SQL
* Hadoop HDFS

Spark SQL 在大规模数据下的性能表现：
* Spark 的性能基本上与数据量大小保持线性关系
* 扩充未来集群的计算能力变得非常简单，只需要增加计算节点即可

### 第6章 Spark 流式计算

流式计算：
* 需要实时对大量数据进行快速处理，处理周期短，一般是分钟级甚至秒级响应
* 7*24 小时连续不断进行计算
* Spark 通过 Spark Streaming 组件提供了支持

Spark Streaming：
* 基于 Spark 核心，具备可拓展性、高吞吐量、自动容错等特性
* 数据源：支持 HDFS/S3/NFS、Kafka、Flume、Twitter、Kinesis、MQTT、自定义输入流
* 数据处理：Spark Streaming 可以使用 map、reduce、join、window 等高级函数来实现复杂逻辑
* 计算结果：可写入 HDFS、数据库、数据仪表盘

Spark Streaming 计算过程：
* 输入数据流
* 按周期将数据分成多批次（batch），按批次提交给 Spark 核心来调度计算
* 结果按小批次输出

#### Spark Streaming 基础知识

基本概念：
* StreamingContext：基本环境对象，提供基本的功能入口
* DStream：表示连续的数据流，Streaming 下的 RDD
* 输入 DStream：用于从各类数据源接受数据
* DStream 操作：Transformation 操作和 output 操作
* 窗口操作：处理最近几个周期的数据，可以设置数据的滑动窗口，将数个原始 DStream 合并成一个窗口 DStream（需设置窗口长度和滑动区间）

高级操作：
* 持久化：缓存到内存中，调用 persist()，窗口函数和 updateStateByKey 默认会自动持久化，因为数据的确会被多次使用
* 打包、发布和监控
* 部署：打包 JAR 包、配置足够的内存、配置检查点、配置 Driver 程序自动重启
* 更新程序代码：需要重启程序，如果想要在重启过程中不丢失数据，有两种方法，新旧程序同时运行或先停止旧的程序再启动新的程序
* 监控流式计算程序运行：Web 页面中 Scheduling Delay + Processing Time 加起来就是 Spark Streaming 一次计算周期的总时间

```
// Spark Streaming 版本的 WordCount
// 1、启动 TCP socket，在 Linux 下通过 nc 启动，端口为 9999
nc -lk 9999

// 2、启动 spark-shell
./bin/spark-shell

// 3、在 spark-shell 中输入代码运行程序
import org.sapache.spark.SparkConf
import org.apache.spark.streaming.{Seconds, StreamingContext}

// 指定运行周期为五秒，表示流式计算每间隔五秒执行一次，需要综合考虑程序的延时需求和集群的工作负载，应该大于每次的运行时间
// 每个 RDD 对应一个计算周期，所有操作都会映射为对内部的 RDD 操作
val ssc = new StreamingContext(sc, Seconds(5))
var lines = ssc.socketTextStream("localhost", 9999)  // 创建 DStream

// 基本的操作函数与 RDD 同名
var words = lines.flatMap(_.split(" "))
var pairs = words.map(word +> (word, 1))
val wordCounts = pairs.reduceByKey(_ + _)

// 打印结果到标准输出（只打印 DStream 中每个 RDD 的前十个元素
// print 不同于 RDD 中的 Action 操作，不会触发真正的调度执行
wordCounts.print()

// 这里才正式启动计算
ssc.start()

// 等待执行结束（出错或 Ctrl-C 退出）
ssc.awaitTermination()

// 4、在前面启动的 nc 客户端中输入文本，就可以看到计算结果了
```

#### 深入理解 Spark Streaming

DStream 的两类操作：
* Transformation 操作：对应 RDD 的 Transformation 操作
* Output 操作：触发 DStream 的实际执行，作用非常类似于 RD 的 Action 操作

一般流式计算过程：
* 输入数据流：数据接收
* 数据操作：Transformation 操作
* 结果输出：Output 操作

两类可能发生的错误：
* worker 节点失效：一旦计算节点失效，所有内存中的数据都会丢失且无法恢复
* Driver 节点失效：SparkContext 也会随之失效，整个 Streaming 程序会退出，内存中的数据全部丢失

容错保障的效果：
* 至多一次：每条记录最多被计算一次，或者根本没有计算就丢失了
* 到少一次：保证每条记录都不丢失，最少计算一次，但可能会重复多次计算
* 精准一次：保证每条记录都不丢失，并且只计算一次，不多不少，显然这是最佳的容错保障

结果输出容错：
* 本身提供至少一次级别的容错性能，但可能通过一些辅助手段来实现精准一次的容错效果
* 幂等更新：确保多操作的效果与一次操作的效果相同
* 事务更新：更新时带上事务信息，确保更新只进行一次，实现精准一次的容错效果

检查点：
* 调用了有状态的 Transformation 操作，必须弃用检查点功能
* 如果期望程序在因 Driver 节点失效后的重启之后可以继续运行，也建议开启检查点功能，可以记录配置、操作以及未完成批次，方便重启后继续运行

性能调优方向：
* 减少批处理时间：每个批次的处理时间尽可能短
* 设置合理批次间隔时间：收到数据后，尽可能快地处理

#### 应用场景：一个类似百度统计的流式实时系统

基于网站的访问日志分析是典型的流式实时计算应用场景：
* 流量分析：一段时间内用户网站的流量变化趋势，针对不同的 IP 对用户网站的流量进行细分，常见指标是总 PV 和各 IP 的 PV
* 来源分析：各种搜索殷勤来源给用户网站带来的流量情况，常见指标是搜索引擎、关键词和终端类型的 PV
* 网站分析：各个页面的访问情况，哪些页面最吸引访客或更容易导致访客流式，从而更有针对性地改善网站质量，常见指标是各页面的 PV
* 转化分析
* 安全分析：用来识别 CC 攻击、SQL 注入分析、脱库等

日志实时采集：
* 一般在 HTTP 服务器收集，比如 Nginx access 日志文件
* 一个典型的方案是 Nginx 日志文件 + Flume + Kafka + Spark Streaming

具体方案：
* 接收服务器：用 Nginx，根据负载可以部署多台，数据落地至本地日志文件
* 每个 Nginx 节点上部署 Flume，使用 tail -f 实时读取 Nginx 日志，发送到 Kafka 集群
* 专用的 Kafka 集群用户连接实时日志和 Spark 集群
* Spark Streaming 程序实时消费 Kafka 集群上的数据，实时分析和输出
* 结果写入 MySQL 数据库
* 进一步优化：CGI 程序直接发日志消息到 Kafka，节省了写访问日志的磁盘开销

```
# 我们简单模拟一下数据收集和发送的环节
# 用一个 Python 脚本随机生成 Nginx 访问日志，并通过脚本的方式自动上传至 HDFS，移动到指定目录
# Spark Streaming 程序监控 HDFS 目录，自动处理新的文件

# 1、生成 Nginx 访问日志，保存为文件 sample_web_log.py
# !/usr/bin/env python
# -*- coding: utf-8 -*-
import random
import time

class WebLogGeneration(object):
    def __init__(self):
        self.user_agent_dist = {}  # 浏览器类型和版本
        self.ip_slice_list = []
        self.url_path_list = []
        self.http_refer = []
        self.search_keyword = []
    
    def sample_ip(self):
        slice = random.sample(self.ip_slice_llist, 4)  # 随机选择 4 个 ip
        return ".".join([str(item) for item in slice])

    def sample_url(self):
        return random.sample(self.url_path_list, 1)[0]

    def sample_user_agent(self):
        dist_uppon = ramdon.uniform(0, 1)
        return self.user_agent_dist[float('0.1f' % dist_uppon)]
    
    # 主要搜索引擎 referrer 参数
    def sample_refer(self):
        if random.uniform(0, 1) > 0.2:  # 只有 20% 流量有 refer
            return "-"
        
        refer_str=random.sample(self.http_refer, 1)
        query_str=random.sample(self.search_keyword, 1)
        return refer_str[0].format(query=query_str[0])

    def sample_one_log(eslf, count = 3):
        time_str = time.strftime("%Y-%m-%d %H:%M:%s", time.localtime())
        while count > 1:
            query_log = "{ip} - - [{local_time}] \"GET /{url} HTTP/1.1\" 200 0
                \"{refer}\" \"{user_agent}\" \"-\""
                .format(ip=self.sample_ip(),
                        local_time=time_str,
                        url=self.sample_url(),
                        refer=self.sample_refer(),
                        user_agent=self.sample_user_agent())
            print query_log
            count = count - 1
    
if __name__ == "__main__":
    web_log_gene = WebLogGeneration()
    web_log_gene.sample_one_log(random.uniform(30000, 50000))

# 2、调用上面的脚本来随机生成日志，上传至 HDFS，并移动到目标目录
# !/bin.bash

# HDFS命令
HDFS="/usr/local/myhadoop/hadoop-2.6.0/bin/hadoop fs"

# 网站日志存放的目录，也是Streaming程序监听的目录
streaming_dir="/spark/streaming"

# 清空旧数据
$HDFS -rm "${streaming_dir}" '/tmp/*' > /dev/null 2>&1
$HDFS -rm "${streaming_dir}" '/*'     > /dev/null 2>&1

# 一直运行
while [ 1 ]; do
    ./sample_web_log.py > test.log

    # 给日志加上时间戳，避免重名
    tmplog="access.`date +'%s'`.log"

    # 先放在临时目录，再move至Streaming程序监控的目录下，确保原子性
    # 临时目录用的是监控目录的子目录，因为子目录不会被监控
    $HDFS -put test.log ${streaming_dir}/tmp/$tmplog
    $HDFS -mv           ${streaming_dir}/tmp/$tmplog ${streaming_dir}/

    echo "`date ="%F %T"` put $tmplog to HDFS secceed"
    sleep 1
done

# 日志示例
# 46.202.124.63 - - [2015-11-26 09:54:27] "GET /view.php HTTP/1.1" 200 0
# "http://www.google.cn/search?q=hadoop" "Mozilla/5.0 (compatible; MSIE 10.0; Windows
# NT 6.2; Trident/6.0)" "-"

# 3、Spark Streaming 程序代码
import org.apache.spark.SparkConf
import org.apache.spark.streaming.{Seconds, StreamngContext}

// 设计计算的周期，单位：秒
val batch = 10

/*
 * 这是 bin.spark-shell 交互模式下创建 StreamingContext 的方法
 * 非交互式请使用下面的方法来创建
 */
val ssc = new StreamingContext(sc, Seconds(batch))

/* 
// 非交互式模式下创建 StreamingContext 的方法
val conf = new SparkCOnf().setAppName("NgixAnay")
val ssc = new StreamingContext(conf, Seconds(batch))
*/

/* 创建输入 DStream，是文本文件目录类型
 * 本地模式下也可以使用本地文件系统的目录，比如 file:///home/spark/streaming
 */
val lines = ssc.textFileStream("hdfs:///spark/streaming")

/*
 * 下面是统计各项指标，调试时可以只进行部分统计，方便观察结果
 */

// 1.总PV
lines.count().print()

// 2.各IP的PV，按PV倒序
lines.map(line => {(line.split(" ")(0), 1)}).reduceByKey(_ + _).transform(rdd => {
    rdd.map(ip_pv => (ip_pv._2, ip_pv._1))
}).print()

// 3.搜索引擎PV
var refer = lines.map(_.split("\"")(3))

// 先输出搜索引擎和关键词，避免统计搜索关键词时重复统计
// 输出 (host, query_keys)
var searchEngineInfo = refer.map(r => {
    var f = r.split('/')
    var searchEngines = Map(
        "www.google.cn" -> "q",
        "www.yahoo.com" -> "p",
        "cn.bing.com" -> "q",
        "www.baidu.com" -> "wd",
        "www.sougou.com" -> "query"
    )

    if (f.length > 2) {
        val host = f(2)

        if (searchEngines.contains(host)) {
            val query = r.split('?')(1)
            if (query.length > 0) {
                val arr_search_q = 
                    query.split('&').filter(_.indexOf(searchEngines(host)+"=") ==0)
                if (arr_search_q.length > 0)
                    (host, arr_search_q(0).split('=')(1))
                else
                    (host, "")
            } else {
                (host, "")
            }
        } else
            ("", "")
    } else
        ("", "")
})

// 输出搜索引擎PV
searchEngineInfo.filter(_._1.length > 0).map(p => {(p._1, 1)}).reduceByKey(_ + _).print()

// 4.关键词PV
searchEngineInfo.filter(_._2.length > 0).map(p => {(p._2, 1)}).reduceByKey(_ + _).print()

// 5.终端类型PV
// (Andriod, 4281)
// (Default, 35745)
// (iPhone, 4348)
lines.map(_.spllit("\"")(5)).map(agent => {
    val types = Seq("iPhone", "Android")
    var r = "Default"
    for (t <- types) {
        if (agent.indexOf(t) != -1)
            r = t
    }
    (r, 1)
}).reduceByKey(_ + _).print()

// 6.各页面PV
lines.map(line => {(line.split("\"")(1).split(" ")(1), 1)}).reductByKey(_ + _).print()

// 启动计算，等待执行结束（出错或Ctrl+C退出）
ssc.start()
ssc.awaitTermination()

// 除了常规的每个固定周期进行一次统计，还可以对连续多个周期的数据进行统计，利用窗口方法实现

// 窗口方法必须配置 checkpoint
ssc.checkpoint("hdfs://spark/checkpoint")

// 这是常规每10秒一个周期的PV统计
lines.count().print()

//这是每分钟（连续多个周期）一次的PV统计
lines.countByWindow(Seconds(batch*6), Seconds(batch*6)).print()
```


### 第7章 Spark 图计算

* 社交网络中人与人之间的关系，用图来表示最合适，顶点表示社交中的人，边则表示人与人之间的关系
* 图是基础的数据结构，和链表、树不同，它是一种非线性数据结构
#### Spark GraphX 库简介

#### 应用场景：基于新浪微博数据的社交网络分析


### 第8章 Spark MLlib

#### 机器学习简介

#### MLlib 库简介

#### 应用场景：搜索广告点击率预估系统
