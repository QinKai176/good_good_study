## ElasticSearch

### 1.elasticsearch的增删改查

```
PUT blog/_doc/1
{
  "title":"elasticsearch的基本操作",
  "date":"2020-11-26",
  "content":"关于增删改查"
}

PUT blog/_doc/2
{
  "title":"elasticsearch的分词器",
  "date":"2020-11-26",
  "content":"关于分词器"
}

PUT blog/_doc/1
{
  "title":"elasticsearch的基本操作",
  "date":"2020-11-26",
  "content":"关于增删改查"
}

POST blog/_doc
{
  "title":"elasticsearch的基本操作",
  "date":"2020-11-26",
  "content":"关于增删改查"
}

GET blog/_doc/2

POST blog/_mget
{
  "ids":["1","22"]
}

POST blog/_update/1
{
  "script":{
    "lang":"painless",
    "source":"ctx._source.title=params.title",
    "params":{
      "title":666
    }
  }
}

POST blog/_update/1
{
  "script":{
    "lang":"painless",
    "source":"ctx._source.tags=[\"java\",\"php\"]"
  }
}


POST blog/_update/1
{
  "script":{
    "lang":"painless",
    "source":"ctx._source.tags.add(\"java1\")"
  }
}

POST blog/_update/1
{
  "script":{
    "lang":"painless",
    "source":"if (ctx._source.tags.contains(\"java\")){ctx.op=\"delete\"}else{ctx.op=\"none\"}"
  }
}

POST blog/_update_by_query
{
  "script":{
    "lang":"painless",
    "source":"ctx._source.content=\"888\""
  },
  "query":{
    "term":{
      "title":"666"
    }
  }
}

DELETE blog/_doc/xb6qBHYBZ5uUU1PAZm0y
{
  "script":{
    "lang":"painless",
    "source":"ctx._source.content=\"888\""
  },
  "query":{
    "term":{
      "title":"666"
    }
  }
}


POST blog/_delete_by_query
{
  "query":{
   "match_all":{
     
   }
  }
}

PUT blog/_doc/a
{
  "title":"a"
}

GET _cat/shards/blog?v

GET blog/_search



```



