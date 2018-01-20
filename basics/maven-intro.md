# Maven项目构建

知识点

* lifecycle中各个操作的含义，整个工作模型梳理[DONE]
* 子模块，代码目录结构组织的一些基本原则[DONE]
* 依赖和仓库相关[DONE]
* 常用配置[DONE]
* 原型项目[DONE]
* 插件[DONE]
* 和webpack和npm对比一下





## lifecycle

构建（building）和分发（distributing）一个项目（project/artifact）的过程



三种内置的lifecycles:

* default：项目发布部署（deployment）
* clean：项目清理（cleaning）
* site：创建项目文档（creation of your project's site documentation）



每个build lifecycle包含一系列phases



如default lifecycle包含下列常用的phase

* validate 验证项目结构正确、有必要的配置信息
* compile 编译项目的源代码
* test 使用合适的测试框架测试编译后的源码。不需要源码package或deploy
* package 把编译后的代码打包成待分发的格式，比如jar
* verify 运行集成测试检查确保满足质量需求
* install 将package安装到本地仓库，以便其他本地项目依赖
* deploy 完成本地构建，将最终的package复制到远程仓库



phases之间有顺序的，例如执行install，会先依次执行validate/compile/test/package/verify



可以在build phases之间绑定plugin goals。

一个build phase由多个plugin goals组成。

一个build phase可以绑定零或多个goals。如果一个build phase没有绑定任何goals，这个phase不会执行。如果绑定多个goals，这些goals都会执行。

default lifecycle的build phases大多都有默认绑定的goals



使用build lifecycle的两种方式

* 配置 packaging - 预定义的一些build phase集合
* 配置 plugins - 自己把goal绑定到build phase上，来完成项目的构建



## 依赖和仓库相关

### 仓库

* local repository
* remote repository

结构一样



* 当本地仓库没有的时候或者是SNAPSHOT版本的时候，会去远程仓库下载依赖
* 默认从central repository仓库下载，可以在setting.xml或pom.xml指定mirror全局或局部覆盖。



### Depencency Scope

* __compile__ 默认，dependencies are available in all classpaths of a project
* __provided__ expect the JDK或container在运行时提供依赖。例如在Java Web应用中，Servlet API就可以由容器提供。
* __runtime__ in runtime and test classpaths
* __test__
* system
* import



### Dependency Management

当多个项目有公共的parent project时，可以把依赖信息配置在parent 公共的pom.xml中，在子项目中直接引用。



## 常用配置

* basic project information: version/description/developers....
* project dependencies
* plugins or goals that can be executed
* build profiles



##子项目和模块

* __Project Inheritance__: configure  `<parent></parent>` in child pom.xml
* __Project Aggregation__: commands in parent -> commands in all child modules
  * Change the parent POMs packaging to the value "pom" .
  * Specify in the parent POM the directories of its modules (children POMs)



### Project Interpolation and Variables

* `project.basedir`
* `project.baseUri`



## 原型项目

* [https://maven.apache.org/guides/mini/guide-creating-archetypes.html](https://maven.apache.org/guides/mini/guide-creating-archetypes.html)

