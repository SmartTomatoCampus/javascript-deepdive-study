# 발표문 - DOM(1)

# 너두님

### 알게된것

HTML 요소의 구조 이름

상속에대한 타입과 프로토타입 관점

# 애한님

### 오늘 느낀점.

다양한 노드 취득법.. css 선택자 사용법 등에 대해서 다시금 머리에 새기게 되는 유익한 시간이었다고 생각합니다.

노드리스트... 배열도 아닌데 배열처럼 생긴 유사배열 객체이면서 이터러블....
이거때문에 어제~오늘 이틀간 코테문제풀면서 시간 날려먹었는데.. 하필 그 개념에 대해 공부하기 바로 1절 앞에서 진도가 멈춰서.. 킹받음...
내일 빡집중하겠습니다.

# 꽁치님

1. 생각하지 않고 써왔던 DOM API 들에 대해서 객체 구조와 프로토타입 체인상에서 프로퍼티와 메서드들을 사용할 수 있다는 것을 알았다.
2. querySelectorAll 또는 getElementsByClassName 처럼 여러 요소 노드를 취득하는 메서드는 HTMLCollection 이라는 유사 배열 객체이면서 이터러블인 객체를 반환한다는 것을 오늘 처음 알았다.

# 루피님

1. 노드 객체들로 구성된 트리 자료구조를 DOM이라 한다.
2. 브라우저 환경의 모든 자바스크립트 코드는 script 태그에 의해 분리되어 있어도 하나의 전역 객체 window를 공유한다.
3. HTML 문서의 모든 요소 노드를 취득하려면 getElementsByTagName 메서드의 인수로 `‘’`를 전달한다.
4. CSS 선택자는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법이다.

어트리뷰트 선택자: input 요소 중에 type 어틜뷰트 값이 'text'인 요소를 모두 선택

```jsx
input[type=text] { ... }
```

후손 선택자: div 요소의 후손 요소 중 p 요소를 모두 선택

```jsx
div p { ... }
```

자식 선택자 : div 요소의 자식 요소 중 p 요소를 모두 선택

```jsx
div > p { ... }
```

# 초생님

1. 자바스크립트는 생각보다 세분화되서 움직인다.
2. DOM은 html 문서의 구조와 정보를 표현하며 이를 조작하는 API를 제공하는 트리 자료구조
3. html 요소는 요소 노드 객체로 변환되는데 어트리뷰트는 어트리뷰트 노드로 텍스트콘텐츠는 텍스트 노드로 변환된다. 처음 알았음;
4. 요소 노드 객체는 프로토타입 체인을 통해 프로퍼티나 메서드를 상속받는데 공통된 기능은 체인 상위에 고유의 기능은 체인 하위에 있는 구조이다.
5. 요소 노드는 id, class, tag, css 선택자, matches를 통해 취득할 수 있다.
6. id는 getElementById로 취득하지만 이외의 것은 querySelector, querySelectorAll을 사용하는 것이 더 좋다.

# 명성님

### HTML 문서를 구성하는 개별적인 요소로 구분

- <div는 시작 태그이다. (start tag)
- class는 어트리뷰트 이름이다. (attribute name)
- greeting은 어트리뷰트 값이다. (attribute value)
- Hello는 콘텐츠이다. (contents)
- </div>는 종료 태그이다. (end tag

### Node로 구분

- div는 요소 노드이다. (element node)
- class="greeting"은 어트리뷰트 노드이다. (attribute node)
- Hello는 텍스트 노드이다. (text node)

# 뽀또님

### DOM

- **`DOM(Document Object Model)`**:
    - HTML 문서의 계층적 구조와 정보를 표현
    - HTML 요소를 제어할 수 있는 API로 프로퍼티와 메서드를 제공하는 노드 객체들로 구성된 트리 자료 구조
- **`HTML 요소`**: HTML 문서를 구성하는 개별적인 요소
    - HTML 요소 -(JS엔진에 의한 파싱)-> 요소 노드 객체
    - HTML 어트리뷰트 -(파싱)-> 어트리뷰트 노드
    - HTML 요소의 콘텐츠 -(파싱)-> 텍스트 노드
        - 시작태그
        - 어트리뷰트 이름
        - 어트리뷰트 값
        - 콘텐츠
        - 종료태그
    - HTML 요소의 중첩 관계 특성으로 다른 요소로 포함 가능함 -> 모든 노드들이 트리 자료 구조로 구성
- **`트리 자료구조`**: 부모노드, 자식노드로 구성되어 노드들의 계층 구조(비선형 자료구조)로 이뤄짐
    - 비선형 자료구조: 하나의 자료 뒤에 여러개의 자료가 존재할 수 있는 자료구조
    - 루트노드: 최상위 노드(부모 노드가 없음)
    - 리프노드: 자식 노드가 없는 노드

### 12개의 노드 타입

- Node.ELEMENT_NODE - 1
- Node.ATTRIBUTE_NODE - 2
- Node.TEXT_NODE - 3
- Node.CDATA_SECTION_NODE - 4
- Node.ENTITY_REFERENCE_NODE - 5
- Node.ENTITY_NODE - 6
- Node.PROCESSING_INSTRUCTION_NODE - 7
- Node.COMMENT_NODE - 8
- Node.DOCUMENT_NODE - 9
- Node.DOCUMENT_TYPE_NODE - 10
- Node.DOCUMENT_FRAGMENT_NODE - 11
- Node.NOTATION_NODE - 12

### 노드 객체의 상속 구조

- DOM을 구성하는 노드 객체는 DOM API를 통해 자신의 부모, 형제, 자식 탐색 가능, 자신의 어트리뷰트와 텍스트를 조작 가능
- DOM을 구성하는 노드 객체는 호스트 객체이자, JS 객체이므로 **프로토타입에 의한 상속 구조**를 갖음
- 개발자 도구의 Elements의 Properties 패널에서 상속 구조 확인 가능
- 모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속 받음
    - Document, HTMLDocument 인터페이스 - (상속) -> 문서 노드
    - Attr 인터페이스 - (상속) -> 어트리뷰트 노드
    - CharacterData 인터페이스 - (상속) -> 텍스트 노드
    - Element, 세분화된 HTMLhtmlElement, HTMLHeadElement, HTMLBodyElement, HTMLUListElement 등의 인터페이스 - (상속) -> 요소 노드
- 상속 구조
    - Object (객체)
    - EventTarget (이벤트를 발생시키는 객체)
    - Node (트리 자료구조의 노드 객체)
    - Element (브라우저가 렌더링할 수 있는 웹 문서의 요소(HTML,XML,SVG)를 표현하는 객체)
    - HTMLElement (웹 문서의 요소 중에서 HTML요소를 표현하는 객체)
    - HTML특정Element (HTML 요소중에서 특정 요소를 표현하는 객체)
    - 특정 요소 노드 객체

### **HTMLCollection, NodeList**

- DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬랙션 객체
- 둘다, 유사 배열 객체이면서 이터러블로 **for...of, 스프레드 문법, Array.from 사용하여 간단히 배열로 변환 가능**
- HTMLCollection은 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 객체임
    - NodeList는 아님
- **`HTMLCollection`**
    - getElementsByTagName, getElementsByClassName 메서드가 반환하는 객체
    - 노드 객체의 상태 변화를 실시간으로 반영하는 살아있는 DOM 컬렉션 객체임
        - 순회 사용시 순회 과정에서 처리하면 바로 상태를 변경시키기 때문에 주의 해야함(담아온 객체에 바로 반영어 순회중에도 객체가 변함)
        - **HTMLCollection을 처리 할때는 배열에 풀어 배열 형태로 만들어 배열 고차 함수로 순회할 것을 추천**
- **`NodeList`**
    - querySellectorAll 메서드가 반환하는 객체로 실시간으로 노드 객체의 상태 변경을 하지 않는 객체임
    - 순회시 NodeList.prototype.forEach 메서드 상속받아 사용 가능
    - **단, childNodes 프로퍼티가 반환하는 NodeList 객체는 실시간으로 노드 객체 상태 변경 반영함**
        - 배열에 풀어 배열 형태로 만들어 배열 고차 함수로 순회할 것을 추천
