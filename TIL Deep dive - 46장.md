### 제네레이터란?

ES6에서 도입된 제네레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수다.


#### 제네레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.

일반 함수를 호출하면 제어권이 함수에게 넘어가고 함수 코드를 일괄 실행한다. 
즉 함수 호출자는 함수를 호출한 이후 함수 실행을 제어할 수 없다.
제네레이터 함수는 함수 실행을 함수 호출자가 제어할 수 있다.
다시 말해, 함수 호출자가 함수 실행을 일시 중지시키거나 재개시킬 수 있다.
이는 함수의 제어권을 함수가 독점하는 것이 아니라 함수 호출자에게 양도할 수 있다는 것을 의미한다.

#### 제네레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.
일반 함수를 호출하면 매개변수를 통해 함수 외부에서 값을 주입받고 함수 코드를 일괄 실행하여 결과값을 함수 외부로 반환한다.
즉, 함수를 실행하고 있는 동안에는 함수 외부에서 함수 내부로 값을 전달하여 함수의 상태를 변경할 수 없다.
제네레이터 함수는 함수 호출자와 양방향으로 함수의 상태를 주고받을 수 있다.

다시 말해, 제네레이터 함수는 함수 호출자에게 상태를 전달할 수 있고 함수 호출자로부터 상태를 전달받을 수도 있다.

#### 제네레이터 함수를 호출하면 제네레이터 객체를 반환한다.

일반 함수를 호출하면 함수 코드를 일괄 실행하고 값을 반환한다.
제네레이터 함수를 호출하면 함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제네레이터 객체를 반환한다.
___
### 제네레이터 함수의 정의 

제네레이터 함수는 function* 키워드로 선언한다.
그리고 하나 이상의 yield 표현식을 포함한다. 이것을 제외하면 일반 함수를 정의하는 방법과 같다.

```js
// 제네레이터 함수 선언문
function* genDenFunc() {
  yield 1;
}

// 제네레이터 함수 표현식
const genExpFunc = function*() {
  yield 1;
};

//제네레이터 메서드
const obj = {
  * genObjMethod() {
    yield 1;
  }
};

//제네레이터 클래스 메서드

class MyClass {
  * genCLsMethod() {
    yield 1;
  }
}
```

에스터리스크`*`의 위치는 function 키워드와 함수 이름 사이라면 어디든지 상관없다.

제네레이터 함수는 화살표 함수로 정의할 수 없으며 new 연산자와 함께 생성자 함수로 호출할 수 없다.


### 제네레이터 객체

**제네레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제네레이터 객체를 생성해 반환한다.**
제네레이터 함수가 반환한 **제네레이터 객체는 이터러블이면서 동시에 이터레이터다.**

다시 말해, 제네레이터 객체는 Symbol.iterator 메서드를 상속 받는 이터러블이면서 value,done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 next 메서드를 소유하는 이터레이터다. 제네레이터 객체는 next 메서드를 가지는 이터레이터이므로 Symbol.iterator 메서드를 호출해서 별도로 이터레이터를 생성할 필요가 없다.

```js

//제네레이터 함수

function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

// 제네레이터 함수를 호출하면 제네레이터 객체를 반환한다
const generator = genFunc();

// 제네레이터 객체는 이터러블이면서 동시에 이터레이터다.
// 이터러블은 Symbol.iterator 메서들르 직접 구현하거나
// 프로토타입 체인을 총해 상속받은 객체다.

console.log(Symbol.iterator in generator); //true

//이터레이터는 next 메서드를 갖는다.
console.log('next' in generator); // true
```
제너레이터 객체는 next 메서드를 갖는 이터레이터이지만 이터레이터에 없는 return,throw 메서드를 갖는다.
제네레이터 객체의 세 개의 메서드를 호출하면 다음과 같이 동작한다.

- next 메서드를 호출하면 제너레이터 함수의 yield 표현식까지 코드 블록을 실행하고 yield된 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
- return 메서드를 호출하면 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

```js
function* genFunc() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.error(e)
  }
}

const generator = genFunc();

console.log(generator.next()); // { value: 1, done:false }
console.log(generator.return('End!'));; // { value: "End!", done:true}
```
___
throw 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
```js

function* genFunc() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.error(e);
  }
}

const generator = genFunc();

console.log(generator.next()); // { value: 1, done: false }
console.log(generator.throw('Error')); // { value: undefined, done:true }
```

### 제네레이터의 일시 중지와 재개

제네레이터는 yield 키워드와 next 메서드를 통해 실행을 일지 중지했다가 필요한 시점에 다시 재개할 수 있다.

제네레이터 함수를 호출하면 제네레이터 함수의 코드 블록이 실행되는 것이 아니라 제네레이터 객체를 반환한다고 했다.

이터러블이면서 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖는다.
제너레이터 객체의 next 메서드를 호출하면 제너레이터 함수의 코드 블록을 실행한다.

단, 일반 함수처럼 한번에 코드 블록의 모든 코드를 일괄 실행하는 것이 아니라, yield 표현식까지만 실행한다.
yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평과 결과를 제너레이터 함수 호출자에게 반환한다.

```js

function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
// 이터러블이면서 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖는다.

const generator = genFunc();

// 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체 { value, done } 을 반환한다.
// value 프로퍼티에는 첫 번째 yield 표현식에서 yield된 값 1이 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당된다.
console.log(generator.next()); // {value :1, done: false}

// 다시 next 메서드를 호출하면 두 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체 { value, done }을 반환한다.
// value 프로퍼티에는 두 번째 yield 표현식에서 yield된 값 2가 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false 가 할당된다.
console.log(generator.next());

// 다시 next 메서드를 호출하면 세 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체 { value, done }을 반환한다.
// value 프로퍼티에는 세 번째 yield 표현식에서 yield된 값 3이 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false 가 할당된다.
console.log(generator.next());

// 남은 yield 표현식이 없으므로 제너레이터 함수의 마지막까지 실행된다.
// next 메서드는 이터레이터 리절트객체 { value, done }을 반환한다.
// value 프로퍼티에는 제너레이터 함수의 반환값 undefined가 할당된다.
// done 프로퍼티에는 제너리에터 함수가 끝까지 실행되었음을 나타내는 true가 할당된다.
console.log(generator.next()); // { value: undefind, done:true }
```
> ##### 제너레이터 객체의 next 메서드는 value,done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다는 점을 기억하자 
##### 이터레이터의 next 메서드와 달리 제네레이터 next 메서드에는 인수를 전달할 수 있다. 제너레이터 객체의 next 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당된다.
**yield 표현식을 할당받는 변수에 yield 표현식의 평가 결과가 할당되지 않는 것에 주의해야 한다.**

```js

function* genFunc() {
// 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 중지된다
// 이때 yield 값 1은 next 메서드가 반환한 리절트 객체의 value 프로퍼티에 할당된다.
// x 변수에는 아직 아무것도 할당되지 않았다.
// x 변수의 값은 next 메서드가 두 번째 호출될 때 결정된다.
  const x = yield 1;
  
  const y = yield( x + 10 );
  
  return x + y;
}

const generator = genfunc(0);

// 처음 호출하는 next 메서드에는 인수를 전달하지 않는다.
// 만약 처음 호출하는 next 메서드에 인수를 전달하면 무시된다.
let res = generator.next();
console.log(res); // { value: 1, done: false }

// x 변수에 10이 할당된다.
res = generator.next(10);
console.log(res); // { value: 20, done: false}


// y 변수에 20이 할당된다.
res = generator.next(20);
console.log(res); // { value: 30, done: false }
```

##### 이처럼 제네레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있다.
함수 호출자는 next 메서드를 통해 yield 표현식까지 함수를 실행시켜 제네레이터 객체가 관리하는 상태(yield된 값)를 꺼내올 수 있고, next 메서드에 인수를 전달해서 제너레이터 객체에 상태(yield 표현식을 할당받는 변수)를 밀어넣을 수 있다.
이러한 제네레이터 특성을 활용하면 비동기 처리를 동기 처리처럼 구현할 수 있다.
___

### 제너레이터의 활용

#### 피보나치 수열 구현
```js

const infiniteFibonacci = (function* () {
  let [pre,cur] = [0,1];
  
  while (true) {
    [pre, cur] = [cur, pre + cur];
    yield cur;
  }
}());

// infiniteFibonacci는 무한 이터러블이다.
for (const num of infiniteFibonacci) {
  if(num > 10000) break;
  console.log(num); // 1 2 3 5 8 ...
}
```

#### 비동기 처리

제너레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있다. 이러한 특성을 활용하면 프로미스를 사용한 비동기 처리를 동기 처리처럼 구현할 수 있다. 다시 말해, 프로미스의 후속 처리 메서드 then/catch/finally 없이 비동기 처리 결과를 반환하도록 구현할 수 있다. 다음 예제를 살펴보자.

```js
//node-fetch는 Node.js 환경에서 window.fetch 함수를 사용하기 위한 패키지다.
// 브라우저 환경에서 이 예제를 실행한다면 아래 코드는 필요 없다.

const fetch = require('node-fetch');

//제네레이터 실행기

const async = generatorFunc => {
  const generator = generatorFunc() // 2
  
  const onResolved = arg => {
    const result = generator.next(arg); // 5
    
    return result.done
    ? result.value
    : result.value.then(res => onResolved(res)); // 7
  };
  return onResolved // 3
};

(async(function* fetchTodo() { // 1
  const url = 'https://www.some.com/todo/1';
  
  const response = yield fetch(url); // 6
  const todo = yield response.json(); // 8
  console.log(todo);
  // { userid ... }
})()); // 4
```
실행의 순서

>##### 1. async 함수가 호출(1)되면 인수로 전달받은 제너레이터 함수 fetchTodo를 호출하여 제너레이터 객체를 생성하고(2)onResolved 함수를 반환한다.(3) onResolved 함수는 상위 스코프의 generator 변수를 기억하는 클로저다. async 함수가 반환한 onResolved 함수를 즉시호출하여(4) 생성된 제너레이터 객체의 next 메서드를 처음 호출한다.(5)
##### 2. next 메서드가 처음 호출되면 제너레이터 함수 fetchTodo의 첫 번째 yield 문까지 실행된다.(6) 이때 next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 false, 즉 아직 제너레이터 함수가 끝까지 실행되지 않았다면 이터레이터 리절트 객체의 value 프로퍼티 값, 즉 첫 번째 yield된 fetch 함수가 반환한 프로미스가 resolve한 Response 객체를 onResolved 함수에 인수로 전달하면서 재귀 호출한다.(7)
##### 3. onResolved 함수에 인수로 전달된 Response 객체를 next 메서드에 인수로 전달하면서 next 메서드를 두 번째로 호출(5)한다. 이때 next 메서드에 인수로 전달한 Response 객체는 제너레이터 함수 fetchTodo의 response 변수(6)에 할당되고 제너레이터 함수 fetchTodo의 두 번째 yild문(8)까지 실행된다.
##### 4. next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 false, 즉 아직 제너레이터 함수 fetchTodo가 끝까지 실행되지 않았다면 이터레이터 리절트 객체의 value 프로퍼티 값, 즉 두 번째 yield된 response.json 메서드가 반환한 프로미스가 resolve한 todo 객체를 onResolved 함수에 인수로 전달하면서 재귀 호출한다.(7)
##### 5. onResolved 함수에 인수로 전달된 todo 객체를 next 메서드에 인수로 전달하면서 next 메서드를 세 번째로 호출(5) 한다. 이때 next 메서드에 인수로 전달한 todo 객체는 제너레이터 함수 fetchTodo의 todo변수(8)에 할당되고 제너레이터 함수 fetchTodo가 끝까지 실행된다.
##### 6. next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 true, 즉  제너레이터 함수 fetchTodo가 끝까지 실행되었다면 이터레이터 리절트 객체의 value 프로퍼티 값, 즉 제너레이터 함수 fetchTodo의 반환값인 undefined를 그대로 반환(9)하고 처리를 종료한다.

async/await을 사용하면 async 함수와 같은 제네레이터 실행기를 사용할 필요가 없지만 필요하다면 co 라이브러리 사용을 추천한다.

