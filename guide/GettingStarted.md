# Getting Started

Elasticsearch 简介

<pre>
1. 基于Lucene的一个开源的搜索引擎，Lucene是一个全文检索的java库，由于lucene的复杂，ES让全文检索变得简单、提供RESSTful API。
2. 分布式实时文档存储，任意字段均可被索引和搜索
3. 支持分布式实时统计
4. 支持横向扩展，可以扩容到上百台规模集群、PB级别数据
5. 使用JSON作为文档序列存储结构
</pre>

Elasticsearch 知识

<pre>
1. 和关系型数据库的对比
   Relational DB => Databases => Tables => Rows      => Colums
   Elasticsearch => Indices   => Types  => Documents => Fields
   
   和关系型数据库一样，一个ES集群可以容纳多个索引(Indices)，一个索引可以有很多类型(Types),类型下可以有很多文档(Doc)，一个文档可以有很多字段(Field)

2. ES集群

   1). 空集群 
       ![当前节点](http://ww4.sinaimg.cn/mw690/9147fb05gw1ewaia7w9e9j20ku069aa5.jpg)
       
       可以通过/_cluster/health查看集群状态
   
   2). 增加Indices
   
       ```
        PUT /blogs
        {
          "settings" : {
            "number_of_shards" : 3,
            "number_of_replicas" : 1
          }
        }
       ``` 
       当前集群状态为
       ![](http://ww1.sinaimg.cn/mw690/9147fb05gw1ewaifxqto9j20ku069q35.jpg)
       
   
   3). 增加一个节点,集群状态
       
       ![](http://ww3.sinaimg.cn/mw690/9147fb05gw1ewaii5lkkpj20ku069t92.jpg)
       
   4). 再增加一个节点,集群状态
       ![](http://ww4.sinaimg.cn/mw690/9147fb05gw1ewaikgphmoj20ku069q3i.jpg)

       修改number_of_replicas为2,集群状态变为:
       ![](http://ww4.sinaimg.cn/mw690/9147fb05jw1ewaim8r9kxj20ku0693z3.jpg)
       
       通过上面的过程，可以知道，ES可以横向扩展
       
   5). 关闭一个节点,集群状态
       ![](http://ww1.sinaimg.cn/mw690/9147fb05jw1ewaio9uwrpj20ku069q3c.jpg)
       
       发现ES可以支持自动故障转移，并且重新选举了Primary shard,不会影响服务
       
</pre>
