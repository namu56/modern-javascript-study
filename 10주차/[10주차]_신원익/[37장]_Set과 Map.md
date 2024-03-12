# Set과 Map

## Set

-   Set 객체는 중복되지 않는 유일한 값들의 집합
-   동일한 값을 중복 포함할 수 없고, 요소 순서에 의미가 없으며, 인덱스로 요소에 접근할 수 없다
-   Set은 수학적 집합을 구현하기 위한 자료구조다

### Set 객체의 생성

-   Set 생성자 함수로 생성 (`new Set()`)
-   이터러블을 인수로 전달받아 Set 객체를 생성할 수 있다
-   이 때, 중복된 요소는 저장되지 않는다

### 요소 개수 확인

-   `Set.prototype.size` 프로퍼티를 사용
-   size 프로퍼티는 요소 개수를 변경할 수 없다

### 요소 추가

-   `Set.prototype.add` 메서드 사용
-   메서드 연속 호출 가능
-   객체나 배열과 같이 자바스크립트의 모든 값을 요소로 저장할 수 있다

### 요소 존재 여부 확인

-   `Set.prototype.has` 메서드 사용

### 요소 삭제

-   `Set.prototype.delete` 메서드 사용
-   삭제 성공 여부를 나타내는 불리언 값을 반환
-   연속 호출할 수 없다

### 요소 일괄 삭제

-   `Set.prototype.clear` 메서드 사용
-   clear 메서드는 언제나 undefined를 반환한다

### 요소 순회

-   `Set.prototype.forEach` 메서드 사용
-   인수를 전달할 수 있다
    -   첫 번째 인수: 현재 순회 중인 요소값
    -   두 번째 인수: 현재 순회 중인 요소값
    -   세 번째 인수: 현재 순회 중인 Set 객체 자체
-   Set 객체는 이터러블이다.

### 집합 연산

-   Set 객체를 통해 교집합, 합집합, 차집합 등을 구현할 수 있다

#### 교집합

```js
Set.prototype.intersection = function (set) {
    return new Set([...this].filter((v) => set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}

// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

#### 합집합

```js
Set.prototype.union = function (set) {
    return new Set([...this, ...set]);
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}

// setB와 setA의 합집합
console.log(setB.union(setA)); // Set(4) {1, 2, 3, 4}
```

#### 차집합

```js
Set.prototype.difference = function (set) {
    return new Set([...this].filter((v) => !set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.difference(setB)); // Set(2) {1, 3}

// setB와 setA의 합집합
console.log(setB.difference(setA)); // Set(0) {}
```

#### 부분 집합과 상위 집합

```js
// this가 subset의 상위 집합인지 확인
Set.prototype.isSuperset = function (subset) {
    const supersetArr = [...this];
    return [...subset].every((v) => supersetArr.includes(v));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true

// setB와 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
```

## Map

-   Map 객체는 키와 값의 쌍으로 이루어진 컬렉션이다
-   객체와 유사하지만 차이가 있다
    -   키로 객체를 포함한 모든 값을 사용할 수 있다
    -   이터러블이다
    -   요소 개수를 확인하기 위해 size 프로퍼티를 사용한다

### Map 객체의 생성

-   Map 생성자 함수 `new Map()`를 사용한다
-   이터러블을 인수로 전달받을 수 있다
-   인수로 전달된 이터러블은 키와 값의 쌍우로 이루어진 요소로 구성되어야 한다.
-   중복된 키를 갖는 요소가 존재하면 값이 덮어써진다

### 요소 개수 확인

-   `Map.prototype.size` 프로퍼티 사용

### 요소 추가

-   `Map.prototype.set` 메서드 사용
-   메서드 연속 호출 가능
-   Map 객체는 키 타입에 제한이 없다

### 요소 취득

-   `Map.prototype.get` 메서드 사용

### 요소 존재 여부 확인

-   `Map.prototype.has` 메서드 사용

### 요소 삭제

-   `Map.prototype.delete` 메서드 사용
-   삭제 성공 여부를 나타내는 불리언 값을 반환
-   연속 호출할 수 없다

### 요소 일괄 삭제

-   `Map.prototype.clear` 메서드 사용
-   clear 메서드는 언제나 undefined를 반환한다

### 요소 순회

-   `Map.prototype.forEach` 메서드를 사용한다
-   forEach 메서드의 콜백 함수에 인수를 전달할 수 있다

    -   첫 번째 인수: 현재 순회 중인 요소값
    -   두 번째 인수: 현재 순회 중인 요소키
    -   세 번째 인수: 현재 순회 중인 Map 객체 자체

-   Map 객체는 이터러블이다
-   Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다
