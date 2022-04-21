## DOM

##### DOM은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조다. DOM에 대해 자세히 살펴보자.

### NODE

#### HTML 요소와 노드 객체
<span style="font-size:16px"> HTML element는 HTML문서를 구성하는 개별적인 요소를 의미한다.</span>
```html
<div class="greeting">Hello</div>
```
HTML 문서를 구성하는 개별적인 요소로 구분 
`<div`는 시작 태그이다. (start tag)
`class`는 어트리뷰트 이름이다. (attribute name)
`greeting`은 어트리뷰트 값이다. (attribute value)
`Hello`는 콘텐츠이다. (contents)
`</div>`는 종료 태그이다. (end tag)

##### HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다. 이때 HTML 요소의 어트리뷰트는 어트리뷰트 노드로, HTML요소의 텍스트 콘텐츠는 텍스트 노드로 변환된다.

<span style="font-size:16px">Node로 구분
`div`는 요소 노드이다. (element node)
`class="greeting"`은 어트리뷰트 노드이다. (attribute node)
`Hello`는 텍스트 노드이다. (text node)</span>

 
<span style="font-size:16px">HTML 문서는 HTML 요소들의 집합으로 이뤄지며, HTML 요소는 중첩 관계를 갖는다. 즉 HTML 요소의 콘텐츠 영역(시작 태그와 종료 태그 사이)에는 텍스트 뿐만 아니라 다른 HTML 요소도 포함될 수 있다.
이때 HTML 요소 간에는 중첩 관계에 의해 계층적인 부자관계가 형성된다. 이러한 `HTML 요소 간의 부자 관계를 반영하여 HTML 문서의 구성 요소인 HTML 요소를 객체화한 모든 노드 객체들을 트리 자료구조`로 구성한다.</span>

#### 트리 자료구조
<span style="font-size:14px"> 트리 자료구조는 노드들의 계층 구조로 이뤄진다. 즉 트리 자료구조는 부모 노드와 자식 노드로 구성되어 노드 간의 계층적 구조를 표현하는 비선형 자료구조를 말한다. 트리 자료구조는 하나의 최상위 노드에서 시작한다. 최상위 노드는 부모 노드가 없으며, 루트 노드라 한다. 루트 노드는 0개 이상의 자식 노드를 갖는다. 자식 노드가 없는 노드를 리프 노드라 한다.</span>
<span style="font-size:14px; font-weight:bold; color:gold"> `노드 객체들로 구성된 트리 자료구조를 DOM`이라 한다. 노드 객체의 트리로 구조화되어 있기 때문에 `DOM을 DOM 트리라고 부르기도 한다.`</span>
  ><span style="font-size:16px;font-weight:bold">  비선형 자료구조</span>
  <span style="font-size:14px"> 비선형 자료구조는 하나의 자료 뒤에 여러 개의 자료가 존재할 수 있는 자료구조다. 비선형 자료구조에는 트리와 그래프가 있다.</span>
    -
    <span style="font-size:16px;font-weight:bold">선형 자료구조</span>
    <span style="font-size:14px">하나의 자료 뒤에는 하나의 자료만 존재하는 자료구조다. 선형 자료구조에는 배열,스택,큐,링크드 리스트, 해시 테이블이 있다.</span>

<br/>

##### DOM은 노드 객체의 계층적인 구조로 구성된다.
<span style="font-size:16px"> 노드 객체는 종류가 있고, 상속 구조를 갖는다. 노드 객체에는 총 12개의 종류(노드 타입)가 있다. 이중에서 중요한 노드 타입은 4가지다.</span>
  - <span style="font-size:16px"> 문서 노드</span>
  - <span style="font-size:16px"> 요소 노드</span>
  - <span style="font-size:16px"> 어트리뷰트 노드</span>
  - <span style="font-size:16px"> 텍스트 노드 </span>
  
#### 문서 노드 document node
<span style="font-size:14px"> 문서 노드는 DOM 트리의 최상위에 존재하는 루트노드로서 document 객체를 가리킨다. `document 객체는 브라우저가 랜더링한 HTML 문서 전체를 가리키는 객체`로서, 전역 객체 window의 document 프로퍼티에 바인딩되어 있다. 따라서 문서 노드는 window.document 또는 document로 참조할 수 있다.
  브라우저 환경의 모든 자바스크립트 코드는 script 태그에 의해 분리되어 있어도 하나의 전역 객체 window를 공유한다. `HTML 문서당 document 객체는 유일하기 때문`이다. document 객체인 문서 노드는 DOM 트리의 루트 노드이므로 DOM 트리의 노드들에 접근하기 위한 진입점 역할을 담당한다. 즉 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야한다.</span>
  
  #### 요소 노드 element node
<span style="font-size:14px"> 요소 노드는 HTML 요소를 가리키는 객체다. 요소 노드는 HTML 요소 간의 중첩에 의해 부자 관계를 가지며, 이 부자 관계를 통해 정보를 구조화한다. 따라서 `요소 노드는 문서의 구조를 표현`한다고 할 수 있다.</span>
#### 어트리뷰트 노드 attribute node
<span style="font-size:14px">어트리뷰트 노드는 HTML 요소의 어트리뷰트를 가리키는 객체다. 어트리뷰트 노드는 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결되어 있다. 단 요소노드는 부모 노드와 연결되어 있지만 `어트리뷰트 노드는 부모 노드와 연결되어 있지 않고 요소 노드에만 연결되어 있다.` `즉 어트리뷰트 노드는 부모 노드가 없으므로 요소 노드의 형제 노드는 아니다.` 따라서 어트리뷰트 노드에 접근하여 어트리뷰트를 참조하거나 변경하려면 먼저 요소 노드에 접근해야 한다.</span>
#### 텍스트 노드 text node
<span style="font-size:14px">텍스트 노드는 HTML 요소의 텍스트를 가리키는 객체다.  `요소 노드가 문서의 구조`를 표현한다면 `텍스트 노드는 문서의 정보를 표현`한다고 할 수 있다. 텍스트 노드는 요소 노드의 자식 노드이며, 자식 노드를 가질 수 없는 리프 노드다. 즉 텍스트 노드는 DOM 트리의 최종단이다. 따라서 텍스트 노드에 접근하려면 부모 노드인 요소 노드에 접근해야 한다.</span>

### 노드 객체의 상속 구조

<span style="font-size:14px"> DOM은 HTML 문서의 계층적 구조와 정보를 표현하며, 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조라고 했다. 즉 DOM을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 DOM API를 사용할 수 있다. 이를 통해 노드 객체는 자신의 부모,형제,자식을 탐색할 수 있으며, 자신의 어트리뷰트와 텍스트를 조작할 수도 있다.
  DOM을 구성하는 노드 객체는 ECMAScript 사양에 정의된 표준 빌트인 객체가 아니라 브라우저 환경에서 추가적으로 제공하는 호스트 객체다. 하지만 노드 객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 갖는다.
  모든 노드 객체는 Object,EventTarget,Node 인터페이스를 상속 받는다. 추가적으로 문서 노드는 Document, HTMLDocument 인터페이스를 상속 받고, 어트리뷰트 노드는 Attr, 텍스트 노드는 CharacterData 인터페이스를 각각 상속받는다.
  요소 노드는 Element 인터페이스를 상속받는다. 또한 요소 노드는 추가적으로 HTMLElement와 태그의 종류별로 세분화된 HTMLHtmlElement, HTMLHeadElement,HTMLBodyElement,HTMLULListElement 등의 인터페이스를 상속받는다.이를 프로토타입 관점에서 살펴보면 예를 들어, input 요소를 파싱하여 객체화 한 input 요소 노드 객체는 HTMLInputElement,HTMLElement,Element,Node,EventTarget,Object의 prototype에 바인딩되어 있는 프로토타입 객체를 상속받는다. 즉 요소 노드 객체는 프로토타입 체인에 있는 모든 프로토타입의 프로퍼티나 메서드를 상속받아 사용할 수 있다.
  배열이 객체인 동시에 배열인 것처럼 input 요소 노드 객체도 다양한 특성을 갖는 객체이며 이러한 특성을 나타내는 기능들을 상속을 통해 제공받는다.
  <table>
    <tr>
      <td>input 요소 노드 객체의 특성 </td><td>프로토타입을 제공하는 객체</td>
    </tr>
    <tr>
      <td> 객체 </td><td>Object</td>
    </tr>
    <tr>
      <td>이벤트를 발생시키는 객체</td><td>EventTarget</td>
    </tr>
    <tr>
      <td>트리 자료구조의 노드 객체</td><td>Node</td>
    </tr>
    <tr>
      <td>브라우저가 렌더링 할 수 있는 웹 문서의 요소를 표현하는 객체</td><td>Element</td>
    </tr>
    <tr>
      <td>웹 문서의 요소 중에서 HTML 요소룰 표현하는 객체</td><td>HTMLElement</td>
    </tr>
    <tr>
      <td>HTML요소 중에서 input 요소를 표현하는 객체</td><td>HTMLInputElement</td>
    </tr>
  </table>
  </span>
  
  <br/>

  ___
  
  
  
<span style=font-size:14px>노드 객체에는 노드의 종류, 즉 노드 타입에 상관없이 모든 노드 객체가 공통으로 갖는 기능도 있고, 노드 타입에 따라 고유한 기능도 있다. 예를 들어, `모든 노드 객체는 공통적으로 이벤트를 발생시킬 수 있다.` 이벤트에 관련된 기능은 EventTarget 인터페이스가 제공한다.</span>
> EventTarget 객체가 제공하는 이벤트에 관련된 기능
- <span style=font-size:16px> EventTarget.addEventListener
- EventTarget.removeEventListener
- EventTarget.dispatchEvent</span>

<span style=font-size:14px> 또한 모든 노드 객체는 트리 자료구조의 노드로서 공통적으로 트리 탐색 기능이나 노드 정보 제공 기능이 필요하다. 이 같은 노드 관련 기능은 Node 인터페이스가 제공한다.</span>
> Node 객체가 제공하는 Node 탐색 / 정보에 관련된 기능 
- <span style=font-size:16px> Node.parentNode
- Node.childNodes
- Node.previousSibling
- Node.nextSibling
- Node.nodeType
- Node.nodeName</span>

  
<span style=font-size:14px> HTML 요소가 객체화 된 `요소 노드 객체`는 HTML 요소가 갖는 공통적인 기능이 있다. 예를 들어 input 요소 노드 객체와 div 요소 노드 객체는 모두 HTML 요소의 스타일을 나타내는 style 프로퍼티가 있다. 이처럼 `HTML 요소가 갖는 공통적인 기능은 HTMLElement 인터페이스가 제공한다.`</span>

<span style=font-size:14px> 하지만 `요소 노드 객체는 HTML 요소의 종류에 따라 고유한 기능`도 있다. 예를 들어, input 요소 노드 객체는 value 프로퍼티가 필요하지만 div 요소 노드 객체는 value 프로퍼티가 필요하지 않다. 따라서 `필요한 기능을 제공하는 인터페이스가 HTML 요소의 종류에 따라 각각 다르다.`</span>



<span style=font-size:14px> 이처럼 노드 객체는 공통된 기능일수록 프로토타입 체인의 상위에, 개별적인 고유 기능일수록 프로토타입 체인의 하위에 프로토타입 체인을 구축하여 노드 객체에 필요한 기능, 즉 프로퍼티와 메서드를 제공하는 상속 구조를 갖는다.</span>

<span style=font-size:16px> 지금까지 살펴본 바와 같이 DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 **`노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공한다.`** 이 DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.
  DOM API를 사용하기 위해 노드 객체의 상속 구조를 자세히 알아야 할 필요는 없다. 중요한 것은 DOM API, 즉 `DOM이 제공하는 프로퍼티와 메서드를 사용하여 노드에 접근하고 HTML의 구조나 내용 또는 스타일 등을 동적으로 변경하는 방법을 익히는 것`이다. **`프론트엔드 개발자에게 HTML은 단순히 태그와 어트리뷰트를 선언적으로 배치하여 뷰를 구성하는 것 이상의 의미를 갖는다. 즉 HTML을 DOM과 연관지어 바라보아야 한다. `**</span>

### 요소 노드 취득
<span style=font-size:14px> HTML의 구조나 내용 또는 스타일 등을 동적으로 조작하려면 먼저 요소 노드를 취득해야 한다. 텍스트 노드는 요소 노드의 자식 노드이고, 어트리뷰트 노드는 요소 노드와 연결되어 있기 때문에 텍스트 노드나 어트리뷰트 노드를 조작하고자 할 때도 마찬가지다.
  예를 들어 HTML 문서 내의 h1 요소의 텍스트를 변경하고 싶은 경우를 생각해보자. 이 경우 먼저 DOM 트리 내에 존재하는 h1 요소 노드를 취득할 필요가 있다. 그리고 취득한 요소 노드의 자식 노드인 텍스트 노드를 변경하면 해당 h1 요소의 텍스트가 변경된다. 이처럼 요소 노드의 취득은 HTML 요소를 조작하는 시작점이다. 이를 위해 DOM은 요소 노드를 취득할 수 있는 다양한 메서드를 제공한다.</span>

#### id를 이용한 요소 노드 취득
<span style=font-size:14px> Document.prototype.getElementById 메서드는 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환한다. `getElementById 메서드는 Document.prototype의 프로퍼티다.`
  따라서 반드시 문서 노드인 document를 통해 호출해야 한다.
  HTML 요소에 id 어트리뷰트를 부여하면 id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수 효과가 있다. 단 id 값과 동일한 이름의 전역 변수가 이미 선언되어 있으면 이 전역 변수에 노드 객체가 재할당되지 않는다. </span>
#### 태그 이름을 이용한 요소 노드 취득

<span style=font-size:14px> Document.prototype / Element.prototype.getelementsByTagName 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환한다.
  메서드 이름에 포함된 Elements가 복수형인 것에서 알 수 있듯이, `getElementsByTagName 메서드는 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환한다.`
함수는 하나의 값만 반환할 수 있으므로 여러 개의 값을 반환하려면 배열이나 객체와 같은 자료구조에 담아 반환해야 한다. getElementsByTagName 메서드가 반환하는 DOM 컬렉션 객체인 HTMLCollection 객체는 유사 배열 객체이면서 이터러블이다.</span>
<span style=font-size:14px>getElementsByTagName 메서드는 Document.prototype에 정의된 메서드와 Element.prototype에 정의된 메서드가 있다. Document.prototype.getElementsByTagName 메서드는 Dom의 루트 노드인 문서 노드, 즉 document를 통해 호출하며 DOM 전체에서 요소 노드를 탐색하여 반환한다. 하지만 Element.prototype.getelementsByTagName 메서드는 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다.</span>

#### class를 이용한 요소 노드 취득
<span style=font-size:14px> getElementsByTagName 메서드와 마찬가지로 Document.prototype과 Element.prototype에 정의된 메서드가 있다.
</span>
#### CSS 선택자를 이용한 노드 취득
<span style=font-size:14px> CSS 선택자는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법이다. Document.prototype / Element.prototype querytSelector 메서드는 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환한다.
querySelector,querySelectorAll 메서드또한 Document.prototype,Element.prototpye에 정의된 메서드가 있다. document를 통해 문서 전체를 탐색하며 Element를 통해 하위 요소의 특정 요소를 탐색하여 반환한다.</span>

#### 특정 요소 노드를 취득할 수 있는지 확인하는 방법
<span style=font-size:14px> Element.prototype.matches 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득 할 수 있는지 확인한다. Element.prototype.matches 메서드는 이벤트 위임을 사용할 때 유용하다.</span>

### HTMLCollection과 NodeList
<span style=font-size:14px> DOM 컬렉션 객체인 `HTMLCollection과 NodeList는 DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체`다. HTMLCollection과 NodeList는 모두 `유사 배열 객체`이면서 `이터러블`이다. 따라서 for ... of 문으로 순회할 수 있으며 스프레드 문법을 사용하여 간단히 배열로 변환할 수 있다.</span>
<span style=font-size:14px> HTMLCollection과 NodeList의 중요한 특징은 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 객체라는 것이다. `HTMLCollection은 언제나 live 객체로 동작`한다. 단, `NodeList`는 대부분의 경우 노드 객체의 상태 변화를 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 `non-live 객체로 동작`하지만 경우에 따라 live 객체로 동작할 때가 있다.</span>

#### HTMLCollection
<span style=font-size:14px> getElementsByTagName, getElementsByClassName 메서드가 반환하는 HTMLCollection 객체는 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 DOM 컬렉션 객체다. 따라서 HTMLCollection 객체를 살아 있는 객체라고 부르기도 한다.</span>

```html
<!-- ... --!>
<ul>
<li class="red">Apple</li>
<li class="red">Apple</li>
<li class="red">Apple</li>
</ul>
<!-- ... --!>
```

```js
// class 값이 'red'인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환한다.
const $elems = document.getElementsByClassName('red');
// 이 시점에 HTMLCollection 객체에는 3개의 요소 노드가 담겨 있다.
console.log($elems); // HTMLCollection(3) { li.red, li.red, li,red }
// HTMLCollection 객체의 모든 요소의 class 값을 'blue'로 변경한다.
for(let i = 0; i < $elems.length; i++){
  $elems[i].className = 'blue';
}

// HTMLCollection 객체의 요소가 3개에서 1개로 변경된다.
console.log($elems); // HTMLCollection(1) {li.red}

```

<span style=font-size:14px> getElementsByClassName 메서드로 class 값이 'red'인 요소 노드를 모두 취득하고, 취득된 요소 노드를 담고 있는 HTMLCollection 객체를 for문으로 순회하며 className 프로퍼티를 사용하여 모든 요소의 class 값을 'red' 에서 'blue'로 변경한다.
  따라서 위 예제가 에러 없이 실행 되면 모든 li 요소의 class 값이 'blue'로 변경되어 모든 li 요소는 CSS에 의해 파란색으로 렌더링 될 것이다. 하지만 위 예제를 실행해 보면 예상대로 동작하지 않는다. 다음 그림처럼 두 번째 li 요소만 class 값이 변경되지 않는다.</span>
  
1. 첫번째 반복
<span style=font-size:14px> 첫번째 반복을 실행하며 className 프로퍼티에 의해 $elems[0]은 class의 값이 'red' 에서 'blue'로 변경된다. 이때 첫 번째 li 요소는 class 값이 'red'에서 'blue'로 변경되었으므로 getelementsByClasName 메서드의 인자로 전달한 'red'와 더는 일치하지 않기 때문에 $elems에서 실시간으로 제거된다. 이처럼 HTMLCollection 객체는 실시간으로 노드 객체의 상태 변경을 반영하는 살아 있는 DOM 컬렉션 객체다.</span>
  2. 두번째 반복
  <span style=font-size:14px> 첫 번째 반복에서 첫 번째 li 요소는 $elems에서 실시간으로 제거되었다. 따라서 $elems[1]은 이제 세 번째 li요소이다. 이 세번째 li 요소의 class 값도 'blue'로 변경되고 마찬가지로 HTMLCollection 객체인 $elems에서 실시간으로 제외된다.</span>
  3. 세번째 반복
  <span style=font-size:14px> i의 값이 2가 되었고 $elems에는 2번째 li 요소 노드만 남게 되었다. 이제 i는 $elems.length보다 크므로 false로 평가되어 반복이 종료된다. 따라서 2번째 li 요소의 class 값은 변경되지 않는다.</span>

이처럼 HTMLCollection 객체는 실시간으로 노드 객체의 상태 변경을 반영하여 요소를 제거할 수 있기 때문에 HTMLCollection 객체를 for문으로 순회하며 노드 객체의 상태를 변경해야 할 때 주의해야 한다. 이 문제는 for문을 역방향으로 순회하는 방법으로 회피하거나 while문을 사용하여 노드 객체가 남아있지 않을 때까지 무한 반복하는 방법으로 회피할 수도 있다.
```js
let i = 0;
while($elems.length > i){
  $elems[i].className = 'blue';
}
```
더 간단한 해결책은 부작용을 발생시키는 원인인 HTMLCollection 객체를 사용하지 않는 것이다. 유사 배열객체이면서 이터러블인 HTMLCollection 객체를 배열로 변환하면 부작용을 발생시키는 HTMLCollection 객체를 사용할 필요가 없고 유용한 배열의 고차 함수를 사용할 수 있다.
```js
[...$elems].forEach(elem => elem.className = 'blue');
```

### Nodelist
<span style=font-size:14px> HTMLCollection 객체의 부작용을 해결하기 위해 getElementsByTagName, getelementsByClassName 메서드 대신 querySelectorAll 메서드를 사용하는 방법도 있다. `querySelectorAll 메서드는 DOM 컬랙션 객체인 NodeList 객체를 반환한다.` 이때 NodeList 객체는 실시간으로 노드 객체의 상태 변경을 반영하지 않는 객체다.</span>
```js
// querySelectorAll은 DOM 컬렉션 객체인 NodeList를 반환한다.
const $elems = document.querySelectorAll('.red');
  
// NodeList 객체는 NodeList.prototype.forEach 메서드를 상속받아 사용할 수 있다.
$elems.forEach(elem => elem.classNae = 'blue');
```
querySelectorAll이 반환하는 NodeList 객체는 NodeList.prototype.forEach 메서드를 상속받아 사용할 수 있다.
NodeList.prototype.forEach 메서드는 Array.prototype.forEach 메서드와 사용 방법이 동일하다.
NodeList.prototype은 forEach 외에도 item, entries, keys, values 메서드를 제공한다.
NodeList 객체는 대부분의 경우 노드 객체의 상태 변경을 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 non-live 객체로 동작한다. 하지만 childNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 객체와 같이 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작하므로 주의가 필요하다.

** HTMLCollection 이나 NodeList 객체는 예상과 다르게 동작할 때가 있어 다루기 까다롭고 실수하기 쉽다. 따라서 노드 객체의 상태 변경과 상관 없이 안전하게 DOM 컬렉션을 사용하려면 `HTMLCollection이나 NodeList 객체를 배열로 변환하여 사용하는 것을 권장`한다. HTMLCollection과 NodeList 객체가 메서드를 제공하기는 하지만 배열의 고차 함수만큼 다양한 기능을 제공하지 않는다.**
**`HTMLCollection과 NodeList 객체 모두 유사 배열 객체이면서 이터러블`이기에 `스프레드 문법`이나 Array.from 메서드를 사용하여 간단한 배열로 변환할 수 있다.**
### 노드 탐색
<span style=font-size:14px> 요소 노드를 취득한 다음, 취득한 요소 노드를 기점으로 DOM 트리의 노드를 옮겨 다니며 부모,형제,자식 노드 등을 탐색해야 할 때가 있다. 이때 DOM 트리 상의 노드를 탐색할 수 있도록 Node.Element 인터페이스는 트리 탐색 프로퍼티를 제공한다.
  **`parentNode`,`previusSibling`,`firstChild`,`childNodes` 프로퍼티는 `Node.prototype`이 제공하고 프로퍼티 키에 `Element`가 포함된 `previousElementSibling`, `nextElementsibling`과 `children` 프로퍼티는 `Element.prototype`이 제공된다. 노드 탐색 프로퍼티는 모두 접근자 프로퍼티다. 단 노드 탐색 프로퍼티는 setter 없이 getter만 존재하여 참조만 가능한 읽기 전용 접근자 프로퍼티다. 읽기 전용 접근자 프로퍼티에 값을 할당하면 아무런 에러 없이 무시된다.**</span>
### 요소 노드의 텍스트 조작
<span style=font-size:14px> nodeValue 프로퍼티는 탐색,접근했던 프로퍼티들과는 다르게 setter,getter가 모두 존재하는 접근자 프로퍼티다. 따라서 nodeValue 프로퍼티는 참조와 할당 모두 가능하다. 노드 객체의 nodeValue 프로퍼티를 참조하면 노드 객체의 값을 반환한다. 노드 객체의 값이란 텍스트 노드의 텍스트다. 따라서 `텍스트 노드가 아닌 노드, 즉 문서 노드나 요소 노드의 nodeValue 프로퍼티를 참조하면 null을 반환한다.`</span>
```html
<div id="foo">Hello!!</div>
```
```js
// 문서 노드의 nodeValue 프로퍼티를 참조한다. 
console.log(document.nodeValue); // null

// 요소 노드의 nodeValue 프로퍼티를 참조한다.
const $foo = document.getElementById('foo');
console.log($foo.nodeValue); // null 

// 텍스트 노드의 nodeValue 프로퍼티를 참조한다.
const $textNode = $foo.firstChild;
console.log($textNode.nodeValue); // Hello
```

이처럼 텍스트 노드의 nodeValue 프로퍼티를 참조할 때만 텍스트 노드의 값, 즉 텍스트를 반환한다. 텍스트 노드가 아닌 노드 객체의 nodeValue 프로퍼티를 참조하면 null을 반환하므로 의미가 없다.


### DOM 조작

 DOM 조작은 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것을 말한다. 이때 리플로우와 리페인트가 발생되므로 DOM 조작은 성능 최적화를 위해 주의해서 다루어야 한다.

#### innerHTML

Element.prototype.innerHTML 프로퍼티는 setter와 getter 모두 존재하는 프로퍼티로서 HTML 마크업을 취득하거나 변경한다. 요소 노드의 innerHTML 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역(시작 태그와 종료 태그 사이) 내에 포함된 모든 HTML 마크업을 문자열로 반환한다.
앞서 살펴본 textContent 프로퍼티를 참조하면 HTML 마크업을 무시하고 텍스트만 반환하지만 innerHTML프로퍼티는 HTML 마크업이 포함된 문자열을 그대로 반환한다.요소 노드의 innerHTML 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열에 포함되어 있는 HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영된다.
```html
<!--...-->

<ul id="fruits">
<li class="apple">Apple</li>
</ul>
<!--...-->
```
```js
const #fruits = document.getElementById('fruits');

// 노드 추가 
$fruits.innerHTML += '<li class="banana">Banana</li>';

//노드 교체
$fruits.innerHTML = '<li class="orange">Orange</li>';

//노드 삭제
$fruits.innerHTML = '';
```
 innerHTML은 크로스 사이트 스크립팅 공격에 약하기에 DOMPurify와 같은 세니티제이션 라이브러리르 사용하여 잠재적 위험을 제거해야 한다.
```
DOMPurify.sanitize('<img src="x" onerror="alert(document.cookie)">');
//  => <img src="x">
```
또한 innerHTML은 항상 모든 자식 요소를 삭제하고 처음부터 다시 반영하기에 효율적이지 않으며 새로운 요소를 삽입할 때 삽입 위치를 지정할 수 없다는 단점도 있다.

#### insertAdjacentHTML 메서드
`Element.prototype.insertAdjacentHTML(position,DOMString)` 메서드는 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다.
  innerAdjacentHTML 메서드는 두 번째 인수로 전달한 HTML 마크업 문자열(DOMString)을 파싱하고 그 결과로 생성된 노드를 첫 번째 인수로 전달한 위치(position)에 삽입하여 DOM에 반영한다. 첫 번째 인수로 전달할 수 있는 문자열은 `'beforebegin'`,`'afterbegin'`,`'beforeend'`,`'afterend'` 4가지다.
  
```html
<body>
<!-- beforebegin -->
  <div id="foo">
    <!-- afterbegin -->
    text
    <!-- beforeend -->
  </div>
  <!-- afterend -->
</body>
```
```js
const $foo = document.getByElementId('foo');

$foo.insertAdjacentHTML('beforebegin','<p>beforebegin</p>');
$foo.insertAdjacentHTML('afterbegin','<p>afterbegin</p>');
$foo.insertAdjacentHTML('beforeend','<p>beforeend</p>');
$foo.insertAdjacentHTML('afterend','<p>afterend</p>');
```
 기존 요소에 영향을 주지 않고 새롭게 삽입될 요소만을 파싱하기에 innerHTML보다 효율적이고 빠르지만 크로스사이트스크립팅 공격에 취약하다는 점은 동일하다.

#### 노드 생성과 추가
 지금까지 살펴본 innerHTML과 insertAdjacentHTML 메서드는 HTML 마크업 문자열을 파싱하여 노드를 생성하고 DOM에 반영한다. DOM은 노드를 직접 생성/삽입/삭제/치환하는 메서드도 제공한다.

##### 노드 생성
`document.prototype.createElement(tagname)`

 텍스트 노드 또한 생성할 수 있다.
`document.prototype.createTextNode(text)`
 텍스트 노드 생성시 요소 노드에 추가하는 처리가 별도로 필요하다.
 `tag.appendChild(textnode)`

`textContent`를 사용할수도 있지만 innerHTML과 같이 모든 자식 노드가 제거되어 할당된 문자열이 텍스트로 추가되므로 주의해야 한다.

#### 복수의 노드 생성과 추가

리플로우,리페인트를 최대한 방지하기 위해 DocumentFragment 노드를 통해 복수의 노드를 생성한다. DocumentFragment 노드는 문서,요소,어트리뷰트,텍스트 노드와 같은 노드 객체의 일종으로 부모 노드가 없어서 기존 DOM과는 별도로 존재한다는 특징이 있다. DocumentFragment 노드는 자식 노드들의 부모 노드로서 별도의 서브DOM을 구성하여 기존 DOM에 추가하기 위한 용도로 사용한다.
DocumentFragment 노드는 기존 DOM과는 별도로 존재하므로 DocumentFragment 노드에 자식 노드를 추가하여도 기존 DOM에는 어떠한 변경도 발생하지 않는다. 또한 `DocumentFragment 노드를 DOM에 추가하면 자신은 제거되고 자신의 자식 노드만 DOM에 추가`된다.
```html
<!-- ... -->
<ul id="fruits"></ul>
<!-- ... -->
```
```js
const $fruits = document.getElementById('fruits');

const $fragment = document.createDocumentFragment();

['Apple','Banana','Orange'].forEach(text => {
  const $li = document createElement('li');
  
  const textNode = document.createTextNode(text);
  
  $li.appendChild(textNode);
  
  // li를 묶어주기 위해 DocumentFragment에 추가한다.
  $fragment.appendChild($li);
  
  // DocumentFragment 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
  $fruits.appendChild($fragmant);
});
```
위와 같이 DocumentFragment를 사용하면 리플로우와 리페인트가 여러번 발생하지 않고 한 번만 실행된다. 따라서 여러 개의 요소 노드를 DOM에 추가하는 경우 DocumentFragment 노드를 사용하는 것이 효율적이다. 

___
#### 지정한 위치에 노드 사입
Node.prototype.insertBefore(newNode,childNode) 메서드는 첫 번째 인수로 전달 받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입한다.
두 번째 인수로 전달받은 노드는 insertBefore 메서드를 호출한 노드의 자식노드여야 한다.
#### 노드 이동
DOM에 이미 존재하는 노드를 appendChild 또는 insertBefore 메서드를 사용하여 기존 위치에서 노드를 제거하고 새로운 위치에 노드를 추가하는 방식으로  노드를 이동시킬 수 있다.

#### 노드 복사
Node.prototype.cloneNode([deep: true| false]) 메서드는 노드의 사본을 생성하여 반환한다. 매개변수 deep에 true로 인수를 전달하면 노드를 깊은 복사하여 모든 자손 노드가 포함된 사본을 생성하고, false 를 인수로 전달하거나 생략하면 노드를 얕은 복사 하여 노드 자시만의 사본을 생성한다. 얕은 복사로 생성된 요소노드는 자손노드를 복사하지 않으므로 텍스트 노드도 없다.


___  

### 어트리뷰트

#### 어트리뷰트 노드와 attributes 프로퍼티

요소 노드는 여러 개의 어트리뷰트 노드를 가질 수 있다. 글로벌 어트리뷰트와 이벤트 핸들러 어트리뷰트는 모든 HTML 요소에서 공통으로 사용할 수 있지만, 특정 HTML 요소에만 한정적으로 사용 가능한 어트리뷰트도 있다.
id,class,style 어트리뷰트는 모든 HTML 요소에 사용할 수 있지만, type,value,checked 어트리뷰트는 input 요소에만 사용할 수 있다.
HTML 문서가 파싱될 때 HTML 요소의 어트리뷰트는 어트리뷰트 노드로 변환된어 요소 노드와 연결된다. 이때 HTML 어트리뷰트당 하나의 어트리뷰트 노드가 생성된다. 즉 위 input 요소는 3개의 어트리뷰트가 있으므로 3개의 어트리뷰트 노드가 생성된다. 이때 모든 어트리뷰트 노드의 참조는 유사 배열 객체이자 이터러블인 NameNodeMap 객체에 담겨서 요소 노드의 attributes 프로퍼티에 저장된다.
따라서 요소 노드의 모든 어트리뷰트 노드는 요소 노드의 Element.prototype.attributes 프로퍼티로 취득할 수 있다. attributes 프로퍼티는 읽기 전용 접근자 프로퍼티이며 요소 노드의 모든 어트리뷰트 노드의 참조가 담긴 NameNodeMap 객체를 반환한다.

> ##### 요소 노드는 여러 개의 어트리뷰트 노드를 갖는다. 어트리뷰트는  HTML 문서가 파싱 될때 어트리뷰트 노드로 변환되어 요소 노드와 연걸된다. 모든 요소 노드는  NameNodeMap 객체에 담겨 요소 노드의 attributes 프로퍼티에 저장된다. Element.prototype.attributes 프로퍼티로는 값에 대한 접근만 가능하다.

#### HTML 어트리뷰트를 조작하려면?
`Element.prototype.attributes` 가 아닌 `Element.prototype.getAttribute/setAttribute` 를 사용한다.
해당 메서드를 사용하면 attributes 프로퍼티를 통하지 않고 요소 노드에서 직접 HTML 어트리뷰트 값을 취득하거나 변경할 수 있어서 편리하다.
이 외에도 어트리뷰트 값의 존재를 확인하기 위한 `.hasAttribute`와 어트리뷰트를 삭제하기 위한 `.removeAttribute`가 있다.

#### HTML 어트리뷰트 VS DOM 프로퍼티

요소 노드 객체(DOM)에는 HTML 어트리뷰트에 대응하는 프로퍼티가 조냊한다. 이 요소 노드 객체의 프로퍼티들은 HTML 어트리뷰트 값을 초기값으로 가지고 있다.
예를 들어 `<input id="user" type="text" value="ungmo2">` 요소가 파싱되어 생성된 요소 노드 객체에는 id,type,value 어트리뷰트에 대응하는 id,type,value 프로퍼티가 존재하며, `DOM 프로퍼티들은 HTML 어트리뷰트의 값을 초기값으로 갖고 있다`.

DOM 프로퍼티는 getter,setter가 모두 존재하는 접근자 프로퍼티다. 따라서 DOM 프로퍼티는 참조와 변경이 가능하다.
```js
const $input = document.getElementById('user');

$input.value = 'foo';

console.log($input.value) // foo
```

1. 요소 노드의 attributes 프로퍼티에서 관리하는 어트리뷰트 노드
2. HTML 어트리뷰트에 대응하는 DOM 프로퍼티
HTML 어트리뷰트는 DOM 프로퍼티와 같이 중복해서 관리되고 있는 것처럼 보이지만 중복 관리의 개념이 아니다.

HTML 어트리뷰트의 역할은 초기 상태를 지정하는 것이다. 즉 `HTML 어트리뷰트 값은 HTML 요소의 초기 상태를 의미하며 이는 변하지 않는다`.

위의 input 요소의 value 어트리뷰트는 input 요소가 렌더링 될 때 필드에 표시할 초기값을 지정한다. 즉 input 요소가 렌더링되면 입력 필드에 초기값으로 지정한 value 어트리뷰트 값 "ungmo2"가 표시된다.

이때 input 요소의 value 어트리뷰트는 어트리뷰트 노드로 변환되어 요소 노드의 attributes 프로퍼티에 저장된다. 이와는 별도로 value 어트리뷰트의 값은 요소 노드의 value 프로퍼티에 할당된다. 따라서 input 요소의 요소 노드가 생성되어 첫 렌더링이 끝난 시점까지 어트리뷰트 노드의 어트리뷰트 값과 요소 노드의 value 프로퍼티에 할당된 값은 HTML 어트리뷰트 값과 동일하다.

> 첫 렌더링이 끝난 시점까지의 어트리뷰트 노드의 어트리뷰트 값과 요소 노드의 value 프로퍼티에 할당된 값은 같다.

하지만 첫 렌더링 이후 사용자가 input 요소에 무언가를 입력하면 상황이 달라진다.

**요소 노드는 상태를 가지고 있다**
예를 들어, input 요소 노드는 사용자가 입력 필드에 입력한 값을 상태로 가지고 있으며, checkbox 요소 노드는 사용자가 입력 필드에 입력한 체크 여부를 상태로 가지고 있다.
`사용자가 input 요소의 입력 필드에 값을 입력한 경우 input 요소 노드는 입력한 값을 상태로 갖게 되는 변화하면서 살아있는 것이 된다. `

사용자의 입력에 의해 변경된 최신 상태를 관리하며, HTML 어트리뷰트로 지정한 초기 상태 또한 관리해야하낟. 초기 상태 값을 관리하지 않으면 웹페이지를 처음 표시하거나 새로고침할 때 초기상태를 표시할 수 없기 떄문이다.
이처럼 `요소 노드는 2개의 상태, 즉 초기 상태와 최신 상태를 관리해야하며 요소 노드의 초기 상태는 어트리뷰트 노드가 관리하며, 요소 노드의 최신 상태는 DOM 프로퍼티가 관리한다.`

#### 어트리뷰트 노드

HTML 어트리뷰트로 지정한 HTML 요소의 초기 상태는 어트리뷰트 노드에서 관리한다. 어트리뷰트 값은 입력에 의해 상태가 변경되어도 변하지 않고 HTML 어트리뷰트로 지정한 (초기 렌더링까지의 값) HTML 요소의 초기 상태를 그대로 유지한다.
어트리뷰트 노드가 관리하는 초기 상태 값을 취득,변경하려면 getAttribute,setAttribute 메서드를 사용한다. getAttribute 메서드로 취득한 값은 어트리뷰트 노드에서 관리하는 HTML 요소에 지정한 어트리뷰트 값, 즉 초기 상태 값이며 setAttribute를 통해 어트리뷰트 노드에서 관리하는 HTML 요소에 지정한 어트리뷰트 값, 즉 초기 상태를 변경한다.



#### DOM 프로퍼티

사용자가 입력한 최신 상태는 HTML 어트리뷰트에 대응하는 요소 노드의 DOM 프로퍼티가 관리한다.
DOM 프로퍼티는 사용자의 입력에 의한 상태 변화에 반응하여 언제나 최신 상태를 유지한다.

DOM 프로퍼티로 취득한 값은 HTML 요소의 최신 상태 값을 의미한다. 이 최신 상태 값은 사용자의 입력에 의해 언제든지 동적으로 변경되어 최신 상태를 유지한다.
이에 반해 getAttribute 메서드로 취득한 HTML 어트리뷰트 값, 즉 초기 상태 값은 변하지 않고 유지된다.

> 이벤트나 필드 입력으로 value가 변경되면 DOM 프로퍼티가 변경되지만 HTML 어트리뷰트는 변경되지 않는다.

물노 모든 DOM 프로퍼티가 사용자에 입력에 의해 변경되어 최신 상태를 관리하는 것은 아니다. id 어트리뷰트에 대응하는 id 프로퍼티는 사용자의 입력과 아무 관계가 없다.
따라서 `사용자에 입력에 의한 상태 변화`와 관계 없는 id 어트리뷰트와 id프로퍼티는 사용자 입력과 관계 없이 항상 동일한 값을 유지한다. 즉 id 어트리뷰트 값이 변하면 id 프로퍼티 값도 변한다.

> 사용자 입력에 의한 상태 변화와 관계있는 DOM 프로퍼티만 최신 상태 값을 관리한다. 그 외는 항상 동일한 값으로 연동한다.


#### HTML 어트리뷰트와 DOM 프로퍼티의 대응 관계
대부분의 HTML 어트리뷰트는 HTML 어트리뷰트 이름과 동일한 DOM 프로퍼티와 1:1로 대응하지만, 언제나 1:1로 대응하는 것은 아니며, HTML 어트리뷰트 이름과 DOM 프로퍼티 키가 반드시 일치하는 것도 아니다.

- class 어트리뷰트는 className, classList 프로퍼티와 대응한다.
- for 어트리뷰트는 htmlFor 프로퍼티와 1:1 대응한다.
- textContent 프로퍼티는 대응하는 어트리뷰트가 존재하지 않는다.
- etc...

#### DOM 프로퍼티 값의 타입
getAttribute 메서드로 취득한 어트리뷰트 값은 언제나 문자열이지만 DOM 프로퍼티로 취득한 최신 상태 값은 당연히 문자열이 아닐 수도 있다. 예를 들어, checkbox 요소의 checked 어트리뷰트 값은 문자열이지만, checked 프로퍼티 값은 불리언타입이다.

`<input type=checkbox" checked />`
```js
const $checkbox = document.querySelector('input[type=checkbox]');

console.log($checkbox.getAttribute('checked')); // ''
console.log($checkbox.checked); // true
```

#### data 어트리뷰트와 dataset 프로퍼티
data 어트리뷰트와 dataset 프로퍼티를 사용하면 HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터를 교환할 수 있다.
data 어트리뷰트는 data-user-id, data-role과 같이 data- 접두사 다음에 임의의 이름을 붙여 사용한다.

	<li data-user-id="7621" data-role="admin">LEE</li>
```js
const users = [...document.querySelector('.users').children];

const user = users.find(user => user.dataset.userId === '7621');

console.log(user.dataset.role); // "admin"

user.dataset.role = 'subs';
console.log(user.dataset); // DOMStringMap { userId: "7621", role: "subs" }

```
data 어트리뷰트 값은 HTMLElement.dataset 프로퍼티로 취득할 수 있다. dataset 프로퍼티는 HTML 요소의 모든 data 어트리뷰트의 정보를 제공하는 DOMStringMap 객체를 반환한다. DOMStringMap 객체는 data 어트리뷰트의 data- 접두사 다음에 붙인 임의의 이름을 카멜 케이스로변환한 프로퍼티를 가지고있다. 이 프로퍼티로 어트리뷰트의 값을 취득하거나 변경할 수 있다.

### 스타일

#### 인라인 스타일 조작

HTMLElement.prototype.style 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 인라인 스타일을 취득하거나 추가 또는 변경한다.
`$div.style.width = '100px'`
`$div.style.backgroundColor='yellow'`

CSS 프로퍼티 background-color 에 대응하는 CSSStyleDeclaration 객체의 프로퍼티는 backgroundColor다.


### 클래스 조작

class 어트리뷰트에 대응하는 DOM 프로퍼티는 className과 classList다. class는 자바스크립트의 예약어이기 때문에 다른 이름을 사용한다. 

#### className
getter,setter 모두 존재하는 접근자 프로퍼티로서 class 어트리뷰트 값을 취득하거나 변경한다.
요소 노드의 className 프로퍼티를 참조하면 class 어트리뷰트 값을 문자열로 반환하고 요소 노드의 className 프로펕에 문자열을 할당하면 class 어트리뷰트 값을 할당한 문자열로 반환한다.
```js
const $box = document.querySelector('.box');

console.log($box.className); // 'box red'
// red 부분만 blue로 변경.
$box.className = $box.className.replace('red','blue');
```

className 프로퍼티는 문자열을 반환하므로 공백으로 구분된 여러 개의 클래스를 반환하는 경우 다루기가 불편하다.

### classList

Element.prototype.classList 프로퍼티는 class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환한다.
classList가 반환하는 DOMTokenList 객체는 HTMLCollection과 NodeList와 같이 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 객체다.

DOMTokenList 객체는 class어트리뷰트의 정보를 나타내는 컬랙션 객체이며 유사배열객체이며 이터러블이다.
DOMTokenList 객체는 다음과 같은 유용한 메서드들을 제공한다.
```js
// add. 1개 이상의 문자열을 class 어트리뷰트 값으로 추가
$box.classList.add('fruits','apple','red');

// remove. 1개 이상의 문자열과 일치하는 클래스를 class 어트리뷰트에서 삭제
$box.classList.remove('apple','red');

// item. item 메서드를 통해 index에 맞는 클래스를 반환한다.
$box.classList.item(0); // 'fruits'

// contains. 문자열과 일치하는 class 어트리뷰트를 찾는다
$box.classList.contains('apple') // false

// replace. 첫번째 인수로 전달한 문자열을 두번째 인수로 변경한다.
$box.classList.replace('fruits','veggies');

// toggle. 인수로 전달한 문자열과 일치하는 클래스를 확인하여 제거하거나 추가한다.
$box.classList.toggle('radish'); // class="veggies radish"
```
이 밖에도 DOMTokenList는 forEach,entries,keys,values,supports 메서드르 제공한다.

#### 요소에 적용되어 있는 CSS 스타일 참조

```js

// .box 요소에 적용된 모든 CSS 스타일을 담고 있는 CSSStyleDeclaration 객체를 취득
const computedStyle = window.getComputedStyle($box);


console.log(computedStyle);//CSSStyleDeclaration
console.log(computedStyle.width) // 100px
console.log(computedStyle.border) // 1px solid rgb(0,0,0)
```
