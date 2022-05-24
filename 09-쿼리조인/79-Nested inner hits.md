# Nested inner hits

```json
GET /department/_search
{
  "_source": false, ✍️
  "query": {
    "nested": {
      "path": "employees",
      "inner_hits": {}, ✍️
      "query": {
        "bool": {
          "must": [
            {
              "match": {
                "employees.position": "intern"
              }
            },
            {
              "term": {
                "employees.gender.keyword": {
                  "value": "F"
                }
              }
            }
          ]
        }
      }
    }
  }
}

```

```json
{
    "took": 4,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 2,
            "relation": "eq"
        },
        "max_score": 2.3905568,
        "hits": [
            {
                "_index": "department",
                "_type": "_doc",
                "_id": "1",
                "_score": 2.3905568,
                "inner_hits": {
                    "employees": {
                        "hits": {
                            "total": {
                                "value": 1,
                                "relation": "eq"
                            },
                            "max_score": 2.3905568,
                            "hits": [
                                {
                                    "_index": "department",
                                    "_type": "_doc",
                                    "_id": "1",
                                    "_nested": {
                                        "field": "employees",
                                        "offset": 3
                                    },
                                    "_score": 2.3905568,
                                    "_source": {
                                        "gender": "F",
                                        "name": "Julie Powell",
                                        "position": "Intern",
                                        "age": 26
                                    }
                                }
                            ]
                        }
                    }
                }
            },
            {
                "_index": "department",
                "_type": "_doc",
                "_id": "2",
                "_score": 2.3905568,
                "inner_hits": {
                    "employees": {
                        "hits": {
                            "total": {
                                "value": 2,
                                "relation": "eq"
                            },
                            "max_score": 2.3905568,
                            "hits": [
                                {
                                    "_index": "department",
                                    "_type": "_doc",
                                    "_id": "2",
                                    "_nested": {
                                        "field": "employees",
                                        "offset": 2
                                    },
                                    "_score": 2.3905568,
                                    "_source": {
                                        "gender": "F",
                                        "name": "Margaret Harris",
                                        "position": "Intern",
                                        "age": 19
                                    }
                                },
                                {
                                    "_index": "department",
                                    "_type": "_doc",
                                    "_id": "2",
                                    "_nested": {
                                        "field": "employees",
                                        "offset": 7
                                    },
                                    "_score": 2.3905568,
                                    "_source": {
                                        "gender": "F",
                                        "name": "Evelyn Henderson",
                                        "position": "Intern",
                                        "age": 24
                                    }
                                }
                            ]
                        }
                    }
                }
            }
        ]
    }
}
```
