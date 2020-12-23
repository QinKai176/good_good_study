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

### 2.简单搜索

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

只能查询词语，不能全局查询，会跟分词去比较，不会全局比较；

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

#### 2.12 terms

多个terms的查询

```
GET books/_search
{
  "query":{
    "terms": {
        "name": ["程序","java"]
      }
  }
}
```

#### 2.13 range

范围查询，可以按照日期，数字范围等查询；

range query的参数主要有四个：gt,lt,gte,lte

```
GET books/_search
{
  "query":{
    "range": {
        "price": {
        	"gte":10,
        	"lte":20
        }
      }
  },
  "sort":{
    "price":{
      "order":"desc"
    }
  }
}
```

#### 2.14  exists

返回指定字段至少有一个字段不为空值的文档，空字符串也为有值

```
GET books/_search
{
 "query": {
  "exists": {
   "field": "javaboy"
  }
 }
}
```

#### 2.15 prefix

前缀查询，效率低

```
GET books/_search
{
  "query": {
    "prefix": {
      "name": {
        "value": "大学"
      }
    }
  }
}
```

#### 2.16 wildcard

通配符查询，支持单字符和多字符通配符查询

？表示一个任意字符， *表示零个或者多个字符

```
GET books/_search
{
  "query": {
    "wildcard": {
      "author": {
        "value": "张?"
      }
    }
  }
}
```

#### 2.17  regexp

正则表达式查询

```
GET books/_search
{
 "query": {
  "regexp": {
   "author": "张."
  }
 }
}
```

#### 2.18 fuzzy query

在实际搜索中，有时我们可能会打错字，从而导致搜索不到，在 match query 中，可以通过 fuzziness 属性实现模糊查询。

fuzzy query 返回与搜索关键字相似的文档。怎么样就算相似？以LevenShtein 编辑距离为准。编辑距离是指将一个字符变为另一个字符所需要更改字符的次数，更改主要包括四种：

- 更改字符（javb--〉java）
- 删除字符（javva--〉java）
- 插入字符（jaa--〉java）
- 转置字符（ajva--〉java）

为了找到相似的词，模糊查询会在指定的编辑距离中创建搜索关键词的所有可能变化或者扩展的集合，然后进行搜索匹配。

```
GET books/_search
{
 "query": {
  "fuzzy": {
   "name": "jaav"
  }
 }
}
```

#### 2.19 ids查询

根据指定的 id 查询。

```
GET books/_search
{
  "query": {
    "ids":{
      "values":  [1,2111,3111]
    }
  }
}
```

