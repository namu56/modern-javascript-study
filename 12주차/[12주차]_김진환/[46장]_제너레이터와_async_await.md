# 46장 제너레이터와 async/await

## 46.1 제너레이터란?

ES6에 도입된 제너레이터는 코드 블럭의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 **함수**다. 제너레이터와 일반 함수의 차이는 다음과 같다.

---

1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
2. 제너레이터 함수는 함수호출자와 함수의 상태를 주고받을 수 있다.
3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.

---

## 46.2 제너레이터 함수의 정의

제너레이터 함수는 function\* 키워드로 선언한다. 그리고 하나 이상의 yield 표현식을 포함한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a426d739-80d6-4ae6-9002-2284f586e1e5/Untitled.png)

화살표함수 불가

new 연산자와 함께 생성자 함수로 호출 불가

## 46.3 제너레이터 객체

제너레이터 함수 호출 시 일반 함수처럼 함수 코드 블럭을 실행하는 것이 아니라

제너레이터 객체를 생성해 반환한다.

제너레이터 함수가 반환한 제너레이터 객체는 이터러블 하면서 동시에 이터레이터다.

Symbol.iterator 메서드를 상속받는 이터러블이면서

value, done 프로퍼티를 갖고 next 메서드를 소유하는 이터레이터다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9c50a25b-1f59-48ca-a9a2-20481d0b420f/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/91ae193d-e105-4873-95b4-9460f83ec66f/Untitled.png)

- next : yield 표현식까지 코드 블럭을 실행하고, yield된 값을 value 프로퍼티 값

  false를 done 프로퍼티 값으로 갖는 이터레이터 result 객체를 반환한다.

- return : 인수로 전달받은 값을 value 프로퍼티 값으로,
  true를 done 프로퍼티 값으로 같은 result객체를 반환한다.
  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1b2c5829-9eee-4f78-a922-adf017d2f1b9/Untitled.png)

next 메서드를 호출하면 yield 표현식이 실행되고 일시중지된다.

이때 함수의 제어권이 generator로 양도(yield)된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/52038f89-e466-448f-94a0-f935be2e6327/Untitled.png)

제너레이터는 반환값에 의미가 없기 때문에 return은 종료의 의미만으로 사용한다.

## 46.5 제너레이터의 활용

### 1. 이터러블의 구현

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ac91f27e-182d-49e8-a9e9-c5b435fffd29/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/31cc2f70-5ca1-409d-8e50-f779acfbf5f3/Untitled.png)

### 2. 비동기 처리

제너레이터 함수는 next와 yield를 통해 then/catch/finally 없이 비동기 처리 결과를 반환하도록 구현할 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b26d085c-b612-4d18-bea7-cca9b8dae8eb/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/80dcf828-9fdc-4050-bc95-78e163d826e5/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9aab3150-74aa-4c52-b02e-d2c13d19c3ee/Untitled.png)

사실 완전하지 않음

→ 제너레이터 실행기가 굳이 필요하면 co 라이브러리를 사용할 수 있다.

async/await 으로 사용함

## 46.6 async/await

제너레이터를 통한 비동기처리는 가독성이 나쁨

→ async/await 등장

프로미스 기반으로 작동한다.

프로미스 후속처리 메서드 없이 마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/94e55d29-93a1-4a25-9d40-7d9aedebd88d/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5684ae67-c098-46ec-83d4-8fedbb0ff274/Untitled.png)

### 1. async 함수

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fe155b42-df45-40e4-891a-16ecebdd2c40/Untitled.png)

async 키워드를 통해 정의하고,

명시적으로 프로미스를 반환하지 않아도 암묵적으로 반환값을 resolve하는 프로미스를 반환한다.

따라서 인스턴스를 반환하는 클래스의 constructor 메서드가 될 수 없다.

### 2. await 키워드

await 키워드는 프로미스가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다.

await 키워드는 반드시 프로미스 앞에서 사용해야 한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8f5465bd-b1d2-4983-b51f-94eb0fb5dac5/Untitled.png)

fetch 함수가 반환한 프로미스가 settled될 때까지 대기 후

settled 상태가 되면 프로미스가 resolve한 처리 결과가 res 변수에 할당된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2b13d052-3705-4852-9b85-56405abb6db5/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ddd65d43-3930-4a65-8619-f46c8150bd80/Untitled.png)

await을 따로 처리하면 순차적으로 진행되지만

작업 순서의 연관이 없으므로 하나의 키워드로 프로미스를 처리하면 병렬적으로 처리될 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a6471192-9481-4cb6-8779-72093c308570/Untitled.png)

순서가 필요한 경우

### 3. 에러처리

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/182b62df-fc68-45e5-a574-0d87cee677b4/Untitled.png)

비동기 함수의 콜백함수를 호출한 것은 비동기함수가 아니기 때문에 try…catch로 에러처리를 할 수 없다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2271e08f-699e-4a81-aac7-8c3b8b4f7c57/Untitled.png)

프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 때문에 호출자가 명확하다.

async 함수 내에서 catch문을 사용해 에러처리를 하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스를 반환한다.
