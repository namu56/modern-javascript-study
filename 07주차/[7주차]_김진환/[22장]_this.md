# 22장 this

### 1. this 키워드

`this` 는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 **자기 참조 변수**

`this` 를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

이때, `this` 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.

1. 전역 컨텍스트

```jsx
console.log(this); // 전역 객체 (브라우저 환경에서는 window)
```

2. 함수 블럭 내부

```jsx
function showThis() {
  console.log(this);
}

showThis(); // 전역 객체 또는 undefined (strict mode에서
```

3. 메서드 내부

```jsx
const obj = {
  prop: "I am a property",
  showThis: function () {
    console.log(this.prop); // 현재 객체의 프로퍼티에 접근
  },
};

obj.showThis(); // 'I am a property'
```

4. 이벤트 핸들러 내부

```jsx
<button id="myButton">Click me</button>
<script>
  document.getElementById('myButton').addEventListener('click', function() {
    console.log(this); // 클릭한 버튼 요소
  });
</script>
```

5. 콜백함수 내부

```jsx
const myObject = {
  data: "Hello",
  processData: function (callback) {
    callback();
  },
};

function callbackFunction() {
  console.log(this.data); // undefined 또는 에러 (strict mode에서)
}

myObject.processData(callbackFunction);
```

6. 화살표 함수

```jsx
function regularFunction() {
  console.log(this); // 전역 객체 또는 undefined (strict mode에서)
}

const arrowFunction = () => {
  console.log(this); // 전역 객체 또는 상위 스코프의 this (렉시컬 스코프)
};

regularFunction();
arrowFunction();
```

| 함수 호출 방식                                             | this 바인딩                              |
| ---------------------------------------------------------- | ---------------------------------------- |
| 일반 함수 호출                                             | 전역 객체                                |
| 메서드 호출                                                | 메서드를 호출한 객체                     |
| 생성자 함수 호출                                           | 생성자 함수가 생성할 인스턴스            |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | 해당 메서드에 첫 번째 인수로 전달한 객체 |
