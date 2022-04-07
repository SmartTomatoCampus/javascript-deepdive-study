### 함수의 구분

##### ES6 이전의 함수는 사용 목적에 따라 명확히 구분하지 않아 모든 함수는 일반 함수로서 호출할 수도, 메서드가 될 수도, 생성자 함수로서도 호출할 수 있었다. ES6 이전의 모든 함수는 callable이면서 constructor이다.

> ##### callable과 constructor/non-constructor
###### [[Call]]: 호출할 수 있는 함수 객체
###### [[Construct]] : 인스턴스를 생성할 수 있는 함수 객체

##### 주의할것은 ES6 이전에 일반적으로 메서드라고 부르던 객체에 바인딩 된 함수도 callable이며 constructor라는 것이다. 따라서 객체에 바인딩된 함수도 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수도 있다.
```js
var obj = {
  x: 10,
  f: function() { return this.x; }
};

obj.f() // 10

var bar = obj.f
bar() // undefined : undefined인 이유는 x가 전역변수로 존재하지 않기 때문이다.

new obj.f() // f{}
```

##### 객체에 바인딩된 함수를 생성자 함수로 호출하는 경우가 흔치는 않겠지만 문법상 가능하다는 것은 문제가 있다. 그리고 이는 성능 면에서도 문제가 있다. 객체에 바인딩된 함수가 constructor라는 것은 객체에 바인딩된 함수가 prototype 프로퍼티를 가지며 프로토타입 객체도 생성한다는 것을 의미하기 때문이다.
```js
[1,2,3].map(function (item){ return item * 2 });
//[2,4,6]
```
##### 콜백 함수를 사용하는 고차함수 map<br/>콜백 함수도 constructor이며 프로토타입을 생성한다.
___
##### 이처럼 ES6 이전의 모든 함수는 사용 목적에 따라 구분이 없으므로 호출 방식에 특별한 제약이 없고 생성자 함수로 호출하지 않아도 프로토타입 객체를 생성한다. 이는 혼란스러우며 실수를 유발할 가능성이 있고 성능에도 좋지 않다.
##### 이러한 문제를 해결하기 위해 ES6에서는 함수를 사용 목적에 따라 세가지 종류로 명확히 구분했다.

<table>
  <tr>
    <td>ES6 함수의 구분</td><td>constructor</td><td>prototype</td><td>super</td><td>arguments</td>
  </tr>
  <tr>
    <td>일반함수</td><td>O</td><td>O</td><td>X</td><td>O</td>
  </tr>
  <tr>
    <td>메서드</td><td>X</td><td>X</td><td>O</td><td>O</td>
  </tr>
  <tr>
    <td>화살표 함수</td><td>X</td><td>X</td><td>X</td><td>X</td>
  </tr>
  </table>

##### 일반함수는 함수 선언문이나 함수 표현식으로 정의한 함수를 말하며 ES6 이전의 함수와 차이가 없다. 하지만 ES6의 메서드와 화살표 함수는 명확한 차이가 있다.
##### 일반 함수는 constructor이지만 ES6의 메서드와 화살표 함수는 non-constructor이다.
___
### 메서드

##### ES6 이전 사양에는 메서드에 대한 명확한 정의가 없었다. 일반적으로 메서드는 객체에 바인딩된 함수를 일컫는 의미로 사용되었다.
##### <span style="color:gold"> ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다</span>

```js
const obj = {
  x: 1,
  foo() { return this.x; } // foo는 메서드다
  
  //bar에 바인딩 된 함수는 메서드가 아닌 일반 함수다.
  bar: function() { return this.x; }
};

obj.foo // 1
obj.bar // 1
```

##### ES6 사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor이다. 따라서 생성자 함수로서 호출할 수 없다.
```js
new obj.foo(); // is not a function
```
ES6의 메서드는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.
```js
obj.foo.hasOwnProperty('prototype'); // false
obj.bar.hasOwnProperty('prototype'); // true
```

##### <span style="color:gold"> 표준 빌트인 객체가 제공하는 프로토타입 메서드와 정적 메서드는 모두 non-constructor이다.</span>
```js
String.prototype.toUpperCase.prototype // undefined
Number.isFinite.prototype // undefined
Array.prototype.map.prototype // undefined
Array.from.prototype // undefined
```
##### <span style="color:gold"> ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 갖는다. super 참조는 내부 슬롯 [[HomeObject]]를 사용하여 수퍼클래스의 메서드를 참조하므로 내부 슬롯 [[HomeObject]]를 갖는 ES6메서드는 super 키워드를 사용할 수 있다.</span>

```js
const base = {
  name: 'Lee',
  sayHi() {
  return `Hi! ${this.name}`;
  }
};

const derived = {
  __proto__:base,
  // sayHi는 ES6의 메서드이기에 [[HomeObject]]를 갖는다.
  //sayHi의 [[HomeObject]]는 sayHi가 바인딩된 객체인 derived를 가리키고
  // super는 sayHi의[[HomeObject]]의 프로토타입인 base를 가리킨다.
  sayHi(){
    return `${super.sayHi()}`
  }
};

derived.sayHi()
```

##### ES6 메서드가 아닌 함수는 super키워드를 사용할 수 없다. ES6 메서드가 아닌 함수는 내부 슬롯 [[HomeObject]]를 갖지 않기 때문이다.
##### ES6 메서드는 본연의 기능(super)을 추가하고 의미적으로 맞지 않는 기능(constructor)는 제거했다. 따라서 메서드를 정의할 때 프로퍼티 값으로 익명 함수 표현식을 할당하는 이전의 방식은 사용하지 않는 것이 좋다.
___
### 화살표 함수
##### 화살표 함수는 표현만 간략한 것이 아니라 내부 동작도 기존의 함수보다 간략하다. 특히 화살표 함수는 콜백 함수 내부에서 this가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용하다.
##### 화살표 몸체에서 중괄호를 생략할 수 있는데, 이때 함수 몸체 내부의 문이 값으로 평가될 수 있는 표현식인 문이라면 암묵적으로 반환한다.

##### <span style="color:gold"> 함수 몸체를 감싸는 중괄호{}를 생략한 경우 함수 몸체 내부의 문이 표현식이 아닌 문이라면 에러가 발생한다. 표현식이 아닌 문은 반환할 수 없기 때문이다.</span>
```js
// 함수 몸체가 하나의 문으로 구성된다 해도 함수 몸체의 문이 표현식이 아닌 문이라면 중괄호를 생략할 수 없다
const arrowfunc = () => const x = 1; // Unexpected token 'const'
// 위 표현은 다음과 같이 해석된다.
const arrowfunc = () => { return const x = 1 };

const arrow = () => { const x = 1; } // undefined
```

##### 객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호로 감싸주어야 한다. 소괄호로 감싸지 않으면 객체 리터럴의 중괄호를 함수 몸체를 감싸는 중괄호로 잘못 해석한다.
```js
const create = (id,content) => ({id,contend});
create(1,'javascript') // {id: 1, content: "javascript"}
```
##### 함수 몸체가 여러개의 문으로 구성된다면 중괄호를 생략할 수 없고, 반환값이 있다면 명시적으로 반환을 해야한다.

##### 화살표 함수도 즉시 실행 함수로 사용할 수 있다.
```js
const person = (name => ({
  sayHi() {return `hi? ${name}`;}
}))('Lee');

person.sayHi() // hi? Lee
```

##### 화살표 함수도 일급 객체이므로 map,filter,reduce 같은 고차 함수에 인수로 전달할 수 있다.
```js
[1,2,3].map(v => v * 2);
```

##### 화살표 함수는 콜백 함수로서 정의할 때 유용하다. 화살표 함수는 일반 함수의 기능을 간략화 했으며 this도 편리하게 설계되었다. 일반 함수와 화살표 함수의 차이에 대해 살펴보자.

### 화살표 함수와 일반 함수의 차이

- ##### 화살표 함수는 non-constructor이기에 인스턴스를 생성할 수 없다.<br/>인스턴스를 생성할 수 없기에 프로퍼티도 없고 프로토타입도 생성하지 않는다.
- ##### 중복된 매개변수 이름을 선언할 수 없다. 
```js
const arrowFunc = (a,a,a,a,a,a) => return a+a+a+a+a+a;
// parameter name not allowed in this context
```
>- ##### 화살표 함수는 함수 자체의 this,arguments,super,new.target 바인딩을 갖지 않는다.
##### 따라서 화살표 함수 내부에서 this.arguments,super,new.target을 참조하면 스코프체인을 통해 상위 스코프의 this,arguments,super,new.target을 참조한다.
##### 만약 화살표 함수와 화살표 함수가 중첩되어 있다면 스코프 체인 상에서 가장 가까운 상위 함수의 this,arguments,super,new.target을 참조한다.
___

### <span style="color:gold"> 화살표 함수의 this</span>
##### 화살표 함수는 다른 함수의 인수로 전달되어 콜백 함수로 사용되는 경우가 많다.<br/> 화살표 함수의 this는 일반 함수의 this와 다르게 동작하여 콜백 함수 내부의 this 문제, 내부의 this가 외부 함수의 this와 다르기 때문에 발생하는 문제를 해결하기 위해 의도적으로 설계된 것이다.
##### this의 바인딩은 함수를 호출할 때 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.(즉 스코프가 렉시컬환경에 따라 함수가 어디에 정의되었는지에 따라 정적으로 생성되는 것과는 정 반대의 개념이다)

##### 이때 주의할 것은 일반 함수로서 호출되는 콜백 함수의 경우인데, 고차 함수의 인수로 전달되어 고차 함수 내부에서 호출되는 콜백 함수도 중첩 함수라고 할 수 있다. 주어진 배열의 각 요소에 접두어를 추가하는 다음 예제를 살펴보자.
```js
class Prefixer{
  constructor(prefix){
    this.prefix = prefix;
  }
  
  add(arr) {
    //add 메서드는 인수로 전달된 배열 arr를 순회하며 배열의 모든 요소에 prefix를 추가한다
    
    return arr.map(function(item){
      return this.prefix + item;
      // Cannot read property 'prefix' of undefined
    });
  }
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition','user-select']));
```

##### 위 예제를 실행했을 때 기대하는 결과는 ['-webkit-transition','-webkit-user-select']다. 하지만 결과는 typeError가 발생한다.

##### 그 이유는 프로토타입 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다. 그렇지만 map으로 전달한 콜백함수의 this는 undefined를 가리킨다. map 메서드가 콜백함수를 일반 함수로서 호출하기 때문이다.
##### 일반 함수로서 호출되는 모든 함수 내부의 this는 전역 객체를 가리킨다. 하지만 class 내부의 모든 code에는 stric mode가 암묵적으로 적용 되고, stric mode에서 일반 함수로서 호출된 함 수 내부의 this에는 undefined가 바인딩되므로, 일반 함수로서 호출된 map 메서드의 콜백 함수 내부의 this는 undefined가 바인딩된다.

##### 그래서 es6 이전에는 다음과 같은 방법을 사용했다.
```js
1.
add(arr){
  const that = this; // 메서드 내부에서 정의한 this는 자신을 호출한 객체를 가리킨다.
  return arr.mao(function(item) {
    return that.prefix + ' ' + item;
  })
}

2. map의 두번째 인수로 add 메서드를 호출한 prefixer 객체를 가리키는 this를 호출한다.
두번째 인수로 콜백 함수 내부에서 this를 사용하므로 this로 사용할 객체를 전달할 수 있다는 것이다.

arr(arr){
  return arr.map(function(item){
    return this.prefix + ' ' + item;
  },this); // this에 바인딩 된 값이 콜백 함수 내부의 this에 바인딩된다.
  // 여기서의 this는 map 함수 내부에서 선언된 것이 아니기 때문에 가능하다.
}
```
___
##### function.prototype.bind 메서드를 사용하여 add 메서드를 호출한 prefixer 객체를 가리키는 this를 바인딩한다.
```js

arr(arr){
  return arr.map(function(item){
    return this.prefix + ' ' + item;
  }.bind(this)); // this에 바인딩된 값이 콜백 함수 내부의 this에 바인딩된다.
}
```
ES6에서는 화살표 함수를 사용하여 콜백 함수 내부의 this 문제를 해결할 수 있다.

##### 화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다. 이를 lexical this라 한다. 마치 렉시컬 스코프와 같이 화살표 함수의 this가 정의된 위치에 의해 결정된다는 것을 의미한다.

___
#### 메서드를 화살표함수로 정의하는 것을 피해야 한다.
```js
const person = {
  name: 'Lee',
  sayHi: () => console.log( `Hi `${this.name}`);
};
// sayHi 프로퍼티에 할당된 화살표 함수 내부의 this에는 상위 스코프인 전역의 this가 가리키는
// 전역 객체를 가리키므로 이 예제를 브라우저에서 실행하면 this.name은 빈 문자열을 갖는
// window.name과 같다. 전역 객체 window에는 빌트인 프로퍼티 name이 존재한다.
person.sayHi() // Hi
```

##### 위 예제의 경우 sayHi 프로퍼티에 할당한 화살표 함수 내부의 this는 메서드를 호출한 객체인 person을 가리키지 않고 상위 스코프인 전역의 this가 가리키는 전역 객체를 가리킨다. 따라서 화살표 함수로 메서들르 정의하는 것은 바람직하지 않다. 메서드를 정의할 때는 ES6 메서드 축약 표현으로 정의한 ES6 메서드를 사용하는 것이 좋다.
```js
const person = {
  sayHi() {
    console.log(`Hi ${this.name}`);
  }
};
person.sayHi();
```

##### 프로토타입 객체의 프로퍼티에 화살표 함수를 할당하는 경우도 동일한 문제가 발생한다.

##### 프로퍼티를 동적 추가할때에는 일반 함수를 할당하여 추가한다.
```JS
Foo.prototype.sayHi = function () { console.log() };
```
##### 일반 함수가 아닌 ES6 메서드를 동적 추가하고 싶다면 객체 리터럴을 바인딩하고, 프로토타입의 constructor 프로퍼티와 생성자 함수간의 연결을 재설정한다.
___
### Rest 파라미터

##### Rest 파라미터는 가장 마지막 파라미터에 위치해야 하며 하나만 선언할 수 있다. 또한 함수 정의 시 선언한 매개변수 개수를 나타내는 함수 객체의 length프로퍼티에 영향을 주지 않는다.
```js
function foo(...rest) {}
foo.length // 0
function foo2(x,...rest){}
foo2.length // 1
```

##### ES6에서는 rest 파라미터를 사용하여 가변 인자 함수의 인수 목록을 배열로 직접 전달 받을 수 있다.
```js
function sum(...args) {
  return args.reduce((pre,cur) => pre + cur,0);
}
sum(1,2,3,4,5) // 15
```
##### 함수와 ES6메서드 모두 Rest파라미터와 arguments 객체를 모두 사용할 수 있지만 화살표 함수는 arguments 객체를 갖지 않는다. 따라서 화살표 함수로 가변 인자 함수를 구현해야 할 때는 반드시 Rest파라미터를 이용해야 한다. 
___
### 매개변수 기본값

##### 함수를 호출할 때 매개변수의 갯수만큼 인수를 전달하는 것이 바람직하지만 그렇지 않은 경우에도 에러가 발생하지 않는다. 이는 자바스크립트 엔진이 매개변수의 개수와 인수의 개수를 체크하지 않기 때문이다. 인수가 전달되지 않은 매개변수의 값은 undefined다. 이를 방치하면 의도치 않은 결과가 나온다.
```js
function sum(x,y){
  return x + y;
}
console.log(sum(1)); //NaN
```
##### ES6에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화를 간소화 할 수 있다.
```js
function sum(x = 0 , y = 0){
  return x + y;
}

sum(1,2) // 3
sum(1) // 1
```
##### 매개변수의 기본값은 매개변수에 인수를 전달하지 않는 경우와 값이 undefined를 전달한 경우에만 유효하다.

##### 매개변수 기본값은 함수 정의 시 선언한 매개변수 개수를 나타내는 함수 객체의 length 프로퍼티와 arguments 객체에 아무런 영향을 주지 않는다.
```js
function sum(x, y=0){
  console.log(arguments);
}
console.log(sum.length); // 1
sum(1) // Arguments { '0', 1 }
sum(1,2) // Arguments {'0':1, '1':2}

