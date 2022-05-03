# Creating Custom analyzer

## 실습

```json
PUT /analyzer_test
{
  "settings": {
    "analysis": {
      "analyzer": {
        "my_custom_analyzer": {

        }
      }
    }
  }
}

POST /_analyze
{
  "analyzer": "standard",
  "text": "I&apos;m in a <em>good</em> mood&nbsp;-&nbsp;and I <strong>love</strong>"
}
```

```json
{
    "tokens": [
        {
            "token": "i",
            "start_offset": 0,
            "end_offset": 1,
            "type": "<ALPHANUM>",
            "position": 0
        },
        {
            "token": "apos",
            "start_offset": 2,
            "end_offset": 6,
            "type": "<ALPHANUM>",
            "position": 1
        },
        {
            "token": "m",
            "start_offset": 7,
            "end_offset": 8,
            "type": "<ALPHANUM>",
            "position": 2
        },
        {
            "token": "in",
            "start_offset": 9,
            "end_offset": 11,
            "type": "<ALPHANUM>",
            "position": 3
        },
        {
            "token": "a",
            "start_offset": 12,
            "end_offset": 13,
            "type": "<ALPHANUM>",
            "position": 4
        },
        {
            "token": "em",
            "start_offset": 15,
            "end_offset": 17,
            "type": "<ALPHANUM>",
            "position": 5
        },
        {
            "token": "good",
            "start_offset": 18,
            "end_offset": 22,
            "type": "<ALPHANUM>",
            "position": 6
        },
        {
            "token": "em",
            "start_offset": 24,
            "end_offset": 26,
            "type": "<ALPHANUM>",
            "position": 7
        },
        {
            "token": "mood",
            "start_offset": 28,
            "end_offset": 32,
            "type": "<ALPHANUM>",
            "position": 8
        },
        {
            "token": "nbsp",
            "start_offset": 33,
            "end_offset": 37,
            "type": "<ALPHANUM>",
            "position": 9
        },
        {
            "token": "nbsp",
            "start_offset": 40,
            "end_offset": 44,
            "type": "<ALPHANUM>",
            "position": 10
        },
        {
            "token": "and",
            "start_offset": 45,
            "end_offset": 48,
            "type": "<ALPHANUM>",
            "position": 11
        },
        {
            "token": "i",
            "start_offset": 49,
            "end_offset": 50,
            "type": "<ALPHANUM>",
            "position": 12
        },
        {
            "token": "strong",
            "start_offset": 52,
            "end_offset": 58,
            "type": "<ALPHANUM>",
            "position": 13
        },
        {
            "token": "love",
            "start_offset": 59,
            "end_offset": 63,
            "type": "<ALPHANUM>",
            "position": 14
        },
        {
            "token": "strong",
            "start_offset": 65,
            "end_offset": 71,
            "type": "<ALPHANUM>",
            "position": 15
        }
    ]
}
```

### char_filter

```json
POST /_analyze
{
  "analyzer": "standard",
  "text": "I&apos;m in a <em>good</em> mood&nbsp;-&nbsp;and I <strong>love</strong>"
}
```

```json
{
    "tokens": [
        {
            "token": "I'm in a good mood - and I love",
            "start_offset": 0,
            "end_offset": 72,
            "type": "word",
            "position": 0
        }
    ]
}
```

## Custom

```json
PUT /analyzer_test
{
  "settings": {
    "analysis": {
      "analyzer": {
        "my_custom_analyzer": {
          "type": "custom",
          "char_filter": ["html_strip"],
          "tokenizer": "standard",
          "filter": [
            "lowercase",
            "stop",
            "asciifolding"
          ]
        }
      }
    }
  }
}

POST /analyzer_test/_analyze
{
  "analyzer": "my_custom_analyzer",
  "text": "I&apos;m in a <em>good</em> mood&nbsp;-&nbsp;and I <strong>love</strong>"
}

```

```json
{
    "tokens": [
        {
            "token": "i'm",
            "start_offset": 0,
            "end_offset": 8,
            "type": "<ALPHANUM>",
            "position": 0
        },
        {
            "token": "good",
            "start_offset": 18,
            "end_offset": 27,
            "type": "<ALPHANUM>",
            "position": 3
        },
        {
            "token": "mood",
            "start_offset": 28,
            "end_offset": 32,
            "type": "<ALPHANUM>",
            "position": 4
        },
        {
            "token": "i",
            "start_offset": 49,
            "end_offset": 50,
            "type": "<ALPHANUM>",
            "position": 6
        },
        {
            "token": "love",
            "start_offset": 59,
            "end_offset": 72,
            "type": "<ALPHANUM>",
            "position": 7
        }
    ]
}
```

-   라틴문자 ca도 영문알파벳 ca로 변환됨

anaylzer는 다음에 강의 자세히 들어보기..
이런 기능이 있구나 정도로만 이해..
