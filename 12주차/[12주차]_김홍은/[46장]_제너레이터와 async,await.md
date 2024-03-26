# 46장. 제너레이터와 async/await

- cf) 46.6절만 내용 정리

## 6) `async/await`

- ES8(ECMAScript 2017)에 도입
- 프로미스 기반으로 동작
- 프로미스의 후속 처리 메서드 없이 비동기 처리를 동기 처리처럼 프로미스 처리 결과를 반환하도록 구현할 수 있음

<br/>

### **(1) `async` 함수**

- `await` 키워드는 반드시 `async` 함수 내부에서 사용해야 한다.
- `async` 함수는 `async` 키워드를 사용해 정의하며 언제나 프로미스를 반환한다.
  - `async` 함수가 명시적으로 프로미스를 반환하지 않더라도 `async` 함수는 암묵적으로 반환값을 `resolve`하는 프로미스를 반환
  - 클래스의 `constructor` 메서드는 인스턴스를 반환해야 하므로, 언제나 프로미스를 반환해야 하는 `async` 메서드가 될 수 없다.

```jsx
// async 함수 선언문
async function foo(n) { return n; }
foo(1).then(v => console.log(v)); // 1

// async 함수 표현식
const bar = async function (n) { return n; };
bar(2).then(v => console.log(v)); // 2

// async 화살표 함수
const baz = async n => n;
baz(3).then(v => console.log(v)); // 3

// async 메서드
const obj = {
  async foo(n)  { return n; }
}
obj.foo(4).then(v => console.log(v)); // 4

// async 클래스 메서드
class MyClass {
  async bar(n) { return n; }
}
const myClass = new MyClass();
myClass.bar(5).then(v => console.log(v)); // 5

// 클래스의 constructor는 async 메서드가 될 수 없다.
class MyClass {
  async constructor() {}
	// ❌ SyntaxError: Class constructor may not be an async method
}
const myClass = new MyClass();
```

<br/>

### **(2) `await` 키워드**

- 프로미스가 `settled` 상태가 될 때까지 대기하다가 `settled` 상태가 되면 프로미스가 `resolve`한 처리 결과를 반환
- `await` 키워드는 반드시 프로미스 앞에서 사용해야 한다.
- `await` 키워드는 다음 실행을 일시 중시시켰다가 프로미스가 `settled` 상태가 되면 다시 재개
  - 비동기 처리의 처리 순서가 보장되어야 하는 경우 `await` 키워드를 사용하여 순차적으로 처리 가능
- 단, 모든 프로미스에 `await` 키워드를 사용하는 것은 주의해야 한다.
  ```jsx
  // ex) 서로 연관 없는 프로미스 3개를 수행하는 foo 함수
  async function foo(){
  	const a = await new Promise(resolve => setTimeout(() => resolve(1), 3000));
  	const b = await new Promise(resolve => setTimeout(() => resolve(2), 2000));
  	const c = await new Promise(resolve => setTimeout(() => resolve(3), 1000));

  	console.log([a, b, c]); // [1, 2, 3]
  }
  foo(); // 약 6초 소요

  async function foo(){
  	const res = await Promise.all([
  	new Promise(resolve => setTimeout(() => resolve(1), 3000)),
  	new Promise(resolve => setTimeout(() => resolve(2), 2000)),
  	new Promise(resolve => setTimeout(() => resolve(3), 1000))

  	console.log(res); // [1, 2, 3]
  }
  foo(); // 약 3초 소요
  ```
  - 위의 예제에서 `foo` 함수가 수행하는 3개의 비동기 처리는 서로 연관 없이 개별로 수행되는 비동기 처리이므로, 앞선 비동기 처리가 완료될 때까지 대기해서 순차적으로 처리할 필요가 없다. ⇒ 두 번째 예시 코드처럼 작성
  - 만약 앞선 비동기 처리의 결과를 가지고 다음 비동기 처리를 수행해야 할 경우엔 모든 프로미스에 `await` 키워드를 사용하여 순차적으로 처리하도록 해주면 된다. ⇒ 첫 번째 예시코드처럼 작성

<br/>

### (3) 에러 처리

- `async`/`await`에서 에러 처리는 `try...catch` 문을 사용할 수 있다.
  - 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 때문에 호출자가 명확하기 때문
- `async` 함수 내에서 `catch`문을 사용해서 에러 처리를 하지 않으면 `async` 함수는 발생한 에러를 `reject`하는 프로미스를 반환한다.
  - 따라서, `async` 함수를 호출하고 `Promise.prototype.catch` 후속 처리 메서드를 사용해 에러를 캐치할 수도 있다.
