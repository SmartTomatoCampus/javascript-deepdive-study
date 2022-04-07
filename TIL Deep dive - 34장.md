### Iteration protocol

<span style="font-size:14px"> ES6에서 도입된 이터레이션 프로토콜은 순회 가능한 데이터 컬렉션을 만들기 위해 ECMAScript 사양에 정의하여 미리 약속한 규칙이다.
ES6 이전의 순회 가능한 데이터 컬렉션, 즉 배열, 문자열, 유사 배열 객체, DOM 컬렉션 등은 통일된 규약 없이 각자 나름의 구조를 가지고 for문, for...in문,forEach 메서드 등 다양한 방법으로 순회할 수 있었다.
ES6에서는 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 for ... of 문, 스프레드 문법, 배열 디스트럭쳐링 할당의 대상으로 사용할 수 있도록 일원화했다.
Iteration protocol은 iterable protocol, iterator protocol로 나뉜다.
  ### Iteration Protocol의 2가지 종류
  - #### Iterable protocol</span>
  <span style="font-size:14px"> Well-known Symbol인 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속 받은 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. 이터러블 프로토콜을 준수한 객체를 이터러블이라 한다. 이터러블은 for ... of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭쳐링 할당의 대상으로 사용할 수 있다.</span>
  - #### Iterator potocol
    <span style="font-size:14px"> 이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. 이터레이터는 next 메서드를 소유하며, next 메서드를 호출하면 이터러블을 순회하며 value와 done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다. 이러한 규약을 이터레이터 프로토콜이라 하며, 이터레이터 프로토콜을 준수한 객체를 이터레이터라 한다. 이터레이터는 이터러블의 요소를 탐색하기 위한 포인터 역할을 한다.</span>
  
> #### Well-known Symbol ?
<span style="font-size:14px">자바스크립트가 기본 제공하는 빌트인 심벌 값이 있다.
빌트인 심벌 값은 Symbol 함수의 프로퍼티에 할당되어 있다.
자바스크립트가 기본 제공하는 빌트인 심벌 값을 ECMAScript 사양에서는 Well-known Symbol이라 부른다. Well-known Symbol은 자바스크립트 엔진의 내부 알고리즘에 사용된다.<br/>
예를 들어 Array, String, Map, Set, TypedArray, arguments, NodeList, HTMLCollection 과 같이
  for ... of 문으로 순회 가능한 빌트인 이터러블은 Symbol.iterator을 키로 갖는 메서드를 가지며 Symbol.iterator 메서드를 호출하면 이터레이터를 반환하도록 ECMAScript 사양에 규정되어 있다.<br/>
  빌트인 이터러블은 이러한 규정인 이터레이션 프로토콜을 준수한다.
        <br/>만약 빌트인 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면 이터레이션 프로토콜을 따르면 된다. </span>

___

### 이터러블

<span style="font-size:14px"> 이터러블 프로토콜을 준수한 객체를 이터러블이라 한다. 
  이터러블은 Symbol.iterator를 프로퍼티 키로 사용하는 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체를 말한다.</span>
<span style="font-size:14px"> 예를 들어, **배열**은 Array.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다. 이터러블은 for ... of 문으로 순회할 수 있으며 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.</span>
```js
const array = [1,2,3];
  
  // 배열은 Array.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in array); // true
  // 이터러블인 배열은 for ... of 문으로 순회 가능하다
  for (const item of array) {
  console.log(item);
  }
  
  // 이터러블인 배열은 스프레드 문법의 대상으로 사용할 수 있다.
  console.log([...array]); // [1,2,3]

// 이터러블인 배열은 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.
const [a, ...rest] = array;
console.log(a,rest); // 1,[2,3]
  ```
##### Symbol.iterator 메서드를 직접 구현하지 않거나 상속 받지 않는다면 이터러블 프로토콜을 준수한 이터러블이 아니다. 따라서 일반 객체는 for ... of 문으로 순회할 수 없으며 스프레드 문법과 배열 디스트럭쳐링 할당의 대상으로 사용할 수 없다.
___
### iterator
##### 이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. 이러터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.
<span style="font-size:14px"> 이터레이터의 next 메서드는 이터러블의 각 요소를 순회하기 위한 포인터의 역할을 한다. 즉 next 메서드를 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 순회 결과를 나타내는 이터레이터 리절트 객체를 반환한다.</span>
```js
  // 배열은 이터러블 프로토콜을 준수한 이터러블이다.
  const array = [1,2,3];
  
//Symbol.iterator 메서드는 이터레이터를 반환한다.
const iterator = array[Symbol.iterator]();

console.log(iterator.next()); // { value: 1, done:false};
console.log(iterator.next()); // { value: 2, done:false};
console.log(iterator.next()); // { value: 3, done:false};
console.log(iterator.next()); // { value: undefined, done:true};
```
##### 이터레이터의 next 메서드가 반환하는 이터레이터 리절트 객체의 value 프로퍼티는 현재 순회중인 이터러블의 값을 나타내며 done 프로퍼티는 이터러블의 순회 완료 여부를 나타낸다.
___
### 빌트인 이터러블
<span style="font-size:14px"> 자바스크립트는 이터레잇현 프로토콜을 준수한 객체인 빌트인 이터러블을 제공한다.
  다음의 표준 빌트인 객체들은 빌트인 이터러블이다.</span>
  <table>
    <tr>
      <td>빌트인 이터러블</td><td>Symbol.iterator 메서드</td>
    </tr>
    <tr>
      <td>Array</td><td>Array.prototype[Symbol.iterator]</td>
    </tr>
    <tr>
      <td>String</td><td>String.prototype[Symbol.iterator]</td>
    </tr>
    <tr>
      <td>Map</td><td>Map.prototype[Symbol.iterator]</td>
    </tr>
    <tr>
      <td>Set</td><td>Set.prototype[Symbol.iterator]</td>
    </tr>
    <tr>
      <td>TypedArray</td><td>TypedArray.prototype[Symbol.iterator]</td>
    </tr>
    <tr>
      <td>arguments</td><td>argument[Symbol.iterator]</td>
    </tr>
    <tr>
      <td>Dom Collection</td><td>NodeList.prototype[Symbol.iterator]<br/>
      HTMLCollection.prototype[Symbol.iterator]</td>
    </tr>
  </table>
___

### for ... of 문
##### for ... of 문은 이터러블을 순회하면서 이터러블의 요소를 변수에 할당한다. for...of 문의 문법은 다음과 같다.
```js
for(변수선언문 of 이터러블) { ... }
```
##### <span style="color:gold">for ... of 문은 내부적으로 이터레이터 next 메서드를 호출하여 이터러블을 순회하며 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티 값을 for ... of 문의 변수에 할당한다.</span> 그리고 이터레이터 리절트 객체의 done 프로퍼티 값이 false이면 이터러블의 순회를 계속하고 true이면 이터러블의 순회를 중단한다.
- ##### for ... of 문은 next 메서드를 통해 순회한다. done 프로퍼티 값에 따라 중단할 수 있다.
#### for ... of 문의 동작과 for문으로 표현한 for ... of 문
```js
for (const item of [1,2,3]){
  // item 변수에 순차적으로 value 1, 2, 3이 할당된다.
  console.log(item) // 1 2 3
}

//iterable
const iterable = [1,2,3];

//iterable Symbol.iterator 메서드를 호출하여 이터레이터를 생성한다.
const iterator = iterable[Symbol.iterator]();

for(;;){
  //이터레이터의 next 메서드를 호추랗여 이터러블을 순회한다.
  // 이때 next 메서드는 이터레이터 리절트 객체를 반환한다.
  const res = iterator.next();
  
  // next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 true 이면 이터러블의 순회를 중단한다.
  if(res.done) break;
  
  // iterator 리절트 객체의 value 프로퍼티 값을 item 변수에 할당한다.
  const item = res.value;
  console.log(item); // 1 2 3
}
```

___
### for ... in 문
##### for ... of 문과 for ... in 문의 형식은 매우 유사하다
```js
for (변수선언문 in 객체){ ... }
```
##### for ... in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거한다. 이때 프로퍼티 키가 심벌인 프로퍼티는 열거하지 않는다.
 - ##### for ... in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티를 순회하며 열거한다.
___

### 이터러블과 유사 배열 객체

<span style="font-size:14px"> 유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체를 말한다. 유사 배열 객체는 length 프로퍼티를 갖기 때문에 for 문으로 순회할 수 있고 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로 가지므로 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.</span>
```js
const arrayLike= {
  0: 1,
  1: 2,
  2: 3,
  length:3
};

// 유사 배열 객체는 length 프로퍼티를 갖기 때문에 for 문으로 순회할 수 있다.
for (let i = 0; i < arrayLike.length; i++){
  //유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.
  console.log(arrayLike[i]); // 1 2 3
}
```

**<span style="font-size:14px; color:gold"> 유사 배열 객체는 이터러블이 아닌 일반 객체다. 따라서 유사 배열 객체에는 Symbol.iterator 메서드가 없기 때문에 for ... of 문으로 순회할 수 없다.</span>**
```js
//유사 배열 객체는 이터러블이 아니기 때문에 for ... of 문으로 순회할 수 없다.
for(const item of arrayLike) {
  console.log(item);
}
// is not iterable
```

##### 단 arguments,NodeList,HTMLCollection은 유사배열이면서 이터러블이다. 정확히 말하면 ES6에서 이터러블이 도입되며 유사 배열 객체인 arguments,NodeList, HTMLCollection 객체에 Symbol.iterator 메서드를 구현하여 이터러블이 되었다. 하지만 이터러블이 된 이후에도 length 프로퍼티를 가지며 인덱스로 접근할 수 있는 것에는 변함이 없으므로 유사 배열 객체이면서 이터러블인 것이다.
##### 배열도 마찬가지로 ES6에서 이터러블이 도입되면서 Symbol.iterator 메서드를 구현하여 이터러블이 되었다.

##### Array.from 메서드를 사용하여 배열로 변환할 수도 있다. Array.from 메서드는 유사배열 객체 또는 이터러블을 인수로 전달받아 배열로 변환하여 반환한다.

```js
const arayLike = {
  0:1,
  1:2,
  2:3,
  length:3
};

//Array.from은 유사 배열 객체 또는 이터러블을 배열로 반환한다.
const arr = Array.from(arrayLike);
console.log(arr); // [1,2,3]
```
___

### 사용자 정의 이터러블

##### 이터레이션 프로토콜을 준수하지 않는 일반 객체도 이터레이션 프로토콜을 준수하도록 구현하면 사용자 정의 이터러블이 된다. 
```js

// 피보나치 수열을 구현한 사용자 정의 이터러블
const fibonacci = {
  // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수한다.
  [Symbol.iterator]() {
    let[pre, cur] = [0,1];
    const max = 10; // 수열의 최대값 설정
    
    //Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환해야 하고
    // next 메서드는 이터레이터 리절트 객체를 반환해야 한다.
    return {
      next() {
        [pre,cur] = [cur,pre + cur];
        //이터레이터 리절트 객체를 반환한다.
        return { value: cur, done: cur >=max};
      }
    };
  }
};

// 이터러블인 fibonacci 객체를 순회할 때마다 next 메서드가 호출된다.
for(const num of fibonacci){
  console.log(num); // 1 2 3 5 8
}
```

##### 사용자 정의 이터러블은 이터레이션 프로토콜을 준수하도록 Symbol.iterator 메서드를 구현하고 Symbol.iterator 메서드가 next 메서드를 갖는 이터레이터를 반환하도록 한다. 그리고 이터레이터의 next 메서드는 done과 value 프로퍼티를 가지는 이터레이터 리절트 객체를 반환한다. for ... of 문은 done 프로퍼티가 true가 될 때까지 반복하며 done 프로퍼티가 true가 되면 반복을 중지한다.
##### 이터러블은 for ... of문 뿐만 아니라 스프레드 문법, 배열 디스트럭처링 할당에도 사용할 수 있다.

```js
const arr = [...fibonacci];
console.log(arr); // [1,2,3,5,8]

// 이터러블은 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [first,second, ...rest] = fibonacci;
console.log(first,second,rest); // 1 2 [3,5,8]
```
___
#### 이터러블을 생성하는 함수
##### 최대값 max를 외부에서 받아 사용하는 방법도 있다.
```js
const fibonacciFunc = function(max) {
  let [pre,cur] = [0,1];
  
  return {
    [Symbol.iterator]() {
      return{
        next(){
          [pre,cur] = [cur,pre+cur];
          return { value: cur, done: cur >= max };
        }
      }
    }
  };
};
// 이터러블을 반환하는 함수에 수열의 최대값을 인수로 전달하면서 호출한다.
// fibonacciFunc(10)은 이터러블을 반환한다.
for(const num of fibonacciFunc(10)){
  console.log(num); // 1 2 3 5 8
}
```
