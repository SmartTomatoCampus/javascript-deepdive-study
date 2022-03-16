### strict mode

```js
function foo() {
  x = 10;
}
foo()

console.log(x)
```

##### 위와 같이 선언하였을 때 자바스크립트 엔진은 x변수가 어디서 선언되었는지 스코프체인을 검색하기 시작한다.
##### 먼저 foo 함수 블록 스코프에서 x변수의 선언을 검색한다. foo 함수의 스코프에는 x 변수의 선언이 없으므로 검색에 실패할 것이고, 자바스크립트 엔진은 x변수를 검색하기 위해 foo 함수 컨텍스트의 상위 스코프에서 x 변수 선언을 검색한다. 
##### 이때 변수 선언 없이도 자바스크립트에서는 암묵적 전역(implicit global)이 발생되어 전역변수처럼 사용할 수 있게 된다.

##### <span style="color:gold"> 개발자의 의도와는 상관없이 발생한 암묵적 전역은 오류를 발생시키는 원인이 된다. 오타나 실수는 언제나 발생하기에 이러한 잠재적인 오류를 발생시키지 않는 환경에서 개발하는 것이 근본적인 해결책이라고 할 수 있다.</span><br/> 이를 지원하기 위해, ES5부터 strict mode가 추가되었다. strict mode는 자바스크립트 언어의 문법을 조금 더 엄격하게 적용하여 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.
##### ESLint같은 툴도 strict mode와 유사한 효과를 얻을 수 있다. ESLint는 정적 분석 기능을 통해 소스코드를 실행하기 전에 소스코드를 스캔하여 문법적 오류 뿐만 아니라 자매오류까지 찾아내주고 리포팅하는 유용한 도구다.
##### ESLint는 오류는 물론 코딩 컨벤션을 설정 파일 형태로 정의하고 강제할 수 있기 때문에 더욱 강력한 효과를 얻을 수 있다.
___
### 전역에 strict mode를 피해야 하는 이유

##### 스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정되어 적용된다.
##### 하지만 strict mode 스크립트와 non-strict mode 스크립트를 혼용하는 것은 오류를발생 시킬 수 있다. 특히 외부 서드파티 라이브러리를 사용하는 경우 라이브러리가 non-strict mode인 경우도 있기 때문에 전역에 strict mode를 적용하는 것은 바람직 하지 않다. 이러한 경우 즉시실행함수로 스크립트 전체를 감싸 스코프를 구분하고 즉시 실행 함수의 선두에 strict mode를 적용한다.

```js
(function(){
  'use strict';
  // something statement
}())
```


#### 함수 단위로 strict 모드도 피해야 한다.
##### 번거로울 뿐 아니라, strict mode가 적용된 함수가 참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않는다면 이 또한 문제가 된다.
```js

(function(){
  var let = 10;
  function foo() {
    'use strict';
    let = 20 
    }
  foo() // strict mode reserved word
}())
```

따라서 strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

#### strict mode를 사용하면 알 수 있는 에러

##### 암묵적 전역 - 선언하지 않은 변수를 참조하면 ReferenceError가 발생한다.

##### 변수,함수,매개변수의 삭제 - delete 연산자로 변수,함수,매개변수를 삭제하면 SyntaxError가 발생한다.

##### 매개변수 이름의 중복
```js
function(){
  'use strict'
  
  //Duplicate parameter name not allowed in this context
  function foo(x,x){
    return x + x;
  }
  console.log(foo(1,2));
}());
```

#### with문의 사용
##### with문을 사용하면 SyntaxError이 발생한다. with문은 전달된 객체를 스코프 체인에 추가한다. with문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어 코드가 간단해지지만 성능과 가독성이 나빠지기에 사용을 지양해야 한다.
___
### strict mode 적용에 의한 변화 

#### 일반 함수의 this
##### strict mode에서 함수를 일반 함수로서 호출하면 this에 undefined가 바인딩된다. 생성자 함수가 아닌 일반 함수 내부에서 this를 사용할 필요가 없기 때문이다.
```js
(function (){
  'use strict'
  
  fonction foo() {
    console.log(this); //undefined
  }
  foo();
  
  function Foo() {
    console.log(this); // Foo
  }
  new Foo();
}());
```
#### arguments 객체
##### strick mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.
```js
(function (a) {
'use strict';
 // 매개변수에 전달된 인수를 재할당하여 변경
a = 2;

  //변경된 인수가 arguments 객체에 반영되지 않는다.
  console.log(arguments);// { 0: 1, length: 1 }
}(1));
```
