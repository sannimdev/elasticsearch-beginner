# Defining field aliases

## Field Aliases 소개

-   필드 이름은 재색인할 때 바뀔 수 있다
    -   아마도 많은 document에 대해 쓸모가 없을 것이다 (효용성이 없을 것)
-   그래서 대안으로 필드 별칭을 사용한다.
    -   documents의 재색인을 요구하지 않는다
    -   comment에서 content를 가리키는 하나를 추가해 보자
    -   별칭은 쿼리에서 사용될 수 있다
    -   별칭은 필드 매핑 시 정의된다

## 실습

```json
PUT /reviews/_mapping
{
  "properties": {
    "comment": {
      "type": "alias",
      "path": "content"
    }
  }
}

#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "acknowledged" : true
}

```

```json
GET /reviews/\_search
{
    "query": {
        "match": {
            "content": "outstanding"
        }
    }
}
```

```json
GET /reviews/\_search
{
    "query": {
        "match": {
            "comment": "outstanding"
        }
    }
}
```

-   다음과 같이 같은 결과가 나타난다.

```json
{
    "took": 2,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 1,
            "relation": "eq"
        },
        "max_score": 0.9116392,
        "hits": [
            {
                "_index": "reviews",
                "_type": "_doc",
                "_id": "1",
                "_score": 0.9116392,
                "_source": {
                    "rating": 5.0,
                    "content": "Outstanding course! Bo really taught me a lot about Elasticsearch!",
                    "product_id": 123,
                    "author": {
                        "first_name": "John",
                        "last_name": "Doe",
                        "email": "johndoe123@example.com"
                    }
                }
            }
        ]
    }
}
```

## Updating field aliases

-   필드 별칭은 변경될 수 있다
    -   대상 필드만
-   `"path"` 값으로 매핑 업데이트를 수행하기만 하면 된다
-   인덱싱하는 데 영향을 미치지 않음

    -   쿼리 수준의 레벨로 구성된다.

### POST 쿼리

-   번역되기 전
    ```json
    POST /reviews/_doc
    {
        "rating": 5.0,
        "comment: "Outstanding course!"
    }
    ```
-   번역된 후
    ```json
    POST /reviews/_doc
    {
        "rating": 5.0,
        "content: "Outstanding course!"
    }
    ```

### 검색(GET) 쿼리

-   번역되기 전
    ```json
    GET /reviews/_search
    {
        "query": {
            "match": {
                "comment": "outstanding"
            }
        }
    }
    ```
-   번영된 후
    ```json
    GET /reviews/_search
    {
        "query": {
            "match": {
                "content": "outstanding"
            }
        }
    }
    ```

## Index aliases

-   필드 별칭과 유사한데 Elasticsearch에서는 인덱스 별칭도 지원한다
-   일반적으로 큰 데이터 볼륨을 다룰 때 사용한다.
-   지금은 다루지 않을 예정
