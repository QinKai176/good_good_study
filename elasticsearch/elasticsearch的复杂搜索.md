## ElasticSearch的复合搜索

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

### 3.复合查询

#### 3.1.constant_score查询

当我们不关心检索词项的频率（TF）对搜索结果排序的影响时，可以使用 constant_score 将查询语句或者过滤语句包裹起来。

```
GET books/_search
{
 "query": {
  "constant_score": {
   "filter": {
    "term": {
     "name": "java"
    }
   },
   "boost": 1.5
  }
 }
}
```

#### 3.2 bool查询

bool query 可以将任意多个简单查询组装在一起，有四个关键字可供选择，四个关键字所描述的条件可以有一个或者多个。

- must：文档必须匹配 must 选项下的查询条件。
- should：文档可以匹配 should 下的查询条件，也可以不匹配。
- must_not：文档必须不满足 must_not 选项下的查询条件。
- filter：类似于 must，但是 filter 不评分，只是过滤数据

例如查询 name 属性中必须包含 java，同时书价不在 [0,35] 区间内，info 属性可以包含 程序设计 也可以不包含程序设计：

```
GET books/_search
{
 "query": {
  "bool": {
   "must": [
    {
     "term": {
      "name": {
       "value": "java"
      }
     }
    }
   ],
   "must_not": [
    {
     "range": {
      "price": {
       "gte": 0,
       "lte": 35
      }
     }
    }
   ],
   "should": [
    {
     "match": {
      "info": "程序设计"
     }
    }
   ]
  }
 }
}
```

这里还涉及到一个关键字，`minmum_should_match` 参数。

`minmum_should_match` 参数在 es 官网上称作最小匹配度。在之前学习的 `multi_match` 或者这里的 should 查询中，都可以设置 `minmum_should_match` 参数。

```
GET books/_search
{
 "query": {
  "bool": {
   "should": [
    {
     "term": {
      "name": {
       "value": "语言"
      }
     }
    },
    {
     "term": {
      "name": {
       "value": "程序设计"
      }
     }
    },
    {
     "term": {
      "name": {
       "value": "程序"
      }
     }
    },
    {
     "term": {
      "name": {
       "value": "设计"
      }
     }
    }
   ],
   "minimum_should_match": "50%"
  }
 }
}
```

#### 3.3 dis_max 查询

```
PUT blog
{
 "mappings": {
  "properties": {
   "title":{
    "type": "text",
    "analyzer": "ik_max_word"
   },
   "content":{
    "type": "text",
    "analyzer": "ik_max_word"
   }
  }
 }
}

POST blog/_doc
{
 "title":"如何通过Java代码调用ElasticSearch",
 "content":"松哥力荐，这是一篇很好的解决方案"
}

POST blog/_doc
{
 "title":"初识 MongoDB",
 "content":"简单介绍一下 MongoDB，以及如何通过 Java 调用 MongoDB，MongoDB 是一个不错 NoSQL 解决方案"
}
```

现在假设搜索 **Java解决方案** 关键字，但是不确定关键字是在 title 还是在 content，所以两者都搜索：

```
GET blog/_search
{
 "query": {
  "bool": {
   "should": [
    {
     "match": {
      "title": "java解决方案"
     }
    },
    {
     "match": {
      "content": "java解决方案"
     }
    }
   ]
  }
 }
}
```

我们需要来看下 should query 中的评分策略：

1. 首先会执行 should 中的两个查询
2. 对两个查询结果的评分求和
3. 对求和结果乘以匹配语句总数
4. 在对第三步的结果除以所有语句总数

在这种查询中，title 和 content 相当于是相互竞争的关系，所以我们需要找到一个最佳匹配字段。

为了解决这一问题，就需要用到 dis_max query（disjunction max query，分离最大化查询）：匹配的文档依然返回，但是只将最佳匹配的评分作为查询的评分。

在 dis_max query 中，还有一个参数 `tie_breaker`（取值在0～1），在 dis_max query 中，是完全不考虑其他 query 的分数，只是将最佳匹配的字段的评分返回。但是，有的时候，我们又不得不考虑一下其他 query 的分数，此时，可以通过 `tie_breaker` 来优化 dis_max query。`tie_breaker` 会将其他 query 的分数，乘以 `tie_breaker`，然后和分数最高的 query 进行一个综合计算。

#### 3.4 function_score 查询