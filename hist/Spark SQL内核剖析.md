#### 前言
#### 第1章 Spark SQL背景
#### 第2章 Spark基础知识介绍
#### 第3章 Spark SQL执行全过程概述
#### 第4章 Spark SQL编译器Parser
#### 第5章 Spark SQL逻辑计划
#### 第6章 Spark SQL物理计划
#### 第7章 Spark SQL之Aggregation实现
#### 第8章 Spark SQL之Join实现
#### 第9章 Tungsten技术实现
#### 第10章 Spark SQL连接Hive
#### 第11章 Spark SQL开发与实践

[Spark SQL 示例](https://hadoopdoc.com/spark/spark-sql-tutorial)

```
// 将json文件放到hdfs上面
hdfs dfs -put employee.json /tmp

// 读取文件（包含age/id/name属性）
val dfs = spark.read.json("hdfs:/tmp/employee.json")

// 查看数据
dfs.show()

// 查看数据模式（列与类型）
dfs.printSchema()

// 查询数据
dfs.select("name").show()

// 过滤数据
dfs.filter(dfs("age")>23).show()

// 数据聚合
dfs.groupBy("age").count().show()

// 加载数据
val peopleDF = spark.read.format("json").load("hdfs:/tmp/people.json")

// 保存数据
peopleDF.select("name","age").write.format("parquet").save("/tmp/peoplenew.parquet")

// Parquet文件
val parquetFileDF=spark.read.parquet("hdfs:/tmp/peoplenew.parquet")
peopleDF.write.parquet("/tmp/people.parquet")

// Orc文件
val df=spark.read.json("hdfs:/tmp/people.json")
df.write.mode("append").orc("/tmp/people")
spark.read.orc("/tmp/people").show()

// Json文件
val df=spark.read.json("hdfs:/tmp/people.json")
df.write.mode("overwrite").json("/tmp/peoplejson/")
spark.read.json("/tmp/peoplejson/").show()

// Hive表
import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.SparkSession
val spark = SparkSession.builder().enableHiveSupport().getOrCreate()
spark.sql("select count(1) from tmp.tmp_building_num").show()

// JDBC（mysql）
df.repartition(1).write.mode("append")
  .option("user", "root").option("password", "password").jdbc("jdbc:mysql://localhost:3306/test","alluxio",new Properties())
val fromMysql = spark.read.option("user", "root")
  .option("password", "password").jdbc("jdbc:mysql://localhost:3306/test","alluxio",new Properties())

// SparkSession（SparkSQL编码入口）
import org.apache.spark.sql.SparkSession
val spark = SparkSession
  .builder()
  .appName("Spark SQL basic example")
  .config("spark.some.config.option", "some-value")
  .getOrCreate()
import spark.implicits._

// DataSet
case class Person(name:String, age:Long)
val caseClassDS = Seq(Person("Andy",32)).toDS()
caseClassDS.show()

val primitiveDS = Seq(1,2,3).toDS()
primitiveDS.map(_ + 1).collect()
val path="hdfs:/tmp/people.json"

// DataFrame转换成DataSet
val peopleDS = spark.read.json(path).as[Person]
peopleDS.show()

// RDD、DataFrame、Dataset共同点
// 1、都是spark平台下的分布式弹性数据集，为处理超大型数据提供便利
// 2、都会根据spark的内存情况自动缓存运算，这样即使数据量很大，也不用担心会内存溢出
// 3、有许多共同的函数，如filter，排序等

// 4、都有惰性机制
val sparkconf = new SparkConf().setMaster("local").setAppName("test").set("spark.port.maxRetries","1000")
val spark = SparkSession.builder().config(sparkconf).getOrCreate()
val rdd=spark.sparkContext.parallelize(Seq(("a", 1), ("b", 1), ("a", 1)))
rdd.map{ line=>
  println("运行")  // 惰性机制，不会执行
  line._1
}

// 5、都有partition的概念
// 对每一个分区进行操作时，就跟在操作数组一样，不但数据量比较小，而且可以方便的将map中的运算结果拿出来
var predata = data.repartition(24).mapPartitions{
      PartLine => {
        PartLine.map{
          line =>
            println(“转换操作”)
        }
      }
}
// 如果直接用map，map中对外面的操作是无效的
val rdd = spark.sparkContext.parallelize(Seq(("a", 1), ("b", 1), ("a", 1)))
var flag = 0
val test = rdd.map{ line=>
  println("运行")
  flag += 1
  println(flag)
  line._1
}

// 6、在对DataFrame和Dataset进行操作许多操作都需要这个包进行支持
import spark.implicits._  //这里的spark是SparkSession的变量名

// 7、DataFrame和Dataset均可使用模式匹配获取各个字段的值和类型
// DataFrame：
testDF.map{
  case Row(col1:String,col2:Int)=>
    println(col1);println(col2)
    col1
  case _ => ""
}
// Dataset：
case class Coltest(col1:String, col2:Int) extends Serializable
testDS.map{
  case Coltest(col1: String, col2: Int) =>
    println(col1);println(col2)
    col1
  case _ => ""
}

// DataFrame、Dataset共同点
// 1、DataFrame与Dataset一般与spark ml同时使用
dataDF.createOrReplaceTempView("tmp")
spark.sql("select ROW,DATE from tmp where DATE is not null order by DATE").show(100,false)

// 2、DataFrame与Dataset支持保存文件和读取文件
val options = Map("header" -> "true", "delimiter" -> "\t", "path" -> "hdfs://172.xx.xx.xx:9000/test")  // 配置项
dataDF.write.format("com.databricks.spark.csv").mode(SaveMode.Overwrite).options(options).save()  // 保存
val datarDF= spark.read.options(options).format("com.databricks.spark.csv").load()  // 读取

// 3、Dataset和DataFrame拥有完全相同的成员函数，区别只是每一行的数据类型不同
// DataFrame：也可以叫Dataset[Row]，每一行的类型固定为Row，每一列的值没法直接访问，只有通过解析或模式匹配才能获取各个字段的值
testDF.foreach{
  line =>
    val col1=line.getAs[String]("col1")
    val col2=line.getAs[String]("col2")
}
// Dataset：每一行是什么类型是不一定的，在自定义了case class之后可以很自由的获得每一行的信息
case class Coltest(col1: String, col2: Int) extends Serializable  // 定义字段名和类型
val test: Dataset[Coltest] = rdd.map{ line =>  // rdd ("a", 1) ("b", 1) ("a", 1)
  Coltest(line._1,line._2)
}.toDS
test.map{
  line =>
    println(line.col1)
    println(line.col2)
}
// 可以看出，Dataset在需要访问列中的某个字段时是非常方便的，然而，如果行的类型又不确定，用DataFrame即Dataset[Row]就能比较好的解决问题

// RDD、DataFrame、Dataset转换
// 1、DataFrame/Dataset转RDD
import spark.implicits._
val rdd1 = testDF.rdd
val rdd2 = testDS.rdd

// RDD转DataFrame：一般用元组把一行的数据写在一起，然后在toDF中指定字段名。
val testDF = rdd.map {line=>
      (line._1,line._2)
    }.toDF("col1","col2")

// RDD转Dataset：定义每一行的类型（case class）时，已经给出了字段名和类型，后面只要往case class里面添加值即可
case class Coltest(col1: String, col2: Int) extends Serializable
val testDS = rdd.map {line=>
      Coltest(line._1,line._2)
    }.toDS

// 2、Dataset和DataFrame互转
// Dataset转DataFrame：只要把case class封装成Row即可
val testDF = testDS.toDF
// DataFrame转Dataset：给出每一列的类型后，使用as方法
case class Coltest(col1: String, col2: Int) extends Serializable
val testDS = testDF.as[Coltest]
```

#### 前言

Spark SQL
* SQL-on-Hadoop 解决方案（包括 Hive、Presto 和 Impala 等）
* 结合了数据库 SQL 处理和 Spark 分布式计算模型两个方面的技术
* 目标是取代传统的数据仓库


#### 第1章 Spark SQL背景

##### 1.1 大数据与Spark系统

大数据的特点（5V）：
* Volume（体量大）
* Velocity（时效高）
* Variety（类型多）
* Veracity（真实性）
* Value（价值大）

Spark的优势：
* 提供了更加丰富灵活的数据操作方式，有些需要分解成几轮的 MapReduce 作业操作，可以在 Spark 里一轮实现
* 每轮的计算结果都可以分布式地存放在内存中，下一轮作业直接从内存中读取上一轮的数据，节省了大量的磁盘 IO 开销
* Spark 对 Hadoop 计算的兼容，以及对迭代型计算有着优异表现

典型应用场景：
* Hadoop（HDFS）作为大数据存储的标准
* Spark 作为计算引擎的核心

##### 1.2 关系模型与SQL语言

SQL-on-Hadoop 依赖：
* 分布式存储技术（如 HDFS、HBase 等）
* 分布式计算框架（如 MapReduce、Tez、Spark 等）

SQL-on-Hadoop 解决方案：
* 三层结构，松耦合，具有通用性和灵活性
* 隔离各层特性，限制深度集成优化的空间
* 应用（语言）层：为用户提供数据管理查询的接口（如 HiveQL 和 SparkSQL 的 DataFrame 等）
* 分布式执行层：通过一定的规则或策略奖 SQL 语句转换为对应的计算模型
* 数据存储层：对关系数据表这样的逻辑视图进行存储和管理，与分布式执行层通过特定的数据读写接口进行交互

##### 1.3 Spark SQL发展历程

Spark SQL 的前身（Shark）：
* 目标是既能达到 EDW（企业数据仓库）的性能，又能够具有 MapReduce 的水平拓展功能
* 在 Hive 的基础上修改了内存管理、物理计划、执行三个模块钟的部分逻辑
* 与 Hive 完全兼容，并支持用户编写机器学习或数据处理函数，对 HiveQL 执行结果进行进一步分析

Shark 瓶颈：
* Hive 的语法解析和查询优化等模块本身针对的是 MapReduce，限制了在 Spark 系统上的深度优化和维护
* 过度依赖 Hive 制约了技术栈钟各个组件的灵活集成

Spark SQL 发展：
* 抛弃原有 Shark 的架构方式，但汲取了 Shark 的优点，如内存列存储、Hive 兼容性等
* 摆脱了对 Hive 的依赖，在数据兼容、性能优化、组件拓展方面都得到了极大的提升


#### 第2章 Spark基础知识介绍

##### 2.1 RDD编程模型

RDD：（弹性分布式数据集）
* Spark 的核心数据结构
* 在编程接口层面提供一种高度受限的共享内存模型
* 本质是一种分布式的内存抽象，表示一个只读的数据分区集合
* 一个 RDD 通常只能通过其他 RDD 转换而创建，新的 RDD 包含了如何从其他 RDD 衍生所必须的信息（依赖关系）
* 丰富的转换操作：map、join 和 filter 等

RDD 依赖类型：
* 窄依赖：RDD 之间分区是一一对应的；所有转换操作可以通过类似管道的方式全部执行
* 宽依赖：下游 RDD 的每个分区与上游 RDD 的每个分区都有关，是多对多的关系；数据需要在不同节点之间 Shuffle 传输

RDD 计算：
* 通过一个 compute 函数得到每个分区的数据
* 通过已有的文件系统构建：compute 函数读取指定文件系统中的数据
* 通过其他 RDD 转换而来的：compute 函数执行转换逻辑，将其他 RDD 的数据进行转换

RDD 操作算子：
* transformation：用来将 RDD 进行转换，构建 RDD 的依赖关系（通过对象上的各种方法调用来转换）
* action：用来触发 RDD 的计算，得到 RDD 的相关计算结果或将 RDD 保存到文件系统中（惰性计算）

RDD 计算任务：
* 从稳定的物理存储（如 HDFS）中加载记录，记录被传入由一组确定性操作构成的 DAG（有向无环图），然后写回稳定存储
* 可以将数据集缓存到内存中，使得在多个操作之间可以很方便地重用数据集

容错性：
* 基于 RDD 之间的依赖，一个任务流可以描述为 DAG
* 在实际执行的时候，RDD通过 Lineage 信息（血缘关系）来完成容错，即使出现数据分区丢失，也可以重建分区
* 如果在应用程序中多次使用同一个 RDD，则可以将这个 RDD 缓存起来，第二次用到的时候会直接从缓存读取
* RDD 支持用 checkpoint 机制将数据保存到持久化的存储中，之后的 RDD 不再需要知道它的父 RDD，可以从 checkpoint 处获取数据

```
// RDD 编程模型（WordCount 案例）
def main(args: Array[String]) {
    val conf = new SparkConf()
    val sc = new SparkContext(conf)
    val result = sc.texfFile("hdfs://......")  // 初始 RDD
        .flagMap(line => line.split(" "))  // 转换 RDD
        .map(word => (word, 1))  // 映射得到新的 RDD
        .reduceByKey(_ + _)  // 统计后得到最终的 RDD
    result.collect()  // 直到这一步才会触发 RDD 执行
}
```

##### 2.2 DataFrame与Dataset

DataFrame：
* 与 RDD 一样，都是不可变分布式弹性数据集
* RDD 不包含结构信息；与之不同，可以直接将结构化数据集导入 DataFrame，相比 RDD 多了数据特性，因此可以进行更多的优化

```
// DataFrame（WordCount 案例）
val conf = new SparkConf()
val sc = new SparkContext(conf)
val linesDF = sc.texfFile("hdfs://......").toDF("line")  // 只有line一列的DataFrame
val wordsDF = lindsDF.explode("line", "word")((line: String) => line.split(" "))
val wordCountDF = wordsDF.groupBy("word").count()
wordCountD.collect()
```

DataSet：
* 结合了 RDD 和 DataFrame 的优点，提供类型安全和面向对象的编程接口
* 引入了编码器（Encoder）的概念

```
case class Person(name: String, age: Long)  // Encoder
val caseClassDS = Seq(person("Andy", 32)).toDS()
caseclassDS.show()
```


#### 第3章 Spark SQL执行全过程概述

##### 3.1 从SQL到RDD：一个简单的案例

```
val spark = SparkSession.builder().appName("example").master("local").getOrCreate()
spark.read.json("student.json").createOrReplaceTempView("student")  // json文件中包含id/name/age三项属性
spark.sql("select name from student where age > 18").show()

SQL查询 -> 逻辑计划 -> 物理计划
           Project    ProjectExec(1)
           Filter     FilterExec(2)
           Relation   FileSourceScanExec(3)

// 转换后基于RDD的程序
val rdd0 = FileSourceScanExec.inputRDD  // (3)
val rdd1 = rdd0.FileSourceScanExec_execute()  // (3)
val rdd2 = rdd1.FilterExec_execute()  // (2)
val rdd3 = rdd2.ProjectExec_execute()  // (1)
```

从 SQL 到 Spark 中 RDD 的执行需要经过两个大的阶段：
* 逻辑计划（LogicalPlan）
* 物理计划（PhysicalPlan）

逻辑计划：
* SQL语句转换成树型数据结构（逻辑算子树），蕴含的逻辑映射到逻辑算子树的不同节点
* 逻辑算子树并不会直接提交执行，仅作为中间阶段
* 逻辑算子树阶段：未解析（仅数据结构，不包含信息）-> 解析 -> 优化

物理计划：
* 将上一步逻辑计划阶段生成的逻辑算子树进行进一步转换，生成物理算子树
* 物理算子树的节点会直接生成 RDD 或对 RDD 进行 transformation 操作
* 物理算子树阶段：生成列表 -> 选取最优 -> 提交前的准备工作
* RDD 执行 action 操作（如例子中的 show），即可提交执行

##### 3.2 重要概念

Catalyst：
* Spark SQL 内部实现上述流程中平台无关部分的基础框架
* 主要包括InternalRow体系、TreeNode体系和Expression体系

InternalRow体系：
* 表示一行行数据的类
* BaseGenericlnternalRow：实现get类型方法
* JoinedRow：主要用于Join操作，将两个InternalRow放在一起形成新的InternalRow
* UnsafeRow：不采用 Java对象存储的方式，避免了JVM中垃圾回收（GC）的代价

TreeNode体系：
* 提供的仅仅是一种泛型
* 包含了QueryPlan和Expression两个子类继承体系

Expression体系：
* 不需要触发执行引擎而能够直接进行计算的单元
* 例如加减乘除四则运算、逻辑操作、转换操作、过滤操作等

##### 3.3 内部数据类型系统


#### 第4章 Spark SQL编译器Parser

##### 4.1 DSL工具之ANTLR简介
##### 4.2 SparkSqlParser之AstBuilder
##### 4.3 常见SQL生成的抽象语法树概览

#### 第5章 Spark SQL逻辑计划

##### 5.1 Spark SQL逻辑计划概述
##### 5.2 LogicalPlan简介
##### 5.3 AstBuilder机制：Unresolved LogicalPlan生成
##### 5.4 Analyzer机制：Analyzed LogicalPlan生成
##### 5.5 Spark SQL优化器Optimizer

#### 第6章 Spark SQL物理计划

##### 6.1 Spark SQL物理计划概述
##### 6.2 SparkPlan简介
##### 6.3 Metadata与Metrics体系
##### 6.4 Partitioning与Ordering体系
##### 6.5 SparkPlan生成
##### 6.6 执行前的准备

#### 第7章 Spark SQL之Aggregation实现