# this

## 1) this 키워드

*this*란, 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.
*this*를 통해, 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다
<br>
<br>
*this*의 특징은 다음과 같다

-   객체 리터럴의 메서드 내부에서의 *this*는 메서드를 호출한 객체를 가리킨다

    ```js
    const circle = {
        radius: 5,
        getDiameter() {
            // this는 메서드를 호출한 객체를 가리킨다
            return 2 * this.radius;
        },
    };

    console.log(circle.getDiameter()); // 10
    ```

-   생성자 함수 내부의 *this*는 생성자 함수가 생성할 인스턴스를 가리킨다

    ```js
    function Circle(radius) {
        this.radius = radius,
    };

    Circle.prototype.getDiameter = function () {
        // this는 생성자 함수가 생성할 인스턴스를 가리킨다
        return 2 * this.radius;
    }

    const circle = new Circle(5);
    console.log(circle.getDiameter()); // 10
    ```

-   위와 같이 자바스크립트는 함수가 호출되는 방식에 따라 _this_ 바인딩이 동적으로 결정된다

    > 참고: 자바나 C++ 같은 클래스 기반 언어에서 *this*는 언제나 클래스가 생성하는 인스턴스를 가리킨다

-   *this*는 코드 어디에서든 참조 가능하다
-   strict mode가 적용된 일반 함수 내부의 *this*에는 undefined가 바인딩된다

## 2) 함수 호출 방식과 this 바인딩

_this_ 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.

> 참고 : 렉시컬 스코프와 _this_ 바인딩은 결정 시기가 다르다. 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정하고, _this_ 바인딩은 함수 호출 시점에 결정된다

동일한 함수도 다양한 방식으로 호출할 수 있는데, 함수 호출 방식에 따라 _this_ 바인딩이 어떻게 결정되는지 알아보자

### 2-1) 일반 함수 호출

기본적으로 *this*는 전역 객체가 바인딩된다

> 일반 함수란, 단순히 함수 선언이나 함수 표현식을 사용해 정의된 함수를 말한다

<br>
<br>

일반 함수를 호출했을 때, _this_ 바인딩 예시는 다음과 같다

-   중첩 함수를 일반 함수로 호출했을 때, 중첩 함수 내부의 *this*에는 전역 객체가 바인딩된다
    ```js
    function foo() {
        console.log("foo's this: ", this); // window
        function bar() {
            console.log("bar's this: ", this); // window
        }
        bar();
    }
    foo();
    ```
-   메서드 내에서 정의한 중첩 함수도 마찬가지다
-   콜백 함수가 알반 함수로 호출된다면, 콜백 함수 내부의 *this*에도 전역 객체가 바인딩된다

    ```js
    var value = 1;

    const obj = {
        value: 100,
        foo() {
            console.log("foo's this: ", this); // {value: 100, foo: f}
            // 콜백 함수 내부으 ㅣthis에는 전역 객체가 바인딩된다
            setTimeout(function () {
                console.log("callback's this: ", this); // window
                console.log("callback's this.value: ", this.value); // 1
            });
        },
    };
    ```

메서드 내부의 중첩 함수나 콜백 함수의 _this_ 바인딩을 메서드의 _this_ 바인딩과 일치시키기 위한 방법은 다음과 같다

-   _this_ 바인딩을 변수에 할당한 후, 그 변수를 this 대신 참조한다

    ```js
    var value = 1;
    const obj = {
        value: 100,
        foo() {
            // this 바인딩(obj)을 변수 that에 할당
            const that = this;

            // 콜백 함수 내부에서 this 대신 that을 참조
            setTimeout(function () {
                console.log(that.value); // 100
            }, 100);
        },
    };
    obj.foo();
    ```

-   Function prototye은 *this*를 명시적으로 바인딩할 수 있는 메서드를 사용할 수 있다(apply, call, bind)
-   화살표 함수를 사용해서 *this*을 일치시킬 수 있다

    ```js
    var value = 1;

    const obj = {
        value: 100,
        foo() {
            // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다
            setTimeout(() => console.log(this.value), 100); // 100
        },
    };

    obj.foo();
    ```

### 2-1) 메서드 호출

메서드 내부의 *this*에는 메서드를 호출한 객체가 바인딩된다.
<br><br>
메서드 내부의 _this_ 바인딩 시, 주의할 점은 다음과 같다.

-   메서드는 프로퍼티에 바인딩된 함수이기 때문에, 메서드는 객체에 포함된 것이 아니라, 독립적으로 존재하는 별도의 객체다
    ![image](https://github.com/namu56/modern-javascript-study/assets/107787137/111c416e-08a6-4147-ae3f-aef0eabf7718)

-   메서드는 다른 객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될 수도 있고, 일반 변수에 할당하여 일반 함수로 호출될 수도 있다

    ```js
    const person = {
        name: 'Lee',
        getName() {
            return this.name;
        }
    }

    const anotherPerson = {
        name: 'Kim';
    };

    // getName 메서드를 anotherPerson 객체의 메서드로 할당
    anotherPerson.getName = person.getName;

    // getName 메서드를 호출한 객체는 anotherPerson
    console.log(anotherPerson.getName()); // Kim

    // getName 메서드를 변수에 할당
    const getName = person.getName;

    // getName 메서드를 일반 함수로 호출
    console.log(getName()) // '' 일반함수로 호출했기 때문에, this.name은 전역객체의 name과 같다
    ```

    -   따라서 메서드 내부의 *this*는 프로퍼티로 메서드를 가리키고 있는 객체와는 관계가 없고, 메서드를 호출한 객체에 바인딩된다
        ![image](https://github.com/namu56/modern-javascript-study/assets/107787137/8e974b91-c68b-4bfb-b7b3-a7d010411e57)

### 2-3) 생성자 함수 호출

생성자 함수 내부의 *this*에는 생성자 함수가 생성할 인스턴스가 바인딩된다

```js
function Circle(radius) {
    //  생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

### 2-4) Function prototype의 apply/call/bind 메서드에 의한 간접 호출

apply, call, bind 메서드는 Function prototype의 메서드고, 명시적으로 _this_ 값을 설정하기 위해 사용한다. 이들 메서드는 모든 함수가 상속받아 사용할 수 있다
<br>
<br>
사용법은 다음과 같다

-   apply와 call 메서드

    -   첫번째 인자로 *this*로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출한다
    -   apply 메서드는 인수를 배열 형태로, call 메서드는 인수를 쉼표로 구분한 리스트 형식으로 전달한다

        ```js
        function getThisBinding() {
            console.log(arguments);
            return this;
        }

        // this로 사용할 객체
        const thisArg = { a: 1 };

        console.log(getThisBinding.apply(thisArg, [1, 2, 3]));

        console.log(getThisBinding.call(thisArg, 1, 2, 3));
        ```

    -   apply와 call 메서드는 대표적으로 arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용할 때 사용된다

        > 유사 배열 객체란, 배열처럼 인덱스로 접근할 수 있고 length 속성을 가지지만, 배열의 모든 메서드를 사용할 수 없는 객체를 말한다

        ```js
        function convertArgsToArray() {
            console.log(arguments);

            // arguments 객체를 배열로 변환
            // slice를 인수 없이 호출하면 배열의 복사본을 생성한다
            const arr = Array.prototype.slice.call(arguments);
            console.log(arr);

            return arr;
        }

        convertArgsToArray(1, 2, 3); // [1, 2, 3]
        ```

        -   Array prototype은 함수 객체가 아니지만, Array prototype에 정의된 메서드들은 함수 객체이기 때문에, Function prototype의 메서드를 사용할 수 있다

-   bind 메서드

    -   첫 번째 인수로 전달한 값으로 _this_ 바인딩이 교체된 함수를 새롭게 생성해 반환한다
    -   apply와 call 메서드와 달리 함수를 호출하지 않는다

        ```js
        // getThisBinding 함수를 새롭게 생성해 반환한다
        console.log(getThisBinding.bind(thisArg)); // getThisBinding

        // bind 메서드는 함수를 호출하지는 않으므로, 명시적으로 호출해야 한다
        console.log(getThisBinding.bind(thisArg)()); // {a: 1}
        ```

    -   메서드의 *this*와 메서드 내부의 _this_ 불일치 문제를 해결하기 위해 사용된다
