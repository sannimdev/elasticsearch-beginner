# Introduction to analysis

-   때때로 텍스트 분석이라고 한다
-   텍스트 필드/값에 적용할 수 있다.
-   텍스트 값은 document를 인덱싱할때 분석된다.
-   검색등 효율적인 데이터 구조에 결과가 저장된다.
-   `_source` 객체는 검색할 때는 사용되지 않는다
    -   문서 색인 작성 시 지정된 정확한 값을 포함한다.

텍스트 값이 색인화될 때 분석기라고 불리는 것이 다음 값을 처리하는 데 사용됩니다.
다른 말로 값은 분석되는 것이다.

-   Analyzer(분석기)
    -   Character filters
        -   원본 문자를 받아내어 첨가/삭제 작업 등을 통해 변형시킬 수 있다.
        -   분석기는 0개 혹은 여러 개의 필터를 포함한다.
        -   Character filters는 지정된 순서대로 적용된다 (ex: html_strip filter)
            -   입력
                ```html
                I&apos;m in a <em>good</em> mood&nbsp;-&nbsp; and I <strong>love</strong> acai!
                ```
            -   출력
                ```html
                I'm in a good mood - and I love acai!
                ```
    -   Tokenizer
        -   1개의 토크나이저를 갖는다.
        -   텍스트를 토큰으로 분해하는 과정(?)이다.
        -   토큰화의 일부로 문자열을 제거할 수 있다.
            -   Example
                -   입력: "I REALLY like beer!"
                -   출력: ["I", "REALLY", "like", "beer"]
    -   Token filters
        -   Tokenizer의 추력을 입력으로 수신한다.
        -   Token filter는 토큰을 추가, 수정, 삭제할 수 있다.
        -   Analyzer는 0개~여러 개의 토큰 필터를 포함할 수 있다.
        -   Token filters는 지정된 순서대로 적용된다.
            -   Example (lowercase filter)
                -   입력: ["I", "REALLY", "like", "beer"]
                -   출력: ["i", "really", "like", "beer"]

텍스트 값의 분석 결과는 검색 가능한 데이터 구조에 저장되는 것이다.

-   빌트인과 Custom 컴포넌트
    -   Analyzers, Character Filters, Tokenizers, Token filters는 빌트인(내장)되어 있어 바로 사용할 수 있다.
    -   Custom 컴포넌트로 만들 수도 있다.
        -   다음 Section에서 배움
    -   지금은 어떻게 기본적으로 텍스트가 분석되는지를 파악해보자.

Character Filters -> Tokenizer -> Token filters
(none) -> (standard) -> ["lowercase"]

입력: I REALLY like beer! -> I REALLY like beer! -> "I", "REALLY", "like" ,"beer"
출력: I REALLY like beer! -> ["I", "REALLY", "like", "beer"] -> ["i", "really", "like", "bear"]
