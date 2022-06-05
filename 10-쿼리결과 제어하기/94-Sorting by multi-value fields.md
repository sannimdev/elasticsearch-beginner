# Sorting by multi-value fields

-   GET /recipe/\_search
    ```json
    {
        "_source": false,
        "query": {
            "match_all": {}
        },
        "sort": [
            {
                "ratings": {
                    "order": "desc",
                    "mode": "avg"
                }
            }
        ]
    }
    ```
