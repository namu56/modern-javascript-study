# 📌 요약 정리

## async/await

-   비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있다
-   제너레이터보다 간단하고 가동성이 높다
-   프로미스를 기반으로 동작하지만, 프로미스의 후속 처리 메서드 없이 처리 결과를 반환하도록 구현할 수 있다

### async 함수

-   암묵적으로 Promise로 감싸지며, resolve됐을 때의 결과 값을 반환한다
-   클래스의 constructor 메서드는 인스턴스를 반환하기 때문에, async 메서드가 될 수 없다

### await 키워드

-   반드시 async 함수 내부에서 사용해야 한다
-   settled 상태가 될 때까지 대기 → setted 상태가 되면 프로미스가 resolve한 처리 결과를 반환

### 에러 처리

-   try ... catch 문을 사용한다
-   async 함수 내에서 명시적인 에러 처리가 없다면 reject하는 프로미스를 반환한다
-   `Unhandled Promise Rejection` 상태가 되고, 브라우저에서는 경고 메시지가 뜨고, Node.js 환경에서는 promise 객체의 unhandledRejection 이벤트를 발생시킨다

---

# acync/await

-   제너레이터를 사용하여 비동기 처리를 동기 처리처럼 동작하도록 구현하는 것은 장황하고 가독성이 나쁨
-   제너레이터보다 간단하고 가독성 좋게 작성할 수 있는 async/await 도입
-   async/await은 프로미스를 기반으로 동작
-   프로미스의 후속 처리 메서드 없이 마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현

## async 함수

-   await 키워드는 반드시 async 함수 내부에서 사용해야 한다
-   언제나 프로미스 반환
-   명시적으로 프로미스를 반환하지 않더라도 암묵적으로 resolve하는 프로미스를 반환
-   클래스의 constructor 메서드는 인스턴스를 반환해야하기 때문에, async 메서드가 될 수 없다

```js
async foo(n) {
    return n;
}

foo(1).then(v => console.log(v)); // 1


```

## await

-   settled 상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환
-   반드시 프로미스 앞에서 사용해야 한다

```js
const fetch = require('node-fetch');

const getGithubuserName = async (id) => {
    const res = await fetch(`https://api.github.com/users/${id}`); // fetch 함수가 반환한 프로미스가 settled 상태가 될 때까지 대기
    const { name } = await res.json(); // 프로미스가 settled 상태가 되면 resolve한 처리 결과가 res 변수에 할당
    console.log(name);
};

getGithubUserName('ungmo2');
```

-   서로 연관없는 비동기 처리들은 비동기 처리들이 완료될 때까지 대기하여 순차적으로 처리할 필요 없다

```js
async function foo() {
    const a = new Promise((resolve) => setTimeout(() => resolve(1), 3000));
    cosnt b = new Promise((resolve) => setTimeout(() => resolve(2), 2000));
    const c = new Promise((resolve) => setTimeout(() => resolve(3), 1000));

    console.log(res); // [1, 2, 3]
}

foo(); // 약 6초 소요


async function foo() {
    const res = await Promise.all([
        new Promise((resolve) => setTimeout(() => resolve(1), 3000)),
        new Promise((resolve) => setTimeout(() => resolve(2), 2000)),
        new Promise((resolve) => setTimeout(() => resolve(3), 1000)),
    ]);
    console.log(res); // [1, 2, 3]
}

foo(); // 약 3초 소요
```

## 에러 처리

-   에러는 콜 스택의 아래 방향으로 전파된다
-   비동기 함수를 호출한 것은 비동기 함수가 아니기 때문에, try ... catch문을 사용해 에러를 캐치할 수 없다
-   async/await 에서 에러 처리는 try ... catch 문을 사용할 수 있다
-   프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 때문에 호출자가 명확하다

    ```js
    const fetch = require('node-fetch');

    const foo = async () => {
        try {
            const wrongUrl = 'http://wrong.url';

            const response = await fetch(wrongUrl);
            const data = await response.json();
            conssole.log(data);
        } catch (err) {
            console.error(err);
        }
    };

    foo();
    ```

-   try ... catch 문을 사용하지 않더라도 async 함수는 발생한 에러를 reject하는 프로미스를 반환한다

    ```js
    const fetch = require('node-fetch');

    const foo = async () => {
        const wrongUrl = 'http://wrong.url';

        const response = await fetch(wrongUrl);
        const data = await response.json();
        return data;
    };

    foo().then(console.log).catch(console.error);
    ```
