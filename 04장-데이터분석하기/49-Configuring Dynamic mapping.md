# Configuring dynamic mapping

-   "dynamic: false" 로 지정

```json
PUT /people
{
"mappings": {
    "dynamic": false,
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
```

```json
GET /people/_search
{
  "query" : {
    "match": {
      "first_name": "Bo"
    }
  }
}

GET /people/_search
{
  "query" : {
    "match": {
      "last_name": "Andersen"
    }
  }
}
```

## dynamic을 false로 설정하기

-   새로운 필드는 무시된다
    -   인덱싱되지 않으나 `_source`의 일부이기는 함.
-   last_name 필드의 inverted index는 생성되지 않음.
    -   쿼리에 대한 결괏값이 주어지지 않음
-   필드는 매핑없이 인덱싱되지 않는다
    -   활성화되면(enabled) 동적 매핑이 값을 인덱싱하기 전에 하나를 만든다
-   새 필드는 명시적으로 매핑되어야 한다

## 더 나은 방법

-   dynamic은 strict로 설정하기
-   Elasticsearch는 매핑되지 않은 필드를 거절한다
    -   모든 필드는 명시적으로 매핑된다
    -   RDB의 동작과 유사해짐

```json
PUT /people
{
  "mappings": {
    "dynamic": "strict",
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
  "error" : {
    "root_cause" : [
      {
        "type" : "strict_dynamic_mapping_exception",
        "reason" : "mapping set to strict, dynamic introduction of [last_name] within [_doc] is not allowed"
      }
    ],
    "type" : "strict_dynamic_mapping_exception",
    "reason" : "mapping set to strict, dynamic introduction of [last_name] within [_doc] is not allowed"
  },
  "status" : 400
}
```

### Numeric detection

```json
PUT /computers
{
    "mappings": {
        "numeric_detection": true
    }
}

POST /cmputers/_doc
{
    "specifications": {
        "other": {
            "max_ram_gb": "32", // long
            "bluetooth": "5.2", // float
        }
    }
}
```

### Default date detection formats

```js
['strict_date_optional_time', 'yyyy/MM/dd HH:mm:ss Z||yyyy/MM/dd Z'];
```

```json
PUT /computers
{
    "mappings": {
        "date_detection": false
    }
}

PUT /computres
{
    "mappings": {
        "dynamic_date_formats": ["dd-MM-yyyy"]
    }
}
```
