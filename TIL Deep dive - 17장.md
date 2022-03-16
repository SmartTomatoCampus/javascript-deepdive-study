### 객체 리터럴 이외의 객체 생성 방식.


#### 1.Object 생성자 함수

##### new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다. 빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있다.

```js
const person = new object();

//프로퍼티 동적생성
person.name = 'Lee';
person.sayHello = function() {
  console.log('Hi! my name is ' + this.name);
}

console.log(person); // {name: 'Lee', sayHello: f}
person.sayHello(); // Hi ! My name is Lee
```
##### 자바스크립트는 new 연산자로 String,RegExp, Date, Array,Function,Boolean,Number,Promise등을 빌트인 생성자 함수로 제공한다.


### 2.생성자 함수

#### <span style="color:gold">객체 리터럴에 의한 객체 생성 방식의 문제점 </span>

##### 객체리터럴에 의한 객체 생성은 직관적이고 간편하지만 단 하나의 객체만 생성한다. 따라서 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적이다.
```js

const circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  }
};

const circle2 = {
  radius: 10,
  getDiameter() {
    return 2 * this.radius;
  }
};
```
##### 객체는 프로퍼티를 통해 객체 고유의 상태를 표현한다. 그리고 메서드를 통해 상태 데이터인 프로퍼티를 참조하고 조작하는 동작을 표현한다. (state - behavior)<br/>따라서 프로퍼티는 객체마다 프로퍼티 값이 다를 수 있지만 메서드는 내용이 동일한 경우가 일반적이다.
##### 위 객체는 프로퍼티 구조가 동일하다. radius의 값은 객체마다 다를 수 있지만 getDiameter 메서드는 완전히 동일하다.<br/>하지만 객체 리터럴에 의해 겍체를 생성하는 경우프로퍼티 구조가 동일함에도 불구하고 매번 같은 프로퍼티와 메서드를 기술해야 한다. 앞의 에제처럼 객체가 한두 개라면 넘어갈 수도 있겠지만 만약 수십 개의 객체를 생성해야 한다면 문제가 크다.
___
#### 생성자 함수에 의한 객체 생성 방식의 장점
##### 생성자 함수에 의한 객체 생성 방식은 마치 객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

```js
function Circle(radius){
  //생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
}

//인스턴스의 생성

const circle1 = new Circle(5); 
const circle2 = new Circle(10);
```

### <span style="color:gold"> this </span>
#### <span style="color:gold"> <u>this는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수다.</u> this가 가리키는 값, 즉 <u> this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.</u></span>

- #### 일반함수의 this바인딩: 전역 객체
- #### 메서드의 this바인딩 : 메서드를 호출한 객체(마침표 앞의 객체)
- #### 생성자 함수로서 호출된 this : 생성자 함수가 (미래에)생성할 인스턴스
```js
function foo() {
  console.log(this);
}
foo() // output : window

const obj = { foo } ;
obj.foo(); // obj

//생성자 함수로서 호출
const inst = new foo(); // inst
```
#### <span style="color:gold"> 생성자 함수는 이름 그대로 객체(인스턴스)를 생성하는 함수다. 자바스크립트에서의 생성자 함수는 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다. 만약 new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작한다.</span>
#### 일반 함수로서 호출된 함수는 반환문이 없으므로 암묵적으로 undefined를 반환한다.
___

#### 생성자 함수의 인스턴스 생성 과정

##### 먼저 생성자 함수의 함수 몸체에서 수행해야 하는 것이 무엇인지 생각해보자. 생성자 함수의 역할은 <span style="color:gold"> 프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿(클래스)으로서 동작하여 인스턴스를 생성하는 것과 생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당)하는 것이다.</span><br/> 생성자 함수가 인스턴스를 생성하는 것은 필수이고, 생성된 인스턴스를 초기화하는 것은 옵션이다.

```js
//생성자 함수(template)
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
}
//인스턴스 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
```
##### 생성자 함수 내부의 코드를 살펴보면 this에 프로퍼티를 추가하고 필요에 따라 전달된 인수를 프로퍼티의 초기 값으로 할당하여 인스턴스를 초기화한다. 하지만 인스턴스를 생성하고 반환하는 코드는 보이지 않는다.
##### <span style="color:gold">자바스크립트 엔진은 암묵적인 처리를 통해 인스턴스를 생성하고 반환한다.</span><br/>new연산자와 함께 생성자 함수를 호출하면 자바스크립트 엔진은 암묵적으로 인스턴스를 생성하고 초기화한 후 암묵적으로 인스턴스를 반환한다.
___
### 인스턴스 생성과 this 바인딩
1. #### 암묵적으로 빈 객체가 생성된다. 빈 객체는 미완성 상태이지만 생성자 함수가 생성한 인스턴스다.
2. #### <span style="color:gold"> 암묵적으로 생성된 빈 객체(인스턴스)는 this에 바인딩된다.<br/>생성자 함수 내부의 this가 생성자 함수가 생성할 인스턴스를 가리키는 이유가 바로 이것이다. </span>이 처리는 생성자 함수 몸체의 코드가 한 줄씩 실행되는 런타임 이전에 실행된다.

>#### <span style="color:gold"> 바인딩 (name binding) </span>
##### 바인딩이란 식별자와 값을 연결하는 과정을 의미한다. 예를 들어, 변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것이다.<br/><span style="color:gold"> this 바인딩은 this와 this가 가리킬 객체를 바인딩하는 것이다.</span>
```js
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
  console.log(this); // Circle {}
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
}
```

#### 인스턴스 초기화
##### 생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다.<br/>this에 바인딩되어 있는 인스턴스에 프로퍼티나 메서드를 추가하고 생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당한다.
```js
function Circle(radius){
  //1.암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
  
  //2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
}
```
___
#### 인스턴스 반환
##### 생성자 함수 내부의 모든 처리가 끝나면 (완성된 인스턴스가 바인딩 된) this가 암묵적으로 반환된다.

```js
function Circle(radius){
  //1.암묵적으로 인스턴스가 생성되고 this에 바인딩된다
  
  //2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
  
  //3. (완성된 인스턴스가 바인딩 된) this가 암묵적으로 반환된다
};

//인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: f}
```
##### 만약 this가 아닌 다른 객체를 명시적으로 반환하면 this가 반환되지 못하고 return 문에 명시한 객체가 반환된다.
```js
function Circle(radius){
  //1.암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
  
  //2.this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
  
  // 3.암묵적으로 this를 반환한다.
  // 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다.
  return {};
  
  //인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
  const circle = new Circle(1);
  console.log(circle) // {}
  ```
  ##### 하지만 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.
```js
function Circle(radius) {
  //1.암묵적으로 빈 객체가 생성되고 this에 바인딩된다.

  //2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
  
  // 3.암묵적으로 this 반환한다.
  // 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.
  return 100;
}

//인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1 , getDiameter: f}
```

##### <span style="color:gold">이처럼 생성자 함수 내부에서 명시적으로 this가 아닌 다른 값을 반환하는 것은 생성자 함수의 기본 동작을 훼손한다. 따라서 생성자 함수 내부에서 return문을 반드시 생략해야 한다.</span>
___

#### 내부 메서드 [[call]]과 [[construct]]

##### 함수 선언문 또는 함수 표현식으로 정의한 함수는 일반적인 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다. 생성자 함수로서 호출한다는 것은 new연산자와 함께 호출하여 객체를 생성하는 것을 의미한다.
##### 함수는 객체이므로 일반 객체와 동일하게 동작할 수 있다. 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드를 모두 가지고 있기 때문이다.
```js
//일반 함수도 객체다.
function foo(){}

//함수는 객체이기에 프로퍼티를 소유할 수 있다.
foo.prop = 10;

//함수는 객체이므로 메서드를 소유할 수 있다.
foo.method = function () {
  console.log(this.prop);
};

foo.method(); // 10
```
##### 다만 일반 객체와 다른 점은, 일반 객체는 호출할 수 없지만 함수는 호출할 수 있다는 점이다. 따라서 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드는 물론, 함수로서 동작하기 위해 함수 객체만을 위한 [[Environment]],[[FormalParameters]] 등의 내부 슬롯과 [[Call]], [[Construct]] 같은 내부 메서드를 추가로 갖고 있다.

```js
function foo() {}

//일반적인 함수로서 호출: [[Call]]이 호출된다.
foo();

//생성자 함수로서 호출: [[Construct]]가 호출된다.
new foo();
```

내부 메서드 [[Call]]을 갖는 함수 객체를 callable이라 하며, 내부 메서드 [[Construct]]를 갖는 함수 객체를 constructor, [[Construct]]를 갖지 않는 함수 객체를 non-constructor라고 부른다.
constructor는 생성자함수로서 호출할 수 있는 함수.
non-constructor는 객체를 생성자 함수로서 호출할 수 없는 함수를 의미한다.

모든 함수 객체가 [[Constructor]]를 갖고 있는 것은 아니다.
다시 말해, 함수 객체는 constructor일 수도 있고, non-constructor일수도 있다.

### constrouctor / non-constructor
#### 자바스크립트 엔진이 어떻게 구분하는지 살펴보자. 자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때 함수 정의 방식에 따라 함수를 구분한다.

- ##### constructor : 함수 선언문, 함수 표현식, 클래스(클래스도 함수다).
- ##### non-constructor: 메서드(ES6 메서드 축약 표현), 화살표 함수

<br/>

#### <span style="color:gold">이때 주의할 것은 ECMAScript 사양에서 메서드로 인정하는 범위가 일반적인 의미의 메서드보다 좁다는 것이다.</span>

```js
//일반 함수 정의: 함수 선언문, 함수 표현식
function foo() {}
const bar = function() {};


//프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수다. 이는 메서드로 인정하지 않는다.
const baz = {
  x: function() {}
};

//일반 함수로 정의된 함수만이 constructor다.
new foo(); // foo {}
new bar() // bar{}
new baz.x(); // x {}

//화살표 함수 정의
const arrow = () => {};

new arrow() // arrow is not a constructor

//메서드 정의: ES6의 메서드 축약 표현만 메서드로 인정한다.
const obj = {
  x() {}
};

new obj.x(); //  is not a constructor
```
##### 함수를 프로퍼티 값으로 사용하면 일반적으로 메서드로 통칭하지만 ECMAScript 사양에서 메서드란 ES6의 메서드 축약 표현만을 의미한다. 다시 말해 <span style="color:gold">함수가 어디에 할당되어 있는지가 메서드를 판단하는 것이 아니라, 함수의 정의 방식에 따라 constructor,non-constructor를 구분한다.<span> 따라서 위 예제와 같이 일반함수, 즉 함수 선언문과 함수 표현식으로 정의된 함수만이 constructor이고, ES6의 화살표함수와 메서드 축약 표현으로 정의된 함수는 non-constructor이다.</span>
  
  ##### 함수를 일반 함수로서 호출하면 함수 객체의 내부 메서드 [[call]]이 호출되고 new연산자와 함께 생성자 함수로서 호출하면 내부 메서드 [[Construct]]가 호출된다. 
  
  #### new 연산자
  
  ##### 일반 함수와 생성자 함수에 특별한 형식적 차이는 없다. new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작한다. 다시 말해, 함수 객체의 내부 메서드 [[Call]]이 호출되는 것이 아니라, [[Constructor]]가 호출된다. 단 new 연산자와 함께 호출하는 함수는 constructor이어야 한다.
  
  ```js

  
  // 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출한다면
  // 함수가 객체를 반환하지 않았으므로 반환문이 무시된다.
  // 따라서 빈 객체가 생성되어 반환된다.
  function add (x,y){
  return x + y;
  }
  
  let inst = new add();
  
  console.log(inst); // {}
  

  
  //객체를 반환하는 일반함수
  function createUser(name,role){
  return { name, role };
  }
  
  //일반 함수를 new 연산자와 함께 호출
  inst = new createuser('Lee', 'admin');
  //함수가 생성한 객체를 반환한다.
  console.log(inst); // {name: 'Lee', role: 'admin'}
  ```
  
  ##### 일반 함수와 생성자함수는 특별한 형식적 차이가 없다. 따라서 생성자 함수는 파스칼 케이스로 명명하여 일반 함수와 구별할 수 있도록 노력한다.
  ___
  
  #### new.target
  
  ##### 생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 파스칼케이스컨벤션을 사용한다. 실수는 언제나 발생할 수 있기에 ES6에서는 new.target을 지원한다.
  ##### <span style="color:gold">new.target은 this와 유사하게 constructor인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용되며, 메타 프로퍼티라고 부른다.</span> 참고로 IE는 new.target을 지원하지 않으므로 주의하기 바란다.

##### 함수 내부에서 new.target을 사용하여 new 연산자와 생성자 함수로서 호출했는지 확인할 수 있다. <br/>new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 기리킨다. new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined다.

  ##### 따라서 함수 내부에서 new.target을 사용하여 new연산자와 생성자 함수로서 호출했는지 확인하여 그렇지 않은 경우 new연산자와 함께 재귀 호출을 통해 생성자 함수로서 호출할 수 있다.
  
  ```js
  //생성자 함수
  function Circle(radius){
  //이 함수가 new 연산자와 함꼐 호출되지 않는다면 new.target은 undefined다.
  if(!new.target){
  //new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
  return new Circle(radius);
  }
  this.radius = radius;
  this.getDiameter = funcion() {
  return 2 * this.radius;
  };
  }
  
  //new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
  
  const circle = Circle(5);
  console.log(circle.getDiameter());
  ```
  ___
  #### 스코프 세이프 생성자 패턴
  
  ##### new.target은 IE 지원을 하지 않는다. new.target을 사용할 수 없는 상황이라면 스코프 세이프 생성자 패턴을 사용할 수 있다.
  ```js
  function Circle(radius){
  
  if(!(this instanceof Circle)){
  return new Circle(radius);
  }
  
 this.radius = radius;
  this.getDiameter = function () {
  return 2 * this.radius;
  };
  };
  
  //new 연산자 없이 생성자 함수를 호출하여도 생성자 함수로서 호출된다.
  const circle = circle(5);
  console.log(circle.getdiameter());
  ```
  new 연산자와 함께 생성자 함수에 의해 생성된 객체(인스턴스)는 프로토타입에 의해 생성자 함수와 연결된다. 이를 이용해 new연산자와 함께 호출되었는지 확인할 수 있다.
  