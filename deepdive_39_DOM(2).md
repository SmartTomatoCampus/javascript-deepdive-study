# 발표문 - DOM(2)

# 꽁치님

1. HTMLCollection 객체는 살아있는 객체로서 변경사항을 실시간으로 반영되기 때문에 안전하게 배열로 변환해서 사용하는 것이 좋다
2. 반면 NodeList 는 실시간으로 반영되지 않지만 일부 경우는 실시간으로 반영되기 때문에 배열로 변환해서 사용하는 것이 베스트라는 것을 알았다.

# 뽀또님

### ****노드 탐색****

- 취득한 요소 노드를 기점으로 DOM 트리의 노드를 옮겨 다니며 부모, 형제, 자식 노드등을 탐색하는 것
    - 노드 탐색 프로퍼티 (getter 접근자 프로퍼티로, 읽기 전용이고 값 할당 불가)
    - Node, Element 인터페이스가 트리 탐색 프로퍼티를 제공
    - Node.prototype -> parentNode, previousSibling, firstChild, childNodes 프로퍼티
    - Element.prototype -> previousElementSibling, nextElementSibling, children 프로퍼티
- **`공백 텍스트 노드`**
    - HTML 사이의 스페이스, 탭, 개행 등의 공백 문자는 텍스트 노드를 생성함
- **`자식 노드 탐색`**
    - Node.prototype.childNodes : 자식 노드를 모두 탐색하여 NodeList에 담아 반환(텍스트 노드도 포함되어 있음)
    - **Element.prototype.children** : 자식 노드 중에서 요소 노드만 탐색하여 HTMLCollection에 담아 반환(텍스트 노드 불포함)
    - Node.prototype.firstChild : 첫번째 자식 노드를 반환(텍스트 or 요소 노드임)
    - Node.prototype.lastChild : 마지막 자식 노드를 반환(텍스트 or 요소 노드임)
    - **Element.prototype.firstElementChild : 첫번째 자식 요소 노드 반환(요소 노드만 반환)**
    - **Element.prototype.lastElementChild : 마지막 자식 요소 노드 반환(요소 노드만 반환)**
- **`자식 노드 존재 확인`**
    - Node.prototype.hasChildNodes : 자식 노드가 존재하면 true, 아니면 false 반환(텍스트 노드 포함)
    - **Element.prototype.children.length 프로퍼티** (텍스트 노드 불포함)
    - **Element.prototype.childElementCount 프로퍼티** (텍스트 노드 불포함)
- **`부모 노드 탐색`**
    - Node.prototype.parentNode 프로퍼티 (부모노드는 텍스트 노드 일 수 없음)
- **`형제 노드 탐색`**
    - Node.prototype.previousSibling : 부모 노드가 같은 형제노드 중 자신의 이전 형제 노드 반환(텍스트 노드 포함)
    - Node.prototype.nextSibling : 부모 노드가 같은 형제노드 중 자신의 이후 형제 노드 반환(텍스트 노드 포함)
    - **Element.prototype.previousElementSibling** : 부모 노드가 같은 형제노드 중 자신의 이전 형제 요소 노드를 탐색 반환(텍스트 노드 X)
    - **Element.prototype.nextElementSibling** : 부모 노드가 같은 형제노드 중 자신의 이후 형제 요소 노드를 탐색 반환(텍스트 노드 X)

# 애한님

## 오늘 정리

1. NodeList에 대해 보다 이해하게되었다.
2. 스프레드 문법이나 Array.from 메서드를 사용해서 배열로 변환후 사용하는게 좋다.
3. 노드탐색에 다양한 방법들이 있다는 걸 알았는데, 생각보다 이미 써봤던 것들이 많았다.
4. 텍스트 노드에 대해서 알게되었다. (노드 탐색에 다양한 프로퍼티가 있지만 잘 쓰지 않는 이유)
5. innerText 대신 textContent를 쓰는 습관을 들여야겠다.

# 초생님

1. 공백 텍스트 노드에 대해서 처음 알았음
2. 요소 사이의 스페이스, 탭, 줄바꿈 등 공백 문자는 텍스트 노드를 생성하는데 안쓰면 가독성이 나쁘니까 그냥 쓰자
3. Node.prototype.childNodes는 Node는 DOM 컬랙션 객체인 NodeList에 담아 반환하며 NodeList에는 요소 노드 뿐만 아니라 텍스트 노드도 포함됨
4. Element.prototype.children 자식 노드 중에서 요소 노드만 탐색해서 HTMLCollection에 담아 반환
5. nodeValue는 setter getter를 사용하는 접근자 프로퍼티이며 노드 객체의 텍스트 노드의 텍스트를 반환한다.
6. textContent도 접근자 프로퍼티이고 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 취득하거나 변경한다.
7. inner 프로퍼티보다 textCotent 를 쓰자

# 너두님

### 알게된것

HTMLCollection 객체가 실시간으로 반영되는 객체였다

querySelectorAll는 단점이 많은줄 알았는데 알고보니 단점을 완벽히 보완한 객체였다

접근자 프로퍼티의 종류가 많다는걸 알게되었다 그렇지만 생각만큼 자유로워 보이지는 않는다

### 모르겠는것

getElementsByTagName or ByClassName는 왜 deprecate되지 않았나

textnode나 textcontent등의 차이는 어떨때 알고 쓰면 좋을까

# 명성님

노드를 여러개 취득할 때에는 HTMLCollection , NodeList로 나뉠 수 있으니 배열로 변환하여 사용하자.

노드를 탐색하거나, 존재를 확인하는 프로퍼티들은 모두 readonly이다.
nodeValue는 setter / getter 모두 존재하는 접근자 프로퍼티다.
