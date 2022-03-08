# 15. let, const 키워드와 르록 레벨 스코프

생성일: 2022년 3월 6일 오후 8:20

## 15. 1 var 키워드로 선언한 변수의 문제점

### 15. 1. 1 변수 중복선언 허용

- var 키워드는 중복선언이 가능하다 → 먼저 선언된 변수 값이 변경 되는 문제점이 발생함

```jsx
var x = 1;
var y = 1;

var x = 100;
var y;

console.log(x); // 100
console.log(y); // 1
```

### 15. 1. 2 함수 레벨 스코프

- var 키워드로 선언한 변수는 함수의 코드 블록만 지역변수로 인정함
- 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에 선언해도 모두 전역변수가 된다.

```jsx
var x = 1;

if(true){
	
	var x = 10;
}

console.log(x); // 10

var i = 10;
for(var i = 10; i < 5; i++){
	console.log(i); // 0 1 2 3 4
}

console.log(i); //5 -> 의도치 않게 값이 변경 됨
```

### 15. 1. 3 변수 호이스팅

- var 키워드 사용 시 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어올려진 것 처럼 작동한다. 이는 프로그램의 가독성을 떨어뜨리고 오류를 발생 시킬 수 있다.

```jsx
console.log(foo); // undefined

foo = 123;

console.log(foo); // 123

var foo;
```

## 15. 2 let 키워드

### 15. 2. 1 변수 중복 선언 금지

- var 키워드로는 변수 중복 선언이 가능하지만 let 키워드는 중복 선언을 허용하지 않는다.

```jsx
var foo = 123;
var foo = 456;

let bar = 123;
let bar = 456; // syntaxError
```

### 15. 2. 2 블록 레벨 스코프

- let 키워드는 모든 코드 블록을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다

```jsx
let foo = 1;
{
	let foo = 2;
	let bar = 3;
}

console.log(foo); // 1
console.log(bar); // ReferenceError
```

### 15. 2. 3 변수 호이스팅

- let 키워드로 선언한 변수는 “선언단계”와 “초기화단계”가 분리되어 진행된다.
- let 키워드 또한 호이스팅이 발생하지만 ES6에서는 호이스팅이 발생하지 않는 것 처럼 동작한다.

```jsx
console.log(foo);  // RefereneceError: foo is not defined

let foo; // 변수 선언문에서 초기화 단계가 실행된다.
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); 1

```

### 15. 2. 4 전역 객체와 let

- var 키워드로 선언한 전역변수와 전역함수, 암묵적 전역은 전역개체 window의 프로퍼티가 된다.
- 전역 객체의 프로퍼티를 참조할 때 window를 생략 할 수 있다.

```jsx
// 전역 변수
var x = 1;
y = 2;

function foo(){}

// var 키워드로 선언한 전역변수는 전역객체 window의 프로퍼티다. 
console.log(window.x);
console.log(window.foo); // f foo() {} 함수 전역 객체 window의 프로퍼티
```

## 15. 3 const 키워드

### 15. 3. 1 선언과 초기화

- const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화 해야함
- const 키워드로 선언한 변수는 let 키워드로 선언한 변수와 마찬가지로 블록 스코프를 갖고 변수 호이스팅이 발생하지 않는 것 처럼 동작한다.

```jsx
{
	console.log(foo); // ReferenceError
	const foo = 1;
}
console.log(foo) // ReferenceError
```

### 15. 3. 2 재할당 금지

- const로 선언한 변수는 재할당이 금지된다.

```jsx
const foo = 1;
foo = 2; // TypeError
```

### 15. 3. 3 상수

- 상수는 재할당이 금지된 변수를 말한다.
- 그러므로 const 키워드에서는  할당 된 값을 변경 할 수 있는 방법은 없다.

### 15. 3. 4 cost 키워드와 객체

- const 키워드로 선언된 변수에 객체를 할당한 경우에는 값을 변경 할 수 있다.
- const 키워드는 재할당이 금지된 것이지 불변을 의미하지는 않는다.

```jsx
const person = {
	name: 'Lee'
};

person.name = 'Kim';

console.log(person);
```

## 15. 4 var vs. let vs. const

- ES6를 사용하면 var 키워드는 사용하지 않는다.
- 재할당이 필요한 경우에만 let 키워드를 사용한다.
- 읽기 전용으로 사용하는 원시값에는 const 키워드를 사용한다.