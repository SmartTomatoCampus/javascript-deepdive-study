# [JavaScript] 46장 제너레이터와 async/await

# 46.1 제너레이터란?

ES6 에서 도입된 제너레이터(generator)는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수다.
제너레이터와 일반 함수의 차이는 다음과 같다.

1. **제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.**
    
    일반 함수의 호출자는 함수를 호출한 후 함수 실행을 제어할 수 없다. 반면 제너레이터 함수는 함수 실행을 함수 호출자가 제어할 수 있다.
    
    즉, 호출자가 함수 실행을 일시 중지시키거나 재개시킬 수 있으며, 이는 **함수의 제어권을 함수가 독점하는 것이 아니라 함수 호출자에게 양도(yield)**할 수 있다는 것을 의미한다.
    
2. **제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.**
    
    일반 함수를 호출하면 매개변수를 통해 외부에서 값을 주입받고 함수 코드를 일괄 실행하여 결과값을 반환한다.
    제너레이터 함수는 호출자와 양방향으로 함수의 상태를 주고받을 수 있다.
    
    즉, **제너레이터 함수는 함수 호출자에게 상태를 전달할 수 있고 함수 호출자로부터 상태를 전달받을 수도 있다.**
    
3. **제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.**
    
    일반 함수를 호출하면 함수 코드를 일괄 실행하고 값을 반환한다.
    
    **제너레이터 함수를 호출하면 함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다.**
    

# 46.2 제너레이터 함수의 정의

제너레이터 함수는 `function*` 키워드로 선언한다.
그리고 하나 이상의 `yield` 표현식을 포함한다.

```jsx
// 제너레이터 함수 선언문
function* getDecFunc() {
	yield 1
}

// 제너레이터 함수 표현식
const getExpFunc = function* () {
	yield 1
}

// 제너레이터 메서드
const obj = {
	* genObjMethod() {
		yield 1
	}
}

// 제너레이터 클래스 메서드
class MyClass {
	* genClsMethod() {
		yield 1
	}
}
```

애스터리스크(*)의 위치는 function 키워드와 함수 이름 사이라면 어디든지 상관없다.

그러나 일관성을 위해 function* 처럼 바로 붙이는 것이 권장된다.

제너레이터 함수는 화살표 함수로 정의할 수 없다.

```jsx
const getArrowFunc = *() => {
	yield 1
} // Syntax Error
```

제너레이터 함수는 `new` 연산자와 함께 생성자 함수로 호출할 수 없다.

```jsx
function* genFunc() {
	yield 1
}

new genFunc() // TypeError
```

# 46.3 제너레이터 객체

**제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라, 제너레이터 객체를 생성해 반환한다.
제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.**

다시말해, 제너레이터 객체는 `Symbol.iterator` 메서드를 상속받는 이터러블이면서 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 `next` 메서드를 소유하는 이터레이터다.
제너레이터 객체는 `next` 메서드를 가지는 이터레이터이므로 `Symbol.iterator` 메서드를 호출해서 별도로 이터레이터를 생성할 필요가 없다.

```jsx
function* genFunc() {
	yield 1
	yield 2
	yield 3
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
const generator = genFunc()
// 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.
// 이터러블은 Symbol.iterator 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체다.
console.log(Symbol.iterator in generator) // true
// 이터레이터는 next 메서드를 갖는다.
console.log('next' in generator) // true
```

제너레이터 객체는 `next` 메서드를 갖는 이터레이터지만 이터레이터에는 없는 return, throw 메서드를 갖는다.
제너레이터 객체의 세 개의 메서드를 호출하면 다음과 같다.

- `next` 메서드를 호출하면 제너레이터 함수의 `yield` 표현식까지 코드 블록을 실행하고 yield 된 값을 value 프로퍼티 값으로, false 를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
- return 메서드를 호출하면 인수로 전달받은 값을 value 프로퍼티 값으로, true 를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
    
    ```jsx
    function* genFunc() {
    	try {
    		yield 1
    		yield 2
    		yield 3
    	} catch(e) {
    		console.error(e)
    	}
    }
    
    const generator = genFunc()
    
    console.log(generator.next()) // {value: 1, done: false}
    console.log(generator.return('End!') // {value: 'End!', done: true}
    ```
    
- throw 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 undefined 를 value 프로퍼티 값으로, true 를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
    
    ```jsx
    function* genFunc() {
    	try {
    		yield 1
    		yield 2
    		yield 3
    	} catch(e) {
    		console.error(e)
    	}
    }
    
    const generator = genFunc()
    
    console.log(generator.next()) // {value: 1, done: false}
    console.log(generator.throw('End!') // {value: undefined, done: true}
    ```
    

# 46.4 제너레이터의 일시 중지와 재개

제너레이터는 `yield` 키워드와 `next` 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수 있다.
일반 함수는 호출 이후 제어권을 함수가 독점하지만 제너레이터는 함수 호출자에게 제어권을 양도(yield)하여 필요한 시점에 함수 실행을 재개할 수 있다.

단, 일반 함수처럼 한 번에 코드 블록의 모든 코드를 일괄 실행하는 것이 아니라 `yield` 표현식까지만 실행한다.
`**yield` 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 `yield` 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환한다.**

```jsx
// 제너레이터 함수
function* genFunc() {
	yield 1
	yield 2
	yield 3
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
// 이터러블이면서 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖는다.
const generator = genFunc()

// 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체를 반환한다.
console.log(generator.next()) // {value: 1, done: false}
console.log(generator.next()) // {value: 2, done: false}
console.log(generator.next()) // {value: 3, done: false}
console.log(generator.next()) // {value: undefined, done: true}
```

<aside>
💻 **제너레이터 객체**

---

- 제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 실행되고 일시 중지된다. 이때 함수의 제어권이 호출자로 양도(yield)된다.
- 이때 제너레이터 객체의 next 메서드는 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.
- 제너레이터 객체의 next 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당한다.
    - 코드 예시
        
        ```jsx
        function* genFunc() {
        	// 처음 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지된다.
        	// 이때 yield 된 값 1은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
        	// x 변수에는 아직 아무것도 할당되지 않았다. x 변수의 값은 next 메서드가 두번째 호출될 때 결정된다.
        	const x = yield 1
        
        	const y = yield (x + 10)
        
        	return x + y
        }
        
        const generator = genFunc(0)
        
        let res = generator.next()
        console.log(res) // {value: 1, done: false}
        
        res = generator.next(10)
        console.log(res) // {value: 20, done: false}
        
        res = generator.next(20)
        console.log(res) // {value: 30, done: true}
        ```
        
</aside>

# 46.5 제너레이터의 활용

## 46.5.1 이터러블의 구현

---

제너레이터 함수를 사용하면 이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 간단히 이터러블을 구현할 수 있다.

```jsx
// 무한 이터러블을 생성하는 제너레이터 함수
const infiniteFibonacci = (function* () {
	let [pre, cur] = [0, 1]

	while(true) {
		[pre, cur] = [cur, pre + cur]
		yield cur
	}
}())

// infiniteFibonacci 는 무한 이터러블이다.
for (const num of inifiniteFibonacci) {
	if (num > 1000) break
	console.log(num) // 1 2 3 5 8 ... 2584 4181 6765
}
```

## 46.5.2 비동기 처리

---

제너레이터 함수는 `next` 메서드와 `yield` 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있다.
이러한 특성을 활용해 프로미슬을 사용한 비동기 처리를 동기 처리처럼 구현할 수 있다.

다시 말해, 프로미스의 후속 처리 메서드 then/catch/finally 없이 비동기 처리 결과를 반환하도록 구현할 수 있다.

```jsx
// node-fetch 는 Node.js 환경에서 window.fetch 함수를 사용하기 위한 패키지다.
// 브라우저 환경에서 이 예제를 실행한다면 아래 코드는 필요 없다.
const fetch = require('node-fetch')

// 제너레이터 실행기
const async = generatorFunc => {
	const generator = generatorFunc() // 2)

	const onResolved = arg => {
		const result = generator.next(arg) // 5)

		return result.done
			? result.value // 9)
			: result.value.then(res => onResolved(res)) // 7)
	}

	return onResolved // 3)
}

(async(function* fetchTodo () { // 1)
	const url = 'https://....'
	
	const response = yield fetch(url) // 6)
	const todo = yield response.json() // 8)
	console.log(todo)
	// {userId; 1, id: 1, title: 'delectus aut autem', completed: false}
})()) // 4)
```

위 예제는 다음과 같이 동작한다.

1. async 함수가 호출(1)되면 인수로 전달받은 제너레이터 함수 FetchTodo 를 호출하여 제너레이터 객체를 생성(2)하고 onResolved 함수를 반환(3)한다. onResolved 함수는 상위스코프의 generator 변수를 기억하는 클로저다.
async 함수가 반환한 onResolved 함수를 즉시 호출(4)하여 2)에서 생성한 제너레이터 객체의 next 메서드를 호출(5)한다.
2. next 메서드가 처음 호출(5)되면 제너레이터 함수 fetchTodo 의 첫번째 yield 문(6)까지 실행된다. 이때 next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 false, 즉 아직 제너레이터 함수가 끝까지 실행되지 않았다면 value 프로퍼티값, 즉 첫번째 yield 된 fetch 함수가 반환한 프로미스가 resolve 한 Response 객체를 onResolved 함수에 인수로 전달하면서 재귀 호출(7)한다.
3. onResolved 함수에 인수로 전달한 Response 객체를 next 메서드에 인수로 전달하면서 next 메서드를 두번째로 호출(5)한다. 이때 next 메서드에 인수로 전달한 Response 객체는 제너레이터 함수 fetchTodo 의 response 변수(6)에 할당되고 제너레이터 함수 fetchTodo 의 두번쨰 yield 문(8)까지 실행된다.
4. next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 false, 즉 아직 제너레이터 함수 fetchTodo 가 끝까지 실행되지 않았다면 이터레이터 리절트 객체의 value 프로퍼틱밧, 즉 두번쨰 yield 된 response.json 메서드가 반환한 프로미스가 resolve 한 todo 객체를 OnResolved 함수에 인수로 전달하면서 재귀호출(7)한다.
5. onResolved 함수에 인수로 전달된 todo 객체를 next 메서드에 인수로 전달하면서 next 메서드를 세번째로 호출(5)한다. 이때 next 메서드에 인수로 전달한 todo 객체는 제너레이터 함수 FetchTodo 의 todo 변수(8)에 할당되고 제너레이터 함수 fetchTodo가 끝까지 실행된다.
6. next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티값이 true, 즉 제너레이터 함수 fetchTodo 가 끝까지 실행되었다면 이터레이터 리절트 객체의 value 프로퍼티값, 즉 제너레이터 함수 FetchTodo 의 반환값이 undefined 를 그대로 반환(9)하고 처리를 종료한다.

# 46.6 async/await

제너레이터를 사용해서 비동기 처리를 동기 처리처럼 동작하도록 구현했지만 코드가 무척이나 장황해지고 가독성도 나빠졌다. ES8(ECMAScript 2017)에서는 제너레이터보다 간단하고 가독성 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있는 async/await 가 도입되었다.

async/await 는 프로미스를 기반으로 동작한다. async/await 를 사용하면 프로미스의 then/catch/finally 후속처리 메서드에 콜백 함수를 전달해서 비동기 처리 결과를 후속처리할 필요 없이 마치 동기 처리처럼 프로미스를 사용할 수 있다.

다시 말해, 프로미스의 후속 처리 메서드 없이 마치 동기처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있다.

```jsx
const fetch = require('node-fetch')

async function fetchTodo() {
	const url = 'https://...'

	const response = await fetch(url)
	const todo = await response.json()
	console.log(todo)
}

fetchTodo()
```

## 46.6.1 async 함수

---

`await` 키워드는 반드시 `async` 함수 내부에서 사용해야 한다. `async` 함수는 `async` 키워드를 사용해 정의하며 언제나 프로미스를 반환한다. `async` 함수가 명시적으로 프로미스를 반환하지 않더라도 `async` 함수는 암묵적으로 반환값을 resolve 하는 프로미스를 반환한다.

## 46.6.2 awiat 키워드

---

`await` 키워드는 프로미스가 settled 상태(비동기 처리가 수행된 상태)가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve 한 처리 결과를 반환한다. `await` 키워드는 반드시 프로미스 앞에서 사용해야 한다.

## 46.6.3 에러 처리

---

async/await 구문에서 에러처리는 `try...catch` 구문을 사용할 수 있다. 콜백함수를 인수로 전달받는 비동기와는 달리 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 때문에 호출자가 명확하다.

```jsx
const fetch = require('node-fetch')

const foo = async () => {
	try {
		const wrongUrl = 'https://wrong.url'

		const response = await fetch(wrongUrl)
		const data = await response.json()
		cnosole.log(data)
	} catch(err) {
		console.error(err)
	}
}

foo()
```

위 예제의 foo 함수의 catch 문은 HTTP 통신에서 발생한 네트워크 에러 뿐 아니라 try 코드 블록 내의 모든 문에서 발생한 일반적인 에러까지 모두 캐치할 수 있다.

**async 함수 내에서 catch 문을 사용해서 에러처리를 하지 않으면 async 함수는 발생한 에러를 reject 하는 프로미스를 반환한다.**
따라서 async 함수를 호출하고 catch 후속 처리 메서드를 사용해 에러를 캐치할 수 있다.

```jsx
const fetch = require('node-fetch')

const foo = async () => {
	const wrongUrl = 'https://wrong.url'

	const response = await fetch(wrongUrl)
	const data = await response.json()
	cnosole.log(data)
}

foo()
	.then(cnosole.log)
	.catch(console.error)
```