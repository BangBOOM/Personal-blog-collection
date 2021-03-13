# Spark工作流程

基本模块

+ RDD(Resilient,Distributed,Dataset) 不可变的分布式对象集合
+ DAG 有向无环图: 描述了RDD的依赖关系，这种关系也被称之为lineage，RDD的依赖关系使用Dependency维护。
+ 宽依赖、窄依赖，DAGScheduler根据 wide dependency划分Stage
+ RDDs 算子: Transformations and Actions, **Spark is lazy**

- Transformations:
  - Narrow Transformation: map, union, flatmap, filter...
  - Wide Transformation: join, reduceByKey, groupByKey, sortByKey...
- Action: reduce, count, countByKey, collect, first, take, foreach, OutputFunction like saveAsTextFile

![Spark-DAG](C:\Users\Wentw\Desktop\Spark-DAG.png)

Job: 提交给Spark的任务。 Stage：每一个奇偶比处理过程要分为的几个阶段（根据宽依赖划分）

stage里的task的数量由输入文件的切片个数来决定的。
在HDFS中不大于128m的文件算一个切片（默认128m）。
通过算子修改了某一个rdd的分区数量，task数量也会同步修改。

Task：是每一个job处理过程要分为几次任务。Task是任务运行的最小单位。最终是要以task为单位运行在executor中。

```tex
Job<---->n stage<--->n task (n>=1)
```

运行流程

1. 构建Spark Application的运行环境（启动SparkContext），SparkContext向资源管理器（可以是Standalone、Mesos或YARN）注册并申请运行Executor资源；
2. 资源管理器分配Executor资源并启动StandaloneExecutorBackend。
3. SparkContext构建成DAG图，将DAG图分解成Stage，并把Taskset发送给Task Scheduler。Executor向SparkContext申请Task，Task Scheduler将Task发放给Executor运行同时SparkContext将应用程序代码发放给Executor。
4. Task在Executor上运行，运行完毕释放所有资源。

