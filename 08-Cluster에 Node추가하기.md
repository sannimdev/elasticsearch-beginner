# Cluster에 Node 추가하기 (개발 모드) 🤔

## 소개

-   Elasticsearch를 확장하기 위해서 추가적인 노드 추가가 필요하다.
-   복제 샤드는 이때 할당된다.
-   이러한 접근법은 Elastic Cloud에서는 사용될 수 없다.
-   Cluster에 2개의 노드를 각각 다른 방법으로 추가할 것이다.
-   상용 환경이 아닌 개발 환경에서 고안된 방법

## Right 방법

-   Download 후 Elasticsearch 압축 풀기
-   Cluster와 Node 이름 설정하기
    -   이름 설정은 `config/elasticsearch.yml`에서 할 수 있다.
        ```
        #! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
        index                               shard prirep state      docs   store ip        node
        .kibana-event-log-7.15.2-000001     0     p      STARTED       3  17.7kb 127.0.0.1 node-2
        .apm-custom-link                    0     p      STARTED       0    208b 127.0.0.1 node-2
        .tasks                              0     p      STARTED       4  27.3kb 127.0.0.1 node-2
        .ds-ilm-history-5-2021.12.05-000001 0     p      STARTED                 127.0.0.1 node-2
        .kibana_7.15.2_001                  0     p      STARTED      50   4.7mb 127.0.0.1 node-2
        .geoip_databases                    0     p      STARTED      43  40.7mb 127.0.0.1 node-2
        pages                               0     p      STARTED       0    208b 127.0.0.1 node-2
        pages                               0     r      UNASSIGNED
        .apm-agent-configuration            0     p      STARTED       0    208b 127.0.0.1 node-2
        .kibana_task_manager_7.15.2_001     0     p      STARTED      15 412.9kb 127.0.0.1 node-2
        ```

## 덮어쓰기 세팅

1. 존재하는 Elasticsearch directory 다시 사용하기
2. Elasticsearch
    1. cluster.name
    2. node.name
    3. path.data
    4. path.logs

```
bin/elasticsearch -Enode.name=node-3 -Epath.data=./node-3/data -Epath.logs=./node-3/logs
```
