# 概念：

Spring Data是一个用于简化数据库、非关系型数据库、索引库访问，并支持云服务的开源框架。其主要目标是使得对数据的访问变得方便快捷，并支持map-reduce框架和云计算数据服务。 Spring Data可以极大的简化JPA（Elasticsearch…）的写法，可以在几乎不用写实现的情况下，实现对数据的访问和操作。除了CRUD外，还包括如分页、排序等一些常用的功能。 

按照Spring Data的规范的规定，根据方法名称自动实现查询功能。查询方法以`find | read | get`开头（比如`find、findBy、read、readBy、get、getBy`），涉及查询条件时，条件的属性用条件关键字连接，要注意的是：条件属性以首字母大写。直接在接口中定义查询方法，如果是符合规范的，可以不用写实现，即不用写SQL，目前支持的关键字写法如下：

![](../../../%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Java/%E6%A1%86%E6%9E%B6/Spring%20Data/Spring%20Data%E4%B8%8E%E6%95%B0%E6%8D%AE%E5%BA%93.png)

