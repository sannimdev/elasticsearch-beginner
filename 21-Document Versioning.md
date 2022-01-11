# Document Versioning

## Versioning 소개

1. 문서의 개정 이력이 아님
2. Elasticsearch는 모든 문서와 함께 `_version` 메타데이터 필드를 저장
    - integer 형식
    - 수정될 때마다 증가
    - Document를 삭제할 때 60초 동안 유지됨
        - index.gc_deletes 세팅 설정에 따름
    - \_version 필드는 문서를 검색할 때 반환된다.

## 예시

```json
{
    "_index": "people",
    "_type": "_doc",
    "_id": "123",
    "_version": 2,
    "_seq_no": 0,
    "_primary": 1,
    "_source": {
        "name": "Bo Andersen",
        "country": "Denmark"
    }
}
```

## Versioning 유형

-   Internal versioning은 기본적인 versioning 타입이다.
-   외부 버저닝 타입도 존재한다.
    -   Elasticsearch의 바깥에 유지할 수 있다.
        -   RDBMS에 저장된 경우

## Versioning 관리 목적

-   얼마나 많이 Document가 수정되었는지를 확인할 수 있음
    -   그다지 유용하지는 않을 듯
-   Versioning은 더 이상 거의 사용되지 않고, 대부분 과거의 물건입니다. (🤷‍♂️ 그럼 진작 일찍 말하지!!)
-   이전에는 동시성 제어를 하는 방법이었다.
    -   지금은 더 나은 방법이 존재한다.
-   이 필드는 이전 버전을 실행하는 클러스터에 사용된다.
