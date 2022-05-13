# Flexible matching with the match query

## GET /recipe/default/\_search

-   구글 검색과 같이 사용자가 입력하는 단서를 검색하는 데 유용하며 간단한 쿼리이지만 매우 유연한 일치를 허용한다.
-   매치가 쿼리 공개 조회이기 때문에 Pasta라는 용어만 포함되고 다른 용어는 포함되지 않는다.
    -   public inquiry이기 때문
-   관련성이 높은 문서가 있다고 하더라도 중요하지 않은 항마다 일부 쿼리가 일치하지 않을 수도 있다.

```json
{
    "query": {
        "match": {
            "title": "Recipes with pasta or spaghetti"
        }
    }
}
```

```json
{
    "query": {
        "match": {
            "title": {
                "query": "pasta or spaghetti",
                "operator": "and"
            }
        }
    }
}
```
