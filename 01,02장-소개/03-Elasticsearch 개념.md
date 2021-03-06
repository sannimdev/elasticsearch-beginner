# Elasticsearch 개념 (용어 정리)

## Node

-   Node란 데이터를 저장하는 Elasticsearch라는 본질적 인스턴스가 된다.
    -   필요한 경우 수 TB급 데이터를 저장할 수 있도록 필요한 만큼 노드를 실행할 수 있다.
    -   각각의 노드는 데이터의 일부를 저장한다.
    -   이러한 방식으로 여러 가상 머신 또는 물리적 머신에 데이터를 저장할 수 있으므로 각 머신의 디스크 용량이 몇 테라바이트에 불과하더라도 수 테라바이트의 데이터를 저장할 수 있다.
    -   노드는 시스템이 아닌 Elasticsearch 인스턴스를 나타내므로 동일한 시스템에서 원하는 수의 노드를 실행할 수 있다.
    -   즉, 개발 시스템에서 가상 시스템이나 컨테이너를 처리할 필요 없이 원하는 경우 노드 5개를 시작할 수 있음
-   물리, 가상머신, 도커를 이용한 컨테이너 등의 환경에서 구동할 수 있는 인스턴스

## 데이터 저장 위치

-   데이터는 노드 전체에 어떻게 분산되며, ElasticSearch는 주어진 데이터의 저장 위치를 어떻게 알 수 있을까?
    -   각 노드는 클러스터라고 하는 것에 속한다.

## Cluster

-   클러스터는 모든 데이터를 포함하는 관련 노드들의 모음이다
-   원하는 만큼 많은 클러스터를 가질 수 있다. 그러나 보통 하나로도 충분하다.
    -   클러스터는 기본적으로 서로 완전히 독립적이다.
    -   클러스터 간 검색을 수행할 수도 있으나 흔한 경우는 아니다.
    -   서로 다른 용도로 사용되는 여러 클러스터를 실행할 수 있음
        -   서로 다른 용도로 사용되는 여러 클러스터를 실행할 수 있습니다. 예를 들어 전자 상거래 애플리케이션 검색의 전원을 공급하기 위한 클러스터와 줄여서 APM(Application Performance Management)을 위한 클러스터가 있을 수 있다.
        -   여러 클러스터로 분할하는 이유는 일반적으로 논리적으로 구분하고 다르게 구성할 수 있기 때문이다.
-   본 강의에서는 하나로 클러스터를 구성한다.
-   노드를 시작할 때 실제로 클러스터가 자동으로 형성된다.
    -   클러스터가 이미 구성된 경우에는 노드가 시작될 때 기존 클러스터에 포함되거나 해당 노드로만 구성된 자체 클러스터를 생성하게 된다.
-   Elasticsearch 노드는 다른 노드가 없더라도 항상 클러스터의 한 부분이 된다.
    -   보통 하나의 노드만 가지는 것은 가용성과 확장성에 문제가 될 수 있으나, 개발 목적으로는 싱글 노드로도 충분하다.

## Documents

-   Documents는 원하는 데이터가 포함되어 있는 JSON 객체이다.
    -   문서를 색인화할 때 Elasticsearch로 보낸 원래 JSON 객체는 Elasticsearch가 내부적으로 사용하는 일부 메타데이터와 함께 저장된다.
        -   person이라는 것을 저장하고자 한다면, 객체는 아마 다음과 같이 보일 것이다.
            -   원래 객체
                ```json
                {
                    "name": "김민수",
                    "country": "대한민국"
                }
                ```
            -   내부적으로 사용하는 일부 메타데이터와 함께 저장된 객체
                실제 people에 관한 객체는 `_source` 키에 해당하는 객체로 저장된다는 것을 확인할 수 있음
                ```json
                {
                    "_index": "people",
                    "_type": "_doc",
                    "_id": "123",
                    "_version": 1,
                    "_seq_no": 0,
                    "_primary_term": 1,
                    "_source": {
                        "name": "김민수",
                        "country": "대한민국"
                    }
                }
                ```

## Index

-   Documents가 저장되는 곳(index)으로 documents가 같은 성격의 Document를 모은 것이다.
    -   물리적인 제한 개수는 없음.
-   RDBMS의 개념에 빗대면 Table이라고 볼 수 있다.

## Indicies

-   [Index vs Indicies](https://racoonlotty.tistory.com/entry/Elasticsearch-Index-vs-Indices)

Elasticsearch에서 사용하는 용어

-   Index: `색인`한 데이터
-   Indexing: 색인하는 과정
-   Indicies: `매핑 정보를 저장하는 논리적인 데이터 공간`
