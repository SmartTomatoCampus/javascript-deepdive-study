## 39.7 어트리뷰트

### .1 어트리뷰트 노드와 attributes 프로퍼티

HTML 요소는 여러개의 어트리뷰트(속성)을 가질 수 있다. HTML 어트리뷰트는 HTML 요소의 시작 태그에 `어트리뷰트 이름="어트리뷰트 값` 형식으로 정의

### .2 HTML 어트리뷰트 조작

참조 - Element.prototype.getAttribute/setAttribute
변경 - Element.prototype.setAttribute(attributeName, attributeValue)
확인 - Element.prototype.hasAttribute(attributeName)
삭제 - Element.prototype.removeAttribute(attributeName)

### .3 HTML 어트리뷰트 vs. DOM 프로퍼티

HTML 어트리뷰트는 DOM에서 중복 관리되고 있는 것 처럼 보인다.

1. 요소 노드의 attributes 프로퍼티에서 관리하는 어트리뷰트 노드
2. HTML 어트리뷰트에 대응하는 요소 노드의 프로퍼티(DOM 프로퍼티)

HTML 어트리뷰트의 역할은 HTML 요소의 초기 상태를 지정하는 것. HTML 어트리뷰트 값은 HTML 요소의 초기상태를 의미하며 변하지 않는다.

요소노드는 상태를 가지고 있다. 요소노드는 2개의 상태, 즉 초기상태와 최신상태를 관리해야하는데 초기상태는 어트리뷰트 노드가, 최신상태는 DOM 프로퍼티가 관리.

> 어트리뷰트 노드
>
> > HTML 어트리뷰트로 지정한 HTML 요소의 초기상태는 어트리뷰트 노드에서 관리.
> > getAttribute 취득 / setAttribute 변경

> DOM 프로퍼티
>
> > 사용자가 입력한 최신 상태를 HTML 어트리뷰트에 대응하는 요소 노드의 DOM 프로퍼티가 관리. DOM 프로퍼티는 사용자의 입력에 의한 상태변화에 반응하여 언제나 최신 상태 유지.  
> > DOM 프로퍼티에 값을 할당하는 것은 HTML 요소의 최신 상태 값을 변경하는 것을 의미. 즉, 사용자가 상태를 변경하는 행위와 같다. 이 때 HTML 요소에 지정한 어트리뷰트 값에는 어떠한 영향도 주지 않는다.

> HTML 어트리뷰트와 DOM 프로퍼티의 대응 관계
>
> > 대부분의 HTML 어트리뷰트는 HTML 어트리뷰트 이름과 동일한 DOM 프로퍼티와 1:1로 대응, 단, 항상 그런것은 아니며, HTML 어트리뷰트 이름과 DOM 프로퍼티 키가 반드시 일치하는 것도 아님.

### .4 data 어트리뷰트와 dataset 프로퍼티

data 어트리뷰트와 dataset 프로퍼티를 사용해 HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트간에 데이터 교환 가능  
data 어트리뷰트는 `data-` 접두사 다음에 임의의 이름을 붙여 사용  
data 어트리뷰트 값은 HTMLElement.dataset 프로퍼티로 취득
dataset 프로퍼티는 HTML 요소의 모든 data 어트리뷰트의 정보를 제공하는 DOMStringMap 객체를 반환. ODMSTringMap 객체는 data 어트리뷰트의 data- 접두사 다음에 붙인 임의의 이름을 카멜 케이스로 변환한 프로퍼티를 가지고 있다. 이 프로퍼티로 data 어트리뷰트의 값을 취득하거나 변경 가능.

data 어트리뷰트의 data- 접두사 다음에 존재하지 않는 이름을 키로 사용하여 dataset 프로퍼티에 값을 할당하면 HTML 요소에 data 어트리뷰트가 추가된다. 이 때 dataset 프로퍼티에 추가한 카멜케이스의 프로퍼티 키는 data 어트리뷰트의 data- 접두사 다음에 케밥케이스로 자동 변경되어 추가된다.

## 39.8 스타일

### .1 인라인 스타일 조작

HTMLElement.prototype.style 프로퍼티는 요소노드의 인라인 스타일을 취득, 추가, 변경  
CSS 프로퍼티는 케밥 케이스를 따른다.  
케밤 케이스의 CSS 프로퍼티를 그대로 사용하려면 객체의 마침표 표기법 대신 대괄호 표기법을 사용한다.  
단위지정이 필요한 CSS 프로퍼티의 값은 반드시 단위를 지정해줘야함.

### .2 클래스 조작

Element.prototype.className 프로퍼티 : HTML 요소의 class 어트리뷰트 값을 취득하거나 변경  
Element.prototype.classList 프로퍼티 : class 어트리뷰트의 정보를 담은 DOMTokenList 반환

- 메서드(add(...className),remove(...className),item(index), contains(className),replace(oldClassName,newClassName), toogle(className[,force]))

### .3 요소에 적용되어 있는 CSS 스타일 참조

보통 sytle 프로퍼티로 참조. 모든 CSS 스타일 참조해야할 경우, getComputedStyle 메서드 사용  
window.getComputedStyle(element[,pseudo]) 는 첫 번째 인수로 전달한 요소 노드에 적용되어 있는 평가된 스타일을 CSSStyleDeclaration 객체에 담아 반환.

- 평가된 스타일: 최종적으로 적용된 스타일
  두 번째 인수로 의사요소를 지정하는 문자열을 전달할 수 있다. 의사요소가 아닌 일반요소인 경우 생략된다.

## 39.9 DOM 표준

W3C 와 WHATWG 두 단체가 협력하면서 만들었지만.
2018년 4월부터 구글, 애플, 마이크로소프트, 모질라로 구성된 4개 주류 브라우저 벤더사가 주도하는 WHATWG가 단일표준을 내놓기로함.

## 금일 요약

data 어트리뷰트가 사용된 코드는 그동안 많이 봐왔지만 어떻게 사용하는지 그리고 왜 사용하는지 제대로 이해하지못해 코드 해석에 난항을 겪었다. 때문에 이러한 dataset프로퍼티에 대해 배움으로써 앞으로 보다 다양한 상황에서 이를 이용할 수 있을 것 같다. 물론 공부가 더 필요하겠지만...  
 css에 관하여는 가급적 css관련 동작은 css단에서 하는게 좋다고하므로 참고적으로만 배우는 방향으로 해야겠다.

getCOmputedStyle은 최종적으로 어떤게 적용되어있는지 알고 싶을 때 사용할 수 있을 것 같다. 참고하면 좋을 것 같다.
