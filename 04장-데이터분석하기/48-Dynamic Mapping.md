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
