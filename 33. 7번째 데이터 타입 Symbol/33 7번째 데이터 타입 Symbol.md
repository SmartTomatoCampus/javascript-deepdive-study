# 33 7번째 데이터 타입 Symbol

생성일: 2022년 3월 31일 오후 8:36

## 33.1 심벌이란?

- 변경 불가능한 원시 타입의 값이다.
- 다른 값과 중복되지 않는 유일무이한 값이다.

## 33. 2 심벌 값의 생성

### 33. 2. 1 Symbol 함수

- Symbol 함수를 호출하여 생성한다.
- Symbol 함수는 String, Number, Boolean 생성자 함수와 달리 new 연산자와 함께 호출하지 않는다.
- Symbol은 암묵적으로 래퍼 객체를 생성한다.
- 심벌값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.

```jsx
const mySymbol = Symbol();
console.log(typeof mySymbol);
```

### 33. 2. 2 Symbol.for / Symbol.keyFor 메서드

- 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 잇는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.
- 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환한다.
- Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출 할 수 있다.

```jsx
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환
const s2 = Symbol.for('mySymbol');

console.log(s1 === s2);  // true

Symbol.keyFor(s1);  // mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s3 = Symbol('foo');
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s2); // undefined
```

## 33. 3 심벌과 상수

```jsx
// 중복 될 가능성이 없는 심벌 값으로 상수 값을 생성한다.
const Direction = {
	UP: Symbol('up');
	DOWN: Symbol('down');
	LEFT: Symbol('left');
	RIGHT: Symbol('right');
};

const myDirection = Direction.UP;

if(myDirection === Direction.UP){
	console.log('You are going UP');
}
```

## 33. 4 심벌과 프로퍼티 키

- 심벌 값을 프로퍼티 키로 사용하려면 프로퍼티 키로 사용할 심벌 값에 대괄호를 사용해야 한다.

```jsx
const obj = {
	[Sybol.for('mySymbol')] : 1
};

obj[Symbol.for('mySymbol')]; // 1
```

## 33. 5 심벌과 프로퍼티 은닉

- 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 for ... in 문이나 Object.keys, Object.goetOwnPropertyNames 메서드로 사용 할 수 없다

→ 외부에 노출할 필요가 없는 프로퍼티를 은닉 할 수 있다.

## 33. 6 심벌과 표준 빌트인 객체 확장

- 예시로 Array.prototype.find 메서드는 ES6 도입 이후에 표준 빌트인 메서드로 도입되었는데 이전에 개발자가 사용자 정의 메서드로 같은 이름을 가진 메서드를 추가 했을 경우 사용자 정의 메서드가 덮어씌여지는 문제가 발생한다. 하지만 심벌 값으로 프로퍼티 키를 생성해서 정의하면 프로퍼티 키와 충돌할 위험이 없어 안전하다.

## 33. 7 Well-know Symbol

- 심벌은 중복되지 않는 상수 값을 생성하고 기존 코드에 영향으로 주지 않으며 새로운 프로퍼티를 추가하기 위해 도입되었다.