# How the keyword date type works.

-   keyword Analyzer가 keywords 필드를 분석한다
-   keyword analyzer는 no-op 분석기라고 불린다.
    -   수정되지 않은 입력 문자열을 단일 토큰으로 반환한다
    -   그런 다음 토큰은 반전된 인덱스에 배치된다.
-   keyword 필드는 exact matching, aggregations, 정렬에 사용한다.

```json
POST /_analyze
{
    "text": "2 guys walk into   a bar, but the third... DUCKS! :-)",
    "analyzer": "keyword"
}
```

```json
    {
      "token" : "2 guys walk into   a bar, but the third... DUCKS! :-)",
      "start_offset" : 0,
      "end_offset" : 53,
      "type" : "word",
      "position" : 0
    }
  ]
}

```
