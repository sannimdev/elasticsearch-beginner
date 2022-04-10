# Updating existing mappings

-   product ID가문자열에 포함되었다고 가정할 경우
-   product_id필드의 데이터 타입을 text나 keyword로 바꿔야 할 필요가 있다.
    -   full-text 검색을 사용하지 않을 것이다
    -   필터링을 사용할 것이고 keyword 데이터 타입이 가장 이상적이다

## Mapping 갱신 시 제약사항

-   일반적으로 Elasticsearch는 필드 매핑이 바뀌지 않는다
-   새로운 필드 매핑을 추가할 수 있으나 그것이 전부이다.
-   기존 매핑에 대해 몇 가지 매핑 매개 변수를 업데이트할 수 있다.

```json
PUT /reviews/_mapping
{
    "properties": {
        "product_id": {
            "type": "keyword"
        }
    }
}
```

```json
PUT /reviews/_mapping
{
    "properties": {
        "author": {
            "properties": {
                "email": {
                    "type": "keyword",
                    "ignore_above":256,
                }
            }
        }
    }
}
```

-   기존 문서에서는 매핑을 업데이트할 수 있는 것이 문제가 된다.
    -   Text값이 이미 분석되었거나
    -   일부 데이터 유형을 변경하려면 전체 데이터 구조를 재구성해야 한다.
-   빈 인덱스더라도 mapping을 갱신할 수 없다
-   필드 매핑은 삭제될 수 없다
    -   documents를 인덱싱할 때 필드를 생략한다
-   Query API를 통한 Update는 디스크 공간을 회수할 수 있다
-   해결책은 documents를 다시 색인하는 것이다. (sorry 😟)

(다음 강의에서는 재색인에 관하여 배울 것)
