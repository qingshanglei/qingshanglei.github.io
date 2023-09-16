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



# 安装：



ES安装流程:

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/ElasticSearch/%E5%AE%89%E8%A3%85ES%E6%B5%81%E7%A8%8B.png)

## [安装ES:](https://www.elastic.co/cn/downloads/past-releases#elasticsearch)

 前提：JDK11，但好像JDK1.8+也能用。

   解压ES本次7.8版本,解压完后进入bin目录，双击运行elasticsearch.bat。

| 目录    | 说明           |
| ------- | -------------- |
| bin     | 可执行脚本目录 |
| config  | 配置目录       |
| jdk     | 内置jdk目录    |
| lib     | 类库           |
| logs    | 日志目录       |
| modules | 模块目录       |
| plugins | 插件目录       |

启动成功图：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/ElasticSearch/%E5%AE%89%E8%A3%85%E6%88%90%E5%8A%9F.png)

测试访问: http://localhost:9200/

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/ElasticSearch/%E4%BD%BF%E7%94%A8ES.png)

**注意：**
   出现闪退，通过路径访问发现“空间不足”
  **修改config/jvm.options文件**的22行23行，把2改成1，让Elasticsearch启动的 时候占用1个G的内存。
  -Xmx512m：设置JVM最大可用内存为512M。
  -Xms512m：设置JVM初始内存512m。此值可设置与-Xmx相同，以避免每次垃圾回收完成后JVM重新分配内存。



## [ik分词器-插件：](https://github.com/medcl/elasticsearch-analysis-ik)

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

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/ElasticSearch/%E5%AE%89%E8%A3%85IK.png)



## kibana-客户端工具：

 elasticsearch服务是一个restful风格的http服务。我们可以采用postman作为客户端来进行操作，elastic stack官方也给我们提供了kibana来进行客户端操作，这个相比postman要友好一点，因为里面有些自动补全的代码提示

[下载地址:]( https://www.elastic.co/cn/downloads/past-releases/kibana-7-8-0)

本次7.8版本。

   注意：①要跟ES版本保持一致，否则报错。
​              ②启动Kibbana之前必须先启动ES。
​              ③启动Kibbana需要1多分钟左右，若不行关机重启 或删除再配置一遍。

### 安装：

解压并修改config目录中的kibana.yml文件：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/ElasticSearch/Kibana/%E5%AE%89%E8%A3%85Kibana.png)

启动Kibana:

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/ElasticSearch/Kibana/%E5%90%AF%E5%8A%A8%E6%88%90%E5%8A%9FKana.png)

浏览器访问：http://127.0.0.1:5601

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/ElasticSearch/Kibana/%E6%B5%8F%E8%A7%88%E5%99%A8%E8%AE%BF%E9%97%AEKibana.png)



### Kibana报错 server is not ready yet：

![](../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/SQL/NoSQL/ElasticSearch/Kibana%E5%90%AF%E5%8A%A8%E6%8A%A5%E9%94%99%20server%20is%20not%20ready%20yet.png)

​    注意：此bug有两个可能，第一个Kibana还在加载启动，若是第一个等一会就好了；第二个启动报以下错。

命令行启动报错：

```sh
"warning","migrations","pid":6181,"message":"Another Kibana instance appears to be migrating the index. Waiting for that migration to complete. If no other Kibana instance is attempting migrations, you can get past this message by deleting index .kibana_index_1 and restarting Kibana.
```

解决方法：

```sh
# 1. 停止kibana
service kibana stop
# 2. 删除kibana索引
curl -XDELETE http://localhost:9200/.kibana*
# 3. 启动kibana
service kibana start
```



### 测试分词器:

1. ik_max_word：会将文本做最细粒度的拆分
2. ik_smart：会做最粗粒度的拆分，智能拆分



## 问题：



```sh
# 将JDK版本修改为ES中自带的版本
export JAVA_HOME=/opt/ElasticSearc7.8/elasticsearch-7.8.0/jdk
export PATH=$JAVA_HOME/bin:$PATH

#添加jdk判断
if [ -x "$JAVA_HOME/bin/java" ]; then
        JAVA="/opt/ElasticSearc7.8/elasticsearch-7.8.0/jdk"
else
        JAVA=`which java`
fi
```



# 整合SpringBoot:

依赖：

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
    </dependency>
</dependencies>
```

主启动：

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.openfeign.EnableFeignClients;

@SpringBootApplication(exclude = DataSourceAutoConfiguration.class)//取消数据源自动配置,排序mysql数据库
@EnableDiscoveryClient
@EnableFeignClients
public class ServiceSearchApplication {

    public static void main(String[] args) {
        SpringApplication.run(ServiceSearchApplication.class, args);
    }

}
```



## 实体类：

```java
/**
 * @Document:这是一个使用Elasticsearch的Java文档注解，用于定义索引的名称、分片和副本数。 - `indexName` 参数指定了要创建文档的索引名称，这里设置为"skues"。
 * - `shards` 参数指定了创建索引时的分片数，即将索引的数据分成几个部分存储在不同的节点上。这里设置为3，意味着数据将被分为3个分片。
 * - `replicas` 参数指定了每个分片的副本数，即为每个分片创建多少个备份。这里设置为1，表示每个分片将有一个副本。
 * 通过使用这个注解，可以在创建索引的同时指定索引的名称、分片数和副本数。
 */
@Data
@Document(indexName = "skues", shards = 3, replicas = 1)
public class SkuEs {

    // 商品Id= skuId
    @Id
    private Long id;

    // 指定ES
    //  type：参数指定了字段的类型
    //  FieldType.Text: 表示该字段是文本类型。
    //  analyzer 参数指定了分词器的名称，这里设置为ik_max_word，表示使用ik分词器进行最大化分词。
    @Field(type = FieldType.Text, analyzer = "ik_max_word")
    private String keyword;

    @Field(type = FieldType.Integer, index = false)
    private Integer skuType;

    @Field(type = FieldType.Integer, index = false)
    private Integer isNewPerson;

    @Field(type = FieldType.Long)
    private Long categoryId;

    @Field(type = FieldType.Text)
    private String categoryName;

    //  type 参数指定了字段的类型，这里设置为FieldType.Keyword，表示该字段是关键字类型。
    //  index 参数设置为false，表示该字段不会被索引。
    @Field(type = FieldType.Keyword, index = false)
    private String imgUrl;

    //  es 中能分词的字段，这个字段数据类型必须是 text！keyword 不分词！
    @Field(type = FieldType.Text)
    private String title;

    @Field(type = FieldType.Double)
    private Double price;

    @Field(type = FieldType.Integer, index = false)
    private Integer stock;

    @Field(type = FieldType.Integer, index = false)
    private Integer perLimit;

    @Field(type = FieldType.Integer, index = false)
    private Integer sale;

    @Field(type = FieldType.Long)
    private Long wareId;

    //  商品的热度！
    @Field(type = FieldType.Long)
    private Long hotScore = 0L;

    @Field(type = FieldType.Object, index = false)
    private List<String> ruleList;

}
```

## 主启动：

```java
@SpringBootApplication(exclude = DataSourceAutoConfiguration.class) // 取消数据源自动配置,排序mysql数据库
@EnableDiscoveryClient // 开启服务注册
@EnableFeignClients // 开启服务调用
public class SearchApplication8204 {
    public static void main(String[] args) {
        SpringApplication.run(SpringApplication.class, args);
    }
}
```

## repository:

```java
import com.qsl.ssyx.model.search.SkuEs;
import org.springframework.data.elasticsearch.repository.ElasticsearchRepository;

// ES   继承ElasticsearchRepository主要是继承基本增、删、该、查等接口， ElasticsearchRepository<实体类,实体类主键类型>
public interface SkuRepository extends ElasticsearchRepository<SkuEs, Long> {

}
```

## Service:

```java
@Service
public interface SkuService {


    // 上架商品
    void upperSku(Long skuId);

    // 下架商品
    void lowerSku(Long skuId);
}
```

impl:

```java
import com.qsl.ssyx.client.product.ProductFeignClient;
import com.qsl.ssyx.enums.SkuType;
import com.qsl.ssyx.model.product.Category;
import com.qsl.ssyx.model.product.SkuInfo;
import com.qsl.ssyx.model.search.SkuEs;
import com.qsl.ssyx.search.repository.SkuRepository;
import com.qsl.ssyx.search.service.SkuService;
import org.springframework.beans.factory.annotation.Autowired;

public class SkuServiceImpl implements SkuService {

    @Autowired
    private ProductFeignClient productFeignClient;
    @Autowired
    private SkuRepository skuRepository;


    //    上架商品
    @Override
    public void upperSku(Long skuId) {
        // 根据skuId获取相关信息
        SkuInfo skuInfo = productFeignClient.getSkuInfo(skuId);
        if (skuInfo != null) {
            return;
        }
        Category category = productFeignClient.getCategory(skuInfo.getCategoryId());
        // 获取数据封装SkuES对象
        SkuEs skuEs = new SkuEs();
        //封装分类
        if (category != null) {
            skuEs.setCategoryId(category.getId());
            skuEs.setCategoryName(category.getName());
        }

        skuEs.setId(skuInfo.getId());
        skuEs.setKeyword(skuInfo.getSkuName() + "," + skuEs.getCategoryName());
        skuEs.setWareId(skuInfo.getWareId());
        skuEs.setIsNewPerson(skuInfo.getIsNewPerson());
        skuEs.setImgUrl(skuInfo.getImgUrl());
        skuEs.setTitle(skuInfo.getSkuName());
        if (skuInfo.getSkuType().equals(SkuType.COMMON.getCode())) { // 商品类型：0->普通商品 1->秒杀商品
            skuEs.setSkuType(0);
            skuEs.setPrice(skuInfo.getPrice().doubleValue()); // 金额
            skuEs.setStock(skuInfo.getStock()); // 库存
            skuEs.setStock(skuInfo.getSale()); // 销量
            skuEs.setPerLimit(skuInfo.getPerLimit()); // 限购个数/每天（0：不限购）
        }

        // 调用方法添加ES
        SkuEs save = skuRepository.save(skuEs);
    }

    // 下架商品
    @Override
    public void lowerSku(Long skuId) {
        skuRepository.deleteById(skuId);
    }
}
```



## controller:

```java
@Api(tags = "ES接口")
@RestController
@RequestMapping("api/search/sku")
public class SkuApiController {

    @Autowired
    private SkuService skuService;

    @ApiOperation("上架商品")
    @GetMapping("inner/upper/{skuId}")
    public Result upper(@ApiParam(value = "skuId", required = true) @PathVariable Long skuId) {
        skuService.upperSku(skuId);
        return Result.ok(true);
    }

    @ApiOperation("下架商品")
    @GetMapping("inner/lowerSku/{skuId}")
    public Result lowerSku(@ApiParam(value = "skuId", required = true) @PathVariable Long skuId) {
        skuService.lowerSku(skuId);
        return Result.ok(true);
    }

}
```



```sh
get /_cat/indices?v   # 查看所有索引库(数据库)

POST /skues/_search{  # 查询skues索引库所有数据
  "query:{
    "match_all":{
  }
}
```



