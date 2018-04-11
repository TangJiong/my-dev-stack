# 开发技术栈和知识整理
* 主要梳理目前工作涉及的大数据和前端两个领域的技术问题
* 也会涵盖相关的网络、算法基础知识以及工程方法
* 内容以原创为主，也会直接搬运看过的好的文章
* 持续更新中



## 基础

### 数据结构和基础算法
* [[Data Structure & Algorithm] 七大查找算法](http://www.cnblogs.com/maybe2030/p/4715035.html#top)[转载]

  > 计算机应用中，查找（搜索）是常用的运算。常见查找算法包括：顺序查找、二分查找、插值查找、斐波拉契查找、树表查找、分块查找、哈希查找。重点掌握树表查找、哈希查找。


### 项目构建

* [项目构建需要了解的maven基础](./basics/maven-intro.md)[原创]

  > 内容包含理解maven的lifecycle机制、mavem的插件机制、maven的依赖管理和仓库、子模块和代码组织、常用配置

## DevOps

### 系统监控与运营
* [Collecting the right data](https://www.datadoghq.com/blog/monitoring-101-collecting-data/)[转载]
  > 系列文章，包括[Collecting the right data](https://www.datadoghq.com/blog/monitoring-101-collecting-data/)、[Alerting on what matters](https://www.datadoghq.com/blog/monitoring-101-alerting/)、[Investigating performance issues](https://www.datadoghq.com/blog/monitoring-101-investigation/)。文章不涉及具体的技术选型，重点提出了一种将监控指标分成`work metrics`、`resource metrics`、`events`三个层次的思路，在回答运营监控系统需要什么数据以及怎么用数据去定位问题时，都可以借鉴这种思路。

## 分布式系统
* 当我们在说分布式系统时，是在说什么 [XMind导图](./distributed_system)
* 分布式系统的基本抽象模型 [XMind导图](./distributed_system)
* 内容整理自[http://book.mixu.net/distsys/index.html](Distributed systems for fun and profit)，内容比较浅，适合入门，可以对分布式系统主要关注点和基本模型有个基本的了解，特别是里面讲CAP和Replica/Patition的部分。
