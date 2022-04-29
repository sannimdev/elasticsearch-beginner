# Analyzers and search queries

-   분석기(analyzers)가 text 필드를 인덱싱할 때 사용된다고 말한 적이 있다.
    -   검색 쿼리에도 사용되기 때문에 이는 절반에 불과하다.

```js
const textField = "I loved drinking bottles of wine on last year's vacation.";
const analyzed = ['i', 'love', 'drink', 'bottl', 'of', 'wine', 'on', 'last', 'year', 'vacat'];
```

```json
GET /stemming_test/_search
{
    "query": {
        "match": {
            "description": "drinking"
        }
    }
}
```

-   위의 쿼리에서도 description 항목으로 요청한 "drinking"이 똑같은 방식으로 text 분석기에 의해 분석된다.
    1.  소문자화
    2.  stemming
    3.  stop words

[analyzer 원리를 다루는 강의 내용이 포함되어 있음]
