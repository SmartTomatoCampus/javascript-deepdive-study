# 3월 7일 17장 생성자 함수에 의한 객체 생성

# 17장 생성자 함수에 의한 객체 생성

생성자 함수를 사용하여 객체를 생성하는 방식(10장 객체리터럴 참조)

- 5장 리터럴 : 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용하여 값을 생성하는 표기법
- 10장 객체 리터럴에 의한 객체 생성 : 중괄호 { ... } 내에 0개 이상의 프로퍼티를 정의

객체 리터럴을 사용하여 객체를 생성하는 방식

객체리터럴을 사용 vs 생성자 함수 사용 장단점

<hr>

## 17.1 Object 생성자 함수

new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다. 빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있다.

생성자함수 : new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수<br>
인스턴스 : 생성자 함수에 의해 생성된 객체

- 자바스크립트는 Object 생성자 함수 이외에도 String, Number, Boolean, Function, Array, Date, RegExp, Promise 등의 빌트인 생성자 함수를 제공

- 객체를 생성하는 방법은 특별한 경우를 제외하고는 객체 리터럴을 사용하는 것이 더 간편.
<hr>

## 17.2 생성자 함수

### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

객체리터럴에 의한 객체 생성방식 : 직관적, 간편
단점 : 단 하나의 객체만 생성 (동일한 프로퍼티를 갖는 객체를 여러개 생성해야하는 경우 매번 같은 프로퍼티를 기술해야하기 때문에 비효율적)

```javascript
const circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  },
};

console.log(circle1.getDiameter()); // 10

const circle2 = {
  radius: 10,
  getDiameter() {
    return 2 * this.radius;
  },
};

console.log(circle2.getdiameter()); // 20
```

- 객체는 프로퍼티를 통해 객체 고유의 상태를 표현, 메서드를 통해 상태 데이터인 프로퍼티를 참조하고 조작하는 동작을 표현. 따라서 프로퍼티는 객체마다 프로퍼티 값이 다를 수는 있지만 메서드는 내용이 동일한 경우가 일반적.

\*위 예제에서 원을 표현한 객체인 circle1 객체와 circle2 객체는 프로퍼티 구조가 동일. 객체 고유의 상태 데이터인 raidus 프로퍼티의 값은 객체마다 다를 수 있지만 get Diameter 메서드는 완전히 동일

### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

생성자 함수에 의한 객체 생성 방식 : 프로퍼티 구조가 동일한 객체 여러개를 간편하게 생성 가능

```javascript
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스의 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

> this
>
> > this는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수(Self-referencing variable). this가 가리키는 값. 즉 this 바인딩은 함수 호출방식에 따라 동적으로 결정된다.
> > |함수 호출 방식| this가 가리키는 값(this 바인딩) |
> > | --- | --- |
> > | 일반 함수로서 호출 | 전역객체|
> > | 메서드로서 호출 | 메서드를 호출한 객체( 마침표 앞의 객체) |
> > | 생성자 함수로서 호출 | 생성자 함수가 (미래에) 생성할 인스턴스 |
> >
> > - 22장 this에서 자세히 소개 예정

생성자 함수는 개체(인스턴스)를 생성하는 함수. 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 **new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.** 만약 new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아닌 일반함수로 동작한다.

```javascript
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다.
// 즉, 일반 함수로서 호출된다.
const circle3 = Circle(15);

//일반 함수로서 호출된 Circle은 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3); // undefined

// 일반 함수로서 호출된 Circle 내의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

### 17.2.3 생성자 함수의 인스턴스 생성 과정

생성자 함수의 역할 : 프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿(클래스)으로서 동작하여 _**인스턴스를 생성**_ 하는 것 & _**생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당)**_ 하는 것.

- 생성자 함수가 인스턴스를 생성하는 것은 필수, 생성된 인스턴스를 초기화하는 것은 옵션

- 자바스크립트 엔진은 암묵적인 처리를 통해 인스턴스를 생성, 초기화,반환

1.  인스턴스 생성과 this 바인딩<br/>
    암묵적으로 빈 객체가 생성된다. 이 빈 객체가 바로 (아직 완성되지는 않았지만) 생성자 함수가 생성한 인스턴스다. 그리고 암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩 된다. 생성자 함수 내부의 this가 생성자 함수가 생성할 인스턴스를 가리키는 이유가 바로 이것이다. 이 처리는 함수 몸체의 코드가 한 줄씩 실행되는 런타임 이전에 실행된다.

> 바인딩
>
> > 바인딩이란 식별자와 값을 연결하는 과정을 의미한다. 예를 들어 변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것이다. this바인딩은 this(키워드로 분류되지만 식별자 역할을 한다)와 this가 가리킬 객체를 바인딩하는 것이다.

```javascript
function Circle(radius) {
  //1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
  console.log(this); // Circle {}

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}
```

2. 인스턴스 초기화
   생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 this에 바인딩 되어 있는 인스턴스를 초기화한다. 즉, this에 바인딩 되어 있는 인스턴스에 프로퍼티나 메서드를 추가하고 생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당한다. 이 처리는 개발자가 기술한다.

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩 되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}
```

3. 인스턴스 반환
   생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩 된 this가 암묵적으로 반환된다.

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩 되어있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
}
// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); //Circle {radius: 1, getDiameter: f}
```

만약 this가 아닌 다른 객체를 명시적으로 반환하면 this가 반환되지 못하고 return 문에 명시된 객체가 반환된다.

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩 되어있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
  // 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다.
  return {};
}

// 인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
const circle = new Circle(1);
console.log(circle); // {}
```

하지만 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩 되어있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
  // 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.
  return 100;
}

// 인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다. ?????????????????

const circle = new Circle(1);
console.log(circle); //Circle {radius: 1, getDiameter: f}
```

이처럼 생성자 함수 내부에서 명시적으로 this가 아닌 다른 값을 반환하는 것은 생성자 함수의 기본 동작을 훼손한다. 따라서 생성자 함수 내부에서 return문을 반드시 생략해야 한다.

### 17.2.4 내부 메서드 [[call]]과 [[Construct]]

함수 선언문 또는 함수 표현식으로 정의한 함수는 일반적인 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있따. 생성자 함수로서 호출한다는 것은 new 연산자와 함께 호출하여 객체를 생성하는 것을 의미한다.

함수는 객체이므로 일반 객체와 동일하게 동작할 수 있다. 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드를 모두 가지고 있기 때문이다.

```js
// 함수는 객체다.
function foo() {}

// 함수는 객체이므로 프로퍼티를 소유할 수 있다.
foo.prop = 10;

// 함수는 객체이므로 메서드를 소유할 수 있다.
foo.method = function () {
  console.log(this.prop);
};

foo.method(); // 10
```

함수는 객체이지만 일반 객체와는 다르다. 일반 객체는 호출할 수 없지만 함수는 호출할 수 있다. 따라서 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 매부 메서드는 물론, 함수로서 동작하기 위해 함수 객체만을 위한 [[Environment]], [[FormalParameters]] 등의 내부 슬롯과 [[Call]], [[Construct]]와 같은 내부 메서드를 추가로 가지고 있다.
함수가 일반 함수로서 호출되면 함수 객체 내부메서드[[Call]]이 호출되고 new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드 [[Construct]]가 호출된다.

내부 메서드 [[Call]]을 갖는 함수 객체를 callable이라 하며, 내부 메서드[[Construct]]를 갖는 함수 객체를 constructor, [[Construct]]를 갖지 않는 함수 객체를 non-constructor라고 부른다.<br>
callable = 호출할 수 있는 객체, 즉 함수<br>
constructor = 생성자 함수로서 호출할 수 있는 함수<br>
non-constructor = 객체를 생성자 함수로서 호출할 수 없는 함수<br>

호출할 수 없는 객체 !== 함수객체 이므로 함수객체는 반드시 callable이어야 한다.

- 모든 함수 객체는 호출할 수 있지만 모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아님.

### 17.2.5 constructor와 non-constructor의 구분

함수 객체를 생성할 때 함수 정의 방식에 따라 구분한다.

constructor : 함수 선언문, 함수 표현식, 클래스(클래스도 함수다)

non-constructor : 메서드(ES6 메서드 축약표현), 화살표 함수

주의 \* 생성자 함수로서 호출될 것을 기대하고 정의하지 않은 일반함수(callable이면서 constructor)에 new 연산자를 붙여 호출하면 생성자 함수처럼 동작할 수 있다.

### 17.2.6 new 연산자

일반 함수와 생성자 함수에 특별한 형식적 차이는 없다. new 연산자와 함께 함수를 호출하면 해당 함수는 _생성자 함수로 동작_ 한다. 다시말해, 함수 객체의 내부 메서드 [[Call]]이 호출되는 것이 아니라 [[Construct]]가 호출된다. 단 new연산자와 함께 호출하는 함수는 non-constructor가 아닌 constructor 이어야한다.

반대로 new 연산자 없이 생성자 함수를 호출하면 일반 함수로 호출된다. 다시 말해, 함수 객체의 내부 메서드 [[Constructor]]가 호출되는 것이 아니라 [[Call]]이 호출된다.

```js
//생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수 호출하면 일반 함수로서 호출된다.
const circle = Circle(5);
console.log(circle); //undefined

// 일반 함수 내부의 this는 전역 객체 window를 가리킨다.
console.log(radius); // 5
console.log(getDiameter()); // 10

circle.getDiameter();
// TypeError: Cannot read property 'getDiameter' of undefined
```

Circle 함수를 new 연산자와 함께 생성자 함수로서 호출하면 함수 내부의 this는 Circle 생성자 함수가 생성할 인스턴스를 가리킨다. 하지만 Circle 함수를 일반적인 함수로서 호출하면 함수 내부의 this는 전역 객체 window를 가리킨다.

위 예제의 Circle 함수는 일반 함수로서 호출되었기 때문에 Circle 함수 내부의 this는 전역객체 window를 가리킨다. 따라서 radius 프로퍼티와 getDiameter 메서드는 전역 객체의 프로퍼티와 메서드가 된다.

일반 함수와 생성자 함수의 특별한 형식적 차이는 없다. 따라서 생성자 함수는 일반적으로 첫 문장을 대문자로 기술하는 파스칼 케이스로 명명하여 일반 함수와 구별할 수 있도록 노력한다.

### 17.2.7 new.target

생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 파스칼 케이스 컨벤션을 사용한다 하더라도 실수는 언제나 발생할 수 있다. 이러한 위험성을 회피하기 위해 ES6에서는 new.target을 지원한다.

new.target: this와 유사하게 constructor인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용되며 메타 프로퍼티라고 부른다.

- IE는 new.target 지원x

new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다. new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined다.

따라서 함수 내부에서 new.target을 사용하여 new 연산자와 생성자 함수로서 호출했는지 확인하여 그렇지 않은경우 new 연산자와 함께 재귀 호출을 통해 생성자 함수로서 호출할 수 있다.

```js
//생성자함수
function Circle(radius) {
  //이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined다.
  if (!new.target) {
    //new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
    return new Circle(radius);
  }
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter());
```

> 스코프 세이프 생성자 패턴
>
> > new.target은 IE에서 지원하지 않음. new.target을 사용할 수 없다면 스코프 세이프 생성자 패턴을 사용 가능
> >
> > ```js
> > // scape-Safe constructor pattern
> > function Circle(radius) {
> >   // 생성자 함수가 new 연산자와 함께 호출되면 함수의 선두에서 빈 객체를 생성하고
> >   // this에 바인딩한다. 이 때 this와 Circle은 프로토타입에 의해 연결된다.
> >
> >   // 이 함수가 new 연산자와 함께 호출되지 않았다면 이 시점의 this는 전역 객체 window를 가리킨다.
> >   // 즉, this와 Circle은 프로토타입에 의해 연결되지 않는다.
> >   if (!(this instanceof Circle)) {
> >     // new 연산자와 함께 호출하여 생성된 인스턴스를 반환한다.
> >     return new Circle(radius);
> >   }
> >
> >   this.radius = radius;
> >   this.getDiameter = function () {
> >     return 2 * this.radius;
> >   };
> > }
> >
> > // new 연산자 없이 생성자 함수를 호출하여도 생성자 함수로서 호출된다.
> > const circle = Circle(5);
> > console.log(circle.getDiameter()); // 10
> > ```

new 연산자와 함께 생성자 함수에 의해 생성된 객체(인스턴스)는 프로토 타입에 의해 생성자 함수와 연결된다. 이를 이용해 new 연산자와 함께 호출되었는지 확인할 수 있다.

- Object와 Function 생성자 함수는 new 연산자 없이 호출해도 new 연산자와 함께 호출했을 때와 동일하게 동작
- String, Number, Boolean 생성자 함수는 new 연산자 없이 호출하면 문자열, 숫자, 불리언 값을 반환한다. 이를 통해 데이터 타입을 변환하기도 한다.

# 17장 요약<br>

## 객체 생성 방식 2가지<br>

### 생성자 함수를 사용하여 객체를 생성하는 방식<br>

### 객체 리터럴을 사용하여 객체를 생성하는 방식<br>

- 리터럴 : 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용하여 값을 생성하는 표기법
- 객체 리터럴에 의한 객체 생성 : 중괄호 { ... } 내에 0개 이상의 프로퍼티를 정의
- 생성자함수 : new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수
- 인스턴스 : 생성자 함수에 의해 생성된 객체

## 객체 리터럴 사용하여 객체 생성 장단점

장점 : 직관적, 간편<br>
단점 : 단 하나의 객체만 생성

## 생성자 함수에 의한 객체 생성의 장점

: 프로퍼티 구조가 동일한 객체 여러개를 간편하게 생성 가능

- this : this는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수

- new 연산자와 함께 호출시 해당 함수는 생성자 함수로 동작
- 생성자 함수 : 인스턴스 생성, 생성된 인스턴스 초기화<br>
  (생성자 함수는 항상 인스턴스 생성, 생성된 인스턴스를 초기화하는건 옵션)

### 생성자 함수의 인스턴스 생성 과정

1.  인스턴스 생성과 this 바인딩
2.  인스턴스 초기화 생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 this에 바인딩 되어 있는 인스턴스 초기화
3.  인스턴스 반환 생성자 내부의 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환<br>

- this가 아닌 다른 객체를 명시적으로 반환하면 this가 반환되지 못하고 return문에 명시된 객체가 반환
- 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환

결론. 생성자 내부에서 return문을 반드시 생략해야 한다.

## 내부메서드 [[call]] & [[Construct]]

함수 == 객체 : 일반 객체와 동일하게 동작<br>
함수 객체 !== 일반 객체<br>

callable = 호출할 수 있는 객체, 즉 함수<br>
constructor = 생성자 함수로서 호출할 수 있는 함수<br>
non-constructor = 객체를 생성자 함수로서 호출할 수 없는 함수<br>

호출할 수 없는 객체 !== 함수객체 이므로 함수객체는 반드시 callable이어야 한다.

모든 함수 객체는 호출할 수 있지만 모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아님.

constructor : 함수 선언문, 함수 표현식, 클래스(클래스도 함수다)

non-constructor : 메서드(ES6 메서드 축약표현), 화살표 함수

## new 연산자

일반 함수와 생성자 함수에 특별한 형식적 차이는 없다. new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작 <br>
일반 함수와 생성자 함수의 특별한 형식적 차이는 없다. 따라서 생성자 함수는 일반적으로 첫 문장을 대문자로 기술하는 파스칼 케이스로 명명하여 일반 함수와 구별할 수 있도록 노력<br>
(이름의 첫 글자가 대문자인 함수는 new를 붙여서 실행)

## new.target

- IE 미지원
- 재귀호출을 통해 생성자 함수로 호출 가능

```js
//생성자함수
function Circle(radius) {
  //이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined다.
  if (!new.target) {
    //new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
    return new Circle(radius);
  }
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter());
```

이 방법으로 객체를 만드는 경우에도 new를 생략하면 코드가 정확히 무슨일을 하는지 알기 어렵다. new 가 붙어있으면 새로운 객체를 만든다는 걸 누구나 알 수 있기 때문에 new를 생략해서 객체를 만드는 것은 자제할 것.

- 스코프 세이프 생성자 패턴 : IE에서 new.target 지원 안해서 사용 (본문참조)

- 참조하면 좋을 사이트 https://ko.javascript.info/constructor-new
