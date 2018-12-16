# 0.对Spark的一些理解

## 出现是为了解决什么问题？

* MapReduce解决了像写单机程序一样写集群并行计算任务的问题，解决的是任务分布和容错的问题。中间数据复用效率不高，需要写到存储中，涉及重分区、磁盘IO、序列化等开销。
* 迭代算法，交互式数据挖掘。
* 可以让用户显示地指定把中间结果保存在内存中。

# 1.基本数据抽象RDD

## 1.1RDD的本质

* RDD是只读的、分区记录的集合。Distributed memory abstraction。

### 1.1.1 RDD的创建

* data in stable storage
* other RDDs。即Transformation。

### 1.1.2 RDD的核心特性

* RDDs do not need to be materialized all times。RDD含有如何从其他RDD衍生(计算)出本RDD的相关信息(即Lineage)，因此在RDD部分分区数据丢失的时候可以从物理存储的数据计算出相应的RDD分区。
* RDD和DSM(Distributed Shared Memory)模型的区别：RDD只记录转换的操作，不记录数据本身。

###1.1.3 RDD的基本属性

* 一组分片（Partition）。
* 一个计算每个分区的函数。
* RDD之间的依赖关系。
* 一个Partitioner，即RDD的分片函数。
* 一个列表，存储存取每个Partition的优先位置。（对于HDFS文件来说，就是每个Partition所在的块的位置）

```scala

  // =======================================================================
  // Methods that should be implemented by subclasses of RDD
  // =======================================================================

  /**
   * :: DeveloperApi ::
   * Implemented by subclasses to compute a given partition.
   */
  @DeveloperApi
  def compute(split: Partition, context: TaskContext): Iterator[T]

  /**
   * Implemented by subclasses to return the set of partitions in this RDD. This method will only
   * be called once, so it is safe to implement a time-consuming computation in it.
   *
   * The partitions in this array must satisfy the following property:
   *   `rdd.partitions.zipWithIndex.forall { case (partition, index) => partition.index == index }`
   */
  protected def getPartitions: Array[Partition]

  /**
   * Implemented by subclasses to return how this RDD depends on parent RDDs. This method will only
   * be called once, so it is safe to implement a time-consuming computation in it.
   */
  protected def getDependencies: Seq[Dependency[_]] = deps

  /**
   * Optionally overridden by subclasses to specify placement preferences.
   */
  protected def getPreferredLocations(split: Partition): Seq[String] = Nil

  /** Optionally overridden by subclasses to specify how they are partitioned. */
  @transient val partitioner: Option[Partitioner] = None
```

### 1.1.4 RDD操作（API）

* __transformation__ lazy，不会直接计算结果。当发生一个action要求返回结果给Driver时，这些转换才会真正运行。
* __action__ 

### 1.1.5 缓存和检查点

将中间结果缓存

### 1.1.6 DAG的生成 

#### (1) RDD 的依赖关系

* 窄依赖(Narrow Dependency)：每一个parent RDD的partition最多被子RDD的一个Partition使用。

  ![image-20181209154955726](/var/folders/57/4b3xq2g96f3f7prn1qfdtck00000gn/T/abnerworks.Typora/image-20181209154955726.png)

* 宽依赖(Wide Dependency)：多个子RDD的Partition会依赖同一个parent RDD的Partition

  ![image-20181209155014252](/var/folders/57/4b3xq2g96f3f7prn1qfdtck00000gn/T/abnerworks.Typora/image-20181209155014252.png)



#### (2) DAG的生成

RDD经过一系列转换就行程了DAG。RDD之间的依赖关系包含了RDD由哪些parent RDD(s)转换而来和它依赖的parent RDD(s)的哪些Partitions。

Spark根据宽依赖划分Stage。在一个Stage内部，每个Partition都会被分配一个计算任务（Task）。对于窄依赖，由于Partititon依赖关系的确定性，Partition的转换处理就可以在同一个线程里完成，被划分到同一个Stage；对于宽依赖，只能在parent RDD(s) Shuffle处理完成后，才能开始接下来的计算。



### 1.1.7 RDD的计算

原始的RDD经过一系列的转换后，会在最后一个RDD上触发一个动作，这个动作会生成一个Job。在Job被划分为一批计算任务（Task）后，这批Task会被提交到集群的计算节点上去执行。Executor在准备好Task的运行时环境后，会通过调用Task.run()来执行计算。



# 2.一个任务的执行过程

