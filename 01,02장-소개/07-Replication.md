# Replication

-   기본적으로 인덱스는 단일 샤드로 구성된다는 것을 앞서 배웠다.

## 그런데 샤드가 저장된 노드가 고장나는 디스크 장애가 발생했다면 어떻게 될까?

-   A: 복제하지 않았으면 데이터는 소실된다.

이것은 명백한 주요 문제이며 하드웨어가 언제든 잘못될 수 있다.
그러므로 언제든지 대체하여 작동할 수 있는 복제(replication)에 대해 고려해야 한다.

Elasticsearch는 샤드 복제를 지원하며 별도의 설정 없이 기본적으로 작동한다.
많은 DB가 기본적으로 복제 기능이 있으나, 이 기능을 설정하는 것은 복잡할 수 있다.

그래서 Elasticsearch는 복제 작업이 어떻게 작동하는가? index는 많은 샤드에 데이터를 저장하도록 설정되어 있다. 그리고 이것은 여러 노드에 교차하여 저장될 것이다. 이와 같이 복제는 index 레벨에서 설정된다.

1회 이상 복제된 샤드는 primary shard로써 참조될 수 있다.
주 샤드와 해당 복제본 샤드를 복제 그룹이라고 한다.
색인을 작성할 때 원하는 각 샤드의 복제본 수를 선택할 수 있으며, 하나는 기본값이다.

## 작동 원리

-   인덱스는 여러 노드에 걸쳐 저장될 수 있는 여러 샤드 내에 데이터를 저장하도록 구성된다.
-   Replication은 샤드의 사본을 만드는 일이다.
-   한 번 이상 복제된 샤드를 Primary 샤드라고 한다.
-   Primary 샤드와 복제본 샤드를 복제 그룹이라 한다.
-   복제된 샤드는 Primary 샤드와 마찬가지로 검색 요청을 처리할 수 있는 완전한 복사본이다

### 데이터 소실을 방지하는 방법

-   해당 Primary 샤드의 내용을 저장하지 않고 다른 노드의 샤드를 저장한다.
    얼마나 많은 복사본이 남아있는지는 인덱스의 설정에 따른다
-   Node A
    -   Primary Shard A
    -   Replica B1
    -   Replica B2
-   Node B
    -   Primary Shard B
    -   Replica A1
    -   Replica A2

복제본 샤드가 분산되어 가용성을 높일 수 있고 두 개의 노드가 동시에 중단되더라도 데이터가 손실되지 않는다.

## 복제된 샤드 개수 정하기

-   얼마나 많은 복제된 샤드가 이상적인지는 케바케
-   데이터가 RDBMS와 같은 이외의 곳에 저장되는지의 여부
-   복원하는 동안 사용할 수 없어도 괜찮은지
-   중요한

## 스냅숏

스냅숏을 생성하면 특정 시점으로 데이터를 복원할 수 있도록 백업을 수행할 수 있다.
특정 인덱스르 스냅숏으로 생성하거나 전체 클러스터를 생성할 수 있다.
스냅숏을 생성할 수 있는데도 Replica가 필요한 이유는 복제는 실제로 데이터 손실을 방지하는 방법이지만 실시간 데이터에서만 작동한다.

반면 스냅숏을 이용하면 클러스터의 현재 상태를 파일로 내보낼 수 있고 해당 파일을 사용하여 클러스터 또는 인덱스의 상태를 해당 상태로 복원할 수 있다. 중간 과정에서 엉망이 된 경우에 변경 사항을 원상태로 되돌리기 위해서는 스냅숏을 이용하여 되돌릴 수 있다.

## 🤔

인덱스는 복제본 샤드를 포함하지만 해당 샤드는 노드에 할당되지 않는다.

## 복제를 통한 쿼리 처리량 증가

-   Replication group에서 복제 샤드는 각각 다른 요청을 동시에 처리할 수 있다.
-   ES는 나중에 더 좋은 샤드로 요청을 전달한다.
-   다중의 복제 샤드가 같은 노드에 저장된 경우 CPU 병렬처리 성능을 개선한다.
    -   이 경우 동일한 인덱스를 검색하는 세 개의 쿼리를 동시에 실행할 수 있다.

## 키바나 실습

```
PUT /pages
```

```json
{
    "acknowledged": true,
    "shards_acknowledged": true,
    "index": "pages"
}
```

```
GET /_cluster/health
```

```json
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "cluster_name" : "elasticsearch",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 9,
  "active_shards" : 9,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 1,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 90.0
}
```

```
GET /_cat/indices
```

```
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
green  open .geoip_databases                RfFXY2uJR1ODJ7g19EShGg 1 0 43  43 122.4mb 122.4mb
yellow open pages                           gUX4AgJQQCOk3lFNpEbnFg 1 1  0   0    208b    208b
green  open .apm-custom-link                CQ8f60CgSsG_DwdeGXuyow 1 0  0   0    208b    208b
green  open .apm-agent-configuration        2E9sV76ZQLGuGYiH6opNEw 1 0  0   0    208b    208b
green  open .kibana-event-log-7.15.2-000001 IA6gcC0-RReLfBBq09SUoQ 1 0  2   0  11.8kb  11.8kb
green  open .kibana_7.15.2_001              DNXneSIjQgaW7eJPSPgsug 1 0 34  14   4.7mb   4.7mb
green  open .kibana_task_manager_7.15.2_001 HEkOrSc6T-Kn8EpR0urwag 1 0 15 216   1.6mb   1.6mb
green  open .tasks                          GzLIPhq4RYix1rUJPc5rRQ 1 0  2   0  13.6kb  13.6kb

```

인덱스 복제본 샤드를 포함하지만 해당 샤드는 노드에 할당되지 않는다.
복제된 샤드는 기본 샤드와 동일한 노드에 할당되지 않기 때문에 그 원인은 이미 알고 있다.
또한 클러스터는 단일 노드만 포함하므로 복제본 샤드를 할당할 곳이 없다.
따라서 복제본 샤드는 노드를 추가할 경우 할당된다. 이것이 바로 노란색 상태인 이유이다.
그래서 인덱스는 완벽하게 작동하나 노드가 중단되면 데이터 손실 위험이 있어서 노란색으로 표시되는 것이다.

```
GET /_cat/shards?v
```

```
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
index                               shard prirep state      docs  store ip        node
.kibana-event-log-7.15.2-000001     0     p      STARTED       2 11.9kb 127.0.0.1 DESKTOP-컴퓨터이름
.tasks                              0     p      STARTED       2 13.7kb 127.0.0.1 DESKTOP-컴퓨터이름
.apm-agent-configuration            0     p      STARTED       0   208b 127.0.0.1 DESKTOP-컴퓨터이름
■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
pages                               0     p      STARTED       0   208b 127.0.0.1 DESKTOP-컴퓨터이름
pages                               0     r      UNASSIGNED
■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
.kibana_7.15.2_001                  0     p      STARTED      42  4.7mb 127.0.0.1 DESKTOP-컴퓨터이름
.geoip_databases                    0     p      STARTED      43 40.7mb 127.0.0.1 DESKTOP-컴퓨터이름
.ds-ilm-history-5-2021.12.05-000001 0     p      STARTED                127.0.0.1 DESKTOP-컴퓨터이름
.kibana_task_manager_7.15.2_001     0     p      STARTED      15  1.7mb 127.0.0.1 DESKTOP-컴퓨터이름
.apm-custom-link                    0     p      STARTED       0   208b 127.0.0.1 DESKTOP-컴퓨터이름
```

-   p -> primary shard
    -   기본 샤드의 상태는 STARTED로 완전히 동작하며 요청에 사용된다
-   r -> replica shard
    -   복제본 샤드의 상태는 미할당된 상태이며 이를 적용하려면 클러스터에 노드를 하나 더 추가해야 한다.
