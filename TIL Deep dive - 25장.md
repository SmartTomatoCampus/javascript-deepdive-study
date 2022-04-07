### 클래스는 프로토타입의 문법적 설탕인가?

##### 자바스크립트는 클래스가 필요 없는 객체지향 프로그래밍 언어다. ES5에서는 클래스 없이도 다음과 같이 생성자 함수와 프로토타입을 통해 객체지향 언어의 상속을 구현할 수 있다.
```js

var Person = (function () {
  //생성자 함수
  function Person(name){
    this.name = name;
  }
  
  //프로토타입 메서드
  Person.prototype.sayHi = function() {
    console.log('Hi' + this.name);
  };
  
  // 생성자 함수 반환
  return Person;
}());

//인스턴스 생성
var me = new Person('Lee');
me.sayHi(); // Hi Lee
```
##### 클래스 기반 언어에 익숙한 프로그래머들은 프로토타입 기반 프로그래밍 방식에 혼란을 느낄 수 있으며 자바스크립트를 어렵게 느끼게 하는 하나의 장벽처럼 인식되었다.
##### ES6에서 도입된 클래스는 기존의 프로토타입 기반 객체지향 프로그래밍보다 자바나 C#과 같은 클래스 기반 객체지향 프로그래밍에 익숙한 프로그래머가 더욱 빠르게 학습할 수 있도록 클래스 기반 객체지향 프로그래밍 언어와 매우 흡사한 새로운 객체 생성 메카니즘을 제시한다. 
##### 클래스와 생성자 함수는 모두 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작하지는 않는다. 클래스는 생성자 함수보다 엄격하며 생성자 함수에서는 제공하지 않는 기능도 제공한다.
___
### 클래스와 생성자함수의 차이점

1. #### 클래스를 new 연산자 없이 호출하면 에러가 발생되지만 생성자 함수를 new 연산자 없이 호출하면 일반함수로서 호출된다.
2. #### 클래스는 상속을 지원하는 extends와 super 키워드를 제공한다. 하지만 생성자 함수는 extends와 super 키워드를 지원하지 않는다.
3. #### 클래스는 호이스팅이 발생되지 않는 것처럼 동작한다. 하지만 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다.
4. 클래스 내의 모든 코드에서는 암묵적으로 strict mode가 지정되어 실행되며 strict mode를 해제할 수 없다.
5. #### 클래스의 constructor,프로토타입 메서드,정적 메서드는 모두 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false로 열거되지 않는다.

##### 생성자 함수와 클래스는 프로토타입 기반의 객체지향을 구현했다는 점에서 매우 유사하지만 클래스는 생성자 함수 기반의 객체 생성 방식보다 견고하고 명료하다. 특히 클래스의 extends와 super 키워드는 상속 관계 구현을 더욱 간결하고 명료하게 한다. 따라서 클래스를 프로토타입 기반 객체 생성 패턴의 단순한 문법적 설탕이라고 보기보다는 새로운 객체 생성 메커니즘으로 보는 것이 좀 더 타당하다.
___
### 클래스 정의
##### 클래스는 함수다. 함수이기에 일급 객체다. 일급 객체이기에 다음과 같은 특징을 갖는다
- ##### 무명의 리터럴로 생성할 수 있다. 즉 런타임에 생성이 가능하다.
- #####  변수나 자료구조에 저장할 수 있다.
- ##### 함수의 매개변수에 전달할 수 있다.
- ##### 함수의 반환값으로 사용할 수 있다.

##### 클래스의 몸체 내부에서는 0개 이상의 메서드만 정의할 수 있다. 클래스 몸체에서 정의할 수 있는 메서드는 constructor(생성자), 프로토타입 메서드, 정적 메서드 3가지다.

```js
//클래스 선언문
class Person {
  //생성자
  constructor(name){
    //인스턴스 생성 및 초기화
    this.name = name; // name 프로퍼티는 public이다.
  }
  
  //프로토타입 메서드
  sayHi(){
    console.log(` Hi ${this.name}`);
  }
  
  //정적 메서드
  static.sayHello(){
    console.log('Hello!');
  }
}

//인스턴스 생성
const me = new Person('Lee')

//인스턴스의 프로퍼티 참조
console.log(me.name); // Lee
//프로토타입 메서드 호출
me.sayHi(); // Hi Lee
//정적 메서드 호출
Person.sayHello(); // Hello! 
```

#### 생성자 함수와 클래스의 정의 방식 비교
```js
// 생성자 함수
var Person = (function(){
function Person(name){
  this.name = name;
  }
  ...
}
// 클래스의 생성자
  Class Person {
constructor(name){
  this.name = name;
  }
  ...
}
```
#### 생성자 함수와 클래스의 프로토타입 메서드 비교
```js

//생성자 함수의 프로토타입 메서드 정의
Person.prototype.sayHi = function() {
  console.log('Hi' + this.name);
};

//클래스의 프로토타입 메서드 정의
sayHi(){
  console.log(`Hi + ${this.name}`); // ES6 template literal
}
```

#### 생성자 함수와 클래스의 정적 메서드
```js

// 생성자 함수의 정적 메서드 정의
Person.sayHello = function() {
  console.log('Hello!');
};

// 클래스의 정적 메서드 정의
static sayHello(){
  console.log('Hello!');
};
```
#### 생성자 함수 반환
```js
return person
}());
```
___

### 클래스 호이스팅

클래스는 함수로 평가된다.
```js
class Person {}

console.log(typeof Person); // function
```
##### 클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 소스코드 평가 과정, 즉 런타임 이전에 먼저 평가되어 함수 객체를 생성한다. 이때 클래스가 평가되어 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 함수인 constructor다. 생성자 함수로서 호출할 수 있는 함수는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다. 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재하기 때문이다. 단, 클래스는 클래스 정의 이전에 참조할 수 없다.

##### 클래스 선언문은 마치 호이스팅이 발생하지 않는 것처럼 보이나 let,const 키워드로 선언한 변수처럼 호이스팅된다. 따라서 클래스 선언문 이전에 일시적 사각지대 (TDZ)에 빠지기 때문에 호이스팅이 되지 않는 것처럼 동작한다.
##### var let const function function* class 키워드를 사용하여 선언된 모든 식별자는 호이스팅된다. 모든 선언문은 런타임 이전에 먼저 실행되기 때문이다.(실행 컨텍스트 생성 후 렉시컬 환경의 명시적 환경 레코드,객체 환경 레코드,함수 환경 레코드에 등록되는 변수,함수,지역변수,전역변수,중첩함수,매개변수)
___
### 인스턴스 생성

##### 클래스는 생성자 함수이며 new 연산자와 함께 호출되어 인스턴스를 생성한다.
```js
class Person {}

//인스턴스 생성
const smallPerson = new Person();
console.log(smallPerson) // Person {}
```
##### 함수는 new 연산자의 사용 여부에 따라 일반 함수로 호출되거나 인스턴스 생성을 위한 생성자 함수로 호출되지만 클래스는 인스턴스를 생성하는 것이 유일한 존재 이유이므로 반드시 new 연산자와 함께 호출해야 한다.

##### 클래스 표현식으로 정의된 클래스의 경우 클래스를 가리키는 식별자를 사용해 인스턴스를 생성하지 않고 기명 클래스 표현식의 클래스 이름을 사용해 인스턴스를 생성하면 에러가 발생한다.
```js
const Person = class MyClass{};

// 클래스를 가리키는 식별자로 인스턴스 생성
const me = new Person();

console.log(MyClass) // is not defined

const you = new MyClass() // is not defined
```
##### 이는 기명함수 표현식과 마찬가지로 클래스 표현식에서 사용한 클래스 이름은 외부 코드에서 접근이 불가능하기 때문이다.
___
### 메서드
##### 클래스 몸체에는 0개 이상의 메서드만 선언할 수 있다. 클래스 몸체에서 정의할 수 있는 메서드는 constructor(생성자), 프로토타입 메서드, 정적 메서드의 세가지가 있다.

> ##### ECMAScript사양(ES11)에 따르면 인스턴스 프로퍼티는 반드시 constructor 내부에서 정의해야 한다. 하지만 클래스 몸체에 메서드뿐만이 아니라 프로퍼티를 직접 정의할 수 있는 새로운 표준 사양이 제안되어 있다. 이 제안 사양에 의해 머지않아 클래스 몸체에서 메서드뿐만 아니라 프로퍼티도 정의할 수 있게 될 것으로 보인다 (크롬과 같은 모던브라우저에서는 이미 사용이 가능하다.)

```js
class Declaration {
  x = 1;
}
const multi = new Declaration();
```

#### constructor
##### constructor는 인스턴스를 생성하고 초기화하기 위한 특수한 메서드다.<br/> constructor는 이름을 변경할 수 없다.
```js
class Person {
  //생성자
  constructor(name) {
    //인스턴스 생성 및 초기화
    this.name = name;
  }
}
```
##### 앞에서 살펴보았듯이 클래스는 인스턴스를 생성하기 위한 생성자 함수다. 클래스의 내부를 들여다 보기 위해 다음 코드를 크롬 브라우저의 개발자 도구에서 실행해보자.
```js
console.log(typeof Person); // function
console.dir(Person);
```
##### 이처럼 클래스는 평가되어 함수 객체가 된다. 함수 객체의 프로퍼티에서 살펴보았듯이 클래스도 함수 객체 고유의 프로퍼티를 모두 갖고 있다. 함수와 동일하게 프로토타입과 연결되어 있으며 자신의 스코프체인을 구성한다.
##### 모든 함수 객체가 가진 prototype 프로퍼티가 가리키는 프로토타입 객체의 constructor 프로퍼티는 클래스 자신을 가리키고 있다. 이는 클래스가 인스턴스를 생성하는 생성자 함수라는 것을 의미한다. 즉 new 연산자와 함께 클레스를 호출하면 클래스는 인스턴스를 생성한다.

##### 클래스가 생성한 인스턴스의 내부를 들여다보자

```js
const me = new Person('Lee');
console.log(me);
```
##### Person 클래스의 constructor 내부에서 this에 추가한 name 프로퍼티가 클래스가 생성한 인스턴스의 프로퍼티로 추가된 것을 확인할 수 있다. 
##### 생성자 함수와 마찬가지로 constructor 내부에서 this에 추가한 프로퍼티는 인스턴스 프로퍼티가 된다. constructor 내부의 this는 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스를 가리킨다.

```js
// class 선언
class Person{
  //생성자
  constructor(name){
    //인스턴스 생성 및 초기화
    this.name = name;
  }
}

// 생성자 함수
function Person(name) {
  //인스턴스 생성 및 초기화
  this.name = name;
}
```

#### 그런데 흥미로운 것은 클래스가 평가되어 생성된 함수 객체나 클래스가 생성한 인스턴스 어디에도 constructor 메서드가 보이지 않는 다는 것이다. 이는 클래스 몸체에 정의한 constructor가 단순한 메서드가 아니라는 것을 의미한다.

##### constructor는 메서드로 해석되는 것이 아니라 <span style="color:gold">클래스가 평가되어 생성한 함수 객체 코드의 일부가 된다. 다시 말해 , 클래스 정의가 평가되면 constructor의 기술된 동작을 하는 함수 객체가 생성된다.</span>

> #### constructor는 메서드로 해석되지 않는다?
##### 클래스의 평가도 함수와 마찬가지로 실행 컨텍스트 생성 후 렉시컬 환경을 구성한다. 렉시컬 환경은 함수 환경 레코드와 외부 렉시컬 환경에 대한 참조로 이루어 져 있다. 함수 환경 레코드에는 지역변수,내부함수,매개변수를 등록한다. 그리고 등록되는 식별자들의 [[Environment]]에 현재 실행 중인 실행 컨텍스트인 클래스가 등록된다. 이때 constructor는 클래스가 갖고 있는 메서드(내장 함수)가 아니고 클래스로 평가되어 생성한 함수 객체 코드의 일부가 된다. 즉 클래스 자체와 인스턴스의 일부가 된다.

> ##### 클래스의 constructor 메서드와 프로토타입의 constructor 프로퍼티
###### 클래스의 constructor 메서드와 프로토타입의 constructor 프로퍼티는 이름이 같아 혼동하기 쉽지만 직접적인 관련이 없다. 프로토타입의 constructor 프로퍼티는 모든 프로토타입이 가지고 있는 프로퍼티이며 생성자 함수를 가리킨다.

___
#### Constructor와 생성자 함수의 몇가지 차이점

- constructor는 클래스 내에서 1개만 존재할 수 있다.
- constructor는 생략할 수 있다.
- constructor를 생략하면 클래스에 다음과 같이 빈 constructor가 암묵적으로 정의된다.
- constructor를 생략한 클래스는 빈 constructor에 의해 빈 객체를 생성한다.
```js
class Person {
  // constructor를 생략하면 아래와 같이 빈 constructor가 암묵적으로 정의된다.
  constructor(){}
}

//빈 객체가 생성된다.
const me = new Person();
console.log(me); // Person{}
```
##### 프로퍼티가 추가되어 초기화 된 인스턴스를 생성하려면 constructor 내부에서 this에 인스턴스 프로퍼티를 추가한다.
```js
class Person {
  constructor() {
    //고정값으로 인스턴스 초기화
    this.name = 'Lee';
    this.address = 'Seoul';
  }
}

// 인스턴스 프로퍼티가 추가된다.
const me = new Person()
console.log(me) // Person { name: 'Lee', address: 'Seoul' }
```
##### 인스턴스를 생성할 때 클래스 외부에서 인스턴스 프로퍼티의 초기값을 전달하려면 다음과 같이 constructor에 매개변수를 선언하고 인스턴스를 생성할 때 초기값을 전달한다. 이때 초기값은 constructor의 매개변수에게 전달된다.
```js
class Person {
  constructor(name,address){
    //인수로 인스턴스 초기화
    this.name = name;
    this.address = address;
  }
}

//인수로 초기값을 전달한다. 초기값은 constructor에 전달된다.
const me = new Person('Lee','Seoul');
console.log(me); // Person {name: "Lee, address: "Seoul" }
```
##### constructor 내에서는 인스턴스의 생성과 동시에 인스턴스 프로퍼티 추가를 통해 인스턴스의 초기화를 실행한다. 따라서 인스턴스를 초기화하려면 constructor를 생략해서는 안된다. constructor는 별도의 반환문을 갖지 않아야 한다. 이는 new연산자와 함께 클래스가 호출되면 생성자 함수와 동일하게 암묵적으로 this, 즉 인스턴스를 반환하기 때문이다.
##### 만약 this가 아닌 다른 객체를 명시적으로 반환하면 this, 즉 인스턴스가 반환되지 못하고 return 문에 명시한 객체가 반환된다.
___
### 프로토타입 메서드
##### 생성자 함수와는 다르게 클래스에서의 프로토타입 메서드는 기본적으로 프로토타입 메서드가 된다.
```js
class Person {
  constructor(name){
    this.name = name;
  }
  
  //프로토타입 메서드
  sayHi(){
    console.log()
  }
}
```
##### 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 된다.
```js
Object.getPrototypeOf(me) === Person.prototype; // true
me instanceof Person // true

Object.getPrototypeOf(Person.prototype) === Object.prototype // true
me instanceof object; // true

me.constructor === Person; // true
```
##### 클래스 몸체에서 정의한 메서드는 인스턴스의 프로토타입에 존재하는 프로토타입 메서드가 된다. 인스턴스는 프로토타입 메서드를 상속받아 사용할 수 있다.
##### 프로토타입 체인은 기존의 모든 객체 생성 방식뿐만 아니라 클래스에 의해 생성된 인스턴스에게도 동일하게 적용된다. 생성자 함수의 역할을 클래스가 한 것일 뿐이다. 
##### 결국 클래스는 생성자 함수와 같이 인스턴스를 생성하는 생성자 함수라고 볼 수 있다. 다시 말해, 클래스는 생성자 함수와 마찬가지로 프로토타입 기반의 객체 생성 메카니즘이다.
___

### 정적 메서드
##### 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말한다.
##### 생성자 함수에서는 명시적으로 생성자 함수에 메서드를 추가해야 했다. 생성자 함수와는 다르게 클래스에서는 static 키워드를 붙이면 정적 메서드가 된다.
##### 정적 메서드는 클래스에 바인딩 된 메서드가 된다. 클래스는 함수 객체로 평가되므로 자신의 프로퍼티/메서드를 소유할 수 있다. 클래스는 클래스 정의가 평가되는 시점에 함수 객체가 되므로 인스턴스와 달리 별다른 생성 과정이 필요 없다. 따라서 정적 메서드는 클래스 정의 이후 인스턴스를 생성하지 않아도 호출할 수 있다.

___

### 프로퍼티
##### 인스턴스 프로퍼티는 constructor 내부에서 정의해야 한다. constructor 내부의 this에는 이미 클래스가 암묵적으로 생성한 인스턴스인 빈 객체가 바인딩되어 있다.
##### 생성자 함수에서 생성자 함수가 생성할 인스턴스의 프로퍼티를 정의하는 것과 마찬가지로 constructor 내부에서 this에 인스턴스 프로퍼티를 추가한다. 이로써 클래스가 암묵적으로 생성한 빈 객체, 즉 인스턴스에 프로퍼티가 추가되어 인스턴스가 초기화된다.
```js
class Person {
  constructor(name){
    this.name = name;
  }
}

const me = new Person('Lee');

console.log(me.name);
```
##### 접근자 프로퍼티는 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티다.
##### 접근자 프로퍼티는 클래스에서도 사용할 수 있다. 위 예제의 객체 리터럴을 클래스로 표현하면 다음과 같다.
```js
class Person {
  constructor(firstName,lastName){
    this.firstName = firstName;
    this.lastName = lastName;
  }
  //fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
  //getter 함수 사용
  get fullName() {
    return ${this.firstName} ${this.lastName}
  }
  
  //setter 함수 사용
  set fullName(name){
    [this.firstName,this.lastName] = name.split(' ');
  }
}

const me = new Person ('Ungmo', 'Lee');

//데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(`${me.firstName} ${me.lastName}`); // Ungmo Lee

//접근자 프로퍼티를 통한 프로퍼티 값의 저장
//접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.

me.fullname = 'Heegun Lee';
console.log(me); // {fistName: "Heegun", lastName: "Lee"}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
console.log(me.fullName); //Heegun Lee

//fullName은 접근자 프로퍼티다.
// 접근자 프로퍼티는 get set enumerable,configurable 프로퍼티 어트리뷰트를 갖는다.

console.log(Object.getOwnPropertyDescriptor(Person.Prototype, 'fullName'));
// {get : f, set: f, enumerable: false, configurable: true}
```

##### 접근자 프로퍼티는 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 떄 사용하는 접근자 함수, 즉 getter와 setter 함수로 구성되어 있다.

##### getter는 인스턴스 프로퍼티에 접근할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용한다. getter는 메서드 이름 앞에 get 키워드를 사용해 정의한다. setter는 인스턴스 프로퍼티에 값을 할당할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용한다. setter는 메서드 이름 앞에 set 키워드를 사용해 정의한다.


##### 이때 getter와 setter 이름은 인스턴스 프로퍼티처럼 사용된다. 다시 말해 getter는 호출하는 것이 아니라 프로퍼티처럼 참조하는 형식으로 사용하며 참조 시 내부적으로 getter가 호출된다. setter도 호출하는 것이 아니라 프로퍼티처럼 값을 할당하는 형식으로 사용하며 할당 시에 내부적으로 setter가 호출된다.
##### getter는 이름 그대로 무언가를 취득할 때 사용하므로 반드시 무언가를 반환해야 하고 setter는 무언가를 프로퍼티를 할당해야 할 때 사용하므로 반드시 매개변수가 있어야 한다. setter는 단 하나의 값만 할당받기 때문에 단 하나의 매개변수만 선언할 수 있다.

##### 클래스의 메서드는 기본적으로 프로토타입 메서드가 된다. 따라서 클래스의 접근자 프로퍼티 또한 인스턴스 프로퍼티가 아닌 프로토타입의 프로퍼티가 된다.

### 클래스 필드 정의 제안

##### 먼저 클래스 필드가 무엇인지 살펴보자. 클래스 필드는 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어다. 클래스 기반 객체지향 언어인 자바의 클래스 정의를 살펴보자. 자바의 클래스 필드는 마치 클래스 내부에서 변수처럼 사용된다.
```js
public class Person {
  // 클래스 필드 정의
  // 클래스 필드는 클래스 몸체에 this 없이 선언해야 한다.
  private String firstName = '';
  private String lastName = '';

//생성자
Person(String firstName, String lastName){
  this.firstName = firstName;
  this.lastName = lastName;
}

public String getFullName(){
  //클래스 필드 참조
  // this 없이도 클래스 필드를 참조할 수 있다.
  return firstName + ' ' + lastName;
}
}
```
##### 자바스크립트의 클래스에서 인스턴스 프로퍼티를 선언하고 초기화하려면 반드시 constructor 내부에서 this에 프로퍼티를 추가해야 한다.
##### 또한 클래스에서 인스턴스 프로퍼티를 참조하려면 반드시 this를 사용하여 참조해야 한다.
##### 클래스 기반 객체지향 언어의 this는 언제나 클래스가 생성할 인스턴스를 가리킨다. this는 주로 클래스 필드가 생성자 또는 메서드의 매개변수 이름과 동일할 때 클래스 필드임을 명확히 하기 위해 사용한다.
##### 클래스 몸체에는 메서드만 선언할 수 있다.
##### 하지만 최신 브라우저에서는 조금 다르게 동작한다. 자바스크립트에서도 인스턴스 프로퍼티를 마치 클래스 기반 개체지향 언어의 클래스 필드처럼 정의할 수 있는 새로운 표준 사양이 2021년도에 제안되어있다.
##### 최신 브라우저와 최신 Node.js에서는 클래스 필드를 클래스 몸체에 정의할 수 있다.
```js
class Person {
  name= 'Lee';
}
const me = new Person();
console.log(me); 
```

##### 클래스 몸체에서 클래스 필드를 정의하는 경우 this에 클래스 필드를 바인딩해서는 안된다. this는 클래스의 constructor와 메서드 내에서만 유효하다.
##### 클래스 필드를 참조하는 경우라도 자바스크립트에서는 this를 반드시 사용해야 한다.
```js
class Person {
  name = 'Lee';

constructor() {
  console.log(name); // is not defined
}
}

new Person();

```
___

##### 클래스 필드에 초기값을 할당하지 않으면 undefined를 갖는다.
```js
class Person { name } ;

const me = new Person();
console.log(me); // Person { name : undefined }
```
____
##### 인스턴스를 생성할 때 외부의 초기값으로 클래스필드를 초기화해야 할 필요가 있다면 constructor에서 클래스 필드를 초기화해야한다.
```js
class Person {
  name;
  constructor(name){
    this.name = name;
  }
}

const me = new Person('Lee');
console.log(me); // Person { name: "Lee" }
```
___
##### 함수는 일급 객체이므로 클래스 필드에 할당할 수 있다. 따라서 클래스 필드를 통해 메서드를 정의할 수도 있다.
```js
class Person {
  name = 'Lee';

getName = function() {
  return this.name;
}
gotName = () => this.name;

const me = new Person();
console.log(me);
console.log(me.getName());
```
##### 클래스 필드에 함수를 할당하는 경우 이 함수는 프로토타입 메서드가 아닌 인스턴스 메서드가 된다. 모든 클래스 필드는 인스턴스 프로퍼티가 되기 때문이다. 따라서 클래스 필드에 함수를 할당하는 것은 권장하지 않는다.
___

### private 필드

##### 클래스 내부에서만 참조할 수 있다.  javascript에서는 식별자 앞에 #을 붙여준다. 클래스 외부에서 private필드에 접근할 수 있는 방법은 없다. 다만 접근자 프로퍼티를 통해 간접적으로 접근할 수 있다. 

```js
class Person {
  #name = '';
  
  constructor(name) {
    this.#name = name;
  }

get name() {
  return this.#name.trim();
}
}

const me = new Person('Lee');
console.log(me.name); //Lee
```

##### private 필드는 반드시 클래스 몸체에 정의해야 한다. constructor에 정의하면 에러가 발생한다.
___
### static 필드
##### static 키워드를 통해 사용한다
___

### 상속에 의한 클래스 확장
##### 중요한 점은 상속에 의한 클래스 확장은 지금까지 살펴본 프로토타입 기반 상속과는 다른 개념이다. 프로토타입 기반 상속은 프로토타입 체인을 통해 다른 객체의 자신을 상속받는 개념이지만, 상속에 의한 클래스 확장은 기존 클래스를 상속 받아 새로운 클래스를 확장하여 정의하는 것이다.
##### 클래스와 생성자 함수는 인스턴스를 생성할 수 있는 함수라는 점에서 매우 유사하다. 하지만 클래스는 상속을 통해 기존 클래스를 확장할 수 있는 문법이 기본저긍로 제공되지만 생성자 함수는 그렇지 않다. 클래스끼리의 상속에서 상속받는 클래스는 상속을 제공하는 클래스의 속성을 그대로 사용하면서 자신만의 고유한 속성을 추가하여 확장할 수 있다. 상속에 의한 클래스 확장은 코드 재사용 관점에서 매우 유용하다.
##### 클래스는 상속을 통해 다른 클래스를 확장할 수 있는 문법인 extends 키워드가 기본적으로 제공된다.
### extends
##### 클래스를 확장하려면 extends 키워드를 사용하여 상속 받을 클래스를 정의한다.
```js
// super(베이스/부모) class
class Base{}

class Derived extends Base{}
```
##### 상속을 통해 확장된 클레스는 서브,자식,파생클레스, 서브클레스에게 상속된 클레스를 수퍼,부모,베이스클래스라 부른다.
##### extends 키워드의 역할은 부모-자식클래스간의 상속 관계를 설정하는 것이다. 클래스도 프로토타입을 통해 상속관계를 구현한다.

##### 수퍼-서브 클래스는 인스턴스의 프로토타입 체인뿐만 아니라 클레스 간의 프로토타입 체인도 생성한다. 이를 통해 프로토타입 메서드, 정적 메서드 모두 상속이 가능하다.
___
### 동적 상속

##### extends 키워드 다음에는 클래스 뿐만 아니라 [[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다. 이를 통해 동적 상속 대상을 결정할 수 있다.
```js

function Base1(){}
class Base2{}

let condition = true;

class Derived extends (condition ? Base1 : Base2 ) {}
```

___
### 서브클래스의 constructor

##### constructor에서 살펴보았듯이 클래스에서 constructor를 생략하면 클레스에 비어있는 constructor가 암묵적으로 정의된다.
##### 서브 클레스에서 constructor를 생략하면 클레스에 앞서 본 암묵적 constructor와는 조금 다른 모습으로 정의된다.
```js
constructor(...args) { super(...args); }
```
##### args는 new 연산자와 함께 클레스를 호출할 때 전달한 인수의 리스트다.
##### super()는 수퍼클래스의 constructor를 호출하여 인스턴스를 생성한다.

```js
class Base {
  // 암묵적 constructor 생성
  //constructor() {}
}

class Derived extends Base{
  // 암묵적 constructor 생성
  // constructor(...args) { super(...args); }
  
}

const derived = new Derived();
console.log(derived); // Derived {}
```

##### 위 예제와 같이 수퍼,서브 모두 constructor를 생략하면 빈 객체가 생성된다 프로퍼티를 소유하는 인스턴스를 생성하려면 constructor내부에서 인스턴스에 프로퍼티를 추가해야 한다.

### super
##### super는 함수처럼 호출도하고 this같이 식별자처럼 참조할 수 있는 특수 키워드다.
- ##### super를 호출하면 수퍼클래스의 constructor를 호출한다.
- ##### super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.

#### super 호출
##### 수퍼클래스의 constructor 내부에서 추가한 프로퍼티를 그대로 갖는 인스턴스를 생성한다면 constructor를 생략할 수 있다. 이때 new 연산자와 함께 서브클레스를 호출하면서 전달한 인수는 모두 서브클레스에 암묵적으로 정의된 constructor의 super 호출을 통해 수퍼클래스의 constructor에 전달된다.
```js
class Base {
  constructor(a,b) {
    this.a = a;
    this.b = b;
  }
}

//서브클래스
class Derived extends Base{
  // 다음과 같이 암묵적으로 constructor가 정의된다.
  // constructor(...args) {super(...args);}
}

const derived = new Derived(1,2)
console.log(derived) // Derived {a:1 , b:2 }
```
___
##### 수퍼클레스에서 추가한 프로퍼티와 서브클레스에서 추가한 프로퍼티를 갖는 인스턴스를 생성한다면 서브클래스의 constructor를 생략할 수 없다. 이때 new 연산자와 함께 서브클래스를 호출하면서 전달한 인수 중에서 수퍼클래스의 constructor에 전달할 필요가 있는 인수는 서브클래스의 constructor에서 호출하는 super를 통해 전달한다.
```js
class SuperBase{
  constructor(a,b)
  this.a = a;
  this.b = b;
}
}

class SubDerived extends SuperBase {
  constructor(a,b,c){
    super(a,b);
    this.c =c;
  }
}

const derived = new Derived(1,2,3);
console.log(derived) // Derived { a:1,b:2,c:3 }
```
new 연산자와 함께 Deried 클래스를 호출하고 전달한 인수 1,2,3은 클래스의 constructor에 전달되고 super 호출을 통해 SuperBase클래스의 constructor에 일부가 전달된다.
이처럼 인스턴스 초기화를 위해 전달한 인수는 수퍼클래스와 서브클래스에 배분되고 상속관계의 두 클래스는 서로 협력하여 인스턴스를 생성한다.

### super 호출 시 주의 사항.

1. ##### 서브클래스에서 constructor를 생략하지 않은경우에는 수퍼클래스에 constructor가 없더라도 서브클래스의 constructor에서는 반드시 super를 호출해야 한다
2. ##### 서브클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없다.
3. ##### super는 반드시 서브클래스의 constructor 내에서만 호출한다.
4. ##### 메서드 내에서 super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.
```js
class Base {
  constructor(name){
    this.name = name;
  }
  
  sayHi(){
    return `Hi + ${this.name}`;
  }
}

class Derived extends Base {
  sayHi(){
    return `${super.sayHi()}`
  }
}

const d = new Derived('Lee');

d.sayHi() // Hi Lee
```

#### [[HomeObject]]
super 참조가 동작하기 위해서는 super를 참조하고 있는 메서드가 바인딩 되어 이쓴 객체의 프로토타입을 찾을 수 있어야 한다. 이를 위해 메서드는 내부 슬롯 [[HomeObject]]를 가지며, 자신을 바인디하고 있는 객체를 가리킨다.
##### 주의할 것은 ES6 메서드 축약 표현으로 정의된 함수만이 [[HomeObject]]를 갖는다는 것이다.
##### [[HomeObject]]를 갖는다는 것은 수퍼클래스의 메서드를 참조할 수 있다는 뜻이다. 즉 ES6의 메서드 축약 표현으로 정의된 함수만이 super의 메서드를 사용할 수 있다.

#### 객체 리터럴의 super참조
```js
const base = {
  name: 'Lee',
  sayHi(){}
}
}

const derived = {
  __proto__:base,
	// es6 메서드 축약 표현으로 [[HomeObject]]를 갖게 됨.  
  sayHi(){return super.sayHi()}
}
}
```
___
### 관계하는 클래스의 인스턴스 생성과정
##### 자바스크립트 엔진은 클레스를 평가할 때 수퍼,서브를 구분하기 위해 base,derived의 값을 갖는 내부 슬롯 [[ConstructKind]]를 갖는다. 다른 클래스를 상속받지 않는 클래스는 내부 슬롯 [[ConstructKind]]의 값은 base, 다른 클래스를 상속받는 서브클래스는 내부슬롯 [[ConstructKind]]의 값이 derived이다.
##### 하지만 서브클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼 클래스에게 생성을 위임한다. 이것이 바로 서브클래스의 constructor에서 반드시 super를 호출해야 하는 이유이다.

##### 수퍼클래스가 생성하더라도 new.target은 서브클래스를 가리킨다. 즉 생성된 인스턴스는 new.target이 가리키는 서브클래스가 생성한 것으로 처리한다