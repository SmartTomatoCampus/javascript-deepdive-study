### 자바스크립트 객체의 분류

#### 표준 빌트인 객체
##### 표준 빌트인 객체는 ECMAScript 사양에 정의된 객체를 말하며 애플리케이션 전역의 공통 기능을 제공한다. 표준 빌트인 객체는 자바스크립트 실행 환경과 관계 없이 언제나 사용할 수 있다.<br/> 표준 빌트인 객체는 전역 객체의 프로퍼티로서 제공된다. 따라서 별도의 선언 없이 전역 변수처럼 참조할 수 있다.

#### 호스트 객체
##### 호스트 객체는 ECMAScript 사양에 정의되어 있지 않지만 자바스크립트 실행 환경에서 추가로 제공하는 객체를 말한다.<br/> 브라우저 환경에서는 DOM, BOM, Canvas, XMLHttpRequest, fetch, requestAnimationFrame, SVG, Web Storage, Web Component, Web Worker와 같은 클라이언트 사이드 Web API를 호스트 객체로 제공하고, Node.js 환경에서는 Node.js 고유의 API를 호스트 객체로 제공한다.

#### 사용자 정의 객체
##### 사용자 정의 객체는 사용자가 직접 정의한 객체를 말한다.

___
### 표준 빌트인 객체
##### 자바스크립트는 40여개의 표준 빌트인 객체를 제공하며<span style='color:gold'> Math, Reflect, Json을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다. 생성자 함수 객체인 표준 빌트인 객체는 프로토타입 메서드와 정적 메서드를 제공하고 생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공한다.</span>
> ###### Math가 제공하는 round, floor, ceil과 Json이 제공하는 stringify,parse 등은 정적 메서드로 제공되기에 프로토타입을 통해 사용할 수 없다.

#### 표준 빌트인 객체를 생성자 함수로서 호출하여 생성한 인스턴스의 프로토타입은, 자신을 생성한 생성자함수의 prototype이다.
```js

// string 생성자 함수에 의한 String 객체 생성
const strObj = new String("Lee"); // String {"Lee"}

// string 생성자 함수를 통해 생성한 strObj객체의 프로토타입은
// String.prototype이다.
console.log(Object.getPrototypeOf(strObj) === String.prototype); // true
```
___
#### 표준 빌트인 객체는 프로토타입 메서드와 정적 메서드를 제공한다.
```js
const numObj = new Number(1.5); // Number {1.5}

//toFixed는 Number.prototpye의 프로토타입 메서드다.
// Number.prototype.toFixed는 소수점 자리를 반올림하여 문자열로 반환한다.
console.log(numObj.toFixed()); // 2

//isInteger는 Number의 정적메서드다.
// Number를 붙이지 않고 사용할 수 없다.
console.log(numObj.isInteger()) // Error : not a function
```
___
### 원시값과 래퍼 객체
##### 문자열이나 숫자, 불리언 등의 원시값이 있는데도 문자열,숫자,불리언 객체를 생성하는 String,Number,Boolean등의 표준 빌트인 생성자 함수가 존재하는 이유는 무엇일까?
##### 다음 예제를 살펴보면, 원시값은 객체가 아니므로 프로퍼티나 메서드를 가질 수 없는데도 원시값인 문자열이 마치 객체처럼 동작한다.

```js
const str = 'hello';

// 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작한다.
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
```
##### 이는 원시값인 문자열, 숫자, 불리언 값의 경우 이들 원시값에 대해 마치 객체처럼 <span style='color:gold'>마침표 표기법(또는 대괄호 표기법)으로 접근하면 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환</span>해 주기 때문이다. 즉 원시값을 객체처럼 사용하면 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다.
##### <span style='color:gold'> 이처럼, 문자열,숫자,불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체(Wrapper object)라 한다.</span>
##### 문자열에 대해 마침표 표기법으로 접근하면 그 순간 래퍼 객체인 String 생성자 함수의 인스턴스가 생성되고 문자열은 래퍼 객체의 [[StringData]]내부 슬롯에 할당되는데, 이때 문자열 래퍼 객체인 String 생성자 함수의 인스턴스는 String.prototype의 메서드를 상속받아 사용할 수 있다.
##### 사용 후 래퍼 객체의 처리가 종료되면 [[StringData]]내부 슬롯에 할당된 원래의 원시값을 갖도록 되돌리고, 래퍼 객체는 가비지 컬랙션의 대상이 된다.

```js

const str = 'hello'

// 식별자 str은 암묵적으로 생성된 래퍼 객체를 가리킨다.
// 식별자 str의 값 'hello'는 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된다.
// 레퍼 객체에 name 프로퍼티가 동적 추가된다.
str.name = 'Lee';

// 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의
// [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.
// 래퍼 객체를 참조하는 대상이 없으므로 가비지 컬랙션의 대상이 된다.

// 식별자 str은 새롭게 암묵적으로 생성된 래퍼 객체를 가리킨다.
// 새롭게 생성된 래퍼 객체에는 name프로퍼티가 존재하지 않는다.
console.log(str.name) // undefined

// 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에
// 할당된 원시값을 찾는다.
// 이때 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬랙션의 대상이 된다.

console.log( typeof str, str); // string hello
```

##### 식별자 str을 선언하고 .name을 붙이면 일시적으로 래퍼 객체로 변환된다. 이후 참조하는 값이 없으므로 가비지 컬랙션의 대상이 되어 사라진다. console.log로 str.name을 호출하지만. 첫번째 마침표 표기법으로 만들어진 래퍼 객체와는 다른 래퍼객체이기에 undefined를 갖는다. 
___

#### 숫자 값도 마찬가지 원리로 동작한다.
##### 마침표 표기법으로 접근하면 래퍼 객체인 Number 생성자 함수의 인스턴스가 생성되고, 숫자는 래퍼 객체의 [[NumberData]] 내부 슬롯에 할당된다. 그 후, 래퍼 객체의 처리가 종료되면 [[NumberData]] 내부 슬롯에 할당된 원시값을 되돌리고 래퍼 객채는 가비지 컬랙션의 대상이 된다.

```js

const num = 1.5;

console.log(num.toFixed()); // 2

console.log(type of num, num) // number 1.5
```

##### 이처럼, 문자열,숫자,불리언,심벌은 암묵적으로 생성되는 래퍼 객체에 의해 마치 객체처럼 사용할 수 있으며, 표준 빌트인 객체의 프로토타입 메서드,프로퍼티를 참조할 수 있다. 따라서 생성자 함수를 new 연산자와 함께 호출하여 인스턴스를 생성할 필요가 없으며 권장하지도 않는다.
##### null과 undefined는 래퍼 객체를 생성하지 않기에 객체처럼 사용하면 에러가 발생한다.

___
### 전역 객체 ( Global Object )

##### 전역 객체는 코드가 실행되기 이전에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에 속하지도 않는 최상위 객체이다.
##### 브라우저 환경에서는 window,self,this,frames가 전역객체를 가리키고, Node.js에서는 global이 전역 객체를 가리킨다.

>##### globalThis
###### globalThis는 ES11에서 도입되었고 전역 객체를 가리키던 다양한 식별자를 통일한 식별자다. globalThis는 표준 사양이므로 모든 환경에서 사용할 수 있다.

##### 전역 객체는 표준 빌트인 객체, 환경에 따른 호스트 객체, 그리고 var 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖는다.

##### 즉 전역 객체는 계층적 구조상 어떤 객체에도 속하지 않은 모든 빌트인 객체의 최상위 객체이다.<br/> 하지만 전역 객체가 최상위 객체라는 말이 프로토타입 상속 관계상에서 최상위 객체라는 의미는 아니다. 전역 객체 자신은 어떤 객체의 프로퍼티도 아니고, 단지 객체의 계층적 구조상 표준 빌트인 객체와 호스트 객체를 프로퍼티로 소유한다는 것을 말한다.

#### 전역 객체의 특징
- ##### 전역 객체는 개발자가 의도적으로 생성할 수 없다. 즉 전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않는다.
- ##### 전역 객체의 프로퍼티를 참조할 때 window를 생략할 수 있다.
```js
//문자열 'F'를 16진수로 해석하여 10진수로 반환한다.
window.parseInt('F',16) // 15
parseInt('F',16) // 15
```


- ##### 전역 객체는 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
- ##### 자바스크립트의 실행 환경에 따라 추가적인 메서드와 프로퍼티를 갖는다.
> ##### DOM BOM,XMLHttpREquest,Fetch,requestAnimationFrame, SVG, Web Storage, Web Component, Web Worker 같은 클라이언트 사이드 Web API를 호스트 객체로 제공하며 Node.js 환경에서는Node.js 고유의 API를 호스트 객체로 제공한다.

- ##### var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 그리고 전역 함수는 전역 객체의 프로퍼티가 된다.
- ##### let,const로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다. let이나 const 키워드로 선언한 전역 변수는 보이지 않는 개념적인 블록 (전역 랙시컬 환경의 선언적 환경 래코드)내에 존재하게 된다.
```js
var one = 1;
two = 2;
function three() { return one + two; }
let four = 4
window.one // 1
window.two // 2
window.three // 3
window.four // undefined
```

### 빌트인 전역 프로퍼티 
##### 빌트인 전역 프로퍼티는 전역 객체의 프로퍼티를 말한다. 주로 애플리케이션 전역에서 사용하는 값을 제공한다.

#### Infinity
```js
console.log(3/0) // Infinity

console.log(typeof Infinity) // number
```

#### NaN
```js
console.log(Number('xyt')) // NaN
console.log(1 * 'tem thousend') // NaN
console.log(typeof NaN) // number
```
#### undefined
```js
var one;

console.log(one) // undefined
console.log(typeof undefined) // undefined
```
___
### 빌트인 전역 함수

##### 빌트인 전역 함수는 애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드다.

#### eval
##### eval함수는 자바스크립트 코드를 나타내는 문자열을 인수로 전달받는다. 전달받은 문자열 코드가 표현식이라면 eval 함수는 문자열 코드를 런타임에 평가하여 값을 생성하고, 전달받은 인수가 표현식이 아닌 문이라면 eval함수는 문자열 코드를 런타임에 실행한다. 문자열 코드가 여러 개의 문으로 이루어져 있다면 모든 문을 실행한다.

##### <span style='color:gold'> eval을 통해 입력받은 콘텐츠를 실행하는 것은 보안에 매우 취약하고 자바스크립트에 의해 최적화가 실행되지 않으므로 처리 속도가 느리다. 따라서 사용을 금지해야 한다.</span>
___

#### isFinite
##### 유한수면 true 무한수면 false를 반환한다.
```js

//인수가 유한수면 true
isFinite(0) // true
isFinite(2e64); // true
isFinite('10') // true
isFinite(null) // true . null은 암묵적 타입 변환으로 인해 0으로 평가된다.

//인수가 NaN이어도 false를 반환한다
isFinite(NaN); // false
inFinite('hello'); // false
isFinite('2005/12/12') // false
```

#### isNaN

```js
isNaN('') // false '' => 0
isNaN(' ') // false ' ' => 0
isNaN(undefined) // true
isNaN({}); // true
isNaN(new Date().toString()) // true
```

#### parseFloat
##### 전달 받은 문자열 인수를 부동 소수점 숫자, 실수로 해석하여 반환한다.
```js
parseFloat('3.14') // 3.14

//공백으로 구분된 문자열은 첫 번째 문자열만 반환한다.
parseFloat('34 45 66') // 34
parseFloat('40 years') // 40

//첫번째 문자열을 숫자로 반환할 수 없다면 NaN을 반환한다.
parseFloat('He got 40 doller') // NaN

//앞 뒤의 공백은 무시된다.
parseFloat('   60          ') // 60
```

#### parseInt
##### 전달 받은 문자열 인수를 정수로 해석하여 반환한다.
```js
parseInt('10.3243241385582234') // 10

//숫자도 위와 같게 10이 출력되지만 전달 받은 인수가 문자열이 아니기에 문자열로 변환한 다음, 정수로 해석하여 반환한다.
parseInt(10.3243241385582234) // 10
```
##### 진법을 나타내는 기수(2~36)을 전달하여 변한활 수 있다.
```js
parseInt('10',8) // 8
parseInt('10',2) // 2
parseInt('10',16) // 16
```
##### 숫자를 문자열로 변환하여 반환하고 싶을때에는 Number.prototype.toString 메서드를 사용한다.

##### 두 번째 인수로 기수를 지정하지 않더라도 첫 번째 인수로 전달된 문자열이 '0x' 또는 '0X'로 시작하는 16진수 리터럴이라면 16진수로 해석하여 10진수 정수로 반환한다.
```js
parseInt('0xf') // 15
parseInt('f',16); // 15
```
##### 2진수 8진수는 기수 등록 없이는 불가능하다.

##### 첫 번째 인수로 전달한 문자열의 두 번째 문자부터 해당 진수를 나타내는 숫자가 아닌 문자(예를 들어 2진수의 경우 2)와 마주치면 이 문자와 계속되는 문자들은 전부 무시되며 해석된 정수값만 반환한다.
```js
parseInt('1A0') // 1
parseInt('102',2) // 2
parseInt('58',8) // 5
parseInt('FG',16) // 15
```

#### encodeURI / decodeURI
##### encodeURI 함수는 완전한 URI를 문자열로 전달받아 이스케이핑한다. encoding이란 URI의 문자들을 이스케이핑 하는 것을 의미하는데, 이스케이핑은 네트워크를 통해 정보를 공유할 때 모든 시스템이 읽을 수 있는 아스키 문자 셋으로 변환하는 것이다. 예를 들어 한글 '가'는 %EC%9E%90으로 인코딩된다.
##### URL은 아스키 문자 셋으로만 구성되어야 하며, URL에 올수 없는 글자나 URL내에서 의미를 갖고 있는 문자, 또는 시스템에 의해 해석될 수 있는 문자를 이스케이프 처리하여 야기될 수 있는 문제를 예방해야 한다. 알파벳,0~9숫자, -_.!~*'()은 이스케이핑에서 제외된다.
```js
const uri = 'https://www.naver.com?name=김명성&job=개발자';
const escape = encodeURI(uri);
console.log(escape);
'https://www.naver.com?name=%EA%B9%80%EB%AA%85%EC%84%B1&job=%EA%B0%9C%EB%B0%9C%EC%9E%90'
```
#### decode는 이전으로 돌아간다.
___


### 암묵적 전역
```js
var x = 10;

function foo() {
  y= 20;
}
foo()

console.log(x + y) // 30
```

##### foo 함수 내의 y는 선언하지 않은 식별자이기에 전역 변수처럼 동작한다. 이는 선언하지 않은 식별자에 대해 값을 할당하면 전역 개체의 프로퍼티가 되기 때문이다.

##### foo함수가 호출되면 자바스크립트 엔진은 y 변수에 값을 할당하기 위해 먼저 스코프 체인을 통해 선언된 변수인지 확인한다.이때 전역 스코프 어디에서도 y 변수의 선언을 찾을 수 없으므로 참조 에러가 발생하지만 자바스크립트 엔진은 y = 20을 window.y = 20으로 해석하여 전역 객체에 프로퍼티를 동적 생성한다. 결국 y는 전역 객체의 프로퍼티가 되어 마치 전역 변수처럼 동작한다.
##### 하지만 y는 변수가 아니며 전역객체 window의 프로퍼티로 추가되었을 뿐이다. y는 변수가 아니므로 변수 호이스팅이 발생하지 않는다.
##### 또한 변수가 아니라 프로퍼티이기에 delete 연산자로 삭제할 수 있다.
```js
console.log(x) // undefined
console.log(y) // Error . y is not defined

var x = 10;

function foo () {
  y = 20
}
foo();

cosole.log( x + y) // 30

delete x;
delete y;

console.log(window.x) // 10
console.log(window.y) // undefined
```

