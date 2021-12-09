# Sharding and scalability

-   이전 강좌에서 `클러스터`가 어떻게 하나 이상의 노드로 구성되는지를 알아보았다.
-   `클러스터`는 Elasticsearch가 데이터 저장과 공간을 확장할 수 있는지에 관한 방법 중 하나이다.
    -   만약 1TB의 데이터를 저장하기 원하는데 500GB의 싱글 노드를 가지고 있다면 이것은 불가능할 것이다.
    -   그러나 만약에 용량이 충분한 노드를 추가한다면 Elasticsearch가 두 노드에 데이터를 저장할 수 있으므로 클러스터 스토리지 용량이 충분하다고 할 수 있따.
    -   그런데 실제로 어떻게 동작할까?
    -   Elasticsearch는 샤딩이라는 것을 사용한다.
    -   데이터베이스의 context라는 용어를 들어봤을 수도 있는데 이것과 같은 개념이므로 한번 살펴보자

## Sharding(샤딩[셰딩])

`샤딩은 분리된 조각에 인덱스를 매겨 나누는 방법`인데 `이 각각의 조각들을 샤드`라고 부르는 것이다.
샤딩이 클러스터 또는 노드 레벨이 아닌 인덱스 레벨에서 수행되는 방식을 알아보자.
하나의 인덱스가 수십 억의 documents를 포함할 수 있는 반면, 다른 인덱스는 수백 개의 documents만 포함할 수 있다. 이는 어떻게 인덱스를 설정하느냐에 따라 달라진다.
가장 주된 이유로는 인덱스를 여러 개의 샤드로 나누는 것인데 이는 수평적으로 데이터의 볼륨의 크기를 확대할 수 있다.

### The Simple Example Case

-   Node A(Capacity: 500GB)
-   Node B(Capacity: 500GB)

Elasticsearch에서 각각 500GB 크기의 2개의 노드 저장 공간을 사용할 수 있다고 가정하자.
이 상황에서 600GB 크기의 인덱스를 가지고 있다면 전체 노드에 맞지 않을 것이다.
따라서 샤드는 단일 노드에 배치되어야 하므로 단일 샤드에서 인덱스를 실행하는 것은 선택 사항이 아니다.
대신 인덱스를 각각 300기가바이트의 디스크 공간이 필요한 두 개의 샤드로 나눌 수 있다.

이렇게 하면 샤드를 각각 2개의 노드에 배치할 수 있다. 또한 원한다면 4개의 샤드를 150GB씩 배치할 수도 있다.
여전히 여분이 있기 때문에 필요한 만큼 다른 indices에 쓸 수 있다.

간단히 말해, 샤드는 어떠한 노드에도 배치할 수 있는데 인덱스에 5개의 샤드가 있는 경우 5개의 다른 노드에 분산시킬 필요가 없다. 할 수는 있지만 할 필요는 없다.

### 샤드의 특징

-   각 샤드는 독립적이며, 샤드는 그 자체로 완전한 기능을 하는 인덱스이다.
-   100% 정확한 것은 아니고 이에 가깝다고 보면 된다.
-   각각의 샤드는 사실 Lucene index이다.
    -   즉 5개의 샤드가 있는 Elasticsearch 인덱스는 실제로 후드 아래에 5개의 Lucene 인덱스로 구성된다는 것이다.

### 샤딩의 목적

-   더 많은 documents를 저장할 수 있다.
-   샤딩을 사용함으로써 하나의 인덱스에 수십 억 개의 documents를 저장할 수 있는데 이는 보통 샤딩 없이는 불가능하다.
-   인덱스를 샤딩하는 또 다른 일반적인 사용 사례는 인덱스를 노드에 더 쉽게 맞출 수 있도록 더 작은 청크로 나누는 것이다.
-   성능 향상
    -   검색 쿼리를 여러 샤드에서 동시에 처리할 수 있으므로 성능과 처리량도 향상할 수 있다.
    -   샤드는 서로 다른 노드에 저장될 수 있으므로 여러 노드의 하드웨어를 사용할 수 있습니다.

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

여기서 pri 항목이 `primary shards`를 의미한다.
이 열이 주어진 인덱스의 샤드 수를 나타낸다고 생각하자.
(7버전까지는 실제로 인덱스에 5개의 샤드가 있는 것이었다. 그러나 5개의 샤드를 작은 indices로 가지는 것은 매우 불필요하다.)
작은 크기의 indices를 만들면 너무 많은 샤드가 만들어지는 over-shading 현상의 문제가 발생한다.

이전에는 샤드의 개수를 변경하는 것이 생성된 이후에는 불가능했다.
그래서 이전에는 더 많은 샤드가 필요했다면, 새로운 인덱스를 만들어 documents 위로 이동해야 했다 (?)
