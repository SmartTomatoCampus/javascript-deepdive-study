# [JavaScript] 28장 Number

# 28.1 Number 생성자 함수

---

표준 빌트인 객체인 `Number` 객체는 생성자 함수 객체다. 따라서 `new` 연산자와 함께 호출하여 `Number 인스턴스`를 생성할 수 있다.

```jsx
const numOjb = new Number()
console.log(numObj) // Number { [[PrimitiveValue]]: 0 }
```

`Number` 생성자 함수의 인수로 숫자를 전달하면서 `new` 연산자와 함께 호출하면 `[[NumberData]]` 내부 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체를 생성한다.

```jsx
const numObj = new Number(10)
console.log(numObj) // Number { [[PrimitiveValue]]: 10 }
```

9.3절 명시적 타입 변환에서 살펴봤듯이, `new` 연산자를 사용하지 않고 `Number` 생성자 함수를 호출하면 `Number` 인스턴스가 아닌 숫자를 반환한다. 이를 이용하여 명시적으로 타입을 변환하기도 한다.

```jsx
// 문자열 타입 => 숫자 타입
Number('0')      // 0
Number('-1')     // -1
Number('10.53')  // 10.53

// 불리언 타입 => 숫자 타입
Number(true)    // 1
Number(false)   // 0
```

# 28.2 Number 프로퍼티

---

## Number.EPSILON

ES6 에서 도입된 `Number.EPSILON` 은 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같다.

대략 `2.2204460492503130808472633361816E-16` 또는 `2^-52` 의 값을 갖는다

다음 예제와 같이 부동소수점 산술 연산은 정확한 결과를 기대하기 어렵다.
정수는 2진법으로 오차 없이 저장이 가능하지만 부동소수점을 표현하기 위해 가장 널리 쓰이는 표준인 `IEEE 754` 는 2진법으로 변환했을 때 무한소수가 되어 미세한 오차가 발생할 수 밖에 없는 구조적 한계를 가진다.

```jsx
0.1 + 0.2         // 0.300000000000000004
0.1 + 0.2 === 0.3 // false
```

`Number.EPSILON` 은 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용한다.

```jsx
function isEqual(a, b) {
	// a 와 b 를 뺸 값의 절대값이 Number.EPSILON 보다 작으면 같은 수로 인정한다.
	return Math.abs(a - b) < Number.EPSILON
}

isEqual(0.1 + 0.2, 0.3) // true
```

## Number.MAX_VALUE

`Number.MAX_VALUE` 는 자바스크립트에서 가장 큰 양수값 `1.79E+308 (2^1024)` 이다.

`Number.MAX_VALUE` 보다 큰 숫자는 `Infinity` 다.

```jsx
Number.MAX_VALUE < Infinity // true
```

## Number.MIN_VALUE

`Number.MIN_VALUE` 는 자바스크립트에서 표현할 수 있는 가장 작은 양수값 `5 x 10^-324)` 이다.

`Number.MIN_VALUE` 보다 작은 숫자는 0 이다.

```jsx
Number.MIN_VALUE > 0 // true
```

## Number.MAX_SAFE_INTEGER

`Number.MAX_SAFE_INTEGER` 는 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값이다. (9007199254740992)

## Number.MIN_SAFE_INTEGER

`Number.MIN_SAFE_INTEGER` 는 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값이다. (-9007199254740992)

## Number.POSITIVE_INFINITY

`Number.POSITIVE_INFINITY` 는 양의 무한대를 나타내는 숫자값 `Infinity` 와 같다.

## Number.NEGATIVE_INFINITY

`Number.NEGATIVE_INFINITY` 는 음의 무한대를 나타내는 숫자값 `-Infinity` 와 같다.

## Number.NaN

`Number.NaN` 은 `NaN` 과 같다.

# 28.3 Number 메서드

---

[[JavaScript] Number 타입 & 메서드](https://www.notion.so/JavaScript-Number-6d8cef3b68474b18936b5b568b13cfe9) 

> 오늘 새로 알게된 것 TIL (2022.03.29 화)
> 
> 
> ---
> 
> 1. 자바스크립트에서 부동소수점 연산은 이진법 변환의 구조적 한계로 인해 오차가 발생한다는 것을 알았다. 때문에 `Number.EPSILON` 으로 확인하는 것을 알았다.
>