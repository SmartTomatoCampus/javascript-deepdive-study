# 31. RegExp

생성일: 2022년 3월 30일 오후 8:55

## 31. 1 정규 표현식이란?

- 패턴 매칭 기능을 제공한다.

```jsx
const tel = '010-1234-567팔';

const regExp = /^\d{3)-\d(4)-\d(4)$/;

regExp.test(tel);
```

### 31. 2 정규 표현식의 생성

- 정규 표현식 객체를 만들기 위해서는 리터럴과 RegExp 생성자 함수를 사용 할 수 있다.

![Untitled](31%20RegExp%20a9bc7/Untitled.png)

```jsx
const target = 'Is this all there is?';

const regexp = /is/i;

regexp.test(target);  // true, target 문자열에 대해 정규표현식 regExp 패턴을 검색하여 매칭
											// 결과를 boolean 값으로 반환한다.

const regExp = new RegExp(/is/i);
```

### 31. 3 RegExp 메서드

### 31. 3. 1 RegExp.prototype.exec

```jsx
const target = 'Is this all there is?';
const regExp = /is/;

regExp.exec(target); // ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

### 31. 3. 2 RegExp.prototype.test

- 인수로 전달받은 문자열에 대해 정규표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.

```jsx
const target = 'Is this all there is?';
const regExp = /is/;

regExp.test(target);  // true
```

### 31. 3. 3 String.prototype.match

- exec 메서드와 유사하지만 exec는 모든 패턴을 검색하는 g 플래그를 지정해도 첫 번째 매칭 결과만 반환한다. 하지만 match 메서드는 모든 매칭 결과를 배열로 반환한다.

```jsx
const target = 'Is this all there is?';
const regExp = /is/;

target.match(regExp);  
// ["is", index: 5, input: "Is this all there is?", groups: undefined"]
```

## 31. 4 플래그

- 플래그는 옵션이므로 선택적으로 사용 할 수 있다
- 플래그를 사용하지 않은 경우에는 대소문자를 구별해서 패턴을 검색한다.

![Untitled](31%20RegExp%20a9bc7/Untitled%201.png)

## 31. 5 패턴

- 문자열의 일정한 규칙을 표현하기 위해 사용함

### 31. 5. 1 문자열 검색

- RegExp 메서드를 사용하여 검색 대상 문자열과 정규 표현식의 매칭 결과를 구하면 검색이 수행된다.

### 31. 5. 2 임의의 문자열 검색

```jsx
const target = 'Is this all there is?';

const regExp = / ... /g;
target.match(regExp);  // ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
```

### 31. 5. 3 반복 검색

- {m, n}은 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열을 의미한다. 콤마뒤에 공백이 있으면 정상동작 하지 않는다.
- {n}은 앞선 패턴이 n번 반복되는 문자열을 의미한다.
- {n,}은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미한다.

```jsx
const target = 'A AA B BB Aa Bb AAA';

const regExp = /A{1,2}/g;
target.match(regExp);  // ['A', 'AA', 'A', 'AA', 'A']
```

### 31. 5. 4 OR 검색

|은 or의 의미를 갖는다.

```jsx
const target = 'A AA B BB Aa Bb';

const regExp = /A|B/g;

target.match(regExp);
```

### 31. 5. 5 NOT 검색

- [...] 내의 ^는 not의 의미를 갖는다.

### 31. 5. 6 시작 위치로 검색

- [...] 밖의 ^는 문자열의 시작을 의미한다.

### 31. 5. 7 마지막 위치로 검색

- $는 문자열의 마지막을 의미한다.