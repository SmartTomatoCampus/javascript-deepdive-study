# 발표문 - DOM(4)

# 뽀또님

### **어트리뷰트**

- **`어트리뷰트`** : HTML 요소의 동작 제어를 위한 추가적인 정보를 제공함
    - 어트리뷰트 이름="어트리뷰트 값" 형식으로 정의
    - 글로벌 어트리뷰트 : id, class, style, title, lang, tabindex, draggble, hidden 등
    - 이벤트 핸들러 어트리뷰트 : onclikc, onchange, onfocus, onblur, oninput, onkeypress, onkeydown, onkeyup, onmouseover, onsubmit, onload 등
    - 특정 어트리뷰트 :
        - type, value, cheked -> input 요소에서만 사용
    - HTML 요소의 어트리뷰트 -(파싱)-> 어트리뷰트 노드
    - 어트리뷰트 값은 항상 문자열 임
- **`Eleement.prototype.attributes 프로퍼티`**
    - 요소 노드의 모든 어트리뷰트 노드의 참조가 담긴 NamedNodeMap 객체 반환
    - getter만 존재하는 읽기 전용임
    - **요소 노드의 attributes 프로퍼티의 어트리뷰트 노드들이 관리됨**
- **`Element.prototype.hasAttribute(attributeString)`**
    - 요소 노드에 해당 어트리뷰트가 있는지 확인
- **`Element.prototype.removeAtrribute(attributeString)`**
    - 요소 노드에 해당 어트리뷰트 삭제
- **`어트리뷰트 노드`** : HTML 요소의 초기 상태(초기 값)의 어트리뷰트를 어트리뷰트 노드에서 관리
    - **`Element.prototype.getAttribute(attributeString)`** :
    - **`Element.prototype.setAttribute(attributeString, attributeValue)`** :
        - 요소 노드에서 메서드를 통해 직접 HTML 어트리뷰트 초기값 취득 또는 변경 가능
- **`DOM 프로퍼티`**:
    - 사용자가 입력한 HTML 어트리뷰트 최신 상태의 값을 관리(요소 노드의 프로퍼티로 각 어트리뷰트에 대한 프로퍼티가 있음)
    - setter, getter 모두 존재하는 접근자 프로퍼티 임
        - 최신 상태 값을 변경하는 것으로, 기존 HTML 지정 값은 변하지 않음
        - 사용자 입력과 관련된 어트리뷰트만 최신 상태 값을 가짐
- **`DOM 프로퍼티 vs attributes프로퍼티의 어트리뷰트 노드`**
    - **요소 노드의 초기 상태 -> 어트리뷰트 노드**
    - **요소 노드의 최신 상태 -> DOM 프로퍼티가 관리**
- **`DOM 프로퍼티와 HTML 어트리뷰트의 대응 관계`**
    - id 어트리뷰트 - id 프로퍼티 : 항상 1:1 대응, 동일 값 유지
    - input value 어트리뷰트 - value 프로퍼티 : 1:1 대응, 서로 값은 다를 수 있음
    - class 어트리뷰트 - className, classList 프로퍼티
    - td요소 colspan 어프리뷰트 대응 프로퍼티 X
    - textContent 프로퍼티 대응 어트리뷰트 X
    - 어트리뷰트 이름 대소문자 구분X / 프로퍼티 키 = 카멜케이스
    - DOM 프로퍼티 값은 문자열이 아닐수 있음

### **data 어트리뷰트와 data 프로퍼티**

- **data 어트리뷰트와 data프로퍼티를 통해서 HTML과 JS 간의 데이터 교환 가능**
- **`data 어트리뷰트`**
    - 'data-이름' 형식으로 어트리뷰트 이름을 사용함
- **`HTMLElement.dataset 프로퍼티`** : data 어트리뷰트 값 취득 가능
    - **DOMStringMap 객체 반환** (HTML 요소의 data 어트리뷰트에 따른 모든 data 프로퍼티를 담고 있음)
- **`data 프로퍼티`**
    - data 어트리뷰트에 대응하여, 카멜케이스로 만들어진 프로퍼티
        - data 어트리뷰트의 값 취득 및 변경 가능
        - data 어트리뷰트에 존재하지 않는 이름을 키로 사용하여 dataset 프로퍼티에 값 할당시 동적으로 data 어트리뷰트가 추가됨(케밥 케이스 형태로 어트리뷰트가 추가됨)

### **스타일**

- **`HTMLElement.prototype.style 프로퍼티`** : 요소 노드의 인라인 스타일을 취득, 추가, 변경
    - setter, getter 모두 존재하는 접근자 프로퍼티
    - CSSStyleDeclaration 타입 객체 반환
    - **`CSSStyleDeclaration 객체`**
        - 다양한 CSS 프로퍼티에 대응하는 프로퍼티로 구성됨
        - CSSStyleDeclaration 객체의 프로퍼티는 카멜 케이스 사용 (케밥 케이스 사용시 대괄호 사용)
        - **CSSStyleDeclaration 객체의 프로퍼티 값은 항상 `단위`를 붙여야함**
    - **`CSS 프로퍼티`** :
        - 해당 프로퍼티에 값 할당시 반영됨
        - CSS 프로퍼티는 케밥 케이스 사용

### **클래스 조작**

- class 어트리뷰트에 대응하는 DOM프로퍼티는 className, classList 프로퍼티 임
- **`Element.prototype.className 프로퍼티`** : HTML 요소의 class 어트리뷰트 값을 취득하거나 변경
    - setter, getter 모두 존재
- **`Element.prototype.classList 프로퍼티`** : class 어트리뷰트 정보를 담은 **DOMTokenList 객체 반환**
    - **`DOMTokenList`** : 유사 배열 객체이면서 이터러블 임
        - **`add(...className)`** : 인수로 전달한 1개 이상의 문자열을 class 어트리뷰트 값으로 추가
        - **`remove(...className)`** : 인수로 전달한 1개 이상의 문자열과 일치하는 class 어트리뷰트 값을 제거함 (없으면 무시됨)
        - **`item(index)`** : index에 해당하는 클래스를 class 어트리뷰트에서 반환함
        - **`contains(className)`** : 인수로 전달한 문자열과 일치하는 클래스가 class 어트리뷰트에 있는지 확인
        - **`replace(oldClassName, newClassName)`** : class 어트리뷰의 특정 클래스를 찾아 새로운 클래스 이름으로 변경함
        - **`toggle(className, 조건식)`** : class 어트리뷰트에 인수로 전달한 문자열과 일치하는 클래스 존재시 제거, 존재안하면 추가
            - 전달한 조건식의 평가가 true이면 첫번째 인수의 문자열을 추가, false이면 제거
        - forEach, entries, keys, values, supports 등 메서드 제공
- **`window.getComputedStyle(element, pseudo)`**
    - stype 프로퍼티의 값 뿐만 아니라, 클래스를 적용한 스타일, 암묵적으로 적용된 스타일을 보기 위해 사용함
        - element에 적용되어 평가된 스타일을 CSSStyleDeclaration 객체에 담아 반환(최종적으로 적용된 스타일 확인 가능)
        - pseudo: 가상요소 선택자를 넣어서 해당 가상요소에 대한 스타일을 볼수 있음
            - :after, :before

### **DOM 표준**

- 현재 WHATWG이 단일 표준을 내놓음
- DOM은 현재 4개의 버전이 있음

# 꽁치님

1. HTML 어트리뷰트와 DOM 프로퍼티라는 것이 존재한다는 것 자체를 새로 알았다.
    - HTML 어트리뷰트는 getter 만 존재하는 읽기 전용 프로퍼티로 참조만 가능
    - (getAttribute 또는 setAttribute 메서드를 사용하면 참조와 변경이 가능하다 )
    - DOM 프로퍼티는 getter 와 setter 만 존재하여 참조와 변경이 가능
2. HTML 어트리뷰트는 초기 상태를, DOM 프로퍼티는 변경된 상태(최신 상태)를 관리한다.
3. HTML 어트리뷰트는 말그대로 HTML 요소에 포함된 ‘속성'의 의미가 있기 때문에 본래 소속이 HTML 이라는 생각이 든다.
그러나 DOM 프로퍼티는 자바스크립트로 DOM 요소를 컨트롤하기 위한 DOM API 의 일원이기 때문에 자바스크립트를 위해 존재하는 것 같다. 때문에 카멜케이스를 따르고 일부 프로퍼티 값은 문자열 뿐만 아니라 다른 타입의 값이 오기도 하는 것 같다.
4. 일반적으로 사용했던 style 프로퍼티는 인라인 스타일만 가져온다는 것을 알았다. 최종적으로 평가된 모든 스타일을 가져오려면 getComputedStyle 메서드를 사용해야 한다는 것을 알았다. 
( 그런데 HTML 태그에 인라인으로 적용된 스타일이 최우선이기 때문에 스타일을 적용할 때 임시방편으론 유효하지 않을까 싶다 )

# 너두님

### 알게된점

요소 노드의 attributes 프로퍼티는 getter만 존재한다

attributes 프로퍼티는 유사배열객체이다

클래스에 클래스리스트라는게 있단걸 알게됨

### 궁금한점

리액트에서도 getAttribute를 target.value 처럼 쓸수있을까? 되는데 안쓰는거면 왜 안쓰는걸까

# 명성님

- 사용자 입력에 의한 상태 변화와 관계있는 DOM 프로퍼티만 최신 상태 값을 관리한다. 그 외는 항상 동일한 값으로 연동한다.
- Element.prototype.classList 프로퍼티는 class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환한다.
- classList가 반환하는 DOMTokenList 객체는 HTMLCollection과 NodeList와 같이 노드 객체의 상태 변화를 실시간으로 반영하는 Live 객체다.
- 라이브객체라는 것을 주의하여 사용할때에는 배열로 변환시키자!

# 초생님

1. DomTokenList 객체는 class 어트리뷰트의 정보를 나타내는 컬렉션 객체로서 유사 배열 객체이면서 이터러블이다.
2. html 요소에 적용되어 있는 모든 css 스타일을 참조해야할 경우 getComputedStyle 메서드를 사용한다.
