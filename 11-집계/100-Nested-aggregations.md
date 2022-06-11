# Nested aggregations

-   이것은 문서 그룹에 대한 문서 수를 검색하는 것 외에는 그 자체로 그리 유용하지 않다

```json
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "status_terms": {
      "terms": {
        "field": "status"
      },
      "aggs": {
        "status_stats": {
          "field": "total_amount"
        }
      }
    }
  }
}
```

-   강의 내용처럼 결과화면이 안 뜬다 ㅠㅠ
