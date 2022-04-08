# DOM - 발표문(3)

# 꽁치님

1. 기존 자바스크립트에서도 `React.Fragment` 처럼 `DocumentFragment`노드가 존재하는 것을 알았다.
2. 노드의 삽입/수정/삭제 등은 브라우저 성능에 영향을 주기 때문에 가급적이면 최소화해야 한다.

# 명성님

- 다수의 노드를  생성,추가할 때에는 여러번 리플로우,리페인팅이 된다는 것을 알았는데 개선하기 위한 방법 중 CreateDomfragment를 알게 되었다.
- react의 fragment가 이 원리로 사용되는 것 같다
- 이 친구는 DOM에 소속되어 있지 않기에 리플로우,리페인트가 한번만 실행되게 할 수 있다.
대단한 친구군

# 애한님

### 오늘자 요약

- innerHTML의 다른 사용 방법인 insertAdjacentHTML를 알았다.
- 하지만 크로스 사이트 스크립팅 공격에 취약하므로 주의하자.
- HTML 새니티제이션(sanitization)으로 방지할 수 있다.
- 노드의 생성,추가,이동,삽입,교체,삭제 등의 방법에 대해 배웠다.
- 음... 원래 노드 생성,추가,이동,삽입등이 이렇게 복잡했던가...? 생각보다 복잡하게 동작하고 있다는걸 깨달았습니다.

# 너두님

### 알게된것

- 크로스 사이트 스크립팅 공격
- insertAdjacentHTML
- beforebegin,afterbegin,beforeend,afterend
- insertBefore
- cloneNode
- html에서도 깊은 복사 개념이 적용되는줄 몰랐었다
- replaceChild
- removeChild

### 모르겠는것

- 그냥 좀 헷갈려서 나중에 한번 써봐야겠다

# 뽀또님

### **노드 정보 취득**

- **`Node.prototype.nodeType`** : 노드 타입을 나타내는 상수 반환
    - Node.ELEMENT_NODE: 요소 노드 타입 상수 1
    - Node.TEXT_NODE: 텍스트 노드 타입 상수 3
    - Node.DOCUMENT_NODE: 문서 노드 타입 상수 9
- **`Node.prototype.nodeName`**: 노드 이름을 문자열로 반환
    - 요소노드 -> 대문자 문자열로 태그 이름 반환
    - 텍스트 노드 -> #text 반환
    - 문서노드 -> #document 반환

### **요소 노드의 텍스트 조작**

- **`Node.prototype.nodeValue 프로퍼티`** : 노드 객체의 값(텍스트 노드의 텍스트)을 반환
    - setter, getter 존재하는 접근자 프로퍼티 (할당, 참조 모두 가능)
    - 텍스트 노드가 아닌 노드에 nodeValue 프로퍼티를 참조하면 null 반환
    - 텍스트 노드의 nodeValue 프로퍼티에 값 할당시 텍스트를 변경할 수 있음
- **`Node.prototype.textContent 프로퍼티`** : 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경함
    - setter, getter 존재하는 접근자 프로퍼티 (할당, 참조 모두 가능)
    - 노드 구조를 무시하고, 해당 노드 아래로 모두 텍스트를 취함
    - 텍스트 노드가 아닌 노드에 nodeValue 프로퍼티를 참조하면 null 반환
    - **textContent에 값을 할당하면, 모든 자식 노드가 제거되고 할당한 문자열이 텍스트로 추가됨**
        - **HTML 마크업을 text로 할당해도 파싱되지 않고, 문자열로 취급됨**
    - **`innerText 프로퍼티`** : CSS에의해 visibility: hidden으로 된 요소노드의 텍스트를 반환하지 않음
        - 그리고 textContetn 보다 느림

### **DOM 조작**

- **`DOM 조작`** : 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것
    - DOM 조작시 새로운 노드 추가, 삭제 되면서 리플로우와 리페인트가 발생하므로 주의해야함
- **`Element.prototype.innerHTML 프로퍼티`** : 요소 노드의 콘텐츠 영역 내에 포함된 모든 HTML 마크업을 문자열로 반환함
    - setter, getter 존재하는 접근자 프로퍼티 (할당, 참조 모두 가능)
        - textContent 프로퍼티는 HTML 마크업을 무시하고 텍스트만 반환
        - innerHTML 프로퍼티는 HTML 마크업이 포함된 문자열을 반환
    - innerHTML 프로퍼티에 문자열 할당시 요소 노드의 모든 자식 노드가 제거되고 할당한 **문자열에 포함된 HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영**
    - 단점:
        - **`+=`** 연산자를 통해서 계속 추가한다고 해도, 기존의 있던 것도 모두 제거하고 다시 생성함 -> 비효율적
        - 삽입될 새로운 요소의 위치 지정 불가함
    - **`XXS(크로스 사이트 스크립팅 공격)`**
        - 사용자로 부터 입력받은 데이터를 그대로 innerHTML 프로퍼티에 할당하는 것은 **XSS(크로스 사이트 스크립팅 공격)**에 취약함
            - HTML5의 경우 innerHTML에 삽인된 script 요소 내의 JS 코드는 실행하지 않음
            - 하지만, 에러 이벤트를 강제로 발생시켜 자바스크립트 코드가 실행되도록 할 수 있음
    - **`HTML 새니티제이션(HTML sanitization)`** :
        - 크로스 사이트 스크립팅 공격 예방을 위해 잠재적 위험 제거 기능
        - **DOMPurify 라이브러리 사용 권장**
- **`Element.prototype.insertAdjacentHTML(position, DOMString)`**
    - 기존 요소를 제거하지 않으면서 위치를 지정해서 새로운 요소 삽입
    - position : 'beforebegin', 'afterbegin', 'beforeend', 'afterend'
        - 해당 요소 시작 태그와 종료 태그를 기준으로 함
    - DOMString : 추가할 HTML 마크업 문자열

### **노드 조작**

**생성**

- **`Document.prototype.createElement(tagName)`** : 요소 노드를 생성하여 반환
    - 요소 노드를 생성할 뿐 DOM에 추가하진 않음
- **`Document.prototype.createTextNode(text)`** : 텍스트 노드를 생성하여 반환
    - 텍스트 노드를 생성할 뿐 요소 노드의 자식 노드로 추가하진 않음

**추가**

- **`Node.prototype.appendChild(childNode)`** : 호출한 노드의 **마지막 자식 노드로** 인수로 전달한 노드를 추가함
    - 생성된 텍스트 노드를 요소노드 자식으로 추가하는 것은 textContent 프로퍼티로 하는 것이 간편함
    - 생성된 요소 노드를 DOM으로 연결된 요소의 자식으로 추가하면 DOM 트리에 추가되어 연결됨
    - 다수의 요소 노드를 DOM으로 연결시
        - 매번 생성해서 바로 DOM으로 추가하면, 그 추가하는 횟수 만큼 리플로우와 리페인트가 실행됨
        - **`Document.prototype.createDocumentFragment()`** : 비어있는 DocumentFragment 노드를 생성하여 반환함
            - **DocumentFragment 노드를 생성하여 DocumentFragment노드에 추가할 여러 노드를 구현해 놓고, DOM으로 1번만에 연결함**
                - DocumentFragment 노드는 Dom과 연결시 자신은 제거됨
- **`Node.prototype.insertBefore(newNode, childNode)`**: newNode를 childNode의 앞에 삽입함
    - childNode 인수: 두번째 인수의 노드는 반드시 insertBefore 메서드를 호출한 노드의 자식 노드이어야 함
        - childNode가 null이면 insertBefore를 호출한 노드의 마지막 자식 노드로 추가됨 (appenchild 처럼)

**이동**

- 이미 존재하는 노드를 appendChild 또는 insertBefore 사용하여 추가하면 새로운 위치로 이동함(기존의 위치에서 제거하고)

**복사**

- **`Node.prototype.cloneNode(deep)`** : 호출한 노드의 사본을 생성하여 반환
    - deep: true(노드를 모든 자손 노드 포함된 사본 생성), false or 생략(노드 자신만의 사본을 생성)

**교체**

- **`Node.prototype.replaceChild(newChild, oldChild)`** : 자신을 호출 노드의 자식 노드를 다른 노드로 교체
    - newChild : 교체할 새로운 노드
    - oldChild : 이미 존재하는 교체될 노드 (replaceChild를 호출한 노드의 자식 노드이어야 함)

**삭제**

- **`Node.prototype.removeChild(child)`** : 호출한 노드에서 전달한 child노드를 삭제함
    - child : 제거할 노드(removeChild를 호출한 노드의 자식 노드이어야 함)
