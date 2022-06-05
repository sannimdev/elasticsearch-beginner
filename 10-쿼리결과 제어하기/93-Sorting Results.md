# Sorting results

-   ES에서 결과를 정렬하는 것은 매우 쉽다

```json
GET /recipe/_search
{
  "_source": false,
  "query": {
    "match_all": {}
  },
  "sort": [
      "preparation_time_minutes"
  ]
}
```

## 내림차순

```json
GET /recipe/_search
{
  "_source": ["prepration_time_minutes", "created"],
  "query": {
    "match_all": {}
  },
  "sort": [
      {"preparation_time_minutes": "asc"},
      {"created": "desc"}
  ]
}
```
