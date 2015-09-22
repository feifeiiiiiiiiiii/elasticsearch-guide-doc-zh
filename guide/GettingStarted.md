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
   
       <img src="images/elas_0201.png">
       
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
       
       <img src="images/elas_0202.png">
       
   
   3). 增加一个节点,集群状态
       
       <img src="images/elas_0203.png">
       
   4). 再增加一个节点,集群状态
       <img src="images/elas_0204.png">

       修改number_of_replicas为2,集群状态变为:
       <img src="images/elas_0205.png">
       
       通过上面的过程，可以知道，ES可以横向扩展
       
   5). 关闭一个节点,集群状态
       <img src="images/elas_0206.png">
       
       发现ES可以支持自动故障转移，并且重新选举了Primary shard,不会影响服务
 
 3. 分布式文档存储
   
   1). Routing a Document to a Shard
       当索引一个文档，它被存储在一个Primary分片上。ES怎么知道文档具体属于哪个Shard上? 创建一个文档，怎样知道该文档应该存储在Shard1还是Shard2上，所有的这一切都不是随机的，毕竟还有获取这些文档，所有的这一切可以被一个简单的公式决定:
       shard = hash(routing) % number_of_primary_shards (?这也解释了索引shards数量一旦创建就不能被修改)
       
       tips: **routing** 值是经过hash函数生成的数值
       
       所以对文档操作的api(get,index,delete,bulk,update,mget)均支持**routing**参数,这样做的好处就是相同**routing**都存在一个shard上，检索快 (留个疑问为什么快?)

   2). Primary and Replica Shards Interact
       
       a. 考虑三个节点的集群有一个索引如下:
           <img src="images/elas_0401.png">
       
       b. 创建、索引、删除文档
           <img src="images/elas_0402.png">
           
           tips: 
               1. 客户端首先发送create、index、delete请求到节点Node 1上
               2. 节点根据Document`s _id 确定文档属于 shard 0, 此时该节点转发请求到shard 0对应primary所在的 Node 3上
               3. Node 3在Primary上执行请求,如果执行成功，该Node会并行的将执行操作的请求转发到Primary对应的Replica上，
                  所以的Replica node执行成功，Node 3会报告成功指令,保证数据在各个shard上的一致性
               ![具体看官方文档](https://www.elastic.co/guide/en/elasticsearch/guide/current/distrib-write.html#img-distrib-write)
               
        c. 获取文档的过程
           <img src="images/elas_0403.png">
        
        d. 部分更新文档
           <img src="images/elas_0404.png">
</pre>

