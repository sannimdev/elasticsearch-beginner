# Cluster (in Kibana)

-   Dev Tools 메뉴 (三 > dev Tools)
-   Rest API 예제

1. GET \_cluster/health

-   Method /API/COMMAND

-   ! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
    ```json
    {
        "cluster_name": "elasticsearch",
        "status": "green",
        "timed_out": false,
        "number_of_nodes": 1,
        "number_of_data_nodes": 1,
        "active_primary_shards": 7,
        "active_shards": 7,
        "relocating_shards": 0,
        "initializing_shards": 0,
        "unassigned_shards": 0,
        "delayed_unassigned_shards": 0,
        "number_of_pending_tasks": 0,
        "number_of_in_flight_fetch": 0,
        "task_max_waiting_in_queue_millis": 0,
        "active_shards_percent_as_number": 100.0
    }
    ```

2. GET /\_cat/nodes?v

```
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
ip        heap.percent ram.percent cpu load_1m load_5m load_15m node.role   master name
127.0.0.1           24          56   4                          cdfhilmrstw *      컴퓨터이름
```

3. GET /\_cat/indicies?v

```
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
health status index                           uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .geoip_databases                RfFXY2uJR1ODJ7g19EShGg   1   0         43           25     40.9mb         40.9mb
green  open   .apm-custom-link                CQ8f60CgSsG_DwdeGXuyow   1   0          0            0       208b           208b
green  open   .kibana-event-log-7.15.2-000001 IA6gcC0-RReLfBBq09SUoQ   1   0          1            0        6kb            6kb
green  open   .apm-agent-configuration        2E9sV76ZQLGuGYiH6opNEw   1   0          0            0       208b           208b
green  open   .kibana_7.15.2_001              DNXneSIjQgaW7eJPSPgsug   1   0         28            5      2.9mb          2.9mb
green  open   .kibana_task_manager_7.15.2_001 HEkOrSc6T-Kn8EpR0urwag   1   0         15         6789    801.9kb        801.9kb

```
