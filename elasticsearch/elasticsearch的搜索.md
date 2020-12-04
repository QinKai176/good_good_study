## ElasticSearch的搜索

### 1.准备工作

#### 1.1.创建索引

```
PUT books
{
  "mappings": {
   "properties": {
     "name":{
       "type":"text",
       "analyzer": "ik_max_word"
     },
     "publish":{
       "type":"text",
        "analyzer": "ik_max_word"
     },
     "type":{
        "type":"text",
        "analyzer": "ik_max_word"
     },
     "author":{
        "type":"keyword"
      },
      "info":{
        "type":"text",
        "analyzer": "ik_max_word"
      },
      "price":{
        "type":"double"      
      }
   } 
  } 
}
```

#### 1.2导入数据

```
curl -XPOST "http://localhost:9200/books/_bulk?pretty" -H "content-type:application/json" --data-binary @bookdata.json
```

### 2.搜索

#### 2.1 match_all

所有的查询都写在query里面，默认查询10条记录

```
GET books/_search
{
  "query":{
    "match_all":{}
  }
}
```

#### 2.2 term

只能查询词语，不能全局查询

```
GET books/_search
{
  "query":{
    "term": {
        "name": "十一五"
      }
  }
}
```

#### 2.3.分页查询

```
GET books/_search
{
  "query":{
    "term": {
        "name": "十一五"
      }
  },
  "size": 20,
  "from": 10
}
```

#### 2.4.指定返回的字段

```
GET books/_search
{
  "query":{
    "term": {
        "name": "十一五"
      }
  },
  "size": 20,
  "from": 10,
  "_source": ["name","author"]
}
```

#### 2.5 高亮搜索

```
GET books/_search
{
  "query":{
    "term": {
        "name": "十一五"
      }
  },
  "size": 20,
  "from": 10,
  "highlight": {
    "fields": {"name":{}}
  }
}
```

#### 2.6 match

Match query先对查询语句进行分词，如果分词后的任何一个词项被索引到，就会被查到。

默认关系是or的关系，可以修改为and的关系；

```
GET books/_search
{
  "query":{
    "match": {
        "name": "美术计算机"
      }
  }
}

GET books/_search
{
  "query":{
    "match": {
        "name": {
        	"query":"美术计算机",
        	"operator":"and"
        }
      }
  }
}
```

#### 2.7 match_phrase

match_phrase query也会对查询的关键字进行分词，但是它分词后有两个特点：

a.分词是and的关系

b.分词的顺序一致

可以修改为and的关系；

```
GET books/_search
{
  "query":{
    "match_phrase": {
        "name": "十一五计算机",
        "slop":8
      }
  }
}
```

#### 2.8 match_phrase_prefix

类似match_phrase query，多了一个匹配符，效率低（了解即可）

```
GET books/_search
{
  "query":{
    "match_phrase_prefix": {
        "name": "计"
      }
  }
}
```

 max_expansion可以指定查询分词的数目,如果max_expansion为1，可能返回多个文档，但是只有一个词，这是我们预期的结果，有时候返回结果和我们预期结果不一致，因为这个查询是分片级别的，不同的分片确实只返回了一个词，但是结果可能来自不同的分片，所以看到了多个词。

```
GET books/_search
{
  "query":{
    "match_phrase_prefix": {
        "name": "计",
        "max_expansion":3
      }
  }
}
```

#### 2.9 multi_match

match的升级版本,^表示权重

```
GET books/_search
{
  "query":{
    "multi_match": {
        "query": "java",
        "field":["name^4","info"]
      }
  }
}
```

#### 2.10 query_string 

紧密结合Lucene的查询方式

```
GET books/_search
{
  "query":{
    "query_string": {
        "default_field": "name",
        "query":"(十一五) AND (计算机)"
      }
  }
}
```

#### 2.11 simple_query_string

紧密结合Lucene的查询方式,+和-代替AND,OR,NOT等

```
GET books/_search
{
  "query":{
    "simple_query_string": {
        "fields": ["name"],
        "query":"(十一五) + (计算机)"
      }
  }
}
```

