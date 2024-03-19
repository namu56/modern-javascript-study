### 📎 목차

1. [Ajax란?](#1-ajax란)
2. [JSON](#2-json)

---

# 43장. Ajax

## 1) Ajax란?

- **Ajax(Asynchronous JavaScript and XML)** 란, 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식
- Ajax는 브라우저가 제공하는 Web API인 `XMLHttpRequest` 객체를 기반으로 동작
  - `XMLHttpRequest` : HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공

<br/>

#### **◈ Ajax가 등장하기 이전의 웹 페이지 동작 방식(전통적인 방식)**

![6](https://github.com/hongii/book-shop-fe/assets/93701887/78e5104d-df8c-4073-888f-26ebc790ac30)

- html 태그로 시작해서 html 태그로 끝나는 완전한 HTML을 서버로부터 전송받아 웹 페이지 전체를 처음부터 다시 렌더링 하는 방식으로 동작
- 화면이 전환될 경우, 서버로부터 새로운 HTML을 전송받아 웹페이지 전체를 처음부터 다시 렌더링
- **전통적인 방식의 단점**
  - 이전 웹페이지와 차이가 없어서 변경할 필요가 없는 부분까지 포함된 완전한 HTML을 서버로부터 매번 다시 전송받기 때문에 불필요한 데이터 통신이 발생
  - 변경할 필요가 없는 부분까지 처음부터 다시 렌더링하여 화면 전환이 일어나면서 화면이 순간적으로 깜박이는 현상 발생
  - 클라이언트와 서버와의 통신이 동기 방식으로 동작하기 때문에 서버로부터 응답이 있을 때까지 다음 처리 블로킹

<br/>

#### **◈ Ajax 등장 이후의 웹 페이지 동작 방식**

![7](https://github.com/hongii/book-shop-fe/assets/93701887/ab805ad5-07c0-4f29-a756-19f1d88a5943)

- Ajax의 등장으로 서버로부터 웹페이지의 변경에 필요한 데이터만 비동기 방식으로 전송받아 웹페이지를 변경할 필요가 없는 부분은 다시 렌더링 하지 않고, 변경할 필요가 있는 부분만 한정적으로 렌더링하는 방식이 가능해짐
- 이를 통해 브라우저에서도 빠른 퍼포먼스와 부드러운 화면 전환이 가능
- **Ajax 방식의 장점**
  - 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않음
  - 변경할 필요가 없는 부분은 다시 렌더링하지 않으므로 화면이 순간적으로 깜박이는 현상이 발생하지 않음
  - 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않음

<br/><br/>

## 2) JSON

- **JSON(JavaScript Object Notation) :** 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷. 자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷 ⇒ 대부분의 프로그래밍 언어에서 사용 가능

### (1) JSON 표기 방식

```json
{
  "name": "Lee",
  "age": "20",
  "alive": "true",
  "hobby": ["traveling", "tennis"]
}
```

- 키와 값으로 구성된 순수한 텍스트
- JSON의 키는 반드시 큰따옴표(작은따옴표 사용 불가)로 묶어야 한다.
- JSON의 값은 자바스크립트 객체 리터럴과 같은 표기법을 그대로 사용할 수 있으나, 문자열의 경우 반드시 큰따옴표로 묶어야 한다.

<br/>

### (2) **`JSON.stringify`**

- `JSON.stringify` 메서드는 객체를 JSON 포맷의 문자열로 변환한다. (객체 뿐만 아니라 배열도 변환 가능)
- 직렬화(serializing) : 객체를 문자열화하여 클라이언트에서 서버로 데이터를 전송 가능한 형태로 만드는 것

```jsx
const obj = {
  name: "Lee",
  age: 20,
  alive: true,
  hobby: ["traveling", "tennis"],
};

// ex1) 객체를 JSON 포맷의 문자열로 변환
const json = JSON.stringify(obj);
console.log(typeof json, json);
// string {"name":"Lee","age":20,"alive":true,"hobby":["traveling","tennis"]}

// ex2) 공백과 들여쓰기를 추가 ⇒ 객체를 JSON 포맷의 문자열로 변환하면서 들여쓰기
const prettyJson = JSON.stringify(obj, null, 2);
console.log(typeof prettyJson, prettyJson);
/*
string {
  "name": "Lee",
  "age": 20,
  "alive": true,
  "hobby": [
    "traveling",
    "tennis"
  ]
}
*/

// ex3) 특정 속성 제외: replacer 매개변수(두 번째 매개변수)를 사용하여 특정 속성을 제외
function filter(key, value) {
  // 값이 타입이 Number일 경우 필터링되어 반환되지 않음
  // undefined: 반환하지 않음
  return typeof value === "number" ? undefined : value;
}

const strFilteredObject = JSON.stringify(obj, filter, 2);
console.log(typeof strFilteredObject, strFilteredObject);
/* value가 number 타입인 age 속성은 제거됨
string {
  "name": "Lee",
  "alive": true,
  "hobby": [
    "traveling",
    "tennis"
  ]
}
*/
```

<br/>

### (3) **`JSON.parse`**

- `JSON.parse` 메서드는 JSON 포맷의 문자열을 객체로 변환한다.
- 역직렬화(deserializing) : 서버에서 클라이언트로 전송된 JSON 포맷의 문자열을 객체화하는 것
- 배열이 JSON 포맷의 문자열로 변환되어 있는 경우, `JSON.parse`는 문자열을 배열 객체로 변환한다.
  - 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환한다.

```jsx
const todos = [
  { id: 1, content: "HTML", completed: false },
  { id: 2, content: "CSS", completed: true },
  { id: 3, content: "JavaScript", completed: false },
];

// 배열을 JSON 포맷의 문자열로 변환
const todoJson = JSON.stringify(todos);

// JSON 포맷의 문자열을 배열로 변환 ⇒ 배열의 요소까지 객체로 변환됨
const todoParsed = JSON.parse(todoJson);
console.log(typeof todoParsed, todoParsed);
/*
object [
  { id: 1, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'JavaScript', completed: false }
]
*/
```
