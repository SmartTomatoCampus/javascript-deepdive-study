### Number
##### 표준 빌트인 객체인 Number는 원시 타입인 숫자를 다룰 때 유용한 프로퍼티와 메서드를 제공한다.
___

### Number 생성자 함수
##### Number 생성자 함수에 인수를 전달하지 않고 new 연산자를 함께 호출하면 [[NumberData]]내부 슬롯에 0을 할당한 Number 래퍼 객체를 생성한다.
##### new 연산자를 사용하지 않고 Number 생성자 함수를 호출하면 Number 인스턴스가 아닌 숫자를 반환한다. 이를 이용하여 명시적으로 타입을 변환하기도 한다.
```js
Number('0'); // 0
```
### Number의 프로퍼티
#### Number.prototype.toFixed
##### toFixed 메서드는 숫자를 반올림하여 문자열로 반환한다. 인수를 생략하면 기본값 0이 지정된다.
```js
(12345.69342).toFixed(); // 12346
(1.54).tofixed(1); // 1.5
```
#### Number.prototype.toPrecision
##### 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환한다
```js
(12345.6789).toPrecision(1) // "1e+4"
(12345.6789).toPrecision(6) // "12345.7"
```
#### Number.prototype.toString
##### 숫자를 문자열로 변환하여 반환한다.
```js
(10).toString() // "10"
(16).toString(2) // "10000"
(16).toString(8) // "20"
(16).toString(16) // "10"
```
