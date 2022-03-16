### This

#### 객체지향 프로그래밍에서 살펴보았듯이 객체는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조다.

##### 메서드는 자신이 속한 객체의 프로퍼티를 참조하고 변경할 수 있어야 하고, 참조하기 위해서는 먼저 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.
##### 객체 리터럴 방식으로 생성한 객체의 경우 매서드 내부에서 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다.
```js

const circle = {
  radius: 5,
  getDiameter(){
    return 2 * circle.radius;
  }
};

console.log(circle.getDiameter()); //10
```

##### getDiameter 매서드 내에서 메서드 자신이 속한 객체를 가리키는 식별자 Circle을 참조하고 있다. 이 참조 표현식이 평가되는 시점은 getDiameter 메서드가 호출되어 함수 몸체가 실행되는 시점이다.

##### 위 예제의 객체 리터럴은 circle 변수에 할당되기 직전에 평가된다. 따라서 getDiameter 메서드가 호출되는 시점에는 이미 객체 리터럴의 평가가 완료되어 객체가 생성되었고 circle 식별자에 생성된 객체가 할당된 이후다. 따라서 메서드 내부에서 circle 식별자를 참조할 수 있다.
##### 하지만 자기 자신이 속한 객체를 재귀적으로 참조하는 방식은 일반적이지 않으며 바람지갛지도 않다. 생성자 함수 방식으로 인스턴스를 생성하는 경우를 생각해보자.

```js
function Circle(radius){
  리터럴 표기법으로 생성하는 방식과는 다르게 이 시점에는 생성자 함수 자신이
 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
 ????.radius = radius;
}
 Circle.prototype.getDiameter = function(){
   이 시점에도 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
   return 2 * ????.radius;
 };
  생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 한다.
  const circle = new Circle(5);
  ```
  ##### 생성자 함수를 정의하는 시점에는 아직 인스턴스를 생성하기 이전이므로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다. 따라서 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 특수한 식별자가 필요하다. <span style='color:gold'>이를 위해 자바스크립트는 this라는 특수한 식별자를 제공한다.</span>
  
  #### <span style='color:gold'> this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.</span>
  ##### this는 자바스크립트 엔진에 의해 암묵적으로 생성되며, 코드 어디서든 참조할 수 있다. 함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달된다.<br/> 함수 내부에서 arguments 객체를 지역 변수처럼 사용할 수 있는 것처럼 this도 지역 변수처럼 사용할 수 있다.
  ##### <span style='color:gold'> 단, this가 가리키는 값, 즉 this바인딩은 함수 호출 방식에 의해 동적으로 결정된다.</span>
  
  >##### this 바인딩
  ###### 바인딩이란 식별자와 값을 연결하는 과정을 의미한다. <br/> 예를 들어, 변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것이다. <br/>this 바인딩은 this와 this가 가리킬 객체를 바인딩하는 것이다.
  
```js
  const circle = {
  radius: 5,
  getDiameter(){
  //this를 통해 메서드를 호출한 객체를 가리킬 수 있게 되었다.
  return 2 * this.radius
  }
  };
  
  
  console.log(circle.getDiameter()); // 10
  ```
  
  ##### 객체 리터럴의 메서드 내부에서의 this는 메서드를 호출한 객체, 즉 circle을 가리킨다.
  
  ```js
  function Circle(radius){
  
  this.radius = radius;
  
  Circle.prototype.getDiameter = function(){
  return 2 * this.radius;
  };
  
  const circle = new Circle(5);
  console.log(circle.getDiameter()); //10
  ```
  ##### 생성자 함수 내부에서의 this는 생성자 함수가 생성할 인스턴스를 가리킨다. 이처럼 this는 상황에 따라 가리키는 대상이 다르다.
  ##### <span style='color:gold'> 자바스크립트의 this는 함수가 호출되는 방식에 따라 this에 바인딩 될 값, 즉 this 바인딩이 동적으로 결정된다. this는 코드 어디에서든 참조 가능하며 전역에서도 함수 내부에서도 참조할 수 있다.</span>
  
  ```js
  //this는 어디서든지 참조 가능하다.
  // 전역 this는 window를 가리킨다.
  console.log(this) // window
  
  function square(number){
  //일반 함수 내에서의 this는 전역 객체 window를 가리킨다.
  console.log(this); // window
  return number * number;
  }
  square(2);
  
  const person = {
  name: 'Lee',
  getName(){
  // 메서드 내부에서 this는 호출한 객체를 가리킨다.
  console.log(this); // {name: 'Lee', getName: f}
  return this.name;
  }
  };
  console.log(person.getName()); // Lee
  
  function Person(name){
  this.name = name;
  //생성자 함수 내에서의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: 'Lee'}
  };
  const me = new Person('Lee');
  ```
##### strict mode가 적용된 일반 함수에서의 this는 undefined가 바인딩된다. 일반 함수 내부에서 this를 사용할 필요가 없기 때문이다. this는 객체의 프로퍼티,메서드를 참조하기 위한 자기 참조 변수이므로 일반적으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다.
___
#### 함수 호출 방식과 this 바인딩
##### this 바인딩은 함수 호출 방식, 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.<br/> 바인딩이란 this에 묶일 값이다.
>##### 랙시컬 스코프와 this 바인딩은 결정 시기가 다르다.<br/>함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정한다. 하지만 this 바인딩은 함수 호출 시점에 결정된다.

##### 함수를 호출하는 방식은 다음과 같다.

- 일반 함수 호출
- 메서드 호출
- 생성자 함수 호출
- Function.prototype.apply/call/bind 메서드에 의한 간접 호출
```js
const foo = function() {
  console.dir(this);
};

//1. foo 함수를 일반 함수 호출방식으로 호출하면 this는 window를 가리킨다.
foo(); // window

//2.메서드 호출로 foo 함수를 프로퍼티 값으로 할당하여 호출하면 foo 함수 내부의 this는
// 메서드를 호출한 객체 obj를 가리킨다.
const obj = { foo };
obj.foo(); // obj

//3.생성자 함수 호출. foo 함수를 new 연산자와 함께 생성자로 호출하면
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo() // foo {}

//4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출은
// foo 함수 내부의 this는 인수에 의해 결정된다

const bar = { name: 'bar' };
foo.call(bar) // bar
foo.apply(bar) // bar
foo.bind(bar)(); // bar
```

#### 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는 전역 객체가 바인딩된다.
```js
var value = 1;

const obj = {
  value: 100,
  foo(){
    console.log(this); // {value:100, foo: f}
    console.log(this.value) // 100
    
    //bar는 foo 메서드 안에서 정의된 중첩 함수다.
    function bar(){
      console.log(this) // window
      console.log(this.value) // 1
    }
    //메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면
    // 중첩 함수 내부의 this에는 전역 객체가 바인딩된다.
    bar()
  }
}

obj.foo();
```
___
#### 콜백함수가 일반 함수로 호출된다면 콜백 함수 내부의 this에도 전역 객체가 바인딩된다. 어떠한 함수라도 일반 함수로 호출되면 this에 전역 객체가 바인딩된다.
```js
var value = 1;

const obj = {
  value:100,
  foo(){
    console.log(this); // {value:100, foo: f }
    setTimeout(funtion(){
		console.log(this) // window
    	console.log(this.value) // 1;
  },100);
}
}

obj.foo();
```
##### 이처럼 일반 함수로 호출된 모든 함수(중첩,콜백) 내부의 this에는 전역 객체가 바인딩된다.
> ###### setTimeout 함수는 두 번째 인수로 전달한 시간만큼 대기한 다음, 첫 번째 인수로 전달한 콜백 함수를 호출하는 타이머 함수다.

##### 콜백함수, 중첩함수가 호출 될 때 this가 전역객체를 바인딩 하는것은 문제가 있다. 중첩함수와 콜백함수는 외부 함수를 돕는 핼퍼 함수의 역할을 하므로 외부 함수의 일부 로직을 대신하는 경우가 대부분이기에 this또한 메서드와 같은 바인딩을 가져야 한다.<br/> 매서드 내부의 중첩 함수나 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키기 위한 방법은 다음과 같다.
___
    
```js
var value = 1;
const obj = {
  value: 100,
  foo() {
    // this 바인딩(obj)을 변수 that에 할당한다.
    const that = this;
    setTimeout(function() {
      console.log(that.value) // 100
    }, 100);
  }
};

obj.foo()
```
##### 위와 같이 메서드 내부에서 this를 변수에 할당한 뒤 내부함수에 사용하면 같은 this바인딩이 이루어진다.
___

#### 위 방법 이외에도 자바스크립트는 this를 명시적으로 바인딩할수 있는 Function의 prototype인 apply,call,bind를 제공한다.

```js
var value = 1;

const obj = {
  value: 100,
  foo(){
    setTimeout(function(){
      console.log(this.value); // 100
    }.bind(this),100);
  }
};

obj.foo();
```
#### 화살표 함수를 사용해서 this바인딩을 일치시킬 수도 있다.
```js
var value = 1;

const obj = {
  value: 100,
  foo(){
    //화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
    setTimeout(() => console.log(this.value), 100); // 100
  }
};

obj.foo()
```
___

### 메서드 호출
##### 메서드 내부의 this는 메서드를 호출한 객체, 마침표 연산자 앞에 기술한 객체가 바인딩된다. <br/>주의할 것은 메서드 내부의 this는 메서드를 소유한 객체가 아닌 메서드를 호출하 ㄴ객체에 바인딩된다는 것이다.
```js
const person = {
  name: 'Lee',
  getName(){
    //매서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
    return this.name;
  }
};

console.log(person.getName()); //'Lee'
```
##### 위 예제의 getName 메서드는 person 객체의 메서드로 정의되었다. 메서드는 프로퍼티에 바인딩된 함수다.<br/>person 객체의 getName 프로퍼티가 가리키는 함수 객체는 person 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체다. getName 프로퍼티가 함수 객체를 가리키고 있을 뿐이다.<br/> 따라서, getName 메서드는 다른 객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될수도 있고 일반 변수에 할당하여 일반 함수로 호출될 수도 있다.
```js

const anotherPerson = {
  name: 'Kim'
};

//getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName()); // Kim

//gertName 메서드를 변수에 할당.
const getName = person.getName;

console.log(getName()); // ''

//일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
//window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
```

##### 따라서 메서드 내부의 this는 프로퍼티로 메서드를 가리키고 있는 객체와는 관계가 없고 메서드를 호출한 객체에 바인딩된다.(메서드와 객체는 별도의 객체이기 때문이다.)
##### 프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩된다.

___
### 생성자 함수 호출
##### 생성자 함수 내부의 this에는 생성자 함수가 생성할 인스턴스가 바인딩된다.
```js
function Circle(radius){
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```
##### 생성자 함수는 이름 그대로 객체를 생성하는 함수다. 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다. 만약 new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작한다.
___
### Function의 prototype인 apply/call/bind 메서드에 의한 간접호출

##### apply,bind,call메서드는 모든 함수가 상속받아 사용할 수 있다.

#### apply와 call 메서드는 this로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출한다.
```js
function seekOfCigarettePrice(){
  return this;
}

const thisOriginal = { price: 4100 };
const thisPlus = { price : 4200 };
console.log(thisplus());
// window. thisplus가 일반함수로 호출되어 this는 window객체를 가리킨다.
seekofCigarettePrice.apply(thisOriginal); // {price: 4100}
seekofCigarettePrice.call(thisPlus) // {price: 4200}
```
##### apply와 call 메서드의 본질적인 기능은 함수를 호출하는것이다.<br/>apply와 call 메서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 동일하게 동작한다.<br/> 위 예제는 호출할 함수 seekOfCigarettePrice에 인수를 전달하지 않는다. 함수를 호출하면서 인수를 전달해보자.
```js
function seekOfCigarettePrice(HPF,VAT){
  return this.price + this.consumptiontax + HPF + VAT;
}

const thisOriginal = { price: 800,consumptiontax: 1600 };
const thisPlus = { price : 800, consumptiontax:1600 };

console.log(seekOfCigarettePrice.apply(thisOriginal,[1100,500])); // 4000
console.log(seekOfCigarettePrice.call(thisPlus,1100,600)); // 4100
```
___
##### bind 메서드는 apply와 call 메서드와는 달리 함수를 호출하지 않는다. 다만 첫번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.

```js
function seekOfCigarettePrice(HPF,VAT){
  return this.price + this.consumptiontax + HPF + VAT;
}

const thisOriginal = { price: 800,consumptiontax: 1600 };

//bind 메서드는 첫 번째 인수로 전달한 thisOriginal로
//this 바인딩이 교체된 seekOfCigarette함수를 새롭게 생성해 반환한다.
console.log(seekOfCigarettePrice.bind(thisOriginal));

//bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(seekOfCigarettePrice.bind(thisOriginal)(1100,500)) // 4000
```
##### bind 메서드는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this 가 불일치하는 문제를 해결하기 위해 유용하게 사용된다.

```js
const person = {
  name: 'Lee',
  foo(callback){
    setTimeout(callback,100);
  }
};

person.foo(function(){
  console.log(`${this.name}`); // ''
});
```
```js
const person = {
  name: 'Lee',
  foo(callback){
    setTimeout(callback.bind(this),100);
  }
};

person.foo(function(){
  console.log(`${this.name}`); // 'Lee'
});
```

##### 콜백 함수의 내부 this를 외부 함수 this와 일치 시킬 때 bind 메서드를 사용하여 this를 일치 시킬 수 있다.
___
