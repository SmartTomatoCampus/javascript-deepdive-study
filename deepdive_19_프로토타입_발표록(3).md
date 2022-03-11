# 발표문 - 19장 프로토타입 (3)

# 명성님

- `Object.hasOwnProperty`를 올바로 사용하는 방법을 알게 되었습니다.
- `hasOwnProperty`가 프로퍼티키만 찾고 프로퍼티 값은 찾지 못한다는 것을 알게 되었습니다.
- 객체 리터럴 내부에서 `__proto__`를 사용하여 다른 객체를 직접 상속할 수 있다는 것을 알았습니다.
- 생성자 함수 내에서`this`를 사용하지 않는다면 생성자 함수가 갖는 정적 메서드/프로퍼티가 된다는 것을 알게 되었습니다
    - 정정 메서드 : 인스턴스로 접근이 불가하다.
    - 프로토타입 메서드 : 체인에 속한 객체에 접근이 가능하다.
- `in` 연산자에 대해 자세히 알게 되었습니다.
`for ...in`문으로 묶어서 하나로 사용하는 줄 알았는데,  in만을 사용하여 `boolean` 값을 받을 수 있다는 점을 알게 되었습니다.
    - `validation`을 할 때 조건식으로 참 좋을것 같습니다.
    - 둘다 프로토타입의 프로퍼티까지 찾는점이 같으나 for in은 `enumerable`한 프로퍼티만을 찾습니다!
- `Object keys`, `values`, `entries` 메서드에 대해 알게 되었습니다.
이제 배열 안의 객체가 나와도 겁먹지 않고 해체할 수 있게 되었습니다.

# 꽁치님

- `instanceof` 연산자는 우변이 자신의 생성자 함수인지가 아니라, 우변이 자신의 프로토타입 체인 상에 존재하는지를 찾는다.
- 프로토타입 체인에 속한 프로퍼티나 메서드는 아래것들이 `참조`/`호출`할 수 있지만, 생성자 함수가 직접 소유한 프로퍼티나 메서드는 인스턴스가 직접 `참조`/`호출`할 수 없다!
- `in` 연산자는 프로토타입을 포함해 객채 내에 특정 프로퍼티가 존재하는지, `Object.prototype.hasOwnProperty` 메서드는 해당 객체에만 특정 프로퍼티가 존재하는지 확인한다.
- 오직 객체 자신이 소유한 열거 가능한 프로퍼티를 열거하려면, `Object.keys`, `Object.values`, `Object.entries` 를 쓰는게 좋다

# 애한님

### instanceof

### 직접 상속

- 직접상속에는 `Object.create`에 의한 직접상속과 객체 리터럴 내부에서 `__proto__`에 의한 직접상속이 있습니다

### 프로퍼티를 확인하는 법

- 프로퍼티 존재 확인하는 방법에는 `in` 연산자와 `hasOwnProperty` 메서드가 있는데, 그중  `in` 연산자는 상속받은 모든 프로토타입의 프로퍼티를 확인하지만, `hasOwnProperty`는 상속받은 프로토타입의 프로퍼티는 무시합니다.

Toss : 오늘 행복한 3시간이었다고 하신 `**명성님**`께서 추가로 설명해주시면 좋을것 같읍니다

# 뽀또님

```jsx
console.log( 객체 instanceof 생성자함수 )
```

`instanceof` 로 `prototype`에 바인딩된 객체가 프로토타입 체인 상에 존재하면 `boolean`으로 반환한다.

```jsx
key in object
```

프로퍼티를 `in` 연산자로 객체내에 프로퍼티가 존재하는지 여부를 확인할 수 있다.

```jsx
for ( 변수선언문 in 객체 ) { ... }
for ( const key in person ) {
	console.log(key + ': ' + person[key])
}
```

프로퍼티를 순회하며 열거할 수 있다.

`Object.keys` 열거 가능한 프로퍼티 값을 배열로 반환가능하다.

# 루피님

- `instatnceof` 연산자를 사용하면 좌변에 객체를 가리키는 식별자가 우변의 생성자 함수의 `prototype` 체인상에 존재하면 `ture`, 아닐경우 `false`를 반환한다
- 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조하거나 호출 할 수 없다.
- `in` 연산자는 대상 객체가 상속받는 모든 프로토타입의 프로퍼티를 확인한다
- `for...in` 문은 프로퍼티키가 심벌인 프로퍼티는 열거하지 않는다

```jsx
// __proto__ 사용
var obj = {};
var descriptor = Object.create(null); // 상속받은 속성 없음
descriptor.value = 'static';

// 기본 값은 열거 불가, 설정 불가, 쓰기 불가
Object.defineProperty(obj, 'key', descriptor);

// 기본 값 명시하기
Object.defineProperty(obj, 'key', {
  enumerable: false,
  configurable: false,
  writable: false,
  value: 'static'
});

// 같은 객체를 재활용하기
function withValue(value) {
  var d = withValue.d || (
    withValue.d = {
      enumerable: false,
      writable: false,
      configurable: false,
      value: null
    }
  );

  // 중복 할당 방지
  if (d.value !== value) d.value = value;

  return d;
}

Object.defineProperty(obj, 'key', withValue('static'));

// 객체 동결을 사용할 수 있다면 프로토타입의 변형을 방지하기
// (value, get, set, enumerable, writable, configurable)
(Object.freeze || Object)(Object.prototype);
```

# 정리

- 내부 슬롯과 내부 메소드는 자바스크립트의 내부 동작방식을 설명하는 의사코드이다.
- 프로퍼티의 종류는 데이터 프로퍼티가 있고, 접근자 프로퍼티가 있다.
- 데이터 프로퍼티의 어트리뷰트(속성값)는 객체의 해당 프로퍼티에 대한 삭제, 추가, 변경등의 가능 여부에 대해 정의한다.
- 객체에 새로운 프로퍼티가 추가될 때마다 자바스크립트 엔진은 내부적으로 프로퍼티 어트리뷰트를 자동으로 정의한다.
- 접근자 프로퍼티는 접근자 함수로 구성된 프로퍼티로, 다른 데이터 프로퍼티의 값을 읽거나 변경할때 쓰인다.
