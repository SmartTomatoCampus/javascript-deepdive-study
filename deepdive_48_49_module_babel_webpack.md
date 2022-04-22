# 꽁치

1. 모듈과 기본적인 babel 과 webpack 설정 방법을 알았다.
2. babel 은 트랜스파일링을, webpack 은 번들링에 대한 세팅을 해준다는 것을 알았다.

# 명성

모듈

export로 내보내서 다른 모듈이 재사용할수 있게 만들어준다

선언문 앞에 매번 export 키워드를 붙이는 것이 번거롭다면 export 할 대상을 하나의 객체로 구성하여 한번에 export 할 수도 있다.
export { some, some2, some3 };

받아오는 모듈은 import로 받아온다.

에스터리스크로 한번에 보내고 as로 별칭을 만들어 객체의 프로퍼티로 할당하여 사용할 수 있다.
import * as lib from './lib.mjs';
lib.some

모듈이 export한 식별자 이름을 변경하여 import할 수도 있다.
import { some as SOME, myFunc as getSum } from './lib.mjs';
getSum(4,20)

export default 앞에는 const / let / var 키워드를 사용할 수 없다

default 키워드와 함께 export한 모듈은 중괄호 없이 임의의 이름으로 import한다.
 import Modal from './lib.mjs'

# 애한

모듈 
- CommonJs방식, AMD방식, ES6 모듈 방식 3가지가 있다.
- Node.js환경에서는 CommonJS방식을 주로쓰는편.
- export/import 키워드
- default 키워드를 사용시 var, let, const 키워드 사용 불가
- default 키워드와 함께 export한 모듈은 {} 없이 임의의 이름으로 import한다.

바벨(Babel)
- 최신의 문법을 구 브라우저에서도 사용할 수 있게 변환해줌.(크로스 브라우징 이슈 해결)
- 코드 자체를 변경시켜준다.

웹펙(Webpack)
- 모듈을 번들링함(여러개를 하나로 묶어준다)

# 뽀또

# 에러 처리

- 에러가 발생하지 않는 코드를 작성하는 것은 불가능 하고, 에러 발생시 프로그램은 강제 종료 됨
- try...catch 문을 통해서 프로그램 강제 종료를 막을 수 있음
- 예외 처리 : 직접적으로 에러를 발생하지는 않는 상황
    - 예외적인 상황에 의해서 다른 코드가 에러를 발생할 수 있음
    - 예) querySelector의 경우 없으면 null을 반환함 -> null을 할당 받은 변수에 프로퍼티를 참조하여 사용하는 경우 error 발생
        - querySelector의 반환 값을 단축 평가 또는 옵셔널 체이닝 연산자 사용해 예외 처리 하거나, 에러 처리 코드를 미리 등록해 두고 에러 발생시 에러 처리 코드로 점프 되도록 하는 방법

## try...catch...finally 문

- 에러 처리(error handling) : try...catch...finally 문을 사용하여 에러를 처리하는 것
    - try : 에러가 발생할 가능성이 있는 코드 블럭
    - catch : try에서 에러 발생시 실행되는 코드 블럭 (try에서 발생한 Error 객체가 전달됨)
    - finally : 에러 발생과 상관 없이 반드시 한번 실행되는 코드 블럭

## Error 객체

- new Error(errorMessage)
    - errorMessage: 에러를 상세히 설명하는 에러 메세지(string)를 인수로 받음
- message 프로퍼티 : Error 생성자 함수에 전달한 인수를 값으로 갖는 프로퍼티
- stack 프로퍼티 : 에러를 발생시킨 콜스택의 호출 정보를 나타내는 문자열 (디버깅 목적)
- 에러 객체 종류 7가지
    - Error : 일반적인 에러 객체
    - SyntaxError : JS 문법에 맞지 않는 문 해석시 발생하는 에러 객체
    - ReferenceError : 참조 함수 없는 식별자를 참조시 발생하는 에러 객체
    - TypeError : 피연산자 또는 인수의 데이터 타입이 유효하지 않을 때
    - RangeError : 숫자값의 허용 범위를 벗어났을 때 발생
    - URIError : encodeURI 또는 decodeURI 함수에 부적절한 인수 전달시 발생
    - EvalError : eval 함수에서 발생하는 에러 객체

## thorw 문

- 해당 에러 객체로 에러를 발생 시킴
- try 문에 error 발생시, catch 문에 error 변수가 생성되고 해당 error 객체가 할당됨

## 에러 전파

- throw된 에러를 캐치 하지 않으면, 에러는 호출자 방향으로 전파 됨 (콜 스택의 아래 방향으로 전파됨)
- 비동기 함수, 프로미스 후속 처리 메서드의 콜백 함수는 호출자가 없기 때문에 에러 전파가 되지 않음

# 모듈

- script 에 type="module" 쓰고 .mjs 확장자를 쓴다

# 초생

1. 모듈이란 애플리케이션을 구성하는 개별적 요소이다. script 어트리뷰트에 type=’module’하면 됨
2. 모듈은 독자적인 스코프를 갖는다.  그냥 자바스크립트를 script 태그로 구분해서 불러도 똑같은 스코프를 공유함
3. 모듈은 캡슐화 되어 있어서 외부에 노출이 안되는데 외부에 노출하고 싶은 식별자는 앞에 export를 붙여준다. 그러면 다른 모듈에서 import로 받아 재사용을 할 수 있다.