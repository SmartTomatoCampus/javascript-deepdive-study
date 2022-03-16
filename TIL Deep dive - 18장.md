- ##### 무명의 리터럴로 생성할 수 있다.(런타임에 생성이 가능하다)
- ##### 변수나 자료구조(객체,배열)에 저장할 수 있다.
- ##### 함수의 매개변수에 전달할 수 있다.
- ##### 함수의 반환값으로 사용할 수 있다.


##### 위 4가지 조건을 만족하는 객체를 <span style="color:gold">일급 객체</span>라 한다.

___

### <span style="color:gold">함수가 일급 객체인 이유</span>
```js

1.함수는 무명의 리터럴로 생성이 가능하다.
2-1.함수는 변수에 저장할 수 있다.

//런타임(할당)단계에서 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당된다.

const increase = function(num) {
  return ++num;
};

conse decrease = function(num){
  return --num;
};

2-2. 함수는 객체에도 저장할 수 있다.
const auxs = {increase,decrease}


3.함수는 매개변수에 전달할 수 있다.
4.함수는 함수의 반환값으로 사용할 수 있다.

function makeCounter(aux){
  let num = 0;
  
  return function() {
    num = aux(num);
    return num;
  };
}

3-2.함수는 매개변수에게 함수를 전달할 수 있다.
const increaser = makeCounter(auxs.increase);
console.log(increaser()); //1
console.log(increaser()); //2
```

##### 함수가 일급 객체라는 것은 함수를 객체와 동일하게 사용할 수 있다는 의미다. <span style="color:gold">객체는 값이므로 함수는 값과 동일하게 취급할 수 있다.</span>따라서 함수는 값을 사용할 수 있는 곳<span style="color:gold"> (변수할당문,객체의 프로퍼티 값, 배열의 요소,함수 호출의 인수, 함수 반환문)</span>이라면 어디서든지 리터럴로 정의할 수 있으며 런타임에 함수 객체로 평가된다.<br/><span style="color:gold">일급 객체로서 함수가 가지는 가장 큰 특징은 일반 객체와 같이 함수의 매개변수에 전달할 수 있으며, 함수의 반환값으로도 사용할 수 있는 것이다.</span> 이는 함수형 프로그래밍을 가능케하는 자바스크립트의 장점 중 하나다.
##### 함수는 객체이지만, 일반 객체와는 차이가 있다. 일반 객체는 호출할 수 없지만, 함수 객체는 호출할 수 있다. 그리고 함수 객체는 일반 객체에는 없는 함수 고유의 프로퍼티를 소유한다.
___
### 함수 객체의 프로퍼티

```js
function square(number){
  return number * number;
}

console.log(Object.getOwnPropertyDescriptor(square));

```
- ##### length { value : 1, ... }
- ##### name { value : "square", ...}
- ##### arguments { value : null, ... }
- ##### caller {value: null, ...}
- ##### prototype {value: {...}, ...}
```js
console.log(Object.getOwnPropertyDescriptor(square, '__proto'));
// output : undefined

// __proto__는 Object.prototype 객체의 접근자 프로퍼티다.
// 함수는 Object.prototype 객체로부터 __proto__접근자 프로퍼티를 상속받는다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'))
// { get : f , set: f ... }
```
##### 이처럼 arguments,caller,length,name,prototype 프로퍼티는 모두 함수 객체의 데이터 프로퍼티다. 이들 프로퍼티는 일반 객체에는 없는 함수 고유의 프로퍼티다. 하지만 __proto__는 접근자 프로퍼티이며 함수 객체 고유의 프로퍼티가 아니라 Object.prototype 객체의 프로퍼티를 상속받은 것을 알 수 있다.
##### Object.prototype 객체의 프로퍼티는 모든 객체가 상속받아 사용할 수 있다. 즉, Object.prototype 객체의 ____proto____ 접근자 프로퍼티는 모든 객체가 사용할 수 있다.

___
### arguments 프로퍼티

#### 함수 객체의 arguments 프로퍼티 값은 arguments 객체다.<br/> arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 <span style="color:gold"> 유사 배열 객체이며,</span> 함수 내부에서 지역변수처럼 사용되어 함수 외부에서는 참조할 수 없다.
##### 자바스크립트는 함수의 매개변수와 인수의 개수가 일치하는지 확인하지 않기에 개수가 같지 않아도 에러가 발생하지 않는다.

```js
function multiply(x,y){
  console.log(arguments);
  return x * y ;
}
console.log(multyply()) //NaN
console.log(multiply(1)); // NaN
console.log(multiply(1,2)); // 2
console.log(multiply(1,2,3)); // 2
```

##### 함수를 정의할 때 선언한 <span style="color:gold">매개변수는 함수 몸체 내부에서 변수와 동일하게 취급된다. </span><br/> 함수가 호출될 때 함수 몸체 내에서 암묵적으로 매개변수가 선언되고 undefined로 초기화 된 이후 인수가 할당된다.
##### 선언된 매개변수의 개수보다 인수를 적게 전달했을 경우 선언되지 않은 매개변수는 undefined로 초기화 된 상태를 유지한다.
##### 인수가 매개변수보다 많은 경우 무시되나, 버려지는 것이 아니고 arguments 객체의 프로퍼티로 보관된다.

##### arguments 객체는 인수를 프로퍼티 값으로 소유하며 프로퍼티 키는 인수의 순서이다. callee 프로퍼티는 함수 자신을 가리키고 arguments.length 프로퍼티는 인수의 개수를 가리킨다.
            
##### 선언된 매개변수의 개수와 함수를 호출할 때 전달하는 인수의 갯수를 확인하지 않는 자바스크립트 특성상 인수의 개수를 확인하고 이에 따라 함수의 동작을 달리 정의할 필요가 있을 수 있다. 이때 arguments 객체가 유용하게 사용된다.

##### arguments 객체는 매개변수 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용하다.
```js
function sum(){
  let res = 0;
  
  //arguments 객체는 length 프로퍼티가 있는 유사 배열 객체이므로 for문으로 순회할 수 있다.
  for( let i = 0; i < arguments.length; i++){
    res += arguments[i];
  }
  return res;
}

console.log(sum(1,2,3)); // 6
```
#### arguments 객체는 배열 형태로 인자 정보를 담고 있지만 실제 배열이 아닌 유사 배열 객체이다.<span style="color:gold"> 유사 배열 객체란 length 프로퍼티를 가진 객체로 for문으로 순회할 수 있는 객체를 말한다.</span>

>### 유사 배열 객체와 이터러블
#### 이터레이션 프로토콜을 준수하면 순회 가능한 자료구조인 이터러블이 된다. arguments 객체는 유사 배열 겍체이면서, 이터러블이다

##### 유사 배열 객체는 배열이 아니므로 배열 메서드를 사용할 경우 에러가 발생한다. 따라서 배열 메서드를 사용하려면 function.prototype.call, function.prototype.apply를 사용해 간접 호출해야 하는 번거로움이 있다.

```js
function sum() {
  //arguments 객체를 배열로 변환
  const array = Array.prototype.slice.call(arguments);
  return array.reduce(function (pre,cur){
    return pre + cur;
  }, 0);
}

console.log(sum(1,2)); //3
console.log(sum(1,2,3,4,5)); // 15
```

##### ES6 이상부터는 Rest 파라미터를 도입했다.
```js
function sum(...args){
return args.reduce((pre,cur) => pre + cur, 0);
}

console.log(sum(1,2)); //3
console.log(sum(1,2,3,4,5)); //15
```
##### ES6에서 Rest파라미터를 도입하면서 arguments 객체의 중요성이 이전같지는 않지만 알아둘 필요가 있다.
___
### length 프로퍼티
#### 함수 객체의 length 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.
```js
function foo() {}
console.log(foo.length); //0

function bar(x){
  return x;
}
console.log(bar.length); //1

function baz(x,y){
  return x * y;
}
console.log(baz.length); //2
```
##### arguments 객체의 length 프로퍼티와 함수 객체의 length 프로퍼티의 값은 다를 수 있으므로 주의해야 한다.<span style="color:gold"> arguments 객체의 length 프로퍼티는 인자의 개수를 가리키고, 함수 객체의 length 프로퍼티는 매개변수의 개수를 가리킨다.</span>
___
### name 프로퍼티
#### 함수 객체의 name 프로퍼티는 함수 이름을 나타낸다. name 프로퍼티는 ES6에서 정식 표준이 되었다.

##### ES5에서의 name 프로퍼티는 빈 문자열을 값으로 갖는다
##### ES6에서의 name 프로퍼티는 함수 객체를 가리키는 <span style="color:gold">변수 이름의 값으로 갖는다.</span>

#### <span style="color:gold"> 중요한 점은 name 프로퍼티는 함수의 이름이 아닌 함수 객체를 가리키는 식별자라는 것.</span>
___
### prototype 프로퍼티
#### prototype 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor만이 소유하는 프로퍼티다. 일반 객체와 생성자 함수로 호출할 수 없는 non-constructor에는 prototype 프로퍼티가 없다.
##### prototype 프로퍼티는 생성자함수가 생성할 객체의 프로토타입을 가리킨다.
