# Dynamic Templates

## 사용되는 때

-   Dynamic Templates는 동적 매핑이 활성화되었을 때
-   새로운 필드를 만났지만 존재하는 매핑 정보가 없을 때

## JSON value -> match_mapping_type

-   true/false -> boolean
-   { ... } (objects) -> object
-   "string value" -> string
-   "2020/01/01" -> date
-   123.4 -> double
-   123 -> long
-   Any -> \*

## 실습

```json
PUT /dynamic_template_test
{
  "mappings": {
    "dynamic_templates": [
      {
        "integers": {
          "match_mapping_type": "long",
          "mapping": {
            "type": "integer"
          }
        }
      }
    ]
  }
}


POST /dynacmic_template_test/_doc
{
  "in_stock": 123
}
```

```json
GET /dynamic_template_test/_mapping

{
  "dynamic_template_test" : {
    "mappings" : {
      "dynamic_templates" : [
        {
          "integers" : {
            "match_mapping_type" : "long",
            "mapping" : {
              "type" : "integer"
            }
          }
        }
      ]
    }
  }
}
```

## match와 unmatch의 파라미터

-   필드 이름에 대한 조건을 지정하는 데 사용됩니다.
-   필드 이름은 match 파라미터에서 지정된 조건과 일치해야만 한다.
-   unmatch는 match 매개변수와 일치하는 필드를 제외하는 데 사용된다
-   파라미터는 패턴과 와일드카드(\*)를 지원한다.
    -   하드 코딩 필드 이름은 의미가 없다

```json
PUT /test_index
{
    "mappings": {
        "dynamic_templates": [
            {
                "strings_only_text": {
                    "match_mapping_type": "string",
                    "match": "text_*",
                    "unmatch": "*_keyword",
                    "mapping": {
                        "type": "text"
                    }
                }
            },
            {
                "strings_only_keyword": {
                    "match_mapping_type": "string",
                    "match": "*_keyword",
                    "mapping": {
                        "type": "keyword"
                    }
                }
            }
        ]
    }
}
```

```json
POST /test_index/_doc
{
    "text_product_description": "A description",
    "text_product_id_keyword": "ABC-123"
}
```

```json
{
    "test_index": {
        "mapping": {
            "properties": {
                "text_product_description": {
                    "type": "text"
                },
                "text_product_id_keyword": {
                    "type": "keyword"
                }
            }
        }
    }
}
```

## path_match와 path_unmatch 파라미터

-   필드 이름뿐만 아니라, 파라미터는 풀 필드 path를 가지고 평가한다.
-   이것이 앞에서 본 점`.` 표기법이다.
    -   ex) name.first_name
-   와일드카드(\*) 또한 지원

```json
PUT /test_index
{
    "mappings": {
        "dynamic_templates": [
            {
                "copy_to_full_name": {
                    "match_mapping_type": "string",
                    "path_match": "employer.name.*",
                    "mapping": {
                        "type": "text",
                        "copy_to": "full_name"
                    }
                }
            }
        ]
    }
}
```

```json
POST /test_index/\_doc
{
    "employer": {
        "name": {
            "first_name": "John",
            "middle_name": "Edward",
            "last_name": "Doe"
        }
    }
}
```

```json
{
    "properties": {
        "employer": {
            "properties": {
                "first_name": {
                    "type": "text",
                    "copy_to": ["full_name"]
                },
                "middle_name": {
                    "type": "text",
                    "copy_to": ["full_name"]
                },
                "last_name": {
                    "type": "text",
                    "copy_to": ["full_name"]
                }
            }
        }
    }
}
```

### {dynamic_type}

```json
PUT /test_index
{
    "mappings": {
        "dynamic_templates": [
            {
                "no_doc_values": {
                    "match_mapping_type": "*",
                    "mapping": {
                        "type": "{dynamic_type}",
                        "index": false
                    }
                }
            }
        ]
    }
}
```

```json
POST /test_index/_doc
{
    "name": "John Doe",
    "age": 26
}
```

```json
{
    "properties": {
        "age": {
            "type": "long",
            "index": false
        },
        "name": {
            "type": "text",
            "index": false
        }
    }
}
```

## Index templates vs Dynamic templates

-   인덱스 템플릿은 일치하는 인덱스에 매핑과 인덱스 설정을 적용한다.
    -   이것은 인덱스가 생성되고 이름이 패턴과 일치할 때 발생합니다.
-   다이내믹 템플릿은 새로운 필드를 만났을 때 평가된다.
    -   다이내믹 매핑이 활성화되었을 때 한정
    -   명시된 필드 매핑은 템플릿의 조건과 일치하는 경우에 추가된다.
