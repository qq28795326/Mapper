#更新日志

##最新版V2.0.0 - 2015-02-04

* 增加一个`CommonMapper`和包装类`EntityMapper`，建议使用`EntityMapper`
* 有关`EntityMapper`的内容请看独立文档，这个类足以独立成一个开源项目
* 增加对JPA注解`OrderBy`的支持，仅对`select`一个方法有效

简单说明，为什么版本这么快就到了2.0?，因为`EntityMapper`，这是另一种形式的通用Mapper。

这里说说`EntityMapper`和通用Mapper的区别。

通用Mapper需要有继承的接口，需要指定泛型类型，可以缓存，和手写的效果一样。

`EntityMapper`无需继承，可以直接使用，而且这一个对象就可以操作全部的实体对象（和通用Mapper注解要求一样，不支持主键策略）和表，是一个更接近Hibernate用法的类，这个类非常强大，支持Mybatis生成的Example查询，还支持一个通用Example查询。

`EntityMapper`功能更全面，但是不支持主键策略，由于该类足以独立成一个开源项目，简单几句不能说明用法，因此详细内容请看独立的文档。

##V1.1.0

* 完善文档
* 解决主键selectKey的一个bug
* 解决@Column注解为空时的bug
* 完善自动增长的配置，增加对JDBC的支持`@GeneratedValue(generator = "JDBC")`,详细请看下面关于主键策略的详细内容
* 增加了一个`notEmpty`参数，该参数会影响所有使用`getAllIfColumnNode`方法的地方,具体到`Mapper<T>`,影响3个方法：select,selectCount,delete。如果设置为`true`，那么`<if ...`的条件中会包含`String`类型`property!=''`的条件。

##v1.0.0正式发布版

* 增加通用Mapper专用的MyBatis Generator插件，可以自动生成实体类注解以及Mapper接口和一个空的xml文件

* 插件后续可能会增加更多的自动生成代码。

* 有关插件的使用，后续完善文档

Maven坐标:

```xml
<dependency>
    <groupId>com.github.abel533</groupId>
    <artifactId>mapper</artifactId>
    <version>1.0.0</version>
</dependency>
```

##v0.3.2版本说明

移除了`MapperInterceptor`类，不在提供拦截器方式的使用。如果有需要可以自己从0.3.1版本获取。
 
另外调整的类的包结构，目的更明确，下面分别介绍这几个类的作用。  

* `com.github.abel533.mapperhelper`包下面的是通用Mapper的关键类

   * `EntityHelper`:实体类工具类 - 处理实体和数据库表以及字段关键的一个类  

   * `MapperHelper`:处理主要逻辑，最关键的一个类  

   * `MapperTemplate`:通用Mapper模板类，扩展通用Mapper时需要继承该类  

* `com.github.abel533.mapper`包下面是通用Mapper自带的一个默认实现

   * `Mapper`:通用Mapper接口类   

   * `MapperProvider`:通用Mapper接口类对应的实现类 

如果你不需要通用Mapper自带的默认实现类`Mapper`，你就不需要`com.github.abel533.mapper`包下面的两个类，你可以根据文档和自己的需求自行扩展。  

##v0.3.1版本说明

支持Spring4泛型注入，详细请看文档[在Spring4中使用通用Mapper](http://git.oschina.net/free/Mapper/blob/master/UseMapperInSpring4.md) 

##v0.3.0版本说明

这个版本的主要目的是消除拦截器，因此针对常用的情况增加了两种更方便的使用方式。


##v0.2.0版本说明

该版本做了大量的重构，在原有基础上增加了两个类，分别为`MapperTemplate`和`MapperProvider`，其他几个类都有相当大的改动。  

**但是**，这次重构并不影响原有的业务代码。  

这次重构的目的是为了方便开发者自行扩展，增加自己需要的通用Mapper。这次重构后，扩展变的更容易。稍后会写一篇**如何进行扩展**的文档。  

这次更新还修复Oracle序列的BUG。