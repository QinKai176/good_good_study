## ElasticSearch的嵌套查询

### 1.嵌套文档 

```
PUT movies
{
  "mappings": {
    "properties": {
      "actors":{
        "type": "nested"
      }
    }
  }
}

PUT movies/_doc/1
{
  "name":"霸王别姬",
  "actors":[
    {
      "name":"张国荣",
      "gender":"男"
    },
    {
      "name":"巩俐",
      "gender":"女"
    }
    ]
}
```

 nested 文档在 es 内部其实也是独立的 lucene 文档，只是在我们查询的时候，es 内部帮我们做了 join 处理，所以最终看起来就像一个独立文档一样。因此这种方案性能并不是特别好。

### 2.父子关系

效率太低，跳过。