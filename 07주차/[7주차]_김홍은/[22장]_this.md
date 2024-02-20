### 📎 목차

1. [this 키워드](#1-this-키워드)
2. [함수 호출 방식과 this 바인딩](#2-함수-호출-방식과-this-바인딩)

---

# 22장. this

## 1) `this` 키워드

- **`this`는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수(self-referencing variable)다.**
  - 자바스크립트는 `this`를 제공함으로써 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.
- `this`는 자바스크립트 엔진에 의해 암묵적으로 생성되며, 코드 어디서든 참조할 수 있다.
- 함수를 호출하면 `arguments` 객체와 `this`가 암묵적으로 함수 내부에 전달된다.
- 함수 내부에서 `arguments` 객체를 지역 변수처럼 사용할 수 있는 것처럼 `this`도 지역 변수처럼 사용할 수 있다.
- **`this`가 가리키는 값, 즉 `this` 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.**
  > **`this` 바인딩**
  >
  > - 바인딩 : 식별자와 값을 연결하는 과정
  >   - ex) 변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것
  > - `this` 바인딩 : `this`와 `this`가 가리킬 객체를 바인딩 하는 것
- `this`는 코드 어디에서든지 참조 가능하다. ⇒ 전역에서도, 함수 내부에서도 `this` 참조 가능

  - 전역에서의 `this` ⇒ 전역 객체 `window`가 바인딩
  - 일반 함수 내부에서의 `this` ⇒ 전역 객체 `window`가 바인딩 (단, strict mode에서는 `undefined`가 바인딩)
  - 객체 리터럴의 메서드 내부에서의 `this` ⇒ 메서드를 호출한 객체가 바인딩
  - 생성자 함수 내부의 `this` ⇒ 생성자 함수가 생성할 인스턴스가 바인딩

  ```jsx
  /* ex1) 전역에서의 this ⇒ 전역 객체 window를 가리킴 */
  console.log(this); // window

  /* ex2) 일반 함수 내부에서의 this ⇒ 전역 객체 window를 가리킴 */
  function square(number) {
    console.log(this); // window
    return number * number;
  }
  square(2);

  /* ex3) 객체 리터럴의 메서드 내부에서의 this ⇒ 메서드를 호출한 객체를 가리킴 */
  const person = {
    name: "Lee",
    getName() {
      console.log(this); // {name: 'Lee', getName: ƒ}
      return this.name;
    },
  };
  console.log(person.getName()); // Lee

  /* ex4) 생성자 함수 내부에서 this ⇒ 생성자 함수가 생성할 인스턴스를 가리킴 */
  function Person(name) {
    this.name = name;
    console.log(this); // Person {name: 'Lee'}
  }
  const me = new Person("Lee");
  ```

<br/><br/>

## 2) 함수 호출 방식과 `this` 바인딩

#### **◈ 다양한 함수 호출 방식**

- 일반 함수 호출
- 메서드 호출
- 생성자 함수 호출
- `Function.prototype.apply/call/bind` 메서드에 의한 간접 호출

**⇒ 함수 호출 방식에 따라 `this` 바인딩이 동적으로 결정된다.**

| 함수 호출 방식                                             | this 바인딩                                                            |
| ---------------------------------------------------------- | ---------------------------------------------------------------------- |
| 일반 함수 호출                                             | 전역 객체                                                              |
| 메서드 호출                                                | 메서드를 호출한 객체                                                   |
| 생성자 함수 호출                                           | 생성자 함수가 (미래에) 생성할 인스턴스                                 |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | Function.prototype.apply/call/bind 메서드에 첫 번째 인수로 전달된 객체 |

> **렉시컬 스코프와 `this` 바인딩은 결정 시기가 다르다.**
>
> - 렉시컬 스코프(함수의 상위 스코프를 결정하는 방식) ⇒ 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정
> - `this` 바인딩 ⇒ 함수 호출 시점에 결정됨
>   - 단, 화살표 함수는 `this` 바인딩을 갖지 않으므로, 스코프 체인을 따라 상위 스코프의 `this`를 그대로 참조한다. 따라서, 화살표 함수의 `this`는 함수가 정의된 위치에 의해 결정된다. ⇒ lexical this (26.3.3절 참고)

<br/>

### (1) 일반 함수 호출

- 기본적으로 `this`에는 전역 객체가 바인딩된다.
- **일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 `this`에는 전역 객체가 바인딩된다.** (단, strict mode가 적용된 일반 함수 내부의 `this`에는 `undefined`가 바인딩된다.)

  ```jsx
  // var로 선언된 전역 변수 value는 전역 객체의 프로퍼티가 된다.
  // cf) let이나 const로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.
  var value = 1;

  const obj = {
    value: 100,
    foo() {
      console.log("foo's this:", this); // foo's this: {value: 100, foo: ƒ}
      console.log("foo's this.value:", this.value); // foo's this.value: 100

      // 메서드 내에서 정의한 중첩 함수
      function bar() {
        console.log("bar's this:", this); // bar's this: window
        console.log("bar's this.value:", this.value); // bar's this.value: 1
      }

      // 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면,
      // 중첩 함수 내부의 this에는 전역 객체가 바인딩된다.
      bar();

      // 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
      setTimeout(function () {
        console.log("callback's this:", this); // callback's this: window
        console.log("callback's this.value:", this.value); // callback's this.value: 1
      }, 100);
    },
  };

  obj.foo();
  ```

- 하지만 메서드 내에서 정의한 중첩 함수 또는 메서드에게 전달한 콜백 함수(보조 함수)가 일반 함수로 호출될 때, 메서드 내의 중첩 함수 또는 콜백 함수의 `this`가 전역 객체를 바인딩하는 것은 문제가 있다.
  - 따라서, 메서드 내부의 중첩 함수나 콜백 함수의 `this` 바인딩을 메서드의 `this` 바인딩과 일치키시기 위한 작업이 필요하다.

<br/>

#### **◈ 메서드의 `this` 바인딩과 일치키시기 위한 방법**

- sol 1) `this` 바인딩(`obj`)을 변수 `that`에 할당하여, `that`을 메서드의 `this`인 것처럼 사용한다.

  ```jsx
  var value = 1; // var키워드 전역 변수는 전역 객체의 프로퍼티가 됨 ⇒ window.value = 1

  const obj = {
    value: 100,
    foo() {
      // this 바인딩(obj)을 변수 that에 할당
      const that = this;

      // 콜백 함수 내부에서 this 대신 that을 참조한다.
      setTimeout(function () {
        console.log(that.value); // 100
        console.log(this.value); // 1 (= window.value)
      }, 100);
    },
  };

  obj.foo();
  ```

- sol 2) `this`를 명시적으로 바인딩할 수 있는 `Function.prototype.apply`, `Function.prototype.call`, `Function.prototype.bind` 메서드를 사용하여 `this` 바인딩 일치 (22.2.4절 참고)

  ```jsx
  var value = 1;

  const obj = {
    value: 100,
    foo() {
      // 콜백 함수에 명시적으로 this를 바인딩한다.
      setTimeout(
        function () {
          console.log(this.value); // 100
        }.bind(this),
        100,
      );
    },
  };

  obj.foo();
  ```

- sol 3) 화살표 함수를 사용하여 `this` 바인딩 일치 (26.3절 참고)

  ```jsx
  var value = 1;

  const obj = {
    value: 100,
    foo() {
      // 화살표 함수는 this 바인딩을 갖지 않는다.
      // 따라서, 화살표 함수 내부의 this는 상위 스코프의 this를 그대로 참조한다.
      setTimeout(() => console.log(this.value), 100); // 100
    },
  };

  obj.foo();
  ```

<br/>

### (2) 메서드 호출

- 메서드 내부의 `this`에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표(`.`) 연산자 앞에 기술한 객체가 바인딩 된다.

> 📌 주의!
>
> 메서드 내부의 `this`는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다.

- 메서드는 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체다. ⇒ 따라서, 메서드를 변수에 할당하는 것이 가능하다.
  - 즉, 메서드를 다른 객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될 수도 있고 일반 변수에 할당하여 일반 함수로 호출될 수도 있다.

```jsx
const person = {
  name: "Lee",
  getName() {
    return this.name;
  },
};

const anotherPerson = {
  name: "Kim",
};

// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

//  getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName()); // Kim

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서의 window.name과 같다.
// cf) 브라우저 환경에서의 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값이 ''이다.
// cf) Node.js 환경에서의 this.name은 undefined다.
console.log(getName()); // ''
```

![image](https://github.com/hongii/Book-Shop/assets/93701887/532eb48e-6e1b-4db9-9489-7f58d9fb05ec)

<br/>

### (3) 생성자 함수 호출

- **생성자 함수 내부의 `this`에는 생성자 함수가 생성할 인스턴스가 바인딩된다.**
  - 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 이를 `new` 연산자와 함께 호출 ⇒ 생성자 함수가 호출되어 인스턴스 객체를 생성
  - `new` 연산자 없이 호출 ⇒ 생성자 함수가 아닌 그냥 일반 함수가 호출

```jsx
/* ex1) 생성자 함수로 생성한 인스턴스에서의 this 바인딩 */
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20

/* ex2) 일반 함수로 생성한 객체에서의 this 바인딩 */
// new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수로 동작하지 않는다.
// 즉, 일반 함수의 호출이다.
const circle3 = Circle(15);

// 일반 함수로 호출될 Circle에는 반환문이 없으므로 암묵적으로 undefined를 반환
console.log(circle3); // undefined

// 일반 함수로 호출된 Circle 내부의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

<br/>

### (4) **`Function.prototype.apply/call/bind` 메서드에 의한 간접 호출**

- `apply`, `call`, `bind` 메서드는 `Function.prototype`의 메서드다. ⇒ 즉, 이 메서드들은 모든 함수가 상속받아 사용할 수 있다.

**① `apply`, `call` 메서드**

```jsx
/**
 * 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수를 호출한다.
 * @param thisArg - this로 사용할 객체
 * @param argsArray - 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
 * @returns 호출된 함수의 반환값
 */
Function.prototype.apply(thisArg[, argsArray])

/**
 * 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수를 호출한다.
 * @param thisArg - this로 사용할 객체
 * @param arg1, arg2, ... - 함수에게 전달할 인수 리스트
 * @returns 호출된 함수의 반환값
 */
Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])
```

- 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 `this`에 바인딩한다.
- `apply`, `call` 메서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 `this`로 사용할 객체를 전달하면서 함수를 호출하는 것은 동일하다.
- `apply`, `call` 메서드의 사용 용도 ⇒ `argements`객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우

  ```jsx
  function convertArgsToArray() {
    console.log(arguments);

    // arguments 객체를 배열로 변환
    // Array.prototype.slice를 인수 없이 호출하면 배열의 복사본을 생성
    const arr = Array.prototype.slice.call(arguments); // call 메서드 사용
    // const arr = Array.prototype.slice.apply(arguments); // 또는 apply 메서드 사용
    console.log(arr);

    return arr;
  }

  convertArgsToArray(1, 2, 3); // [1, 2, 3]
  ```

  - 유사 배열 객체는 배열이 아니기 때문에 배열 메서드를 사용할 수 없으나 `apply` 와 `call` 메서드를 이용하면 가능함

**② `bind` 메서드**

- `bind` 메서드는 `apply` 와 `call` 메서드와 달리 함수를 호출하지 않고, 첫 번째 인수로 전달한 값으로 `this` 바인딩이 교체된 함수를 새롭게 생성하여 반환한다.
- `bind` 메서드의 사용 용도 ⇒ 메서드의 `this`와 메서드 내부의 중첩 함수 또는 콜백 함수의 `this`가 불일치 하는 문제 해결하기 위해 사용

```jsx
const person = {
  name: "Lee",
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    setTimeout(callback.bind(this), 100);
  },
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}`); // Hi! my name is Lee
});
```
