# Terms lookup mechanism

```json
PUT /users/_doc/1
{
  "name": "John Roberts",
  "following" : [2, 3]
}
PUT /users/_doc/2
{
  "name": "Elizabeth Ross",
  "following" : []
}
PUT /users/_doc/3
{
  "name": "Jeremy Brooks",
  "following" : [1, 2]
}
PUT /users/_doc/4
{
  "name": "Diana Moore",
  "following" : [3, 1]
}
PUT /stories/_doc/1
{
  "user": 3,
  "content": "Wow look, a penguin!"
}
PUT /stories/_doc/2
{
  "user": 1,
  "content": "Just another day at the office... #coffee"
}
PUT /stories/_doc/3
{
  "user": 1,
  "content": "Making search great again! #elasticsearch #elk"
}
PUT /stories/_doc/4
{
  "user": 4,
  "content": "Had a blast today! #rollercoaster #amusementpark"
}
PUT /stories/_doc/5
{
  "user": 4,
  "content": "Yay, I just got hired as an Elasticsearch consultant - so excited!"
}
PUT /stories/_doc/6
{
  "user": 2,
  "content": "Chilling at the beach @ Greece #vacation #goodtimes"
}
```

## 검색 쿼리문

```json
GET /stories/_search
{
  "query": {
    "terms": {
      "user": {
        "index": "users",
        "id": "1",
        "path": "following"
      }
    }
  }
}
```

## Elasticsearch

-   Elasticsearch의 term 한계는 약 65,000개 정도
-   Can be configured per index, but keep performance in mind

## 이 강의에서 다루는 것

1. 다른 document를 이용하여 검색하기
2. 접근 방식의 성능 영향
