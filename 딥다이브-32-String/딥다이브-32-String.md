# [JavaScript] 32장 String

# 32.1 String 생성자 함수

---

표준 빌트인 객체인 String 객체는 생성자 함수 객체다. 따라서 `new` 연산자와 함께 호출하여 String 인스턴스를 생성할 수 있다.

```jsx
const strObj = new String()
console.log(strObj) // String {length: 0, [[PrimitiveValue]]: ""}
```

String 생성자 함수의 인수로 문자열을 전달하면서 `new` 연산자와 함께 호출하면 [[StringData]] 내부슬롯에 인수로 전달받은 문자열을 할당한 String 래퍼 객체를 생성한다.

```jsx
const strObj = new String('Lee')
console.log(strObj)
// String {0: 'L', 1: 'e', 2: 'e', length: 3, [[PrimitiveValue]]: 'Lee'}
```

11.1.2절 “문자열과 불변성"에서 보았듯이 String 래퍼객체는 배열과 마찬가지로 length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로, 각 문자를 프로퍼티 값으로 갖는 **유사 배열 객체이면서 이터러블이다.**

```jsx
console.log(strObj[0]) // L
```

단 문자열은 원시 값이므로 변경할 수 없다. 이때 에러가 발생하지 않는다.

```jsx
strObj[0] = 'S'
console.log(strObj) // 'Lee'
```

String 생성자 함수의 인수로 문자열이 아닌 값을 전달하면 인수를 문자열로 강제 변환한 후, [[StringData]] 내부 슬롯에 변환된 문자열을 할당한 String 래퍼객체를 생성한다.

```jsx
let strObj = new String(123)
console.log(strObj)
// String {0: '1', 1: '2', 2: '3', length: 3, [[Primitivevalue]]: '123'}
```

9.3절 “명시적 타입 변환"에서 살펴보았듯이 `new` 연산자를 사용하지 않고 String 생성자 함수를 호출하면 String 인스턴스가 아닌 문자열을 반환한다. 이를 이용하여 명시적 타입을 변환하기도 한다.

```jsx
String(1) // '1'
String(NaN) // 'NaN'
String(Infinity) // 'Infinity'
String(true) // 'true'
String(false) // 'false'
```

# 32.2 length 프로퍼티

---

length 프로퍼티는 문자열의 문자 개수를 반환한다.

```jsx
'Hello'.length // 5
'안녕하세요!'.length // 6
```

# 32.2 String 메서드

---

- **String.indexOf()**
    1. 대상 문자열에서 인수로 전달받은 문자열을 검색하여
    2. 첫 번째 인덱스를 반환
    3. 해당 인자로 들어온 문자가 없다면 -1을 반환
    
    ```jsx
    const str = 'Hello world';
    
    str.indexOf('l') // 2
    
    // 두번째 인자는 검색을 시작할 인덱스
    str.indexOf('l', 3) // 3
    ```
    
- **String.search()**
    1. 문자열에서 인수로 전달받은 정규표현식과 매치하는 문자열을 검색
    2. 일치하는 문자열의 인덱스를 반환
    
    ```jsx
    const str = 'Hello world';
    
    str.search(/o/) // 4
    str.search(/x/) // -1
    ```
    
- **String.includes()**
    1. 대상 문자열에 인자로 전달받은 문자열이 포함되어 있는지 true or false 를 반환
    
    ```jsx
    const str = 'Hello world'
    
    str.includes('Hello') // true
    str.includes('')      // true
    str.includes('x')     // false
    str.includes()        // false
    
    // 두번째 인자는 검색을 시작할 인덱스
    str.includes('l', 3)  // true
    str.includes('H', 3)  // false
    ```
    
- **String.startWith()**
    1. 인자로 전달받은 문자열로 시작하는지 확인, true or false 로 반환
    2. 두번째 인자는 검색을 시작할 인덱스
    
    ```jsx
    const str = 'Hello world'
    
    str.startsWith('He')  // true
    str.startsWith('x')   // false
    
    str.startsWith(' ', 5) // true
    ```
    
- **String.endsWith()**
    1. 인자로 전달받은 문자열로 끝나는지 확인, true or false 를 반환
    2. 두번째 인자로 검색할 문자열의 길이를 전달
    
    ```jsx
    const str = 'Hello world'
    
    str.endsWith('ld')  // true
    str.endsWith('x')   // false
    
    // 문자열 str의 처음부터 5자리까지('Hello')가 'lo'로 끝나는지 확인
    str.endsWith('lo', 5) // true
    ```
    
- **String.charAt()**
    1. 인자로 전달받은 인덱스에 위치한 문자를 검색하여 반환
    
    ```jsx
    const str = 'Hello'
    
    for (let i = 0; i < str.length; i++) {
    	console.log(str.charAt(i))   // H e l l o
    }
    
    // 인덱스가 문자열의 범위 (0 ~ str.length-1) 를 벗어난 경우 빈 문자열을 반환함
    str.charAt(5) // ''
    ```
    
- **String.substring()**
    1. 대상 문자열에서 첫번째 인수로 전달받은 인덱스의 문자부터, 두번째 인자로 전달받은 인덱스의 문자 바로 이전까지 문자열을 반환
    
    ```jsx
    const str = 'Hello world'
    
    // 인덱스 1부터 4 이전까지의 부분 문자열을 반환함 => (인덱스 1~3)의 문자열들
    str.substring(1, 4) // 'ell' 
    
    // 두번째 인자는 생략할 수 있음
    str.substring(1) // 'ello world'
    ```
    
- **String.slice()**
    1. slice 메서드는 substring 메서드와 동일하게 동작하지만, 음수를 전달할 수 있음
    2. 음수인 인수를 전달하면 대상 문자열의 뒤에서부터 시작하여 문자열을 잘라내어 반환함
    
    ```jsx
    const str = 'Hello world'
    
    str.substring(0, 5)   // 'Hello'
    str.slice(0, 5)       // 'Hello'
    
    str.substring(2)      // 'llo world'
    str.slice(2)          // 'llo world'
    
    str.substring(-5)     // 'Hello world' -> substring 은 인수가 0보다 작거나 NaN인 경우 0으로 취급
    str.slice(-5)         // 'world' -> 뒤에서 5자리를 잘라내어 반환
    ```
    
- **String.toUpperCase() ↔  String.toLowerCase()**
    
    대상 문자열을 모두 대문자 또는 소문자로 변경
    
    ```jsx
    const str = 'Hello world'
    
    str.toUpperCase()  // 'HELLO WORLD'
    str.toLowerCase()  // 'hello world'
    ```
    
- **String.trim()**
    1. 대상 문자열 앞뒤에 공백문자가 있을 경우 이를 제거한 문자열을 반환
    
    ```jsx
    const str = '      foo      '
    
    str.trim()  // 'foo'
    
    str.trimStart()  // 'foo     '
    str.trimEnd()    // '     foo'
    ```
    
- **String.repeat()**
    1. 대상 문자열을 인수로 전달받은 정수만큼 반복해서 새로운 문자열을 반환
    2. 인수로 전달받은 정수가 0 이면 빈 문자열을 반환, 음수이면 RangeError 발생
    3. 인수를 생략하면 기본값은 0
    
    ```jsx
    const str = 'abc'
    
    str.repeat()    // ''
    str.repeat(0)   // ''
    str.repeat(1)   // 'abc'
    str.repeat(2)   // 'abcabc'
    str.repeat(2.5) // 'abcabc' (2.5 -> 2)
    str.repeat(-1)  // RangeError: Invalid count value
    ```
    
- **String.replace()**
    1. 대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여, 
    2. 두번째 인자로 전달한 문자열로 치환한 문자열을 반환
    
    ```jsx
    const str = 'Hello world world'
    
    str.replace('world', 'Lee')   // 'Hello Lee world'
    // -> 첫번째로 검색된 문자열만 치환
    ```
    
- **String.split()**
    1. 대상 문자열에서 첫 번째 인자로 전달된 문자열 또는 정규표현식을 검색하여 문자열을 구분한 후, 분리된 각 문자열로 이루어진 배열을 반환
    2. 인수로 빈 문자열을 전달하면 각 문자들을 모두 분리하고,
    3. 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환
    
    ```jsx
    const str = 'How are you doing?'
    
    str.split(' ')     // ['How', 'are', 'you', 'doing?']
    str.split(/\s/)    // ['How', 'are', 'you', 'doing?'] -> \s는 여러가지 공백문자(스페이스, 탭 등)을 의미
    str.split('')      // ['H', 'o', 'w', ' ', 'a', 'r', 'e', ' ', 'y', 'o', 'u', ' ', 'd' 'o', 'i', 'n', 'g', '?']
    str.split()        // ['How are you doing?']
    
    // 두번째 인자로 배열의 길이를 지정할 수 있음
    str.split(' ', 3)  // ['How', 'are', 'you']
    
    // 인수로 전달받은 문자열을 역순으로 뒤집기
    function reverseString(str) {
    	return str.split('').reverse().join('')
    } 
    reverseString('Hello world')  // 'dlrow olleH'
    ```