# ES相关笔记



## <a name='basic-and-algo'> 基本知识
* [ES官方文档](http://www.elastic.co/guide/en/elasticsearch/guide/current/index.html) : ES的相关介绍都在这里面


## <a name='essentials'> 查询优化

* [Warmer](http://www.elastic.co/guide/en/elasticsearch/reference/current/indices-warmers.html): 固定的条件进行预加载.

* [filtered用法和介绍](http://www.elastic.co/guide/en/elasticsearch/guide/current/fielddata-intro.html) :These cached bitsets are “smart”: they are updated incrementally. As you index new documents, only those new documents need to be added to the existing bitsets, rather than having to recompute the entire cached filter over and over. Filters are real-time like the rest of the system; you don’t need to worry about cache expiry.

## <a name='essentials'> 使用心得

## <a name='essentials'> 相关文档


 1. http://jontai.me/blog/2013/01/advanced-scoring-in-elasticsearch/
 2. http://stackoverflow.com/questions/21703909/elasticseach-query-optimization
 3. http://pan.baidu.com/s/1o62zcM2
 4. https://abhishek376.wordpress.com/2014/11/24/how-we-optimized-100-sec-elasticsearch-queries-to-be-under-a-sub-second/
 5. https://speakerdeck.com/elasticsearch/query-optimization-go-more-faster-better
 6. http://www.tuicool.com/articles/RN3qaq
 7. https://speakerd.s3.amazonaws.com/presentations/2930bf30608f01314eea660e5847431f/Query.Optimization.pdf
 8. https://speakerd.s3.amazonaws.com/presentations/b3c91d40657d01315c6632105fd2bdc9/Atlanta.Meetup.Query.Optimization.pdf
