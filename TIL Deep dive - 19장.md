###### 프로토타입 - 상위 객체 역할을 하는 존재. 프로토타입은 자신의 모든 프로퍼티와 메서드를 자신을 통해 만들어진 인스턴스들에게 상속한다.


### 자바스크립트는 명령형/함수형/프로토타입기반 객체지향 프로그래밍을 지원하는 멀티 패러다임 프로그래밍 언어다.
___

##### 자바스크립트는 클래스 기반 객체지향 프로그래밍 언어보다 효율적이며 더 강력한 객체지향 프로그래밍 능력을 지니고 있는 프로토타입 기반의 객체지향 프로그래밍 언어다.

>#### 클래스 Class
##### ES6에서 도입된 클래스는 생성자 함수와 마찬가지로 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작하지는 않는다. 클래스는 생성자 함수보다 엄격하며 생성자 함수가 제공하지 않는 기능도 제공한다.

##### 자바스크립트에서 원시 타입의 값을 제외한 나머지 값들은 모두 객체라는 점을 상기하며 나머지 파트를 읽어보자.
___
### 객체지향 프로그래밍

##### 객체지향 프로그래밍은 실세계의 개체를 프로그래밍에 접목하려는 시도로부터 시작한다. 개체의 특징이나 성질을 나타내는 속성(attribute / property)을 갖고, 이를 통해 개체간의 다름을 인식하고 구별한다.
##### 속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조를 객체라 하며, 객체지향 프로그래밍은 독립적인 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임이다.
1. ##### 객체의 속성은 상태를 나타내는 데이터와 동작으로 나눌 수 있다.
2. ##### 상태는 객체가 갖는 고유한 속성(프로퍼티)이고 동작은 그 값을 가지고 조작하는 함수(메서드)다.

3. ##### 따라서 객체는 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조라고 할 수 있다.

4. ##### 객체는 고유의 기능을 갖는 독립적인 부품으로 볼 수 있지만, 다른 객체와의 관계성도 가질 수 있다.
5. ##### 객체는 다른 객체와 메세지를 주고 받거나 데이터를 처리할 수 있다.
6. ##### 객체는 다른 객체의 상태데이터나 동작을 상속 받아 사용하기도 한다.

___

### 상속과 프로토타입

##### 상속은 객체지향 프로그래밍의 핵심 개념으로, 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다.

##### 자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거한다. 중복의 제거란 기존의 코드를 적극적으로 재사용하는 것이다. 코드 재사용은 개발 비용을 현저히 줄일 수 있는 잠재력이 있으므로 매우 중요하다.

```js
function circle(radius) {
  this.radius = radius;
  this.getArea = function() {
    //Math.PI는 원주율을 나타내는 상수다.
    return Math.PI * this.radius ** 2
  };
}
const circle1 = new Circle(1);
const circle2 = new Circle(2);
```
##### 위와 같이 선언하면 생성자 함수로 생성된 인스턴스는 getArea 메서드를 중복 소유한다.
##### 이처럼 동일한 생성자 함수에 의해 생성된 모든 인스턴스가 동일한 메서드를 중복 소유하는 것은 메모리를 불필요하게 낭비한다. 상속의 불필요한 중복을 제거하기 위해 자바스크립트는 프로토타입을 기반으로 상속을 구현한다.
```js
funcion Circle(radius) {
  this.radius = radius;
  
  Circle.prototype.getArea = function() {
    return Math.PI * this.radius ** 2
  };
}
  const circle1 = new Circle(1);
  const circle2 = new Circle(2);
```
Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받는다.
Circle 생성자 함수가 생성하는 모든 인스턴스는 Circle.prototype에 의해 하나의 getArea를 공유한다.
###### 메서드함수는 non-constructor이다.

상속은 코드의 재사용이란 관점에서 매우 유용하다.
상속은 프로토타입을 통해 이루어진다.
___
### 프로토타입 객체

Prototype은 객체 간 상속(inheritance)을 구현하기 위해 사용된다.
프로토타입은 어떤 객체의 상위(부모)객체의 역할을 하는 객체로서 다른 객체에 공유 프로퍼티(메서드포함)을 제공한다.
프로토타입을 상속받은 하위 객체는 상속받은 상위 객체의 프로퍼티를 자유롭게 사용할 수 있다.

##### 모든 객체는 [[Prototype]]이라는 내부 슬롯을 가지며, 이 내부슬롯의 값은 프로토타입의 참조다.
##### [[Prototype]]에 저장되는 프로토타입은 객체 생성 방식에 의해 결정된다.<br/>예를 들어 객체 리터럴에 의해 생성된 객체의 프로토타입은 Object.prototype이고, 생성자 함수에 의해 생성된 객체는 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.<br/>EX) 

```js
function Hero(){

this.level = level;
  
Hero.prototype.skill(){  
  return 100 * level
   }
}

const hero1 = new Hero(1);
```
sum 객체는 hero1의 프로토타입이다.

#### 모든 객체는 하나의 프로토타입을 갖는다. 그리고 모든 프로토타입은 생성자 함수와 연결되어 있다.
##### [[Prototype]] 내부슬롯에는 (프로퍼티어트리뷰트) 직접 접근이 불가능하고 _proto__ 접근자 프로퍼티를 통해 자신의 프로토타입에 간접적으로 접근할 수 있다.
##### 그리고 프로토타입은 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근할 수 있고, 생성자 함수는 자신의 prototype 프로퍼티를 통해 프로토타입에 접근할 수 있다.



#### _proto__ 접근자 프로퍼티
##### 모든 객체는 _proto__ 접근자 프로퍼티를 통해 자신의 프로토타입, [[Prototype]]에 간접적 접근이 가능하다.접근자 프로퍼티는 [[Get]],[[Set]] 프로퍼티 어트리뷰트를 갖는다.
##### _proto__ 접근자 프로퍼티를 통해 프로토타입에 접근하면 getter 함수인 [[Get]]이 호출된다
##### _proto__접근자 프로퍼티를 통해 새로운 프로토타입을 할당하면 setter 함수인 [[Set]]이 호출된다.

```js
const obj = {};
const parent = { x:1 };

//getter 함수인 get __proto__가 호출되어 obj객체의 프로토타입을 취득한다.
obj.__proto__;

//setter 함수인 set __proto__가 호출되어 obj 객체의 프로토타입을 교체한다.
obj.__proto__ = parent;

console.log(obj.x) // 1
```

```js

__proto__ 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라
object.prototype의 프로퍼티다.
모든 객체는 object.prototype.__proto__ 접근자 프로퍼티를 사용할 수 있다.

const person = {name:'Lee'};

person.hasOwnProperty('__proto__') // false

__proto__프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티다.
모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있다.

```

>#### Object.prototype
##### 모든 객체는 프로토타입 객체에 묶여있다.<br/>자바스크립트 엔진은 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 _proto__접근자 프로퍼티가 가리키는 참조를 따라 부모객체의 프로토타입의 프로퍼티를 순차적으로 검색한다. 모든 프로토타입 체인의 최상위 객체는 Object.prototype이다.
##### _proto__ 접근자 프로퍼티를 사용하는 이유는 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위함이다. 상호 참조라 함은 객체가 서로를 프로토타입으로 참조하려는 것이다. 이 경우 에러를 발생시킨다.
##### 프로토타입은 단방향 링크드 리스트로 구현되어야 한다. 순환참조하는 프로토타입 체인이 만들어지면 최상위 개체가 존재하지 않기 때문에 프로퍼티를 검색할 때 (그 객체에 찾는 프로퍼티가 없다면 부모로 거슬러 올라감) 무한 루프에 빠진다. 따라서 아무런 체크 없이 무조건적으로 프로토타입을 교체할 수 없도록 (알아차리지 못한 체 부모를 바꾸지 못하게) _proto__접근자 프로퍼티를 통해 프로토타입에 접근하고 교체하도록 구현되어있다.


##### 모든 객체가 _proto__ 접근자 프로퍼티를 사용할 수 있는 것은 아니기에 코드 내에서 직접 사용하는 것은 권장하지 않는다. 

##### 따라서 _proto__ 접근자 프로퍼티 대신 프로토타입의 참조를 취득하고 싶은 경우 Object.getPrototypeOf 메서드를 사용하고 교체하고 싶은 경우 Object.setPrototypeOf 메서드를 사용한다.

```js
const obj = {};
const parent = { x: 1 };

Object.getPrototypeOf(obj);


Object.setPrototypeOf(obj, parent);// obj.__proto__ = parent;

obj.x // 1
```


#### 함수 객체의 prototype 프로퍼티
##### 함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.
prototype 프로퍼티는 생성자 함수가 생성할 객체의 프로토타입을 가리킨다. non-constructor인 화살표 함수와 ES6 메서드 축약표현으로 정의한 메서드는 prototype 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.

```js
//Arrow function is non-constructor
const Person name => {
  this.name = name;
}

console.log(Person.hasOwnproperty('prototype')); // false
```
##### 생성자 함수로 호출하기 위해 정의하지 않은 일반 함수도 prototype 프로퍼티를 소유하지만 객체를 생성하지 않는 일반함수의 prototype 프로퍼티는 아무런 의미가 없다.

#### 모든 객체가 Object.prototype으로부터 상속받은 _proto__접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다. 하지만 이들 프로퍼티를 사용하는 주체가 다르다.

>_proto__ : 모든 객체가 소유하며 프로토타입의 참조를 값으로 갖는다. 모든 객체가 사용할 수 있으며 _proto__는 다른 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용된다.
prototype : constructor만이 소유하며 프로토타입의 참조를 값으로 갖는다. 생성자 함수가 사용하며 prototype은 생성자 함수가 자신이 생성할 객체의 프로토타입을 할당하기 위해 사용된다.
___

### 프로토타입의 constructor 프로퍼티와 생성자 함수
##### 모든 프로토타입은 constructor 프로퍼티를 갖는다. 이 constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다. 이 연결은 생성자 함수가 생성될 때, 즉 함수 객체가 생성될 때 이뤄진다.

```js

function Person(name){
  this.name = name;
}

const me = new Person('lee');

console.log(me.constructor === Person);

// me 객체의 생성자 함수는 Person이라는 것은
// me의 constructor는 Person이다와 같은 뜻이다.
```

##### 위 예제에서 Person 생성자 함수는 me 객체를 생성한다. 이때 me 객체는 프로토타입의 constructor 프로퍼티를 통해 생성자 함수와 연결된다. me객체에는 constructor 프로퍼티가 없지만 me 객체의 프로토타입인 Person.prototype에는 constructor 프로퍼티가 있다. 따라서 me 객체는 프로토타입인 Person.prototype의 constructor 프로퍼티를 상속받아 사용할 수 있다.
___           
### 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입
##### 리터럴 표기법에 의해 생성된 객체도 물론 프로토타입이 존재한다. 하지만 <span style='color:gold'>리터럴 표기법에 의해 생성된 객체는 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수 없다.</span>
```js
//obj 객체는 객체 리터럴로 생성했다.
const obj = {};
//하지만 obj 객체의 생성자 함수는 Object 생성자 함수다. 
obj.constructor === Object // true
```
위와 같이 작동하는 이유는 내부적으로 추상연산 OrdinaryObjectCreate를 호출하여 Object.prototype을 프로토타입으로 갖는 빈 객체를 생성하기 떄문이다.

> ##### 추상연산
###### 추상연산은 내부 동작의 구현 알고리즘을 표현한 것으로 ECMAScript 사양에서 설명을 위해 사용되는 함수와 유사한 의사 코드이다.


##### 생성자 함수에 인수를 전달하지 않거나 undefined 또는 null을 인수를 전달하면서 진행되는 추상연산 처럼 내용은 다르기는 하지만 <span style="color:gold">객체 리터럴이 평가될 때에도 OrdinaryObjectCreate를 호출하여 빈 객체를 생성하고 프로퍼티를 추가하도록 정의되어 있다. </span>
##### 호출하여 빈 객체를 생성하는 것은 같지만 객체 리터럴의 평가는 new.target의 확인이나, 프로퍼티를 추가하는 처리 등 세부 내용은 다르다. 어쨋든 객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체가 아니다.(추상연산이 생성함)


___

### 함수를 객체 리터럴로 생성했을때의 constructor

```js

class Foo extends Object {}
new Foo(); // Foo{}
```
##### new.target이 undefined 이거나, Object가 아닌경우 인스턴스 - Foo.prototype - Object.prototype 순으로 프로토타입 체인이 생성된다.

>##### new.target
###### new.target 속성(property)은 함수 또는 생성자가 new 연산자를 사용하여 호출됐는지를 감지할 수 있습니다.
###### new 연산자로 인스턴스화된 생성자 및 함수에서, new.target은 생성자 또는 함수 참조를 반환합니다. 일반 함수 호출에서는, new.target은 undefined입니다.



```js
//foo 함수는 Function 생성자 함수로 생성한 함수 객체가 아니라
//함수 선언문으로 생성했다
function foo() {}

//하지만 constructor 프로퍼티를 통해 확인해보면
// 함수 foo의 생성자 함수는 Function 생성자 함수다.
console.log(foo.constructor === Function) // true
```

##### 함수 객체의 경우 차이가 더 명확한데 Function 생성자 함수에서 살펴보았듯이 Function 생성자 함수를 호출하여 생성한 함수는 전역 함수인 것처럼 스코프를 생성하기에 렉시컬 스코프를 만들지 않고 클로저도 만들지 않는다. 따라서 함수 선언문과 함수 표현식을 평가하여 함수 객체를 생성한 것은 Function 생성자 함수가 아니다. 하지만 constructor 프로퍼티를 통해 확인해보면 함수의 생성자 함수는 Function 생성자 함수다.
##### <span style='color:gold'> 리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요하기에 가상적인 생성자 함수를 갖는다.</span><br/>프로토타입은 생성자 함수와 더불어 생성되며 prototype,constructor 프로퍼티에 의해 연결되어 있기 때문에 프로토타입과 생성자 함수는 언제나 쌍으로 존재한다.

##### 리터럴 표기법에 의해 생성된 객체는 생성자 함수에 의해 생성된 객체는 아니다. 하지만 큰 틀에서 생각해보면 리터럴 표기법으로 생성한 객체도 생성자 함수로 생성한 객체와 본질적인 면에서 큰 차이는 없다.
##### 이 말은 객체,함수 리터럴에 의해 생성된 함수와 Object,Function 생성자 함수에 의해 생성된 함수는 생성 과정과 스코프 클로저 등 차이가 있지만 결국 동일한 특성을 갖는다는 것이다. 따라서 프로토타입의 constructor 프로퍼티를 통해 연결되어 있는 생성자 함수를 리터럴 표기법으로 생성한 객체를 생성한 생성자 함수로 생각해도 크게 무리는 없다.

___

### 프로토타입의 생성 시점

##### 리터럴 표기법에 의해 생성된 객체도 생성자 함수와 연결되는 것을 보았다. 결국 모든 객체는 생성자 함수와 연결되어 있다.
##### 프로토타입은 constructor와 쌍으로 존재하기에 생성자 함수가 생성되는 시점에 더불어 생성된다.
___
### 사용자 정의 생성자 함수와 빌트인 생성자 함수
#### constructor와 non-constructor의 구분에서 살펴본 바와 같이 내부 메서드 [[Construct]]를 갖는 함수 객체, 즉 화살표 함수나 ES6의 메서드 축약 표현으로 정의하지 않고 일반 함수로 정의한 함수 객체는 new 연산자와 함께 생성자 함수로서 호출할 수 있다.

>#### Arrow Function , ES6의 메서드 축약 표현 - new 연산자 사용 불가
#### 일반함수(함수 표현식, 함수 선언문) new 연산자를 통해 생성자 함수로서 호출가능.
###### 일반함수만이 생성자 함수가 될 수 있다.<br/>(내부 메서드 [[Construct]]를 갖는 일반함수)

#### 생성자 함수로서 호출할수 있는 constructor는 <span style='color:gold'>함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.</span>

```js

//함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

console.log(Person.prototype) // {constructor : f}

//생성자 함수
function Person(name){
  this.name = name;
}

생성자 함수 Person의 prototype{constructor : f}의 내부
constructor : f Person(name) // 생성자 함수 person의 constructor 프로퍼티
_proto__ : Object // 접근자 프로퍼티
// 생성된 프로토타입은 오직 constructor 프로퍼티만 갖는다.
// 프로토타입도 객체고 모든 객체는 프로토타입을 가지므로
//프로토타입도 자신의 프로토타입을 갖는다 생성된 프로토타입의 프로토타입은
// Object.prototype이다.
```
#### 위와 같이 생성자 함수보다 console.log를 먼저 써도 평가되는 이유는 Hoisting때문이다. 함수 선언문은 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행된다는 것을 배웠다.<br/> 따라서 함수 선언문으로 정의된 Person 생성자 함수는 어떤 코드보다 먼저 평가되어 함수 객체가 된다.<br/>이때 프로토타입도 더불어 생성되며 생성자 프로토타입은 Person 생성자 함수의 prototype 프로퍼티에 바인딩된다.

##### Person 생성자 함수의 prototype 프로퍼티에 바인딩 된다는 것은 Person 생성자 함수로 생성된 인스턴스들이 단방향 링크드 상위 객체를 생성자 함수 Person으로 갖는다는 것을 말한다.

> ###### 생성자 함수로서 호출할 수 없는 함수, non-constructor는 프로토타입이 생성되지 않는다.<br/>- prototype이 없는 화살표함수,ES6 함축형 메서드는 <span style='color:gold'>상속을 받을수 없다.</span>

#### 위 예시로 이해해야 하는 부분은 빌트인 생성자 함수가 아닌 사용자 정의 생성자 함수는 자신이 평가되어 함수 객체로 생성되는 시점에 프로토타입도 더불어 생성되며 생성된 프로토타입의 프로토타입은 언제나 Object.prototype이다.
___
### 빌트인 생성자 함수와 프로토타입 생성 시점
#### Promise, Date, Function, Number, String 과 같은 빌트인 생성자 함수도 일반 함수와 마찬가지로 생성자 함수가 생성되는 시점에 프로토타입이 생성된다. 
#### 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성된다. 생성된 프로토타입은 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩된다. 모든 빌트인 생성자 함수의 prototype은 Object.prototype이다.

>#### 전역 객체
##### 전역 객체는 코드가 실행되기 이전에 자바스크립트 엔진에 의해 생성되는 특수한 객체다. 전역 객체는 클라이언트 사이드 환경(Browser)에서는 window, 서버 사이드 환경(Node.js)에서는 global 객체를 의미한다.
##### 전역 객체는 표준 빌트인 객체들과 환경에 따른 호스트 객체 그리고 var 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖는다. <br/>Math,Reflect,JSON을 제외한 표준 빌트인 객체는 모두 생성자 함수다.
```js
window.Object === Object // true
빌트인 객체는 전역 객체 window의 프로퍼티다.
```
표준 빌트인 객체인 Object도 전역 객체의 프로퍼티이며 전역 객체가 생성되는 시점에 생성된다.

#### 이처럼 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화되어 존재한다. 이후 생성자 함수 또는 리터럴 표기법으로 생성하면 프로토타입은 생성된 객체의 [[Prototype]]내부 슬롯에 할당된다. 이로써 생성된 객체는 프로토타입을 상속받는다.

___

### 객체 생성 방식과 프로토타입의 결정

-	##### 객체 리터럴
-	##### Object 생성자 함수
-	##### 생성자 함수
-	##### Object.create 메서드
-	##### 클래스(ES6)

##### 각 객체 생성 방식마다 차이는 있으나 추상연산 OrdinaryObjectCreate에 의해 생성된다는 공통점이 있다.<br/>추상연산 OrdinaryObjectCreate는 필수적으로 자신이 생성할 객체의 프로토타입을 인수로 전달받는다.
##### 추상연산 OrdinaryObjectCreate는 빈 객체를 생성한 후, 객체에 추가할 프로퍼티 목록이 인수로 전달된 경우 프로퍼티를 객체에 추가한다. 그리고 인수로 전달받은 프로토타입을 자신이 생성한 객체의 [[Prototype]]내부 슬롯에 할당한 다음, 생성한 객체를 반환한다.
##### 즉 프로토타입은 추상 연산 OrdinaryObjectCreate에 전달되는 인수(자신이 생성할 객체의 프로토타입)에 의해 결정된다. 이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정된다.
___

### 객체 리터럴에 의해 생성된 객체의 프로토타입.
#### 자바스크립트 엔진은 객체 리터럴을 평가하여 객체를 생성할 때 추상연산 OrdinaryObjectCreate를 호출한다. 이때 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype이다.<br/>즉 객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype이다. 

##### <span style='color:gold'> 객체 리터럴이 평가 될떄 추상연산에 의해 Object 생성자 함수, Object.prototype,생성된 객체 사이에 연결이 이루어진다.</span> 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 프로토타입으로 갖게 되고 그 프로퍼티를 상속받는다. obj객체는 Object.prototype의 메서드를 자신의 것처럼 자유롭게 사용할 수 있게 된 것이다.
```js
const obj = { x: 1 };

//객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); //true
console.log(obj.hanOwnProperty('x')); // true
```
___

### Object 생성자 함수에 의해 생성된 객체의 프로토타입
##### Object 생성자 함수 또한 추상연산을 통해 생성되는데, 프로토타입은 Object.prototype을 객체 리터럴 형식과 같이 상속받는다.
##### 다만 객체 리터럴과 생성자 함수에 의한 객체 생성 방식의 차이는 프로퍼티를 추가하는 방식에 있다. 객체 리터럴 방식은 객체 리터럴 내부에 프로퍼티를 추가하지만, Object 생성자 함수 방식은 일단 빈 객체를 생성한 후 프로퍼티를 추가해야 한다.
___

### 생성자 함수에 의해 생성된 객체의 프로토타입
##### new연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하면 다른 객체와 마찬가지로 추상 연산이 호출된다.<span style='color:gold'> 이때 추상 연산에 전달되는 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.</span> 즉 생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 prototpye 프로퍼티에 바인딩되어 있는 객체다.(생성자 함수 또한 다른 생성자 함수를 통해 만들어져 있을 수 있고, _proto__를 통해 프로토타입이 바뀌었을 수도 있다.)

##### 표준 빌트인 객체인 object 생성자 함수와, 더불어 생성된 프로토타입 Object.prototype은 다양한 빌트인 메서드를 갖고 있다.(hasOwnProperty,propertyIsEnumerable 등)<br/> 하지만 사용자 정의 생성자 함수 Person과 더불어 생성된 프로토타입 Person.prototype의 프로퍼티는 constructor 뿐이다.
##### 프로토타입은 객체이므로 일반 객체와 같이 프로토타입에도 프로퍼티를 추가/삭제할 수 있다. 그리고 그렇게 추가/삭제한 프로퍼티는 프로토타입 체인에 즉각 반영된다.

___

### instanceof 연산자

##### instanceof 연산자는 이항 연산자로서 좌변에 객체를 가리키는 식별자, 우변에 생성자 함수를 가리키는 식별자를 피연산자로 받는다. 만약 우변의 피연산자가 함수가 아닌 경우에는 TypeError가 발생한다.

##### 우변의 생성자 함수의 prototype에 바인딩 된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true로 평가되고 그렇지 않은 경우 false로 평가된다.
```js
function Person(name){
  this.name = name;
}

const me = new Person('Lee');

//Person.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Person) // true
console.log(me instanceof Object) // true

const parent = {};

Object.setPrototypeOf(me, parent);

// setPrototype을 통해 프로토타입이 교체되었고, 프로토타입 체인이 끊겼다.
console.log(me instanceof Person); // false

// 교체한 parent의 프로토타입도 Object를 프로토타입으로 갖고 있어 true이다. 
console.log(me instanceof Object); // true

```
##### setPrototypeOf로 me의 부모객체가 된 parent에 Person 생성자함수의 prototype 프로퍼티에 바인딩하면 me의 prototype에 Person이 들어온다 .
```js
Person.prototype = parent

console.log( me instanceof Person) // true
console.log( me instanceof Object) // true
```

##### 이처럼 instanceof 연산자는 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수를 찾는 것이 아니라 생성자 함수의 prototype에 바인딩 된 객체가 프로토타입 체인상에 존재하는지 확인한다.
___

### 직접 상속

#### Object.create에 의한 직접 상속
##### Object.create 메서드는 프로토타입을 지정하여 새로운 객체를 생성한다.<br/> 첫번째 매개변수에는 생성할 객체의 프로토타입이 될 객체,두번째 매개변수에는 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터객체로 이루어진 객체를 전달한다. 두번째 매개변수는 옵셔널이다.

```js

let obj = Object.create(null)

//null로 생성되어 Object.prototype을 상속받지 못함.
console.log(obj.toString()); // is not a function.
```
___

```js
//아래의 방법은 생성자 함수로 생성하는 것과 동일하다.
obj = Object.create(Person.prototype);
obj.name = 'Lee'
```
##### Object.create 메서드는 첫번째 매개변수에 전달할 객체의 프로토타입 체인에 속하는 객체를 생성한다. 즉 객체를 생성하면서 직접적으로 상속을 구현하는 것이다. Object.create 메서드의 장점은 다음과 같다.

- ##### new 연산자가 없이도 객체를 생성할 수 있다.
- ##### 프로토타입을 지정하면서 객체를 생성할 수 있다.
- ##### 객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.
___

#### Object.prototype의 빌트인 메서드는 객체가 직접 호출하는 것을 권장하지 않는다.

##### 그 이유는 Object.create 메서드를 통해 프로토타입 체인의 종점에 위치하는 객체를 생성할 수 있기 때문이다. 프로토타입 체인의 종점에 위치하는 객체는 Object.prototype의 빌트인 메서드를 사용할 수 없다. 
##### 따라서 Object.prototype의 빌트인 메서드는 다음과 같이 간접적으로 호출하는 것이 좋다. 
```js
//프로토타입이 null인 객체 생성
const obj = Object.create(null);
obj.a = 1;

console.log(obj.hasOwnProperty('a'));
// 상속받지 못했기에 Error 발생 (obj.hasOwnProperty is not a function)

//Object.prototype의 빌트인 메서드는 객체로 직접 호출하지 않고 다음과 같이 사용

console.log(Object.prototype.hasOwnProperty.call(obj,'a')); //true
```
___
##### 객체 리터럴 내부에서 _proto__에 의한 직접 상속도 가능하다.

```js
ES6

const myProto = { x:10 };
const obj = {
  y: 20,
  __proto__: myProto
};

console.log(obj.x, obj.y) // 10 20
console.log(Object.getPrototypeOf(obj) === myProto); //true
```
___


### 정적 프로퍼티 / 메서드 (Static property / method)

##### 정적 프로퍼티 / 메서드는 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 메서드를 말한다.

```js
function Person(name) {
  this.name = name;
}

//정적 프로퍼티
Person.staticProp = 'static prop';

//정적 메서드
Person.staticMethod = function() {
  console.log('staticMethod');
};

const me = new Person('Lee');

me.staticMethod(); // me.staticMethod is not a function
```

##### 위 예제처럼 생성자 함수도 객체이므로 자신의 프로퍼티/메서드를 소유할 수 있으며 이를 정적 프로퍼티/메서드라고 한다. 인스턴스는 정적 프로퍼티/메서드를 참조/호출할 수 없다.

##### 앞에서 살펴본 Object.create 메서드는 생성자 함수의 정적 메서드이고, hasOwnProperty메서드는 prototype 메서드이다. 

```js
const obj = Object.create({ name: 'Lee'});

obj.hasOwnProperty('name'); // false
```
##### this를 사용하지 않은 메서드는 정적 메서드로 변경할 수 있다. 인스턴스가 호출한 인스턴스/프로토타입 메서드 내에서 this는 인스턴스를 가리킨다. 프로토타입 메서드를 호출하려면 인스턴스를 생성해야 하지만, 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있다.


>##### MDN과 같은 문서를 보면 STATIC / PROTOTYPE 프로퍼티,메서드를 구분하여 소개하고 있다. 따라서 표기법만으로도 STATIC/PROTOTYPE 프로퍼티,메서드를 구별할수 있어야 한다
##### prototype을 #으로 표기한 문서도 있으니 참고하자.
___

### 프로퍼티 존재 확인

#### in 연산자
```js
key in object

key : 프로퍼티 키를 나타내는 문자열
object : 객체로 평가되는 표현식
```

```js
const person = {
  name: 'Lee',
  address: 'Seoul'
};

console.log('name' in person) // true
console.log('address' in person) // true
```
##### 사용시 주의해야 할 점은 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티를 확인한다.

##### in 연산자 대신 ES6에서 도입된 Reflect.has 메서드를 사용할 수도 있다. in 연산자와 동일하게 동작한다.
___

### Object.prototype.hasOwnProperty 메서드

##### Object.prototype.hasOwnProperty 메서드는 객체에 특정 프로퍼티가 존재하는지 확인할 수 있다. 고유 프로퍼티 키인 경우에만 true를 반환한다. 즉 Object.prototype 에게서 상속받은 메서드는 false다.
___

### 프로퍼티 열거

#### for ... in 문<br/> 객체의 모든 프로퍼티를 순회하며 enumeration하려면 for ...in문을 사용한다. 
```js
const myOffice = {
  address : 'gangnam'
  pay : 'more than 35,000,000 won for year'
};

for (const key in person){
  console.log(key + ': ' + person[key]);
};
  // address: 'gangnam' pay: 'more than 35,000,000 won for year'
  ```
##### for ... in문은 객체의 프로퍼티 개수만큼 순회하며 for... in문의 변수 선언문에서 선언한 변수에 프로퍼티 키를 할당한다.<br/>key라는 변수에 프로퍼티 키를 할당한다는 점을 꼭 기억하자.

##### for... in문은 in연산자처럼 순회 대상 객체의 프로퍼티뿐만 아니라 프로토타입의 프로퍼티까지 열거하지만 Object.prototype의 프로퍼티가 열거되지 않는데, 그 이유는 열거할 수 없도록 [[Enumerable]]이라는 프로퍼티 어트리뷰트의 값이 false이기 때문이다.

#### 따라서 for ...in 문을 정의하자면 프로토타입 체인상 존재하는 모든 프로토타입의 프로퍼티 중 Enumerable의 값이 true인 프로퍼티를 순회하며 열거한다.

##### 또한 프로퍼티키가 문자열이 아닌 Symbol인 프로퍼티도 열거하지 않는다.
___
##### 상속받은 프로퍼티는 제외하고 객체 자신의 프로퍼티만 열거하려면  Object.prototype.hasOwnProperty 메서드를 사용하여 객체 자신의 프로퍼티인지 확인해야 한다.
```js
const person = {
  name: 'Lee',
  address: 'Seoul',
  __proto__: {age:20}
};

for (const key in person) {
  if(!person.hasOwnProperty(key)) continue;
  console.log(key + ': ' + person[key]);
}
// name: Lee
// address: Seoul
```

##### 프로퍼티가 정의된 순서대로 열거되었지만 100% 순서를 보장하지는 않는다. 대부분의 모던 브라우저는 순서를 보장하고 문자로 된 숫자열인 프로퍼티 키에 대해서는 정렬을 실시한다.

##### 배열에서는 for ...in문보다 for문, for...of문, forEach메서드를 사용하길 권장한다.
```js

const arr = [1,2,3];
arr.x = 10; // 배열도 객체이므로 프로퍼티를 가질 수 있다.

for(const i in arr){
  //프로퍼티 x도 출력된다.
  console.log(arr[i]) // 1 2 3 10
};

arr.forEach(v => console.log(v)); // 1,2,3

for(const value of arr) {
  console.log(value); // 1,2,3
};
```
___

### Object.keys/values/entries 메서드

#### 객체 자신의 고유 프로퍼티만 열거하기 위해서는 위의 메서드를 사용하는 것을 권장한다.

##### ES6) Object.keys는 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환한다.
##### ES8) Object.values는 객체 자신의 열거 가능한 프로퍼티 값을 배열로 반환한다.
##### ES8) Object.entries는 객체 자신의 열거 가능한 프로퍼티 키와 값의 쌍의 배열을 배열에 담아 반환한다.

```js
Object.entries

const person = {
  name: 'Lee',
  address: 'Seoul',
  __proto__: { age:20 }
};

console.log(Object.entries(person));
// [["name","Lee"], ["address", "Seoul"]]

Object.entries(person).forEach([key,value]) => console.log(key,value);

/*
name Lee
address Seoul
*/
```

