推荐序
译者序
序
前言
第1章 Spark数据分析导论
第2章 Spark下载与入门
第3章 RDD基础
第4章 键值对操作
第5章 数据读取与存储
第6章 Spark编程进阶
第7章 在集群上运行Spark
第8章 Spark调优与调试
第9章 Spark SQL
第10章 Spark Streaming
第11章 基于MLib的机器学习

只整理最核心的部分和python代码，力求在一个小时之内能看完

### 推荐序

近年来大数据逐渐升温，经常有人问起大数据为何重要。我们处在一个数据爆炸的时代，大量涌现的智能手机、平板、可穿戴设备及物联网设备每时每刻都在产生新的数据。当今世界，有 90% 的数据是在过去短短两年内产生的。到2020 年，将有500 多亿台的互联设备产生Zeta 字节级的数据。带来革命性改变的并非海量数据本身，而是我们如何利用这些数据。大数据解决方案的强大在于它们可以快速处理大规模、复杂的数据集，可以比传统方法更快、更好地生成洞见。

一套大数据解决方案通常包含多个重要组件，从存储、计算和网络等硬件层，到数据处理引擎，再到利用改良的统计和计算算法、数据可视化来获得商业洞见的分析层。这中间，数据处理引擎起到了十分重要的作用。毫不夸张地说，数据处理引擎之于大数据就像CPU之于计算机，或大脑之于人类。

不同于传统的数据处理框架，Spark 基于内存的基本类型（primitive）为一些应用程序带来了100倍的性能提升。Spark 允许用户程序将数据加载到集群内存中用于反复查询，非常适用于大数据和机器学习，日益成为最广泛采用的大数据模块之一。包括 Cloudera 和 MapR 在内的大数据发行版也在发布时添加了Spark。

目前，Spark 正在促使 Hadoop 和大数据生态系统发生演变，以更好地支持端到端的大数据分析需求，例如：Spark 已经超越 Spark 核心，发展到了Spark streaming、SQL、MLlib、GraphX、SparkR 等模块。学习 Spark 和它的各个内部构件不仅有助于改善大数据处理速度，还能帮助开发者和数据科学家更轻松地创建分析应用。从企业、医疗、交通到零售业，Spark 这样的大数据解决方案正以前所未见的力量推进着商业洞见的形成，带来更多更好的洞见以加速决策制定。

这本书不是简单地教开发者如何使用 Spark，而是更深入介绍了 Spark 的内部构成，并通过各种实例展示了如何优化大数据应用。我向大家推荐这本书，或更具体点，推荐这本书里提倡的优化方法和思路，相信它们能帮助你创建出更好的大数据应用。

### 序

Spark 作为下一代大数据处理引擎，在非常短的时间里崭露头角，并且以燎原之势席卷业界。Spark 对曾经引爆大数据产业革命的 Hadoop MapReduce 的改进主要体现在这几个方面：首先，Spark 速度更快；其次，Spark 丰富的 API 带来了更强大的易用性；最后，Spark 不单单支持传统批处理应用，更支持交互式查询、流式计算、机器学习、图计算等各种应用，满足各种不同应用场景下的需求。

### 前言

随着并行数据分析变得越来越流行，各行各业的工作者都迫切需要更好的数据分析工具。Spark 应运而生，并且迅速火了起来。作为 MapReduce 的继承者，Spark 主要有三个优点。首先，Spark 非常好用。由于高级 API 剥离了对集群本身的关注，你可以专注于你所要做的计算本身，只需在自己的笔记本电脑上就可以开发 Spark 应用。其次，Spark 很快，支持交互式使用和复杂算法。最后，Spark 是一个通用引擎，可用它来完成各种各样的运算，包括 SQL 查询、文本处理、机器学习等，而在 Spark 出现之前，我们一般需要学习各种各样的引擎来分别处理这些需求。这三大优点也使得 Spark 可以作为学习大数据的一个很好的起点。

本书的目标读者是数据科学家和工程师。

数据科学家关注如何从数据中发现关联以及建立模型。数据科学家通常有着统计学或者数学背景，他们中的大多数也熟悉 Python 语言、R 语言、SQL 等传统数据分析工具。在本书中，我们不仅会讲到 Spark 中一些机器学习和高级数据分析的程序库，也会把一些 Python 或者 SQL 的应用作为 Spark 使用示例进行展示。如果你是一位数据科学家，我们希望你读完本书之后，能够在获得更快速度和更大数据规模支持的同时，使用早已熟悉的方式来解决问题。

对于工程师，不管你擅长的是 Java 还是 Python，抑或是别的编程语言，我们希望这本书能够教会你如何搭建一个 Spark 集群，如何使用 Spark shell，以及如何编写 Spark 应用程序来解决需要并行处理的问题。如果你熟悉 Hadoop，你就已经在如何与 HDFS 进行交互以及如何管理集群的领域中领先了一小步。即使你没有 Hadoop 经验也不用担心，我们会在本书中讲解一些基本的分布式执行的概念。

### 第1章 Spark数据分析导论

##### 1.1 Spark是什么

Spark 是一个用来实现快速而通用的集群计算的平台。

在速度方面，Spark 扩展了广泛使用的 MapReduce 计算模型，而且高效地支持更多计算模式，包括交互式查询和流处理。在处理大规模数据集时，速度是非常重要的。速度快就意味着我们可以进行交互式的数据操作，否则我们每次操作就需要等待数分钟甚至数小时。

**Spark 的一个主要特点就是能够在内存中进行计算**，因而更快。不过即使是必须在磁盘上进行的复杂计算，Spark 依然比 MapReduce 更加高效。

总的来说，Spark 适用于各种各样原先需要多种不同的分布式平台的场景，包括批处理、迭代算法、交互式查询、流处理。通过在一个统一的框架下支持这些不同的计算，Spark 使我们可以简单而低耗地把各种处理流程整合在一起。而这样的组合，在实际的数据分析过程中是很有意义的。不仅如此，Spark 的这种特性还大大减轻了原先需要对各种平台分别管理的负担。

Spark 所提供的接口非常丰富。除了提供基于Python、Java、Scala 和SQL 的简单易用的 API 以及内建的丰富的程序库以外，Spark 还能和其他大数据工具密切配合使用。例如，Spark 可以运行在 Hadoop 集群上，访问包括 Cassandra 在内的任意 Hadoop 数据源。

##### 1.2 一个大一统的软件栈

Spark 项目包含多个紧密集成的组件。**Spark 的核心是一个对由很多计算任务组成的、运行在多个工作机器或者是一个计算集群上的应用进行调度、分发以及监控的计算引擎**。由于 Spark 的核心引擎有着速度快和通用的特点，因此 Spark 还支持为各种不同应用场景专门设计的高级组件，比如 SQL 和机器学习等。这些组件关系密切并且可以相互调用，这样你就可以像在平常软件项目中使用程序库那样，组合使用这些的组件。

各组件间密切结合的设计原理有这样几个优点。首先，软件栈中所有的程序库和高级组件都可以从下层的改进中获益。比如，当 Spark 的核心引擎新引入了一个优化时，SQL 和机器学习程序库也都能自动获得性能提升。其次，运行整个软件栈的代价变小了。不需要运行 5 到 10 套独立的软件系统了，一个机构只需要运行一套软件系统即可。这些代价包括系统的部署、维护、测试、支持等。这也意味着 Spark 软件栈中每增加一个新的组件，使用Spark 的机构都能马上试用新加入的组件。这就把原先尝试一种新的数据分析系统所需要的下载、部署并学习一个新的软件项目的代价简化成了只需要升级 Spark。

最后，密切结合的原理的一大优点就是，我们能够构建出无缝整合不同处理模型的应用。例如，利用Spark，你可以在一个应用中实现将数据流中的数据使用机器学习算法进行实时分类。与此同时，数据分析师也可以通过SQL 实时查询结果数据，比如将数据与非结构化的日志文件进行连接操作。不仅如此，有经验的数据工程师和数据科学家还可以通过 Python shell 来访问这些数据，进行即时分析。其他人也可以通过独立的批处理应用访问这些数据。IT 团队始终只需要维护一套系统即可。

Spark 的各个组件如图 1-1 所示，下面来依次简要介绍它们。

![](https://upload-images.jianshu.io/upload_images/4311027-9c92831e3702e5bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 1.2.1 Spark Core

Spark Core 实现了 Spark 的基本功能，包含任务调度、内存管理、错误恢复、与存储系统交互等模块。Spark Core 中还包含了对**弹性分布式数据集**（resilient distributed dataset，简称**RDD**）的API 定义。RDD 表示分布在多个计算节点上可以并行操作的元素集合，是 Spark 主要的编程抽象。Spark Core 提供了创建和操作这些集合的多个API。

##### 1.2.2 Spark SQL

Spark SQL 是 Spark 用来操作结构化数据的程序包。通过Spark SQL，我们可以使用 SQL 或者 Apache Hive 版本的 SQL 方言（HQL）来查询数据。Spark SQL 支持多种数据源，比如 Hive 表、Parquet 以及 JSON 等。除了为 Spark 提供了一个SQL 接口，Spark SQL 还支持开发者将 SQL 和传统的 RDD 编程的数据操作方式相结合，不论是使用 Python、Java 还是 Scala，开发者都可以在单个的应用中同时使用 SQL 和复杂的数据分析。通过与 Spark 所提供的丰富的计算环境进行如此紧密的结合，Spark SQL 得以从其他开源数据仓库工具中脱颖而出。Spark SQL 是在Spark 1.0 中被引入的。

在 Spark SQL 之前，加州大学伯克利分校曾经尝试修改 Apache Hive 以使其运行在 Spark 上，当时的项目叫作 Shark。现在，由于 Spark SQL 与 Spark 引擎和 API 的结合更紧密，Shark 已经被 Spark SQL 所取代。

##### 1.2.3 Spark Streaming

Spark Streaming 是 Spark 提供的对实时数据进行流式计算的组件。比如生产环境中的网页服务器日志，或是网络服务中用户提交的状态更新组成的消息队列，都是数据流。Spark Streaming 提供了用来操作数据流的 API，并且与 Spark Core 中的 RDD API 高度对应。这样一来，程序员编写应用时的学习门槛就得以降低，不论是操作内存或硬盘中的数据，还是操作实时数据流，程序员都更能应对自如。从底层设计来看，Spark Streaming 支持与 Spark Core 同级别的容错性、吞吐量以及可伸缩性。

##### 1.2.4 MLlib

Spark 中还包含一个提供常见的机器学习（ML）功能的程序库，叫作 MLlib。MLlib 提供了很多种机器学习算法，包括分类、回归、聚类、协同过滤等，还提供了模型评估、数据导入等额外的支持功能。MLlib 还提供了一些更底层的机器学习原语，包括一个通用的梯度下降优化算法。所有这些方法都被设计为可以在集群上轻松伸缩的架构。

##### 1.2.5 GraphX

GraphX 是用来操作图（比如社交网络的朋友关系图）的程序库，可以进行并行的图计算。与 Spark Streaming 和 Spark SQL 类似，GraphX 也扩展了 Spark 的 RDD API，能用来创建一个顶点和边都包含任意属性的有向图。GraphX 还支持针对图的各种操作（比如进行图分割的 subgraph 和操作所有顶点的 mapVertices），以及一些常用图算法（比如 PageRank 和三角计数）。

##### 1.2.6 集群管理器

就底层而言，Spark 设计为可以高效地在一个计算节点到数千个计算节点之间伸缩计算。为了实现这样的要求，同时获得最大灵活性，Spark 支持在各种集群管理器（cluster manager）上运行，包括 Hadoop YARN、Apache Mesos，以及 Spark 自带的一个简易调度器，叫作独立调度器。如果要在没有预装任何集群管理器的机器上安装Spark，那么 Spark 自带的独立调度器可以让你轻松入门；而如果已经有了一个装有 Hadoop YARN 或 Mesos 的集群，通过Spark 对这些集群管理器的支持，你的应用也同样能运行在这些集群上。第 7 章会详细探讨这些不同的选项以及如何选择合适的集群管理器。

##### 1.3 Spark的用户和用途

Spark 是一个用于集群计算的通用计算框架，因此被用于各种各样的应用程序。在前言中我们提到了本书的两大目标读者人群：数据科学家和工程师。仔细分析这两个群体以及他们使用 Spark 的方式，我们不难发现这两个群体使用 Spark 的典型用例并不一致，不过我们可以把这些用例大致分为两类——数据科学应用和数据处理应用。
当然，这种领域和使用模式的划分是比较模糊的。很多人也兼有数据科学家和工程师的能力，有的时候扮演数据科学家的角色进行研究，然后摇身一变成为工程师，熟练地编写复杂的数据处理程序。不管怎样，分开看这两大群体和相应的用例是很有意义的。

##### 1.3.1 数据科学任务

数据科学是过去几年里出现的新学科，关注的是数据分析领域。尽管没有标准的定义，但我们认为数据科学家（data scientist）就是主要负责分析数据并建模的人。数据科学家有可能具备 SQL、统计、预测建模（机器学习）等方面的经验，以及一定的使用 Python、Matlab 或 R 语言进行编程的能力。将数据转换为更方便分析和观察的格式，通常被称为数据转换（data wrangling），数据科学家也对这一过程中的必要技术有所了解。

数据科学家使用他们的技能来分析数据，以回答问题或发现一些潜在规律。他们的工作流经常会用到即时分析，所以他们可以使用交互式 shell 替代复杂应用的构建，这样可以在最短时间内得到查询语句和一些简单代码的运行结果。Spark 的速度以及简单的 API 都能在这种场景里大放光彩，而 Spark 内建的程序库的支持也使得很多算法能够即刻使用。

Spark 通过一系列组件支持各种数据科学任务。Spark shell 通过提供 Python 和 Scala 的接口，使我们方便地进行交互式数据分析。Spark SQL 也提供一个独立的 SQL shell，我们可以在这个 shell 中使用 SQL 探索数据，也可以通过标准的 Spark 程序或者 Spark shell 来进行 SQL 查询。机器学习和数据分析则通过 MLlib 程序库提供支持。另外，Spark 还能支持调用 R 或者 Matlab 写成的外部程序。数据科学家在使用 R 或 Pandas 等传统数据分析工具时所能处理的数据集受限于单机，而有了Spark，就能处理更大数据规模的问题。

在初始的探索阶段之后，数据科学家的工作需要被应用到实际中。具体问题包括扩展应用的功能、提高应用的稳定性，并针对生产环境进行配置，使之成为业务应用的一部分。例如，在数据科学家完成初始的调研之后，我们可能最终会得到一个生产环境中的推荐系统，可以整合在网页应用中，为用户提供产品推荐。一般来说，将数据科学家的工作转化为实际生产中的应用的工作是由另外的工程师或者工程师团队完成的，而不是那些数据科学家。

##### 1.3.2 数据处理应用

Spark 的另一个主要用例是针对工程师的。在这里，我们把工程师定义为使用Spark 开发生产环境中的数据处理应用的软件开发者。这些开发者一般有基本的软件工程概念，比如
封装、接口设计以及面向对象的编程思想，他们通常有计算机专业的背景，并且能使用工
程技术来设计和搭建软件系统，以实现业务用例。
对工程师来说，Spark 为开发用于集群并行执行的程序提供了一条捷径。通过封装，Spark 不需要开发者关注如何在分布式系统上编程这样的复杂问题，也无需过多关注网络通信和程序容错性。Spark 已经为工程师提供了足够的接口来快速实现常见的任务，以及对应用进行监视、审查和性能调优。其 API 模块化的特性（基于传递分布式的对象集）使得利用程序库进行开发以及本地测试大大简化。

Spark 用户之所以选择Spark 来开发他们的数据处理应用，正是因为Spark 提供了丰富的功能，容易学习和使用，并且成熟稳定。

##### 1.4 Spark简史
Spark 是由一个强大而活跃的开源社区开发和维护的，社区中的开发者们来自许许多多不
同的机构。如果你或者你所在的机构是第一次尝试使用 Spark，也许你会对 Spark 这个项目的历史感兴趣。Spark 是于 2009 年作为一个研究项目在加州大学伯克利分校 RAD  实验室（AMPLab 的前身）诞生。实验室中的一些研究人员曾经用过 Hadoop MapReduce。他们发现MapReduce 在迭代计算和交互计算的任务上表现得效率低下。因此，**Spark 从一开始就是为交互式查询和迭代算法设计的，同时还支持内存式存储和高效的容错机制**。


### 第3章 RDD编程

本章介绍Spark 对数据的核心抽象——弹性分布式数据集（Resilient Distributed Dataset，简称RDD）。RDD 其实就是分布式的元素集合。在 Spark 中，对数据的所有操作不外乎创建 RDD、转化已有 RDD 以及调用 RDD 操作进行求值。而在这一切背后，Spark 会自动将 RDD 中的数据分发到集群上，并将操作并行化执行。

##### 3.1 RDD基础

Spark 中的 RDD 就是一个不可变的分布式对象集合。每个 RDD 都被分为多个分区，这些分区运行在集群中的不同节点上。RDD 可以包含 Python、Java、Scala 中任意类型的对象，甚至可以包含用户自定义的对象。

用户可以使用两种方法创建 RDD：读取一个外部数据集，或在驱动器程序里分发驱动器程
序中的对象集合（比如 list 和 set）。

```
# 例 3-1：在 Python 中过使用 SparkContext.textFile() 来读取文本文件作为一个字符串 RDD
>>> lines = sc.textFile("README.md")
```

创建出来后，RDD 支持两种类型的操作：转化操作（transformation）和行动操作（action）。转化操作会由一个 RDD 生成一个新的 RDD。例如，根据谓词匹配情况筛选数据就是一个常见的转化操作。

```
# 例3-2：调用转化操作 filter()
# 用筛选来生成一个只存储包含单词 Python 的字符串的新的RDD
>>> pythonLines = lines.filter(lambda line: "Python" in line)
```

另一方面，行动操作会对 RDD 计算出一个结果，并把结果返回到驱动器程序中，或把结果存储到外部存储系统（如HDFS）中。

```
例3-3：调用 first() 行动操作返回 RDD 的第一个元素
>>> pythonLines.first()
u'## Interactive Python Shell'
```

转化操作和行动操作的区别在于 Spark 计算 RDD 的方式不同。虽然你可以在任何时候定
义新的 RDD，但 Spark 只会惰性计算这些 RDD。它们只有第一次在一个行动操作中用到
时，才会真正计算。这种策略刚开始看起来可能会显得有些奇怪，不过在大数据领域是很
有道理的。比如，看看例 3-2 和例 3-3，我们以一个文本文件定义了数据，然后把其中包含 Python 的行筛选出来。如果 Spark 在我们运行 lines = sc.textFile(...) 时就把文件中所有的行都读取并存储起来，就会消耗很多存储空间，而我们马上就要筛选掉其中的很多数据。相反， 一旦 Spark 了解了完整的转化操作链之后，它就可以只计算求结果时真正需要的数据。事实上，在行动操作 first() 中，Spark 只需要扫描文件直到找到第一个匹配的行为止，而不需要读取整个文件。

最后，默认情况下，Spark 的RDD 会在你每次对它们进行行动操作时重新计算。如果想在多个行动操作中重用同一个 RDD，可以使用 RDD.persist() 让 Spark 把这个 RDD 缓存下来。我们可以让Spark 把数据持久化到许多不同的地方，可用的选项会在表3-6 中列出。

在第一次对持久化的 RDD 计算之后，Spark 会把 RDD 的内容保存到内存中（以分区方式存储到集群中的各机器上），这样在之后的行动操作中，就可以重用这些数据了。我们也可以把 RDD 缓存到磁盘上而不是内存中。默认不进行持久化可能也显得有些奇怪，不过这对于大规模数据集是很有意义的：如果不会重用该 RDD，我们就没有必要浪费存储空间，Spark 可以直接遍历一遍数据然后计算出结果。

在实际操作中，你会经常用 persist() 来把数据的一部分读取到内存中，并反复查询这部分数据。

```
例 3-4：如果想多次对 README 文件中包含 Python 的行进行计算，可以把 RDD 持久化到内存中
>>> pythonLines.persist
>>> pythonLines.count()
2
>>> pythonLines.first()
u'## Interactive Python Shell'
```

总的来说，每个 Spark 程序或 shell 会话都按如下方式工作。
(1) 从外部数据创建出输入 RDD。
(2) 使用诸如 filter() 这样的转化操作对 RDD 进行转化，以定义新的 RDD。
(3) 告诉 Spark 对需要被重用的中间结果 RDD 执行 persist() 操作。
(4) 使用行动操作（例如 count() 和 first() 等）来触发一次并行计算，Spark 会对计算进行优化后再执行。

###### 3.2 创建RDD

Spark 提供了两种创建 RDD 的方式：
1. 读取外部数据集，
2. 在驱动器程序中对一个集合进行并行化。

创建 RDD 最简单的方式就是把程序中一个已有的集合传给 SparkContext 的 parallelize() 方法。这种方式在学习 Spark 时非常有用，它让你可以在 shell 中快速创建出自己的 RDD，然后对这些 RDD 进行操作。不过，需要注意的是，除了开发原型和测试时，这种方式用得并不多，毕竟这种方式需要把你的整个数据集先放在一台机器的内存中。

```
# 例3-5：Python 中的 parallelize() 方法
lines = sc.parallelize(["pandas", "i like pandas"])

# 例3-6：Scala 中的 parallelize() 方法
val lines = sc.parallelize(List("pandas", "i like pandas"))

# 例3-7：Java 中的 parallelize() 方法
JavaRDD<String> lines = sc.parallelize(Arrays.asList("pandas", "i like pandas"));
```

更常用的方式是从外部存储中读取数据来创建 RDD。外部数据集的读取会在第 5 章详细介绍。不过，我们已经接触了用来将文本文件读入为一个存储字符串的 RDD 的方法 SparkContext.textFile()，用法如例 3-8 至例 3-10 所示。

```
# 例3-8：Python 中的 textFile() 方法
lines = sc.textFile("/path/to/README.md")

# 例3-9：Scala 中的 textFile() 方法
val lines = sc.textFile("/path/to/README.md")

# 例3-10：Java 中的 textFile() 方法
JavaRDD<String> lines = sc.textFile("/path/to/README.md");
```

##### 3.3 RDD操作

我们已经讨论过，RDD 支持两种操作：转化操作和行动操作。RDD 的转化操作是返回一个新的 RDD 的操作，比如 map() 和 filter()，而行动操作则是向驱动器程序返回结果或把结果写入外部系统的操作，会触发实际的计算，比如 count() 和 first()。

Spark 对待转化操作和行动操作的方式很不一样，因此理解你正在进行的操作的类型是很重要的。如果对于一个特定的函数是属于转化操作还是行动操作感到困惑，你可以看看它的返回值类型：转化操作返回的是 RDD，而行动操作返回的是其他的数据类型。

##### 3.3.1 转化操作

RDD 的转化操作是返回新 RDD 的操作。我们会在 3.3.3 节讲到，转化出来的 RDD 是惰性求值的，只有在行动操作中用到这些 RDD 时才会被计算。许多转化操作都是针对各个元素的，也就是说，这些转化操作每次只会操作 RDD 中的一个元素。不过并不是所有的转
化操作都是这样的。
举个例子，假定我们有一个日志文件log.txt，内含有若干消息，希望选出其中的错误消息。我们可以使用前面说过的转化操作 filter()。

```
# 例3-11：用 Python 实现 filter() 转化操作
inputRDD = sc.textFile("log.txt")
errorsRDD = inputRDD.filter(lambda x: "error" in x)

# 例3-12：用 Scala 实现 filter() 转化操作
val inputRDD = sc.textFile("log.txt")
val errorsRDD = inputRDD.filter(line => line.contains("error"))

# 例3-13：用 Java 实现 filter() 转化操作
JavaRDD<String> inputRDD = sc.textFile("log.txt");
JavaRDD<String> errorsRDD = inputRDD.filter(
  new Function<String, Boolean>() {
    public Boolean call(String x) { return x.contains("error"); }
  }
});
```

注意，filter() 操作不会改变已有的inputRDD 中的数据。实际上，该操作会返回一个全新的 RDD。inputRDD 在后面的程序中还可以继续使用，比如我们还可以从中搜索别的单词。

事实上，要再从 inputRDD 中找出所有包含单词 warning 的行。接下来，我们使用另一个转化操作 union() 来打印出包含 error 或 warning 的行数。union() 与 filter() 的不同点在于它操作两个 RDD 而不是一个。转化操作可以操作任意数量的输入 RDD。

```
# 例3-14：用 Python / Scala / Java 进行 union() 转化操作
# 更好的方法是直接筛选出要么包含 error 要么包含 warning 的行，这样只对 inputRDD 进行一次筛选即可
errorsRDD = inputRDD.filter(lambda x: "error" in x)
warningsRDD = inputRDD.filter(lambda x: "warning" in x)
badLinesRDD = errorsRDD.union(warningsRDD)
```

![](https://upload-images.jianshu.io/upload_images/4311027-783336664fbd4962.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过转化操作，你从已有的 RDD 中派生出新的 RDD，Spark 会使用谱系图（lineage graph）来记录这些不同 RDD 之间的依赖关系。Spark 需要用这些信息来按需计算每个 RDD，也可以依靠谱系图在持久化的 RDD 丢失部分数据时恢复所丢失的数据。

##### 3.3.2 行动操作

我们已经看到了如何通过转化操作从已有的 RDD 创建出新的 RDD，不过有时，我们希望
对数据集进行实际的计算。行动操作是第二种类型的 RDD 操作，它们会把最终求得的结
果返回到驱动器程序，或者写入外部存储系统中。由于行动操作需要生成实际的输出，它
们会强制执行那些求值必须用到的 RDD 的转化操作。

继续我们在前几章中用到的日志的例子，我们可能想输出关于 badLinesRDD 的一些信息。为此，需要使用两个行动操作来实现：用 count() 来返回计数结果，用 take() 来收集 RDD 中的一些元素。

```
# 例3-15：在 Python 中使用行动操作对错误进行计数
print "Input had " + badLinesRDD.count() + " concerning lines"
print "Here are 10 examples:"
for line in badLinesRDD.take(10):
  print line

# 例3-16：在 Scala 中使用行动操作对错误进行计数
println("Input had " + badLinesRDD.count() + " concerning lines")
println("Here are 10 examples:")
badLinesRDD.take(10).foreach(println)

# 例3-17：在 Java 中使用行动操作对错误进行计数
System.out.println("Input had " + badLinesRDD.count() + " concerning lines")
System.out.println("Here are 10 examples:")
for (String line: badLinesRDD.take(10)) {
  System.out.println(line);
}
```

在这个例子中，我们在驱动器程序中使用 take() 获取了 RDD 中的少量元素。然后在本地遍历这些元素，并在驱动器端打印出来。RDD 还有一个 collect() 函数，可以用来获取整个 RDD 中的数据。如果你的程序把 RDD 筛选到一个很小的规模，并且你想在本地处理这些数据时，就可以使用它。记住，只有当你的整个数据集能在单台机器的内存中放得下
时，才能使用 collect()，因此，collect() 不能用在大规模数据集上。

在大多数情况下，RDD 不能通过 collect() 收集到驱动器进程中，因为它们一般都很大。此时，我们通常要把数据写到诸如 HDFS 或 Amazon S3 这样的分布式的存储系统中。你可以使用 saveAsTextFile()、saveAsSequenceFile()，或者任意的其他行动操作来把 RDD 的数据内容以各种自带的格式保存起来。我们会在第 5 章讲解导出数据的各种选项。

需要注意的是，每当我们调用一个新的行动操作时，整个 RDD 都会从头开始计算。要避
免这种低效的行为，用户可以将中间结果持久化，这会在 3.6 节中介绍。

##### 3.3.3 惰性求值

前面提过，RDD 的转化操作都是惰性求值的。这意味着在被调用行动操作之前 Spark 不会开始计算。这对新用户来说可能与直觉有些相违背之处，但是对于那些使用过诸如 Haskell 等函数式语言或者类似 LINQ 这样的数据处理框架的人来说，会有些似曾相识。

惰性求值意味着当我们对RDD 调用转化操作（例如调用map()）时，操作不会立即执行。相反，Spark 会在内部记录下所要求执行的操作的相关信息。我们不应该把 RDD 看作存
放着特定数据的数据集，而最好把每个 RDD 当作我们通过转化操作构建出来的、记录如何计算数据的指令列表。把数据读取到 RDD 的操作也同样是惰性的。因此，当我们调用 sc.textFile() 时，数据并没有读取进来，而是在必要时才会读取。和转化操作一样的是，读取数据的操作也有可能会多次执行。

Spark 使用惰性求值，这样就可以把一些操作合并到一起来减少计算数据的步骤。在类似 Hadoop MapReduce 的系统中，开发者常常花费大量时间考虑如何把操作组合到一起，以
减少MapReduce 的周期数。而在 Spark 中，写出一个非常复杂的映射并不见得能比使用很多简单的连续操作获得好很多的性能。因此，用户可以用更小的操作来组织他们的程序，
这样也使这些操作更容易管理。

>虽然转化操作是惰性求值的，但还是可以随时通过运行一个行动操作来强制 Spark 执行RDD 的转化操作，比如使用 count()。这是一种对你所写的程序进行部分测试的简单方法。

##### 3.4 向Spark传递函数

Spark 的大部分转化操作和一部分行动操作，都需要依赖用户传递的函数来计算。在我们
支持的三种主要语言中，向 Spark 传递函数的方式略有区别。

##### 3.4.1 Python

在 Python 中，我们有三种方式来把函数传递给 Spark。
1. 使用lambda 表达式来传递：传递比较短的函数时
2. 传递顶层函数
3. 传递定义的局部函数

```
例3-18：在 Python 中使用lambda 表达式来传递函数
word = rdd.filter(lambda s: "error" in s)
def containsError(s):
  return "error" in s
word = rdd.filter(containsError)
```

传递函数时需要小心的一点是，Python 会在你不经意间把函数所在的对象也序列化传出
去。当你传递的对象是某个对象的成员，或者包含了对某个对象中一个字段的引用时（例如 self.field），Spark 就会把整个对象发到工作节点上，这可能比你想传递的东西大得多（见例3-19）。有时，如果传递的类里面包含 Python 不知道如何序列化传输的对象，也会导致你的程序失败。

```
# 例3-19：传递一个带字段引用的函数（别这么做！）
class SearchFunctions(object):
  def __init__(self, query):
    self.query = query
  def isMatch(self, s):
    return self.query in s
  def getMatchesFunctionReference(self, rdd):
    # 问题：在"self.isMatch"中引用了整个 self
    return rdd.filter(self.isMatch)
  def getMatchesMemberReference(self, rdd):
    # 问题：在"self.query"中引用了整个 self
    return rdd.filter(lambda x: self.query in x)
```

替代的方案是，只把你所需要的字段从对象中拿出来放到一个局部变量中，然后传递这个
局部变量，如例3-20 所示。

```
# 例3-20：传递不带字段引用的Python 函数
class WordFunctions(object):
  ...
  def getMatchesNoReference(self, rdd):
    # 安全：只把需要的字段提取到局部变量中
    query = self.query
    return rdd.filter(lambda x: query in x)
```

##### 3.4.2 Scala

在 Scala 中，我们可以把定义的内联函数、方法的引用或静态方法传递给 Spark，就像 Scala 的其他函数式 API 一样。我们还要考虑其他一些细节，比如所传递的函数及其引用的数据需要是可序列化的（实现了 Java 的 Serializable 接口）。除此以外，与Python 类似，传递一个对象的方法或者字段时，会包含对整个对象的引用。这在 Scala 中不是那么明显，毕竟我们不会像 Python 那样必须用 self 写出那些引用。

类似在例 3-20 中对 Python 执行的操作，我们可以把需要的字段放到一个局部变量中，来避免传递包含该字段的整个对象，如例3-21 所示。

```
# 例3-21：Scala 中的函数传递
# 如果在 Scala 中出现了 NotSerializableException，通常问题就在于我们传递了一个不可序列化的类中的函数或字段
# 传递局部可序列化变量或顶级对象中的函数始终是安全的
class SearchFunctions(val query: String) {
  def isMatch(s: String): Boolean = {
  s.contains(query)
  }
  def getMatchesFunctionReference(rdd: RDD[String]): RDD[String] = {
    // 问题："isMatch"表示"this.isMatch"，因此我们要传递整个"this"
    rdd.map(isMatch)
  }
  def getMatchesFieldReference(rdd: RDD[String]): RDD[String] = {
    // 问题："query"表示"this.query"，因此我们要传递整个"this"
    rdd.map(x => x.split(query))
  }
  def getMatchesNoReference(rdd: RDD[String]): RDD[String] = {
    // 安全：只把我们需要的字段拿出来放入局部变量中
    val query_ = this.query
    rdd.map(x => x.split(query_))
  }
}
```

##### 3.4.3 Java

在 Java 中，函数需要作为实现了 Spark 的 org.apache.spark.api.java.function 包中的任一函数接口的对象来传递。根据不同的返回类型，我们定义了一些不同的接口。

![](https://upload-images.jianshu.io/upload_images/4311027-3d9eabeeb3630b43.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
# 例3-22：在 Java 中使用匿名内部类进行函数传递
RDD<String> errors = lines.filter(new Function<String, Boolean>() {
  public Boolean call(String x) { return x.contains("error"); }
});

# 例3-23：在 Java 中使用具名类进行函数传递
class ContainsError implements Function<String, Boolean>() {
  public Boolean call(String x) { return x.contains("error"); }
}
RDD<String> errors = lines.filter(new ContainsError());

# 例3-24：带参数的 Java 函数类
# 顶级具名类通常在组织大型程序时显得比较清晰，并且可以给它们的构造函数添加参数
class Contains implements Function<String, Boolean>() {
  private String query;
  public Contains(String query) { this.query = query; }
  public Boolean call(String x) { return x.contains(query); }
}
RDD<String> errors = lines.filter(new Contains("error"));

# 例3-25：在 Java 中使用 Java 8 的 lambda 表达式进行函数传递
RDD<String> errors = lines.filter(s -> s.contains("error"));
```

##### 3.5 常见的转化操作和行动操作

##### 3.5.1 基本 RDD

以下转化操作和行动操作受任意数据类型的 RDD 支持。

1.针对各个元素的转化操作

两个最常用的转化操作是 map() 和 filter()。转化操作 map() 接收一个函数，把这个函数用于 RDD 中的每个元素，将函数的返回结果作为结果。我们可以使用 map() 来做各种各样的事情：可以把我们的 URL 集合中的每个 URL 对应的主机名提取出来，也可以简单到只对各个数字求平方值。map() 的返回值类型不需要和输入类型一样。这样如果有一个字符串 RDD，并且我们的 map() 函数是用来把字符串解析并返回一个 Double 值的，那么此时我们的输入 RDD 类型就是 RDD[String]，而输出类型是 RDD[Double]。

![](https://upload-images.jianshu.io/upload_images/4311027-249ba2e28bf2b14a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
# 例3-26：Python 版计算 RDD 中各值的平方
nums = sc.parallelize([1, 2, 3, 4])
squared = nums.map(lambda x: x * x).collect()
for num in squared:
  print "%i " % (num)

# 例3-27：Scala 版计算 RDD 中各值的平方
val input = sc.parallelize(List(1, 2, 3, 4))
val result = input.map(x => x * x)
println(result.collect().mkString(","))

# 例3-28：Java 版计算 RDD 中各值的平方
JavaRDD<Integer> rdd = sc.parallelize(Arrays.asList(1, 2, 3, 4));
JavaRDD<Integer> result = rdd.map(new Function<Integer, Integer>() {
  public Integer call(Integer x) { return x*x; }
});
System.out.println(StringUtils.join(result.collect(), ","));
```

有时候，我们希望对每个输入元素生成多个输出元素。实现该功能的操作叫作 flatMap()。和 map() 类似，我们提供给 flatMap() 的函数被分别应用到了输入 RDD 的每个元素上。不过返回的不是一个元素，而是一个返回值序列的迭代器。输出的 RDD 倒不是由迭代器组成的。我们得到的是一个包含各个迭代器可访问的所有元素的 RDD。flatMap() 的一个简单用途是把输入的字符串切分为单词。

```
# 例3-29：Python 中的 flatMap() 将行数据切分为单词
lines = sc.parallelize(["hello world", "hi"])
words = lines.flatMap(lambda line: line.split(" "))
words.first() # 返回"hello"

# 例3-30：Scala 中的 flatMap() 将行数据切分为单词
val lines = sc.parallelize(List("hello world", "hi"))
val words = lines.flatMap(line => line.split(" "))
words.first() // 返回"hello"

# 例3-31：Java 中的 flatMap() 将行数据切分为单词
JavaRDD<String> lines = sc.parallelize(Arrays.asList("hello world", "hi"));
JavaRDD<String> words = lines.flatMap(new FlatMapFunction<String, String>() {
  public Iterable<String> call(String line) {
    return Arrays.asList(line.split(" "));
  }
});
words.first(); // 返回"hello"
```

![](https://upload-images.jianshu.io/upload_images/4311027-4e012208b914dc4d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.伪集合操作

尽管 RDD 本身不是严格意义上的集合，但它也支持许多数学上的集合操作，比如合并和相交操作。图3-4 展示了四种操作。注意，这些操作都要求操作的 RDD 是相同数据类型的。

![](https://upload-images.jianshu.io/upload_images/4311027-8006db1f6d103173.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


我们的 RDD 中最常缺失的集合属性是元素的唯一性，因为常常有重复的元素。如果只要唯一的元素，我们可以使用 RDD.distinct() 转化操作来生成一个只包含不同元素的新 RDD。不过需要注意，distinct() 操作的开销很大，因为它需要将所有数据通过网络进行混洗（shuffle），以确保每个元素都只有一份。第 4 章会详细介绍数据混洗，以及如何避免数据混洗。

并：最简单的集合操作是 union(other)，它会返回一个包含两个 RDD 中所有元素的 RDD。这在很多用例下都很有用，比如处理来自多个数据源的日志文件。与数学中的 union() 操作不同的是，如果输入的 RDD 中有重复数据，Spark 的 union() 操作也会包含这些重复数据（如有必要，我们可以通过 distinct() 实现相同的效果）。

交：Spark 还提供了 intersection(other) 方法，只返回两个 RDD 中都有的元素。intersection() 在运行时也会去掉所有重复的元素（单个 RDD 内的重复元素也会一起移除）。尽管 intersection() 与 union() 的概念相似，intersection() 的性能却要差很多，因为它需要通过网络混洗数据来发现共有的元素。

差：有时我们需要移除一些数据。subtract(other) 函数接收另一个 RDD 作为参数，返回一个由只存在于第一个RDD 中而不存在于第二个RDD 中的所有元素组成的RDD。和 intersection() 一样，它也需要数据混洗。

笛卡儿积：cartesian(other) 转化操作会返回所有可能的 (a, b) 对，其中 a 是源 RDD 中的元素，而 b 则来自另一个 RDD。笛卡儿积在我们希望考虑所有可能的组合的相似度时比较有用，比如计算各用户对各种产品的预期兴趣程度。我们也可以求一个 RDD 与其自身的笛卡儿积，这可以用于求用户相似度的应用中。不过要特别注意的是，求大规模 RDD 的笛卡儿积开销巨大。

![](https://upload-images.jianshu.io/upload_images/4311027-c36878f174f06f7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

表 3-2 和表 3-3 总结了这些常见的 RDD 转化操作。

![](https://upload-images.jianshu.io/upload_images/4311027-b00af226de1e98dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.行动操作

你很有可能会用到基本 RDD 上最常见的行动操作 reduce()。它接收一个函数作为参数，这个函数要操作两个 RDD 的元素类型的数据并返回一个同样类型的新元素。一个简单的例子就是函数+，可以用它来对我们的 RDD 进行累加。使用 reduce()，可以很方便地计算出 RDD 中所有元素的总和、元素的个数，以及其他类型的聚合操作。

fold() 和 reduce() 类似，接收一个与 reduce() 接收的函数签名相同的函数，再加上一个“初始值”来作为每个分区第一次调用时的结果。你所提供的初始值应当是你提供的操作的单位元素；也就是说，使用你的函数对这个初始值进行多次计算不会改变结果（例如 + 对应的 0，* 对应的 1，或拼接操作对应的空列表）。可以通过原地修改并返回两个参数中的前一个的值来节约在fold() 中创建对象的开销。但是没有办法修改第二个参数。

fold() 和 reduce() 都要求函数的返回值类型需要和我们所操作的 RDD 中的元素类型相同。这很符合像 sum 这种操作的情况。但有时我们确实需要返回一个不同类型的值。例如，在计算平均值时，需要记录遍历过程中的计数以及元素的数量，这就需要我们返回一个二元组。可以先对数据使用 map() 操作，来把元素转为该元素和 1 的二元组，也就是我们所希望的返回类型。这样reduce() 就可以以二元组的形式进行归约了。

```
# 例3-32：Python 中的 reduce()
sum = rdd.reduce(lambda x, y: x + y)

# 例3-33：Scala 中的 reduce()
val sum = rdd.reduce((x, y) => x + y)

# 例3-34：Java 中的 reduce()
Integer sum = rdd.reduce(new Function2<Integer, Integer, Integer>() {
  public Integer call(Integer x, Integer y) { return x + y; }
});
```

aggregate() 函数则把我们从返回值类型必须与所操作的 RDD 类型相同的限制中解放出来。与 fold() 类似，使用 aggregate() 时，需要提供我们期待返回的类型的初始值。然后通过一个函数把 RDD 中的元素合并起来放入累加器。考虑到每个节点是在本地进行累加的，最终，还需要提供第二个函数来将累加器两两合并。

可以用 aggregate() 来计算 RDD 的平均值，来代替 map() 后面接fold() 的方式。

```
# 例3-35：Python 中的 aggregate()
sumCount = nums.aggregate((0, 0),
               (lambda acc, value: (acc[0] + value, acc[1] + 1),
               (lambda acc1, acc2: (acc1[0] + acc2[0], acc1[1] + acc2[1]))))
return sumCount[0] / float(sumCount[1])

# 例3-36：Scala 中的 aggregate()
val result = input.aggregate((0, 0))(
               (acc, value) => (acc._1 + value, acc._2 + 1),
               (acc1, acc2) => (acc1._1 + acc2._1, acc1._2 + acc2._2))
val avg = result._1 / result._2.toDouble

# 例3-37：Java 中的 aggregate()
class AvgCount implements Serializable {
  public AvgCount(int total, int num) {
    this.total = total;
    this.num = num;
  }
  public int total;
  public int num;
  public double avg() {
    return total / (double) num;
  }
}
Function2<AvgCount, Integer, AvgCount> addAndCount =
  new Function2<AvgCount, Integer, AvgCount>() {
    public AvgCount call(AvgCount a, Integer x) {
      a.total += x;
      a.num += 1;
      return a;
    }
};
Function2<AvgCount, AvgCount, AvgCount> combine =
  new Function2<AvgCount, AvgCount, AvgCount>() {
    public AvgCount call(AvgCount a, AvgCount b) {
      a.total += b.total;
      a.num += b.num;
      return a;
    }
};
AvgCount initial = new AvgCount(0, 0);
AvgCount result = rdd.aggregate(initial, addAndCount, combine);
System.out.println(result.avg());
```

RDD 的一些行动操作会以普通集合或者值的形式将 RDD 的部分或全部数据返回驱动器程序中。

`collect()`：将整个 RDD 的内容返回。通常在单元测试中使用，因为此时 RDD 的整个内容不会很大，可以放在内存中，使得 RDD 的值与预期结果之间的对比变得很容易。由于需要将数据复制到驱动器进程中，该操作要求所有数据都必须能一同放入单台机器的内存中。

`take(n)`：返回 RDD 中的 n 个元素，并且尝试只访问尽量少的分区，因此该操作会得到一个不均衡的集合。需要注意的是，这些操作返回元素的顺序与你预期的可能不一样。

这些操作对于单元测试和快速调试都很有用，但是在处理大规模数据时会遇到瓶颈。

`top()`：如果为数据定义了顺序，就可以使用该操作从 RDD 中获取前几个元素。此时会使用数据的默认顺序，但我们也可以提供自己的比较函数，来提取前几个元素。

`takeSample(withReplacement, num, seed)`：可以从数据中获取一个采样，并指定是否替换。

`foreach()`：有时我们会对 RDD 中的所有元素应用一个行动操作，但是不把任何结果返回到驱动器程序中，这也是有用的。比如可以用 JSON 格式把数据发送到一个网络服务器上，或者把数据存到数据库中。都可以使用该操作来对 RDD 中的每个元素进行操作，而不需要把 RDD 发回本地。

![](https://upload-images.jianshu.io/upload_images/4311027-a7e94f6a529f152d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 3.5.2 在不同RDD类型间转换

有些函数只能用于特定类型的 RDD，比如 mean() 和 variance() 只能用在数值RDD 上，而 join() 只能用在键值对 RDD 上。我们会在第 6 章讨论数值 RDD 的专门函数，在第 4 章讨论键值对 RDD 的专有操作。在 Scala 和 Java 中，这些函数都没有定义在标准的 RDD 类中，所以要访问这些附加功能，必须要确保获得了正确的专用 RDD 类。

1.Scala
在 Scala 中，将 RDD 转为有特定函数的 RDD（比如在 RDD[Double] 上进行数值操作）是由隐式转换来自动处理的。这些隐式转换可以隐式地将一个 RDD 转为各种封装类，比如 DoubleRDDFunctions
（数值数据的 RDD）和 PairRDDFunctions（键值对 RDD），这样我们就有了诸如 mean() 和 variance() 之类的额外的函数。

2.Java
在 Java 中，各种 RDD 的特殊类型间的转换更为明确。Java 中有两个专门的类 JavaDoubleRDD 和  JavaPairRDD，来处理特殊类型的 RDD，这两个类还针对这些类型提供了额外的函数。这让你可以更加了解所发生的一切，但是也显得有些累赘。

要构建出这些特殊类型的 RDD，需要使用特殊版本的类来替代一般使用的 Function 类。如果
要从 T 类型的 RDD 创建出一个 DoubleRDD，我们就应当在映射操作中使用 DoubleFunction<T> 来替代 Function<T, Double>。表 3-5 展示了一些特殊版本的函数类及其用法。

![](https://upload-images.jianshu.io/upload_images/4311027-1d2535b7aa13eba2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此外，我们也需要调用 RDD 上的一些别的函数（因此不能只是创建出一个 DoubleFunction
然后把它传给map()）。当需要一个 DoubleRDD 时，我们应当调用 mapToDouble() 来替代 map()，跟其他所有函数所遵循的模式一样。

```
# 例3-38：用 Java 创建 DoubleRDD
# 生成一个 JavaDoubleRDD、计算 RDD 中每个元素的平方值的示例，这样就可以调用 DoubleRDD 独有的函数了，比如 mean() 和 variance()
JavaDoubleRDD result = rdd.mapToDouble(
  new DoubleFunction<Integer>() {
    public double call(Integer x) {
    return (double) x * x;
  }
});
System.out.println(result.mean());
```

3.Python
Python 的 API 结构与 Java 和 Scala 有所不同。在 Python 中，所有的函数都实现在基本的 RDD 类中，但如果操作对应的 RDD 数据类型不正确，就会导致运行时错误。

##### 3.6 持久化(缓存)

如前所述，Spark RDD 是惰性求值的，而有时我们希望能多次使用同一个 RDD。如果简单地对 RDD 调用行动操作，Spark 每次都会重算 RDD 以及它的所有依赖。这在迭代算法中消耗格外大，因为迭代算法常常会多次使用同一组数据。

```
# 例3-39：Scala 中的两次执行
# 先对RDD 作一次计数，再把该 RDD 输出
val result = input.map(x => x*x)
println(result.count())
println(result.collect().mkString(","))
```

为了避免多次计算同一个 RDD，可以让 Spark 对数据进行持久化。当我们让 Spark 持久化存储一个 RDD 时，计算出 RDD 的节点会分别保存它们所求出的分区数据。如果一个有持久化数据的节点发生故障，Spark 会在需要用到缓存的数据时重算丢失的数据分区。如果希望节点故障的情况不会拖累我们的执行速度，也可以把数据备份到多个节点上。

出于不同的目的，我们可以为 RDD 选择不同的持久化级别。

![](https://upload-images.jianshu.io/upload_images/4311027-72573a42611a8865.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
# 例3-40：在 Scala 中使用 persist() 对数据进行缓存
# 在 Scala 和 Java 中，默认情况下 persist() 会把数据以序列化的形式缓存在 JVM 的堆空间中。
# 在 Python 中，会始终序列化要持久化存储的数据，所以持久化级别默认值就是以序列化后的对象存储在 JVM 堆空间中。
# 当我们把数据写到磁盘或者堆外存储上时，也总是使用序列化后的数据。
# 在第一次对这个 RDD 调用行动操作前就调用了 persist() 方法。persist() 调用本身不会触发强制求值。
# RDD 还有一个方法叫作 unpersist()，调用该方法可以手动把持久化的 RDD 从缓存中移除。
val result = input.map(x => x * x)
result.persist(StorageLevel.DISK_ONLY)
println(result.count())
println(result.collect().mkString(","))
```

如果要缓存的数据太多，内存中放不下，Spark 会自动利用最近最少使用（LRU）的缓存策略把最老的分区从内存中移除。对于仅把数据存放在内存中的缓存级别，下一次要用到已经被移除的分区时，这些分区就需要重新计算。但是对于使用内存与磁盘的缓存级别的分区来说，被移除的分区都会写入磁盘。不论哪一种情况，都不必担心你的作业因为缓存了太多数据而被打断。不过，缓存不必要的数据会导致有用的数据被移出内存，带来更多重算的时间开销。

##### 3.7 总结

在本章中，我们介绍了 RDD 运行模型以及 RDD 的许多常见操作。如果你读到了这里，恭喜——你已经学完了 Spark 的所有核心概念。我们在进行并行聚合、分组等操作时，常常需要利用键值对形式的 RDD。下一章会讲解键值对形式的 RDD 上一些相关的特殊操作。然后，我们会讨论各种数据源的输入输出，以及一些关于使用 SparkContext 的进阶话题。


### 第4章 键值对操作

**键值对 RDD** 是 Spark 中许多操作所需要的常见数据类型。本章就来介绍如何操作键值对 RDD。键值对 RDD 通常用来进行聚合计算。我们一般要先通过一些初始ETL（抽取、转化、装载）操作来将数据转化为键值对形式。键值对 RDD 提供了一些新的操作接口（比如统计每个产品的评论，将数据中键相同的分为一组，将两个不同的 RDD 进行分组合并等）。

本章也会讨论用来让用户控制键值对 RDD 在各节点上分布情况的高级特性：分区。有时，使用可控的分区方式把常被一起访问的数据放到同一个节点上，可以大大减少应用的通信开销。这会带来明显的性能提升。我们会使用 PageRank 算法来演示分区的作用。为分布式数据集选择正确的分区方式和为本地数据集选择合适的数据结构很相似——在这两种情况下，数据的分布都会极其明显地影响程序的性能表现。

##### 4.1 动机

Spark 为包含键值对类型的 RDD 提供了一些专有的操作。这些 RDD 被称为 pair RDD。Pair RDD 是很多程序的构成要素，因为它们提供了并行操作各个键或跨节点重新进行数据分组的操作接口。例如，pair RDD 提供 reduceByKey() 方法，可以分别归约每个键对应的数据，还有 join() 方法，可以把两个 RDD 中键相同的元素组合到一起，合并为一个 RDD。我们通常从一个 RDD 中提取某些字段（例如代表事件时间、用户ID 或者其他标识符的字段），并使用这些字段作为 pair RDD 操作中的键。、

##### 4.2 创建 Pair RDD

在 Spark 中有很多种创建 pair RDD 的方式。第 5 章会讲到，很多存储键值对的数据格式会在读取时直接返回由其键值对数据组成的 pair RDD。此外，当需要把一个普通的 RDD 转为 pair RDD 时，可以调用 map() 函数来实现，传递的函数需要返回键值对。

构建键值对 RDD 的方法在不同的语言中会有所不同。

在 Python 中，为了让提取键之后的数据能够在函数中使用，需要返回一个由二元组组成的RDD（见例4-1）。

```
# 例4-1：在Python 中使用第一个单词作为键创建出一个 pair RDD
# 在Python 中，为了让提取键之后的数据能够在函数中使用，需要返回一个由二元组组成的 RDD
# 要从一个内存中的数据集创建 pair RDD 时，只需要对这个由二元组组成的集合调用 SparkContext.parallelize() 方法。
pairs = lines.map(lambda x: (x.split(" ")[0], x))

# 例4-2：在 Scala 中使用第一个单词作为键创建出一个 pair RDD
# 在 Scala 中，为了让提取键之后的数据能够在函数中使用，同样需要返回二元组，隐式转换可以让二元组 RDD 支持附加的键值对函数
# 要从一个内存中的数据集创建 pair RDD 时，只需要对这个由二元组组成的集合调用 SparkContext.parallelize() 方法。
val pairs = lines.map(x => (x.split(" ")(0), x))

# 例4-3：在 Java 中使用第一个单词作为键创建出一个 pair RDD
# Java 没有自带的二元组类型，因此 Spark 的 Java API 让用户使用 scala.Tuple2 类来创建二
元组。
# 这个类很简单：Java 用户可以通过new Tuple2(elem1, elem2) 来创建一个新的二元组，并且可以通过._1() 和._2() 方法访问其中的元素。
# Java 用户还需要调用专门的 Spark 函数来创建 pair RDD。例如，要使用 mapToPair() 函数
来代替基础版的 map() 函数。
# 而要使用 Java 从内存数据集创建 pair RDD 的话，则需要使用 SparkContext.parallelizePairs()。
PairFunction<String, String, String> keyData =
  new PairFunction<String, String, String>() {
    public Tuple2<String, String> call(String x) {
      return new Tuple2(x.split(" ")[0], x);
    }
};
JavaPairRDD<String, String> pairs = lines.mapToPair(keyData);
```

##### 4.3 Pair RDD 的转化操作

Pair RDD 可以使用所有标准 RDD 上的可用的转化操作。

由于pair RDD 中包含二元组，所以需要传递的函数应当操作二元组而不是独立的元素。

![](https://upload-images.jianshu.io/upload_images/4311027-c88ad7fbf08283aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/4311027-928b952c3ecc7632.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/4311027-539962a871cb2731.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Pair RDD 也还是 RDD（元素为 Java 或 Scala 中的 Tuple2 对象或 Python 中的元组），因此同样支持 RDD 所支持的函数。

```
# 拿前一节中的 pair RDD，筛选掉长度超过 20 个字符的行
# 例4-4：用 Python 对第二个元素进行筛选
result = pairs.filter(lambda keyValue: len(keyValue[1]) < 20)

# 例4-5：用 Scala 对第二个元素进行筛选
pairs.filter{case (key, value) => value.length < 20}

# 例4-6：用 Java 对第二个元素进行筛选
Function<Tuple2<String, String>, Boolean> longWordFilter =
  new Function<Tuple2<String, String>, Boolean>() {
    public Boolean call(Tuple2<String, String> keyValue) {
      return (keyValue._2().length() < 20);
    }
};
JavaPairRDD<String, String> result = pairs.filter(longWordFilter);
```

![](https://upload-images.jianshu.io/upload_images/4311027-6758422e29361634.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

有时，我们只想访问 pair RDD 的值部分，这时操作二元组很麻烦。由于这是一种常见的使用模式，因此 Spark 提供了 mapValues(func) 函数，功能类似于 map{case (x, y): (x, func(y))}。可以在很多例子中使用这个函数。

接下来就依次讨论pair RDD 的各种操作。

##### 4.3.1 聚合操作

当数据集以键值对形式组织的时候，聚合具有相同键的元素进行一些统计是很常见的操作。之前讲解过基础 RDD 上的 fold()、combine()、reduce() 等行动操作，pair RDD 上则有相应的针对键的转化操作。Spark 有一组类似的操作，可以组合具有相同键的值。这些操作返回 RDD，因此它们是转化操作而不是行动操作。

`reduceByKey()` 与 `reduce()` 相当类似：它们都接收一个函数，并使用该函数对值进行合并。该操作为数据集中的每个键进行并行的归约操作，每个归约操作会将键相同的值合并起来。因为数据集中可能有大量的键，所以该操作没有被实现为向用户程序返回一个值的行动操作。实际上，它会返回一个由各键和对应键归约出来的结果值组成的新的 RDD。

`foldByKey()` 则与`fold()` 相当类似：它们都使用一个与 RDD 和合并函数中的数据类型相同的零值作为初始值。这两个操作所使用的合并函数对零值与另一个元素进行合并，结果仍为该元素。

reduceByKey() 和 mapValues() 类似：用来计算每个键的对应值的均值。这和使用 fold() 和 map() 计算整个 RDD 平均值的过程很相似。

对于求平均，可以使用更加专用的函数来获取同样的结果，后面就会讲到。

熟悉 MapReduce 中的合并器（combiner）概念的读者可能已经注意到，调用 reduceByKey() 和 foldByKey() 会在为每个键计算全局的总结果之前先自动在每台机器上进行本地合并。用户不需要指定合并器。更泛化的 combineByKey() 接口可以让你自定义合并的行为。

![](https://upload-images.jianshu.io/upload_images/4311027-89cb5d302e5773c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
# 例4-7：在 Python 中使用 reduceByKey() 和 mapValues() 计算每个键对应的平均值
rdd.mapValues(lambda x: (x, 1)).reduceByKey(lambda x, y: (x[0] + y[0], x[1] + y[1]))

# 例4-8：在 Scala 中使用 reduceByKey() 和 mapValues() 计算每个键对应的平均值
rdd.mapValues(x => (x, 1)).reduceByKey((x, y) => (x._1 + y._1, x._2 + y._2))
```

```
# 可以使用 flatMap() 来生成以单词为键、以数字 1 为值的 pair RDD，然后使用 reduceByKey() 对所有的单词进行计数
# 事实上，我们可以对第一个 RDD 使用 countByValue() 函数，以更快地实现单词计数：input.flatMap(x => x.split(" ")).countByValue()
# 例4-9：用 Python 实现单词计数
rdd = sc.textFile("s3://...")
words = rdd.flatMap(lambda x: x.split(" "))
result = words.map(lambda x: (x, 1)).reduceByKey(lambda x, y: x + y)

# 例4-10：用 Scala 实现单词计数
val input = sc.textFile("s3://...")
val words = input.flatMap(x => x.split(" "))
val result = words.map(x => (x, 1)).reduceByKey((x, y) => x + y)

# 例4-11：用 Java 实现单词计数
JavaRDD<String> input = sc.textFile("s3://...")
JavaRDD<String> words = rdd.flatMap(new FlatMapFunction<String, String>() {
  public Iterable<String> call(String x) { return Arrays.asList(x.split(" ")); }
});
JavaPairRDD<String, Integer> result = words.mapToPair(
  new PairFunction<String, String, Integer>() {
    public Tuple2<String, Integer> call(String x) { return new Tuple2(x, 1); }
  }).reduceByKey(
  new Function2<Integer, Integer, Integer>() {
    public Integer call(Integer a, Integer b) { return a + b; }
});
```

combineByKey() 是最为常用的基于键进行聚合的函数。大多数基于键聚合的函数都是用它实现的。和 aggregate() 一样，combineByKey() 可以让用户返回与输入数据的类型不同的返回值。

要理解 combineByKey()， 要先理解它在处理数据时是如何处理每个元素的。由于 combineByKey() 会遍历分区中的所有元素，因此每个元素的键要么还没有遇到过，要么就和之前的某个元素的键相同。

1. 如果这是一个新的元素，combineByKey() 会使用一个叫作 createCombiner() 的函数来创建
那个键对应的累加器的初始值。需要注意的是，这一过程会在每个分区中第一次出现各个键时发生，而不是在整个RDD 中第一次出现一个键时发生。
2. 如果这是一个在处理当前分区之前已经遇到的键，它会使用 mergeValue() 方法将该键的累加器对应的当前值与这个新的值进行合并。

由于每个分区都是独立处理的，因此对于同一个键可以有多个累加器。如果有两个或者更多的分区都有对应同一个键的累加器，就需要使用用户提供的 mergeCombiners() 方法将各个分区的结果进行合并。

如果已知数据在进行 combineByKey() 时无法从 map 端聚合中获益的话，可以禁用它。例如，由于聚合函数（追加到一个队列）无法在 map 端聚合时节约任何空间，groupByKey() 就把它禁用了。如果希望禁用 map 端组合，就需要指定分区方式。就目前而言，你可以通过传递 rdd.partitioner 来直接使用源 RDD 的分区方式。

combineByKey() 有多个参数分别对应聚合操作的各个阶段，因而非常适合用来解释聚合操作各个阶段的功能划分。

```
# 例4-12：在 Python 中使用 combineByKey() 求每个键对应的平均值
sumCount = nums.combineByKey((lambda x: (x,1)),
                             (lambda x, y: (x[0] + y, x[1] + 1)),
                             (lambda x, y: (x[0] + y[0], x[1] + y[1])))
sumCount.map(lambda key, xy: (key, xy[0]/xy[1])).collectAsMap()

# 例4-13：在 Scala 中使用 combineByKey() 求每个键对应的平均值
val result = input.combineByKey(
  (v) => (v, 1),
  (acc: (Int, Int), v) => (acc._1 + v, acc._2 + 1),
  (acc1: (Int, Int), acc2: (Int, Int)) => (acc1._1 + acc2._1, acc1._2 + acc2._2)
  ).map{ case (key, value) => (key, value._1 / value._2.toFloat) }
  result.collectAsMap().map(println(_))

# 例4-14：在 Java 中使用 combineByKey() 求每个键对应的平均值
public static class AvgCount implements Serializable {
  public AvgCount(int total, int num) { total_ = total; num_ = num; }
  public int total_;
  public int num_;
  public float avg() { returntotal_/(float)num_; }
}
Function<Integer, AvgCount> createAcc = new Function<Integer, AvgCount>() {
  public AvgCount call(Integer x) {
    return new AvgCount(x, 1);
  }
};
Function2<AvgCount, Integer, AvgCount> addAndCount =
  new Function2<AvgCount, Integer, AvgCount>() {
    public AvgCount call(AvgCount a, Integer x) {
      a.total_ += x;
      a.num_ += 1;
      return a;
    }
};
Function2<AvgCount, AvgCount, AvgCount> combine =
  new Function2<AvgCount, AvgCount, AvgCount>() {
    public AvgCount call(AvgCount a, AvgCount b) {
      a.total_ += b.total_;
      a.num_ += b.num_;
      return a;
    }
};
AvgCount initial = new AvgCount(0,0);
JavaPairRDD<String, AvgCount> avgCounts =
  nums.combineByKey(createAcc, addAndCount, combine);
Map<String, AvgCount> countMap = avgCounts.collectAsMap();
for (Entry<String, AvgCount> entry : countMap.entrySet()) {
  System.out.println(entry.getKey() + ":" + entry.getValue().avg());
}
```

![](https://upload-images.jianshu.io/upload_images/4311027-424c4678e74cb5da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

有很多函数可以进行基于键的数据合并。它们中的大多数都是在 combineByKey() 的基础上实现的，为用户提供了更简单的接口。不管怎样，在Spark 中使用这些专用的聚合函数，始终要比手动将数据分组再归约快很多。

**并行度调优**

到目前为止，我们已经讨论了所有的转化操作的分发方式，但是还没有探讨Spark 是怎样确定如何分割工作的。每个 RDD 都有固定数目的分区，**分区数决定了在 RDD 上执行操作时的并行度**。

在执行聚合或分组操作时，可以要求 Spark 使用给定的分区数。Spark 始终尝试根据集群的大小推断出一个有意义的默认值，但是有时候你可能要对并行度进行调优来获取更好的性能表现。

本章讨论的大多数操作符都能接收第二个参数，这个参数用来指定分组结果或聚合结果的
RDD 的分区数，如例4-15 和例4-16 所示。

```
# 例4-15：在 Python 中自定义 reduceByKey() 的并行度
data = [("a", 3), ("b", 4), ("a", 1)]
sc.parallelize(data).reduceByKey(lambda x, y: x + y) # 默认并行度
sc.parallelize(data).reduceByKey(lambda x, y: x + y, 10) # 自定义并行度

# 例4-16：在 Scala 中自定义 reduceByKey() 的并行度
val data = Seq(("a", 3), ("b", 4), ("a", 1))
sc.parallelize(data).reduceByKey((x, y) => x + y) // 默认并行度
sc.parallelize(data).reduceByKey((x, y) => x + y) // 自定义并行度
```

有时，我们希望在除分组操作和聚合操作之外的操作中也能改变 RDD 的分区。对于这样的情况，Spark 提供了 repartition() 函数。它会把数据通过网络进行混洗，并创建出新的分区集合。切记，对数据进行重新分区是代价相对比较大的操作。Spark 中也有一个优化版的 repartition()，叫作 coalesce()。你可以使用 Java 或 Scala 中的 rdd.partitions.size() 以及 Python 中的 rdd.getNumPartitions 查看 RDD 的分区数，并确保调用 coalesce() 时将 RDD 合并到比现在的分区数更少的分区中。

##### .3.2 数据分组

对于有键的数据，一个常见的用例是将数据根据键进行分组——比如查看一个顾客的所有订单。

如果数据已经以预期的方式提取了键，groupByKey() 就会使用RDD 中的键来对数据进行分组。对于一个由类型 K 的键和类型 V 的值组成的 RDD，所得到的结果 RDD 类型会是[K, Iterable[V]]。

groupBy() 可以用于未成对的数据上，也可以根据除键相同以外的条件进行分组。它可以接收一个函数，对源 RDD 中的每个元素使用该函数，将返回结果作为键再进行分组。

如果你发现自己写出了先使用 groupByKey() 然后再对值使用 reduce() 或者 fold() 的代码，你很有可能可以通过使用一种根据键进行聚合的函数来更高效地实现同样的效果。对每个键归约数据，返回对应每个键的归约值的 RDD，而不是把 RDD 归约为一个内存中的值。例如，rdd.reduceByKey(func) 与 rdd.groupByKey().mapValues(value => value.reduce(func)) 等价，但是前者更为高效，因为它避免了为每个键创建存放值的列表的步骤。

除了对单个 RDD 的数据进行分组，还可以使用一个叫作 cogroup() 的函数对多个共享同一个键的 RDD 进行分组。对两个键的类型均为 K 而值的类型分别为 V 和 W 的 RDD 进行 cogroup() 时，得到的结果 RDD 类型为 [(K, (Iterable[V], Iterable[W]))]。如果其中的一个 RDD 对于另一个 RDD 中存在的某个键没有对应的记录，那么对应的迭代器则为空。

下一节中要讲的连接操作的构成要素 cogroup() 提供了为多个 RDD 进行数据分组的方法。不仅可以用于实现连接操作，还可以用来求键的交集。除此之外，还能同时应用于三个及以上的 RDD。

##### 4.3.3 连接

将有键的数据与另一组有键的数据一起使用是对键值对数据执行的最有用的操作之一。连接数据可能是 pair RDD 最常用的操作之一。连接方式多种多样：右外连接、左外连接、交叉连接以及内连接。

内连接：普通的 join 操作符表示内连接。只有在两个 pair RDD 中都存在的键才会输出。当一个输入对应的某个键有多个值时，生成的 pair RDD 会包括来自两个输入 RDD 的每一组相对应的记录。

```
例4-17：在Scala shell 中进行内连接
storeAddress = {
  (Store("Ritual"), "1026 Valencia St"),
  (Store("Philz"), "748 Van Ness Ave"),
  (Store("Philz"), "3101 24th St"), 
  (Store("Starbucks"), "Seattle")
}
storeRating = {
  (Store("Ritual"), 4.9), 
  (Store("Philz"), 4.8))
}
storeAddress.join(storeRating) == {
  (Store("Ritual"), ("1026 Valencia St", 4.9)),
  (Store("Philz"), ("748 Van Ness Ave", 4.8)),
  (Store("Philz"), ("3101 24th St", 4.8))
}
```

左外连接 / 右外连接：leftOuterJoin(other) 和 rightOuterJoin(other) 都会根据键连接两个RDD，但是允许结果中存在其中的一个 pair RDD 所缺失的键。

在使用 leftOuterJoin() 产生的 pair RDD 中，源 RDD 的每一个键都有对应的记录。每个键相应的值是由一个源 RDD 中的值与一个包含第二个 RDD 的值的 Option（在 Java 中为 Optional）对象组成的二元组。在 Python 中，如果一个值不存在，则使用 None 来表示；而数据存在时就用常规的值来表示，不使用任何封装。和 join() 一样，每个键可以得到多条记录；当这种情况发生时，我们会得到两个 RDD 中对应同一个键的两组值的笛卡尔积。

```
例4-18：leftOuterJoin() 与 rightOuterJoin()
storeAddress.leftOuterJoin(storeRating) == {
  (Store("Ritual"),("1026 Valencia St",Some(4.9))),
  (Store("Starbucks"),("Seattle",None)),
  (Store("Philz"),("748 Van Ness Ave",Some(4.8))),
  (Store("Philz"),("3101 24th St",Some(4.8)))
}
storeAddress.rightOuterJoin(storeRating) == {
  (Store("Ritual"),(Some("1026 Valencia St"),4.9)),
  (Store("Philz"),(Some("748 Van Ness Ave"),4.8)),
  (Store("Philz"), (Some("3101 24th St"),4.8))
}
```

##### 4.3.4 数据排序

很多时候，让数据排好序是很有用的，尤其是在生成下游输出时。如果键有已定义的顺序，就可以对这种键值对 RDD 进行排序。当把数据排好序后，后续对数据进行 collect() 或 save() 等操作都会得到有序的数据。

我们经常要将 RDD 倒序排列，因此 sortByKey() 函数接收一个叫作 ascending 的参数，表示我们是否想要让结果按升序排序（默认值为 true）。有时我们也可能想按完全不同的排序依据进行排序。要支持这种情况，我们可以提供自定义的比较函数。

```
# 将整数转为字符串，然后使用字符串比较函数来对 RDD 进行排序
# 例4-19：在 Python 中以字符串顺序对整数进行自定义排序
rdd.sortByKey(ascending=True, numPartitions=None, keyfunc = lambda x: str(x))

# 例4-20：在 Scala 中以字符串顺序对整数进行自定义排序
val input: RDD[(Int, Venue)] = ...
implicit val sortIntegersByString = new Ordering[Int] {
  override def compare(a: Int, b: Int) = a.toString.compare(b.toString)
}
rdd.sortByKey()

# 例4-21：在 Java 中以字符串顺序对整数进行自定义排序
class IntegerComparator implements Comparator<Integer> {
  public int compare(Integer a, Integer b) {
    return String.valueOf(a).compareTo(String.valueOf(b))
  }
}
rdd.sortByKey(comp)
```

##### 4.4 Pair RDD 的行动操作

和转化操作一样，所有基础 RDD 支持的传统行动操作也都在 pair RDD 上可用。Pair RDD 提供了一些额外的行动操作，可以让我们充分利用数据的键值对特性。

![](https://upload-images.jianshu.io/upload_images/4311027-75a194fa466d9fba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

就 pair RDD 而言，还有别的一些行动操作可以保存 RDD，会在第 5 章介绍。

##### 4.5 数据分区（进阶）

本章要讨论的最后一个 Spark 特性是对数据集在节点间的分区进行控制。在分布式程序中，通信的代价是很大的，因此控制数据分布以获得最少的网络传输可以极大地提升整体性能。和单节点的程序需要为记录集合选择合适的数据结构一样，**Spark 程序可以通过控制 RDD 分区方式来减少通信开销**。分区并不是对所有应用都有好处的——比如，如果给定 RDD 只需要被扫描一次，我们完全没有必要对其预先进行分区处理。只有当数据集多次在诸如连接这种基于键的操作中使用时，分区才会有帮助。

Spark 中所有的键值对 RDD 都可以进行分区。系统会根据一个针对键的函数对元素进行分组。尽管 Spark 没有给出显示控制每个键具体落在哪一个工作节点上的方法（部分原因是 Spark 即使在某些节点失败时依然可以工作），但 Spark 可以确保同一组的键出现在同一个节点上。比如，你可能使用哈希分区将一个 RDD 分成了 100 个分区，此时键的哈希值对 100 取模的结果相同的记录会被放在一个节点上。你也可以使用范围分区法，将键在同一个范围区间内的记录都放在同一个节点上。

举个简单的例子，我们分析这样一个应用，它在内存中保存着一张很大的用户信息表——也就是一个由 (UserID, UserInfo) 对组成的 RDD，其中 UserInfo 包含一个该用户所订阅的主题的列表。该应用会周期性地将这张表与一个小文件进行组合，这个小文件中存着过去五分钟内发生的事件——其实就是一个由(UserID, LinkInfo) 对组成的表，存放着过去五分钟内某网站各用户的访问情况。例如，我们可能需要对用户访问其未订阅主题的页面的情况进行统计。我们可以使用 Spark 的 join() 操作来实现这个组合操作，其中需要把 UserInfo 和 LinkInfo 的有序对根据 UserID 进行分组。

```
例4-22：简单的Scala 应用
// 初始化代码：从 HDFS 上的一个 Hadoop SequenceFile 中读取用户信息
// userData 中的元素会根据它们被读取时的来源，即 HDFS 块所在的节点来分布
// Spark 此时无法获知某个特定的 UserID 对应的记录位于哪个节点上
val sc = new SparkContext(...)
val userData = sc.sequenceFile[UserID, UserInfo]("hdfs://...").persist()
// 周期性调用函数来处理过去五分钟产生的事件日志
// 假设这是一个包含 (UserID, LinkInfo) 对的 SequenceFile
def processNewLogs(logFileName: String) {
  val events = sc.sequenceFile[UserID, LinkInfo](logFileName)
  val joined = userData.join(events)// RDD of (UserID, (UserInfo, LinkInfo)) pairs
  val offTopicVisits = joined.filter {
  case (userId, (userInfo, linkInfo)) => // Expand the tuple into its components
    !userInfo.topics.contains(linkInfo.topic)
  }.count()
  println("Number of visits to non-subscribed topics: " + offTopicVisits)
}
```

这段代码可以正确运行，但是不够高效。这是因为在每次调用 processNewLogs() 时都会用到 join() 操作，而我们对数据集是如何分区的却一无所知。默认情况下，连接操作会将两个数据集中的所有键的哈希值都求出来，将该哈希值相同的记录通过网络传到同一台机器上，然后在那台机器上对所有键相同的记录进行连接操作。因为 userData 表比每五分钟出现的访问日志表 events 要大得多，所以要浪费时间做很多额外工作：在每次调用时都对 userData 表进行哈希值计算和跨节点数据混洗，虽然这些数据从来都不会变化。

![](https://upload-images.jianshu.io/upload_images/4311027-dc6c03d3100c36d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

要解决这一问题也很简单：在程序开始时，对 userData 表使用 partitionBy() 转化操作，将这张表转为哈希分区。可以通过向partitionBy 传递一个spark.HashPartitioner 对象来实现该操作。

```
例4-23：Scala 自定义分区方式
val sc = new SparkContext(...)
val userData = sc.sequenceFile[UserID, UserInfo]("hdfs://...")
.partitionBy(new HashPartitioner(100)) // 构造100个分区
.persist()
```

processNewLogs() 方法可以保持不变： 在 processNewLogs() 中，eventsRDD 是本地变量，只在该方法中使用了一次，所以为 events 指定分区方式没有什么用处。由于在构建 userData 时调用了 partitionBy()，Spark 就知道了该 RDD 是根据键的哈希值来分区的，这样在调用 join() 时，Spark 就会利用到这一点。具体来说，当调用 userData.join(events) 时，Spark 只会对 events 进行数据混洗操作，将 events 中特定 UserID 的记录发送到 userData 的对应分区所在的那台机器上。这样，需要通过网络传输的数据就大大减少了，程序运行速度也可以显著提升了。

![](https://upload-images.jianshu.io/upload_images/4311027-bff8a5221547c7ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注意，partitionBy() 是一个转化操作，因此它的返回值总是一个新的 RDD，但它不会改变原来的 RDD。**RDD 一旦创建就无法修改**。因此应该对 partitionBy() 的结果进行持久化，并保存为 userData，而不是原来的 sequenceFile() 的输出。此外，传给 partitionBy() 的 100 表示分区数目，它会控制之后对这个 RDD 进行进一步操作（比如连接操作）时有多少任务会并行执行。总的来说，**分区数目至少应该和集群中的总核心数一样**。

如果没有将 partitionBy() 转化操作的结果持久化，那么后面每次用到这个 RDD 时都会重复地对数据进行分区操作。不进行持久化会导致整个 RDD 谱系图重新求值。那样的话，partitionBy() 带来的好处就会被抵消，导致重复对数据进行分区以及跨节点的混洗，和没有指定分区方式时发生的情况十分相似。

事实上，许多其他 Spark 操作会自动为结果 RDD 设定已知的分区方式信息，而且除 join() 外还有很多操作也会利用到已有的分区信息。比如，sortByKey() 和 groupByKey() 会分别生成范围分区的 RDD 和哈希分区的RDD。而另一方面，诸如 map() 这样的操作会导致新的 RDD 失去父 RDD 的分区信息，因为这样的操作理论上可能会修改每条记录的键。

接下来的几节中，我们会讨论如何获取RDD 的分区信息，以及数据分区是如何影响各种Spark 操作的。

**Java 和 Python 中的数据分区**

Spark 的 Java 和 Python 的 API 都和 Scala 的一样，可以从数据分区中获益。不过，在Python 中，你不能将 HashPartitioner 对象传给 partitionBy，而只需要把需要的分区数传递过去，例如rdd.partitionBy(100)。

##### 4.5.1 获取RDD的分区方式
在 Scala 和 Java 中，你可以使用 RDD 的 partitioner 属性（Java 中使用 partitioner() 方法）来获取 RDD 的分区方式。它会返回一个 scala.Option 对象，这是 Scala 中用来存放可能存在的对象的容器类。你可以对这个 Option 对象调用 isDefined() 来检查其中是否有值，调用 get() 来获取其中的值。如果存在值的话，这个值会是一个 spark.Partitioner 对象。这本质上是一个告诉我们 RDD 中各个键分别属于哪个分区的函数。

在 Spark shell 中使用 partitioner 属性不仅是检验各种 Spark 操作如何影响分区方式的一种好办法，还可以用来在你的程序中检查想要使用的操作是否会生成正确的结果。

```
# 例4-24：获取 RDD 的分区方式
scala> val pairs = sc.parallelize(List((1, 1), (2, 2), (3, 3)))
pairs: spark.RDD[(Int, Int)] = ParallelCollectionRDD[0] at parallelize at
<console>:12
//创建一个由 (Int, Int) 对组成的 RDD，初始时没有分区方式信息（一个值为 None 的 Option 对象）
scala> pairs.partitioner
res0: Option[spark.Partitioner] = None
//通过对第一个 RDD 进行哈希分区，创建出第二个 RDD
scala> val partitioned = pairs.partitionBy(new spark.HashPartitioner(2))
partitioned: spark.RDD[(Int, Int)] = ShuffledRDD[1] at partitionBy at <console>:14
scala> partitioned.partitioner
res1: Option[spark.Partitioner] = Some(spark.HashPartitioner@5147788d)
//如果确实要在后续操作中使用 partitioned，那就应当在定义 partitioned 时，在第三行输入的最后加上 persist()
//如果不调用persist() 的话，后续的 RDD 操作会对 partitioned 的整个谱系重新求值，这会导致对 pairs 一遍又一遍地进行哈希分区操作
```

Python API 没有提供查询分区方式的方法，但是 Spark 内部仍然会利用已有的分区信息。

##### 4.5.2 从分区中获益的操作

Spark 的许多操作都引入了将数据根据键跨节点进行混洗的过程。所有这些操作都会从数据分区中获益。就 Spark 1.0 而言，能够从数据分区中获益的操作有：
cogroup()
groupWith()
join()
leftOuterJoin()
rightOuterJoin()
groupByKey()
reduceByKey()
combineByKey() 
lookup()

对于像 reduceByKey() 这样只作用于单个 RDD 的操作，运行在未分区的 RDD 上的时候会导致每个键的所有对应值都在每台机器上进行本地计算，只需要把本地最终归约出的结果值从各工作节点传回主节点，所以原本的网络开销就不算大。而对于诸如 cogroup() 和 join() 这样的二元操作，预先进行数据分区会导致其中至少一个 RDD（使用已知分区器的那个 RDD）不发生数据混洗。如果两个 RDD 使用同样的分区方式，并且它们还缓存在同样的机器上（比如一个 RDD 是通过 mapValues() 从另一个 RDD 中创建出来的，这两个 RDD 就会拥有相同的键和分区方式），或者其中一个 RDD 还没有被计算出来，那么跨节点的数据混洗就不会发生了。

##### 4.5.3 影响分区方式的操作

Spark 内部知道各操作会如何影响分区方式，并将会对数据进行分区的操作的结果 RDD 自动设置为对应的分区器。例如，如果你调用 join() 来连接两个 RDD：由于键相同的元素会被哈希到同一台机器上，Spark 知道输出结果也是哈希分区的，这样对连接的结果进行诸如 reduceByKey() 这样的操作时就会明显变快。

不过，转化操作的结果并不一定会按已知的分区方式分区，这时输出的RDD 可能就会没有设置分区器。例如，当你对一个哈希分区的键值对 RDD 调用 map() 时，由于传给 map() 的函数理论上可以改变元素的键，因此结果就不会有固定的分区方式。Spark 不会分析你的函数来判断键是否会被保留下来。不过，Spark 提供了另外两个操作 mapValues() 和 flatMapValues() 作为替代方法，它们可以保证每个二元组的键保持不变。

这里列出了所有会为生成的结果 RDD 设好分区方式的操作：
cogroup()
groupWith()
join()
leftOuterJoin()
rightOuterJoin()
groupByKey()
reduceByKey()
combineByKey()
partitionBy()
sort()
mapValues()
flatMapValues()：如果父 RDD 有分区方式的话
filter()：如果父 RDD 有分区方式的话

其他所有的操作生成的结果都不会存在特定的分区方式。

最后，对于二元操作，输出数据的分区方式取决于父 RDD 的分区方式。默认情况下，结果会采用哈希分区，分区的数量和操作的并行度一样。不过，如果其中的一个父 RDD 已经设置过分区方式，那么结果就会采用那种分区方式；如果两个父 RDD 都设置过分区方式，结果 RDD 会采用第一个父 RDD 的分区方式。

##### 4.5.4 示例：PageRank

PageRank 是一种从 RDD 分区中获益的更复杂的算法，我们以它为例进行分析。PageRank 算法是以 Google 的拉里· 佩吉（Larry Page）的名字命名的，用来根据外部文档指向一个文档的链接，对集合中每个文档的重要程度赋一个度量值。该算法可以用于对网页进行排序，当然，也可以用于排序科技文章或社交网络中有影响的用户。

PageRank 是执行多次连接的一个迭代算法，因此它是 RDD 分区操作的一个很好的用例。算法会维护两个数据集：一个由 (pageID, linkList) 的元素组成，包含每个页面的相邻页面的列表；另一个由 (pageID, rank) 元素组成，包含每个页面的当前排序值。

```
// 例4-25：Scala 版 PageRank
// 假设相邻页面列表以 Spark objectFile 的形式存储
val links = sc.objectFile[(String, Seq[String])]("links")
              .partitionBy(new HashPartitioner(100))
              .persist()
// 将每个页面的排序值初始化为 1.0；由于使用 mapValues，生成的 RDD 的分区方式会和"links"的一样
var ranks = links.mapValues(v => 1.0)

// 在每次迭代中，对页面 p，向其每个相邻页面（有直接链接的页面）发送一个值为 rank(p)/numNeighbors(p) 的贡献值，不断更新 ranks 变量
// 在此过程中，算法会逐渐收敛于每个页面的实际 PageRank 值
// 在实际操作中，收敛通常需要大约 10 轮迭代
for(i <- 0 until 10) {
  // 对当前的 ranksRDD 和静态的 linksRDD 进行一次 join() 操作，来获取每个页面 ID 对应的相邻页面列表和当前的排序值
  val contributions = links.join(ranks).flatMap {
    case (pageId, (links, rank)) =>
      links.map(dest => (dest, rank / links.size))
  }
  // 再把这些贡献值按照页面ID（根据获得共享的页面）分别累加起来，把该页面的排序值设为 0.15 + 0.85 * contributionsReceived
  // 使用 flatMap 创建出“contributions”来记录每个页面对各相邻页面的贡献
  ranks = contributions.reduceByKey((x, y) => x + y).mapValues(v => 0.15 + 0.85 * v)
}

// 写出最终排名
ranks.saveAsTextFile("ranks")
```

确保 RDD 以比较高效的方式进行分区，以最小化通信开销：

(1)请注意，linksRDD 在每次迭代中都会和 ranks 发生连接操作。由于 links 是一个静态数据集，所以我们在程序一开始的时候就对它进行了分区操作，这样就不需要把它通过网络进行数据混洗了。实际上，linksRDD 的字节数一般来说也会比ranks 大很多，毕竟它包含每个页面的相邻页面列表（由页面 ID 组成），而不仅仅是一个 Double 值，因此这一优化相比 PageRank 的原始实现（例如普通的 MapReduce）节约了相当可观的网络通信开销。
(2)出于同样的原因，我们调用 links 的 persist() 方法，将它保留在内存中以供每次迭代使用。
(3)当我们第一次创建 ranks 时，我们使用 mapValues() 而不是 map() 来保留父 RDD（links）的分区方式，这样对它进行的第一次连接操作就会开销很小。
(4)在循环体中，我们在 reduceByKey() 后使用 mapValues()，因为 reduceByKey() 的结果已经是哈希分区的了，这样一来，下一次循环中将映射操作的结果再次与links 进行连接操作时就会更加高效。

为了最大化分区相关优化的潜在作用，你应该在无需改变元素的键时尽量使用 mapValues() 或 flatMapValues()。

##### 4.5.5 自定义分区方式

虽然 Spark 提供的 HashPartitioner 与 RangePartitioner 已经能够满足大多数用例，但 Spark 还是允许你通过提供一个自定义的 Partitioner 对象来控制 RDD 的分区方式。这可以让你利用领域知识进一步减少通信开销。

举个例子，假设我们要在一个网页的集合上运行前一节中的 PageRank 算法。在这里，每个页面的 ID（RDD 中的键）是页面的 URL。当我们使用简单的哈希函数进行分区时，拥有相似的URL 的页面（比如 http://www.cnn.com/WORLD 和 http://www.cnn.com/US ）可能会被分到完全不同的节点上。然而，我们知道在同一个域名下的网页更有可能相互链接。

由于 PageRank 需要在每次迭代中从每个页面向它所有相邻的页面发送一条消息，因此把这些页面分组到同一个分区中会更好。可以使用自定义的分区器来实现仅根据域名而不是整个 URL 来分区。

要实现自定义的分区器，你需要继承 org.apache.spark.Partitioner 类并实现下面三个方法。
• numPartitions: Int：返回创建出来的分区数。
• getPartition(key: Any): Int：返回给定键的分区编号（0 到 numPartitions-1）。
• equals()：Java 判断相等性的标准方法。这个方法的实现非常重要，Spark 需要用这个方法来检查你的分区器对象是否和其他分区器实例相同，这样 Spark 才可以判断两个 RDD 的分区方式是否相同。

有一个问题需要注意，当你的算法依赖于 Java 的 hashCode() 方法时，这个方法有可能会返回负数。你需要十分谨慎，确保 getPartition() 永远返回一个非负数。

```
// 编写一个前面构思的基于域名的分区器，这个分区器只对 URL 中的域名部分求哈希
// 例4-26：Scala 自定义分区方式
class DomainNamePartitioner(numParts: Int) extends Partitioner {
  override def numPartitions: Int = numParts
  override def getPartition(key: Any): Int = {
    val domain = new Java.net.URL(key.toString).getHost()
    val code = (domain.hashCode % numPartitions)
    if(code < 0) {
      code + numPartitions // 使其非负
    }else{
      code
  }
}
  // 用来让 Spark 区分分区函数对象的 Java equals 方法
  // 在 equals() 方法中，使用 Scala 的模式匹配操作符（match）来检查 other 是否是 DomainNamePartitioner，并在成立时自动进行类型转换；这和 Java 中的 instanceof() 是一样的
  override def equals(other: Any): Boolean = other match {
    case dnp: DomainNamePartitioner =>
      dnp.numPartitions == numPartitions
    case _ =>
      false
  }
}

// 在 Java 中创建一个自定义 Partitioner 的方法与 Scala 中的做法非常相似：只需要扩展 spark.Partitioner 类并且实现必要的方法即可。

# 在 Python 中，不需要扩展 Partitioner 类，而是把一个特定的哈希函数作为一个额外的参数传给 RDD.partitionBy() 函数
# 注意，这里你所传过去的哈希函数会被与其他 RDD 的分区函数区分开来
# 如果你想要对多个 RDD 使用相同的分区方式，就应该使用同一个函数对象，比如一个全局函数，而不是为每个 RDD 创建一个新的函数对象
# 例4-27：Python 自定义分区方式
import urlparse
def hash_domain(url):
  return hash(urlparse.urlparse(url).netloc)
rdd.partitionBy(20, hash_domain)　# 创建20个分区
```

使用自定义的 Partitioner 是很容易的：只要把它传给 partitionBy() 方法即可。Spark 中有许多依赖于数据混洗的方法，比如 join() 和 groupByKey()，它们也可以接收一个可选的 Partitioner 对象来控制输出数据的分区方式。

##### 4.6 总结

本章我们学习了如何使用 Spark 提供的专门的函数来操作键值对数据。第 3 章中讲到的技巧也同样适用于 pair RDD。在下一章中我们会介绍如何读取和保存数据。


### 第5章 数据读取与保存

本章对于工程师和数据科学家都较为实用。工程师会了解到更多的输出格式，有利于找到非常适合用于下游处理程序的格式。数据科学家则可能更关心数据的现有的组织形式。

##### 5.1 动机

我们已经学了很多在 Spark 中对已分发的数据执行的操作。到目前为止，所展示的示例都是从本地集合或者普通文件中进行数据读取和保存的。但有时候，数据量可能大到无法放在一台机器中，这时就需要探索别的数据读取和保存的方法了。

Spark 支持很多种输入输出源。一部分原因是 Spark 本身是基于 Hadoop 生态圈而构建，特别是 Spark 可以通过 Hadoop MapReduce 所使用的 InputFormat 和OutputFormat 接口访问数据，而大部分常见的文件格式与存储系统（例如 S3、HDFS、Cassandra、HBase 等）都支持这种接口。

不过，基于这些原始接口构建出的高层 API 会更常用。幸运的是，Spark 及其生态系统提供了很多可选方案。本章会介绍以下三类常见的数据源。

1. 文件格式与文件系统
对于存储在本地文件系统或分布式文件系统（比如 NFS、HDFS、Amazon S3 等）中的数据，Spark 可以访问很多种不同的文件格式，包括文本文件、JSON、SequenceFile，以及 protocol buffer。

2. Spark SQL 中的结构化数据源
第 9 章会介绍 Spark SQL 模块，它针对包括 JSON 和 Apache Hive 在内的结构化数据源，为我们提供了一套更加简洁高效的 API。

3. 数据库与键值存储
本章还会概述 Spark 自带的库和一些第三方库，它们可以用来连接 Cassandra、HBase、Elasticsearch 以及 JDBC 源。

##### 5.2 文件格式

Spark 对很多种文件格式的读取和保存方式都很简单。从诸如文本文件的非结构化的文件，到诸如 JSON 格式的半结构化的文件，再到诸如 SequenceFile 这样的结构化的文件，Spark 都可以支持。Spark 会根据文件扩展名选择对应的处理方式。这一过程是封装好的，对用户透明。

除了 Spark 中直接支持的输出机制，还可以对键数据（或成对数据）使用 Hadoop 的新旧文件 API。由于 Hadoop 接口要求使用键值对数据，所以也只能这样用，即使有些格式事实上忽略了键。对于那些会忽视键的格式，通常使用假的键（比如 null）。

![](https://upload-images.jianshu.io/upload_images/4311027-975a91ff1b04f3cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 5.2.1 文本文件

在 Spark 中读写文本文件很容易。当我们将一个文本文件读取为 RDD 时，输入的每一行都会成为 RDD 的一个元素。也可以将多个完整的文本文件一次性读取为一个 pair RDD，其中键是文件名，值是文件内容。

**读取文本文件**

只需要使用文件路径作为参数调用 SparkContext 中的 textFile() 函数，就可以读取一个文本文件。如果要控制分区数的话，可以指定 minPartitions。

```
# 例5-1：在 Python 中读取一个文本文件
input = sc.textFile("file:///home/holden/repos/spark/README.md")

# 例5-2：在 Scala 中读取一个文本文件
val input = sc.textFile("file:///home/holden/repos/spark/README.md")

# 例5-3：在 Java 中读取一个文本文件
JavaRDD<String> input = sc.textFile("file:///home/holden/repos/spark/README.md")
```

Spark 支持读取给定目录中的所有文件，以及在输入路径中使用通配字符（如part-*.txt）。大规模数据集通常存放在多个文件中，因此这一特性很有用，尤其是在同一目录中存在一些别的文件（比如成功标记文件）的时候。

如果多个输入文件以一个包含数据所有部分的目录的形式出现，可以用两种方式来处理。

1. 如果文件足够小，那么可以使用 textFile 函数，传递目录作为参数，这样它会把各部分都读取到RDD中。有时候有必要知道数据的各部分分别来自哪个文件（比如将键放在文件名中的时间数据），有时候则希望同时处理整个文件。

2. wholeTextFiles() 方法，该方法会返回一个 pair RDD，其中键是输入文件的文件名，在每个文件表示一个特定时间段内的数据时非常有用。如果有表示不同阶段销售数据的文件，则可以很容易地求出每个阶段的平均值。

```
// 例5-4：在 Scala 中求每个文件的平均值
val input = sc.wholeTextFiles("file://home/holden/salesFiles")
val result = input.mapValues{y =>
  val nums = y.split(" ").map(x => x.toDouble)
  nums.sum / nums.size.toDouble
}
```

**保存文本文件**

saveAsTextFile() 方法接收一个路径，并将 RDD 中的内容都输入到路径对应的文件中。Spark 将传入的路径作为目录对待，会在那个目录下输出多个文件。这样，Spark 就可以从多个节点上并行输出了。在这个方法中，我们不能控制数据的哪一部分输出到哪个文件中，不过有些输出格式支持控制。

```
# 例5-5：在Python 中将数据保存为文本文件
result.saveAsTextFile(outputFile)
```

##### 5.2.2 JSON

JSON 是一种使用较广的半结构化数据格式。读取 JSON 数据的最简单的方式是将数据作为文本文件读取，然后使用 JSON 解析器来对 RDD 中的值进行映射操作。类似地，也可以使用我们喜欢的 JSON 序列化库来将数据转为字符串，然后将其写出去。

**读取 JSON**

将数据作为文本文件读取，然后对 JSON 数据进行解析，这样的方法可以在所有支持的编程语言中使用。这种方法假设文件中的每一行都是一条 JSON 记录。如果你有跨行的 JSON 数据，你就只能读入整个文件，然后对每个文件进行解析。如果在你使用的语言中构建一个 JSON 解析器的开销较大，你可以使用 mapPartitions() 来重用解析器。

处理格式不正确的记录有可能会引起很严重的问题，尤其对于像 JSON 这样的半结构化数据来说。对于小数据集来说，可以接受在遇到错误的输入时停止程序（程序失败），但是对于大规模数据集来说，格式错误是家常便饭。如果选择跳过格式不正确的数据，你应该尝试使用累加器来跟踪错误的个数。

```
# 例5-6：在Python 中读取非结构化的 JSON
import json
data = input.map(lambda x: json.loads(x))

// 在 Scala 和Java 中，通常将记录读入到一个代表结构信息的类中，可能还需要略过一些无效的记录
// 将记录读取为 Person 类
// 例5-7：在Scala 中读取 JSON
import com.fasterxml.jackson.module.scala.DefaultScalaModule
import com.fasterxml.jackson.module.scala.experimental.ScalaObjectMapper
import com.fasterxml.jackson.databind.ObjectMapper
import com.fasterxml.jackson.databind.DeserializationFeature
...
case class Person(name: String, lovesPandas: Boolean) // 必须是顶级类
...
// 将其解析为特定的case class。使用flatMap，通过在遇到问题时返回空列表（None）
// 来处理错误，而在没有问题时返回包含一个元素的列表（Some(_)）
val result = input.flatMap(record => {
  try {
    Some(mapper.readValue(record, classOf[Person]))
  } catch {
    case e: Exception => None
  }
})

// 例5-8：在 Java 中读取 JSON
class ParseJson implements FlatMapFunction<Iterator<String>, Person> {
  public Iterable<Person> call(Iterator<String> lines) throws Exception {
    ArrayList<Person> people = new ArrayList<Person>();
    ObjectMapper mapper = new ObjectMapper();
    while (lines.hasNext()) {
      String line = lines.next();
      try {
        people.add(mapper.readValue(line, Person.class));
      } catch (Exception e) {
      // 跳过失败的数据
      }
    }
    return people;
  }
}
JavaRDD<String> input = sc.textFile("file.json");
JavaRDD<Person> result = input.mapPartitions(new ParseJson());
```

**保存JSON**

写出 JSON 文件比读取它要简单得多，因为不需要考虑格式错误的数据，并且也知道要写出的数据的类型。可以使用之前将字符串 RDD 转为解析好的JSON 数据的库，将由结构化数据组成的 RDD 转为字符串 RDD，然后使用 Spark 的文本文件 API 写出去。这样一来，就可以通过已有的操作文本数据的机制和 JSON 库，使用 Spark 轻易地读取和保存 JSON 数据了。

```
# 假设要选出喜爱熊猫的人，就可以从第一步中获取输入数据，然后筛选出喜爱熊猫的人
# 例5-9：在 Python 保存为 JSON
(data.filter(lambda x: x["lovesPandas"]).map(lambda x: json.dumps(x))
  .saveAsTextFile(outputFile))

// 例5-10：在 Scala 中保存为 JSON
result.filter(p => P.lovesPandas).map(mapper.writeValueAsString(_))
  .saveAsTextFile(outputFile)

// 例5-11：在Java 中保存为JSON
class WriteJson implements FlatMapFunction<Iterator<Person>, String> {
  public Iterable<String> call(Iterator<Person> people) throws Exception {
    ArrayList<String> text = new ArrayList<String>();
    ObjectMapper mapper = new ObjectMapper();
    while (people.hasNext()) {
      Person person = people.next();
      text.add(mapper.writeValueAsString(person));
    }
    return text;
  }
}
JavaRDD<Person> result = input.mapPartitions(new ParseJson()).filter(
  new LikesPandas()
);
JavaRDD<String> formatted = result.mapPartitions(new WriteJson());
formatted.saveAsTextFile(outfile);
```

##### 5.2.3 逗号分隔值与制表符分隔值

逗号分隔值（CSV）文件每行都有固定数目的字段，字段间用逗号隔开（在制表符分隔值文件，即 TSV 文件中用制表符隔开）。记录通常是一行一条，不过也不总是这样，有时也可以跨行。CSV 文件和 TSV 文件有时支持的标准并不一致，主要是在处理换行符、转义字符、非 ASCII 字符、非整数值等方面。CSV 原生并不支持嵌套字段，所以需要手动组合和分解特定的字段。

与 JSON 中的字段不一样的是，这里的每条记录都没有相关联的字段名，只能得到对应的序号。常规做法是使用第一行中每列的值作为字段名。

**读取CSV**

读取 CSV/TSV 数据和读取 JSON 数据相似，都需要先把文件当作普通文本文件来读取数据，再对数据进行处理。由于格式标准的缺失，同一个库的不同版本有时也会用不同的方式处理输入数据。

```
# 如果恰好你的 CSV 的所有数据字段均没有包含换行符，你也可以使用 textFile() 读取并解析数据
# 例5-12：在 Python 中使用 textFile() 读取 CSV
import csv
import StringIO
...
def loadRecord(line):
input = StringIO.StringIO(line) # 解析一行 CSV记录
reader = csv.DictReader(input, fieldnames=["name", "favouriteAnimal"])
return reader.next()
input = sc.textFile(inputFile).map(loadRecord)

// 例5-13：在 Scala 中使用 textFile() 读取 CSV
import Java.io.StringReader
import au.com.bytecode.opencsv.CSVReader
...
val input = sc.textFile(inputFile)
val result = input.map{ line =>
  val reader = new CSVReader(new StringReader(line));
  reader.readNext();
}

// 例5-14：在 Java 中使用 textFile() 读取 CSV
import au.com.bytecode.opencsv.CSVReader;
import Java.io.StringReader;
...
public static class ParseLine implements Function<String, String[]> {
  public String[] call(String line) throws Exception {
    CSVReader reader = new CSVReader(new StringReader(line));
    return reader.readNext();
  }
}
JavaRDD<String> csvFile1 = sc.textFile(inputFile);
JavaPairRDD<String[]> csvData = csvFile1.map(new ParseLine());

# 如果在字段中嵌有换行符，就需要完整读入每个文件，然后解析各段
# 如果只有一小部分输入文件，需要使用wholeFile() 方法，可能还需要对输入数据进行重新分区使得 Spark 能够更高效地并行化执行后续操作。
# 例5-15：在 Python 中完整读取 CSV
def loadRecords(fileNameContents):
  input = StringIO.StringIO(fileNameContents[1]) # 读取给定文件中的所有记录
  reader = csv.DictReader(input, fieldnames=["name", "favoriteAnimal"])
  return reader
fullFileData = sc.wholeTextFiles(inputFile).flatMap(loadRecords)

// 例5-16：在 Scala 中完整读取 CSV
case class Person(name: String, favoriteAnimal: String)
val input = sc.wholeTextFiles(inputFile)
val result = input.flatMap{ case (_, txt) =>
  val reader = new CSVReader(new StringReader(txt));
  reader.readAll().map(x => Person(x(0), x(1)))
}

// 例5-17：在 Java 中完整读取 CSV
public static class ParseLine
  implements FlatMapFunction<Tuple2<String, String>, String[]> {
    public Iterable<String[]> call(Tuple2<String, String> file) throws Exception {
      CSVReader reader = new CSVReader(new StringReader(file._2()));
      return reader.readAll();
    }
}
JavaPairRDD<String, String> csvData = sc.wholeTextFiles(inputFile);
JavaRDD<String[]> keyedRDD = csvData.flatMap(new ParseLine());
```

**保存CSV**

和 JSON 数据一样，写出 CSV/TSV 数据相当简单，同样可以通过重用输出编码器来加速。由于在 CSV 中我们不会在每条记录中输出字段名，因此为了使输出保持一致，需要创建一种映射关系。一种简单做法是写一个函数，用于将各字段转为指定顺序的数组。在 Python 中，如果输出字典，CSV 输出器会根据创建输出器时给定的fieldnames 的顺序帮我们完成这一行为。我们所使用的 CSV 库要输出到文件或者输出器，所以可以使用 StringWriter 或 StringIO来将结果放到RDD 中.

```
# 只能在我们知道所要输出的所有字段时使用
# 如果一些字段名是在运行时由用户输入决定的，最简单的方法是遍历所有的数据，提取不同的键，然后分别输出
# 例5-18：在 Python 中写 CSV
def writeRecords(records):
  output = StringIO.StringIO() #写出一些CSV记录
  writer = csv.DictWriter(output, fieldnames=["name", "favoriteAnimal"])
  for record in records:
    writer.writerow(record)
  return [output.getvalue()]
pandaLovers.mapPartitions(writeRecords).saveAsTextFile(outputFile)

// 例5-19：在 Scala 中写 CSV
pandaLovers.map(person => List(person.name, person.favoriteAnimal).toArray)
.mapPartitions{people =>
  val stringWriter = new StringWriter();
  val csvWriter = new CSVWriter(stringWriter);
  csvWriter.writeAll(people.toList)
  Iterator(stringWriter.toString)
}.saveAsTextFile(outFile)
```

##### 5.2.4 SequenceFile

##### 5.2.5 对象文件

##### 5.2.6 Hadoop输入输出格式

##### 5.2.7 文件压缩

在大数据工作中，我们经常需要对数据进行压缩以节省存储空间和网络传输开销。对于大多数 Hadoop 输出格式来说，我们可以指定一种压缩编解码器来压缩数据。我们已经提过，Spark 原生的输入方式（textFile 和sequenceFile）可以自动处理一些类型的压缩。在读取压缩后的数据时，一些压缩编解码器可以推测压缩类型。

这些压缩选项只适用于支持压缩的 Hadoop 格式，也就是那些写出到文件系统的格式。写入数据库的Hadoop 格式一般没有实现压缩支持。如果数据库中有压缩过的记录，那应该是数据库自己配置的。

选择一个输出压缩编解码器可能会对这些数据以后的用户产生巨大影响。对于像 Spark 这样的分布式系统，我们通常会尝试从多个不同机器上一起读入数据。要实现这种情况，每个工作节点都必须能够找到一条新记录的开端。有些压缩格式会使这变得不可能，而必须要单个节点来读入所有数据，这就很容易产生性能瓶颈。可以很容易地从多个节点上并行读取的格式被称为“可分割”的格式。

![](https://upload-images.jianshu.io/upload_images/4311027-16ae78944ab03dde.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

尽管 Spark 的 textFile() 方法可以处理压缩过的输入，但即使输入数据被以可分割读取的方式压缩，Spark 也不会打开splittable。因此，如果你要读取单个压缩过的输入，最好不要考虑使用 Spark 的封装，而是使用 newAPIHadoopFile 或者 hadoopFile，并指定正确的压缩编解码器。

有些输入格式（例如SequenceFile）允许我们只压缩键值对数据中的值，这在查询时很有用。其他一些输入格式也有自己的压缩控制：比如，Twitter 的 Elephant Bird 包中的许多格式都可以使用 LZO 算法压缩的数据。

##### 5.3　文件系统

Spark 支持读写很多种文件系统，可以使用任何我们想要的文件格式。

##### 5.3.1　本地/“常规”文件系统

Spark 支持从本地文件系统中读取文件，不过它要求文件在集群中所有节点的相同路径下都可以找到。一些像 NFS、AFS 以及 MapR 的 NFS layer 这样的网络文件系统会把文件以常规文件系统的形式暴露给用户。如果你的数据已经在这些系统中，那么你只需要指定输入为一个 file:// 路径；只要这个文件系统挂载在每个节点的同一个路径下，Spark 就会自动处理。

```
// 例5-29：在 Scala 中从本地文件系统读取一个压缩的文本文件
val rdd = sc.textFile("file:///home/holden/happypandas.gz")
```

如果文件还没有放在集群中的所有节点上，你可以在驱动器程序中从本地读取该文件而无需使用整个集群，然后再调用parallelize 将内容分发给工作节点。不过这种方式可能会比较慢，所以推荐的方法是将文件先放到像 HDFS、NFS、S3 等共享文件系统上。

##### 5.3.2 Amazon S3

##### 5.3.3 HDFS

Hadoop 分布式文件系统（HDFS）是一种广泛使用的文件系统，Spark 能够很好地使用它。HDFS 被设计为可以在廉价的硬件上工作，有弹性地应对节点失败，同时提供高吞吐量。

Spark 和 HDFS 可以部署在同一批机器上，这样 Spark 可以利用数据分布来尽量避免一些网络开销。

在Spark 中使用 HDFS 只需要将输入输出路径指定为 hdfs://master:port/path 就够了。

##### 5.4 Spark SQL 中的结构化数据

在各种情况下，我们把一条 SQL 查询给 Spark SQL，让它对一个数据源执行查询（选出一些字段或者对字段使用一些函数），然后得到由 Row 对象组成的 RDD，每个 Row 对象表示一条记录。

在 Java 和 Scala 中，Row 对象的访问是基于下标的。每个 Row 都有一个 get() 方法，会返回一个一般类型让我们可以进行类型转换。在Python 中，可以使用 row[column_number] 以及 row.column_name 来访问元素。

##### 5.4.1 Apache Hive

Apache Hive 是 Hadoop 上的一种常见的结构化数据源。Hive 可以在 HDFS 内或者在其他存储系统上存储多种格式的表。这些格式从普通文本到列式存储格式，应有尽有。Spark SQL 可以读取 Hive 支持的任何表。

```
# 以由行组成的 RDD 的形式拿到返回数据
# 例5-30：用 Python 创建 HiveContext 并查询数据
from pyspark.sql import HiveContext
hiveCtx = HiveContext(sc)
rows = hiveCtx.sql("SELECT name, age FROM users")
firstRow = rows.first()
print firstRow.name

// 例5-31：用 Scala 创建 HiveContext 并查询数据
import org.apache.spark.sql.hive.HiveContext
val hiveCtx = new org.apache.spark.sql.hive.HiveContext(sc)
val rows = hiveCtx.sql("SELECT name, age FROM users")
val firstRow = rows.first()
println(firstRow.getString(0)) // 字段0是name字段

// 例5-32：用 Java 创建 HiveContext 并查询数据
import org.apache.spark.sql.hive.HiveContext;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.SchemaRDD;
HiveContext hiveCtx = new HiveContext(sc);
SchemaRDD rows = hiveCtx.sql("SELECT name, age FROM users");
Row firstRow = rows.first();
System.out.println(firstRow.getString(0)); // 字段0是name字段
```

##### 5.4.2 JSON

如果你有记录间结构一致的 JSON 数据，Spark SQL 也可以自动推断出它们的结构信息，并将这些数据读取为记录，这样就可以使得提取字段的操作变得很简单。

从整个文件中获取由 Row 对象组成的 RDD。除了使用整个 Row 对象，你也可以将 RDD注册为一张表，然后从中选出特定的字段。

```
# 例5-33：JSON 中的示例推文，每行一条记录
{"user": {"name": "Holden", "location": "San Francisco"}, "text": "Nice day out today"}
{"user": {"name": "Matei", "location": "Berkeley"}, "text": "Even nicer here :)"}

# 可以读取这些数据，只从中选取 username（用户名）和 text（文本）字段
# 例5-34：在 Python 中使用 Spark SQL 读取 JSON 数据
tweets = hiveCtx.jsonFile("tweets.json")
tweets.registerTempTable("tweets")
results = hiveCtx.sql("SELECT user.name, text FROM tweets")

// 例5-35：在 Scala 中使用 Spark SQL 读取 JSON 数据
val tweets = hiveCtx.jsonFile("tweets.json")
tweets.registerTempTable("tweets")
val results = hiveCtx.sql("SELECT user.name, text FROM tweets")

// 例5-36：在 Java 中使用 Spark SQL 读取 JSON 数据
SchemaRDD tweets = hiveCtx.jsonFile(jsonFile);
tweets.registerTempTable("tweets");
SchemaRDD results = hiveCtx.sql("SELECT user.name, text FROM tweets");
```

##### 5.5 数据库

通过数据库提供的 Hadoop 连接器或者自定义的 Spark 连接器，Spark 可以访问一些常用的数据库系统。本节来展示四种常见的连接器。

##### 5.5.1 Java数据库连接

##### 5.5.2 Cassandra

##### 5.5.3 HBase

##### 5.5.4 Elasticsearch

##### 5.6 总结

在本章结束之际，你应该已经能够将数据读取到 Spark 中，并掌握了读取和保存大规模数据集的方法，将计算结果以你所希望的方式存储起来。我们调查了数据可以使用的一些不同格式，一些压缩选项以及它们对应的数据处理的方式。

### 第6章 Spark编程进阶

##### 6.1 简介

本章介绍 Spark 编程的各种进阶特性，会介绍两种类型的共享变量：累加器（accumulator）与广播变量（broadcast variable）。累加器用来对信息进行聚合，而广播变量用来高效分发较大的对象。在已有的 RDD 转化操作的基础上，我们为类似查询数据库这样需要很大配置代价的任务引入了批操作。为了扩展可用的工具范围，本章会介绍 Spark 与外部程序交互的方式，比如如何与用 R 语言编写的脚本进行交互。

```
# 例6-1：一条 JSON 格式的呼叫日志示例，其中某些字段已省略
{"address":"address here", "band":"40m","callsign":"KK6JLK","city":"SUNNYVALE",
"contactlat":"37.384733","contactlong":"-122.032164",
"county":"Santa Clara","dxcc":"291","fullname":"MATTHEW McPherrin",
"id":57779,"mode":"FM","mylat":"37.751952821","mylong":"-122.4208688735",...}
```

##### 6.2 累加器

通常在向 Spark 传递函数时，比如使用 map() 函数或者用 filter() 传条件时，可以使用驱动器程序中定义的变量，但是集群中运行的每个任务都会得到这些变量的一份新的副本，更新这些副本的值也不会影响驱动器中的对应变量。Spark 的两个共享变量，累加器与广播变量，分别为**结果聚合**与**广播**这两种常见的通信模式突破了这一限制。

第一种共享变量，即累加器，提供了将工作节点中的值聚合到驱动器程序中的简单语法。累加器的一个常见用途是在调试时对作业执行过程中的事件进行计数。例如，假设我们在从文件中读取呼号列表对应的日志，同时也想知道输入文件中有多少空行（也许不希望在有效输入中看到很多这样的行）。

```
# 使用累加器，而没有分别使用 filter() 和 reduce()
# 例6-2：在 Python 中累加空行
file = sc.textFile(inputFile)
blankLines = sc.accumulator(0) # 创建 Accumulator[Int] 并初始化为 0
def extractCallSigns(line):
  global blankLines # 访问全局变量
  if (line == ""):
    blankLines += 1
  return line.split(" ")
callSigns = file.flatMap(extractCallSigns)
callSigns.saveAsTextFile(outputDir + "/callsigns") # flagmap 是惰性的，执行完saveAsTextFile才能看到正确的计数
print "Blank lines: %d" % blankLines.value

// 例6-3：在 Scala 中累加空行
val sc = new SparkContext(...)
val file = sc.textFile("file.txt")
val blankLines = sc.accumulator(0) // 创建Accumulator[Int]并初始化为0
val callSigns = file.flatMap(line => {
  if (line == "") {
    blankLines += 1 // 累加器加1
  }
  line.split(" ")
})
callSigns.saveAsTextFile("output.txt")
println("Blank lines: " + blankLines.value)

// 例6-4：在 Java 中累加空行
JavaRDD<String> rdd = sc.textFile(args[1]);
final Accumulator<Integer> blankLines = sc.accumulator(0);
JavaRDD<String> callSigns = rdd.flatMap(
  new FlatMapFunction<String, String>() {
    public Iterable<String> call(String line) {
    if (line.equals("")) {
      blankLines.add(1);
    }
    return Arrays.asList(line.split(" "));
  }
});
callSigns.saveAsTextFile("output.txt")
System.out.println("Blank lines: "+ blankLines.value());
```

总结起来，累加器的用法如下所示。

• 通过在驱动器中调用 SparkContext.accumulator(initialValue) 方法，创建出存有初始值的累加器。返回值为 org.apache.spark.Accumulator[T] 对象，其中 T 是初始值 initialValue 的类型。
• Spark 闭包里的执行器代码可以使用累加器的+= 方法（在 Java 中是 add）增加累加器的值。
• 驱动器程序可以调用累加器的 value 属性（在 Java 中使用 value() 或 setValue()）来访
问累加器的值。

```
例6-5：在Python 使用累加器进行错误计数
# 分别创建累加器来记录有效和无效的呼号
validSignCount = sc.accumulator(0)
invalidSignCount = sc.accumulator(0)

def validateSign(sign):
  global validSignCount, invalidSignCount
  if re.match(r"\A\d?[a-zA-Z]{1,2}\d{1,4}[a-zA-Z]{1,3}\Z", sign):
    validSignCount += 1
    return True
  else:
    invalidSignCount += 1
    return False

# 对与每个呼号的联系次数进行计数
validSigns = callSigns.filter(validateSign)
contactCount = validSigns.map(lambda sign: (sign, 1)).reduceByKey(lambda (x, y): x
+ y)

# 强制求值计算计数
contactCount.count()
if invalidSignCount.value < 0.1 * validSignCount.value:
contactCount.saveAsTextFile(outputDir + "/contactCount")
else:
print "Too many errors: %d in %d" % (invalidSignCount.value, validSignCount.
value)
```

对于要在行动操作中使用的累加器，Spark只会把每个任务对各累加器的修改应用一次。因此，如果想要一个无论在失败还是重复计算时都绝对可靠的累加器，我们必须把它放在 foreach() 这样的行动操作中。

对于在RDD 转化操作中使用的累加器，就不能保证有这种情况了。转化操作中累加器可能会发生不止一次更新。因此转化操作中的累加器最好只在调试时使用。

##### 6.3 广播变量

Spark 的第二种共享变量类型是广播变量，它可以让程序高效地向所有工作节点发送一个较大的只读值，以供一个或多个 Spark 操作使用。比如，如果你的应用需要向所有节点发送一个较大的只读查询表，甚至是机器学习算法中的一个很大的特征向量，广播变量用起来都很顺手。

前面提过，Spark 会自动把闭包中所有引用到的变量发送到工作节点上。虽然这很方便，但也很低效。原因有二：首先，默认的任务发射机制是专门为小任务进行优化的；其次，事实上你可能会在多个并行操作中使用同一个变量，但是 Spark 会为每个操作分别发送。

例6-6这个程序可以运行，但是如果表更大（比如表中不是呼号而是IP 地址），signPrefixes 很容易就会达到数 MB 大小，从主节点为每个任务发送一个这样的数组就会代价巨大。而且，如果之后还要再次使用 signPrefixes 这个对象（可能还要在 file2.txt 上运行同样的代码），则还需要向每个节点再发送一遍。

可以把 signPrefixes 变为广播变量来解决这一问题，。广播变量其实就是类型为spark.broadcast.Broadcast[T] 的一个对象，其中存放着类型为T 的值。可以在任务中通过对 Broadcast 对象调用 value 来获取该对象的值。这个值只会被发送到各节点一次，使用的是一种高效的类似 BitTorrent 的通信机制。

```
# 查询 RDD contactCounts 中的呼号的对应位置。将呼号前缀读取为国家代码来进行查询
# 例6-6：在 Python 中查询国家
signPrefixes = loadCallSignTable()
def processSignCount(sign_count, signPrefixes):
  country = lookupCountry(sign_count[0], signPrefixes)
  count = sign_count[1]
  return (country, count)
countryContactCounts = (contactCounts
  .map(processSignCount)
  .reduceByKey((lambda x, y: x + y)))

# 例6-7：在 Python 中使用广播变量查询国家
signPrefixes = sc.broadcast(loadCallSignTable())
def processSignCount(sign_count, signPrefixes):
  country = lookupCountry(sign_count[0], signPrefixes.value)
  count = sign_count[1]
  return (country, count)
countryContactCounts = (contactCounts
  .map(processSignCount)
  .reduceByKey((lambda x, y: x + y)))
countryContactCounts.saveAsTextFile(outputDir + "/countries.txt")
```

使用广播变量的过程很简单：
(1)通过对一个类型 T 的对象调用 SparkContext.broadcast 创建出一个 Broadcast[T] 对象。
任何可序列化的类型都可以这么实现。
(2)通过 value 属性访问该对象的值（在 Java 中为 value() 方法）。
(3)变量只会被发到各个节点一次，应作为只读值处理（修改这个值不会影响到别的节点）。

##### 6.4 基于分区进行操作

基于分区对数据进行操作可以让我们避免为每个数据元素进行重复的配置工作。诸如打开数据库连接或创建随机数生成器等操作，都是我们应当尽量避免为每个元素都配置一次的工作。Spark 提供基于分区的 map 和 foreach，让你的部分代码只对 RDD 的每个分区运行一次，这样可以帮助降低这些操作的代价。通过使用基于分区的操作，可以在每个分区内共享一个数据库连接池，来避免建立太多连接，同时还可以重用 JSON 解析器。

```
# 使用 mapPartitions 函数获得输入 RDD 的每个分区中的元素迭代器，而需要返回的是执行结果的序列的迭代器
# 例6-10：在 Python 中使用共享连接池
# 查询呼号
def processCallSigns(signs):
  http = urllib3.PoolManager() # 创建一个连接池
  urls = map(lambda x: "http://73s.com/qsos/%s.json" % x, signs) # 与每条呼号记录相关联的URL
  requests = map(lambda x: (x, http.request('GET', x)), urls) # 创建请求（非阻塞）
  result = map(lambda x: (x[0], json.loads(x[1].data)), requests) # 获取结果
  return filter(lambda x: x[1] is not None, result) # 删除空的结果并返回

# 获取呼号
def fetchCallSigns(input):
  return input.mapPartitions(lambda callSigns : processCallSigns(callSigns))
  contactsContactList = fetchCallSigns(validSigns)
```

![](https://upload-images.jianshu.io/upload_images/4311027-c32248317310de12.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

除了避免重复的配置工作，也可以使用 mapPartitions() 避免创建对象的开销。有时需要创建一个对象来将不同类型的数据聚合起来。回忆一下第3 章中，当计算平均值时，一种方法是将数值RDD 转为二元组RDD，以在归约过程中追踪所处理的元素个数。现在，可以为每个分区只创建一次二元组，而不用为每个元素都执行这个操作。

```
# 例6-13：在Python 中不使用 mapPartitions() 求平均值
def combineCtrs(c1, c2):
  return (c1[0] + c2[0], c1[1] + c2[1])
def basicAvg(nums): # 计算平均值
  nums.map(lambda num: (num, 1)).reduce(combineCtrs) 

# 例6-14：在Python 中使用mapPartitions() 求平均值
def partitionCtr(nums): # 计算分区的sumCounter
  sumCount = [0, 0]
  for num in nums:
  sumCount[0] += num
  sumCount[1] += 1
  return [sumCount]
def fastAvg(nums): # 计算平均值
  sumCount = nums.mapPartitions(partitionCtr).reduce(combineCtrs)
  return sumCount[0] / float(sumCount[1])
```

##### 6.5 与外部程序间的管道

有三种可用的语言供你选择，这可能已经满足了你用来编写 Spark 应用的几乎所有需求。但是，如果 Scala、Java 以及 Python 都不能实现你需要的功能，那么 Spark 也为这种情况提供了一种通用机制，可以将数据通过管道传给用其他语言编写的程序，比如 R 语言脚本。

##### 6.6 数值 RDD 的操作

Spark 对包含数值数据的 RDD 提供了一些描述性的统计操作。这是我们会在第 11 章介绍的更复杂的统计方法和机器学习方法的一个补充。

Spark 的数值操作是通过流式算法实现的，允许以每次一个元素的方式构建出模型。这些统计数据都会在调用 stats() 时通过一次遍历数据计算出来，并以 StatsCounter 对象返回。

![](https://upload-images.jianshu.io/upload_images/4311027-007f87ddcfc118a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
# 例6-19：用 Python 移除异常值
# 要把 String 类型 RDD 转为数字数据，这样才能使用统计函数并移除异常值
distanceNumerics = distances.map(lambda string: float(string))
stats = distanceNumerics.stats()
stddev = std.stdev()
mean = stats.mean()
reasonableDistances = distanceNumerics.filter(
  lambda x: math.fabs(x - mean) < 3 * stddev
)
print reasonableDistances.collect()
```


### 第7章 在集群上运行 Spark

### 第8章 Spark 调优与调试

### 第9章 Spark SQL

本章介绍 Spark 用来操作结构化和半结构化数据的接口——Spark SQL。结构化数据是指任何有结构信息的数据。所谓结构信息，就是每条记录共用的已知的字段集合。当数据符合这样的条件时，Spark SQL 就会使得针对这些数据的读取和查询变得更加简单高效。具体来说，Spark SQL 提供了以下三大功能：

(1)Spark SQL 可以从各种结构化数据源（例如 JSON、Hive、Parquet 等）中读取数据。
(2)Spark SQL 不仅支持在 Spark 程序内使用 SQL 语句进行数据查询，也支持从类似商业智能软件 Tableau 这样的外部工具中通过标准数据库连接器（JDBC/ODBC）连接 Spark SQL 进行查询。
(3) 当在 Spark 程序内使用 Spark SQL 时，Spark SQL 支持 SQL 与常规的 Python/Java/ Scala代码高度整合，包括连接 RDD 与 SQL 表、公开的自定义 SQL 函数接口等。这样一来，许多工作都更容易实现了。

为了实现这些功能，Spark SQL 提供了一种特殊的 RDD，叫作 SchemaRDD。SchemaRDD是存放 Row 对象的 RDD，每个 Row 对象代表一行记录。SchemaRDD 还包含记录的结构信息（即数据字段）。SchemaRDD 看起来和普通的 RDD 很像，但是在内部，SchemaRDD 可以利用结构信息更加高效地存储数据。此外，SchemaRDD 还支持 RDD 上所没有的一些新操作，比如运行 SQL 查询。SchemaRDD 可以从外部数据源创建，也可以从查询结果或普通 RDD 中创建。

![](https://upload-images.jianshu.io/upload_images/4311027-1d707a1420a89bc0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 9.1 连接Spark SQL

##### 9.2 在应用中使用Spark SQL

Spark SQL 最强大之处就是可以在 Spark 应用内使用。这种方式让我们可以轻松读取数据并使用 SQL 查询，同时还能把这一过程和普通的 Python/Java/Scala 程序代码结合在一起。

要以这种方式使用 Spark SQL，需要基于已有的 SparkContext 创建出一个 HiveContext（如果使用的是去除了 Hive 支持的 Spark 版本，则创建出 SQLContext）。这个上下文环境提供了对 Spark SQL 的数据进行查询和交互的额外函数。使用 HiveContext 可以创建出表示结构化数据的 SchemaRDD，并且使用 SQL 或是类似 map() 的普通 RDD 操作来操作这些 SchemaRDD。

##### 9.2.1 初始化Spark SQL

```
# 例9-5：Python 中 SQL 的 import 声明
# 导入Spark SQL
from pyspark.sql import HiveContext, Row
# 当不能引入hive依赖时
from pyspark.sql import SQLContext, Row

# 例9-8：在 Python 中创建 SQL 上下文环境
hiveCtx = HiveContext(sc)
```

##### 9.2.2 基本查询示例

```
# 例9-11：在 Python 中读取并查询推文
input = hiveCtx.jsonFile(inputFile)
# 注册输入的 SchemaRDD 为临时表
# 临时表是当前使用的 HiveContext 或 SQLContext 中的临时变量，在你的应用退出时这些临时表就不再存在了
input.registerTempTable("tweets")
# 调用 sql() 方法，依据 retweetCount（转发计数）选出推文
topTweets = hiveCtx.sql("""SELECT text, retweetCount FROM
tweets ORDER BY retweetCount LIMIT 10""")
```

##### 9.2.3 SchemaRDD

读取数据和执行查询都会返回 SchemaRDD。SchemaRDD 和传统数据库中的表的概念类似。从内部机理来看，SchemaRDD 是一个由 Row 对象组成的 RDD，附带包含每列数据类型的结构信息。Row 对象只是对基本数据类型（如整型和字符串型等）的数组的封装。

SchemaRDD 仍然是 RDD，所以你可以对其应用已有的 RDD 转化操作，比如 map() 和 filter()。然而，SchemaRDD 也提供了一些额外的功能支持。最重要的是，你可以把任意 SchemaRDD 注册为临时表，这样就可以对它进行查询了，如例 9-11。

**使用Row对象**

Row 对象表示 SchemaRDD 中的记录，其本质就是一个定长的字段数组。

在 Python 中，由于没有显式的类型系统，Row 对象变得稍有不同。我们使用 row[i] 来访问第 i 个元素。除此之外，Python 中的 Row 还支持以 row.column_name 的形式使用名字来访问其中的字段。

```
# 例9-14：在 Python 中访问 topTweet 这个 SchemaRDD 中的 text 列
topTweetText = topTweets.map(lambda row: row.text)
```

##### 9.2.4 缓存

Spark SQL 的缓存机制与 Spark 中的稍有不同。由于我们知道每个列的类型信息，所以 Spark 可以更加高效地存储数据。为了确保使用更节约内存的表示方式进行缓存而不是储存整个对象，应当使用专门的 hiveCtx.cacheTable("tableName") 方法。当缓存数据表时，Spark SQL 使用一种列式存储格式在内存中表示数据。这些缓存下来的表只会在驱动器程序的生命周期里保留在内存中，所以如果驱动器进程退出，就需要重新缓存数据。和缓存 RDD 时的动机一样，如果想在同样的数据上多次运行任务或查询时，就应把这些数据表缓存起来。

也可以使用 HiveQL/SQL 语句来缓存表。只需要运行 CACHE TABLEtableName 或 UNCACHE TABLEtableName 来缓存表或者删除已有的缓存即可。这种使用方式在 JDBC 服务器的命令行客户端中很常用。

##### 9.3 读取和存储数据

Spark SQL 支持很多种结构化数据源，可以让你跳过复杂的读取过程，轻松从各种数据源中读取到 Row 对象。这些数据源包括 Hive 表、JSON 和 Parquet 文件。此外，当你使用 SQL 查询这些数据源中的数据并且只用到了一部分字段时，Spark SQL 可以智能地只扫描这些用到的字段，而不是像SparkContext.hadoopFile 中那样简单粗暴地扫描全部数据。除这些数据源之外， 你也可以在程序中通过指定结构信息， 将常规的 RDD 转化为 SchemaRDD。这使得在 Python 或者 Java 对象上运行SQL 查询更加简单。当需要计算许多数值时，SQL 查询往往更加简洁（比如要同时求出平均年龄、最大年龄、不重复的用户 ID 数目等）。不仅如此，你还可以自如地将这些 RDD 和来自其他 Spark SQL 数据源的 SchemaRDD 进行连接操作。在本节中，我们会讲解外部数据源以及这种使用 RDD 的方式。

##### 9.3.1 Apache Hive

```
# 例9-15：使用 Python 从 Hive 读取
from pyspark.sql import HiveContext
hiveCtx = HiveContext(sc)
rows = hiveCtx.sql("SELECT key, value FROM mytable")
keys = rows.map(lambda row: row[0])
```

##### 9.3.2 Parquet

##### 9.3.3 JSON

如果你有一个 JSON 文件，其中的记录遵循同样的结构信息，那么 Spark SQL 就可以通过扫描文件推测出结构信息，并且让你可以使用名字访问对应字段（如例 9-21 所示）。如果你在一个包含大量 JSON 文件的目录中进行尝试，你就会发现 Spark SQL 的结构信息推断可以让你非常高效地操作数据，而无需编写专门的代码来读取不同结构的文件。

```
# 例9-21：输入记录
{"name": "Holden"}
{"name": "Sparky The Bear", "lovesPandas":true,"knows": {"friends":["holden"]}}
# 例9-22：在 Python 中使用 Spark SQL 读取 JSON 数据
# 如果想获得从数据中推断出来的结构信息，可以在生成的 SchemaRDD 上调用 printSchema 方法
input = hiveCtx.jsonFile(inputFile)

# 例 9-27：用 SQL 查询嵌套数据以及数组元素
# 如果使用 Python，或已经把数据注册为了一张 SQL 表，可以通过.来访问各个嵌套层级的嵌套元素（比如toplevel.nextlevel）
select hashtagEntities[0].text from tweets LIMIT 1;
```

##### 9.3.4 基于 RDD

除了读取数据，也可以基于 RDD 创建 SchemaRDD。在 Scala 中，带有 case class 的 RDD可以隐式转换成 SchemaRDD。

##### 9.4 JDBC/ODBC服务器

Spark SQL 也提供 JDBC 连接支持，这对于让商业智能（BI）工具连接到 Spark 集群上以及在多用户间共享一个集群的场景都非常有用。JDBC 服务器作为一个独立的 Spark 驱动器程序运行，可以在多用户之间共享。任意一个客户端都可以在内存中缓存数据表，对表进行查询。集群的资源以及缓存数据都在所有用户之间共享。

##### 9.4.1 使用 Beeline

在 Beeline 客户端中，你可以使用标准的 HiveQL 命令来创建、列举以及查询数据表。

##### 9.4.2 长生命周期的表与查询

使用 Spark SQL 的 JDBC 服务器的优点之一就是我们可以在多个不同程序之间共享缓存下来的数据表。JDBC Thrift 服务器是一个单驱动器程序，这就使得共享成为了可能。如前一节中所述，你只需要注册该数据表并对其运行 CACHE 命令，就可以利用缓存了。

##### 9.5 用户自定义函数

用户自定义函数，也叫 UDF，可以让我们使用 Python/Java/Scala 注册自定义函数，并在 SQL中调用。这种方法很常用，通常用来给机构内的 SQL 用户们提供高级功能支持，这样这些用户就可以直接调用注册的函数而无需自己去通过编程来实现了。在 Spark SQL 中，编写 UDF 尤为简单。Spark SQL 不仅有自己的 UDF 接口，也支持已有的 Apache Hive UDF。

##### 9.5.1 Spark SQL UDF

我们可以使用 Spark 支持的编程语言编写好函数，然后通过 Spark SQL 内建的方法传递进来，非常便捷地注册我们自己的 UDF。在 Scala 和 Python 中，可以利用语言原生的函数和 lambda 语法的支持，而在 Java 中，则需要扩展对应的 UDF 类。UDF 能够支持各种数据类型，返回类型也可以与调用时的参数类型完全不一样。

```
# 例9-36：Python 版本耳朵字符串长度 UDF
# 写一个求字符串长度的 UDF
hiveCtx.registerFunction("strLenPython", lambda x: len(x), IntegerType())
lengthSchemaRDD = hiveCtx.sql("SELECT strLenPython('text') FROM tweets LIMIT 10")
```

##### 9.5.2 Hive UDF

Spark SQL 也支持已有的 Hive UDF。标准的 Hive UDF 已经自动包含在了 Spark SQL 中。如
果需要支持自定义的 Hive UDF，我们要确保该 UDF 所在的 JAR 包已经包含在了应用中。

要使用 Hive UDF，应该使用 HiveContext，而不能使用常规的 SQLContext。要注册一个 Hive UDF，只需调用 hiveCtx.sql("CREATE TEMPORARY FUNCTION name AS class.function")。

##### 9.6 Spark SQL 性能

Spark SQL 提供的高级查询语言及附加的类型信息可以使 Spark SQL 数据查询更加高效。
Spark SQL 不仅是给熟悉 SQL 的用户使用的。Spark SQL 使有条件的聚合操作变得非常容易，比如对多个列进行求值。利用 Spark SQL 则不再需要像第 6 章中讨论的那样创建一些特殊的对象来进行这种操作。

```
# 例9-40：Spark SQL 多列求和
SELECT SUM(user.favouritesCount), SUM(retweetCount), user.id FROM tweets
GROUP BY user.id
```

Spark SQL 可以利用其对类型的了解来高效地表示数据。当缓存数据时，Spark SQL 使用内存式的列式存储。这不仅仅节约了缓存的空间，而且尽可能地减少了后续查询中针对某几个字段查询时的数据读取。

**谓词下推**可以让 Spark SQL 将查询中的一些部分工作“下移”到查询引擎上。如果我们只需在 Spark 中读取某些特定的记录，标准的方法是读入整个数据集，然后在上面执行筛选条件。然而，在 Spark SQL 中，如果底层的数据存储支持只读取键值在一个范围内的记录，或是其他某些限制条件，Spark SQL 就可以把查询语句中的筛选限制条件推到数据存储层，从而大大减少需要读取的数据。

##### 9.7 总结

现在，我们学完了 Spark 利用 Spark SQL 进行结构化和半结构化数据处理的方式。除了本章探索过的查询语句，第 3 章到第 6 章中讲到的操作 RDD 的方法同样适用于 Spark SQL 中的 SchemaRDD。很多时候，我们会把 SQL 与其他的编程语言结合起来使用，以充分利用 SQL 的简洁性和编程语言擅长表达复杂逻辑的优点。而在使用 Spark SQL 时，Spark 执行引擎也能根据数据的结构信息对查询进行优化，让我们从中获益。


### 第10章 Spark Streaming

许多应用需要即时处理收到的数据，例如用来实时追踪页面访问统计的应用、训练机器学习模型的应用，还有自动检测异常的应用。Spark Streaming 是 Spark 为这些应用而设计的模型。它允许用户使用一套和批处理非常接近的 API 来编写流式计算应用，这样就可以大量重用批处理应用的技术甚至代码。

和 Spark 基于 RDD 的概念很相似，Spark Streaming 使用离散化流（discretized stream）作为抽象表示，叫作 DStream。DStream 是随时间推移而收到的数据的序列。在内部，每个时间区间收到的数据都作为 RDD 存在，而 DStream 是由这些 RDD 所组成的序列（因此得名“离散化”）。DStream 可以从各种输入源创建，比如 Flume、Kafka 或者 HDFS。创建出来的 DStream 支持两种操作，一种是转化操作（transformation），会生成一个新的 DStream，另一种是输出操作（output operation），可以把数据写入外部系统中。DStream提供了许多与 RDD 所支持的操作相类似的操作支持，还增加了与时间相关的新操作，比如滑动窗口。

和批处理程序不同，Spark Streaming 应用需要进行额外配置来保证 24/7 不间断工作。本章会讨论检查点（checkpointing）机制，也就是把数据存储到可靠文件系统（比如 HDFS）上的机制，这也是Spark Streaming 用来实现不间断工作的主要方式。此外，还会讲到在遇到失败时如何重启应用，以及如何把应用设置为自动重启模式。

最后，就 Spark 1.1 来说，Spark Streaming 只可以在 Java 和 Scala 中使用。试验性的Python支持在 Spark 1.2 中引入，不过只支持文本数据。本章就只用 Java 和 Scala 来展示所有的 API，不过类似的概念对 Python 也是适用的。


### 第11章 基于MLlib的机器学习

MLlib 是 Spark 中提供机器学习函数的库。它是专为在集群上并行运行的情况而设计的。MLlib 中包含许多机器学习算法，可以在 Spark 支持的所有编程语言中使用。本章会展示如何在你的程序中调用MLlib，并且给出一些常用的使用技巧。

##### 11.1 概述

MLlib 的设计理念非常简单：把数据以 RDD 的形式表示，然后在分布式数据集上调用各种算法。MLlib 引入了一些数据类型（比如点和向量），不过归根结底，**MLlib 就是 RDD上一系列可供调用的函数的集合**。比如，如果要用 MLlib 来完成文本分类的任务（例如识别垃圾邮件），你只需要按如下步骤操作。
(1)首先用字符串 RDD 来表示你的消息。
(2)运行 MLlib 中的一个特征提取（feature extraction）算法来把文本数据转换为数值特征（适合机器学习算法处理）；该操作会返回一个向量 RDD。
(3)对向量 RDD 调用分类算法（比如逻辑回归）；这步会返回一个模型对象，可以使用该对象对新的数据点进行分类。
(4)使用 MLlib 的评估函数在测试数据集上评估模型。

##### 11.3 机器学习基础

在开始讲 MLlib 中的函数之前，先来简单回顾一下机器学习的相关概念。

机器学习算法尝试根据训练数据（training data）使得表示算法行为的数学目标最大化，并以此来进行预测或作出决定。机器学习问题分为几种，包括分类、回归、聚类，每种都有不一样的目标。拿分类（classification）作为一个简单的例子：分类是基于已经被标记的其他数据点（比如一些已经分别被标记为垃圾邮件或非垃圾邮件的邮件）作为例子来识别一个数据点属于几个类别中的哪一种（比如判断一封邮件是不是垃圾邮件）。

所有的学习算法都需要定义每个数据点的特征（feature）集，也就是传给学习函数的值。举个例子，对于一封邮件来说，一些特征可能包括其来源服务器、提到 free 这个单词的次数、字体颜色等。在很多情况下，正确地定义特征才是机器学习中最有挑战性的部分。例如，在产品推荐的任务中，仅仅加上一个额外的特征（例如我们意识到推荐给用户的书籍可能也取决于用户看过的电影），就有可能极大地改进结果。

大多数算法都只是专为数值特征（具体来说，就是一个代表各个特征值的数字向量）定义的，因此提取特征并转化为特征向量是机器学习过程中很重要的一步。例如，在文本分类中（比如垃圾邮件和非垃圾邮件的例子），有好几个提取文本特征的方法，比如对各个单词出现的频率进行计数。

当数据已经成为特征向量的形式后，大多数机器学习算法都会根据这些向量优化一个定义好的数学函数。例如，某个分类算法可能会在特征向量的空间中定义出一个平面，使得这个平面能“最好”地分隔垃圾邮件和非垃圾邮件。这里需要为“最好”给出定义（比如大多数数据点都被这个平面正确分类）。算法会在运行结束时返回一个代表学习决定的模型（比如这个选中的平面），而这个模型就可以用来对新的点进行预测（例如根据新邮件的特征向量在平面的哪一边来决定它是不是垃圾邮件）。

最后，大多数机器学习算法都有多个会影响结果的参数，所以现实中的机器学习流水线会训练出多个不同版本的模型，然后分别对其进行评估（evaluate）。要这么做的话，通常需要把输入数据分为“训练集”和“测试集”，并且只使用前者进行训练，这样就可以用后者来检验模型是否过度拟合（overfit）了训练数据。MLlib 提供了几个算法来进行模型评估。

![](https://upload-images.jianshu.io/upload_images/4311027-ccc70e60a7f7a6cd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**示例：垃圾邮件分类**

```
# 例11-1：Python 版垃圾邮件分类器
from pyspark.mllib.regression import LabeledPoint
from pyspark.mllib.feature import HashingTF
from pyspark.mllib.classification import LogisticRegressionWithSGD

# spam.txt：垃圾邮件，normal.txt：非垃圾邮件，每行一个
# 根据词频把每个文件中的文本转化为特征向量，然后训练出一个可以把两类消息分开的逻辑回归模型
spam = sc.textFile("spam.txt")
normal = sc.textFile("normal.txt")
# 创建一个 HashingTF 实例来把邮件文本映射为包含 10000 个特征的向量，从文本数据构建词频（term frequency）特征向量
tf = HashingTF(numFeatures = 10000)
# 各邮件都被切分为单词，每个单词被映射为一个特征
spamFeatures = spam.map(lambda email: tf.transform(email.split(" ")))
normalFeatures = normal.map(lambda email: tf.transform(email.split(" ")))
# 创建 LabeledPoint 数据集分别存放阳性（垃圾邮件）和阴性（正常邮件）的例子
positiveExamples = spamFeatures.map(lambda features: LabeledPoint(1, features))
negativeExamples = normalFeatures.map(lambda features: LabeledPoint(0, features))
trainingData = positiveExamples.union(negativeExamples)
trainingData.cache() # 因为逻辑回归是迭代算法，所以缓存训练数据 RDD
# 使用随机梯度下降法（Stochastic Gradient Descent，简称SGD）实现逻辑回归
model = LogisticRegressionWithSGD.train(trainingData)
# 以阳性（垃圾邮件）和阴性（正常邮件）的例子分别进行测试。首先使用一样的 HashingTF 特征来得到特征向量，然后对该向量应用得到的模型
posTest = tf.transform("O M G GET cheap stuff by sending money to ...".split(" "))
negTest = tf.transform("Hi Dad, I started studying Spark the other ...".split(" "))
print "Prediction for positive test example: %g" % model.predict(posTest)
print "Prediction for negative test example: %g" % model.predict(negTest)
```

11.4　MLlib 数据类型

(1)Vector
一个数学向量。MLlib 既支持稠密向量也支持稀疏向量，前者表示向量的每一位都存储下来，后者则只存储非零位以节约空间。
(2)LabeledPoint
在诸如分类和回归这样的监督式学习（supervised learning）算法中，LabeledPoint 用来表示带标签的数据点。它包含一个特征向量与一个标签（由一个浮点数表示）。
(3)Rating
用户对一个产品的评分，用于产品推荐。
(4)各种 Model 类
每个 Model 都是训练算法的结果，一般有一个 predict() 方法可以用来对新的数据点或数据点组成的RDD 应用该模型进行预测。

大多数算法直接操作由 Vector、LabeledPoint 或 Rating 对象组成的 RDD。你可以用任意方式创建出这些对象，不过一般来说你需要通过对外部数据进行转化操作来构建出 RDD：例如，通过读取一个文本文件或者运行一条 Spark SQL 命令。接下来，使用 map() 将你的数据对象转为 MLlib 的数据类型。

**操作向量**

向量有两种：稠密向量与稀疏向量。稠密向量把所有维度的值存放在一个浮点数数组中。例如，一个 100 维的向量会存储 100 个双精度浮点数。相比之下，稀疏向量只把各维度中的非零值存储下来。当最多只有 10% 的元素为非零元素时，我们通常更倾向于使用稀疏向量（不仅是出于对内存使用的考虑，也是出于对速度的考虑）。许多特征提取技术都会生成非常稀疏的向量，所以这种方式常常是一种很关键的优化手段。

创建向量的方式在各种语言中有一些细微的差别。在 Python 中，你在 MLlib 中任意地方传递的 NumPy 数组都表示一个稠密向量，你也可以使用 mllib.linalg.Vectors 类创建其他类型的向量。而在 Java 和 Scala 中，都需要使用 mllib.linalg.Vectors 类。

```
# 例11-4：用 Python 创建向量
from numpy import array
from pyspark.mllib.linalg import Vectors
# 创建稠密向量<1.0, 2.0, 3.0>
denseVec1 = array([1.0, 2.0, 3.0]) # NumPy 数组可以直接传给 MLlib
denseVec2 = Vectors.dense([1.0, 2.0, 3.0]) # 或者使用 Vectors 类来创建
# 创建稀疏向量<1.0, 0.0, 2.0, 0.0>；该方法只接收向量的维度（4）以及非零位的位置和对应的值
# 这些数据可以用一个 dictionary 来传递，或使用两个分别代表位置和值的 list
sparseVec1 = Vectors.sparse(4, {0: 1.0, 2: 2.0})
sparseVec2 = Vectors.sparse(4, [0, 2], [1.0, 2.0])
```


###### 11.5 算法

##### 11.5.1 特征提取

mllib.feature 包中包含一些用来进行常见特征转化的类。这些类中有从文本（或其他表示）创建特征向量的算法，也有对特征向量进行正规化和伸缩变换的方法。

**TF-IDF**

词频—逆文档频率（简称 TF-IDF）是一种用来从文本文档（例如网页）中生成特征向量的简单方法。它为文档中的每个词计算两个统计值：一个是词频（TF），也就是每个词在文档中出现的次数，另一个是逆文档频率（IDF），用来衡量一个词在整个文档语料库中出现的（逆）频繁程度。这些值的积，也就是TF × IDF，展示了一个词与特定文档的相关程度（比如这个词在某文档中很常见，但在整个语料库中却很少见）。

MLlib 有两个算法可以用来计算 TF-IDF：HashingTF 和 IDF， 都在 mllib.feature 包内。

(1)HashingTF 从一个文档中计算出给定大小的词频向量。为了将词与向量顺序对应起来，它使用了哈希法（hasing trick）。在类似英语这样的语言中，有几十万个单词，因此将每个单词映射到向量中的一个独立的维度上需要付出很大代价。而 HashingTF 使用每个单词对所需向量的长度 S 取模得出的哈希值，把所有单词映射到一个0 到S-1 之间的数字上。由此我们可以保证生成一个 S 维的向量。在实践中，即使有多个单词被映射到同一个哈希值上，算法依然适用。MLlib 开发者推荐将S 设置在 218 到 220 之间。

(2)当你构建好词频向量之后，你就可以使用 IDF 来计算逆文档频率，然后将它们与词频相乘来计算TF-IDF。

```
# 例11-7：在 Python 中使用 HashingTF
# HashingTF 可以一次只运行于一个文档中，也可以运行于整个 RDD 中。它要求每个“文档”都使用对象的可迭代序列来表示：例如 Python 中的 list 或 Java 中的 Collection
from pyspark.mllib.feature import HashingTF
sentence = "hello hello world"
words = sentence.split() # 将句子切分为一串单词
tf = HashingTF(10000) # 创建\一个向量，其尺寸 S = 10,000
tf.transform(words)
SparseVector(10000, {3065: 1.0, 6861: 2.0})
rdd = sc.wholeTextFiles("data").map(lambda (name, text): text.split())
tfVectors = tf.transform(rdd) # 对整个 RDD 进行转化操作

# 例11-8：在 Python 中使用 TF-IDF
from pyspark.mllib.feature import HashingTF, IDF
# 将若干文本文件读取为TF向量
rdd = sc.wholeTextFiles("data").map(lambda (name, text): text.split())
tf = HashingTF()
tfVectors = tf.transform(rdd).cache()
# 首先对 IDF 对象调用 fit() 方法来获取一个 IDFModel，它代表语料库中的逆文档频率
# 接下来，对模型调用 transform() 来把 TF 向量转为 IDF 向量
idf = IDF()
idfModel = idf.fit(tfVectors)
tfIdfVectors = idfModel.transform(tfVectors)
```

**缩放**

大多数机器学习算法都要考虑特征向量中各元素的幅值，并且在特征缩放调整为平等对待时表现得最好（例如所有的特征平均值为0，标准差为1）。

```
# 例11-9：在 Python 中缩放向量
from pyspark.mllib.feature import StandardScaler
vectors = [Vectors.dense([-2.0, 5.0, 1.0]), Vectors.dense([2.0, 0.0, 1.0])]
dataset = sc.parallelize(vectors) # 构建特征向量
scaler = StandardScaler(withMean=True, withStd=True) # 进行缩放
model = scaler.fit(dataset) # 为每一列计算平均值和标准差
result = model.transform(dataset) # 缩放一个数据集
# 结果：{[-0.7071, 0.7071, 0.0], [0.7071, -0.7071, 0.0]}
```

**正规化**

在一些情况下，在准备输入数据时，把向量正规化为长度1 也是有用的。使用 Normalizer 类可以实现，只要使用 Normalizer.transform(rdd) 就可以了。

**Word2Vec**

Word2Vec 是一个基于神经网络的文本特征化算法，可以用来将数据传给许多下游算法。Spark 在 mllib.feature.Word2Vec 类中引入了该算法的一个实现。

要训练 Word2Vec，你需要传给它一个用 String 类（每个单词用一个）的 Iterable 表示的语料库。和前面的“TF-IDF”一节所讲的很像，Word2Vec 也推荐对单词进行正规化处理（例如全部转为小写、去除标点和数字）。当你通过 Word2Vec.fit(rdd) 训练好模型之后，你会得到一个 Word2VecModel，可以用来将每个单词通过 transform() 转为一个向量。注意，Word2Vec 算法中模型的大小等于你的词库中的单词数乘以向量的大小（向量大小默认为 100）。你可能希望筛选掉不在标准字典中的单词来控制模型大小。一般来说，比较合适的词库大小约为 100000 个词。

##### 11.5.2 统计

不论是在即时的探索中，还是在机器学习的数据理解中，基本的统计都是数据分析的重要部分。MLlib 通过 mllib.stat.Statistics 类中的方法提供了几种广泛使用的统计函数，这些函数可以直接在 RDD 上使用。

##### 11.5.3 分类与回归

分类与回归是监督式学习的两种主要形式。监督式学习指算法尝试使用有标签的训练数据（也就是已知结果的数据点）根据对象的特征预测结果。分类和回归的区别在于预测的变量的类型：在分类中，预测出的变量是离散的（也就是一个在有限集中的值，叫作类别）；比如，分类可能是将邮件分为垃圾邮件和非垃圾邮件，也有可能是文本所使用的语言。在回归中，预测出的变量是连续的（例如根据年龄和体重预测一个人的身高）。

**线性回归**

线性回归是回归中最常用的方法之一，是指用特征的线性组合来预测输出值。

```
# 例11-10：Python 中的线性回归
from pyspark.mllib.regression import LabeledPoint
from pyspark.mllib.regression import LinearRegressionWithSGD
points = # (创建 LabeledPoint 组成的 RDD)
model = LinearRegressionWithSGD.train(points, iterations=200, intercept=True)
print "weights: %s, intercept: %s" % (model.weights, model.intercept)
```

**逻辑回归**

逻辑回归是一种二元分类方法，用来寻找一个分隔阴性和阳性示例的线性分割平面。

**支持向量机**

支持向量机（简称 SVM）算法是另一种使用线性分割平面的二元分类算法，同样只预期 0 或者 1 的标签。

**朴素贝叶斯**

朴素贝叶斯（Naive Bayes）算法是一种多元分类算法，它使用基于特征的线性函数计算将一个点分到各类中的得分。这种算法通常用于使用 TF-IDF 特征的文本分类，以及其他一些应用。

**决策树与随机森林**

决策树是一个灵活的模型，可以用来进行分类，也可以用来进行回归。决策树以节点树的形式表示，每个节点基于数据的特征作出一个二元决定（比如，这个人的年龄是否大于 20？），而树的每个叶节点则包含一种预测结果（例如，这个人是不是会买一个产品？）。决策树的吸引力在于模型本身容易检查，而且决策树既支持分类的特征，也支持连续的特征。

![](https://upload-images.jianshu.io/upload_images/4311027-a769637197858ec0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 11.5.4 聚类

聚类算法是一种无监督学习任务，用于将对象分到具有高度相似性的聚类中。前面提到的监督式任务中的数据都是带标签的，而聚类可以用于无标签的数据。该算法主要用于数据探索（查看一个新数据集是什么样子）以及异常检测（识别与任意聚类都相距较远的点）。

##### 11.5.5 协同过滤与推荐

协同过滤是一种根据用户对各种产品的交互与评分来推荐新产品的推荐系统技术。协同过滤吸引人的地方就在于它只需要输入一系列用户/ 产品的交互记录：无论是“显式”的交互（例如在购物网站上进行评分）还是“隐式”的（例如用户访问了一个产品的页面但是没有对产品评分）交互皆可。仅仅根据这些交互，协同过滤算法就能够知道哪些产品之间比较相似（因为相同的用户与它们发生了交互）以及哪些用户之间比较相似，然后就可以作出新的推荐。

尽管 MLlib 的 API 使用了“用户”和“产品”的概念，但你也可以将协同过滤用于其他应用场景中，比如在社交网络中推荐用户，为文章推荐要添加的标签，为电台推荐歌曲等。

**交替最小二乘**

MLlib 中包含交替最小二乘（简称 ALS）的一个实现，这是一个协同过滤的常用算法，可以很好地扩展到集群上。

##### 11.5.6 降维

**主成分分析**

给定一个高维空间中的点的数据集，我们经常需要减少点的维度，来使用更简单的工具对其进行分析。例如，我们可能想要在二维平面上画出这些点，或者只是想减少特征的数量使得模型训练更加高效。

机器学习社区中使用的主要的降维技术是主成分分析（简称PCA）。在这种技术中，我们会把特征映射到低维空间，让数据在低维空间表示的方差最大化，从而忽略一些无用的维度。要计算出这种映射，我们要构建出正规化的相关矩阵，并使用这个矩阵的奇异向量和奇异值。与最大的一部分奇异值相对应的奇异向量可以用来重建原始数据的主要成分。

**奇异值分解**

MLlib 也提供了低层的奇异值分解（简称SVD）原语。对于大型矩阵，通常不需要进行完全分解，只需要分解出靠前的奇异值和与之对应的奇异向量即可。这样可以节省存储空间、降噪，并有利于恢复低秩矩阵。

##### 11.5.7 模型评估

无论机器学习任务使用的是何种算法，模型评估都是端到端机器学习流水线的一个重要环节。许多机器学习任务可以使用不同的模型来应对，而且即使使用的是同一个算法，参数设置也可以带来不同的结果。不仅如此，我们还要考虑模型对训练数据过度拟合的风险，因此你最好通过在另一个数据集上测试模型来对模型进行评估，而不是使用训练数据集。

##### 11.6 一些提示与性能考量

##### 11.6.1 准备特征

尽管机器学习演讲中经常着重强调所使用的算法，但切记在实践中，每个算法的好坏只取决于你所使用的特征！许多从事大规模数据机器学习的人员都认为特征准备是大规模机器学习中最重要的一步。添加信息更丰富的特征（例如与其他数据集连接以引入更多信息）与将现有特征转为合适的向量表示（例如缩放向量）都能极大地帮助改进结果。

##### 11.6.2 配置算法

在正规化选项可用时，MLlib 中的大多数算法都会在正则化打开时表现得更好（在预测准确度方面）。此外，大多数基于 SGD 的算法需要大约 100 轮迭代来获得较好的结果。MLlib 尝试提供合适的默认值，但是你应该尝试增加迭代次数，来看看是否能够提高精确度。例如，使用 ALS 算法时，rank 的默认值10 相对较低，所以你应该尝试提高这个值。确保在评估这些参数变化时将测试数据排除在训练集之外。

##### 11.6.3 缓存RDD以重复使用

MLlib 中的大多数算法都是迭代的，对数据进行反复操作。因此，在把输入数据集传给 MLlib 前使用cache() 将它缓存起来是很重要的。即使数据在内存中放不下，你也应该尝试 persist。

##### 11.6.4 识别稀疏程度

当你的特征向量包含很多零时，用稀疏格式存储这些向量会为大规模数据集节省巨大的时间和空间。在空间方面，当至多三分之二的位为非零值时，MLlib 的稀疏表示比它的稠密表示要小。在数据处理代价方面，当至多 10% 的位为非零值时，稀疏向量所要花费的代价也会更小。（这是因为使用稀疏表示需要对向量中的每个元素执行的指令比使用稠密向量表示时要多。）但是如果使用稀疏表示能够让你缓存使用稠密表示时无法缓存的数据，即使数据本身比较稠密，你也应当选择稀疏表示。

##### 11.6.5 并行度

对于大多数算法而言，你的输入 RDD 的分区数至少应该和集群的 CPU 核心数相当，这样才能达到完全的并行。回想一下，默认情况下 Spark 会为文件的每个“块”创建一个分区，而块一般为 64 MB。你可以通过向 SparkContext.textFile() 这样的函数传递分区数的最小值来改变默认行为：例如 sc.textFile("data.txt", 10)。另一种方法是对 RDD 调用 repartition(numPartitions) 来将 RDD 分区成 numPartitions 个分区。你始终可以通过 Spark 的网页用户界面看到每个 RDD 的分区数。同时，注意不要使用太多分区，因为这会增加通信开销。

##### 11.7 流水线 API

流水线就是一系列转化数据集的算法（要么是特征转化，要么是模型拟合）。流水线的每个步骤都可能有参数（例如逻辑回归中的迭代次数）。流水线 API 通过使用所选的评估矩阵评估各个集合，使用网格搜索自动找到最佳的参数集。

##### 11.8 总结

本章概述了 Spark 的机器学习算法库。如你所见，MLlib 与 Spark 的其他 API 紧密联系。它可以让你操作 RDD，而得到的结果也可以在其他 Spark 函数中使用。MLlib 也是 Spark开发最为活跃的组件之一，它还在不断发展中。
