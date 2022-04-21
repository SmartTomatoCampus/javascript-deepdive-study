# DOM 조작

DOM 조작에 의해 DOM에 새로운 노드가 추가되거나 삭제되면 리플로우와 리페인트가 발생하는 원인이 되므로 성능에 영향을 준다.

## .1 innerHTML

HTML 마크업 문자열을 파싱하여 DOM 조작 가능  
 크로스 사이트 스크립팅 공격에 취약하므로 위험

> HTML 새니티제이션(sanitization)
>
> > 크로스 사이트 스크립팅 공격 예방하기 위해 잠재적 위험을 제거하는 기능.  
> > DOMPurify 라이브러리 사용을 권장(해당 함수를 직접 구현할 수도 있다.)

효율적이지 않음. (자식요소 전부 삭제후 추가해서 생성)  
 새로운 요소를 삽입할 때 삽입될 위치를 지정할 수 없음.

## .2 insertAdjacentHTML

첫 번째 인수 : beforebegin, afterbegin, beforeend, afterend  
 두 번째 인수 : 전달할 HTML 마크업 문자열

크로스 사이트 스크립팅 공격에 취약함.

## .3 노드 생성과 추가

### .3.1 요소 노드 생성

Document.prototype.createElement(tagName)

### .3.2 텍스트 노드 생성

Document.prototype.createTextNode(text)

### .3.3 텍스트노드를 요소 노드의 자식 노드로 추가

Node.prototype.appendCHild(childNode)

### .3.4 요소 노드를 DOM에 추가

Node.prototype.appendChild

## .4 복수의 노드 생성과 추가

기존 DOM에 요소 노드를 반복하여 추가하는 것은 비효율적.

컨테이너 요소를 미리 생성한 다음, 추가해야할 요소노드를 컨테이너 요소에 자식노드로 추가하고, 컨테이너 요소를 부모 요소에 자식으로 추가한다면 DOM은 한번만 변경  
 하지만 불필요한 컨테이너 요소가 DOM에 추가됨.

Document.prototype.creaateDocumentFragment 메서드를 통해 해결가능.

## .5 노드 삽입

### .5.1 마지막 노드로 추가

Node.prototype.appendChild

### .5.2 지정한 위치에 노드 삽입

Node.prototype.insertBefore(newNode, childNode)  
 첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입  
 두 번째 인수가 null이면 appendChild 처럼 동작

> DOMException ERROR
>
> > 두 번째 인수로 전달받은 노드가 insertBefore 메서드를 호출한 노드의 자식노드가 아닐 경우 발생

## .6 노드 이동

이미 존재하는 노드를 appendChild 또는 insertBefore 메서드를 사용하여 현재 위치에서 제거하고 새로운 위치에 노드를 추가

## .7 노드 복사

Node.prototype.cloneNode([deep: true | false])  
 매개변수 `deep : true` : 깊은 복사  
 매개변수 `deep : false` : 얕은 복사

## .8 노드 교체

Node.prototype.replaceChild(newChild, oldChild)  
 oldChild 노드를 newChild 노드로 변환

## .9 노드 삭제

Node.prototype.removeChild(child)  
 매개변수에 인수로 전달한 노드를 DOM에서 삭제

## 오늘자 요약

1. innerHTML의 다른 사용 방법인 insertAdjacentHTML를 알았다.
2. 하지만 크로스 사이트 스크립팅 공격에 취약하므로 주의하자.
3. HTML 새니티제이션(sanitization)으로 방지할 수 있다.
4. 노드의 생성,추가,이동,삽입,교체,삭제 등의 방법에 대해 배웠다.
5. 음... 원래 노드 생성,추가,이동,삽입등이 이렇게 복잡했던가...? 생각보다 복잡하게 동작하고 있다는걸 깨달았습니다.
