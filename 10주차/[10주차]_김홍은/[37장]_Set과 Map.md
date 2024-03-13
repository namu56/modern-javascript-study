## 📎 목차

1. [Set](#1-set)
2. [Map](#2-map)

---

# 37장. Set과 Map

## 1) `Set`

- **`Set` 객체** ⇒ 중복되지 않는 유일한 값들의 집합. 배열과 유사
- `Set`을 통해 교집합, 합집합, 차집합, 여집합 등을 구현할 수 있다.

| 구분                                 | 배열 | Set 객체 |
| ------------------------------------ | ---- | -------- |
| 동일한 값을 중복하여 포함할 수 있다. | ⭕   | ❌       |
| 요소 순서에 의미가 있다.             | ⭕   | ❌       |
| 인덱스로 요소에 접근할 수 있다.      | ⭕   | ❌       |

<br/>

### (1) `Set` 객체의 생성

- **`Set` 생성자 함수로 `Set` 객체를 생성한다.**
  - `Set` 생성자 함수에 인수를 전달하지 않으면 빈 `Set` 객체가 생성된다.
  ```jsx
  const emptySet = new Set();
  console.log(emptySet); // Set(0) {size: 0}
  ```
- `Set` 생성자 함수는 이터러블을 인수로 전달받아 `Set` 객체를 생성한다.

  - 이때 이터러블의 중복된 값은 `Set` 객체에 요소로 저장되지 않는다.

  ```jsx
  const set1 = new Set([1, 2, 3, 3]);
  console.log(set1); // Set(3) {1, 2, 3}

  const set2 = new Set("hello");
  console.log(set2); // Set(4) {'h', 'e', 'l', 'o'}
  ```

- 중복을 허용하지 않는 `Set` 객체의 특성을 이용하여 배열에서 중복된 요소를 제거할 수 있다.

  ```jsx
  const numbers = [2, 1, 2, 3, 4, 3, 4];

  // 배열의 중복 요소 제거
  const uniqArray = (array) => array.filter((v, i, self) => self.indexOf(v) === i);
  console.log(uniqArray(numbers)); // (4) [2, 1, 3, 4]

  // Set을 이용한 배열의 중복 요소 제거
  const uniqSet = (array) => [...new Set(array)];
  console.log(uniqSet(numbers)); // (4) [2, 1, 3, 4]
  ```

<br/>

### (2) 요소 개수 확인

- **`Set.prototype.size` 프로퍼티를 사용하여 `Set` 객체의 요소 개수를 확인한다.**
- `size` 프로퍼티는 `setter` 없이 `getter` 함수만 존재하는 접근자 프로퍼티다. ⇒ `size` 프로퍼티에 숫자를 할당하여 `Set` 객체의 요소 개수를 변경할 수 없음

```jsx
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // 3

const set = new Set([1, 2, 3]);
set.size = 10; // 무시된다.
console.log(set.size); // 3
```

<br/>

### (3) 요소 추가

- **`Set.prototype.add` 메서드를 사용하여 `Set` 객체에 요소를 추가한다.**
- `add` 새로운 요소가 추가된 `Set` 객체를 반환한다.
- `add` 메서드를 호출한 후에 `add` 메서드를 연속적 호출을 할 수 있다. ⇒ method chaining

  ```jsx
  const set = new Set();
  console.log(set); // Set(0) {size: 0}

  set.add(1);
  console.log(set); // Set(1) {1}

  // 연속적으로 호출하여 요소 추가
  set.add(2).add(3);
  console.log(set); // Set(2) {1, 2, 3}
  ```

- `Set` 객체에 중복된 요소는 추가되지 않고 무시된다.

  - `Set` 객체는 `NaN === NaN`을 `true`로 평가하여 중복 추가를 허용하지 않음
  - 또한, `+0 === -0`을 `true`로 평가하여 중복을 허용하지 않음

  ```jsx
  const set = new Set();

  // 중복된 요소를 추가할 경우 에러가 발생하지 않고 무시된다.
  set.add(2).add(2);
  console.log(set); // Set(1) {2}

  set.add(NaN).add(NaN); // NaN 중복 추가 안됨
  console.log(set); // Set(1) {NaN}

  set.add(0).add(-0); // 0 중복 추가 안됨
  console.log(set); // Set(2) {NaN, 0}
  ```

- `Set` 객체는 자바스크립트의 모든 값(`number`, `string`, `boolean`, `undefined`, `null`, 배열, 객체, 함수)을 요소로 저장할 수 있다.

<br/>

### (4) 요소 존재 여부 확인

- **`Set.prototype.has` 메서드를 사용하여 `Set` 객체에 특정 요소가 존재하는지 확인한다.**
- `has` 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값을 반환한다.

```jsx
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

<br/>

### (5) 요소 삭제

- **`Set.prototype.delete` 메서드를 사용하여 `Set` 객체의 특정 요소 삭제할 수 있다.**
- `delete` 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다. ⇒ 따라서, `add` 메서드와는 달리 연속적으로 호출할 수 없다.
- `Set` 객체는 순서에 의미가 없고 인덱스를 갖지 않는다. ⇒ 따라서 `delete` 메서드에는 삭제하려는 요소값을 인수로 전달해야한다.
- 존재하지 않는 요소를 삭제하려고 할 경우 에러없이 무시된다.

```jsx
const set = new Set([1, 2, 3]);

// delete 메서드는 연속적 호출 불가능
set.delete(1).delete(2); // TypeError

// 요소 2 삭제
set.delete(2);
console.log(set); // Set(2) {1, 3}

// 요소 1 삭제
set.delete(1);
console.log(set); // Set(1) {3}

// 존재하지 않는 요소를 삭제하려고 하면 에러없이 무시된다.
set.delete(4);
console.log(set); // Set(1) {3}
```

<br/>

### (6) 요소 일괄 삭제

- **`Set.prototype.clear` 메서드를 사용하여 `Set` 객체의 모든 요소를 일괄 삭제할 수 있다.**
- `clear` 메서드는 언제나 `undefined`를 반환한다.

```jsx
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set(0) {size: 0}
```

<br/>

### (7) 요소 순회

- **`Set.prototype.forEach` 메서드를 사용하여 `Set` 객체의 요소를 순회할 수 있다.**
- `Array.prototype.forEach` 메서드와 유사하게 콜백 함수와 `forEach` 메서드의 콜백 함수 내부에서 `this`로 사용될 객체(옵션)를 인수로 전달한다.
  - 콜백함수가 전달 받는 3개의 인수
    - 첫 번째 인수 : 현재 순회 중인 요소값
    - 두 번째 인수 : 현재 순회 중인 요소값
    - 세 번째 인수 : 현재 순회 중인 `Set` 객체 자체
  - `Array.prototype.forEach` 메서드와 인터페이스를 통일하기 위해 첫 번째 인수와 두 번째 인수에 동일한 값을 넣어준다.
  ```jsx
  const set = new Set([1, 2, 3]);
  set.forEach((v1, v2, set) => console.log(v1, v2, set));
  /*
  1 1 Set(3) {1, 2, 3}
  2 2 Set(3) {1, 2, 3}
  3 3 Set(3) {1, 2, 3}
  */
  ```
- **`Set` 객체는 이터러블이다.**

  - `for...of` 문으로 순회 가능
  - 스프레드 문법과 배열 디스트럭처링의 대상이 될 수 있다.

  ```jsx
  const set = new Set([1, 2, 3]);

  // 이터러블인 Set 객체는 for...of 문으로 순회 가능하다.
  for (let value of set) {
    console.log(value); // 1 2 3
  }

  // 이터러블인 Set 객체는 스프레드 문법의 대상이 된다.
  console.log([...set]); // [1, 2, 3]

  // 이터러블인 Set 객체는 배열 디스트럭처링 할당의 대상이 된다.
  const [a, ...rest] = set;
  console.log(a, rest); // 1 [2, 3]
  ```

- `Set` 객체는 요소의 순서에 의미를 갖지 않지만 `Set` 객체를 순회하는 순서는 요소가 추가된 순서를 따른다.

<br/>

### (8) 집합 연산

- `Set` 객체는 집합 연산을 할 수 있는 다양한 프로토타입 메서드를 제공한다.

- **교집합(`A ∩ B`)** ⇒ 집합 A와 집합 B의 공통 요소로 구성

  - `Set.prototype.intersection` 메서드를 사용하여 교집합을 구할 수 있다.

  ```jsx
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

- **합집합(`A ∪ B`)** ⇒ 집합 A와 집합 B의 중복 없는 모든 요소로 구성

  - `Set.prototype.union` 메서드를 사용하여 합집합을 구할 수 있다.

  ```jsx
  Set.prototype.union = function (set) {
    return new Set([...this, ...set]);
  };

  const setA = new Set([1, 2, 3, 4]);
  const setB = new Set([2, 4]);

  // setA와 setB의 합집합
  console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
  // setB와 setA의 합집합
  console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
  ```

- **차집합(`A - B`)** ⇒ 집합 A에는 존재하지만 집합 B에는 존재하지 않는 요소로 구성

  - `Set.prototype.difference` 메서드를 사용하여 차집합을 구할 수 있다.

  ```jsx
  Set.prototype.difference = function (set) {
    return new Set([...this].filter((v) => !set.has(v)));
  };

  const setA = new Set([1, 2, 3, 4]);
  const setB = new Set([2, 4]);

  // setA에 대한 setB의 차집합
  console.log(setA.difference(setB)); // Set(2) {1, 3}
  // setB에 대한 setA의 차집합
  console.log(setB.difference(setA)); // Set(0) {}
  ```

- **부분 집합과 상위 집합**

  - `Set.prototype.isSupersetOf` 메서드 또는 `Set.prototype.isSubsetOf` 메서드 사용
  - 집합 A가 집합 B에 포함되는 경우 **(`A ⊆ B`)**
    - 집합 A는 집합 B의 **부분 집합(subset)**
    - 집합 B는 집합 A의 **상위 집합(superset)**

  ```jsx
  // this가 subset의 상위 집합인지 확인
  Set.prototype.isSupersetOf = function (subset) {
    const supersetArr = [...this];
    return [...subset].every((v) => supersetArr.includes(v));
  };

  const setA = new Set([1, 2, 3, 4]);
  const setB = new Set([2, 4]);

  // setA가 setB의 상위 집합인지 확인
  console.log(setA.isSupersetOf(setB)); // true
  // setB가 setA의 상위 집합인지 확인
  console.log(setB.isSupersetOf(setA)); // false

  // setB가 setA의 부분 집합인지 확인
  console.log(setB.isSubsetOf(setA)); // true
  ```

<br/><br/>

## 2) `Map`

- **`Map` 객체** ⇒ 키와 값의 쌍으로 이루어진 컬렉션. 객체와 유사

| 구분                   | 객체                    | Map 객체              |
| ---------------------- | ----------------------- | --------------------- |
| 키로 사용할 수 있는 값 | 문자열 또는 심벌 값     | 객체를 포함한 모든 값 |
| 이터러블               | ❌                      | ⭕                    |
| 요소 개수 확인         | Object.keys(obj).length | map.size              |

<br/>

### (1) `Map` 객체의 생성

- `Map` 생성자 함수로 생성한다.
  - `Map` 생성자 함수에 인수를 전달하지 않으면 빈 `Map` 객체가 생성된다.
  ```jsx
  const map = new Map();
  console.log(map); // Map(0) {size: 0}
  ```
- **`Map` 생성자 함수는 이터러블을 인수로 전달받아 `Map` 객체를 생성한다.**

  - **인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.**
  - 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값은 덮어써진다.
  - 따라서, `Map` 객체에는 중복된 키를 갖는 요소가 존재할 수 없다.

  ```jsx
  const map1 = new Map([
    ["key1", "value1"],
    ["key2", "value2"],
  ]);
  console.log(map1); // Map(2) {'key1' => 'value1', 'key2' => 'value2'}

  const map2 = new Map([1, 2]); // TypeError: Iterator value 1 is not an entry object

  // 중복된 키 생성 불가 ⇒ 기존의 key1에는 뒤에 전달되는 value2값으로 덮어써진다.
  const map = new Map([
    ["key1", "value1"],
    ["key1", "value2"],
  ]);
  console.log(map); // Map(1) {'key1' => 'value2'}
  ```

<br/>

### (2) 요소 개수 확인

- **`Map.prototype.size` 프로퍼티를 사용하여 `Map` 객체의 요소 개수를 확인할 수 있다.**
- `Set` 객체와 마찬가지로, `size` 프로퍼티는 `getter` 함수만 존재하는 접근자 프로퍼티다. ⇒ `setter` 함수가 없으므로 `size` 프로퍼티에 숫자를 할당하여 `Map` 객체의 요소 개수를 변경할 수 없다.

<br/>

### (3) 요소 추가

- **`Map.prototype.set` 메서드를 사용하여 `Map` 객체의 요소를 추가한다.**

  - `set` 메서드는 새로운 요소가 추가된 `Map` 객체를 반환한다.
  - 따라서, `set` 메서드는 연속적 호출이 가능하다. ⇒ 메서드 체이닝

  ```jsx
  console.log(map); // Map(0){size: 0}

  map.set("key1", "value1");
  console.log(map); // Map(1) {'key1' => 'value1'}

  // 연속적 호출
  map.set("key2", "value2").set("key3", "value3");
  console.log(map);
  // Map(3) {'key1' => 'value1', 'key2' => 'value2', 'key3' => 'value3'}
  ```

- `Map` 객체에는 중복된 키를 갖는 요소가 존재할 수 없으므로 중복된 키를 갖는 요소를 추가하면 값이 덮어써진다. ⇒ 에러가 발생하지 않는다.
- `Set` 객체와 동일하게 `Map` 객체 또한 `NaN`과 `0`/`-0`의 중복 추가를 허용하지 않는다.
- 문자열과 심벌 값만 키로 사용가능한 일반 객체와는 달리, `Map` 객체는 객체를 포함한 모든 값을 키로 사용할 수 있다.

<br/>

### (4) 요소 취득

- **`Map.prototype.get` 메서드를 사용하여 `Map` 객체에서 특정 요소를 취득할 수 있다.**
- `get` 메서드의 인수로 키를 전달하면 `Map` 객체에서 인수로 전달한 키를 갖는 값을 반환한다.
- 인수로 전달한 키를 갖는 요소가 없는 경우에는 `undefined`를 반환한다.

```jsx
const map = new Map();

const lee = { name: "Lee" };
const kim = { name: "Kim" };

map.set(lee, "developer").set(kim, "designer");

console.log(map.get(lee)); // developer
console.log(map.get("key")); // undefined
```

<br/>

### (5) 요소 존재 여부 확인

- **`Map.prototype.has` 메서드를 사용하여 `Map` 객체에 특정 요소가 존재하는지 확인한다.**
- `has` 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값을 반환한다.

```jsx
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

console.log(map.has(lee)); // true
console.log(map.has("key")); // false
```

<br/>

### (6) 요소 삭제

- **`Map.prototype.delete` 메서드를 사용하여 `Map` 객체의 특정 요소 삭제할 수 있다.**
- `delete` 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환하므로, `set` 메서드와는 달리 연속적으로 호출할 수 없다.
- 존재하지 않는 키로 `Map` 객체의 요소를 삭제하려 하면 에러 없이 무시된다.

```jsx
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

map.delete(kim);
console.log(map); // Map(1) {{name: 'Lee'} => 'developer'}

// 존재하지 않는 요소를 삭제하려고 하면 에러없이 무시된다.
map.delete("noKey");
console.log(map); // Map(1) {{name: 'Lee'} => 'developer'}
```

<br/>

### (7) **요소 일괄 삭제**

- **`Map.prototype.clear` 메서드를 사용하여 `Map` 객체의 모든 요소를 일괄 삭제할 수 있다.**
- `clear` 메서드는 언제나 `undefined`를 반환한다.

<br/>

### (8) 요소 순회

- **`Map.prototype.forEach` 메서드를 사용하여 `Map` 객체의 요소를 순회할 수 있다.**
- `Array.prototype.forEach` 메서드와 유사하게 콜백 함수와 `forEach` 메서드의 콜백 함수 내부에서 `this`로 사용될 객체(옵션)을 인수로 전달한다.
- 콜백 함수가 전달받는 3개의 인수
  - 첫 번째 인수 : 현재 순회 중인 **요소 값**
  - 두 번째 인수 : 현재 순회 중인 **요소 키**
  - 세 번째 인수 : 현재 순회 중인 `Map` 객체 자체
- **`Map` 객체는 이터러블이다. ⇒ `for...of` 문으로 순회 가능, 스프레드 문법과 배열 디스트럭처링 할당의 대상이 된다.**
- `Map` 객체는 요소의 순서에 의미를 갖지 않지만 `Map` 객체를 순회하는 순서는 요소가 추가된 순서를 따른다.
- `Map` 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다.

| Map 메서드              | 설명                                                                             |
| ----------------------- | -------------------------------------------------------------------------------- |
| `Map.prototype.keys`    | Map 객체에서 요소키를 값으로 갖는 이터러블이면서 이터레이터인 객체 반환          |
| `Map.prototype.values`  | Map 객체에서 요소값를 값으로 갖는 이터러블이면서 이터레이터인 객체 반환          |
| `Map.prototype.entries` | Map 객체에서 요소키와 요소값을 값으로 갖는 이터러블이면서 이터레이터인 객체 반환 |

```jsx
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

// keys는 Map 객체에서 요소키를 값으로 갖는 이터레이터를 반환
for (const key of map.keys()) {
  console.log(key); // {name: 'Lee'} {name: 'Kim'}
}

// values는 Map 객체에서 요소값을 값으로 갖는 이터레이터를 반환
for (const value of map.values()) {
  console.log(value); // developer designer
}

// entries는 Map 객체에서 요소키와 요소값을 값으로 갖는 이터레이터를 반환
for (const entry of map.entries()) {
  console.log(entry); // [{name: 'Lee'}, 'developer']  [{name: 'Kim'}, 'designer']
}
```
