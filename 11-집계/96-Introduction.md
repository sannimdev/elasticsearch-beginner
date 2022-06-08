# Introduction for aggregation

ES에서 Aggregations(집계) 기능은 RDB와 비교했을 떄 좀 더 강력하다

-   GET `/orders/_mapping`
    ```json
    {
        "orders": {
            "mappings": {
                "properties": {
                    "lines": {
                        "type": "nested",
                        "properties": {
                            "amount": {
                                "type": "double"
                            },
                            "product_id": {
                                "type": "integer"
                            },
                            "quantity": {
                                "type": "short"
                            }
                        }
                    },
                    "purchased_at": {
                        "type": "date"
                    },
                    "sales_channel": {
                        "type": "keyword"
                    },
                    "salesman": {
                        "properties": {
                            "id": {
                                "type": "integer"
                            },
                            "name": {
                                "type": "text"
                            }
                        }
                    },
                    "status": {
                        "type": "keyword"
                    },
                    "total_amount": {
                        "type": "double"
                    }
                }
            }
        }
    }
    ```
