## 39.2.6 HTMLCollection과 NodeList

HTMLCollection과 NodeList는 유사배열 객체이면서 이터러블이다. 따라서 `for...of`문으로 순회 가능, `스프레드 문법`을 사용해 배열로 변환 가능
노드 객체의 상태 변화를 실시간으로 반영하는 살아있는 객체.
HTMLCollection은 항상 live객체, NodeList는 non-live객체지만 경우에 따라 live 객체

### HTMLCollection

getElemetnsByTagName, getElementsByClassName 메서드가 반환.

살아있는 DOM 컬렉션 객체이기 때문에... for문을 역방향으로 순회하거나, while문을 사용해 무한반복하는 방법등을 통해 조작가능(실시간으로 값이 빠져나감.) -> HTMLCollection 객체를 배열로 변환하여 사용.

### NodeList

querySelectorAll 메서드 => NodeList 객체 반환.  
 NodeList.prototype.forEach 메서드(item, entries, keys, values 메서드) 사용  
 non-live 객체로 동작. childNodees 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 객체와 같이 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작.

`스프레드 문법`이나 `Array.from` 메서드를 사용해 간단히 배열로 변환하여 사용할 것.

## 39.3 노드 탐색

요소 노드를 취득한 다음 취득한 요소 노드를 기점으로 DOM 트리의 노드를 옮겨 다니며 부모, 형제, 자식 노드 등을 탐색 해야할 때가 있다.  
 노드 탐색 프로퍼티는 모두 접근자 프로퍼티다.  
 setter 없이 getter만 존재한다.

### 3.1 공백 텍스트 노드

HTML 요소 사이의 `스페이스, 탭, 줄바꿈(개행) 등의 공백 문자`는 텍스트 노드를 생성.

### 3.2 자식 노드 탐색

| 프로퍼티                            | 설명                                                                                                                                                                  |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Node.prototype.childeNodes          | 자식 노드를 모두 탐색하여 DOM 컬렉션 객체인 NodeList에 담아 반환, childNodes 프로퍼티가 반환한 NodeLIst에는 요소노드 뿐만 아니라 텍스트 노드도 포함되어 있을 수 있다. |
| Element.prototype.children          | 자식 노드 중에서 요소 노드만 모두 탐색하여 DOM 컬렉션 객체인 HTMLCollection에 담아 반환, children 프로퍼티가 반환한 HTMLCollection에는 텍스트노드가 포함되지 않음.    |
| Node.prototype.fikrstChild          | 첫 번째 자식노드를 반환. firstChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소노드다.                                                                            |
| Node.prototype.lastChild            | 마지막 자식 노드를 반환, LastChild 프로퍼티가 반환한 노드는 텍스트노드이거나 요소노드                                                                                 |
| Element.prototype.firstElementChild | 마지막 자식 요소 노드를 반환, firstElementChild 프로퍼티가 반환한 노드는 요소노드만 반환                                                                              |
| Element.prototype.lastElementChild  | 마지막 자식 요소 노드를 반환, lastElementChild 프로퍼티가 반환한 노드는 요소노드만 반환                                                                               |

### 3.3 자식 노드 존재 확인

Node.prototype.hasChildNodes 메서드 사용  
 텍스트녿느가 아닌 요소 노드가 존재하는지 확인 = children.length 메서드 or childElementCount 프로퍼티 사용

### 3.4 요소 노드의 텍스트 노드 탐색

firstChild 프로젝터로 접근 가능

### 3.5 부모 노드 탐색

Node.prototype.parentNode 프로퍼티 사용

### 3.6 형제 노드 탐색

| 프로퍼티                                 | 설명                                                                                                                                                                    |
| ---------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Node.prototype.previousSibling           | 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 노드를 탐색하여 반환. previousSibling 프로퍼티가 반환하는 형제 노드는 요소 노드 뿐만 아니라 텍스트 노드일 수도 있다. |
| Node.prototype.nextSibling               | 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색하여 반환, nextSibling 프로퍼티가 반환하는 형제 노드는 요소 노드 뿐만 아니라 텍스트 노드일 수도 있다.     |
| Element.prototype.previousElementSibling | 부모 노드가 같은 형제 요소 노드 중에서 자신의 이전 형제 요소 노드를 탐색하여 반환. previousElementSibling 프로퍼티가 반환하는 형제 노드는 요소노드만 반환한다.          |
| Element.prototype.nextElementSibling     | 부모 노드가 같은 형제 요소 노드 중에서 자신의 다음 형제 요소 노드를 탐색하여 반환, nextElementSibling 프로퍼티가 반환하는 형제 노드는 요소 노드만 반환한다.             |

## 39.4 노드 정보 취득

nodeType 프로퍼티는 DOM 노드의 '타입’을 알아내고자 할 때 쓰이는 구식 프로퍼티입니다.  
nodeName이나 tagName 프로퍼티를 사용하면 DOM 노드의 태그 이름을 알아낼 수 있습니다.

## 39.5 요소 노드의 텍스트 조작

### 5.1 nodeValue

nodeValue 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티다. 참조, 할당 모두 가능

1. 텍스트를 변경할 요소 노드를 취득한 다음 취득한 요소 노드의 텍스트 노드를 탐색. 텍스트 노드는 요소 노드의 자식 노드이므로 firstChild 프로퍼티를 사용하여 탐색
2. 탐색한 텍스트 노드의 nodeValue 프로퍼티를 사용하여 텍스트 노드의 값을 변경

### 5.2 textContent

nodeValue보다 textContent를 쓰는게 코드가 더 간단함.  
 html 마크업을 파싱하지 않음.  
 innerText 프로퍼티와 유사한 기능을 하나 innerText 프로퍼티는 사용을 권장하지 않는다.
이유.

1. css에 순종적, css에 의해 비표시로 지정된 요소 노드의 텍스트를 반환하지 않는다.
2. css를 고려해야 하므로 textContent 프로퍼티보다 느리다.

## 오늘 정리

1. NodeList에 대해 보다 이해하게되었다.
2. 스프레드 문법이나 Array.from 메서드를 사용해서 배열로 변환후 사용하는게 좋다.
3. 노드탐색에 다양한 방법들이 있다는 걸 알았는데, 생각보다 이미 써봤던 것들이 많았다.
4. 텍스트 노드에 대해서 알게되었다. (노드 탐색에 다양한 프로퍼티가 있지만 잘 쓰지 않는 이유)
5. innerText 대신 textContent를 쓰는 습관을 들여야겠다.
