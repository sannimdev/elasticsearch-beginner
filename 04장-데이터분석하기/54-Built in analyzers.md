# Built in analyzers

1. pre-configured combinations of character filters
2. token filters
3. a tokenizer

## Standard analyzer

-   단어 경계에서 텍스트를 분할하고 구두점을 제거한다.
    -   standard tokenzer 에 의해 수행된다.
-   소문자 토큰 필터가 있는 소문자
-   stop 토큰 필터를 포함한다. (기본값: disabled)

-   "Is that Peter's cute-looking dog?"
    -   ["is", "that", "peter's", "cute", "looking", "dog"]

## Simple analyzer

-   Standard analyzer와 유사하다.
    -   문자 이외의 것을 만나면 토큰으로 나뉜다.
-   lowercase tokenizer가 있는 소문자

    -   특이하고 성능이 좋다.

-   "Is that Peter's cute-looking dog?"
    -   ["is", "that", "peter", "s" ,"cute", "looking", "dog"]

## Whitespace analyzer

-   화이트 스페이스 단위로 문자열을 쪼갠다
-   소문자화하지 않는다

-   "Is that Peter's cute-looking dog?"
    -   ["Is", "that", "Peter's", "cute-looking", "dog?"]

## Keyword Analyzer

-   입력 텍스트를 그대로 유지하는 작동하지 않는 분석기이다.
    -   간단하게 단일 토큰으로 출력한다.
-   keyword 필드를 기본적으로 사용한다.

    -   정확하게 매칭한다

-   "Is that Peter's cute-looking dog?"
    -   ["Is that Peter's cute-looking dog?"]

## Pattern analyzer

-   토큰을 구별할 때 정규표현식이 사용된다.
    -   텍스트를 토큰으로 분할해야 하는 항목과 일치해야 한다
-   이 분석기는 매우 유연하다.
-   단어가 아닌 문자열을 기본적으로 패턴으로 매칭시킨다. (\w+)
-   소문자가 기본값

-   "Is that Peter's cute-looking dog?"
    -   ["is", "that", "peter", "s" ,"cute", "looking", "dog"]
