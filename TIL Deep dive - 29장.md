### Math
##### 표준 빌트인 객체인 Math는 수학적인 상수와 함수를 위한 프로퍼티,메서드를 제공한다. Math는 정적 프로퍼티와 정적 메서드만을 제공한다.
___
### Math 프로퍼티

#### Math.PI
##### 원주율을 반환한다.
___
### Math 메서드
___
#### Math.abs
##### 인수로 전달된 숫자의 절대값을 반환한다. 절대값은 반드시 0 또는 양수이다.
```js
Math.abs(-1); // 1
Math.abs(null); // 0
Math.abs([]); // 0
Math.abs(undefined) // NaN
```
___
#### Math.ceil / round / floor
##### 인수로 전달된 숫자의 소수점 이하를 올림/반올림/내림한 정수를 반환한다.
```js
Math.ceil(1.4) // 2
Math.round(1.4) // 1
Math.floor(1.4) // 1
```
___
#### Math.sqrt
##### sqrt 메서드는 인수로 전달된 숫자의 제고븍ㄴ을 반환한다.
```js
Math.sqrt(9) // 3
Math.squr(-9) // NaN
Math.sqrt(2) // 1.414213562373095
```
___
#### Math.random
##### 임의의 난수를 반환한다. 반환하는 난수는 0에서 1미만의 실수다. 즉 0은 포함되지만 1은 포함되지 않는다.
```js
const random = Math.floor((Math.random() * 10) +1);
console.log(random) // 1에서 10범위의 정수
```
___
#### Math.pow
##### Math.pow 첫번째 인수를 base로 두번째 인수를 지수로 거듭제곱한 결과를 반환한다.
```js
Math.pow(2,8) // 256
Math.pow(2,-1) // 0.5
Math.pow(2) // NaN
```
___
#### Math.max
##### Math.max 메서드는 전달받은 인수 중에서 가장 큰 수를 반환한다. 인수가 전달되지 않으면 -Infinity를 반환한다.
```js
Math.max(1) // 1
Math.max(1,2) // 2
Math.max(1,2,3) // 3
```
##### 배열을 인수로 전달받아 배열 요소 중 최대값을 구하려면 Function.prototype.apply 메서드 또는 스프레드 문법을 사용해야 한다.
```js
Math.max.apply(null,[1,2,3]) // 3
Math.max(...[1,2,3]) // 3
```
##### 정 반대의 기능을하는 Math.min도 있다.
___


