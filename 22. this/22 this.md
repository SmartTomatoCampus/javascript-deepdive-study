# 22. this

생성일: 2022년 3월 15일 오후 9:25

## 22. 1 this 키워드

- 메서드가 자신이 속한 객체의 프로퍼티를 참조하기 위해서는 자신이 속한 객체를 가리키는 식별자를 참조 할 수 있어야한다.
- 생성자함수 내부에서 프로퍼티나 메서드를 추가하기 위해서는 자신이 생성할 인스턴스를 참조할 수 있어야 한다.
→ 이전에 생성자 함수가 존재해야하고 그 생성자 함수에는 자신이 속한 객체나 자신이 생성할 인스턴스를 가리키는 특수한 식별자가 필요한데 **이 때 this라는 특수한 식별자를 사용한다.**
- **this는 자신이 속한 객체나 자신이 생성할 인스턴스를 가리키는 자기참조 변수이다.**
- 자바스크립트의 this는 함수가 호출되는 방식에 따라 this가 바인딩 될 값, this 바인딩이 동적으로 결정된다.

```jsx
const circle = {
	radius : 5,
	getDiameter(){
		return 2 * circle.radius;
	}
};

console.log(circle.getDiameter());
```

## 22. 2 함수 호출 방식과 this 바인딩

> this 바인딩은 함수호출 방식에 따라 동적으로 결정된다.
> 
1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

```jsx
// this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
const foo = function (){
	console.dir(this);
};

// 동일한 함수도 다양한 방식으로 호출할 수 있다.

// 1. 일반함수 호출
foo(); // window

// 2. 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킨다.
const obj = { foo };
obj.foo(); // obj

// 3. 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo();  // foo {}

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
// foo 힘수 내부의 this는 인수에 의해 결정된다.
const bar = {name: 'bar'};

foo.call(bar);  // bar
foo.apply(bar); // bar
foo.bind(bar)();  // bar

```

### 22. 2. 1 일반 함수 호출

- 기본적으로 this에는 전역객체가 바인딩된다.
- 하지만 객체를 생성하지 않는 일반함수에서는 this가 의미 없다.
- strict 모드일 경우에는 함수 내부의 this에 undefined가 바인딩된다.
- 메서드 내에서 정의한 중첩함수도 일반함수로 호출되면 this에는 전역객체가 바인딩된다.

```jsx
function foo(){
	console.log("foo's this: ", this);  // window
	function bar(){
		console.log("bar's this: ", this);  // window
	}
	bar();
}
foo();
```

### 22. 2. 2 메서드 호출

> this에 바인딩되는 메서드는 해당 메서드를 호출한 객체에 바인딩 된다.
> 

```jsx
const person = {
	name: 'Lee',
	getName() {
		// 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
		return this.name;
	}
};

console.log(person.getName());  // Lee
```

![Untitled](22%20this%20668b8/Untitled.png)

![Untitled](22%20this%20668b8/Untitled%201.png)

![Untitled](22%20this%20668b8/Untitled%202.png)

### 22. 2. 3 생성자 함수 호출

> 생성자 함수 내부의 this에는 생성자함수가 생성할 인스턴스가 바인딩된다.
> 

```jsx
// 생성자 함수
function Circle(radius){
	this.radius = radius;
	this.getDiameter = function(){
		return 2 * this.radius;
	};
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter());  // 10
console.log(circle2.getDiameter());  // 20

// new 연산자와 호출하지 않으면 일반함수로 호출된다.
const circle3 = Circle(15);

// 일반함수로 호출된 Circle은 반환문이 없기 때문에 암묵적으로 undefined를 반환한다.
console.log(circle3);  // undefined
console.log(radius);  // 15
```

### 22. 2. 4 Function.prototype.apply/call/bind 메서드에 의한 간접호출

> apply, call, bind 메서드는 Function.prototype의 메서드다
> 

→ 모든 함수가 상속받아 사용 할 수 있다.

- apply/call 메서드는 this로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출한다.
- 함수를 호출하면 첫번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩한다.

```jsx
function getThisBinding(){
	return this;
}

const thisArg = { a: 1 };

console.log(getThisBinding());  // window

console.log(getThisBinding.apply(thisArg));  // {a: 1}
console.log(getThisBinding.call(thisArg));  // {a: 1}
```