## 第一部分 基础
### 第1章 Doris概述
### 第2章 Doris的安装与部署
### 第3章 Doris数据对象
### 第4章 Doris数据模型详解
## 第二部分 进阶
### 第5章 数据导入实战
### 第6章 Doris数据查询
### 第7章 Doris查询优化
## 第三部分 拓展
### 第8章 Doris流数据
### 第9章 Doris外部表
### 第10章 Doris集群管理
## 第四部分 实战
### 第11章 数据仓库概述
### 第12章 数据仓库设计
### 第13章 基于Doris的OLAP查询和实时数据仓库实战
### 第14章 基于Doris的流批一体数据仓库实战


[补充：Doris基于bitmap精确去重](https://blog.bcmeng.com/post/doris-bitmap.html)

**bitmap**
* 解决精确去重问题
* 聚合指标->支持上卷->保留明细

**bitmap改进**
* 内存和存储占用：压缩，`Roaring bitmap`
* 支持任意类型输入：将任意类型映射到`Int`类型，用全局字典存储映射

**Roaring bitmap**
* 核心思路：根据数据的不同特征采用不同的存储或压缩方式
* 分桶：将整个`Int`域分成高16位和低16位，每个桶最多包含2^16=65536个元素
* 高位：决定数据位于哪个桶
* 低位：数据在桶里的位置

**Roaring bitmap分类**
* 稀疏：`Array Container`（数据离散分布）
* 稠密：`Bitmap Container`（每个区间都有值）
* 连续：`Run Container`（只需记录开始和结束编号）


### 第1章 Doris概述

**MySQL的Sharding方案痛点**
* 大规模数据导入导致读性能大幅下降，有时还会锁表，频繁导入数据时问题更明显
* 数据量达千万级时性能很差，只能从产品层面限制用户查询时间，抑制用户需求
* 单表存储数据有限，数据量过大时查询变慢，随着数据量快速增长，维护成本飙升

**Doris1改进**
* 聚合模型：数据模型将数据分为key列和value列，key相同的行对应的value进行聚合，查询时需要扫描的数据量变小
* 批量读写：将MySQL逐条插入的方式改为批量更新，通过外围模块将同一批次数据进行排序以及预聚合，排序后的数据能起到聚集索引的作用，提高查询性能
* 物化视图：提供天表、月表这种类似物化视图的功能

**Doris3架构重组**
* 引入`ZooKeeper`来存储元数据，解耦组件，实现系统无单点故障，提高系统稳定性（目前已无）
* 引入分区，数据会按照时间进行分区（比如天分区、月分区），同一个分区里数据会根据用户ID进行分布，这样同一个用户数据会落在不同分区，数据查询时多台机器能同时处理一个用户的数据，实现单用户数据分布式计算
* 结合`MySQL`对`SQL`的支持和`Doris`对大量数据的快速处理能力，尽量将计算下推给`Doris`完成
* 重构存储引擎，使用行列式存储，避免查询时读取不必要的列

**行列式存储**
* 行：多行数据存储在一个块内
* 列：块内相同列的数据一同压缩与存放
* 可以根据数据特征，利用不同的压缩算法来提高数据压缩效率

---

**Doris**
* `MPP架构`：大规模并行处理`Massively Parallel Processing`
* `OLAP数据库`

**Doris特点**
* 极简架构：只有`FE`和`BE`两类进程，都可以横向拓展，不依赖任何第三方系统（如`HDFS`、`ZooKeeper`），极大降低运维成本
* 使用简单：在数据分析的全过程（数据建模、数据导入、用户上手分析、持续使用、维护升级）均体现易用性
* 功能丰富：分区分桶裁剪、合理缓存、Bitmap数据类型、物化视图、基于主键的数据更新
* 开放开源

**FE节点**
* 存储管理模块：管理所有的元数据信息，用户的权限信息，数据的导入任务等
* 状态管理模块：管理所有BE进程的存活状态，查询负载等非持久化信息，并提供发布订阅接口
* 协调模块：接收用户发来的请求，进行语句解析，生成执行规划，根据集群状态对执行规划进行调度
* 元数据模块：对元数据的读写（只有`Leader`角色拥有此权限）
* 元数据缓存模块：同步元数据，以供语句解析、生成执行规划（`Follower`和`Observer`角色）

**BE节点**
* 可以无限拓展，所有`BE节点`的角色都是对等的，部分`BE节点`下线不影响提供服务
* 存储引擎：管理本地的`Tablet`数据；发送或接收数据并保存副本；定期合并、更新多个版本的数据以减少存储占用；接收来自查询执行器的数据读取请求和批量数据导入请求
* 查询执行器：`MPP集群`执行查询时，会分解为一个树状的执行树，每个叶子节点分配一个查询执行器

**易用性**
* 数据建模
  - 支持三种模型，满足各种应用场景，只增加分布式系统特性（分布键、分桶）
* 数据导入
  - 提供多种数据导入方案，同时在数据导入过程中保证原子性（完整的事务操作）不会出现部分数据写入的情况
  - 保证导入作业仅能成功导入一次，多次导入会报错（至多一次）结合上游系统的至少一次语义，可以实现精确一次语义
* SQL应用
  - 支持标准的`SQL`语言，兼容`MySQL`方言，极大降低迁移与学习成本
  - 支持复杂聚合语法和`UDF`、`UDAF`自定义
  - 可以代替部分Hive等离线系统功能，使用户在一套数据库中满足所有需求
* 工具
  - `FE节点`兼容`MySQL`协议，方便用户用各种类库进行连接，支持各种开发语言和工具
* 集群可靠性
  - 采用“内存存储+检查点+镜像日志文件”模式，管理多副本并自行修复
  - 部分节点失效，集群正常运行，数据不会丢失
  - 无外部依赖，仅需`FE和BE模块`即可搭建集群
  - 支持在线更改表（加减列、创建`Rollup`）不会影响当前服务，不会阻塞读写等操作
* 集群扩缩容
  - 基于分布式管理框架，自动管理数据副本的分布、修复和均衡，不影响其他服务，无需运维人员进行额外操作
* 集群升级
  - 只需替换二进制程序，滚动重启集群即可
  - 版本完全向前兼容，支持通过灰度发布方式进行新版本的验证和测试

**分区分桶裁剪**
* 第一层：分区，支持范围和列表的划分方式
* 第二层：分桶，将数据通过哈希值进行水平划分，数据分片`Tablet`在集群中被均匀打散
* 分桶裁剪：将查询固定到极少数分片上，降低单个查询的资源消耗，提升集群并发查询能力
![图1-8](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris1-8.png)

**合理缓存**
* `SQL级别缓存`：以`SQL`语句的哈希值为`key`，直接缓存`SQL`查询结果，适合更新频率不高但查询频繁的场景
* `Partition级别缓存`：将`SQL`查询结果中不同分区的数据进行存储，之后查询时可以利用已缓存分区的数据及新分区实时查询数据得到最终结果，降低重复数据查询，减少系统资源消耗
![图1-9](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris1-9.png)

**Bitmap数据类型**
* 利用位图来存储整形数据，降低内存开销
* 可用于高基数精确去重场景

**去重方式对比**
* 传统去重：在内存中构建哈希表，在基数非常高的情况下会占用大量内存
* 位图去重：将数值型转换成位图上的0和1，去重时将多个位图求交集并计算1的个数

**物化视图**
* 满足用户对原始明细数据任意维度分析，快速对固定维度进行分析和查询
* 可以使用明细数据模型存储明细数据，在此基础上选择任意维度和指标创建聚合物化视图
* 物理表中数据发生修改，物化视图会自动更新，保证原始表和物化视图中的数据一致
* 物化视图对用户透明，会自动根据查询语句匹配到最合适的物化视图进行查询

---

**Doris核心设计**
* 存储引擎
* 查询引擎

**存储引擎**
* 存储：数据以列进行连续存储
* 压缩：列数据类型相同因此压缩率极高
* 编码：对不同的数据类型提供不同的编码方式
* 重复：对于字符串类型，如果列中的重复值较多，不再使用字典编码，切换为`Plain Text`

**文件格式**
* `Segment`：对应一个物理文件
* `Header`：
* `Data Region`：按列存储数据
* `Index Region`：存储数据索引
* `Footer`
* `Page`：最小存储单元
![图1-10](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris1-10.png)

**索引**
* 智能索引：数据写入时自动生成，无需用户干预，包括前缀稀疏索引、MinMax索引等
* 二级索引：在某些列上添加辅助索引，需自行创建，如`Bloom Filter索引`、`Bitmap索引`等

**前缀稀疏索引**
* 建立在排序列结构上，按照排序列有序存储
* 基于排序列数据，每`1024`行创建一个稀疏索引项，索引`key`即当前第一行的前缀排序列的值
* 适用：当用户的查询条件包含这些排序列时，可以快速定位到起始行

**MinMax索引**
* 记录每一列中的最大值和最小值
* 适用：进行等值或范围查询时，快速过滤掉不需要读取的行

![图1-11](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris1-11.png)

**Bloom Filter索引（布隆过滤器）**
* 使用固定空间的位图来快速判断一个值是否存在
* 适用：高基数列上的等值查询

**Bitmap索引**
* `key`值为实际列值，`value`值为`key`在数据文件中的偏移量
* 可以快速定位到列值对应的行号
* 适用：基数较低的列上的等值查询

**延迟物化**
* 根据有索引的列，定位一个查询范围
* 根据有过滤条件的列进一步过滤，以缩小查询范围
* 读取其他需要读取的列
* 优势：减少不必要的数据读取，降低查询对IO的资源消耗

**并行查询设计**
* 基于查询语句生成逻辑执行计划
* 根据数据分布形成物理执行计划
* 按照相同`key`进行数据重分布，更好利用多节点资源并行进行数据处理

![图1-12](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris1-12.png)

**聚合算子优化（两阶段聚合）**
* 第一级聚合：在数据所在节点，以减少第二级聚合的数据
* 第二级聚合：将`key`相同的数据汇聚到同一个节点，进行最终聚合
* 聚合算子：阻塞型算子，需要等全部数据处理完才将数据发送给上层节点

**自适应聚合**
* 在第一级聚合中，如果发现聚合效果很差，无法有效减少传输数据，自动停止聚合
* 转换为非阻塞的流式算子，直接将读取的数据发送到上层节点，减少不必要的阻塞等待

![图1-13](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris1-13.png)

**Join算子优化**
* 右表（数据量小）：`BuildTable`
* 左表（数据量大）：`ProbeTable`
* 先读取右表，在内存中构建哈希表
* 然后开始读取左表数据，并在哈希表中进行连接匹配，返回符合连接条件的数据

**Runtime Filter**
* 内存中构建右表哈希表的同时，为连接列生成一个过滤器，把过滤器推给左表
* 左表利用过滤器对数据进行过滤，减少`Probe`节点需要传输和比对的数据
* 适用于大部分`Join`场景，例如分布式`Shuffle Join`中，先将多个节点产生的过滤器进行合并，再下推到数据读取节点

**过滤器**
* `In Predicate`
* `Bloom Filter`
* `MinMax`

![图1-14](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris1-14.png)

---

**查询优化器**
* 基于规则的查询优化
* 基于代价的查询优化

**基于规则的查询优化**
* 常量折叠：预先对常量表达式进行计算，有助于规划器进行分区裁剪，和索引进行过滤
* 子查询改写：将子查询改写为关联，从而利用`Join`算子的优化来提升查询效率
* 提取公共表达式：将析取范式转换成合取范式，对执行引擎更友好（`OR`->`AND`）
* 智能过滤器：将析取范式转换成合取范式，并提炼出公共条件，预先过滤数据
* 谓词下推：不仅穿透查询层，还能进一步下推到存储层，利用索引进行数据过滤

![图1-15](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris1-15.png)

**基于代价的查询优化**
* 针对`Join`算子进行大量优化

**Join算子优化**
* `Join Recorder`：通过表统计信息，自动调整关联顺序，有效减少中间数据集大小
* `Colocation Join`：利用数据的分布情况，将原本需要去重后才进行关联的数据，在本地进行关联，避免去重时大量的数据传输（需要在建表时就指定表的分布，以保证需要关联的若干表有相同的数据分布）
* `Bucket Join`：`Colocation Join`的通用版本，判断关联条件和数据分布之间的关系，将原本需要同时去重左右两张表中数据的操作，变成将右表数据重分布到左表所在节点，从而减少数据移动

![图1-16](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris1-16.png)
![图1-17](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris1-17.png)
![图1-18](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris1-18.png)

---

**传统数据库**
* 典型的迭代模型，执行计划中的每个算子通过调用下一个算子的`next`方法来获取数据
* 从最底层的数据块中一条条读取数据，最终返回给用户
* 问题：每个`Tuple`都要调用一次函数，开销太大
* 问题：`CPU`每次只处理一条数据，无法利用技术升级带来的新特性（比如`SIMD`）

**向量化执行引擎**
* `Vector`：高效的向量数据结构
* `nextBatch`：批量化处理模型，每次处理一批数据
* `batch`内性能优化：得益于批量处理
* 适用：基于列存储的`OLAP数据库`，极大提高并行查询效率

---

**Doris应用场景**
* 实时大屏：数据高频写入和实时查询（`Doris`对接`Kafka`和`Flink`）
* 固定报表：直接基于明细数据或者轻度汇总数据进行查询，无需预先计算汇总数据
* 自助分析：多维分析，支持任意粒度的组合查询数据（维度数据+事实数据）
* 用户画像：`Bitmap`去重在用户圈选时有巨大优势，快速筛选用户
* 多源联邦查询：快速接入外部数据源，像访问普通表一样，避免手动建立外表映射
* 实时数据仓库：`Kafka`数据可以直接通过`Routine Load`进入`Doris`，也可以通过`FLink`加工处理后写入
* 流批一体数据仓库：一套代码实现全量和增量逻辑；确保数据准确性的前提下满足实时要求

**自助分析场景中Doris的优势**
* 基于行列混合存储数据，只需要提取部分列进行计算，极大减少磁盘的IO操作
* 支持丰富的索引对数据进行预过滤，减少参与计算的数据量，大幅提升查询性能


### 第3章 Doris数据对象

**数据类型**
* 数值类型：`INT`，`DECIMAL`，`LARGEINT`
* 日期时间类型：`DATE`和`DATETIME`，没有`TIMESTAMP`（可读性差）
* 字符串类型：`CHAR`，`VARCHAR`和`TEXT`
* 其他扩展类型：`BITMAP`，`HLL`，`BOOLEAN`

**BITMAP类型**
* 范围：只能用于聚合表，不需要指定长度和默认值，长度根据数据的聚合程度调整
* 建表：搭配`BITMAP_UNION`使用
* 查询：搭配`BITMAP_UNION_COUNT`/`BITMAP_UNION`/`BITMAP_HASH`使用
* 性能：对于千万级以下数据去重，不推荐`BITMAP`，直接用`COUNT DISTINCT`，两者性能差不多

```
// 利用BITMAP查询每小时累计UV
SELECT hour, BITMAP_UNION_COUNT(pv) OVER(ORDER BY hour) AS uv
  FROM (SELECT hour, BITMAP_UNION(device_id) AS pv
          FROM event_log
         WHERE dt = '2020-06-22'
         GROUP BY hour
         ORDER BY 1
       ) final;
```

**HLL类型（HyperLogLog）**
* 统计数据集中唯一值个数的高效近似算法，计算快，节省空间
* 范围：只能用于聚合表，不需要指定长度和默认值，长度根据数据的聚合程度调整
* 对象：不能用在`key`列
* 建表：搭配`HLL_UNION`使用
* 性能：模糊去重性能优于`BITMAP`和`COUNT DISTINCT`
* 误差：误差率通常为1%，最大可达2.3%

**各类算法对比**
* 排序算法：时间复杂度至少为`O(nlogn)`
* 哈希算法：时间复杂度为`O(n)`，仅扫描一次全表，但空间消耗巨大，需存储原始数据
* HLL算法：时间复杂度为`O(n)`，不需要存储原始数据，只需维护HLL数据结构，时间和空间上都有优势

```
// 利用HLL查询每小时累计UV
SELECT hour, HLL_UNION_AGG(pv) OVER(ORDER BY hour) AS uv
  FROM (SELECT hour, HLL_RAW_AGG(device_id) AS pv
          FROM event_log
         WHERE dt = '2020-06-22'
         GROUP BY hour
         ORDER BY 1
       ) final;
```

**BOOLEAN类型**
* 简写为`BOOL`，只有`0`、`1`、`NULL`三个值
* 建议用`CHAR(1)`或`VARCHAR(1)`来代替，字段值用`Y`和`N`

---

**键类型**
* `Aggregate Key`：键相同的记录，值按照指定的聚合类型进行聚合
* `Unique Key`：键相同的记录，值按照导入顺序进行覆盖，键清单为主键字段
* `Duplicate Key`：键清单一般为排序字段或者查询字段，主要用于索引优化

**分布描述**
* 分桶数：支持自定义分桶数，根据集群规模大小选择
* 分布键：支持多个字段组合作为分布键

**数据分布方式（全部切分）**
* 分布：所有表中的数据都切分，平均分布到不同节点
* 查询：任何一次查询都需要所有主节点参与计算和数据交互
* 性能：耗费大量网络资源，不利于提高查询并发能力

**数据分布方式（复制小表）**
* 分布：将小表数据复制到每个节点
* 性能：存储膨胀，数据同步有瓶颈

**Doris数据分布**
* 灵活的分桶数：基于分桶数来分布数据，根据不同表设置不同分桶数
* 支持联合字段的分布键
* 表级副本
* 优势：将数据随机分布到部分`BE节点`，读取数据时有更高的灵活度和并发度

```
// 建表1：OLAP表，哈希分桶，列存，覆盖相同键值记录，设置初始存储介质和冷却时间
CREATE TABLE example_db.table_hash (
    k1 BIGINT,
    k2 LARGEINT,
    v1 VARCHAR (2048) REPLACE,
    v2 SMALLINT SUM DEFAULT "10"
)
ENGINE=olap
AGGREGATE KEY (k1, k2)
DISTRIBUTED BY HASH (k1, k2) BUCKETS 16
PROPERTIES (  // 键值对配置表达式
    "storage_medium" = "SSD",
    "storage_cooldown_time" = "2015-06-04 00:00:00",
    "replication_num" = "3"
);

// 建表2：动态分区表，每天提前创建3天的分区，并删除3天前的分区
CREATE TABLE example_db.dynatic_partition (
    k1 DATE,
    k2 INT,
    k3 SMALLINT,
    v1 VARCHAR (2048),
    v2 DATETIME DEFAULT "2014-02-04 15:36:00"
)
ENGINE=olap
DUPLICATE KEY (k1, k2, k3)
PARTITION BY RANGE (k1)  // 按天分区
DISTRIBUTED BY HASH (k2) BUCKETS 32  // 按键分桶
PROPERTIES (
    "storage_medium" = "SSD",
    "dynamic_partition.time_unit" = "DAY",
    "dynamic_partition.start" = "-3",  // 删除3天前的分区
    "dynamic_partition.end" = "3",     // 创建3天内的分区
    "dynamic_partition.prefix" = "p",  // 分区名=p+8位日期
    "dynamic_partition.buckets" = "32"
);

// 建表3：创建表结构相同的空表
CREATE TABLE test2.table2 LIKE test1.table1 [WITH ROLLUP]  // 可选：同时复制ROLLUP对象
```

**分区表**
* 动态分区：通过`PROPERTIES`设置
* 自定义分区：通过`PARTITION BY`设置

**自定义分区**
* 范围分区：`RANGE`
* 列表分区：`LIST`分区值被明确定义的分区表，可以针对单列或多列进行分区
* 注意：不在分区范围内的数据都会被丢弃

**范围分区**
* `LESS THAN`分区
* `FIXED RANGE`分区：更灵活，左右区间完全由用户决定，不受其他分区影响

**LESS THAN分区**
* 分区：左闭右开区间，首个分区的左边界为最小值
* 空值：存放在包含最小值的分区（该分区删除后将无法导入空值）
* 分区列：可以指定一列或多列，如果分区值缺省默认填充最小值

```
CREATE TABLE example_db.table_less_range (
    k1 DATE,
    k2 INT,
    k3 SMALLINT,
    v1 VARCHAR (2048),
    v2 DATETIME DEFAULT "2014-02-04 15:36:00"
)
ENGINE=olap
DUPLICATE KEY (k1, k2, k3)

// 1、创建LESS THAN分区
PARTITION BY RANGE (k1)
(
PARTITION p1 VALUES LESS THAN ("2014-01-01")
PARTITION p2 VALUES LESS THAN ("2014-07-01")
PARTITION p3 VALUES LESS THAN ("2015-01-01")
)

// 2、创建单列分区
PARTITION BY LIST (k1)
(
PARTITION p1 VALUES IN ("1","2","3")
PARTITION p2 VALUES IN ("4","5","6")
PARTITION p3 VALUES IN ("7","8","9")
)

// 3、创建多列分区
PARTITION BY LIST (k1, k2)
(
PARTITION p1 VALUES IN (("1","beijing"), ("1","shanghai"))
PARTITION p2 VALUES IN (("2","beijing"), ("2","shanghai"))
PARTITION p3 VALUES IN (("3","beijing"), ("3","shanghai"))
)

DISTRIBUTED BY HASH (k2) BUCKETS 32
PROPERTIES (...);
```

**外部表引擎**
通过表定义的`PROPERTIES`来补充连接信息
* `MySQL`
* `Broker`：读取外部数据文件，映射成外部表
* `Hive`
* `Iceberg`
* `ODBC`

```
// 1、MySQL表引擎
// 通过PROPERTIES创建外部表
CREATE TABLE example_db.table_mysql
(...)
ENGINE=mysql
PROPERTIES (
    "host" = "127.0.0.1",
    "port" = "8239",
    "user" = "mysql_user",
    "password" = "mysql_passwd",
    "database" = "mysql_db_test",
    "table" = "mysql_table_test"
);

// 通过RESOURCE对象创建外部表（可复用配置）
CREATE EXTERNAL RESOURCE "mysql_resource"  // 先创建RESOURCE对象
PROPERTIES (
    "type" = "odbc_catalog",
    "host" = "127.0.0.1",
    "port" = "8239",
    "user" = "mysql_user",
    "password" = "mysql_passwd"
);
CREATE EXTERNAL TABLE example_db.table_mysql  // 再基于RESOURCE对象创建表
(...)
ENGINE=mysql
PROPERTIES (
    "odbc_catalog_resource" = "mysql_resource",
    "database" = "mysql_db_test",
    "table" = "mysql_table_test"  // 只需指定库表，配置读取上面设置好的
);

// 2、Broker引擎：读取外部数据文件，映射成外部表
// 读取HDFS数据文件，数据用竖线分割，换行符换行
CREATE TABLE example_db.table_broker
(...)
ENGINE=broker
PROPERTIES (
    "broker_name" = "hdfs",
    "path" = "hdfs://path1,hdfs://path2",
    "column_separator" = "|",
    "line_delimiter" = "\n"
)
BROKER PROPERTIES (
    "username" = "hdfs_user",
    "password" = "hdfs_password"
);

// 3、Hive表引擎
CREATE TABLE example_db.table_hive
(...)
ENGINE=hive
PROPERTIES (
    "database" = "hive_db_name",
    "table" = "hive_table_name",
    "hive.metastore.uris" = "thrift://127.0.0.1:9083"
);

// 4、Iceberg引擎
CREATE TABLE example_db.table_iceberg
(...)
ENGINE=ICEBERG
PROPERTIES (
    "iceberg.database" = "iceberg_db",
    "iceberg.table" = "iceberg_table",
    "iceberg.hive.metastore.uris" = "thrift://127.0.0.1:9083",
    "iceberg.catalog.type" = "HIVE_CATALOG"
);
```

**修改表操作**
* 变更表结构（异步）
* 变更聚合（异步）
* 变更键值对：修改`PROPERTIES`内容
* 变更索引
* 变更分区
* 数据交换：将两张表的数据互换
* 重命名
* 删除表：短时间内可通过`RECOVER`恢复，`FORCE`操作的表不可恢复
* 清空表：清空表中数据，但保留表或分区，被删除的数据不可恢复

```
// 异步操作查看进度
SHOW ALTER;

// 变更表结构：修改key列类型，并移动到另一列后面
ALTER TABLE example_db.my_table MODIFY COLUMN col1 BIGINT KEY AFTER col2;

// 变更键值对：修改默认副本数量
ALTER TABLE example_db.my_table SET ("default.replication_num" = "2");

// 变更索引：删除索引
ALTER TABLE example_db.my_table DROP INDEX [IF EXISTS] idx_date;

// 变更分区：增加分区
ALTER TABLE example_db.my_table ADD PARTITION p1 VALUES LESS THAN ("2014-01-01");

// 重命名：重命名ROLLUP
ALTER TABLE example_db.my_table RENAME ROLLUP rollup1 rollup2;
```

**视图**
* 通过视图完成数据加工（不支持存储过程）
* 只有结构定义，没有数据存储（只存储在`FE节点`的元数据中）
* 更适合于一段逻辑多次调用

```
CREATE VIEW example_db.example_view
(
    k1 COMMENT "first key",
    k2 COMMENT "second key",
    k3 COMMENT "third key",
    v1 COMMENT "first value"
)
COMMENT "my first view"
AS
SELECT c1 AS k1, k2, k3, SUM(vl)
FROM example_table
WHERE k1 = 20160112
ORDER BY k1, k2, k3;
```

**函数**
* 日期函数：取日期的上限和下限
* 正则匹配函数：`REGEXP`/`REGEXP_EXTRACT`/`REGEXP_REPLACE`/`NOT REGEXP`
* `BITMAP函数`
* `JSON函数`：生成`JSON对象`、获取键值
* 表函数：将一条记录转换为多条记录，配合`LATERAL VIEW`使用
* 窗口函数

**COUNT DISTINCT精确去重**
* 优点：可以保留明细数据，灵活性较高
* 缺点：需要进行多次重分布，导致查询性能随着数据量增加而直线下降

**BITMAP函数**
* 解决去重过程中需要多次重分布的问题


### 第4章 Doris数据模型详解

#### 数据模型

**列的分类（业务角度）**
* 维度列：`key`
* 指标列：`value`

**模型分类**
* 明细模型：`DUPLICATE`
* 聚合模型：`AGGREGATE`
* 主键模型：`UNIQUE`
* 只能在建表时指定，一旦创建无法修改

**明细模型**
* 默认数据模型，不会对导入的数据进行任何处理
* 适用场景：数仓ODS层，可以通过物化视图在明细模型基础上建立聚合模型

**聚合模型**
* 需要在建表时显式区分维度列和指标列，自动对维度相同的指标进行聚合
* 聚合类型：`SUM`/`MAX`/`MIN`/`REPLACE`/`BITMAP_UNOIN`/`HLL_UNION`
* 合并方式：读时合并
* 适用场景：固定报表，可以通过`ROLLUP`预聚合极大减少扫描量和计算量

**主键模型**
* 主键唯一性约束，除主键列外，其余列用最新数据更新（同一批次数据顺序不保证）
* 合并方式：写时合并，可以用聚合模型代替，但比聚合模型性能好
* 适用场景：不受聚合模型的约束，发挥列存模式的优势（只读取相关列）
* 特殊情况：主键模型是聚合模型的特例

**数据合并方式**
* 读时合并：不适合需要高频写入和更新的主键模型
* 写时合并：先删除再插入，可以更好地实现行级更新，避免合并操作

**数据聚合阶段（针对聚合模型和主键模型）**
* 数据写入阶段：针对每一个批次的导入数据，先在批次内进行数据合并
* 数据压实阶段：数据写入后，`BE节点`不定期进行跨批次的数据合并
* 数据查询阶段：查询时进行最终聚合

```
// 销售订单数据：明细模型
CREATE TABLE IF NOT EXISTS example_db.sale_order_dk
(
    ticket_id        BIGINT   NOT NULL COMMENT "小票ID",
    ticket_line_id   BIGINT   NOT NULL COMMENT "小票行ID",
    ticket_date      DATE              COMMENT "订单日期",
    sku_code         VARCHAR(60)       COMMENT "商品SKU编码",
    shop_code        VARCHAR(60)       COMMENT "门店编码",
    qty              INT               COMMENT "销售数量",
    amount           DECIMAL(22,4)     COMMENT "销售金额",
    last_update_time DATETIME NOT NULL COMMENT "数据更新时间"
)
DUPLICATE KEY (ticket_id)  // 只是用来指明顺序存储时的列
DISTRIBUTED BY HASH (ticket_id) BUCKETS 3
PROPERTIES (...);

// 销售订单数据：聚合模型
CREATE TABLE IF NOT EXISTS example_db.sale_order_ak
(
    ticket_date      DATE        COMMENT "订单日期",
    sku_code         VARCHAR(60) COMMENT "商品SKU编码",
    shop_code        VARCHAR(60) COMMENT "门店编码",  // 需要将主键列提到前面来
    ticket_id        BITMAP BITMAP_UNION NULL COMMENT "小票ID",
    ticket_line_id   BITMAP BITMAP_UNION NULL COMMENT "小票行ID",  // 计数列
    qty              INT    SUM               COMMENT "销售数量",
    amount           DECIMAL(22,4) SUM COMMENT "销售金额",  // 聚合列
    last_update_time DATETIME NOT NULL COMMENT "数据更新时间"  // 更新列
)
AGGREGATE KEY (ticket_id, sku_code, shop_code)  // 指定主键列
DISTRIBUTED BY HASH (sku_code) BUCKETS 3
PROPERTIES (...);

// 销售订单数据：主键模型
CREATE TABLE IF NOT EXISTS example_db.sale_order_uk
(
    ticket_id        BIGINT NOT NULL   COMMENT "小票ID",
    ticket_line_id   BIGINT NOT NULL   COMMENT "小票行ID",
    ticket_date      DATE              COMMENT "订单日期",
    sku_code         VARCHAR(60)       COMMENT "商品SKU编码",
    shop_code        VARCHAR(60)       COMMENT "门店编码",
    qty              INT               COMMENT "销售数量",
    amount           DECIMAL(22,4)     COMMENT "销售金额",
    last_update_time DATETIME NOT NULL COMMENT "数据更新时间"
)
UNIQUE KEY (ticket_id, ticket_line_id)  // 除主键列外全部更新
DISTRIBUTED BY HASH (ticket_id) BUCKETS 3
PROPERTIES (...);
```

#### 表数据存储

**RCFile**
* 先水平划分：将表分成几个行组
* 再垂直划分：对每个行组内的数据按列进行存储
* 游程编码：相同的数据不会重复存储，节约存储空间
* 懒加载：存储到表中的数据都是压缩的，读取数据时对其进行解压缩，跳过不需要查询的列

**RCFile行组信息**
* `HDFS`块信息：区分块上的相邻行组
* 元数据信息：数据行数、列的字段信息等
* 数据块信息：采用列存方式存储，相同的列都在一个块上

**ORCFile（Doris采用）**
* 基于`RCFile`拓展
* 字典编码：不直接存储字段值，存储字典值和字段在字典中的位置
* `Bit`编码：所有字段都采用`Bit`值采判断列是否为空，空值不占用存储空间
* 优势：具有传统行存储和列存储的优点，实现读写性能和存储空间的均衡，非常高效

**ORCFile组成**
* `Stripe`：每个文件横向切成多个块，每个块内部以列存储，所有列存储在一个文件中
* `Postscript`：存储表的行数、压缩参数、压缩大小、列等信息
* `Stripe Footer`：存储某块的统计结果，包括最大值、最小值、计数等信息
* `File Footer`：存储表的统计结果，以及各块的位置信息
* `Index Data`：存储某块上数据的位置、总行数等信息（偏移量）
* `Row Data`：以流的方式存储数据信息

**ORCFile读取流程**
* 读取块信息
* 读取数据偏移量
* 读取数据

**数据存储方式对比**
![图4-4](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris4-4.png)
![图4-5](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris4-5.png)
![图4-6](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris4-6.png)

![表4-5](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris4t5.png)

#### 分区与分桶

**分区**
* 数据按照分区列划分成若干个分区
* 可以视为逻辑上的最小管理单元
* 数据导入与删除都可以或仅能针对一个分区进行
* 分区类型：范围分区，列表分区
* 删除中间分区不会改变已存在分区的取数范围，但可能出现分区范围断层导致数据丢失
* 不可以添加重复范围的分区

**分桶**
* 分区表：针对每个分区，数据进一步按照哈希值进行分桶，确定数据的具体分布位置
* 非分区表：针对整个表的数据分桶
* 分桶数在建表时指定，也可以后期修改
* 数据分片：分区的每个分桶，是数据移动和复制等操作的最小物理单元
* 分桶类型：仅支持哈希分桶

**分桶数量选择**
* 对查询吞吐和查询并发的权衡

**大吞吐低并发查询场景**
* 分桶数量多，数据分布更均匀
* 如果查询条件不包含分桶列的等值条件，会触发所有分桶同时扫描，增加吞吐减少延迟

**高并发查询场景**
* 分桶数量少，数据更集中
* 对应的点查询可以仅触发一个分桶扫描，多个查询并发时很大概率触发不同的分桶扫描
* 各个查询之间影响较小，尤其是当不同桶分布在不同磁盘时

**最佳实践**
* 分桶列：单字段，且尽可能平均分布数据
* 分桶数：一个分区的分桶数一旦指定不能更改，预先考虑集群扩容情况
* 存储块总数量：分区数 X 分桶数
* 存储块数量原则：一个表的存储块数量略多于整个集群的磁盘数量
* 存储块数据量原则：单个存储块的数据量理论上没有上下界，但要保持合适的大小
* 数据量过小：聚合效果不佳，元数据管理压力大
* 数据量过大：不利于副本迁移和补齐，且提高数据操作失败概率

#### DDL语句执行过程
* `FE节点`启动事件监听器来监听用户连接
* 用户发起执行请求，提交给`FE节点`
* 事件监听器识别命令类型
* 词法解析和语法解析
* 语义解析：判断命令是否合法
* 命令执行
* 元数据修改：`Master FE节点`修改`Catalog`数据，通知`BE节点`创建分区对应的文件夹
* 元数据持久化：把修改信息持久化到日志文件中，以便数据库重启后通过回放恢复元数据


### 第5章 数据导入实战

**数据导入**
* `INSERT INTO VALUES/SELECT`
* `Stream Load`
* `Broker Load`
* `Routine Load`
* `Binlog Load`
* `DataX`
* `Spark Load`
* `Flink Connector`

**Stream Load**
* （同步操作）通过`HTTP`批量导入`CSV`或`JSON`格式数据
* 向`Master FE`发送事务请求
* 从`FE节点`获取导入执行计划，接收实时数据
* 其他`Executor BE节点`接收分发过来的数据，将数据写入存储层

**Broker Load**
* （异步操作）读取数据源中的数据
* 用户创建任务并提交给`FE`
* `FE`根据文件存储大小和文件个数，制定数据分片导入计划
* `FE`按照计划指挥多个`BE`节点导入指定的文件或者分片数据
* `BE`通过`Broker`拉取数据，写入磁盘，完成后反馈消息给`FE`
* `FE`持续下发任务给`BE`直到所有文件数据导入完成，反馈给用户

**Routine Load**
* 实时数据：数据->`Kafka`->`Flink`->`Kafka`->`加载到Doris`（`Lambda架构`）
* 流式数据：`Binlog`或提供接口更新数据到`Kafka`
* `FE`不断发起任务，由`BE`执行导入任务

**Binlog Load**
* 实时同步`MySQL`库的增删改操作，以`Canal`为媒介，`Doris`对其进行解析
* 绕过`Kafka`直接读取`MySQL`的`CDC`日志（`Change Data Capture`数据变更捕获）

```
// 1、INSERT INTO
INSERT INTO test PARTITION (p1, p2) WHTI LABEL label1  // 可以指定作业label
SELECT * FROM test2;

// 2、Stream Load
// 导入CSV格式：可以只导入指定的列，导入时对不同列进行处理
curl --location-trusted -u root -H "timeout:100" -H "label:123" -H "columns: k1, k2, v1=hll_hash(k1), v2=hll_empty(), v3=to_bitmap(k1), v4=bitmap_empty()" -T test_tb2.csv http://127.0.0.1:8030/api/demoDB/test_tbl/_stream_load

// 导入JSON格式：可以只导入指定的列
curl --location-trusted -u root -H "format: json" -H "jsonpaths: [\"$.category\",\"$.price\",\"$.author\"]" -T testData http://127.0.0.1:8030/api/demoDB/test_tbl/_stream_load

// 3、Broker Load
// 导入HDFS数据
LOAD LABEL db.label1(
    DATA INFILE("hdfs://abc.com:8888/user/palo/test/ml/file1")
    INTO TABLE tbl1
    COLUMNS TERMINATED BY ","
    (tmp_c1, tmp_c2)
    SET (id=tmp_c2, name=tmp_c1),
    DATA INFILE("hdfs://abc.com:8888/user/palo/test/ml/file2")
    INTO TABLE tbl2
    COLUMNS TERMINATED BY ","
    (col1, col2)
    WHERE col1 > 1  // 导入时可以加限制条件
)
WITH BROKER 'broker'(
    "username" = "hdfs_username",
    "password" = "hdfs_password"
)
PROPERTIES(
    "timeout" = "3600",
    "max_filter_ratio" = "0.00002"
);

// 4、Routine Load
// 为表创建Kafka导入任务，默认消费所有分区，从有数据的位置开始订阅
CREATE ROUTINE LOAD example_db.test1
ON example_tbl
COLUMNS TERMINATED BY ","
COLUMNS (k1, k2, k3, v1, v2, v3 = k1*100)
PROPERTIES(...)
FROM KAFKA (
   "kafka_broker_list" = "broker1:9092,broker2:9092,broker3:9092",
   "kafka_topic" = "my_topic",
   "property.group.id" = "xxx",
   "property.client.id" = "xxx",
   "property.kafka_default_offsets" = "OFFSET_BEGINNING"  // 从头开始订阅
);

// 5、Binlog Load
// 为多张表创建数据同步作业，一一对应多张MySQL源表，并显式指定列映射
CREATE SYNC test_db.job1 (
    FROM mysql_db.t1 INTO test1 COLUMNS (k1, k2, v1) PARTITIONS (p1, p2),
    FROM mysql_db.t2 INTO test2 COLUMNS (k3, k4, v2) PARTITION p1
)
FROM BINLOG (
    "type" = "canal",
    "canal.server.ip" = "xx.xxx.xxx.xx",
    "canal.server.port" = "12111",
    "canal.destination" = "example",
    "canal.username" = "username",
    "canal.password" = "password"
);
```


### 第6章 Doris数据查询

**条件范围查询**
* `IN(子查询)`：适用于主表大从表小的情况
* `EXISTS`：数据量大的情况下效率更高（实际转换为`LEFT SEMI JOIN`执行）
* `JOIN`：数据量大的情况下效率更高
* `NOT IN(子查询)`：适用于主表大从表小的情况，从主表中逐条取出记录，在从表中匹配每一行，遇到匹配则跳出循环
* `NOT EXISTS`：主表小从表大或两表差不多，且两表都有索引，从两表中分别取出数据，按照索引字段进行匹配（实际转换为`LEFT ANTI JOIN`执行）

---

**JOIN算法实现**
* 循环嵌套连接
* 归并连接
* 哈希连接

**循环嵌套连接**
* 概念：基本连接，查询选择性强，约束性高，且仅返回小部分记录
* 适用：驱动表较小，被驱动表的连接列有唯一索引或选择性强的非唯一索引
* 返回：不断将查询结果返回，不需要等全部循环结束再返回结果
* 总结：大表连接小表，小表作为驱动表

**归并连接（排序合并连接）**
* 概念：不分驱动表，两个表先按照连接列进行排序再连接，返回大数据量结果集
* 适用：等值连接或非等值连接，排序操作开销很大，性能比较差
* 总结：两个小表连接，且连接字段离散

**哈希连接**
* 构建阶段：优化器选择小表作为驱动表，运用哈希函数对连接列进行计算，产生一张哈希表（在内存中完成，运算很快）
* 探测阶段：优化器对被驱动表的连接列运用同样的哈希函数进行计算，与哈希表中的记录对比，返回符合条件的记录
* 适用：等值连接，驱动表记录相对多，连接条件有一定区分度，但是没有索引
* 总结：两个大表连接，且多个字段关联

---

**分布式JOIN优化策略**
* Shuffle Join
* Bucket Shuffle Join
* Broadcast Join
* Colocate Join
* 优先级：`Colocate Join`>`Bucket Shuffle Join`>`Broadcast Join`/`Shuffle Join`

**Shuffle Join**
* 概念：将两张表中的数据按照关联字段的哈希值打散，键值相同的数据分配到同一个节点
* 适用：两张表数据量基本相同（小表）
* 总结：两张表都打散

![图6-28](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris6-28.png)

**Bucket Shuffle Join**
* 概念：连接的两张表拥有相同的分布字段，将小表按照大表的分布键进行重分布
* 适用：大表关联小表，连接键为分布字段
* 总结：打散小表，保留大表

![图6-29](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris6-29.png)

**Broadcast Join**
* 概念：将小表复制到所有大表数据的节点，用大表的部分数据关联小表的全部数据
* 适用：大表关联小表
* 总结：复制小表，保留大表

![图6-30](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris6-30.png)

**Colocate Join（Local Join）**
* 概念：多个表关联时没有数据移动和网络传输，每个节点只在本地进行数据关联
* 适用：两个大表关联，均为`OLAP表`，相同的`Colocate_with`属性、分桶键、分桶数和副本数，且分桶键为连接键之一
* 总结：高频查询，性能瓶颈

![图6-31](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris6-31.png)

![表6-3](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris6t3.png)

---

**窗口函数类型**
* 汇总函数：`avg`/`sum`/`max`/`min`/`count`
* 求值函数：`lag`/`lead`/`first_value`/`last_value`
* 排序函数：`rank`/`dense_rank`/`row_number`

**窗口函数运算范围（Window从句）**
* 范围：以当前行为准，前后若干行作为窗口函数运算对象
* 特殊：汇总函数结合分组和排序语句，可用于计算逐行累计值
* 语法：`ROWS BETWEEN`
* 不限范围：`UNBOUNDED PRECEDING`/`UNBOUNDED FOLLOWING`
* 上下若干：`n PRECEDING`/`n FOLLOWING`
* 当前行：`CURRENT ROW`

**排序函数区别**
* `rank`：序号跳跃，可重复
* `dense_rank`：序号连续，可重复
* `row_number`：序号连续，不可重复

```
// 计算累计值
SELECT * FROM (
SELECT t.emp_id,  -- 员工编号
       t.emp_name,  -- 员工姓名
       t.age,
       t.salary,
       t.dept_id,  -- 部门编号
       -- 求和
       SUM(t.salary) OVER() AS sum1,  -- 全局汇总
       SUM(t.salary) OVER(ORDER BY t.emp_id) AS sum2,  -- 逐行累加
       SUM(t.salary) OVER(PARTITION BY t.dept_id) AS sum3,  -- 分组汇总
       SUM(t.salary) OVER(PARTITION BY t.dept_id ORDER BY t.emp_id) AS sum4,  -- 分组逐行累加
       -- 求均值
       AVG(t.salary) OVER() AS comp_avg_salary,  -- 全局均值
       AVG(t.salary) OVER(PARTITION BY t.dept_id) AS dept_avg_salary  -- 分组均值
  FROM emp_info t
) WHERE dept_avg_salary > comp_avg_salary  -- 部门平均工资高于公司平均工资
```

---

**精确去重**
* 基于明细去重：`COUNT DISTINCT`
* 基于预计算去重：`BITMAP`

**基于明细去重（COUNT DISTINCT）**
* 优点：保留明细数据，灵活性较高
* 缺点：计算过程中需要进行多次数据重分布，计算和存储资源消耗极大，性能随数据量增大而直线下降

**基于预计算去重（BITMAP）**
* 适用：不关心明细数据，只想知道去重后的结果（仅支持聚合表）
* 构建：导入数据时将字段转换为`BITMAP`类型，或在列上创建`BITMAP`类型的`ROLLUP`
* 使用：聚合函数为`BITMAP_UNION`
* 转换：在`BITMAP`列上使用`COUNT DISTINCT`，会自动转换为`BITMAP_UNION_COUNT`计算
* 优点：用空间换时间，将计算提前到数据导入过程，减少存储成本和查询时的计算成本
* 降维：`ROLLUP`表降维，进一步减少现场计算的数据集大小

---

**近似去重（HyperLogLog，HLL）**
* 空间复杂度：O(mloglogn)
* 时间复杂度：O(n)
* 计算误差：1%~10%，与数据集大小和采用的哈希函数有关
* 使用方法：与`BITMAP`类似

---

**GROUPING SETS多维组合**
* 提前按照各种指定维度预聚合数据，通过拓展存储来提高特定组合条件下的查询性能
* 拓展子句：`ROLLUP`和`CUBE`

```
SELECT visit_date,
       pate_id,
       COUNT(DISTINCT user_id) AS uv,
       SUM(visit_cnt) AS pv
  FROM page_visit_detail

// 以下两种写法等价
GROUP BY GROUPING SETS ((visit_date,pate_id),(visit_date),());
GROUP BY ROLLUP(visit_date,pate_id);  // ROLLUP：分组字段从后往前减少

// 以下两种写法等价
GROUP BY GROUPING SETS ((visit_date,pate_id),(visit_date),(pate_id),());
GROUP BY CUBE(visit_date,pate_id);  // CUBE：全部分组字段组合
```


### 第7章 Doris查询优化

**执行计划**
* 由`FE节点`生成
* 通过查询语句生成计划树，也就是转化成具体的查询算子
* 把每个查询算子按照数据的分布生成分布式执行计划，包括数据的传输和汇总

**查询优化器**
* 在`FE节点`实现
* 词法、语法解析：按照关键词解析，判断查询语句是否符合语法要求，得到抽象语法树
* 语义解析：检查元数据校验语义准确性，包括表名、列名、列类型等
* 查询改写：表达式改写和子查询改写（`IN`改成`LEFT SEMI JOIN`）
* 单机执行优化：关联顺序优化和谓词下推
* 生成执行计划：结合数据的物理分布，生成分布式执行计划（关键：关联方式选择和分布式聚合）

![图7-1](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris7-1.png)
![图7-2](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris7-2.png)
![图7-6](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris7-6.png)
![图7-8](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris7-8.png)
![图7-9](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris7-9.png)

**关联顺序优化（Join Order）**
* 调整关联顺序，每次查询都找产生中间结果最小的表进行合并
* 适用：多表关联场景
* 例子：表一和表二关联，可能会产生百亿结果，改为先用表一和表三关联后再和表二关联
![图7-10](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris7-10.png)

**谓词下推**
* 把查询条件或者关联条件放到关联操作之前，先过滤无效数据，再执行关联操作

**关联方式选择**
* 关联方式：`Shuffle Join`/`Bucket Shuffle Join`/`Broadcast Join`/`Colocate Join`
* 尽可能引导数据库按照最优的关联方式实现，执行计划中会体现关联方式

**分布式聚合**
* 常规聚合方式：数据按照分组字段进行重分布，相同键值在同一节点执行计算，返回汇总数据
* 两阶段聚合：先在本地对数据进行汇总，然后根据哈希值进行数据重分布，减少网络传输，提高第二阶段汇总速度
![图7-13](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris7-13.png)

---

**索引**
* 内建的智能索引：`ZoneMap`索引和前缀索引
* 用户创建的二级索引：`Bloom FIlter`索引和`BITMAP`索引

**ZoneMap索引**
* 在列存储格式上对每一列索引信息自动维护，包括最大值、最小值、空值个数等
* 对用户透明，无法优化和调整

**前缀索引**
* 表数据可以按照指定的列进行排序存储，以排序列为条件查询非常高效
* 在排序的基础上，根据给定前缀列快速查询数据，以`Block`为粒度创建稀疏索引
* `Block`：包含1024行数据，以第一行数据的前缀列的值为索引
* 构成：一行数据的前`36B`，将表数据的所有排序键顺序拼接起来，超出则截断
* 关键：建表时的字段顺序

**三种数据模型的排序列**
* 聚合模型：按照聚合键排序存储
* 主键模型：按照主键排序存储
* 明细模型：按照明细键排序存储

```
// 前缀索引：按照排序键的字段顺序拼接
SELECT user_id, age, message, effect_time, expire_time  -- 前3个字段为排序键
  FROM table
 WHERE user_id = 1829239 and age = 20  -- 查询列是前缀索引的前缀，效率很高
 WHERE age = 20  -- 比上面的查询条件慢
```

**Bloom FIlter索引（BF索引）**
* 本质上是位图索引，用于快速判断一个给定值是否在一个集合中
* 以`Block`为粒度创建，指定列的值作为一个集合生成索引条目
* 适用：在查询时快速过滤不满足条件的数据，比较适合创建在高基序列上（比如客户号）
* 准确性：有小概率误判可能，返回否时一定不在集合中，返回是时则有可能在集合中，所以只能用来过滤

```
// 建表时新增BF索引
PROPERTIES("bloom_filter_columns"="k1,k2,k3")

// 通过修改表结构增加BF索引
ALTER TABLE my_table SET ("bloom_filter_columns"="k1,k2,k3");
```

**BITMAP索引**
* 适用`bit`数组进行数据存储，占用空间非常小，创建和查询速度非常快
* 位置编码的每一位表示键值对应数据行的有无，一个位图指向成百上千行数据的位置
* 创建：只支持在单列上创建，并且只支持定长类型的字段查询
* 适用：适合低基数查询场景（比如职业、地市等），结果集越小优势越明显
* 键值查询：根据索引快速定位到具体的行
* 逻辑条件：直接用索引的位图进行与、或运算（位运算）

```
// 建表时指定列上创建BITMAP索引
ticket_id BITMAP BITMAP_UNION NULL COMMENT "小票ID"

// 通过修改表命令增加BITMAP索引
ALTER TABLE my_table ADD INDEX bitmap_index (ticket_id) USING BITMAP COMMEMENT "";

// 直接创建BITMAP索引
CREATE INDEX bitmap_index ON my_table (ticket_id) USING BITMAP COMMENT "";
```

---

**物化视图**
* 结合表和视图的优点，既支持动态刷新数据，又可以满足高效查询需求，以空间换时间
* 保证物化视图和基础表的数据一致性，在查询时自动匹配合适的物化视图，无需改变查询
* 聚合场景：基于明细模型表部分列建立聚合视图，同时满足对明细数据和聚合数据的快速查询
* 非聚合场景和补充前缀索引无法实现的场景

**物化视图创建**
* 从单表的大量查询语句中，抽象出频率较高的分组和聚合方式，作为物化视图的定义
* 给常用的维度组合创建物化视图即可，从而达到空间和时间上的平衡

**物化视图查询**
* 只能是基于单表的简单查询，且不能放在子查询中
* 不支持关联操作、字段表达式查询、逻辑匹配、函数处理
* 查询涉及的所有列只允许使用一次
* 可以不指定分组列，如果指定，顺序必须和查询字段顺序一致
* 可以不指定排序列，如果不指定，根据规则自动补充排序列（聚合类型：所有分组列，非聚合类型：前`36B`）

**物化视图保证**
* 一致性：将增删改等对基础表的操作同步到物化视图中，使用增量更新提高更新效率
* 原子性：通过事务方式保证，例如插入数据到基础表，数据会同步插入到物化视图，均写入成功后才会成功返回

**ROLLUP**
* 将数据按照指定粒度进行进一步聚合（在聚合模型和主键模型中，才有上卷的作用）
* 在基础表之上，可以创建任意多个`ROLLUP`表，数据在物理上是独立存储的
* 作用：通过聚合减少数据量，或者修改列顺序以匹配前缀索引
* 命中条件：查询所涉及的所有列（包括查询列和查询条件列）都存在于`ROLLUP`表的列中
* 特殊情况：某种类型的查询（如计数）在任何条件下都无法命中`ROLLUP`表

**ROLLUP与物化视图**
* `ROLLUP`：通过预聚合提升查询效率，但有一定局限性（不能基于明细模型做预聚合）
* 物化视图：覆盖`ROLLUP`功能的同时，还能支持更丰富的聚合函数

![表7-4](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris7t4.png)

```
// 销售明细表
CREATE TABLE sales_detail (
    record_id  INT,
    seller_id  INT,
    store_id   INT,
    sale_date  DATE,
    sale_amt   BIGINT
)
DISTRIBUTED BY HASH (record_id)
PROPERTIES ("replication_num"="1");

// 1、物化视图
// 针对门店销售额统计，创建物化视图
CREATE MATERIALIZED VIEW mv_store_sales AS
SELECT store_id, SUM(sale_amt)
  FROM sales_detail
 GROUP BY store_id;

// 查询门店销售额，从基础表查询，会自动匹配到视图
SELECT store_id, SUM(sale_amt) FROM sales_detail GROUP BY store_id;

// 物化视图补充前缀索引无法实现的场景
CREATE MATERIALIZED VIEW mv_store_sales AS
SELECT store_date,store_id,seller_id,record_id,sale_amt
  FROM sales_detail
 ORDER BY store_date,store_id,seller_id;  -- 针对这三个字段新建稀疏索引

// 2、ROLLUP
// ROLLUP调整排序列顺序，补充前缀索引，达到与物化视图一样的效果
ALTER TABLE sales_detail
ADD ROLLUP store_rollup_index (store_date,store_id,seller_id)
PROPERTIES ("timeout"="3000");

// 针对聚合模型和主键模型，可以在建表时定义ROLLUP，也可以在创建完后添加
CREATE TABLE rollup_index_table (...)
AGGREGATE KEY (event_day,siteid,citycode,username)
DISTRIBUTED BY HASH (siteid) BUCKETS 10
ROLLUP (
    r1(event_day,siteid),
    r2(event_day,citycode),
    r3(event_day)
)  -- 定义ROLLUP组合
PROPERTIES ("replication_num"="3);
```

---

**传统数据库查询**
* 一元一次组的`Pipeline模式`，一行行扫描数据并计算
* `CPU`：大部分时间不是在真正处理数据，而是在遍历查询操作树，有效利用率不高
* 磁盘读写：顺序读写比随机读取效率高，远远跟不上`CPU`处理数据的速率

**列存储+向量化存储引擎解决痛点**
* 列存储：相同列数据存放在一起，数据类型相同，压缩比高
* `OLAP`特点：单词查询读取的列数少，IO总量低，数据读取时间大大缩短
* 向量化存储引擎：将串行执行变成并行执行，大大提升`CPU`处理速率

**向量化**
* 字面角度：一次对一个值进行运算`->`一次对一组值进行运算
* `CPU`角度：将单个应用指令应用于多个数据（用`SIMD`指令进行向量化计算）
* 数据库角度：一行行数据扫描并计算`->`对一列值进行计算，所有算子并行执行
![图7-18](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris7-18.png)

---

**查询优化策略总结**
* 数据模型选择：聚合模型、主键模型、明细模型
* 内存表：将数据行数不多的维度表放进内存里
* 关联方式：`Colocate Join`分布相同的相关表采用相同的分桶列和分桶数量
* 减少大宽表，优选星型模型：区分维度表和指标表
* 维度表：小表放到内存里，少量更新放到`Doris`里，设置更多的副本数来提高效率
* 分区和分桶：范围分区，哈希分桶
* 索引：稀疏索引、`BF`索引、`BITMAP`索引
* 物化视图和ROLLUP


### 第11章 数据仓库概述

**数据仓库**
* 面向主题：对主题进行分析后确定主题的边界（主题域）
* 集成：将不同数据源的数据格式统一，对命名、计量单位等进行规范
* 随时间变化：存储着反映某一历史时点数据的快照，生命周期结束后归档明细数据
* 非易失：一旦进入数据仓库，数据就不应该再有变化，数据是静态的

![图11-2](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris11-2.png)

**Hadoop与MPP数据库的区别**
* `Hadoop`：将执行环境和代码打包给存储数据的节点，子节点分阶段执行并返回结果
* `MPP数据库`：将程序由主节点拆分后交由子节点执行，主节点汇总数据返回给用户

**MPP数据库的优势**
* 模式：数据写入前会进行格式验证，提高数据质量
* 查询速度普遍高于`Hive`内置的查询引擎
* 独立运行，无外部依赖或依赖较少
* 支持删除和修改操作
* 拓展非结构化数据`XML`和`JSON`格式支持
* 支持快速扩容
* 支持标准`SQL`或兼容标准`SQL`，开发更简单

**数据仓库设计理论**
* 设计思想
* 建模思想
* 开发和测试流程
* 上线和持续迭代方案

**大数据技术**
* 为了解决数据量膨胀问题
* 依靠分布式架构分而治之，将大量数据切分成小的数据块来处理


### 第12章 数据仓库设计

**Inmon第三范式建模**
* 适合：业务成熟度高，变化相对缓慢的企业

**Kimball维度建模**
* 自上而下针对某个数据域或者业务进行维度建模，得到最细粒度的事实表和维度表
* 形成适用于某一个数据域或者业务域的数据集市之后，再将各个数据集市集成数据仓库
* 模型：采用多维模型的设计思想（一般是星型模型），保留最小粒度的数据存储
* 要点：保持各集市之间的维度和事实一致，保证各个模块的连通性和关联性，确保没有数据差异
* 适合：更追求业务灵活性的互联网企业、零售企业、快消品企业

**数据仓库分层优势**
* 清晰数据结构：每一层都有不同作用域，设计表的时候应用一套标准，使用时减少沟通成本
* 追踪数据来源：快速定位
* 减少重复开发：规范数据架构分层，开发通用的中间组件，减少重复计算，提高复用性
* 复杂任务拆分：每一层只完成单一的操作，便于保证数据准确性，简化数据修复步骤
* 屏蔽业务影响：业务调整时不需要全部重新接入数据

**数据仓库分层**
* `ODS`：贴源层，保留全部数据，不做任何处理
  - 来自业务库、买点日志、消息队列等
* `DWD`：明细层，基于贴源层做清洗和规范化
  - 规范命名、去除空值、转换码值、整合数据、多源数据对齐
* `DWS`：服务层，基于上层应用和产品的指标需求，构建公共粒度的汇总指标事实表
  - 主要对外服务层，整合明细数据层数据和维度数据，形成大宽表
* `DIM`：维度层，建立一致性维度，降低数据汇总口径和聚合结果不统一的风险
* `ADS`：应用层，面向特定应用创建的数据集，针对不同数据分析场景创建聚合表
* `DWD`/`DWS`/`DIM`同属于`DW`层（数据仓库层）

![图12-3](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris12-3.png)

---

**实时数据仓库**
* `Lambda`架构
* `Kappa`架构

**Lambda架构**
* 批处理层：存储管理主数据集和离线处理数据，基于完整的数据集来重新计算
* 速度处理层：实时处理新数据，提供最新数据的实时视图来最小化延迟
* 响应查询的服务层：合并查询批处理层和速度处理层的数据，将最终结果返回给用户
* 数据迭代：批处理层处理完成后，速度层的旧数据就被替代了
* 缺点：需要同时开发维护两套代码，维护困难；同样的逻辑需要计算两次，占用资源
![图12-4](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris12-4.png)

**Kappa架构**
* 简化版，移除批处理部分
* 需求修改或历史数据重新处理，通过上游重放来完成
* 问题：流式重新处理历史数据的吞吐能力低于批处理，通过增加计算资源来弥补
![图12-5](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris12-5.png)

**流式重新处理**
* 选择具有消息重放功能，能保留历史数据，且支持多消费者的消息队列
* 按照新逻辑写一个新作业，然后从上游消息队列的最开始重新消费，把结果写到新表
* 当新作业赶上进度时，切换应用数据源，读取新结果表数据
* 停止旧作业，删除旧结果表

**两种架构混合**
* 实时指标：`Kappa架构`计算，中间结果落地以支持机器学习引擎、明细查询等需求
* 关键指标：`Lambda架构`完成批处理重新计算，增加一次校对
![图12-7](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris12-7.png)

---

**流批一体**
* 一套班子：统一开发人员，不再区分实时计算开发和离线处理开发
* 一套系统：统一数据处理技术，实时计算和离线处理都使用`Flink`
* 一个逻辑：通过`Flink SQL`让实时计算逻辑和离线处理逻辑保持一致

**流批一体技术选型**
* 消息系统：`Kafka`削峰填谷
* 流式系统：`Flink`
* `OLAP`引擎：`Doris`具备`Upsert`功能，数据查询准确度高，延时小

**ODS数据实时接入**
* `Kafka`->`Flink`->`Doris`
* `Doris`直接读取`MySQL BINLOG`
* `Doris`直接读取`Kafka`

![图12-9](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris12-9.png)
![图12-10](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris12-10.png)
![图12-11](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris12-11.png)


### 第13章 基于Doris的OLAP查询和实时数据仓库实战

**项目背景**
* 某头部运动品牌的子品牌的零售`BI`项目
* 内容：主数据梳理、零售业务分析、商品库存分析

**业务痛点**
* 数据孤岛：打通`SAP`系统、新零售系统、电商系统、渠道系统等业务系统的数据，形成完整的业务视图
* 报表制作：成本高耗时长，底层数据没有统一规范
* 分析维度：企业现有分析维度单一，缺乏全链路数据分析维度
* 数据价值：未能发挥历史数据的价值，缺乏完整商业分析思路

**业务建模**
* 零售模型：以店铺为中心，分为线下和线上，不同电商平台数据采集效率不一
* 库存模型：以店仓为中心，分为大仓库存、门店库存、电商库存

**前端展示**
* 移动端`BI`报表：包括总部零售看板、区域零售看板、门店零售看板、总部商品看板等
* `PC`端自助分析报表：包括零售多维、销存结构多维、售罄率多维自助分析等
* 实时销售报表：包括总部、区域、门店三个维度的当日实时销售数据分析报表

**项目选型**
* 开源解决方案
* 数据抽取：`DataX`
* 实时数据加工：`Flink`
* 离线数据加工：`Hive on Spark`
* 数据查询：`Doris`

![图13-1](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris13-1.png)

**数据抽取**
* 通过接口抽取各个数据库的数据，标准化抽取配置，自动生成建表语句和数据同步任务
* 全量数据抽取
* 增量数据抽取：更复杂，要求数据不丢失、不重复、不多
  - 不丢失：增加修改时间字段
  - 不重复：汇总数据后通过业务主键进行去重和合并
  - 不多：尽可能避免删除操作，使用删除标记字段

**项目结构**
![图13-2](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris13-2.png)

**实时数据加工**
![图13-3](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris13-3.png)


### 第14章 基于Doris的流批一体数据仓库实战

![图14-2](https://raw.githubusercontent.com/zhongjianru/studying/master/images/doris/doris14-2.png)