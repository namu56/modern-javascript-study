**REST는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처**고, REST API는 REST를 기반으로 서비스 API를 구현한 것을 의미한다.

## 1. RESA API의 구성

REST API는 자원(resource), 행위(verb), 표현(representations)의 3가지 요소로 구성된다. REST는 자체 표현 구조(self-de-scriptiveness)로 구성되어 REST API만으로 HTTP 요청의 내용을 이해할 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8bd6354e-8da5-484b-9772-116c6c9a4676/Untitled.png)

## 2. RESA API 설계 원칙

1. **URI는 리소스를 표현해야 한다.**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5cfc7629-9cac-4354-9afe-ceacab1af064/Untitled.png)

1. **리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.**

   HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적(리소스에 대한 행위)을 알리는 방법으로 주로 5개의 요청 메서드를 사용해 CRUD를 구현한다.

   ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a0c20c60-c830-498b-9c0c-f2fd587bc251/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b78694eb-5f91-4c0d-91a5-8b2889dfed2e/Untitled.png)

## 3 JSON Server를 이용한 REST API 실습

### 1 JSON Server 설치

```jsx
$npm install json-server --sava-D
```

### 2 db.json 파일 생성

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e8512160-6118-4f2b-b052-ba52c17db7c8/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3538c9ca-a465-4cea-9784-b3e5afbd931c/Untitled.png)

### 3 JSON Server 실행

- **JSON Server —watch**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/70f40690-f2c3-4453-8744-9480e8983e1c/Untitled.png)

- **npm start**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5b3f9662-25e6-48e4-a6e0-7a106a4717dc/Untitled.png)

### 4 GET 요청

todos 리소스에서 모든 todo를 취득(index)한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a684003e-7fab-48ec-bfea-b2af9618b35a/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/827b84cb-a903-4177-8d2e-3971b580cd7f/Untitled.png)

### 3. POST 요청

todos 리소스에 새로운 todo를 생성한다. POST 요청시에는 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MINE 타입을 지정한다.

### 6 PUT 요청

특정 리소스 전체를 교체할 때 사용한다.

### 7 PATCH 요청

PATCH는 특정 리소스의 일부를 수정할 때 사용한다.

### 8 DELETE 요청

todos 리소스에서 id를 사용하여 todo를 삭제한다.
