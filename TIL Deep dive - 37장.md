## Set

<span style=font-size:14px>Set 객체는 중복되지 않는 유일한 값들의 집합(Set)이다. Set 객체는 배열과 유사하지만 다음과 같은 차이가 있다.</span>

<table style="text-align:center">
  <tr >
    <td>구분</td><td> 배열</td><td>Set 객체</td>
  </tr>
  <tr>
    <td>동일한 값을 중복하여 포함할 수 있다.</td><td>O</td><td>X</td>
  </tr>
  <tr>
    <td>요소 순서에 의미가 있다.</td><td>O</td><td>X</td>
  </tr>
  <tr>
    <td>인덱스로 요소에 접근할 수 있다.</td><td>O</td><td>X</td>
  </tr>
  </table>
  
  <span style=font-size:14px> Set 객체의 특성은 수학적 집합의 특성과 일치한다.</span>
  <span style=color:gold;font-size:14px>Set은 수학적 집합을 구현하기 위한 자료구조다.
    따라서 Set을 통해 교집합,합집합,차집합,여집합 등을 구현할 수 있다.</span>

___
### Set 객체의 생성
##### Set 객체는 Set 생성자 함수로 생성한다. Set 생성자 함수에 인수를 전달하지 않으면 빈 Set 객체가 생성된다.
<span style=font-size:14px> Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다. 이때 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않는다.</span>
```js
const set = new Set();
console.log(set); // Set(0) {}

const set1 = new Set([1,2,3,3]);
console.log(set1); // Set(3) {1,2,3}

const set2 = new Set('hello');
console.log(set2) // Set(4) {"h","e","l","o"}

```

<span style=font-size:14px> 중복을 허용하지 않는 Set 객체의 특성을 활용하여 배열에서 중복된 요소를 제거할 수 있다.</span>
```js
// 배열의 중복 요소 제거
const uniq = array => array.filter((v,i,self) => self.indexOf(v) === i);
console.log(uniq([2,1,2,3,4,3,4])); // [2,1,3,4]

const uniq = array => [... new Set(array)];
console.log(uniq([2,1,2,3,4,3,4,])); // [2,1,3,4]
```
___
#### Set의 프로토타입 프로퍼티와 프로토타입 메서드
<span style=font-size:14px>1. **Set.prototype.size**
  Set 객체의 요소 개수를 확인할 때는 **Set.prototype.size** 프로퍼티를 사용한다.</span>
```js
const { size } = new Set([1,2,3,3]);
console.log(size); // 3
```
<span style=font-size:14px> Size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티다. 따라서 size 프로퍼티에 숫자를 할당하여 Set 객체의 요소 개수를 변경할 수 없다.</span>
  ___
<span style=font-size:14px>2. ** Set.prototype.add **
  Set 객체에 요소를 추가할 때는 add 메서드를 사용한다. add 메서드는 새로운 요소가 추가된 Set 객체를 반환하며, 여러개를 연속적으로 호출할 수 있다.</span>
  ```js
  const set = new Set();

set.add(1).add(2)
console.log(set); // Set(2) {1,2}
```
중복된 요소의 추가는 허용되지 않으며 에러는 발생하지 않고 무시된다.
___
<span style=font-size:14px>3. ** Set.prototype.has **
  Set 객체에 특정 요소가 존재하는지 확인하기 위해 사용한다. 요소의 존재 여부는 불리언 값으로 반환한다.</span>
  ```js
const set = new set([1,2,3]);
console.log(set.has(2)); //true
console.log(set.has(4)); // false
```
___
<span style=font-size:14px> **4. Set.prototype.delete **
  Set 객체의 특정 요소를 삭제하기 위해 사용되며 삭제 성공 여부를 나타내는 불리언 값을 반환한다. add와는 다르게 불리언 값을 반환하므로 연속적으로 호출할 수 없다.</span>
  
  ```js
const set = new Set([1,2,3]);
set.delete(2);
console.log(set) // Set(2) {1,3}
```

  
<span style=font-size:14px> ** 5. Set.prototype.clear **
  Set 객체의 모든 요소를 일괄 삭제하려면 clear 메서드를 사용한다. clear는 언제나 undefined를 반환한다.</span>
  ```js
const set = new Set([1,2,3]);

set.clear();

console.log(set); // Set(0) {}
```

<span style=font-size:14px> ** 6. Set.prototype.forEach **
  Set 객체의 요소를 순회하기 위해 사용한다 . Array의 프로토타입 forEach 메서드와 유사하지만 콜백 함수의 2번째 인수는 인덱스가 아닌 첫번째와 같은 현재 순회 중인 요소값이다. 즉 첫 번째 인수와 두 번째 인수는 현재 순회 중인 요소값으로 같다.</span>
  ```js
const set = new Set([1,2,3]);
set.forEach((v,v2,set) => console.log(v,v2,set));
1 1 Set(3) {1,2,3}
2 2 Set(3) {1,2,3}
3 3 Set(3) {1,2,3}
```

Set 객체는 이터러블이기에 for ... of 문과 스프레드 문법, 배열 디스트럭쳐링의 대상이 될 수도 있다.

```js
const set = new Set([1,2,3]);

// Set 객체는 Set.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.

console.log(Symbol.iterator in set) // true

// 이터러블인 Set 객체는 for ... of 문으로 순회할 수 있다.

for (const value of set) {
  console.log(value); // 1 2 3
}

// 이터러블인 Set 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...set]); // [1,2,3]

// 이터러블인 Set 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [a, ...rest] = set;
console.log(a,rest); // 1, [2,3]
```
Set 객체는 요소의 순서에 의미를 갖지 않지만 Set 객체를 순회하는 순서는 요소가 추가된 순서를 따른다.
___
### 집합 연산
##### Set 객체는 수학적 집합을 구현하기 위한 자료구조다. 따라서 Set 객체를 통해 교집합,합집합,차집합 등을 구현할 수 있다. 집합 연산을 수행하는 프로토타입 메서드를 구현하면 다음과 같다.

#### 교집합
##### 교집합은 집합 A와 집합 B의 공통 요소로 구성된다.
```js
// 1번째 방법
Set.prototype.intersection = function(set){
  const result = new Set();
  for(const value of set) {
    // 2개의 set의 요소가 공통되는 요소이면 교집합의 대상이다.
    if(this.has(value)) result.add(value);
  }
  return result;
};

const setA = new Set([1,2,3,4]);
const setB = new SEt([2,4]);

console.log(setA.intersection(setB)); //Set(2) {2,4}
console.log(setB.intersection(setA)); //Set(2) {2,4}

//2번째 방법
Set.prototype.intersection = function(set){
  return new Set([...this].filter(v=> set.has(v)));
};
const setA = new Set([1,2,3,4]);
const setB = new SEt([2,4]);

console.log(setA.intersection(setB)); //Set(2) {2,4}
console.log(setB.intersection(setA)); //Set(2) {2,4}

```

#### 합집합

##### 합집합은 집합 A와 집합 B의 중복 없는 모든 요소로 구성된다.

```js
// 1번째 방법
Set.prototype.union = function(set){
  //this(Set 객체)를 복사
  const result = new Set(this);
  
  for(const value of set) {
    result.add(value);
  }
  return result;
};
const setA = new Set([1,2,3,4]);
const setB = new Set([1,3]);

console.log(setA.union(setB)); // Set(4) {1,2,3,4}
console.log(setB.union(setA)); // Set(4) {1,3,2,4}

// 2번째 방법

Set.prototype.union = function(set){
  return new Set([...this, ...set]);
};
```
___
### 차집합
#### 차집합은 집합 A에는 존재하지만 집합 B에는 존재하지 않는 요소로 구성된다.

```js
//1번째 방법

Set.prototype.difference = function(set){
  //this(Set 객체)를 복사
  const result = new Set(this);
  
  for(const value of set){
    result.delete(value);
  }
  return result;
};

const setA = new Set([1,2,3,4]);
const setB = new Set([2,4]);

//SetA에 대한 setB의 차집합
console.log(setA.difference(setB)); //Set(2) {1,3}
console.log(setB.difference(setA)); //Set(0) {}

//2번쨰 방법.
Set.prototype.difference = function(set){
  return new Set([...this].filter(v => !set.has(v)));
};

const setA = new Set([1,2,3,4]);
const setB = new Set([2,4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1,3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```

> ###### 일치 비교 연산자를 사용하면 NaN과 NaN을 다르다고 평가하지만 Set 객체에서는 같다고 평가하여 중복 추가를 허용하지 않는다. 

___

## Map

#### Map 객체는 키와 값의 쌍으로 이루어진 컬렉션이다. Map 객체는 객체와 유사하지만 다른 점이 있다.
1. #### 객체를 포함한 모든 값을 키로 사용할 수 있다.
2. #### 이터러블하다.
3. #### 요소의 개수 확인은 map.size를 통해 확인할 수 있다.(일반 객체는 length)

##### Map 생성자 함수의 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값을 덮어 씌워서 중복된 키를 갖지 않게 한다.
```js
const map1 = new Map([['key1','value1'],['key1','value2']]);
console.log(map1) // Map(2) {"key1" => "value1", "key2" => "value2"}
```
##### Size를 통해 요소의 갯수를 확인할 수 있다.
```js
const { size } = new Map([['key1','value1'],['key1','value2']]);

console.log(map.size) // 2
```
##### 요소를 추가할 때에는 Set과 다르게 add가 아닌 Map.prototype.set 메서드를 사용한다. 
```js
//연속적으로 사용이 가능하다.
map.set('key1','value1').set('key2','value2');
```
___
#### Map 객체에는 키 타입에 제한이 없기에 객체를 포함한 모든 값을 키로 사용할 수 있다. 이는 Map 객체와 일반 객체의 가장 두드러지는 차이점이다.
```js
const map = new Map();
const lee = {name: 'Lee'}
const kim = {name: 'kim'}

// 객체도 키로 사용할 수 있다.
map
	.set(lee,'developer');
	.set(kim,'designer');
console.log(map);
//Map (2) { {name: "Lee"} => 'developer', {name:kim} => 'designer'}
```
##### Map 객체에서 특정 요소를 취드갛려면 Map.prototype.get 메서드를 사용한다. get 메서드의 인수로 키를 전달하면 Map 객체에서 인수로 전달한 키를 갖는 값을 반환한다. Map 객체에서 인수로 전달한 키를 갖는 요소가 존재하지 않으면 undefined를 반환한다.
```js
console.log(map.get(lee)); // developer
console.log(map.get('key')); // undefined
```
##### 요소가 존재하는지 확인하려면 has 메서드를 사용한다. has는 불리언 값을 반환한다.
```js
console.log(map.has(lee)); // true
console.log(map.has('key')); // false
```

