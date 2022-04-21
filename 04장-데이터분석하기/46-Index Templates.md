# Index Templates

-   Index 템플릿은 settings와 mappings를 구체화하는 것이다
-   하나 이상의 패턴과 일치하는 인덱스에 적용된다.
-   패턴은 와일드카드(`*`)를 포함할 것이다
-   인덱스 템플릿은 새로운 인덱스를 만들 때 영향을 미친다.

## 실습

```json
PUT /\_template/access-logs
{
    "index_patterns": ["access-logs-*"],
    "settings": {
        "number_of_shards": 2,
        "index.mapping.coerce": false
    },
    "mappings": {
        "properties": {
            "@timestamp": {
                "type": "date"
            },
            "url.original": {
                "type": "keyword"
            },
            "http.request.referrer": {
                "type": "keyword"
            },
            "http.response.status_code": {
                "type": "long"
            }
        }
    }
}
```

`PUT /access-logs-2022-04-20`

```json
{
    "acknowledged": true,
    "shards_acknowledged": true,
    "index": "access-logs-2022-04-20"
}
```

```json
GET /access-logs-2022-04-20
{
    "access-logs-2022-04-20": {
        "aliases": {},
        "mappings": {
            "properties": {
                "@timestamp": {
                    "type": "date"
                },
                "http": {
                    "properties": {
                        "request": {
                            "properties": {
                                "referrer": {
                                    "type": "keyword"
                                }
                            }
                        },
                        "response": {
                            "properties": {
                                "status_code": {
                                    "type": "long"
                                }
                            }
                        }
                    }
                },
                "url": {
                    "properties": {
                        "original": {
                            "type": "keyword"
                        }
                    }
                }
            }
        },
        "settings": {
            "index": {
                "routing": {
                    "allocation": {
                        "include": {
                            "_tier_preference": "data_content"
                        }
                    }
                },
                "mapping": {
                    "coerce": "false"
                },
                "number_of_shards": "2",
                "provided_name": "access-logs-2022-04-20",
                "creation_date": "1650450658979",
                "number_of_replicas": "1",
                "uuid": "ss7KQPW8Q6ee0W-G6IyaLQ",
                "version": {
                    "created": "7150299"
                }
            }
        }
    }
}
```

## 인덱스 템플릿의 우선순위

-   새로운 인덱스는 여러 개의 인덱스 템플릿을 매칭시킬 것이다
-   `order` 파라미터는 인덱스 템플릿의 우선순위를 정의하는 데 사용된다
    -   integer 형식의 값
    -   낮은 수치부터 우선순위가 높다.
