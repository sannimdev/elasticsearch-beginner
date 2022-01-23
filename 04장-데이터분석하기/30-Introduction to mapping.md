# Mapping 소개

## 매핑이란?

-   Document들의 구조를 정의하는 것 (예: 필드와 데이터 타입들)
    -   또한 어떻게 값들이 색인되는것인지를 설정하는 것
-   RDBMS에서 테이블의 스키마와 유사한 개념
    -   MYSQL
        ```sql
        CREATE TABLE employees (
            id INT AUT_INCREMENT PRIMARY KEY,
            first_name VARCHAR(255) NOT NULL,
            last_name VARCHAR(255) NOT NULL,.
            dob DATE,
            description TEXT,
            created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        )
        ```
    -   Elasticsearch
        ```json
        PUT /employees
        {
            "mappings": {
                "properties": {
                    "id": { "type": "integer" },
                    "first_name": { "type": "text" },
                    "last_name": { "type": "text" },
                    "dob": { "type": "date" },
                    "description": { "type": "text" },
                    "created_at": { "type": "date" }
                }
            }
        }
        ```
-   Explicit mapping
    -   직접 정의하는 것
-   Dynamic mapping
    -   Elasticsearch가 필드를 매핑을 생성해주는 것
-   명시적, 동적 매핑 통합할 수 있음
