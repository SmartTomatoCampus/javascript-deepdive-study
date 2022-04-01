# [JavaScript] Number 타입 & 메서드

```jsx
Number('0')     // 0
Number('-1')    // -1
Number('10.63') // 10.63

Number(true)    // 1
Number(false)   // 0
```

- **Number.isFinitie() : 숫자가 유한수인지 확인**
    1. ES6 에서 도입
    2. 인수로 전달된 숫자값이 정상적인 유한수인지 검사하여 불리언 값으로 반환
    
    ```jsx
    Number.isFinite(0)                // true
    Number.isFinite(Number.MAX_VALUE) // true
    Number.isFinite(Number.MIN_VALUE) // true
    
    Number.isFinite(Infinity)  // false
    Number.isFinite(-Infinity) // false
    
    Number.isFinite(NaN)       // false
    
    // Number.isFinite() 는 인수를 숫자로 암묵적 타입 변환하지 않는다.
    Number.isFinite(null)     // false
    // isFinite() 빌트인 전역함수는 인수를 숫자로 암묵적 타입 변환한다. null 은 0으로 변환됨
    isFinite(null)            // true
    ```
    
- **Number.isInteger() : 숫자가 정수인지 확인**
    1. ES6 에서 도입
    2. 인수로 전달된 숫자값이 정수(Integer)인지 검사하여 그 결과를 불리언 값으로 반환함
    3. 검사하기 전에 인수를 숫자로 암묵적 타입 변환하지 않음
    
    ```jsx
    Number.isInteger(0)      // true
    Number.isInteger(123)    // true
    Number.isInteger(-123)   // true
    
    Number.isInteger(0.5)       // false
    Number.isInteger('123')     // false
    Number.isInteger(false)     // false
    Number.isInteger(Infinity)  // false
    Number.isInteger(-Infinity) // false
    ```
    
- **Number.isNaN() : 인수로 전달된 숫자값이 NaN 인지 검사**
    
    ```jsx
    // 인수가 NaN 이면 true 를 반환한다.
    Number.isNaN(NaN) // true
    
    // Number.isNaN 은 인수를 숫자로 암묵적 타입 변환하지 않는다.
    Number.isNaN(undefined) // false
    
    // isNaN 빌트인 전역 함수는 인수를 숫자로 암묵적 타입 변환한다. undefined 는 NaN 으로 암묵적 타입 변환된다.
    isNaN(undefined) // true
    ```
    
- **Number.isSafeInteger() : 인수로 전달된 숫자값이 안전한 정수인지 검사**
    
    안전한 정수는 -(2^53 -1) 과 2^53-1 사이의 정수값이다.
    
    ```jsx
    // 0은 안전한 정수다
    Number.isSafeInteger(0) // true
    // 0.5 는 정수가 아니다
    Number.isSafeInteger(0.5) // false
    // '123' 을 숫자로 암묵적 타입 변환하지 않는다.
    Number.isSafeInteger('123') // false
    // false 를 숫자로 암묵적 타입 변환하지 않는다.
    Number.isSafeInteger(false) // false
    // Infinity/-Infinity 는 정수가 아니다
    Number.isSafeInteger(Infinity) // false
    ```
    
- **Number.isExponential() : 숫자를 지수 표기법으로 변환하여 문자열로 반환**
    
    지수 표기법이란 매우 크거나 작은 숫자를 표기할 때 주료 사용하며 e(Exponent) 앞에 있는 숫자에 10의 n승을 곱하는 형식으로 수를 나타내는 방식이다.
    인수점 소수점 이하로 표현할 자릿수를 전달할 수 있다.
    
    ```jsx
    (77.1234).isExponential() // "7.71234e+1"
    (77.1234).isExponential(4) // "7.7123e+1"
    (77.1234).isExponential(2) // "7.712e+1"
    
    // 참고로 다음과 같이 숫자 리터럴과 함께 Number 프로토타입 메서드를 사용할 경우 에러가 발생한다.
    77.toExponential() // SyntaxError
    
    // 참고로 숫자 뒤에 소수점이 있으면 리터럴로 적어서 해도 에러가 발생하지 않는다.
    // 뒤에 있는 소수점을 프로퍼티 연산자로 인식하기 때문
    77.1234.toExponential() // "7.71234e+1"
    
    // 정수의 경우 괄호를 씌우면 메서드를 붙여 쓸 수 있다.
    (77).toExponential() // "7.7e+1"
    
    // 또는 한칸 공백을 주면 뒤에 . 을 프로퍼티 연산자로 인식한다
    77 .toExponential() // "7.7e+1"
    ```
    
- **Number.toFixed() : 인자로 전달된 자리까지 반올림**
    1. **숫자를 반올림하여 문자열로 반환**
    2. 인자로 전달된 값만큼의 소수점까지 표시되도록 반올림하고
    3. 아무값도 전달하지 않으면 소수점없이 정수로 표현되도록 반올림함
    
    ```jsx
    (12345.6789).toFixed()    // "12346"
    (12345.6789).toFixed(1)   // "12345.7"
    (12345.6789).toFixed(2)   // "12345.68"
    (12345.6789).toFixed(3)   // "12345.679"
    
    문자열로 반환됨을 주의
    ```
    
- **Number.toString() : 숫자를 문자열로 반환**
    
    ```jsx
    // 인수를 생략하면 10진수 문자열을 반환한다.
    (10).toString()   // "10"
    // 2진수 문자열을 반환한다.
    (16).toString(2)  // "10000"
    // 8진수 문자열을 반환한다.
    (16).toString(8)  // "20"
    // 16진수 문자열을 반환한다.
    (16).toString(16) // "10"
    ```
    
    인자로 n이 전달되면 n진법으로 전환하여 문자열로 반환