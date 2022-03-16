# [JavaScript] this (+apply, call, bind)

# this 키워드

---

객체지향 프로그래밍에서 객체는 프로퍼티와 메서드를 갖고,
동작을 나타내는 메서드는 자신이 속한 객체의 프로퍼티를 변경할 수 있어야 한다.

→ 이때 메서드가 자신이 속한 객체의 프로퍼티를 참조하기 위해, **자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.**

```jsx
const circle = {
	radius: 5,
	getDiameter() {
		// 이 메서드가 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면
		// 자신이 속한 객체인 circle 을 참조할 수 있어야 한다.
		return 2 * circle.radius
	}
}
```

위 코드에서 객체 리터럴은 circle 변수에 할당되기 직전에 평가된다.
따라서 getDiameter 메서드가 호출되는 시점에는 이미 객체 리터럴의 평가가 완료되어 객체가 생성되었고 circle 식별자에 생성된 객체가 할당된 이후다.
따라서 메서드 내부에서 circle 식별자를 참조할 수 있다.

반면 이 경우 생성자 함수로 인스턴스를 호출할 때 circle 식별자를 알 수 없다는 문제가 있다.

```jsx
function Circle (radius) {
	// 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
	???.radius = radius
}

Circle.prototype.getDiameter = function() {
	// 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
	return 2 * ???.radius
}

// 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 한다.
const circle = new Circle(5)
```

생성자 함수 내부에서는 프로퍼티/메서드를 추가하기 위해 자신이 생성할 인스턴스를 참조할 수 있어야 하지만,
생성자 함수로 객체를 생성하는 방식은
1) 먼저 생성자 함수를 정의
2) `new` 연산자로 생성자 함수를 호출
인스턴스를 생성하기 위해 이런 단계가 추가로 들어감

또한 생성자 함수를 정의하는 시점에는 아직 인스턴스 생성 전이므로 인스턴스를 가리키는 식별자를 생성자 함수가 알 수 없다.
따라서 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 특수한 식별자가 바로 `**this**` 다!!!

> `**this` 는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수(self-referencing variable)다!!**
> 
> 
> `**this` 를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.**
> 
> 코드 어디서든 `this` 를 참조할 수 있고, `this` 또한 지역변수처럼 사용할 수도 있다.
> 
> 킹치만 `this` 가 가리키는 값, **즉 `this` 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.**
> 

<aside>
💻 `**this` 바인딩**

---

바인딩이란 식별자와 값을 연결하는 과정이다.

`this` 바인딩은 `this`(키워드로 분류되지만 식별자의 역할)와 `this`가 가리킬 객체를 바인딩하는 것!

</aside>

### 객체 리터럴

```jsx
const circle = {
	radius: 5,
	getDiameter() {
		// this 는 메서드를 호출한 객체를 가리킨다.
		return 2 * this.radius
	}
}
console.log(circle.getDiameter()) // 10
```

### 생성자 함수

```jsx
function Circle(radius) {
	// this 는 생성자 함수가 생성할 인스턴스를 가리킨다.
	this.radius = radius
}

Circle.porototype.getDiameter = function() {
	// this 는 생성자 함수가 생성할 인스턴스를 가리킨다.
	return 2 * this.radius
}

// 인스턴스 생성
const circle = new Circle(5)
console.log(circle.getDiameter()) // 10
```

생성자 함수 내부의 `this`는 생성자 함수가 생성할 인스턴스를 가리킨다.

이처럼 `this` 는 상황에 따라 가리키는 대상이 다르다.

> **자바스크립트의 `this` 는 함수가 호출되는 방식에 따라 `this` 에 바인딩될 값, 즉 `this` 바인딩이 동적으로 결정된다.**
> 
> 
> `this` 는 코드 어디에서든 참조 가능하며, 전역에서도 함수 내부에서도 참조할 수 있다.
> 

### 1. 전역에서 `this` 는 전역객체 window 를 가리킨다.

```jsx
// this 는 어디서든지 참조 가능하다.
// 전역에서 this 는 window 전역객체를 가리킨다.
console.log(this) // window

function square(number) {
	// 일반 함수 내부에서 this 는 전역 객체 window 를 가리킨다.
	console.log(this) // window
	return number * number
}
square(2)
```

### 2. 메서드 내부에서 `this` 는 메서드를 호출한 객체를 가리킨다.

```jsx
const person = {
	name: 'KMin',
	getName() {
		// 메서드 내부에서 this 는 메서드를 호출한 객체를 가리킨다.
		console.log(this) // {name: 'KMin', getName: f}
		return this.name
	}
}
console.og(person.getName()) // KMin
```

### 3. 생성자 함수 내부에서 `this` 는 생성자 함수가 생성할 인스턴스를 가리킨다.

```jsx
function Person(name) {
	this.name = name
	// 생성자 함수 내부에서 this 는 생성자 함수가 생성할 인스턴스를 가리킨다.
	console.log(this) // Person {name: 'KMin'}
}

const me = new Person('KMin')
```

> 하지만 `this` 는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로, 일반적으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다.
> 
> 
> 때문에 strict mode 에서 일반함수의 `this` 는 undefined 가 바인딩된다./노랑
> 

# 함수 호출 방식과 this 바인딩

---

`**this` 바인딩(this 에 바인딩될 값)은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다**

<aside>
💻 **렉시컬 스코프와 `this` 바인딩은 결정 시기가 다르다**

---

함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정한다.

하지만 `this` 바인딩은 함수 호출 시점에 결정된다.

</aside>

주의할 것은 함수 호출 방식도 참 다양하다는 것!

1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. `Function.prototype.apply / call / bind` 메서드에 의한 간접 호출

### `**this` 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.**

```jsx
const foo = function() {
	console.log(this)
}
```

1. **일반 함수 호출**

```jsx
foo() // window
```

일반적 방식으로 foo 함수를 호출할 때 함수 내부의 `this` 는 전역객체 window 를 가리킨다

1. **메서드 호출**

```jsx
const obj = { foo }
obj.foo() // obj
```

foo 함수를 프로퍼티 값으로 할당하여 호출

foo 함수 내부의 `this` 는 메서드를 호출한 객체 obj 를 가리킨다.

1. 생성자 함수 호출

```jsx
new foo() // foo {}
```

foo 함수를 `new` 연산자와 함께 생성자 함수로 호출하면 `this` 는 생성자 함수가 생성한 인스턴스를 가리킨다.

1. **Function.prototype.apply/call/bind 메서드에 의한 간접 호출**

```jsx
const bar = { name: 'bar' }

foo.call(bar)   // bar
foo.apply(bar)  // bar
foo.bind(bar)() // bar
```

`Function.prototype.apply / call / bind` 메서드에 의해 foo 함수를 간접 호출하면 내부의 `this` 는 인수에 의해 결정된다.

## 1. 일반 함수 호출

**기본적으로 `this` 에는 전역 객체 window 가 바인딩된다**

```jsx
function foo() {
	console.log(this) // window
	function bar() {
		console.log(this) // window
	}
	bar()
}
foo()
```

위 예제처럼 전역 함수는 물론이고, 중첩함수 또한 일반 함수로 호출하면 내부 함수의 `this` 에는 전역 객에 window 가 바인딩된다.

`this` 는 개체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 객체를 생성하지 않는 일반 함수에서 `this` 는 의마가 없다.

```jsx
var value = 1 // var 키워드로 선언한 전역 변수는 전역객체 window 의 프로퍼티다.

const obj = {
	value: 100,
	foo() {
		console.log(this)       // {value: 100, foo: f}
		console.log(this.value) // 100

		// 메서드 내에서 정의한 중첩함수
		function bar() {
			console.log(this)       // window
			console.log(this.value) // 1
		}
		bar() // 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this 에는 전역 객체가 바인딩된다.
	}
}

obj.foo()
```

> 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 `this` 에는 전역 객체 window 가 바인딩된다.
> 

```jsx
var value = 1

const obj = {
	value: 100,
	foo() {
		console.log(this) // {value: 100, foo: f}
		// 콜백 함수 내부의 this 에는 전역 객체가 바인딩된다.
		setTimeout(function() {
			console.log(this) // window
			console.log(this.value) // 1
		}, 1000)
	}
}

obj.foo()
```

> 콜백 함수가 일반 함수로 호출된다면 콜백 함수 내부의 `this` 에도 전역 객체가 바인딩된다.
> 

### 🍅 메서드 내부 중첩 함수나 콜백 함수의 `this` 를 바인딩하는 방법

```jsx
var value = 1

const obj = {
	value: 100
	foo() {
		const that = this // this 바인딩(obj)을 변수 that 에 할당한다
		
		// 콜백 함수 내부에서 this 대신 that 을 참조한다		
		setTimeout(function() {
			console.log(that.value) // 100
		}, 1000)
	}
}

obj.foo()
```

⇒ 콜백 함수 내부에서 콜백 함수를 호출하는 객체(obj) `this` 를 변수 that 에 할당한뒤,
함수 내부의 중첩함수에서 변수 that 을 참조하는 것!!

```jsx
var value = 1

const obj = {
	value: 100,
	foo() {
		setTimeout(function() {
			console.log(this.value) // 100
		}.bind(this), 1000)
	}
}

obj.foo()
```

⇒ 또는 이렇게 `Function.prototype.apply`, `Function.prototype.call`, `Function.prototype.bind` 메서드를 통해서 바인딩할 수 있다.

```jsx
var value = 1

const obj = {
	value: 100,
	foo() {
		// 화살표 함수 내부의 this 는 상위 스코프의 this 를 가리킨다.
		setTimeout(() => console.log(this.value), 1000) // 100
	}
}

obj.foo()
```

⇒ 또는 이렇게 화살표함수를 사용한다!!

상위 스코프인 foo 함수의 `this` 는 obj.foo() 처럼 메서드로 호출될 것이므로 상위 스코프의 `this` 는 obj 이기 때문에,
화살표 함수 내부의 `this` 또한 자동으로 obj 가 된다.

## 2. 메서드 호출

메서드 내부의 `this` 는 메서드를 호출한, 즉 메서드 이름 앞 마침표(.) 앞의 객체를 가리킨다.

**다만 메서드 내부의 `this` 는 메서드를 소유한 객체가 아닌, 메서드를 호출한 객체에 바인딩된다는 점이다!!**

```jsx
const person = {
	name: 'KMin',
	getName() {
		// 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
		return this.name
	}
}

// 메서드 getName 을 호출한 객체는 person 이다.
console.log(person.getName()) // KMin
```

따라서 getName 프로퍼티가 가리키는 함수 객체, 즉 getName 메서드는 다른 객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될 수 있고 일반 변수에 할당하여 일반 함수로 호출될 수도 있다.

```jsx
const anotherPerson = {
	name: 'Choi'
}

// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName

// getName 메서드를 호출한 객체는 anotherPerson 이다.
console.log(anotherPerson.getName()) // Choi

// getName 메서드를 변수에 할당
const getName = person.getName

// getName 메서드를 일반 함수로 호출
console.log(getName) // ''
// 일반 함수로 호출된 getName 함수 내부의 this.name 은 브라우저 환경에서 window.name 과 같다.
```

(참고로 Node.js 환경에서 [this.name](http://this.name) 은 undefined다)

프로토타입 메서드 내부에서 사용된 `this` 도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩된다.

```jsx
function Person(name) {
	this.name = name
}

Person.prototype.getName = function() {
	return this.name
}

const me = new Person('KMin')

// getName 메서드를 호출한 객체는 me다.
console.log(me.getName()) // 1) KMin

Person.prototype.name = 'Choi'

// getName 메서드를 호출한 객체는 Person.prototype 이다.
console.log(Person.prototype.getName()) // 2) Choi
```

1) 의 경우 getName 메서드를 호출한 객체는 me → 따라서 getName 내부의 `this` 는 me 를 가리키므로 [this.name](http://this.name) 은 ‘KMin’이다

2) 의 경우 getName 메서드를 호출한 객체는 Person.prototype → 따라서 getName 내부의 `this` 는 Person.prototype 을 가리키며 [this.name](http://this.name) 은 ‘Choi’ 다

## 3. 생성자 함수 호출

생성자 함수 내부의 `this` 는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.

```jsx
// 생성자 함수
function Circle(radius) {
	// 생성자 함수 내부의 this 는 생성자 함수가 생성할 인스턴스
	this.radius = radius
	this.getDiameter = function() {
		return 2 * this.radius
	}
}

const circle1 = new Circle(5)
const circle2 = new Circle(10)

console.log(circle1.getDiameter()) // 10
console.log(circle2.getDiameter()) // 20
```

## 4. Function.prototype.apply / call / bind 메서드에 의한 간접 호출

`apply`, `call`, `bind` 메서드는 Function.prototype 의 메서드기 때문에 모든 함수가 상속받아 사용할 수 있다.

### apply, call 메서드

<aside>
💻 **apply 와 call 메서드의 사용법**

---

```jsx
/**
* 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수를 호출한다.
* **@param** **thisArg** - this 로 사용할 객체
* **@param** **argsArray** - 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
* **@returns** 호출된 함수의 반환값
*/
Function.prototype.apply(thisArg[, argsArray])

/**
* 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수를 호출한다.
* **@param** **thisArg** - this 로 사용할 객체
* **@param** **arg1, arg2, ...** - 함수에게 전달할 인수 리스트
* **@returns** 호출된 함수의 반환값
*/
Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])
```

</aside>

```jsx
function getThisBinding() {
	return this
}

// this 로 사용할 객체
const thisArg = { a: 1 }

console.log(getTHisBinding()) // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBInding 함수의 this 에 바인당한다.
console.log(getThisBinding.apply(thisArg)) // { a: 1 }
console.log(getThisBinding.call(thisArg))  // { a: 1 }
```

**apply 와 call 메서드의 본질적인 기능은 함수를 호출하는 것이다.**

→ apply, call 메서드는 함수를 호출하면서 첫번째 인수로 전달한 특정 객체를 호출한 함수의 `this` 에 바인딩한다.

```jsx
function getThisBinding() {
	console.log(arguments)
	return this
}

// this 로 사용할 객체
const thisArg = { a: 1 }

/**
* getThisBinding 함수를 호출하면서 인수로 전달한 thisArg 객체를 getThisBinding 함수의 this 에 바인딩한다.
*/

// apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달
console.log(getThisBinding.apply(thisArg, [1, 2, 3]))
// Arguments(3) [1, 2, 3, callee: f, Symbol(Symbol.iterator): f]
// { a: 1 }

// call 메서드는 호출함 함수으 ㅣ인수를 쉼표로 구분한 리스트 형식으로 전달한다.
console.log(getThisBinding.call(thisArg, 1, 2, 3))
// Arguments(3) [1, 2, 3, callee: f, Symbol(Symbol.iterator): f]
// { a: 1 }
```

- apply, call 메서드의 대표적인 용도는 `arguments` 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우다.
    
    `arguments` 객체는 배열이 아니기 때문에 slice() 메서드를 사용할 수 없으나 apply, call 메서드를 이용하면 가능하다.
    
    ```jsx
    function convertArgsToArray() {
    	console.log(arguments)
    
    	// arguments 객체를 배열로 변환
    	// Array.prototype.slice 를 인수없이 호출하면 배열의 복사본을 생성한다.
    	const arr = Array.prototype.slice.call(arguments)
    	// const arr = Array.prototype.slicel.apply(arguments)
    	console.log(arr)
    
    	return arr
    }
    
    covertArgsToArray(1, 2, 3) // [1, 2, 3]
    ```
    

### bind 메서드

Function.prototype.bind 메서드는 apply, call 메서드와 달리 함수를 호출하지 않는다.

다만 첫번째 인수로 전달한 값으로 `this` 바인딩이 교체된 함수를 새롭게 생성해 반환한다.

```jsx
const getThisBinding() {
	return this
}

// this 로 사용할 객체
const thisArg = { a: 1 }

// bind 메서드는 첫번째 인수로 전달한 thisArg 로 this 바인딩이 교체된 getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.apply(thisArg)) // getThisBinding
console.log(getThisBinding.apply(thisArg())) // { a: 1 }
```

bind 메서드야말로 메서드의 `this` 와 메서드 내부의 중첩함수 또는 콜백함수의 `this` 가 불일치하는 문제를 해결하기 위해 유용하게 쓰인다.

1. bind 메서드로 `this` 바인딩 전
    
    ```jsx
    const person = {
    	name: 'KMin',
    	foo(callback) {
    		// 1)
    		setTimeout(callback, 1000)
    	}
    }
    
    person.foo(function () {
    	console.log(this.name) // 2) ''
    })
    ```
    
    위의 1) 의 시점에서 `this` 는 foo 메서드를 호출한 객체 즉 person 객체를 가리키지만,
    person.foo 콜백함수가 일반함수로 호출된 2) 시점에서 `this` 는 전역객체 window 를 가리킨다.
    
    따라서 2) 시점에서 [this.name](http://this.name) 은 [window.name](http://window.name) 과 같다
    
2. bind 메서드로 `this` 바인딩 후
    
    ```jsx
    const person = {
    	name: 'KMin',
    	foo(callback) {
    		// bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    		setTimeout(callback.bind(this), 1000)
    	}
    }
    
    person.foo(function () {
    	console.log(this.name) // KMin
    })
    ```
    
    보통은 객체 내부의 `this` 와 콜백함수 내부의 `this` 가 다르면 문제가 발생한다
    
    따라서 콜백 함수 내부의 `this` 를 외부 함수 내부의 `this` 일치시키기 위해 bind 메서드를 사용한다
    

# 정리

| 함수 호출 방식 | this 바인딩 |
| --- | --- |
| 일반 함수 호출 | 전역 객체 window |
| 메서드 호출 | 메서드를 호출한 객체 |
| 생성자 함수 호출 | 생성자 함수가 (미래에) 생성할 인스턴스 |
| Fnction.prototype.apply/call/bind 메서드에 의한 간접 호출 | Function.prototype.apply/call/bind 메서드의 첫번째 인수로 전달한 객체 |

> **오늘 알게된 것 TIL (2022.03.15 화)**
> 
> 
> ---
> 
> 1. this 는 자신이 생성한 객체나 생성할 인스턴스의 가리키는 자기 참조 변수이고, 때문에 아직 생성되지도 않은 인스턴스의 메서드나 프로퍼티를 참조할 수 있다!!
> 2. 이전에 패캠 강의에서 this 는 자신을 호출한 녀석이 this 라고 했는데 이제 이해가 간다
>     1. 객체 리터럴 내부 메서드의 this 는 객체가 부르는 애이므로 this 는 바로 메서드가 속한 객체
>     2. 생성자 함수에서 this 는 생성되고 난 후 인스턴스가 써먹을 this 이므로 인스턴스를 가리키고
>     3. 일반 함수는 전역에서 호출되므로(보통은?), this 는 window 이다.
> 3. 객체 내부 중첩함수에서 this 를 바인딩하는 방법
>     1. 원하는 객체를 가리키는 this 를 변수 that 에 할당
>     2. Function.prototype.apply/bind/call 메서드를 써서 바인딩
>     3. 화살표함수를 사용