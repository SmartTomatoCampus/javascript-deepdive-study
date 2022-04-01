# 28장 Number

생성일: 2022년 3월 29일 오후 8:39

## Number

### 28. 1 Number 생성자 함수

- Number 객체는 생성자 함수 객체다
- new 연산자를 사용하지 않고 Number 생성자 함수를 호출하면 Number 인스턴스가 아닌 숫자를 반환한다.

```jsx
const numObj = new Number();
console.log(numberObj);

// 문자열 타입 => 숫자 타입
Number('0');  // 0
Number('-1');  // -1
```

## 28. 2 Number 프로퍼티

### 28. 2. 1 Number.EPSILON

- Number.EPSILON 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같다
- 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용한다.

```jsx
function isEqual(a, b){
	// a와 b를 뺀 값의 절대값이 Number.EPSILON보다 작으면 같은수로 인정한다.
}

isEqual(0.1 + 0.2, 0.3);  // true
```

### 28. 2. 2 Number.MAX_VALUE

- 자바스크립트에서 표현 할 수 있는 가장 큰 값이다.
- Number.MAX_VALUE보다 큰 숫자는 Infinitiy다

```jsx
Number.MAX_VALUE; // 1.7976931348623157e+308
```

### 28. 2. 3 Number.MIN_VALUE

- 자바스크립트에서 표현 할 수 있는 가장 작은 값이다.

```jsx
Number.MIN_VALUE; // 1.7976931348623157e+308
```

### 28. 2. 4 Number.MAX_SAFE_INTEGER

- 자바스크립트에서 안전하게 표현 할 수 있는 가장 큰 정수값이다.

```jsx
Number.MAX_SAFE_INTEGER; // 9007199254740991
```

### 28. 2. 5 Number.MIN_SAFE_INTEGER

- 자바스크립트에서 안전하게 표현 할 수 있는 가장 작은 정수값이다.

```jsx
Number.MAX_SAFE_INTEGER; // -9007199254740991
```

### 28. 2. 6 Number.POSITIVE_INFINITY

- Number.POSITIVE_INFINITY는 양의 무한대를 나타낸다.

```jsx
Number.POSITIVE_INFINITY
```

### 28. 2. 7 Number.NEGATIVE_INFINITY

- Number.POSITIVE_INFINITY는 음의 무한대를 나타낸다.

```jsx
Number.NEGATIVE_INFINITY
```

### 28. 2. 8 Number.NaN

- 숫자가 아님을 나타내는 숫자값이다.

```jsx
Number.NaN;
```

## 28. 3 Number 메서드

### 28. 3. 1 Number.isFinite

- 인수로 전달된 숫자값이 정상적인 유한수인지 검사하여 그 결과값을 불리언 값으로 반환한다.

```jsx
// 인수가 정상적인 유한수이면 true를 반환한다.
Number.isFinite(0);
Number.isFinite(Number.MAX_VALUE);
Number.isFinite(Number.MIN_VALUE);

// 인수가 무한수이면 false를 반환한다.
Number.isFinite(Infinity); // false
```

### 28. 3. 2 Number.isInteger

- 인수로 전달 된 값이 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.

```jsx
Number.isInteger(0)
Number.isInteger(123)
Number.isInteger(-123)

Number.isInteger(0.5)
Number.isInteger('123')
Number.isInteger(false)
Number.isInteger(Infinity)
Number.isInteger(-Infinity)

```