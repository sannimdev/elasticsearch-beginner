# Updating Document

# 1. Document는 불변성을 갖는다.

-   Elasticsearch는 불변성을 가진다.
-   실제로는 이 documents를 대체하는 것이다. (수정이 아니고)
-   Update API는 간단하고 네트워크 트래픽을 줄인다.

## Update 절차

1. 해당 document를 조회
2. 필드를 변경
3. 존재하는 doucment를 수정된 document로 대체

# 2. 실습

```json
POST /products/_update/100
{
  "doc": {
    "in_stock": 3
  }
}

#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "_index" : "products",
  "_type" : "_doc",
  "_id" : "100",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 3,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 1,
  "_primary_term" : 2
}

```

```json
GET /products/_doc/100

#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "_index" : "products",
  "_type" : "_doc",
  "_id" : "100",
  "_version" : 2,
  "_seq_no" : 1,
  "_primary_term" : 2,
  "found" : true,
  "_source" : {
    "name" : "Toaster",
    "price" : 49,
    "in_stock" : 3
  }
}
```

```json
POST /products/_update/100
{
  "doc": {
    "tags": ["electronics"]
  }
}
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "_index" : "products",
  "_type" : "_doc",
  "_id" : "100",
  "_version" : 3,
  "result" : "noop",
  "_shards" : {
    "total" : 0,
    "successful" : 0,
    "failed" : 0
  },
  "_seq_no" : 2,
  "_primary_term" : 2
}
```

```json
GET /products/_doc/100

#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "_index" : "products",
  "_type" : "_doc",
  "_id" : "100",
  "_version" : 2,
  "_seq_no" : 1,
  "_primary_term" : 2,
  "found" : true,
  "_source" : {
    "name" : "Toaster",
    "price" : 49,
    "in_stock" : 3
  }
}

```
