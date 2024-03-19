# 📒44장_ REST API
### 📑목차
- [REST API의 구성](#rest-api의-구성)
- [REST API 설계 원칙](#rest-api-설계-원칙)

### ⚡Quick Summary
- *REST(REpresentational State Transfer)*
    > 로이 필딩(Roy Fielding)의 2000년 논문에서 처음 소개된 개념으로, HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처 스타일 (제약 조건의 집합)
    - RESTful
        > REST의 기본 원칙을 성실히 지킨 서비스
    - REST API
        > REST를 기반으로 서비스 API를 구현한 것
- ⭐REST의 중요한 제약 조건은 **Uniform Interface** ([🎥발표 영상 참고](https://www.youtube.com/watch?v=JKMh3gBPHzs))
    1. Identification of resources (자원에 대한 식별)
        > **URI**로 자원을 식별
    2. Manipulation of resources through representations (표현을 통한 자원 조작)
        > **HTTP 요청 메서드**로 자원에 대한 행위(조작)를 표현
    3. Self-descriptive messages (자기 서술적 메시지)
        > **Content Type Header**을 명시해 HTTP 메시지 내용만 보고도 해석이 가능
    4. Hypermedia as the engine of application state (HATEOAS)
        > [Hyperlink]()를 이용해 애플리케이션의 상태를 전이
    - 보통 REST API는 1번과 2번 제약 조건에 초점을 맞추고 있음: REST API URI 규칙
        1. 대문자 ❌, **소문자** ⭕
        2. 언더바(`_`) ❌, **하이픈(`-`)** ⭕
        3. 마지막에 슬래시(`/`)를 포함 ❌
        4. **행위(목적, CRUD function name) 포함 ❌**
        5. 파일 확장자는 포함 ❌
        6. 동사보다는 **명사**
        7. URI에 작성되는 영어는 **복수형** 사용
        8. 슬래시(`/`)는 계층적 관계를 나타낼 때 사용

        - 예시
            |  | method | URL |
            | --- | --- | --- |
            | 상품 등록 | POST | `http://localhost:8888/product` |
            | 전체 상품 삭제 | DELETE | `http://localhost:8888/products` |
            | 전체 상품 조회 | GET | `http://localhost:8888/products` |
            | 상품 개별 조회 | GET | `http://localhost:8888/products/{id}` |
            | 상품 개별 수정 | PUT | `http://localhost:8888/products/{id}` |

## 📌REST API의 구성
- ⭐REST API는 자원(resoure), 행위(verb), 표현(representations)의 3가지 요소로 구성됨
- REST는 **자체 표현 구조**(self-descriptiveness)로 구성되어 REST API만으로 HTTP 요청의 내용 이해 가능

|구성요소|내용|표현 방법|
|--|--|--|
|자원(resoure)|자원|URI(endpoint)|
|행위(verb)|자원에 대한 행위|HTTP 요청 메서드|
|표현(representations)|자원에 대한 행위의 구체적 내용|payload|

## 📌REST API 설계 원칙
### URI는 리소스를 표현
- 동사보다는 명사 사용
- 행위에 대한 표현 사용 X

### 행위에 대한 정의는 HTTP 요청 메서드로 표현
- 주로 5가지 요청 메서드 사용
    |HTTP 요청 메서드|종류|목적|payload|
    |--|--|--|--|
    |GET|index/retrieve|모든/특정 리소스 취득|❌|
    |POST|create|리소스 생성|⭕|
    |PUT|replace|리소스의 전체 교체|⭕|
    |PATCH|modify|리소스의 일부 수정|⭕|
    |DELETE|delete|모든/특정 리소스 삭제|❌|

```
# bad
GET /getTodos/1
GET /todos/show/1
GET /todos/delete/1

# good
GET /todos/1
DELETE /todos/1
```