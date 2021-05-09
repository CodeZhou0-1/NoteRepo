# Mymall商城项目临时笔记（2）

### ElasticSearch

- 基本概念

![image-20210503193521579](http://img.codezhou.com/img/image-20210503193521579.png)

> 1 、 **Index （索引）**
> 动词，相当于 MySQL 中的 insert；
> 名词，相当于 MySQL 中的 Database
> 2 、 **Type （类型）**
> 在 Index（索引）中，可以定义一个或多个类型。
> 类似于 MySQL 中的 Table；每一种类型的数据放在一起；
> 3 、 **Document （文档）**
> 保存在某个索引（Index）下，某种类型（Type）的一个数据（Document），文档是 JSON 格
> 式的，Document 就像是 MySQL 中的某个 Table 里面的内容；

- 下载：https://www.elastic.co/cn/downloads/past-releases#elasticsearch（可以选择历史版本）
- 安装：https://www.cnblogs.com/hualess/p/11540477.html
- **但是最后kibana连接es的时候报错**：

```
License information from the X-Pack plugin could not be obtained from Elasticsearch for the [data] cluster
```

- 网上查了很多解决方案，大概都是说x-pack是安全防护相关的，然后需要配置改什么试了好多方法，还是一直失败
- 折腾了两个多小时，根据官方的一个问答区的帖子，换成了oss版本，也就是不含x-pack的kibana版本，此版本连接成功





### ik分词器下载安装和配置到es中





### Nignx安装和配置fenci.txt



### 项目使用es

- es的版本对应上，注意springboot的默认版本号也要改为对应版本

```xml
   <properties>
        <java.version>1.8</java.version>
        <elasticsearch.version>7.4.2</elasticsearch.version>
    </properties>
```

