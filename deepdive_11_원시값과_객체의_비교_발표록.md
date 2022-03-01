# 딥다이브 11장 원시 값과 객체의 비교 발표록

### 루피님

1. 원시값을 재할당 할 경우 메모리 공간을 새로 확보하고 변경 된 메모리 주소를 가리킨다.
2. 불변성인 원시 값을 바꾸는 방법은 재할당 외에는 없기 때문에 이 점은 예기치 못한 값의 변경으로부터 자유롭다.
3. 유사배열 객체는 배열과 같이 인덱스로 프로퍼티 값에 접근 가능하고 length 프로퍼티를 갖는다. 따라서 for문을 사용 할 수 있고 문자열도 이에 해당된다.

명성님 질문: 유사배열 객체에 `map`, `filter` 등의 메서드가 사용 가능한가?

→ 루피님: 안된다

명성님 질문: 얕은복사와 깊은 복사의 차이는 무엇인가?

→ 루피님: 얕은 복사란, 참조값을 복사하는 것이고 깊은 복사는 객체 잧페를 모두 복사해서 원시값처럼 완전한 복사본을 만든다

### 명성님

1. 객체는 메모리주소를 공유함으로써 프로퍼티를 공유한다
2. 원시값의 경우에는 메모리에 실제 할당된 값이 들어가서, 변수를 바꾸면 메모리 자체가 재할당된다

### 꽁치님

```jsx
var a = 100   ->    // [100]
var b = 100   ->                          // [100]    => 가비지컬렉터가 가져간다
										// [120]
```

### 뽀또님

```jsx
const c2 = _.cloneDeep(o);
```

 lodash의 깊은 복사 방법 두 변수를 얕은 복사를 하더라도 저장된 메모리 주소는 다르지만 동일한 참조값을 가지게 된다.

얕은 복사를 해도 참조값을 같이 가질 수 있다

프로퍼티 안의 참조값은 얕은 복사를 한뒤 객체를 변경해도 같은 값을 지닌다

```jsx
var person = { name: 'Lee' }; // 참조값을 복사(얕은 복사). copy와 person은 동일한 참조값을 갖는다. 
var copy = person; // copy와 person은 동일한 객체를 참조한다. 
console.log(copy === person); // true // copy를 통해 객체를 변경한다.
[copy.name](http://copy.name/) = 'Kim'; // person을 통해 객체를 변경한다. 
person.address = 'Seoul'; // copy와 person은 동일한 객체를 가리킨다. 
// 따라서 어느 한쪽에서 객체를 변경하면 서로 영향을 주고 받는다. 
console.log(person); // {name: "Kim", address: "Seoul"} 
console.log(copy); // {name: "Kim", address: "Seoul"}
```

```jsx
var person = { name: 'Lee' }; // 참조값을 복사(얕은 복사). copy와 person은 동일한 참조값을 갖는다. var copy = person; // copy와 person은 동일한 객체를 참조한다. console.log(copy === person); // true // copy를 통해 객체를 변경한다. copy.name = 'Kim'; // person을 통해 객체를 변경한다. person.address = 'Seoul'; // copy와 person은 동일한 객체를 가리킨다. // 따라서 어느 한쪽에서 객체를 변경하면 서로 영향을 주고 받는다. console.log(person); // {name: "Kim", address: "Seoul"} console.log(copy); // {name: "Kim", address: "Seoul"}
```

### 열두님

1. 변수라는 것은 식별자이고, 식별자가 가리키는 것은 곧! 메모리 주소를 가리키는 하나의 이름이다
2. 자바스크립트에도 가비지 컬렉터가 있다.
3. 숫자 데이터보다 문자열이 지니는 용량값이 더 크다는 것을 알았다
4. 이모지가 5바이트?? 오히려 적어 오히려 좋아 많이써 많이써

### 초생님

1. 객체에 대해서 아주 심도깊게 알수 있는 시간이었다.

```jsx
var person = { name: '초생' }          [초생]

var 복제인간 = { name: '초생' }          [초생]

복제인간 === person // false
복제인간.name === person.name  // true

----------------------------------

var person1 = {
	name: '꽁치',
	family: {
		father: '꽁치대디',
		mother: '꽁치마미',
	}
}

var person2 = {
	name: '꽁치',
	family: {
		father: '꽁치대디',
		mother: '꽁치마미',
	}
}

person1 !== person2
person1.name === person2.name
person1.family !== person2.family
person1.family.father === person2.family.father
```

- 이렇게 선언하면 person 이 객체이고 메모리 공간에  `{ name: ‘초생’ }` 이 있는 줄 알았는데 그것도 따로 메모리 공간을 가지고 그 주소를 가리키고 있는 것 뿐이었다.

| 꽁치제과 | 명성제과 |
| --- | --- |
| 파란색봉투 | 노란색봉투 |
| 빼뺴로 | 빼뺴로 |
| 초코파이 | 초코파이 |
| 몽쉘 | 몽쉘 |
| 초코하임 | 초코하임 |

둘은 엄연히 다른상품(객체, 다른 참조), 하지만 각 구성(프로퍼티)은 같다