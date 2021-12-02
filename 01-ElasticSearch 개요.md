## Elasticsearch

### ElasticSearch란?

-   오픈소스 분석 툴이자 전체 텍스트 검색 엔진 (Analytics & fulltext search engine)
-   응용 프로그램에 대한 검색 기능을 활성화하는 데 자주 사용된다
-   블로그 포스트, 상품, 카테고리 등등 원하는 곳 모두 사용할 수 있음
    -   Elasticsearch를 사용하여 보기와 유사한 복잡한 검색 기능을 구축할 수 있다.
    -   여기에는 자동 완성, 오타 수정, 일치 항목 강조 표시, 동의어 처리, 관련성 조정 등이 포함된다.

### 사용 예시

-   예를 들어 쇼핑몰을 운영한다고 가정하면, 제품 이름 및 기타 전체 텍스트 필드를 검색하는 것 외에도 결과를 정렬할 때 여러 요인을 고려한다.
    제품에 등급이 있다면, 우리는 아마도 높은 등급의 제품에 대한 관련성을 높이고 싶을 것이다. 우리는 또한 사용자들이 가격대, 브랜드, 크기, 그리고 같은 결과를 필터링하는 것을 허용하기를 원할 수 있다. 예를 들어, 가격이나 관련성에 따라 정렬한다.
-   기본적으로 ES는 강력한 검색엔진으로서 필요한 거의 모든 것을 할 수 있다. 전체 텍스트 검색 이외에도 숫자 및 집계 데이터와 같은
    구조화된 데이터를 조회할 때도 분석 플랫폼으로서 사용되기도 한다. 데이터를 집계하는 쿼리를 작성하고 결과를 원형 차트를 만드는 데 사용할 수 있다. ElasticSearch가 지능적인 툴은 아니나 저장된 데이터에서 값진 정보들을 취득할 수 있다. 다양한 서버 로그를 저장한 후 이를 분석할 수 있다.
-   상점에서 ElasticSearch로 매출을 보낸다면 이 경우 어떤 상점이 가장 많이 팔리는지 분석할 수 있다.
-   관계형 데이터베이스에서 알 수 있는 총계 기능을 사용할 수 있으나 ES에서는 그것보다 더 많은 것을 할 수 있으므로 데이터를 더 잘 분석한다고 볼 수 있다.
    -   머신 러닝을 사용하여 과거 데이터 기반으로 매출을 예측할 수 있음
    -   또는 웹 사이트 방문자 수를 추적하여 웹 서버에 추가해야 하는지 여부와 시기를 예측할 수 있음
    -   또한, (기존 데이터의 규칙성을 띤 상태와 다른) 이상 현상을 감지할 수 있다. - 이를 감지하여 이메일, 슬랙 등으로 노티를 보내는 기능도 구현할 수 있다.
        위의 예시처럼 Elasticsearch를 적용할 수 있는 상황이 많다는 것이 요지이다.

### Elasticsearch 개요

-   데이터는 정보의 한 단위로 구성되는 document(문서)에 저장된다.
-   RDBMS의 개념 중
    -   `행`에 상응하는 개념이 바로 `document`이다. (row = 행)
    -   `열`에 해당하는 개념이 바로 `field`이다. (column = 열)
-   document는 본질적으로 JSON 객체로 구성된다.
    ```json
    {
      “firstName”: “Bo”,
      “lastName”: “Andersen”,
      “interests”: [“Elasticsearch”, “Logstash”, “Kibana”, “Elastic Stack”]
    }
    ```
-   document에 쿼리를 날리는 방법은 RESTful API를 사용하는 것이다.
    -   RESTful API에 익숙하지 않더라도 걱정할 필요가 없다.
    -   HTTP API를 설계하는 방법일 뿐이며 Elasticsearch를 사용하려고 굳이 익숙해질 필요는 없다.
    -   Elasticsearch로 보내는 쿼리도 JSON으로 작성되므로 꽤 사용하기 쉽다.
-   Elasticsearch는 자바로 작성되어 있으며 Apache Lucene 위에 빌드된다.
-   사용하기 쉽기 때문에 인기가 좋다.

### Elastic Stack 소개

-   Kibana: 시각화 툴
-   Logstash
    -   전통적으로는 응용 프로그램에서 처리된 로그를 Elasticsearch에 보내기 위해 사용한다.
    -   그런데 요즘은 데이터 프로세싱 파이프라인을 의미한다.
        ```
        input {
             file {
                 path => “/path/to/apache_access.log”
             }
        }
        filter {
            if [request] in [“/robots.txt”, “/favicon.ico”] {
                drop {}
            }
        }
        output {
            file {
                path => “%{type}_%{+yyyy_MM_dd}.log”
            }
        }
        ```
-   XPack: Elasticssearch & Kibana (A collection of features such as security, monitoring, alerting, reporting, etc.)
-   Graph: 데이터와 연관되어 있음. 음악 재생 중에 다음 노래를 Spotify에서 추천해야 한다면 그것은 사용자가 좋아하는 것과 관련되어 있다.
-   Beats: A collection of data shippers that send data to Elasticsearch of LogStash
-   SQL
