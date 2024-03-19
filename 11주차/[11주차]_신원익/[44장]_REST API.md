# REST API

REST는 HTTP를 기반으로 클라이언트가 **서버의 리소스에 접근하는 방식**을 규정한 아키텍처고, REST API는 REST를 기반으로 서비스 API를 구현한 것을 의미한다.

## REST API의 구성

자원(resource), 행위(verb), 표현(representations)의 3가지 요소로 구성된다

| 구성요소              | 내용                           | 표현 방법        |
| --------------------- | ------------------------------ | ---------------- |
| 자원(resource)        | 자원                           | URI(엔드포인트)  |
| 행위(verb)            | 자원에 대한 행위               | HTTP 요청 메서드 |
| 표현(representations) | 자원에 대한 행위의 구체적 내용 | 페이로드         |

## REST API 설계 원칙

### 1. URI는 리소스를 표현해야 한다

URI는 리소스를 표현하는 데 중점을 두어야 한다. 리소스를 식별하기 위한 이름은 동사보다 명사를 사용해야한다.

```js
# good
GET /todos/1
```

### 2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다

HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법이다. 주로 5가지 요청 메서드(GET, POST, PUT, PATCH, DELETE 등)를 사용하여 CRUD를 구현한다.
|HTTP 요청 메서드|종류|목적|페이로드|
|--|--|--|--|
|GET|index/retrieve|모든/특정 리소스 취득|❌|
|POST|create|리소스 생성|⭕|
|PUT|replace|리소스의 전체 교체|⭕|
|PATCH|modify|리소스의 일부 수정|⭕|
|DELETE|delete|모든/특정 리소스 삭제|❌|
