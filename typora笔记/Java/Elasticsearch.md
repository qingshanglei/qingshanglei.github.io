# [概念：](https://www.elastic.co/cn/elasticsearch/)

ElasticSearch(ES)是一个分布式全文搜索引擎。

Elasticsearch  (简称ES)是一个分布式、高扩展、高实时的、RESTful 风格的搜索与数据分析引擎。它能很方便的使大量数据具有搜索、分析和探索的能力。充分利用Elasticsearch的水平伸缩性，能使数据在生产环境变得更有价值。Elasticsearch 的实现原理主要分为以下几个步骤，首先用户将数据提交到Elasticsearch 数据库中，再通过分词控制器去将对应的语句分词，将其权重和分词结果一并存入数据，当用户搜索数据时候，再根据权重将结果排名，打分，再将返回结果呈现给用户。

   Elasticsearch是面向文档型数据库，一条数据在这里就是一个文档，用JSON作为文档序列化的格式，比如下面这条用户数据：

```json
{
    "name" :     "John",
    "sex" :      "Male",
    "age" :      25,
    "birthDate": "1990/05/01",
    "about" :    "I love to go rock climbing",
    "interests": [ "sports", "music" ]
}
```

##    ElasticSearch和关系型数据区别:

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/ElasticSearch/ElasticSearch%E5%92%8C%E5%85%B3%E7%B3%BB%E5%9E%8B%E6%95%B0%E6%8D%AE%E5%8C%BA%E5%88%AB.png)

###  索引(Index):

一个索引就是一个拥有几分相似特征的文档的集合。比如说，你可以有一个客户数据的索引，另一个产品目录的索引，还有一个订单数据的索引。一个索引由一个名字来标识（必须全部是小写字母），并且当我们要对这个索引中的文档进行索引、搜索、更新和删除的时候，都要使用到这个名字。在一个集群中，可以定义任意多的索引。

能搜索的数据必须索引，这样的好处是可以提高查询速度，比如：新华字典前面的目录就是索引的意思，目录可以提高查询速度。

**Elasticsearch索引的精髓：一切设计都是为了提高搜索的性能。**

### 类型(Type)

在一个索引中，你可以定义一种或多种类型。

一个类型是你的索引的一个逻辑上的分类/分区，其语义完全由你来定。通常，会为具有一组共同字段的文档定义一个类型。不同的版本，类型发生了不同的变化

| 版本    | Type                                               |
| ------- | -------------------------------------------------- |
| 5.x     | 支持多种type                                       |
| 6.x     | 只能有一种type                                     |
| **7.x** | **默认不再支持自定义索引类型（默认类型为：_doc）** |



###  文档(Document)

一个文档是一个可被索引的基础信息单元，也就是一条数据

比如：你可以拥有某一个客户的文档，某一个产品的一个文档，当然，也可以拥有某个订单的一个文档。文档以**JSON（Javascript Object Notation）格式**来表示，而JSON是一个到处存在的互联网数据交互格式。

在一个index/type里面，你可以存储任意多的文档。



#####  字段(Field)

相当于是数据表的字段，对文档数据根据不同属性进行的分类标识。



##### 映射(Mapping)

mapping是处理数据的方式和规则方面做一些限制，如：某个字段的数据类型、默认值、分析器、是否被索引等等。这些都是映射里面可以设置的，其它就是处理ES里面数据的一些使用规则设置也叫做映射，按着最优规则处理数据对性能提高很大，因此才需要建立映射，并且需要思考如何建立映射才能对性能更好。



# [安装：](https://www.elastic.co/cn/downloads/past-releases#elasticsearch)

安装ES流程:

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/ElasticSearch/%E5%AE%89%E8%A3%85ES%E6%B5%81%E7%A8%8B.png)

## 安装ES:

 前提：JDK1.8+。

  解压ES本次7.8版本,解压完后进入bin目录，双击运行elasticsearch.bat。

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/ElasticSearch/%E5%AE%89%E8%A3%85%E6%88%90%E5%8A%9F.png)

测试访问: http://localhost:9200/

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/ElasticSearch/%E4%BD%BF%E7%94%A8ES.png)

**注意：**
   出现闪退，通过路径访问发现“空间不足”
  **修改config/jvm.options文件**的22行23行，把2改成1，让Elasticsearch启动的 时候占用1个G的内存。
  -Xmx512m：设置JVM最大可用内存为512M。
  -Xms512m：设置JVM初始内存512m。此值可设置与-Xmx相同，以避免每次垃圾回收完成后JVM重新分配内存。



## kibana 客户端：

 elasticsearch服务是一个restful风格的http服务。我们可以采用postman作为客户端来进行操作，elastic stack官方也给我们提供了kibana来进行客户端操作，这个相比postman要友好一点，因为里面有些自动补全的代码提示

[下载地址:]( https://www.elastic.co/cn/downloads/past-releases/kibana-7-8-0)

本次7.8版本。



## ik分词器 插件：

概念：

IKAnalyzer是一个开源的，基于Java语言开发的轻量级的中文分词工具包。从2006年12月推出1.0版开始，IKAnalyzer已经推出 了3个大版本。最初，它是以开源项目Lucene为应用主体的，结合词典分词和文法分析算法的中文分词组件。新版本的IKAnalyzer3.0则发展为面向Java的公用分词组件，独立于Lucene项目，同时提供了对Lucene的默认优化实现。

IK分词器3.0的特性如下：
1. 采用了特有的“正向迭代最细粒度切分算法“，具有60万字/秒的高速处理能力。
2. 采用了多子处理器分析模式，支持：英文字母（IP地址、Email、URL）、数字（日期，常用中文数量词，罗马数字，科学计数法），中文词汇（姓名、地名处理）等分词处理。

3. 对中英联合支持不是很好,在这方面的处理比较麻烦.需再做一次查询，同时是支持个人词条的优化的词典存储，更小的内存占用。

4. 支持用户词典扩展定义。
5. 针对Lucene全文检索优化的查询分析器IKQueryParser；采用歧义分析算法优化查询关键字的搜索排列组合，能极大的提高Lucene检索的命中率。



   

[安装：](https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.8.0/elasticsearch-analysis-ik-7.8.0.zip)

​     `ES`安装完成后， 在`ES`的`plugins/ik目录下解压,重新启动ElasticSearch就完成安装了。注意：解压后的zip不要放在plugins目录下。