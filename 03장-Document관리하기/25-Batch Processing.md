# Batch Processing

## Bulk API 소개하기

-   앞으로 어떻게 document를 갱신하고 수정하는지를 배우게 될 것
-   다수의 document가 어떻게 단일 쿼리문으로 작동하는지에 관해 살펴볼 것
    -   Bulk API
-   Bulk API는 NDJSON스펙의 형식을 따르는 값을 기대한다.
    ```
    action_and_metadata\n
    optional_source\n
    action_and_metadata\n
    optional_source\n
    ```

## 추가하기

```json
POST /_bulk
첫 번째 줄은 메타데이터를 선언하는 것이고
두 번째 줄은 document의 내용을 선언한다.
{ "index": { "_index": "products", "_id": 200 } }
{ "name": "Espresso Machine", "price": 199, "in_stock": 5 }
{ "create": {"_index": "products", "_id": 201} }
{ "name": "Mil Frother:", "price": 149, "in_stock": 14 }
```

```json
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "took" : 8,
  "errors" : false,
  "items" : [
    {
      "index" : {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "200",
        "_version" : 1,
        "result" : "created",
        "_shards" : {
          "total" : 3,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 3,
        "_primary_term" : 9,
        "status" : 201
      }
    },
    {
      "create" : {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "201",
        "_version" : 1,
        "result" : "created",
        "_shards" : {
          "total" : 3,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 4,
        "_primary_term" : 9,
        "status" : 201
      }
    }
  ]
}
```

## 수정, 삭제하기

```json
POST /products/_bulk
{ "update": { "_index": "products", "_id": 201 } }
{ "doc": { "price": 129 } }
DELETE는 두 번째 줄을 기대하지 않는다
{ "delete": { "_index": "products", "_id": 200 } }
```

```json
POST /products/_bulk
{ "update": { "_id": 201 } }
{ "doc": { "price": 129 } }
{ "delete": { "_id": 200 } }
```

```json
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "took" : 9,
  "errors" : false,
  "items" : [
    {
      "update" : {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "201",
        "_version" : 2,
        "result" : "updated",
        "_shards" : {
          "total" : 3,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 5,
        "_primary_term" : 9,
        "status" : 200
      }
    },
    {
      "delete" : {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "200",
        "_version" : 2,
        "result" : "deleted",
        "_shards" : {
          "total" : 3,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 6,
        "_primary_term" : 9,
        "status" : 200
      }
    }
  ]
}
```

## 주의사항

-   HTTP Content-Type headers는 다음을 따라야 한다.
    -   Content-Type: application/x-ndjson
    -   `application/json`로도 가능하나 올바르게 동작하지 않을 수도 있음
-   콘솔 도구가 이 작업을 처리한다.
    -   Elasticsearch SDK는 이것들을 처리한다.
    -   HTTP 클라이언트를 사용함으로써 다룰 수 있음
-   매 줄마다 `\n`나 `\r\n`로 개행을 `반드시` 작성
    -   마지막 줄을 포함한다
        -   텍스트 편집기에서 마지막 줄은 비어 있어야 한다 (실수하는 상황)
    -   콘솔 툴에서는 자동으로 처리
    -   일반적으로 스크립트에서 대량 파일을 생성하며, 이 경우 이를 처리해야 한다.
    -   Text editor에 \n나 \r\n을 치지 말아라? (\n \r\n을 직접 치지 말고 엔터를 쳐서 개행하란 의미인가?)
-   실패한 action은 다른 action에 영향을 미치지 않는다.
    -   전체적으로 대량 요청이 중단되는 것도 아니다.
-   Bulk API는 자세한 정보를 각 액션별로 반환해준다.
    -   요청한 것 순서와 반환 받는 순서가 같다고 생각하면 됨.
    -   에러 키는 에러가 발생했는지 쉽도록 알려준다.

## 언제 Bulk API를 사용해야 하는가?

-   동시에 많은 쓰기 작업을 수행해야 하는 경우
    -   가져올 데이터나 수정해야 할 데이터가 많은 경우
-   Bulk API는 쓰기 연산 요청을 따로 보내는 것보다 효율적이다.
    -   네트워크에서 티키타카를 피할 수 있다.
-   라우팅은 Document의 샤드를 위해 사용된다.
    -   라우팅은 필요할 경우 커스터마이징할 수 있다.
-   Bulk API는 낙관적 동시성 제어(Optimistic concurrency control)를 지원한다.
    -   액션 메타데이터에 있는 `if_primary_term`과 `if_seq_no` 파라미터의 포함한다
