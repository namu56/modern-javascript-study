# 43장 Ajax

## 1. Ajax란?

AJAX(Asynchronous JavaScript and XML)

JS로 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신해 웹페이지를 동적으로 갱신하는 프로그래밍 방식

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4a11039e-c298-473f-a4f8-6f269a710858/74956743-3312-4a47-af17-2b65a4ed62bc/Untitled.png)

이전의 웹페이지는 화면이 전환되면 서버로부터 새로운 HTML을 전송받아 웹페이지 전체를 처음부터 다시 렌더링했다.

단점

1. 완전한 HTML을 서버로부터 매번 다시 전송받아 불필ㅇ한 데이터 통신이 발생한다.
2. 변경할 필요 없는 부분까지 처음부터 렌더링 하므로 순간적으로 깜빡이는 현상이 발생한다.
3. 클라이언트와 서버 통신이 동기 방식이기 때문에 응답이 있을 때까지 다음 처리는 블로킹된다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/4a11039e-c298-473f-a4f8-6f269a710858/8ee2dafb-5954-49ab-a092-b5dac79314de/Untitled.png)

AJAX를 사용한 방식

웹페이지의 변경에 필요한 데이터만 비동기 방식으로 전송받아 변경 부분만 한정적으로 렌더링하는 방식

장점

1. 변경할 부분만 통신해 전송받기 때문에 불필요한 통신이 발생하지 않는다.
2. 부분 렌더링을 하므로 순간적으로 화면이 깜빡이는 현상이 발생하지 않는다.
3. 통신이 비동기 방식이기 때문에 블로킹이 발생하지 않는다.

## 2. JSON

**JSON(JavaScript Object Notation)**

HTTP 통신을 위한 텍스트 데이터 포맷, 이름과 달리 JS언어와는 독립적으로, 다른 프로그래밍 언어에서도 사용한다.

### 1) JSON 표기 방식

```jsx
{
  "id": 1,
  "name": "Kim",
  "age": 30,
  "email": "kim@example.com",
  "isSubscribed": true,
  "favorites": ["reading", "traveling", "coding"],
  "address": {
    "street": "123 Maple Street",
    "city": "Seoul",
    "postalCode": "123-456"
  }
}

```

JSON은 JS 객체 리터럴과 같은 문법으로 작성된다.

→ 하지만 순수 텍스트다!

### 2) JSON.stringify

```jsx
const jsonString = JSON.stringify(user);

console.log(jsonString);
```

서버로 객체를 전송할 때 객체를 문자열 하는 메서드이다. → 직렬화

### 3) JSON.parse

```jsx
const user = JSON.parse(jsonString);

console.log(user);
```

문자열화된 JSON 문자열을 다시 객체로 사용하기 위해 파싱하는 메서드
