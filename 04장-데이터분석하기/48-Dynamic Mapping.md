# Introduction to dynamic mapping

-   이는 기본적으로 문서를 인덱싱하기 전에 명시적 필드 매핑을 정의하지 않아도 되어 Elasticsearch를 더 쉽게 사용할 수 있도록 하는 방법이다.

```json
// POST /my-index/\_doc
{
    "tags": ["computer", "electronics"],
    "in_stock": 4,
    "created_at": "2020/01/01 00:00:00"
}
// 결과 GET /my-index/_mapping
{
  "my-index" : {
    "mappings" : {
      "properties" : {
        "created_at" : {
          "type" : "date",
          "format" : "yyyy/MM/dd HH:mm:ss||yyyy/MM/dd||epoch_millis"
        },
        "in_stock" : {
          "type" : "long"
        },
        "tags" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        }
      }
    }
  }
}
```

-   Dynamic mapping 방식은 자동으로 필드를 만들어내는 것이다.
-   데이터 형식도 자동으로 감지
-   숫자의 경우 크기의 한도를 알 수 없으므로 `long`으로 지정

## JSON -> Elasticsearch

1. string
    - keyword 매핑과 함께 text
    - date
    - float or long
2. integer
    - long
3. floating point number
    - float
4. boolean (true or false)
    - boolean
5. object
    - object
6. array
    - 처음 non-null value 값에 의존

```json
GET /products/_mapping
{
  "products" : {
    "mappings" : {
      "properties" : {
        "name" : {
          "type" : "text"
        },
        "reviews" : {
          "type" : "nested"
        }
      }
    }
  }
}
```

## 또 다른 예제

-   동적, 정적 매핑 두 가지 방식을 모두 활용할 수 있는 예제
-   꼭 동적이나 정적 하나만을 선택해야 하는 방식은 아님

```json
PUT /people
{
  "mappings": {
    "properties": {
      "first_name": {
        "type": "text"
      }
    }
  }
}
```

```json
POST /people/_doc
{
  "first_name": "Bo",
  "last_name": "Andersen"
}
// 결과
{
  "_index" : "people",
  "_type" : "_doc",
  "_id" : "J9Q1W4ABeHp6sT-Cdp7N",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 1,
  "_primary_term" : 1
}

```

```json
GET /people/_mapping
{
  "people" : {
    "mappings" : {
      "properties" : {
        "first_name" : {
          "type" : "text"
        },
        "last_name" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        }
      }
    }
  }
}
```

# Clearn up

```
DELETE /people
```
