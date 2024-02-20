# 📒21장_ 빌트인 객체
### 📑목차
- [표준 빌트인 객체](#표준-빌트인-객체)
- [원시값과 래퍼 객체](#원시값과-래퍼-객체)
- [전역 객체](#전역-객체)

### ⚡Quick Summary
<자바스크립트 객체 분류>
- *표준 빌트인 객체(standard built-in objects/native objects/global objects)*
    > ECMAScript 사양에 정의된 객체
    - 실행 환경에 관계없이 전역 객체의 프로퍼티로 제공됨
    - 전역 변수처럼 언제나 참조 가능
    - 자바스크립트는 [40여 개](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects)의 표준 빌트인 객체 제공

    |표준 빌트인 객체 유형|객체|제공 메서드|특징|
    |--|--|--|--|
    |생성자 함수 객체 ⭕|나머지|프로토타입 메서드, 정적 메서드|인스턴스 생성 가능|
    |생성자 함수 객체 ❌|`Math`, `Reflect`, `JSON`|정적 메서드|인스턴스 생성 불가능|
- *호스트 객체(host objects)*
    > ECMAScript 사양에 정의되어 있지 않지만 자바스크립트 실행 환경에서 추가로 제공하는 객체
    - 브라우저 환경: [클라이언트 사이드 Web API](https://developer.mozilla.org/ko/docs/Web/API)
    - Node.js 환경: [Node.js 고유 API](https://nodejs.org/dist/latest/docs/api/repl.html)
- *사용자 정의 객체(user-defined objects)*

## 📌표준 빌트인 객체
- `Math`, `Reflect`, `JSON`을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체: *프로토타입 메서드* 및 *정적 메서드* 제공
- 생성자 함수 객체가 아닌 표준 빌트인 객체: *정적 메서드*만 제공
```jsx
// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(1.5); // Number {1.5}
console.log(Object.getPrototypeOf(numObj) === Number.prototype); // true

// toFixed는 Number.prototype의 프로토타입 메서드
// Number.prototype.toFixed는 소수점 자리를 반올림하여 문자열로 반환
console.log(numObj.toFixed()); // 2

// isInteger는 Number의 정적 메서드
// Number.isInteger는 인수가 정수(integer)인지 검사하여 그 결과를 Boolean으로 반환
console.log(Number.isInteger(0.5)); // false
```

## 📌원시값과 래퍼 객체
- 원시값은 객체가 아니므로 프로퍼티나 메서드를 가질 수 없는데 어떻게 잘 동작하는 걸까?
    ```jsx
    const str = 'hello';

    // 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작
    console.log(str.length); // 5
    console.log(str.toUpperCase()); // HELLO
    ```
    - 문자열, 숫자, 불리언, 심벌만 가능
    - 그외 원시값인 `null`과 `undefined`는 불가능
    - ✔️*동작 과정*
        1. **문자열**, **숫자**, **불리언** 값에 대해 마침표 표기법 또는 대괄호 표기법으로 객체처럼 접근
        2. 자바스크립트 엔진이 일시적으로 연관된 생성자 함수의 인스턴스(래퍼 객체)를 생성 & 원시값을 `[[StringData]]`, `[[NumberData]]`와 같이 `[[Data]]` 내부 슬롯에 할당
        3. 생성된 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출
        4. 식별자가 `[[Data]]` 내부 슬롯에 할당된 원시값 갖도록 되돌리고, 래퍼 객체는 가비지 컬렉션의 대상이 됨

- ⭐***래퍼 객체(wrapper object)***
    > **문자열**, **숫자**, **불리언** 값에 대해 객체처럼 접근하면 생성되는 임시 객체
```jsx
const str = 'hi';

// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환된다.
console.log(str.length); // 2
console.log(str.toUpperCase()); // HI

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
console.log(typeof str); // string
```
![문자열 래퍼 객체의 프로토타입 체인](https://github.com/namu56/modern-javascript-study/assets/71831926/004a385f-b8ed-47b9-bd1a-0c5c0f789c71)

## 📌전역 객체
> 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않은 최상위 객체
- 지칭하는 이름
    - 브라우저 환경: `window`, `self`, `this`, `frames`
    - Node.js 환경: `global`
    - `globalThis`: ES11에서 도입된 전역 객체를 가리키는 다양한 식별자를 통일한 식별자
        ```jsx
        // 브라우저 환경
        globalThis === this   // true
        globalThis === window // true
        globalThis === self   // true
        globalThis === frames // true

        // Node.js 환경(12.0.0 이상)
        globalThis === this   // true
        globalThis === global // true
        ```
- 전역 객체의 프로퍼티
    - 표준 빌트인 객체
    - 환경에 따른 호스트 객체(클라이언트 Web API, Node.js의 호스트 API)
    - `var` 키워드로 선언한 전역 변수(또는 암묵적 전역)와 전역 함수
- 전역 객체의 프로퍼티 참조 시 `window` 또는 `global`과 같은 지칭 생략 가능
- ✔️*브라우저 환경*의 모든 자바스크립트 코드는 **하나의 전역 객체 window를 공유**

### 빌트인 전역 프로퍼티
- *Infinity*
    > 무한대를 나타내는 숫자값 `Infinity`
    ```jsx
    console.log(window.Infinity === Infinity); // true
    ```
- *NaN*
    > 숫자가 아님(Not-a-Number)을 나타내는 숫자값 `NaN`
    ```jsx
    console.log(Object.is(window.NaN, NaN)); // true
    ```
- *undefined*
    > 원시 타입 `undefined`
    ```jsx
    console.log(window.undefined === undefined); // true
    ```
### 빌트인 전역 함수
- *eval*
    > 자바스크립트 코드를 나타내는 문자열을 인수로 받아서 런타임에 평가하여 실행
    - 기존의 스코프를 런타임에 동적으로 수행
        ```jsx
        const x = 1;

        function foo() {
            // eval 함수는 런타임에 foo 함수의 스코프를 동적으로 수정한다.
            eval('var x = 2;');
            console.log(x); // 2
        }

        foo();
        console.log(x); // 1
        ```
    - ✔️eval 함수를 통해 사용자로부터 입력받은 콘텐츠(untrusted data)를 실행하는 것은 보안에 매우 취약하고, 자바스크립트 엔진에 의해 최적화도 되지 않으므로 사용 금지
- *isFinite*
    > 전달받은 인수에 대해 유한수 여부 확인 결과를 불리언 타입으로 반환
    - `NaN`으로 평가되는 값이면 `false` 반환
    - ✔️`null`은 숫자 타입으로 변환하면 `0`이 되기 때문에 `true` 반환
    ```jsx
    // 인수가 유한수이면 true를 반환한다.
    isFinite(0);    // -> true
    isFinite(2e64); // -> true
    isFinite('10'); // -> true: '10' → 10
    isFinite(null); // -> true: null → 0

    // 인수가 무한수 또는 NaN으로 평가되는 값이라면 false를 반환한다.
    isFinite(Infinity);  // -> false
    isFinite(-Infinity); // -> false
    isFinite(NaN);     // -> false
    isFinite('Hello'); // -> false
    isFinite('2005/12/12'); // -> false
    ```
- *isNaN*
    > 전달받은 인수가 `NaN`인지 검사하여 그 결과를 불리언 타입으로 반환
- *parseFloat*
    > 전달받은 문자열 인수를 부동 소수점 숫자, 즉 실수로 해석하여 반환
    ```jsx
    // 문자열을 실수로 해석하여 반환한다.
    parseFloat('3.14');  // -> 3.14
    parseFloat('10.00'); // -> 10

    // 공백으로 구분된 문자열은 첫 번째 문자열만 변환한다.
    parseFloat('34 45 66'); // -> 34
    parseFloat('40 years'); // -> 40

    // 첫 번째 문자열을 숫자로 변환할 수 없다면 NaN을 반환한다.
    parseFloat('He was 40'); // -> NaN

    // 앞뒤 공백은 무시된다.
    parseFloat(' 60 '); // -> 60
    ```
- *parseInt*
    > 전달받은 문자열 인수를 정수로 해석해서 반환<br>⭐두 번째 인수로 진법을 나타내는 기수(2~36) 전달 가능 (default 10)
    ```jsx
    parseInt(string, radix);
    ```
    - 기수를 지정하여 (문자열 → 숫자) 변환
    - 반대로 기수를 지정하여 (숫자 → 문자열) 변환은 `Number.prototype.toString` 메서드 사용 (default 10진수)
    ```jsx
    const x = 15;

    // 10진수 15를 2진수로 변환하여 그 결과를 문자열로 반환한다.
    x.toString(2); // -> '1111'
    // 문자열 '1111'을 2진수로 해석하고 그 결과를 10진수 정수로 반환한다
    parseInt(x.toString(2), 2); // -> 15
    ```
- *encodeURI*/*decodeURI*
    > 완전한 URI를 encode/decode
    ![URI](https://github.com/namu56/modern-javascript-study/assets/71831926/9fa9133a-6029-4a3d-a653-59faad717175)
- *encodeURIComponent*/*decodeURIComponent*
    > URI 구성 요소(component)를 encode/decode

### 암묵적 전역(implicit global)
> *선언하지 않은 식별자*에 값을 할당하면 **전역 객체의 프로퍼티**가 되어 전역 변수처럼 동작하는 현상
- 예를 들어 자바스크립트 엔진은 `x = 10`을 `window.x = 10`으로 해석하기 때문에 전역 객체에 프로퍼티를 동적 생성하게 됨
- 변수 호이스팅 발생 ❌
- 프로퍼티이기 때문에 delete 연산자로 삭제 가능