# How the match query works

-   Match쿼리는 일반적인 쿼리 작성을 단순화하는 풀 쿼리를 단순화한 편리한 래퍼일 뿐이다.
    pool query를 구성하기 위해 응용 프로그램에서 토큰화하는 대신 Elastic Search에서 검색 쿼리를 직접 던질 수 있음.

```json
GET /recipe/\_search
{
    "query": {
        "match": { "title": "pasta carbonara" }
    }
}
```

-   아래의 쿼리도 동일하게 동작한다.

```json
GET /recipe/\_search
{
    "query": {
        "bool": {
            "should": [
                {
                    "term": {
                        "title": "pasta"
                    }
                },
                {
                    "term": {
                        "title": "carbonara"
                    }
                }
            ]
        }
    }
}
```

-   아래의 쿼리 2개도 동일하게 동작한다.

```json
GET /recipe/\_search
{
    "query": {
        "match": {
            "title": {
                "query": "pasta carbonara",
                "operator": "and"
            }
        }
    }
}
```

-   must의 경우 대소문자 구분

```json
GET /recipe/\_search
{
    "query": {
        "bool": {
            "must": [
                {
                    "term": {
                        "title": "pasta"
                    }
                },
                {
                    "term": {
                        "title": "carbonara"
                    }
                }
            ]
        }
    }
}
```
