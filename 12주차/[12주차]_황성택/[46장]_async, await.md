## async/await

- 프로미스를 기반으로 동작하며 프로미스의 then/catch/finally 후속 처리 메서드에 콜백 함수를 전달하여 <br>
비동기 처리 결과를 후속 처리할 필요 없이 마치 동기 처리처럼 프로미스를 사용할 수 있다.

### async 함수

- await 키워드는 반드시 async 함수 내부에서 사용해야 한다.
- async 함수가 명시적으로 프로미스를 반환하지 않더라도 암묵적으로 반환값을 resolve하는 프로미스를 반환한다.

```jsx
// async 함수 선언문
async function foo(n) { return n; }
foo(1).then(console.log); // 1

// async 함수 표현식
const bar = async function(n) { return n; };
bar(2).then(console.log); // 2

// async 화살표 함수
const baz = async (n) => n;
baz(3).then(console.log); // 3

// async 메서드
const obj = {
    async foo(n) { return n; }
};
obj.foo(4).then(console.log); // 4

// async 클래스 메서드
class Qux {
    async qux(n) { return n; }
}
const qux = new Qux();
qux.qux(5).then(console.log); // 5
```

### await 키워드

- 프로미스가 settled 상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다.
- await 키워드는 반드시 프로미스 앞에서 사용해야 한다.

### 에러 처리

- async/await에서 에러 처리는 `try ... catch`문을 사용할 수 있다.
- 콜백 함수를 인수로 전달받는 비동기 함수와는 달리 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 때문에 호출자가 명확하다.
- async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스를 반환한다.
=> async 함수를 호출하고 `Promise.prototype.catch` 후속 처리 메서드를 사용해 에러를 캐치할 수도 있다.
