# Using Analyze API

-   Analyzer는 어떤 것을 사용할지도 파라미터로 날릴 수 있음

```json
POST /_analyze
{
  "text": "2 guys walk into     a bar, but the third... DUCKS! :-)",
  "analyzer": "standard"
}

POST /_analyze
{
  "text": "2 guys walk into     a bar, but the third... DUCKS! :-)",
  "char_filter": [],
  "tokenizer": "standard",
  "filter": ["lowercase"]
}
```

```json
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "tokens" : [
    {
      "token" : "2",
      "start_offset" : 0,
      "end_offset" : 1,
      "type" : "<NUM>", 👈 Numeric
      "position" : 0
    },
    {
      "token" : "guys",
      "start_offset" : 2,
      "end_offset" : 6,
      "type" : "<ALPHANUM>", 👈 AlphaNumeric
      "position" : 1
    },
    {
      "token" : "walk",
      "start_offset" : 7,
      "end_offset" : 11,
      "type" : "<ALPHANUM>",
      "position" : 2
    },
    {
      "token" : "into",
      "start_offset" : 12,
      "end_offset" : 16,
      "type" : "<ALPHANUM>",
      "position" : 3
    },
    {
      "token" : "a",
      "start_offset" : 21,
      "end_offset" : 22,
      "type" : "<ALPHANUM>",
      "position" : 4
    },
    {
      "token" : "bar",
      "start_offset" : 23,
      "end_offset" : 26,
      "type" : "<ALPHANUM>",
      "position" : 5
    },
    {
      "token" : "but",
      "start_offset" : 28,
      "end_offset" : 31,
      "type" : "<ALPHANUM>",
      "position" : 6
    },
    {
      "token" : "the",
      "start_offset" : 32,
      "end_offset" : 35,
      "type" : "<ALPHANUM>",
      "position" : 7
    },
    {
      "token" : "third", 👈 '...' 문자 지웠음
      "start_offset" : 36,
      "end_offset" : 41,
      "type" : "<ALPHANUM>",
      "position" : 8
    },
    {
      "token" : "ducks", 👈 뒤에 이모티콘 :-) 사라짐, lowercase filter 덕분에 소문자화
      "start_offset" : 45,
      "end_offset" : 50,
      "type" : "<ALPHANUM>",
      "position" : 9
    }
  ]
}

```

-   다음 강의에서는 Analyzer가 출력하는 tokens에 무슨 일이 일어나는지에 관해 살펴볼 것
