# Elasticsearch 시작하기

## Elasticsearch 다운로드 후 설치

1. 운영체제에 맞는 [Elasticsearch](https://www.elastic.co/kr/downloads/elasticsearch) 다운로드
    - 이전에 자바 설치 및 JAVA_HOME 세팅 (7.15버전 이후에는 ES_JAVA_HOME으로 바뀌었다.)
2. [Windows 기준] 디렉터리/bin/elasticsearch.bat 실행한다.
3. `localhost:9200`을 대상으로 Rest API를 요청하면 된다.
    - PowerShell의 Curl이나 Postman을 이용하여 요청하기
    - `curl http;//localhost:9200` 요청 결과
        ```
        StatusCode        : 200
        StatusDescription : OK
        Content           : {
                            "name" : "내 컴퓨터 이름",
                            "cluster_name" : "elasticsearch",
                            "cluster_uuid" : "고유번호",
                            "version" : {
                                "number" : "7.15.2",
                                "build_flavor" : "default",
                                "build_typ...
        RawContent        : HTTP/1.1 200 OK
                            X-elastic-product: Elasticsearch
                            Warning: 299 Elasticsearch-7.15.2-무언가 "Elasticsearch built-in
                            security features are not enabled. Without authent...
        Forms             : {}
        Headers           : {[X-elastic-product, Elasticsearch], [Warning, 299 Elasticsearch-7.15.2-무언가
                            ... "Elasticsearch built-in security features are not enabled. Without authentication, yo
                            ur cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/referen
                            ce/7.15/security-minimal-setup.html to enable security."], [Content-Length, 544], [Content-Type, ap
                            plication/json; charset=UTF-8]}
        Images            : {}
        InputFields       : {}
        Links             : {}
        ParsedHtml        : mshtml.HTMLDocumentClass
        RawContentLength  : 544
        ```
    - `Invoke-RestMethod http://localhost:9200` 요청 후 결과
        ```
        name         : 내 컴퓨터 이름
        cluster_name : elasticsearch
        cluster_uuid : 고유번호
        version      : @{number=7.15.2; build_flavor=default; build_type=zip; build_hash=해시 고유번호
                    0c; build_date=2021-11-04T14:04:42.515624022Z; build_snapshot=False; lucene_version=8.9.0; minimum_wire_
                    compatibility_version=6.8.0; minimum_index_compatibility_version=6.0.0-beta1}
        tagline      : You Know, for Search
        ```

## 참고사항

-   폴더 구조 증 modules과 plugins의 차이
    -   modules: Elasticsearch에 의해 제공
    -   plugins: 사용자가 임의로 기능상 Elasticsearch에 추가할 수 있는 것? (서드파티 개념)

## Kibana 다운로드 후 설치

-   [Kibana 다운로드 페이지](https://www.elastic.co/kr/downloads/kibana)에서 운영체제에 맞게 다운로드.
-   kibana.bat을 실행하면 1~2분 정도 소요된다.
-   `localhost:5601`에 웹브라우저를 이용핳여 접속
