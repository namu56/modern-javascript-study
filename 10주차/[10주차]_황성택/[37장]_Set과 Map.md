## Set과 Map

### Set

> 중복되지 않는 유일한 값들의 집합들이 모인 객체 (배열과 유사하지만 차이가 있다.)

|구분|배열|Set 객체|
|:--:|:--:|:--:|
|동일한 값을 중복 포함|⭕|❌|
|요소 순서의 의미|⭕|❌|
|인덱스로 요소에 접근|⭕|❌|

- 수학적 집합의 특성과 일치한다. 수학적 집합을 구현하기 위한 자료구조
- Set을 통해 교집합, 합집합, 차집합, 여집합 등을 구현할 수 있다.

1. Set 객체의 생성

- Set 객체는 Set 생성자 함수로 생성하며, 생성자 함수에 인수를 전달하지 않으면 빈 Set 객체가 생성된다.

```jsx
const set = new Set();
console.log(set); // Set(0) {}
```

- Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성하며, ✨이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않는다.
- 이러한 특성으로 배열에서 중복된 요소를 제거하는데에 사용할 수 있다.

```jsx
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set { 1, 2, 3 }

const set2 = new Set('hello');
console.log(set2); // Set { 'h', 'e', 'l', 'o' }
```

2. 요소 개수 확인

- `Set.prototype.size` 프로퍼티를 사용한다.
- size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티이다.

```jsx
const { size } = new Set([1, 2, 3, 4, 4, 4]);
console.log(size); // 4
```

3. 요소 추가

- `Set.prototype.add` 메서드를 사용한다.
- Set 객체에 중복된 요소를 추가할 경우, 에러가 발생하지 않고 무시된다.
- NaN과 NaN, +0과 -0을 같다고 평가하여 중복 추가를 허용하지 않는다.
- 객체나 배열과 같이 자바스크립트의 모든 값을 요소로 저장할 수 있다.

```jsx
const set = new Set();

set.add(1);
console.log(set); // Set { 1 }
```

4. 요소 존재 여부 확인

- `Set.prototype.has` 메서드를 사용한다.

```jsx
const set = new Set([1, 2, 3]);

console.log(set.has(1)); // true
console.log(set.has(4)); // false
```

5. 요소 삭제

- `Set.prototype.delete` 메서드를 사용한다. (성공 여부를 나타내는 불리언 값을 반환한다.)
- 삭제하려는 요소값을 인수로 전달해야 한다. (배열과 같이 인덱스를 갖지 않는다.)
- 존재하지 않는 요소를 삭제하려 하면 에러 없이 무시된다.
- delete 메서드는 블리언 값을 반환하기에 연속적으로 호출할 수 없다.

```jsx
const set = new Set([1, 2, 3]);

set.delete(2);
console.log(set); // Set { 1, 3 }

set.delete(0);
console.log(set); // Set { 1, 3 }
```


6. 요소 일괄 삭제

- `Set.prototype.clear` 메서드를 사용한다. (clear 메서드는 언제나 undefined를 반환한다.)

```jsx
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set {}
```

7. 요소 순회

- `Set.prototype.forEach` 메서드를 사용한다.
- `(현재 순회 중인 요소값, 현재 순회 중인 요소값, 현재 순회 중인 Set 객체 자체)`

```jsx
const set = new Set([1, 2, 3]);

set.forEach((value, valueAgain, set) => {
    console.log(value, valueAgain, set); 
    /*
    1 1 Set { 1, 2, 3 }
    2 2 Set { 1, 2, 3 }
    3 3 Set { 1, 2, 3 }
    */
});
```

### Map

> 키와 값의 쌍으로 이루어진 컬렉션이다. (객체와 유사하지만 차이가 있다.)

|구분|객체|Map 객체|
|:--:|:--:|:--:|
|키로 사용할 수 있는 값|문자열 또는 심벌 값|객체를 포함한 모든 값|
|이터러블|❌|⭕|
|요소 개수 확인|Object.keys(obj).length|map.size|

1. Map 객체의 생성

- Map 생성자 함수로 생성하며, 인수를 전달하지 않으면 빈 Map 객체가 생성된다.
- 인수로 전달받는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.
- 중복되 키를 갖는 요소가 존재하면 값이 덮어써진다. (✨중복된 키를 갖는 요소가 존재할 수 없다.)

```jsx
const map = new Map();
console.log(map); // Map(0) {}

const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(map1); // Map { 'key1' => 'value1', 'key2' => 'value2' }

const map2 = new Map([1, 2]); // TypeError: Iterator value 1 is not an entry object

const map3 = new Map([['key1', 'value1'], ['key1', 'value2']]);
console.log(map3); // Map { 'key1' => 'value2' }
```

2. 요소 개수 확인

- `Map.prototype.size` 프로퍼티를 사용한다.
- size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티이다.

```jsx
const map = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(map.size); // 2
```

3. 요소 추가

- `Map.prototype.set` 메서드를 사용한다.
- 중복된 키를 갖는 요소를 추가하면 값이 덮어써진다. (에러 발생 X)
- NaN과 NaN, +0과 -0을 같다고 평가하여 중복 추가를 허용하지 않는다.
- Map 객체는 객체를 포함한 모든 값을 키로 사용할 수 있다.

```jsx
const map = new Map();

map.set('name', 'John');
console.log(map); // Map { 'name' => 'John' }

const map = new Map();

map
    .set('key1', 'value1')
    .set('key1', 'value2');

console.log(map); // Map { 'key1' => 'value2' }
```

4. 요소 취득

- `Map.prototype.get` 메서드를 사용한다.
- 인수로 전달한 키를 갖는 요소가 존재하지 않으면 `undefined`를 반환한다.

```jsx
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map
    .set(lee, 'developer')
    .set(kim, 'designer');

console.log(map.get(lee)); // developer
console.log(map.get(jeong)); // undefined
```

5. 요소 존재 여부 확인

- `Map.prototype.has` 메서드를 사용한다.

```jsx
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map
    .set(lee, 'developer')
    .set(kim, 'designer');

console.log(map.has(lee)); // true
console.log(map.has(hwang)); // false
```

6. 요소 삭제

- `Map.prototype.delete` 메서드를 사용한다.
- 존재하지 않는 키로 Map 객체의 요소를 삭제하려 하면 에러 없이 무시된다.

7. 요소 일괄 삭제

- `Map.prototype.clear` 메서드를 사용한다. (clear 메서드는 언제나 undefined를 반환한다.)

8. 요소 순회

- `Map.prototype.forEach` 메서드를 사용한다.
-  `(현재 순회 중인 요소값, 현재 순회 중인 요소키, 현재 순회 중인 Map 객체 자체)`
