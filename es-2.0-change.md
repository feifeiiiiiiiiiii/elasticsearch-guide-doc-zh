ES 2.0 部分功能 https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html
### 移除的功能
	1. Rivers 被移除 (ps: 没用过)
	2. Facets 被移除 (ps: 看es的master那个本书学习到的，目前统计全被aggs替换,幸亏最一开始就是用的aggs)
	3. MVEL 被移除 (ps: 没用过)
	4. Delete-by-query 变成了一个插件 (2.0之前可以使用查询删除一批文档，官方给出移除的原因是: 通过该功能查询删除虽然快但是不安全,
	   导致文档在Primary和replica不一致,还有可能出现内存异常引起集群crash,如果不想用这个却想实现类似的功能，用scroll/scan、bulk实现，虽然慢但是安全）
	5. 组播(multicast) 发现变成插件 (ps: 不懂,^_^)
	6. _shutdown API (该api被移除, 没有替换品，每个node节点增加了start/stop脚本启动节点)
	7. _size 变成插件 (ps: 啥东西啊)
	8. Thrift 和 memcached transport (ps: 不在支持这两个个了,目前只存在HTTP、node、transport java client)
	9. Bulk UDP 被移除
	10. MergeScheduler pluggability (ps: 不知道啥东西)
	
### 网络相关的改变
	 默认绑定的端口是localhost, 当然可以指定具体的端口, 组播被移除 (ps: 自己去看官网，自己翻译吧，表示翻译不出来)
	 
### Multiple path.data striping
	Previously, if the path.data setting listed multiple data paths, then a shard would be “striped” across all paths by writing a whole file to each 
	path in turn (in accordance with the index.store.distributor setting). The result was that files from a single segment in a shard could be spread 
	across multiple disks, and the failure of any one disk could corrupt multiple shards.
	This striping is no longer supported. Instead, different shards may be allocated to different paths, 
	but all of the files in a single shard will be written to the same path.
	If striping is detected while starting Elasticsearch 2.0.0 or later, all of the files belonging to the same shard will be 
	migrated to the same path. If there is not enough disk space to complete this migration, the upgrade will be cancelled and can only be 
	resumed once enough disk space is made available. The index.store.distributor setting has also been removed.
	
### Mapping的变动
	mapping的最大改动就是更严格了
	1. 字段冲突, 同一个索引(index)下不同类型(type)如果有相同的Field,那么这两个Field的属性要一致,2.0之前是可以分别配置的
	2. 字段不能被用short name引用了,必须使用字段的full path.
	3. 类型名称前缀被移除了. 
		```
			GET my_index/_search (2.0之前) 
			{
				"query": {
					"match": {     						
						"my_type.some_field": "quick brown fox"
					}
				}
			}
			
			2.0
			GET my_index/my_type/_search
			{
				"query": {
					"match": {     						
						"some_field": "quick brown fox"
					}
				}
			}
		```
	4. mapping字段不能包含dot了
	5. type 的名称不能用dot作为开始
	6. type 名称最大长度不能超过255个字符
	7. type 不能被删除了,不能被删除了,不能被删除了 重要的事情不要说三遍
	8. type meta-fields(ps 太多了 去翻文档吧)
	9. _timestamp 和 _ttl 被弃用 换成其他了
	10.index_name 和 path 被移除 (啥东东）
	11. 还有好多...
	
### CRUD 和 Routing的变化
	1. 明确自定义的routing (自定义routing的值不在出现在文档body中，只能在查询或者bluk中明确显示
	2. Routing 哈希函数 变为 murmur3
	3. 使用自定义Routing删除api  2.0之前 删除的api 被广播到该index的所有shard上，现在只会发送到文档所在的shard上.
	   ```
	     mapping中定义routing
		 {
			"mappings": {
				"my_type": {
					"_routing": {
						"required": true
					}
				}
		  	}
		 }
	   ```
	4. All stored meta-fields returned by default
	5. Async replication
	6. Documents must be specified without a type wrapper
	7. Term Vectors API
	
### DSL 变化
	最大的变化就是filtered被弃用, or and and now implemented via bool, 自动缓存
	1. Queries and filters merged
	2. filtered query and query filter deprecated
	3. Filter auto-caching
	4. Numeric queries use IDF for scoring
	5. ...

### 搜索的变化
	1. search_type=count 被弃用
	2. The count api internally uses the search api 使用size=0替换
	3. 所有存储的meta-fields 默认被返回, 可以使用fields指定返回的。 fields的改变貌似和以前不一样了aha
	4. Timezone的改变 
	5. ...
	
### 聚合变化
	1. 最小的doc 数量默认为0 之前是1
	2. 日期格式变化
	3. 时区和偏移的变化
	4. Including/excluding terms
	5. Bool字段返回 0/1变为key true/false 变为string keys
	6. java agg的变化 没用 目前用的是rest api
	
### Parent/Child changes (貌似没用)
### Scripting changes (很少用)
### Index api 变化
### 其他
