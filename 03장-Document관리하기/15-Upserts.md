# Upserts

-   Upsert는 상황에 따라 document가 존재하는 경우에는 update를 하고 존재하지 않으면 insert 연산을 한다.
-   아래의 쿼리에서 이미 존재하면 스크립트를 실행하고, 존재하지 않으면 새로운 document를 생성한다.

```json
POST /products/_update/101
{
    "script": {
        "source": "ctx._source.in_stock++"
    },
    "upsert": {
        "name": "Blender",
        "price": 399,
        "in_stock": 5
    }
}

GET /products/_doc/101
```

## 처음 실행했을 때의 결괏값

```json
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "_index" : "products",
  "_type" : "_doc",
  "_id" : "101",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 3,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 7,
  "_primary_term" : 4
}
```

```json
{
    "_index": "products",
    "_type": "_doc",
    "_id": "101",
    "_version": 2,
    "_seq_no": 8,
    "_primary_term": 4,
    "found": true,
    "_source": {
        "name": "Blender",
        "price": 399,
        "in_stock": 5
    }
}
```

## 두 번째 실행했을 때의 결괏값

```json
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "_index" : "products",
  "_type" : "_doc",
  "_id" : "101",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 3,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 8,
  "_primary_term" : 4
}
```

```json
{
    "_index": "products",
    "_type": "_doc",
    "_id": "101",
    "_version": 2,
    "_seq_no": 8,
    "_primary_term": 4,
    "found": true,
    "_source": {
        "name": "Blender",
        "price": 399,
        "in_stock": 6
    }
}
```
