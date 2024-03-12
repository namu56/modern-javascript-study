# 📒37장_ Set과 Map
### 📑목차
- [Set](#set)
- [Set 집합 연산](#set-집합-연산)
- [Map](#map)

### ⚡Quick Summary
- *Set 객체*
    > 중복되지 않는 유일한 값들의 집합(set)
    - 배열과 유사하지만
        - 중복 값 허용 ❌
        - 순서 의미 ❌
        - 인덱스로 요소 접근 ❌
    - *이터러블* → `for … of` 문으로 순회 가능
        - Set 객체를 순회하는 순서는 **요소가 추가된 순서**를 따름
        - 다른 이터러블의 순회와 호환성을 유지하기 위함
    - 수학적 집합 ⇒ 교집합, 합집합, 차집합 등 계산 가능

    |Set 객체|방법|특징|
    |--|--|--|
    |생성|`Set` 생성자 함수|인수를 전달하지 않으면 빈 Set 객체 생성<br>인수로는 *이터러블* 전달 가능|
    |요소 개수 확인|`Set.prototype.size` 프로퍼티|Read-only 접근자 프로퍼티|
    |특정 요소 추가|`Set.prototype.add` 메서드|새로운 요소가 추가된 Set 객체를 반환 → method chaining 가능<br>`NaN`도 중복 구분 가능|
    |특정 요소가 존재하는지 확인|`Set.prototype.has` 메서드|Boolean 값 반환|
    |특정 요소 삭제|`Set.prototype.delete` 메서드|요소값을 인수로 전달<br>삭제 성공 여부를 나타내는 Boolean 값 반환<br>존재하지 않는 요소 삭제 시 에러 없이 무시됨|
    |모든 요소를 일괄 삭제|`Set.prototype.clear` 메서드|언제나 `undefined` 반환|
    |요소 순회|`Set.prototype.forEach` 메서드|`Array.prototype.forEach` 메서드와 유사하지만 Set 객체는 순서에 의미가 없어 배열과 같이 인덱스를 갖지 않기 때문에 두 번째 인수도 첫 번째 인수와 동일(현재 순회 중인 요소값)|
    |요소를 값으로 갖는 이터러블&이터레이터 객체|`Set.prototype.keys` 메서드<br>`Set.prototype.values` 메서드|Map 객체와는 달리 key가 없기 때문에 `keys`와 `values` 모두 Set 객체에서 요소를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환|
    |`[요소, 요소]`를 값으로 갖는 이터러블&이터레이터 객체|`Set.prototype.entries` 메서드|Map 객체의 API와 비슷하게 유지하기 위함|
    |교집합|`Set.prototype.intersection` 메서드|A ∩ B|
    |합집합|`Set.prototype.union` 메서드|A ∪ B|
    |차집합|`Set.prototype.difference` 메서드|A ∖ B|
    |대칭차집합|`Set.prototype.symmetricDifference` 메서드|A △ B = (A ∪ B) ∖ (A ∩ B) = (A ∖ B) ∪ (B ∖ A)|
    |서로소 집합|`Set.prototype.isDisjointFrom` 메서드|A ∩ B = ∅|
    |부분집합|`Set.prototype.isSubsetOf` 메서드|A ⊆ B|
    |상위집합|`Set.prototype.isSupersetOf` 메서드|A ⊇ B|

- *Map 객체*
    > 키-값 쌍으로 이루어진 컬렉션
    - 객체와 유사하지만
        - 객체를 포함한 모든 값을 키로 사용 가능
        - 객체는 요소 개수를 `Object.keys(obj).length`로 확인($O(n)$)하지만 Map 객체는 `map.size`($O(1)$)로 확인
    - 해시 테이블이기 때문에 순서 의미 ❌
    - *이터러블* → `for … of` 문으로 순회 가능
        - Map 객체를 순회하는 순서는 **요소가 추가된 순서**를 따름
        - 다른 이터러블의 순회와 호환성을 유지하기 위함

    |Map 객체|방법|특징|
    |--|--|--|
    |생성|`Map` 생성자 함수|인수를 전달하지 않으면 빈 Map 객체 생성<br>인수로는 *이터러블* 전달 가능(키와 값의 쌍으로 이루어진 요소로 구성되어야 함, 중복 키는 값이 덮어써짐)|
    |요소 개수 확인|`Map.prototype.size` 프로퍼티|Read-only 접근자 프로퍼티|
    |특정 요소 추가|`Map.prototype.set` 메서드|새로운 요소가 추가된 Map 객체를 반환 → method chaining 가능<br>`NaN`도 중복 구분 가능|
    |특정 요소 취득|`Map.prototype.get` 메서드|인수로 키를 전달하면 Map 객체에서 인수로 전달한 키를 갖는 값 반환<br>존재하지 않으면 `undefined` 반환|
    |특정 요소가 존재하는지 확인|`Map.prototype.has` 메서드|Boolean 값 반환|
    |특정 요소 삭제|`Map.prototype.delete` 메서드|요소키를 인수로 전달<br>삭제 성공 여부를 나타내는 Boolean 값 반환<br>존재하지 않는 키로 요소 삭제 시 에러 없이 무시됨|
    |모든 요소를 일괄 삭제|`Map.prototype.clear` 메서드|언제나 `undefined` 반환|
    |요소 순회|`Map.prototype.forEach` 메서드|`Array.prototype.forEach` 메서드와 유사하지만 Map 객체는 두 번째 인수로 요소 키를 가지는데 이는 사실 Array는 인덱스를 키로 갖는 객체라는 점에서 일맥상통함|
    |요소키를 값으로 갖는 이터러블&이터레이터 객체|`Map.prototype.keys` 메서드||
    |요소값을 값으로 갖는 이터러블&이터레이터 객체|`Map.prototype.values` 메서드||
    |`[요소키, 요소값]`를 값으로 갖는 이터러블&이터레이터 객체|`Map.prototype.entries` 메서드||

## 📌Set
> 중복되지 않는 유일한 값들의 집합(set)
- 배열 vs Set 객체
    |구분|배열|Set 객체|
    |--|:--:|:--:|
    |동일한 값을 중복하여 포함 가능|⭕|❌|
    |요소 순서에 의미가 있음|⭕|❌|
    |인덱스로 요소 접근 가능|⭕|❌|
- Set 객체의 특성은 **수학적 집합**의 특성과 일치 ⇒ 교집합, 합집합, 차집합, 여집합 등 구현 가능
- `Set` 생성자 함수로 Set 객체 생성
    - 인수를 전달하지 않으면 빈 Set 객체가 생성됨
        ```js
        const set = new Set();
        console.log(set); // Set(0) {}
        ```
    - **이터러블**을 인수로 전달받음 (중복 요소 제거 가능)
        ```js
        const set1 = new Set([1, 2, 3, 3]);
        console.log(set1); // Set(3) {1, 2, 3}

        const set2 = new Set('hello');
        console.log(set2); // Set(4) {"h", "e", "l", "o"}

        // Set을 사용한 배열의 중복 요소 제거
        const uniq = array => [...new Set(array)];
        console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]
        ```
- `Set.prototype.size` 프로퍼티로 Set 객체의 요소 개수 확인
    - ✔️size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티 (read-only)
    - size 프로퍼티에 숫자를 할당해서 Set 객체의 요소 개수 변경 불가능
- `Set.prototype.add` 메서드로 Set 객체에 요소 추가
    - add 메서드는 새로운 요소가 추가된 Set 객체를 반환하기 때문에 add 메서드를 연속적으로 호출(method chaining) 가능
    - 중복된 요소의 추가는 허용되지 않음 (중복된 요소를 추가하려 하면 에러 없이 무시됨)
    - ✔️일치 비교 연산자 `===`을 사용하면 `NaN`과 `NaN`을 다르다고 평가하지만 Set 객체는 같다고 평가하여 중복 추가를 허용하지 않음
    - 자바스크립트의 모든 값을 요소로 저장 가능
- `Set.prototype.has` 메서드로 Set 객체에 특정 요소가 존재하는지 확인
    - 특정 요소의 존재 여부를 나타내는 boolean 값 반환
- `Set.prototype.delete` 메서드로 Set 객체의 특정 요소 삭제
    - 삭제 성공 여부를 나타내는 boolean 값 반환
    - 삭제하려는 요소값을 인수로 전달
    - 존재하지 않는 요소를 삭제하려 하면 에러 없이 무시됨
- `Set.prototype.clear` 메서드로 Set 객체의 모든 요소를 일괄 삭제
    - 언제나 `undefined` 반환
- `Set.prototype.forEach` 메서드로 Set 객체의 요소 순회
    - `Array.prototype.forEach` 메서드와 유사
    - ⭐단 Set 객체는 순서에 의미가 없어 배열과 같이 인덱스를 갖지 않기 때문에 두 번째 인수는 배열과 같이 인덱스가 아니라 첫 번째 인수와 동일하게 현재 순회 중인 요소값을 가짐
        ||`Array.prototype.forEach`|`Set.prototype.forEach`|
        |--|--|--|
        |첫 번째 인수|현재 순회 중인 요소값|현재 순회 중인 요소값|
        |두 번째 인수|현재 순회 중인 요소의 인덱스|현재 순회 중인 요소값|
        |세 번째 인수|현재 순회 중인 Array 객체 자체|현재 순회 중인 Set 객체 자체|
    - Set 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드 제공
        |Set 메서드|설명|
        |--|--|
        |`Set.prototype.keys`|`Set.prototype.values`의 별칭(Map 객체와는 달리 key가 없기 때문)|
        |`Set.prototype.values`|Set 객체에서 요소를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환|
        |`Set.prototype.entries`|Set 객체에서 `[요소, 요소]`를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환(Map 객체의 API와 비슷하게 유지하기 위함)|
- Set 객체는 **이터러블**로 `for … of` 문으로 순회 가능
    - ⭐Set 객체는 요소의 순서에 의미를 갖지 않지만 Set 객체를 순회하는 순서는 **요소가 추가된 순서**를 따름
    - 다른 이터러블의 순회와 호환성을 유지하기 위함

## 📌Set 집합 연산
### 교집합
```js
Set.prototype.intersection = function (set) {
  return new Set([...this].filter(v => set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

### 합집합
```js
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

### 차집합
```js
Set.prototype.difference = function (set) {
  return new Set([...this].filter(v => !set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```

### 부분 집합과 상위 집합
- A ⊆ B일 때
    - 집합 A는 집합 B의 부분 집합(subset)
    - 집합 B는 집합 A의 상위 집합(superset)
```js
// this가 subset의 상위 집합인지 확인
Set.prototype.isSuperset = function (subset) {
  const supersetArr = [...this];
  return [...subset].every(v => supersetArr.includes(v));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인
console.log(setA.isSuperset(setB)); // true
// setB가 setA의 상위 집합인지 확인
console.log(setB.isSuperset(setA)); // false
```

## 📌Map
> 키와 값의 쌍으로 이루어진 컬렉션
- 객체 vs Map 객체
    |구분|배열|Set 객체|
    |--|:--:|:--:|
    |키로 사용할 수 있는 값|문자열 또는 심벌 값|객체를 포함한 모든 값|
    |이터러블|❌|⭕|
    |요소 개수 확인|`Object.keys(obj).length`|`map.size`|
- `Map` 생성자 함수로 Map 객체 생성
    - 인수를 전달하지 않으면 빈 Map 객체가 생성됨
        ```js
        const map = new Map();
        console.log(map); // Map(0) {}
        ```
    - **이터러블**을 인수로 전달받음 (키와 값의 쌍으로 이루어진 요소로 구성되어야 함, 중복 키는 값이 덮어써짐)
        ```js
        const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
        console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

        const map2 = new Map([1, 2]); // TypeError: Iterator value 1 is not an entry object
        ```
- `Map.prototype.size` 프로퍼티로 Map 객체의 요소 개수 확인
    - ✔️size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티 (read-only)
    - size 프로퍼티에 숫자를 할당해서 Map 객체의 요소 개수 변경 불가능
- `Map.prototype.set` 메서드로 Map 객체에 요소 추가
    - set 메서드는 새로운 요소가 추가된 Map 객체를 반환하기 때문에 set 메서드를 연속적으로 호출(method chaining) 가능
    - 중복된 키를 갖는 요소를 추가하면 값이 덮어써짐
    - ✔️일치 비교 연산자 `===`을 사용하면 `NaN`과 `NaN`을 다르다고 평가하지만 Map 객체는 같다고 평가하여 중복 키의 요소를 덮어씀
    - 자바스크립트의 모든 값을 요소로 저장 가능
- `Map.prototype.get` 메서드로 Map 객체에서 특정 요소 취득
    - 인수로 키를 전달하면 Map 객체에서 인수로 전달한 키를 갖는 값 반환
    - 존재하지 않으면 `undefined` 반환
- `Map.prototype.has` 메서드로 Map 객체에 특정 요소가 존재하는지 확인
    - 특정 요소의 존재 여부를 나타내는 boolean 값 반환
- `Map.prototype.delete` 메서드로 Map 객체의 특정 요소 삭제
    - 삭제 성공 여부를 나타내는 boolean 값 반환
    - 삭제하려는 요소의 키를 인수로 전달
    - 존재하지 않는 키로 요소를 삭제하려 하면 에러 없이 무시됨
- `Map.prototype.clear` 메서드로 Map 객체의 모든 요소를 일괄 삭제
    - 언제나 `undefined` 반환
- `Map.prototype.forEach` 메서드로 Set 객체의 요소 순회
    - `Array.prototype.forEach` 메서드와 유사
    - ⭐Map 객체는 두 번째 인수로 요소 키를 가지는데 이는 사실 Array는 인덱스를 키로 갖는 객체라는 점에서 일맥상통함
        ||`Array.prototype.forEach`|`Map.prototype.forEach`|
        |--|--|--|
        |첫 번째 인수|현재 순회 중인 요소값|현재 순회 중인 요소값|
        |두 번째 인수|현재 순회 중인 요소의 인덱스|현재 순회 중인 요소키|
        |세 번째 인수|현재 순회 중인 Array 객체 자체|현재 순회 중인 Map 객체 자체|
    - Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드 제공
        |Map 메서드|설명|
        |--|--|
        |`Map.prototype.keys`|Map 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환|
        |`Map.prototype.values`|Map 객체에서 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환|
        |`Map.prototype.entries`|Map 객체에서 `[요소키, 요소값]`을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환|
- Map 객체는 **이터러블**로 `for … of` 문으로 순회 가능
    - ⭐Map 객체는 요소의 순서에 의미를 갖지 않지만 Map 객체를 순회하는 순서는 **요소가 추가된 순서**를 따름
    - 다른 이터러블의 순회와 호환성을 유지하기 위함