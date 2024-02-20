## this

> 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수로, 프로퍼티나 메서드를 참조할 수 있다.


### 🔔 this 바인딩

- 식별자와 값을 연결하는 과정으로, this와 this가 가리킬 객체를 바인딩하는 것이다.
- 자바스크립트의 this는 함수가 호출되는 방식에 따라 this 바인딩이 동적으로 결정된다.

```jsx
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this); // window
  return number * number;
}
square(2);

const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: ƒ}
    return this.name;
  },
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}

const me = new Person('Lee');
```

- strict mode가 적용된 일반 함수 내부의 this에는 `undefined`가 바인딩된다. (일반 함수 내부에서는 this를 사용할 필요가 없다.)





















