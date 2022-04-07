# How dates work in Elasticsearch

## 3가지 작동하는 방식

1. Specially formatted strings
2. Milliseconds since the epoch (long)
3. Seconds since the epoch (integer)

## Default behavior of date fields

-   제공되는 기본 형식
    1. time이 없는 date
    2. time이 포함된 date
    3. 밀리초(since the epoch) (long)
-   UTC 타임존 추정 (구체적으로 명시되어 있지 않으면)
-   Dates는 ISO 8601 명세를 따른다

## Dates 필드가 저장되는 과정

-   내부적으로 밀리초(long)로 저장된다.
-   유효한 값은 long value로 내부적으로 변환되어 저장된다
-   Dates는 UTC 타임존으로 변환된다

```json
PUT /reviews/_doc/2
{
  "rating": 4.5,
  "content": "Not bad. Not bad at all!",
  "product_id": 123,
  "created_at": "2015-03-27", 👈 자정으로 가정?
  "author": {
    "first_name": "Average",
    "last_name": "Joe",
    "email": "avgjoe@example.com"
  }
}
```

```json
PUT /reviews/_doc/3
{
  "rating": 3.5,
  "content": "Could be better",
  "product_id": 123,
  "created_at": "2015-04-15T13:07:41Z", 👈 ISO 8601 명세에 따른 형식
  "author": {
    "first_name": "Spencer",
    "last_name": "Pearson",
    "email": "spearson@example.com"
  }
}
```

-   날짜와 시간을 구별하는 구분자 'T'
-   타임존을 의미하는 'Z' (UTC)

```json
PUT /reviews/_doc/4
{
  "rating": 5.0,
  "content": "Incredible!",
  "product_id": 123,
  "created_at": "2015-01-28T09:21:51+01:00", 👈 UTC offset 사용하기
  "author": {
    "first_name": "Adam",
    "last_name": "Jones",
    "email": "adam.jones@example.com"
  }
}
```

```json
# 2015-07-04T12:01:24Z
PUT /reviews/_doc/5
{
  "rating": 4.5,
  "content": "Very useful!",
  "product_id": 123,
  "created_at": 1436011284000,
    "author": {
    "first_name": "Taylor",
    "last_name": "West",
    "email": "twest@example.com"
  }
}
```
