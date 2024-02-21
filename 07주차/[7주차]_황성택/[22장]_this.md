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

### 1. 일반 함수 호출

- 기본적으로 this에는 전역 객체가 바인딩된다.
- strict mode가 적용된 일반 함수 내부의 this에는 `undefined`가 바인딩된다. (일반 함수 내부에서는 this를 사용할 필요가 없다.)

```jsx
function foo() {
  console.log('foo this: ', this); //window
  function bar() {
    console.log('bar this: ', this); //window
  }
  bar();
}
foo();

function foo() {
  'use strict';

  console.log('foo this: ', this); //undefined
  function bar() {
    console.log('bar this: ', this); //undefined
  }
  bar();
}
foo();
```

#### 중첩 함수, 콜백 함수
- 어떤 함수라도 일반 함수로 호출되면 `this`에 전역 객체가 바인딩된다.(중첩 함수, 콜백 함수 포함)

```js
function foo(){
    function bar(){
        console.log(this); // window
    }
}
```

- 그러나 외부 함수의 헬퍼 함수 역할을 하는 중첩 함수나 콜백 함수에 전역 객체가 바인딩 되는것은 문제가 있다. 이때 이렇게 해결할 수 있다.
- 이외에도 `.bind`/`.call`/`.apply` , 화살표 함수를 사용해서도 `this`를 일치시킬 수 있다.

```js
const obj = {
    value: 100,
    foo(){
        const that = this;

        setTimeout(function(){
          console.log(that.value) // 100
        })
        setTimeout(()=> console.log(this.value)) // 100
    }
}

```



### 2. 메서드 호출 

- 메서드 내부의 `this`에서는 **메서드를 호출한 객체**가 바인딩된다.
- 주의할 것은 메서드 내부의 `this`는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다는 것이다.

- 메서드는 프로퍼티에 바인딩된 함수이기 떄문에 함수 객체는 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체이다
- 그래서 다른 객체의 메서드가 될 수도 있고 일반 변수에 할당하여 일반 함수로 호출될 수도 있다.

```js
const person = {
    nams : 'Lee',
    getName() {
        return this.name;
    }
}

const getName = person.getName;
// 이렇게 하면 메서드가 아니라 일반 함수로 실행된다. 
console.log(getName()) // ''
```

### 3. 생성자 함수 호출

- 생성자 함수 내부의 `this`에는 생성자 **함수가 미래에 생성할 인스턴스**가 바인딩된다.
- new 연산자와 함께 생성자 함수를 호출하지 않으면 일반 함수로 동작한다.

```js
function Circle(radius){
    this.radius = radius
    this.getDiameter = function(){
        return 2 * this.radius;
    }
}
const circle1 = new Circle(5);
console.log(circle1.getDiameter()); // 10

const circle2 = Circle(15);
console.log(circl2) // undefined
```

### 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출 

#### apply, call 
- `apply`는 this 바인딩과 인수 리스트 배열을 사용해서 함수를 호출한다.
- `call`은 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수를 호출한다.

```js
Function.prototype.apply(thisArg[, argsArray])
Function.prototype.call(thisArg[, args[, arg2 ...]])
```

- 두 메서드의 본질적인 기능은 함수를 호출하는 것이다. 함수를 호출하면 첫번쨰 인수로 전달한 특정 객체를 호출한 함수에 바인딩한다.

```js
function getThisBinding(){
    return this;
}
const thisArg = {a : 1};

console.log(getThisBinding.apply(thisArg, [1,2,3])) // {a : 1}
console.log(getThisBinding.apply(thisArg, 1,2,3)) // {a : 1}
```

#### bind
- `bind`는 함수를 호출하지 않고, 첫번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 반환한다.
- 중첩 함수 또는 콜백 함수의 `this`가 불일치하는 문제를 해결하기 위해 유용하게 사용된다.

```js
const person = {
    name : 'Lee',
    foo(callback) {
        setTimeout(callback.bind(this), 100);
    }
}

person.foo(function () { // 콜백 함수로 실행됨
    console.log(this.name); // Lee
})
```

📌 함수 호출 방식에 따른 this 바인딩

| 함수 호출 방식  | this 바인딩 |
|---------------------------|-------------------------------------|
| 일반 함수 호출  | 전역 객체 |
| 메서드 호출  | 메서드를 호출한 객체 |
| 생성자 함수 호출  | 생성자 함수가 (미래에) 생성할 인스턴스 |
| apply, call, bind에 의한 간접 호출 | apply, call, bind 메서드에 첫번째 인수로 전달한 객체 |

