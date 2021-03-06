# 발표문 - 클래스(3)

# 너두님

### 알게된것

- extends 상속 받는 값을 동적으로 받을 수 있다
- 서브클래스에도 constructor를 생략해도 암묵적으로 정의되어 있음

```jsx
constructor(...args) { super(...args); }
```

- super의 사용법과 특징에 대해 알게 되었다

### 모르겠는점

- 상속 클래스의 인스턴스 생성과정 간단하게 설명해주실분?

### 꽁치님의 상속클래스 생성과정

1. 서울시 종로구로 이사온 ‘꽁치 (27)’는 새로 주민등록을 하러 종로구청에 필요한 서류를 구비해 방문했다.  
    
    `**서울시청 = 수퍼클래스 , 종로구청 = 서브클래스**`
    
2. 종로구청에서는 꽁치를 종로구민으로 등록하기 이전에 서울시민으로 등록해야 하므로, 꽁치가 들고온 서류 중 서울시민으로 등록하기 위해 필요한 서류를 간추려 서울시청으로 보냈다. `**- super 호출 과정**`
3. 서울시청에서는 종로구청에서 올려보낸 서류를 가지고 꽁치를 서울시민으로 등록해서 완성된 서류를 가지고 종로구청으로 다시 내려보냈다.
4. 서울시민으로 등록된 꽁치의 남은 서류(종로구민으로 등록하기 위한 서류)를 가지고 종로구청에서는 꽁치를 종로구민으로 등록했다.
5. 이 과정에서 처음 꽁치는 서울시청에서 먼저 서울시민으로 등록하는 과정을 거쳤다.

하지만 결과적으로 꽁치의 서울시민 등록 요청을 보낸 것은 종로구청이기 때문에 꽁치는 결과적으로 종로구민이 되었다. ( 종로구청의 인스턴스가 됐다! )
그렇게 완성된 꽁치는 이제 비로소 완전한 종로구민이 되었다!

# 꽁치님

1. `extends`키워드를 통해서 클래스는 생성자 함수와 다르게, 실질적으로 상위 클래스를 상속받아 ‘확장'하는 개념으로서 생성자함수와 차이를 갖지만, 상속을 받을 때는 생성자 함수와 클래스 모두 상속받을 수 있다.
( 추측컨대 기존의 코드와의 호환을 고려해 `ES6` 에서도 `var` 키워드를 유지시킨 자바스크립트의 정책상 기존의 코드에서 생성자 함수를 쓰던 것을 그대로 상속받아서 클래스를 도입할 수 있게 하려고 한 의도가 아닐까 싶다 )

2. 서브클래스에서 `constructor` 를 생략하면 암묵적으로 
`constructor(...args) { super(...args) }` 를 호출하여 수퍼클래스의 `constructor` 를 그대로, 수퍼클래스를 `new 연산자` 로 호출할 때 적은 인수도 그대로 같이 입력받는다는 것을 알았다.
그래서 아무런 확장 없이 고대로 `extends` 로 수퍼클래스를 상속하면 그 서브클래스는 겉껍데기는 같은 객체가 생성된다.

(하지만 엄밀히 말해서 `[[ConstructorKind]]` 내부슬롯에서 다른 값을 가질 것이다 ex. “base”, “derived” )

3. 서브클래스에서 `super` 키워드는 결국, 수퍼클래스의 기능(인스턴스 프로퍼티)를 확장해서 자신의 인스턴스 프로퍼티를 확장했지만, `new 연산자`로 서브클래스의 인스턴스를 호출할 때 부모로부터 전달받은 인스턴스 프로퍼티를 초기화하기 위해 전달되는 인수의 경우는 다시 수퍼클래스로 올려보내 처리해서 자신의 인스턴스 프로퍼티로 생성하는 과정을 가진다.

# 뽀또님

### 상속

extends - 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것이다.

상속을 통해 윗대가리를 사용할수 있다.

### super

super를 사용하면, 수퍼클래스 사용이 가능하다.

1. 서브클래스의super 호출
2. 수퍼클래스의 인스턴스 생성과 this 바인딩
3. 수퍼클래스의 인스턴스 초기화
4. 서브클래스 constructor로의 복귀와 this바인딩
5. 서브클래스의 인스턴스 초기화
6. 인스턴스 반환

### 빌트인객체

빌트인 객체 또한 extends 키워드를 사용하여 확장가능하다.

# 루피님

1. 상속에 의한 클래스 확장은 기존 클래스의 속성을 그대로 사용하면서 자신만의 고유한 속성만 추가해서 확장 할 수 있다.
2. extends 키워드는 수퍼클래스와 서브클래스간의 상속관계를 설정한다.
3. 조건에 따라 동적으로 상속받을 대상을 정할 수도 있다.(동적 상속)
4. super를 호출하면 수퍼클래스의 constructor를 호출하고 수퍼클래스의 메서드를 호출 할 수 있다.
5. String, Number, Array와 같은 표준 빌트인 객체도 extends 키워드로 확장하여 사용 할 수 있다.

# 초생님

1. 수퍼 클래스와 서브 클래스는 인스턴스의 프로토타입 체인 뿐만 아니라 클래스 간의 프로토타입 체인도 생성한다. 이를 통해 프로토타입 메서드, 정적 메서드 모두 상속이 가능하다.
2. 서브클래스의 constructor에서 super 호출하기 전까진 this를 참조할 수 없다.
3. super는 반드시 서브 클래스의 constructor에서 호출한다.
4. 서브 클래스 메서드에서 super 참조하면 부모의 프로토타입 메서드나 정적 메서드에 참조 가능
5. 상속 클래스 인스턴스 생성 과정
- 수퍼와 서브를 구분하기 위해 base (부모) derived (자식) 설정되어 구분됨
- 서브 클래스가 new 연산자와 호출되면 빈 인스턴스 생성 this 바인딩
- 서브 클래스 constructor에서 super 키워드가 호출됨
- 그러면 부모의 constructor가 호출되서 수퍼 클래스가 평가됨
- 그래서 this에 바인딩된 객체 초기화함
- super 종료되면 서브 클래스 constructor로 돌아옴
- 그러면 서브클래스는 super가 반환한 인스턴스를 this에 반환해서 그대로 사용함
⇒ 이래서 super 호출전에 this 호출하지 못하는 거임
- 그래서 서브 인스턴스 초기화 되고 반환함

# 피치세트

### 애한님의 질문

class 상속이랑 prototype 상속 차이를 말씀해주세요

- 수퍼클래스가 서브클래스한테 상속해서 서브클래스가 수퍼클래스 상속받는거고
- 프로토타입 상속은 생성자의 프로토타입을 인스턴스가 상속받는것
- 근데 인스턴스는 수퍼클래스 상속이랑 프로토타입 상속 둘다 갖다받아서 갖다쓸수있슴다

### 너두님의 확장 답변

클래스의 상속은 상속받아서 확장이 가능합니다

# 애한님

- 클래스 상속 : extends를 활용하여 상속 가능, 서브클래스에선 super를 사용하여 수퍼클래스의 생성자 호출가능.
- 의사 클래스 상속 패턴 (Pseudo-classical Inheritance) : 의사클래스 패턴은 자식 생성자 함수의 prototype 프로퍼티를 부모 생성자 함수의 인스턴스로 교체하여 상속을 구현하는 방법이다.

### 의문.

그래서 프로토타입 체인과 클래스의 상속은 비슷한거 같은데 그 두개의 차이는 무엇일까?

- 프로토타입 상속은 체인에 걸친 속성 검색으로 성능에 나쁜 영향을 줄 수 있으며 때때로 치명적일 수 있다.
- 존재하지도 않는 속성에 접근하려는 시도는 항상 모든 프로토타입 체인인 전체를 탐색해서 확인하게 만든다.

ex. 객체의 속성에 걸쳐 루프를 수행하는 경우 프로토타입 체인 전체의 모든 열거자 속성에 대하여 적용.

# 명성님

- 수퍼,서브 클래스는 인스턴스의 프로토타입 체인뿐만 아니라 클레스 간의 프로토타입 체인도 생성한다.
- 서브클래스에서 constructor를 생략하지 않은경우에는 수퍼클래스에 constructor가 없더라도 서브클래스의 constructor에서는 반드시 super를 호출해야 한다
- [[HomeObject]]를 갖는다는 것은 수퍼클래스의 메서드를 참조할 수 있다는 뜻이다. 즉 ES6의 메서드 축약 표현으로 정의된 함수만이 super의 메서드를 사용할 수 있다.
- 서브클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼 클래스에게 생성을 위임한다. 이것이 바로 서브클래스의 constructor에서 반드시 super를 호출해야 하는 이유이다.
