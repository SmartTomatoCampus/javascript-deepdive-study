# set과 map

## 37.1 Set

set 객체는 중복되지 않는 유일한 값들의 집합.  
 배열과 유사하지만 다음과 같은 차이가 있음.

1. 동일한 값을 중복하여 포함할 수 없다.
2. 요소 순서에 의미가 없다.
3. 인덱스로 요소에 접근할 수 없다.

- set을 활용하여 배열에서 중복된 요소 제거 ㄱ나ㅡㅇ

요소 개수 확인

- Set.prototype.size

요소추가

- Set.prototype.add
  - 중복된 요소 추가시 무시됨.
  - 새로운 요소가 추가된 Set 객체 반환 (연속 호출 가능)
  - 자바스크립트의 모든 값을 요소로 저장 가능

요소 존재 여부 확인

- Set.prototype.has (boolean값으로 반환(true/false))

요소 삭제

- Set.prototype.delete
  - 존재하지 않는 값 삭제시 무시됨
  - 삭제 성공 유무를 나타내는 (boolean값 반환(true/false))
  - 연속 호출 불가

요소 일괄 삭제

- Set.prototype.clear
  - 언제나 undefined 반환

요소 순회

- Set.prototype.forEach
  - 첫 번째 인수 : 현재 순회중인 요소 값
  - 두 번째 인수 : 현재 순회중인 요소 값
  - 세 번째 인수 : 현재 순회중인 Set 객체 자체
  - set 객체는 이터러블이다.(`for ... of`문으로 순회 가능, 스프레드 문법과 배열 디스트럭쳐링의 대상이 될 수 있음.)
  - 요소의 순서에 의미가 없으나 Set 객체를 순회하는 순서는 요소가 추가된 순서를 따른다.

집합 연산

- Set 객체를 통해 교집합, 합집합, 차집합 등을 구현 가능

### 교집합

```js
Set.prototype.intersection = function (set) {
  const result = new Set();

  for (const value of set) {
    //2개의 set의 요소가 공통되는 요소이면 교집합의 대상이다.
    if (this.has(value)) result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);
console.log(setA.intersection(setB)); // Set(2) {2,4}
```

```js
Set.prototype.intersection = function (set) {
  return new Set([...this].filter((v) => set.has(v)));
};
```

### 합집합

```js
Set.prototype.union = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    //합집합은 2개의 Set 객체의 모든 요소로 구성된 집합이다. 중복된 요소는 포함되지 않는다.
    result.add(value);
  }

  return result;
};
```

```js
Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};
```

### 차집합

```js
Set.prototype.difference = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 차집합은 어느 한쪽 집합에는 존재하지만 다른 한쪽 집합에는 존재하지 않는 요소로 구성된 집합이다.
    result.delete(value);
  }
  return result;
};
```

```js
set.prototype.difference = function (set) {
  return new Set([...this].filter((v) => !set.has(v)));
};
```

### 부분집합 & 상위집합

```js
Set.prototype.isSuperset = function (subset) {
  for (const value of subset) {
    // superset의 모든 요소가 subset의 모든 요소가 포함하는지 확인
    if (!this.has(value)) return false;
  }
  return true;
};
```

```js
Set.prototype.isSuperset = function (subset) {
  const supersetArr = [...this];
  return [...subset].every((v) => supersetArr.includes(v));
};
```

## 37.2 Map

map 객체는 키와 값의 쌍으로 이루어진 컬렉션  
 객체와 유사하지만 다음과 같은 차이가 잇음
|구분|객체|map 객체|
| --- | --- | --- |
|키로 사용할 수 있는 값 | 문자열 또는 심벌 값 | 객체를 포함한 모든 값|
| 이터러블 | x | O |
| 요소 개수 확인 | Object.keys(obj).length | map.size |

### map 객체 생성

Map 생성자 함수로 생성(인수전달 안하면 빈 Map 객체 생성)

### 요소 개수 확인

- Map.prototype.size
  - getter 함수만 존재하는 접근자 프로퍼티

### 요소 추가

- Map.prototype.set

  - 새로운 요소가 추가된 Map 객체 반환(연속적 호출 가능)
  - 키 타입에 제한이 없다.

### 요소 취득

- Map.prototype.get
  - get 메서드의 인수로 키를 전달하면 Map 객체에서 인수로 전달한 키를 갖는 값을 반환(인수로 전달한 키를 갖는 요소가 없으면 undefined 반환)

### 요소 존재여부 확인

- Map.prototype.has
  - 불리언 값 반환(true/false)

### 요소 삭제

- Map.prototype.delete
  - 존재하지 않는 키로 객체의 요소를 삭제하려하면 무시됨.
  - 삭제 성공여부를 나타내느 불리언 값 반환(연속적 호출 불가)

### 요소 일괄삭제

- Map.prototype.clear
  - clear 메서드는 언제나 undefined 반환

### 요소 순회

- Map.prototype.forEach

  - 첫 번째 인수 : 현재 순회중인 요소 값
  - 두 번째 인수 : 현재 순회중인 요소 키
  - 세 번째 인수 : 현재 순회중인 Map 객체 자체
  - Map 객체는 이터러블(`for ... of`문으로 순회 가능, 스프레드 문법과 배열 디스트럭처링 할당의 대상이 될 수도 있음.)

- map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드 제공
  - Map.prototype.keys : Map 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환
  - Map.prototype.values : Map 객체에서 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환.
  - Map.prototype.entries : Map 객체에서 요소키와 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환.
