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

#### 2.1 基本查询

所有的查询都写在query里面，默认查询10条记录

```
GET books/_search
{
  "query":{
    "match_all":{}
  }
}
```

#### 2.2词项查询

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

