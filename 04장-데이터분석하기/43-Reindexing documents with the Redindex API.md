# Reindexing documents with the Reindex API

## GET /reviews/\_mappings

```json
{
    "reviews": {
        "mappings": {
            "properties": {
                "author": {
                    "properties": {
                        "email": {
                            "type": "keyword"
                        },
                        "first_name": {
                            "type": "text"
                        },
                        "last_name": {
                            "type": "text"
                        }
                    }
                },
                "content": {
                    "type": "text"
                },
                "created_at": {
                    "type": "date"
                },
                "product_id": {
                    "type": "integer"
                },
                "rating": {
                    "type": "float"
                }
            }
        }
    }
}
```

## PUT /reviews_new

```json
PUT /reviews_new
{
 "mappings" : {
      "properties" : {
        "author" : {
          "properties" : {
            "email" : {
              "type" : "keyword"
            },
            "first_name" : {
              "type" : "text"
            },
            "last_name" : {
              "type" : "text"
            }
          }
        },
        "content" : {
          "type" : "text"
        },
        "created_at" : {
          "type" : "date"
        },
        "product_id" : {
          "type" : "keyword"
        },
        "rating" : {
          "type" : "float"
        }
      }
    }
}

```

-   결과

```json
{
    "acknowledged": true,
    "shards_acknowledged": true,
    "index": "reviews_new"
}
```

-   재색인 시에는 많은 문서를 재색인해야 하므로 상당한 작업이 필요하다
-   대신, Elasticsearch에서는 목적에 특화된 편리한 API인 Reindex가 있다.
    -   비즈니스 요구 사항이 변경되거나 확장되어야 할 때 데이터가 저장되는 방식이 변경될 숭 ㅣㅆ는데 이때 유용하다
    -   HTTP Method는 POST이고 끝은 단순히 "\_reindex"이다.
    -   "source", "dest" 두 가지 매개변수가 필요

```json
POST /_reindex
{
  "source":{
    "index": "reviews"
  },
  "dest":{
    "index": "reviews_new"
  }
}
```

```json
{
    "took": 21,
    "timed_out": false,
    "total": 5,
    "updated": 0,
    "created": 5,
    "deleted": 0,
    "batches": 1,
    "version_conflicts": 0,
    "noops": 0,
    "retries": {
        "bulk": 0,
        "search": 0
    },
    "throttled_millis": 0,
    "requests_per_second": -1.0,
    "throttled_until_millis": 0,
    "failures": []
}
```

-   5개의 documents가 재색인되었다.

### \_source 데이터 타입

-   데이터 유형은 값이 인덱싱되는 방식을 반영하지 않습니다.
-   \_source 객체에는 인덱스 시간에 제공된 원래 값이 포함되어 있다
-   검색 결과에서 \_source 값을 사용하는 것이 일반적이다
    -   아마도 키워드 필드에 대한 문자열을 예상할 것
-   재색인 시 `_source` 값을 수정할 수 있다
-   또는 응용 프로그램 수준에서 처리할 수 있습니다.

```js
POST /reviews_new/_delete_by_query
{
  "query": {
    "match_all": {}
  }
}

POST /_reindex
{
  "source": {
    "index": "reviews"
  },
  "dest": {
    "index": "reviews_new"
  },
  "script": {
    "source": """
      if (ctx._source.product_id != null) {
        ctx._source.product_id = ctx._source.product_id.toString();
      }
    """
  }
}

GET /reviews_new/_search
{
    "query": {
        "match_all":{}
    }
}
```

## 필드 지우기

-   필드가 매핑된 것은 삭제될 수 없다
-   문서를 인덱싱할 때 필드를 생략할 수 있다
-   필드에서 사용하는 디스크 공간을 회수하고 싶을 수도 있다
    -   이미 인덱싱된 값은 공간을 여전히 차지한다
    -   큰 데이터 세트의 경우 이는 상당히 의미가 있을 수 있다
        -   더이상 필요 없는 데이터의 경우

## ctx.op 를 스크립트와 사용하는 것

-   일반적으로 query 파라미터는 가능하다
-   고급 사용 사례의 경우 ctx.op을 사용할 수 있음
-   query 파라미터를 사용하는 것은 더 나은 성능을 보이며 이 방식이 선호된다.
-   "delete"를 명시하는 것은 명시한 document를 삭제한다
    -   대상 인덱스는 이 예에서와 같이 비어 있지 않을 수 있음
    -   쿼리별 삭제 API에서도 동일한 작업을 수행할 수 있다

## Parameters for the Reindex API

-   강의에서 다뤘던 것보다 더 맣ㄴ은 파라미터를 사용할 수 있음
    -   버전 충돌 다루기 등
-   스냅숏이 document가 재색인되기 전에 만들어진다
-   버전 충돌은 기본적으로 쿼리를 중단한다
-   대상 인덱스는 꼭 비어있지는 않다

## Batching & Throttling

-   Reindex API는 배치에서 동작한다
    -   Update by Query와 Delete by Query API와 비슷
    -   Scroll API를 내부적으로 사용한다
    -   많은 documents가 효율적으로 재색인될 수 있음
-   성능 영향을 제한하도록 설정할 수 있다.
    -   많은 production 클러스터에서 유용함
-   많은 문서를 다시 색인화해야 하는 경우 문서를 확인하기
