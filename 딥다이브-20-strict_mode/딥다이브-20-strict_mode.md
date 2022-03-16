# [JavaScript] strict mode

# strict mode란?

---

```jsx
function foo() {
	x = 10
}
foo()

console.log(x) // ?
```

위 코드에서 선언하지 않은 변수 x 에 대해 함수 스코프에서 선언을 찾고 그렇지 않으면 상위 스코프(여기선 전역스코프)에서 변수 선언을 찾는다.
변수 x 에 대한 선언이 존재하지 않지만, 자바스크립트 엔진은 에러를 내보내지 않고 암묵적으로 전역 객체에 x 프로퍼티를 동적으로 생성한다.

⇒ 이런 현상을 **암묵적 전역** 이라고 한다.

에러는 의도치 않은 실수로 발생할 수 있기 때문에 ES5 부터 **strict mode(엄격 모드)** 가 도입되어 잠재적 에러를 명시해준다.

또는 ESLint 같은 도구의 도움을 받을 수 있다.

# strict mode 의 적용

---

**strict mode 를 적용하려면 전역의 선두 또는 함수 몸체의 선두에 ‘use strict’; 를 추가한다!**

```jsx
'use strict';

function foo() {
	x = 10; // ReferenceError: x is not defined
}
foo();
```

전역 스코프에 strict mode 를 적용한다

```jsx
function foo() {
	'use strict';
	x = 10 // ReferenceError: x is not defined
}
foo()
```

함수 선두에 추가해서 함수 스코프 안에서 strict mode 를 적용한다

```jsx
function foo() {
	x = 10 // 에러를 발생시키지 않는다.
	'use strict';
}
foo()
```

`'use strict'` 를 적기 전의 코드는 적용되지 않는다.

# 전역에 strict mode 를 적용하는 것은 피하자

---

전역에 적용한 strict mode 는 스크립트 단위로 적용된다.

```html
<!DOCTYPE html>
<html>
<body>
	<script>
		'use strict'
	</script>
	<script>
		x = 1 // 에러가 발생하지 않는다.
	</script>
```

위처럼 strict mode 모드는 스크립트 단위로 적용된다.

또한 strict mode 모드와 non-strict mode 를 혼용하는 것은 좋지 않을 뿐더러 외부 라이브러리 등이 non-strict mode 일 경우도 있다.

이럴 때는 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수의 선두에 strict mode 를 적용한다

```jsx
(function() {
	'use strict'
	...
}())
```

# 함수 단위로 strict mode 를 적용하는 것도 피하자

---

어떤 함수는 strict mode 를 적용하고 어떤 함수는 적용하지 않는 것도 바람직 하지 않으며 모든 함수에 일일히 적용하는 것도 번거롭다

strict mode 는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다

# strict mode 가 발생시키는 에러

---

## 1. 암묵적 전역

선언하지 않은 변수를 참조하면 ReferenceError 가 발생한다

```jsx
(function() {}(
	'use strict'
	
	x = 1
	console.log(x) // ReferenceError: x is not defined
))
```

## 2. 변수, 함수, 매개변수의 삭제

`delete` 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError 가 발생한다

```jsx
(function() {}(
	'use strict'

	var x = 1
	delete x // SyntaxError: Delete of an unqualified identifier in strict mode

	functino foo(a) {
		delete a // SyntaxError
	}
	
	delete foo // SyntaxError
))
```

## 3. 매개변수 이름의 중복

중복된 매개변수의 이름을 사용하면 SyntaxError 가 발생한다

```jsx
(function () {
	'use strict'

	// SyntaxError: Duplicate parameter name not allowed in this context
	function foo(x, x) {
		return x + x
	}
	console.log(foo(1, 2))
}())
```

## 4. with 문의 사용

`with 문` 을 사용하면 SyntaxError 가 발생한다.

`with 문` 은 전달된 객체를 스코프에 추가한다. with 문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서 코드가 간단해지지만 
성능과 가독성이 나빠지므로 사용하지 않는 것이 좋다.

```jsx
(function() {
	'use strict'

	// SyntaxError: Strict mode code may not include a with statement
	with({ x: 1 }) {
		console.log(x)
	}
}())
```

# strict mode 적용에 의한 변화

---

## 1. 일반 함수의 this

strict mode 에서 함수를 일반 함수로서 호출하면 this 에 undefined 가 바인딩된다.

생성자 함수가 아닌 일반 함수 내부에서는 this 를 사용할 필요가 없기 때문이다.
( 이때 에러는 발생하지 않는다.)

```jsx
(function () {
	'use strict'

	function foo() {
		console.log(this) // undefined
	}
	foo()

	function Foo() {
		console.log(this)
	}
	new Foo() // Foo
}())
```

( strict mode 가 아닐 때 일반 함수에서 this 는 뭐가 나오는지 위 코드를 그대로 콘솔에 찍어보자 window 전역 객체가 나왔다 )

## 2. arguments 객체

strict mode 에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

```jsx
(function (a) {
	'use strict'
	// 매개변수에 전달된 인수를 재할당하여 변경
	a = 2
	
	// 변경된 인수가 arguments 객체에 반영되지 않는다.
	console.log(arguments) // { 0: 1, length: 1 }
}())
```

> **새롭게 알게된 것 TIL (2022.03.13 일)**
> 
> 
> ---
> 
> 1. strict mode 에서는 일반 함수에서 this 를 못쓴다는 것을 알았다.
> - 그럼 strict mode 가 아닐때 this 는 뭐가 나올지 찍어봤더니 window 전역객체가 나왔다.