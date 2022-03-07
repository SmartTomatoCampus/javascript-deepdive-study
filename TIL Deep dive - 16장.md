#### 내부 슬롯과 내부 메서드

##### 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 사용하는 의사 프로퍼티와 의사 메서드다. 이중 대괄호로 감싼 이름들이 내부 슬롯과 내부 메서드인데, 자바스크리브 엔진에서 실제로 동작하지만 개발자가 직접 접근할 수 있도록 외부에 공개된 객체의 프로퍼티는 아니다. 단 일부 내부슬롯과 내부메서드에 한해 간접적으로 접근할 수 있는 수단을 제공한다.

##### 모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다.[[Prototype]]은 <br/> **proto**를 통해 간접적으로 접근할 수 있다.

```js
const o = {};
//내부 슬롯은 자바스크립트 엔진의 내부 로직이므로 직접 접근할 수 없다.
o.[[Prototype]] // Unexpected token '['
o.__proto__ // Object.prototype
```

---

#### 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

##### 자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.

> ###### 프로퍼티의 상태란? <br/> 프로퍼티의 값(value), 값의 갱신 가능 여부(writable), 열거 가능 여부(enumerable), 재정의 가능 여부(configurable)를 말한다.

##### 프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태 값인 내부 슬롯이다.(이중배열)<br/>따라서 프로퍼티 어트리뷰트에 직접 접근할 수 없지만, Object.getOwnPropertyDescriptor 메서드를 사용하여 간접적으로 확인할 수 있다.

```js
const person = {
  name: 'Lee'
};

console.log(Object.getOwnPropertyDescriptor(person,'name')
//{value:"Lee", writable: true, enumerable: true, configurable: true}
```

###### Object.getOwnPropertyDescriptor 메서드의 첫번째 매개변수에는 객체의 참조를 전달하고 두번째 매개변수에는 프로퍼티 키를 문자열로 전달한다.<br/>이때, Object.getOwnPropertyDescriptor 메서드는 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환한다. 만약 존재하지 않는 프로퍼티나 상속받은 프로퍼티에 대한 프로퍼티 디스크립터를 요구하면 undefined가 반환된다.<br/>ES8에 도입된 Object.getOwnPropertyDescriptor<span style='color:gold'>s</span>메서드는 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환한다.

###### 내부슬롯 [[something]].

###### 프로퍼티 어트리뷰트 [[Value]],[[Writable]] 와 같이 식별 가능한 이름이 붙은 내부 슬롯

###### 프로퍼티 디스크립터 프로퍼티 어트리뷰트가 모인 객체<br/>{ value: "Lee", writable: true, enumerable: true, configurable:true }

```js
const person = {
  name: "Lee"
};

person.age = 20;

console.log(Object.getOwnPropertyDescriptors(person));

{
  name: {value: "Lee", writable:true, ... }
  age: { value: 20, writable: true, ... }
}
```

---

#### 데이터 프로퍼티 / 접근자 프로퍼티

##### 프로퍼티는 데이터 프로퍼티와 접근자 프로퍼티로 구분할 수 있다.

- ##### 데이터 프로퍼티(data property)<br/> 키와 값으로 구성된 일반적인 프로퍼티 이다.
- ##### 접근자 프로퍼티(accessor property) <br/> 자체적으로 값을 갖고 있지는 않지만, 데이터 프로퍼티 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티다.

#### 데이터 프로퍼티

##### 데이터 프로퍼티는 4가지의 프로퍼티 어트리뷰트를 갖는다. 이 프로퍼티 어트리뷰트는 자바스크립트 엔진이 프로퍼티를 생성할 때 기본값으로 자동 정의된다.

<table style="text-align:center">
  <tr>
    <td>프로퍼티 어트리뷰트</td><td>프로퍼티 디스크립터<br/>객체의 프로퍼티</td><td>설명</td>
  </tr>
  <tr>
    <td>[[Value]]</td><td>value</td><td><li>프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값이다.</li>프로퍼티 키를 통해 프로퍼티 값을 변경하면 [[Value]]에 값을 재할당한다. 이때 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 [[Value]]에 값을 저장한다.</td>
  </tr>
  <tr>
    <td>[[Writable]]</td><td>writable</td><td><li>프로퍼티 값의 변경 가능 여부를 나타내며 불리언 값을 갖는다.</li><li>[[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없는 읽기 전용 프로퍼티가 된다.</li></td>
  </tr>
<tr>
  <td>[[Enumerable]]</td><td>enumerable</td><td><li>프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖는다.</li><li>[[Enumerable]]의 값이 false인 경우 해당 프로퍼티는 for...in문이나, Object.keys 메서드 등으로 열거할 수 없다.</li></td>
  </tr>
  <tr>
  <td>[[Configurable]]</td><td>configurable</td><td><li>프로퍼티의 재정의 가능 여부를 나타내며 불리언 값을 갖는다.</li><li>[[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제,프로퍼티 어트리뷰트 값의 변경이 금지된다. 단[[Writable]]이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용된다.</li></td>
  </tr>
  </table>

##### 초기의 writable,enumerable,configurable 값은 true이다.

---

#### 접근자 프로퍼티

##### 접근자 프로퍼티는 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티다. 접근자 프로퍼티는 다음과 같은 프로퍼티 어트리뷰트를 갖는다.

<table style="text-align:center">
  <tr>
    <td>프로퍼티 어트리뷰트</td><td>프로퍼티 디스크립터<br/>객체의 프로퍼티</td><td>설명</td>
  </tr>
  <tr>
    <td>[[Get]]</td><td>get</td><td><li>접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수다.</li><li>접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 [[Get]]의 값, getter 함수가 호출되고 그 결과가 프로퍼티 값으로 반환된다.</li></td>
  </tr>
  <tr>
    <td>[[Set]]</td><td>set</td><td><li>접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수다.</li><li>접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 [[Set]]의 값 setter함수가 호출되고 그 결과가 프로퍼티 값으로 저장된다.</li></td>
  </tr>
<tr>
  <td>[[Enumerable]]</td><td>enumerable</td><td><li>프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖는다.</li><li>[[Enumerable]]의 값이 false인 경우 해당 프로퍼티는 for...in문이나, Object.keys 메서드 등으로 열거할 수 없다.</li></td>
  </tr>
  <tr>
  <td>[[Configurable]]</td><td>configurable</td><td><li>프로퍼티의 재정의 가능 여부를 나타내며 불리언 값을 갖는다.</li><li>[[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제,프로퍼티 어트리뷰트 값의 변경이 금지된다. 단[[Writable]]이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용된다.</li></td>
  </tr>
  </table>


##### 접근자 함수는 getter/setter 함수라고도 부른다. 접근자 프로퍼티는 getter/setter함수를 모두 정의할 수도 있고 하나만 정의할 수도 있다.

```js
const person = {
  //데이터 프로퍼티
  firstName: 'Myungseong',
  lastName: 'Kim',

  //fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
  //getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  //setter함수
  set fullName(name) {
    //배열 디스트럭처링 할당 -문자열은 iterable한 유사배열이다.
    //띄어쓰기로 나누어진 문자열을 각 프로퍼티 키의 값으로 할당.
    [this.firstName, this.lastName] = name.split(' ');
  },
};
//데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(person.firstName + ' ' + person.lastname); // MyungSeong Kim

//접근자 프로퍼티를 통한 프로퍼티 값의 저장
//접근자 프로퍼티 fullName에 값을 저장하면 setter함수가 호출된다.
person.fullName = 'Heegun Lee';
console.log(person); // {firstName: "Heegun", lastName: "Lee"}

//접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
console.log(person.fullName); //Heegun Lee

//firstName은 데이터 프로퍼티다.
//데이터 프로퍼티는 4가지의 프로퍼티 어트리뷰트를 갖는다.
let descriptor = Object.getOwnPropertyDescriptor(person, 'firstname');
console.log(descriptor);
//output { value: "Heegun", writable: "true", ...}

//fullName은 접근자 프로퍼티다.
//접근자 프로퍼티도 4가지의 프로퍼티 어트리뷰트를 갖는다.
descriptor = Objectg.getOwnPropertyDescriptor(person, 'fullName');
console.log(descriptor);
//output { get: f, set:f, ...}
```

person 객체의 firstName과 lastName 프로퍼티는 일반적인 데이터 프로퍼티다.
get,set메서드는 getter,setter 함수이고, <span style="color:gold">getter/setter 함수의 이름 fullName이 접근자 프로퍼티다.
접근자 프로퍼티는 자체적으로 값(프로퍼티 어트리뷰트[[Value]])을 가지지 않으며 다만 데이터 프로퍼티의 값을 읽거나(get) 저장할 때(set) 관여할 뿐이다.</span>

##### 접근자 프로퍼티 fullName으로 프로퍼티 값에 <span style="color:gold">접근</span>하면 내부적으로 <span style="color:gold">[[Get]]</span> 내부 메서드가 호출되어 다음과 같이 동작한다.

1. ##### 프로퍼티 키의 유효성을 확인한다. 프로퍼티 키는 문자열 또는 심벌이어야 한다. 여기서의 프로퍼티 키 fullName은 문자열이므로 유효한 프로퍼티 키다.

###### fullName이 <span style="color:gold">프로퍼티 키</span>가 될 수 있는 이유?

###### 객체 내에서 함수명은 프로퍼티키, 함수 블록은 프로퍼티 값이다.

2. ##### 프로토타입 체인에서 프로퍼티를 검색한다.(부모 객체까지 찾는다는 의미)<br/> person 객체에 fullName 프로퍼티가 존재한다.
3. ##### 검색된 fullName 프로퍼티가 데이터/접근자 프로퍼티인지 확인한다. fullName은 접근자 프로퍼티다.
4. ##### 접근자 프로퍼티 fullName의 프로퍼티 어트리뷰트 [[Get]]의 값, getter 함수를 호출하여 그 결과를 반환한다.

> ##### 프로토타입 (prototype)

##### 프로토타입은 어떤 객체의 상위 객체의 역할을 하는 객체다. 프로토타입은 하위 객체에게 자신의 프로퍼티와 메서드를 상속한다.<br/>프로토타입의 프로퍼티,메서드를 상속받은 하위 객체는 자신의 프로퍼티,메서드처럼 자유롭게 사용할 수 있다.

##### 프로토타입 체인은 프로토타입이 단방향으로 연결된 상속 구조를 말한다.<br/>객체의 프로퍼티나 메서드에 접근하려고 할 때, 해당 객체에 접근하려는 프로퍼티 또는 메서드가 없다면 프로토타입 체인을 따라 검색한다.

---

#### 프로퍼티 정의

##### 프로퍼티 정의란 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것을 말한다.

##### Object.defineProperty 메서드를 사용하면 프로퍼티의 어트리뷰트를 정의할 수 있다. 인수로는 객체의 참조, 프로퍼티의 키인 문자열, 프로퍼티 디스크립터 객체를 전달한다.

```js
const person = {};

Object.defineProperty(person, 'firstName', {
  value: 'MyungSeong',
  writable: true,
  enumerable: true,
  configurable: true,
});
Object.defineProperty(person, 'lastName', {
  value: 'Kim',
});

// 디스크립터 객체의 프로퍼티를 누락하고 정의한 lastName은 undefined,false가 기본값이된다.)
let descriptor = Object.getOwnPropertyDescriptor(person, 'lastName');
console.log('lastName', descriptor);
// lastName {value: "Kim", writable: false, enumerable: false configurable : false}

//[[Enumerable]]
// Enumerable이 false인 경우 for...in, Object.keys 등으로 열거할 수 없다.
// lastName property는 [[Enumerable]]값이 false이므로 열거되지 않는다.
console.log(Object.keys(person)); // ["firstName"]

//[Writable]]
// Writable이 false인 경우 해당 프로퍼티의 Value 값을 변경할 수 없다.
// lastName은 false이므로 값이 변경되지 않는다. 에러는 발생하지 않고 무시된다.
person.lastName = 'Lee'; // 무시된다.

//[[Configurable]]
//Configurable이 false인 경우 값을 삭제하거나 재정의 할 수 없다.
// lastName은 false이므로 값이 변경되지 않는다. 에러는 발생하지 않고 무시된다.
delete person.lastName; // 무시된다.
Object.defineProperty(person, 'lastName', { enumerable: true });
// output : Cannot redefine property: lastName

_;
// 접근자 프로퍼티 정의
Object.defineProperty(person, 'fullName', {
  get() {
    return `${this.firstName} ${this.lastName}`;
  },
  set(name) {
    [this.firstName, this.lastName] = name.split(' ');
  },
  enumerable: true,
  configurable: true,
});

person.fullName = 'Heechan Choi';
console.log(person); // { firstName: "Heechan" lastName: "Choi"}
```

##### Object.getOwnpropertyDescriptor<span style="color:gold">s</span>와 같이 Object.defineProperty<span style="color:gold">s</span>를 통해 여러 개의 프로퍼티를 한 번에 정의할 수 있다.

---

#### 객체 변경 방지

##### 객체는 변경 가능한 값이므로 재할당 없이 직접 변경할 수 있다. 자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공한다. 객체 변경 방지 메서드들은 객체의 변경을 금지하는 강도가 다르다.

1. #### Object.preventExtensions(객체확장금지)<br/> 프로퍼티 어트리뷰트 중 프로퍼티 추가를 제외한 나머지 어트리뷰트는 사용이 가능하다.
2. #### Object.seal(객체 밀봉)<br/>프로퍼티의 추가와 삭제 어트리뷰트 재정의가 불가능하며 읽고 쓰기만 가능하다.
3. #### Object.freeze(객체 동결) <br/> readonly

---

#### 객체 확장 금지

##### Object.preventExtensions 메서드는 객체확장을 금지한다.프로퍼티를 추가할 수 있는 방법인 프로퍼티 동적 추가와 Object.defineProperty메서드 모두 금지된다.<br/>확장이 가능한 객체인지 확인하는 메서드는 Object.isExtensible이다.

---

#### 객체 밀봉

##### Object.seal 메서드는 객체를 밀봉한다. 밀봉된 객체는 읽기와 쓰기만 가능하며 Object.isSealed 메서드를 통해 확인할 수 있다.

---

#### 객체 동결

##### Object.freeze 메서드는 객체를 동결한다. 동결된 객체는 읽기와 쓰기만 가능하다. Object.isFrozen 메서드를 통해 확인할 수 있다.

---

#### 지금까지 살펴본 변경 방지 메서드들은 얕은 변경 방지로 직속 프로퍼티만 변경이 방지되고 중첩 객체까지는 영향을 주지 못한다. 따라서 Object.freeze 메서드로 객체를 동결하여도 중첩 객체까지 동결할 수 없다. 객체의 중첩 객체까지 동결하여 변경이 불가능한 읽기 전용의 불변 객체를 구현하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 Object.freeze 메서드를 호출해야 한다.

```js
function deepFreeze(target) {
  //객체가 아니거나 동결된 객체는 무시하고 객체이고 동결되지 않은 객체만 동결한다
  if (target && typeof target === 'object' && !Object.isFrozen(target)) {
    Object.freeze(target);
    Object.keys(target).forEach((key) => deepFreeze(taget[key]));
  }
  return target;
}

const person = {
  name: 'Lee',
  address: { city: 'Seoul' },
};

//깊은 객체 동결
deepFreeze(person);

console.log(Object.isFrozen(person)); //true
console.log(Object.isFrozen(person.address)); // true

person.address.city = 'Busan';
console.log(person); //{name: "Lee", address: {city: "Seoul"}
```
