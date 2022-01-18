# Importing data with cURL

## 소개

-   cURL의 HTTP 클라이언트를 사용할 수 있다.
    -   가장 인기있는 command line tool이다.
    -   이미 macOS나 리눅스에서 지원한다
    -   Windows에서도 이미 설치되어 있을 것이다.

## 테스트 데이터 다운로드하기

-   [Github](https://github.com/codingexplained/complete-guide-to-elasticsearch/blob/master/products-bulk.json)

## 명령어 입력하여 데이터 삽입하기

```
curl -H "Content-Type: application/x-ndjson" -XPOST http://localhost:9200 --data-binary "@products-bulk.json"
```

-   파일명에 `@` 붙는 거 아님 주의
