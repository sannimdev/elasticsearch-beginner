# Overview mapping parameters

## 개요

-   가장 중요한 파라미터를 매핑하는 것을 다룰 것이다
-   완벽하게 다루는 것은 아님
    -   대부분의 다른 파라미터는 매우 전문화되었지만, 자주 사용되는 것은 아니다.

## format 파라미터

-   date 필드를 위해 customize하여 사용된다.
-   가능한 한 기본 형식을 사용할 것을 권장한다
    -   "strict_date_optional_time || epoch_millis"
-   Java의 DateFormatter 문법을 사용한다
    -   E.g. "dd/MM/yyyy"
-   기본적으로 제공하는(built-in) 형식을 사용한다
    -   E.g. "epoch_second"

## properties parameter

-   object와 nested 필드를 정의한 것이다

```json
PUT /sales
{
    "mappings": {
        "properties": {
            "sold_by": {
                "properties": {
                    "name": { "type": "text" }
                }
            }
        }
    }
}
```

```json
PUT /sales
{
    "mappings": {
        "properties": {
            "sold_by": {
                "properties": {
                    "type" :"nested",-
                    "name": { "type": "text" }
                }
            }
        }
    }
}
```

## coerce parameter

-   coercion 값 (기본값: enabled)
    -   (강제 형변환)

```json
PUT /sales
{
    "mappings": {
        "properties": {
            "amount": {
                "type": "float",
                "coerce": false
            }
        }
    }
}
```

```json
PUT /sales
{
    "settings": {
        "index.mapping.coerce": false
    },
    "mappings": {
        "properties": {
            "amount": {
                "type": "float",
                "coerce": true
            }
        }
    }
}
```

## doc_values 소개

-   Elasticsearchs는 여러 데이터 구조를 사용한다
    -   모든 용도로 사용할 수 있는 단일 데이터 구조는 없다
-   Inverted indices는 텍스트를 검색하기에 매우 좋다
    -   다른 많은 데이터 액세스 패턴에서는 성능이 좋지 않다.
-   `Doc values`는 Apache Lucene에서 사용하는 또 다른 데이터 구조이다
    -   다른 데이터 접근 패턴(document -> terms)에 최적화되어 있음
-   기본적으로 반전되지 않은 inverted index이다.
-   정렬, 집합(aggregations), 스크립팅을 사용한다
-   추가적은 데이터 구조이며 대체하는 것이 아니다.
-   Elasticsearch는 자동으로 적절한 자료구조를 쿼리로 요청한다

### doc_values 비활성화하기

-   doc_values 파라미터를 false로 disk 공간에 저장한다.
    -   저장공간을 절약하고자 할 때 사용
    -   경미하게 인덱싱 처리량이 증가
-   aggregations(집계), 정렬, 스크립팅을 사용하지 않을 경우에만 비활성화
-   특히 큰 인덱스에 유용하며, 일반적으로 작은 인덱스에 적합하지 않다.
-   document를 새로운 인덱스로 재색인하지 않고서는 변경할 수 없다.
    -   조심해서 사용해야 하며, 필드가 어떻게 쿼리가 요청될 것인지를 예측해야 한다.

## norms 파라미터

-   관련성 채점에 사용되는 정규화 요인
-   종종 결과를 필터링할 뿐만 아니라 순위도 매기고 싶을 수 있음
-   Norms는 디스크 공간을 절약하기 위해 비활성화할 수 있음.
    -   관련성 채점에 사용되지 않는 필드에 유용
    -   ex: tags 필드

```json
PUT /products
{
    "mappings": {
        "properties": {
            "tags": {
                "type": "text",
                "norms": false
            }
        }
    }
}
```

## index 파라미터

-   필드에 대한 인덱싱을 사용하지 않음
-   값은 여전히 `_source` 에 저장된다
-   검색 쿼리를 사용하지 않는 필드의 경우에는 유용
-   디스크 공간을 절약하거나 색인 산출량을 경미하게 개선
-   종종 time series 데이터에 사용된다
-   색인이 비활성화된 필드는 여전히 집계할 때 사용될 수 있음.

```json
PUT /server-metrics
{
    "mappings": {
        "properties": {
            "server_id": {
                "type": "integer",
                "index": false
            }
        }
    }
}
```

## null_value 파라미터

-   `NULL` 값은 검색되거나 인덱싱될 수 없음
-   이 매개 변수를 사용하여 NULL 값을 다른 값으로 바꾼다
-   예외적으로 NULL 값에 관해서만 동작한다
-   대체한 값은 필드의 같은 데이터 타입이어야만 한다.
-   \_source에는 영향을 미치지 않는다.

```json
PUT /sales
{
    "mappings": {
        "properties": {
            "partner_id": {
                "type": "keyword",
                "null_value": "NULL"
            }
        }
    }
}
```

## copy_to 파라미터

-   여러 필드 값을 "그룹 필드"에 복사하는 데 사용된다
-   대상 필드의 이름을 값으로 지정
-   first_name and last_name -> full_name
-   values는 복사되며 temrs/tokens가 되는 것이 아님 (Values are copied, not terms/tokens)
    -   해당 필드의 analyzer가 값에 사용된다
-   해당 필드는 `_source`의 일부분이 아니다

```json
PUT /sales
{
    "mappings": {
        "properties": {
            "fist_name": {
                "type": "text",
                "copy_to": "full_name"
            },
            "last_name": {
                "type": "text",
                "copy_to": "full_name"
            },
            "full_name": {
                "type": "text"
            }
        }
    }
}
```

```json
POST /sales/_doc
{
    "first_name": "John",
    "last_name": "Doe"
}
```
