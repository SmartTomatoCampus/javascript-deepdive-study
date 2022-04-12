# 발표문 - 이벤트(1)

# 너두님

### 알게된것

프레임 워크마다 onclick 이벤트 할당방식이 다 달랐다

수많은 이벤트 핸들러 메서드가 많다는걸 알게되엇다

이벤트 핸들러를 제거를 해야하니까 제거가 있는거라고 생각함

### 모르겠는것

버블링이란게 정확하게 뭘까

단순한 소개가 많아서 나중에 잘 쓸수있을지 모르겠음

# 꽁치님

1. HTML 에서 onclick 등으로 이벤트를 거는게 이벤트 핸들러 어트리뷰트 방식이라는 것을 알았다. 
2. 이벤트 핸들러 어트리뷰트는 '함수 호출문'을 담는 게 리액트와 달라서 흥미로웠다. 하지만 onclick="sayHi('Lee')" 와 같은 방식이 사실은 function onclick 으로 시작하는 함수의 몸통부분이라는 것도 처음 알았고 신기했다.

# 애한님

### 오늘 요약

1. 다양한 이벤트 핸들링 에 대해 복습.
2. 값 변경 이벤트 활용해보면 좋을 것 같다.
3. 이벤트 핸들러의 참조를 변수나 자료구조에 저장하는 습관을 들이자.
4. addEventListener - 세 번재 매개변수에 대해 처음 알았다. true / false 차이
-> [https://stackoverflow.com/questions/17564323/what-does-the-third-parameter-false-indicate-in-document-addeventlistenerdev](https://stackoverflow.com/questions/17564323/what-does-the-third-parameter-false-indicate-in-document-addeventlistenerdev) 참조해주세요.
    - 버블링 : 아래에서 위로 올라감. / 캡처링 : 위에서 아래로 내려감.

Event capturing is useful in event delegation when bubbling is not supported. For example:( 이벤트 캡처는 버블링이 지원되지 않을 때 이벤트 위임에 유용.)
// 버블링이 지원되지 않는 경우의 수는, 그리고 그러한 경우 어떻게 해결해야하는지 -> 이벤트 캡쳐링을 이용하여 해결 가능. 경우의 수는...?

Some events, such as focus, don't bubble but can be captured. The inline handler on the target element triggers before capture handlers for the target element.
(포커스와 같은 일부 이벤트에는 버블링되진 않지만 캡처할 순 있다. 대상 요소의 인라인 처리기는 대상 요소에 대한 캡처 핸들러 전에 트리거된다.)

Many newly specified events in the web platform (such as the media events) do not bubble, which is a problem for frameworks like Ember that rely on event delegation. However, the capture API, which was added in IE9, is invoked properly for all events, and does not require a normalization layer. Not only would supporting the capture API allow us to drop the jQuery dependency, but it would allow us to properly handle these non-bubbling events. This would allow you to use events like playing in your components without having to manually set up event listeners.
(웹 플랫폼에서 새로 지정된 많은 이벤트(예. 미디어 이벤트)는 버블링되지 않는다. 이는 이벤트 위임에 의존하는 Ember와 같은 프레임 워크의 문제. ie9에서 추가된 캡처 api는 모든 이벤트에 대해 제대로 호출되며 정규화 계층이 필요하지 않음. 캡쳐 api를 지원하여 jQuery의 종속성을 삭제할 수 있고 이러한 버블링 이벤트가 지원되지 않는 경우 적절하게 처리 가능. 이를 통해 이벤트 리스너를 수동을 설정할 필요 없이 구성 요소에서 재생과 같은 이벤트를 사용할 수 있음.)

event.stopPropagation() 문제점
1. We create a nested menu. Each submenu handles clicks on its elements and calls stopPropagation so that the outer menu won’t trigger.
(중첩 메뉴를 만든다. 각 하위 메뉴는 요소 클릭을 처리하고 외부 메뉴가 트리거되지 않도록 stopPropagation

을 호출한다. )
2. Later we decide to catch clicks on the whole window, to track users’ behavior (where people click). Some analytic systems do that. Usually the code uses document.addEventListener('click'…) to catch all clicks.
(나중에 우리가 사용자의 행동(사람들이 클릭하는 위치)을 추적하기 위해서 전체 창에서 클릭을 포착하기로 결정했을 때, 일부 분석시스템은 그렇게 한다. 보통 코드는 document.addEventListener("click"...)을 사용한다 모든 클릭들을 잡아내는데...)
3. Our analytic won’t work over the area where clicks are stopped by stopPropagation. Sadly, we’ve got a “dead zone”.
우리의 분석은 stopPropagation에 의해 멈춰진 클릭들의 위치에서 작동하지 않는다. 슬프게도 우리는 죽은 지대를 가지고 있다.

-> 요약 : 이벤트 버블링을 강제로 멈추면, 몇몇 시스템에서 해당 위치에서 제대로 작동하지 않는다.

# 뽀또님

## **이벤트(event)**

**이벤트(event)는 어떤 사건을 의미한다.**

**브라우저에서의 이벤트란 예를 들어 사용자가 버튼을 클릭했을 때, 웹페이지가 로드되었을 때와 같은 것인데 이것은 DOM 요소와 관련이 있다.**

**이벤트가 발생하면 그에 맞는 반응을 하여야 한다.**

**이를 위해 이벤트는 일반적으로 함수에 연결되며 그 함수는 이벤트가 발생하기 전에는 실행되지 않다가 이벤트가 발생되면 실행된다.**

**이 함수를 이벤트 핸들러라 하며 이벤트에 대응하는 처리를 기술한다.**

### **UI Event**

- load : 웹페이지나 스크립트의 로드가 완료되었을 때
- unload : 웹페이지가 언로드될 때(주로 새로운 페이지를 요청한 경우)
- error : 브라우저가 자바스크립트 오류를 만났거나 요청한 자원이 존재하지 않는 경우
- resize : 브라우저 창의 크기를 조절했을 때
- scroll : 사용자가 페이지를 위아래로 스크롤할 때
- select : 텍스트를 선택했을 때

### **Keyboard Event**

- keydown : 키를 누르고 있을 때
- keyup : 누루고 있던 키를 땔 때
- keypress : 키를 누르고 땟을 때
- keyCode : 키 코드 값.

### **Mouse Event**

- click : 마우스 버튼을 클릭했을 때
- dbclick : 마우스 버튼을 더블 클릭했을 때
- mousedown : 마우스 버튼을 누르고 있을 때
- mouseup : 누르고 있던 마우스 버튼을 땔때
- mousemove : 마우스를 움직일 때
- mouseover : 마우스를 요소 위로 움직였을 때
- mouseout : 마우스를 요소 밖으로 움직였을 때
- mouseenter : 해당 요소에 마우스 커서를 올렸다 놓았을 때
- mouseleave : 마우스를 요소 위로 움직였을 때

### **Focus Event**

- focus/focusin : 요소가 포커스를 얻었을 떄
- blur/foucusout : 요소가 포커스를 잃었을 때

### **Form Event**

- input : input 또는 textarea 요소의 값이 변경되었을 때 , contenteditable 어트리뷰트를 가진 요소의 값이 변경되었을 때
- change : select box, checkbox, radio button의 상태가 변경되었을 때
- submit : form을 submit할 때 (버튼 또는 키)
- reset : reset 버튼을 클릭할 때 (최근에는 사용 안함)

### **Clipboard Event**

- cut : 콘텐츠를 잘라내기 할 때
- copy : 콘텐츠를 복사할 때
- paste : 콘텐츠를 붙여넣기 할 때

### **이벤트 핸들러 등록**

**인라인 방식은 이벤트를 이벤트 대상의 태그 속성으로 지정하는 것이다.**

### **프로퍼티 방식**

**프로퍼티 리스너 방식은 이벤트 대상에 해당하는 객체의 프로퍼티로 이벤트를 등록하는 방식이다.**

**인라인 방식에 비해서 HTML과 JavaScript를 분리할 수 있다는 점에서 선호되는 방식이지만 뒤에서 배울 addEventListener 방식을 추천한다.**

**이벤트 핸들러 프로퍼티 방식은 이벤트에 오직 하나의 이벤트 핸들러만을 바인딩할 수 있다**

### **addEventListener 메소드 방식**

**addEventListener 메소드를 이용하여 대상 DOM 요소에 이벤트를 바인딩하고 해당 이벤트가 발생했을 때 실행될 콜백 함수(이벤트 핸들러)를 지정한다.**

**addEventListener 함수 방식은 이전 방식에 비해 아래와 같이 보다 나은 장점을 갖는다.**

- 하나의 이벤트에 대해 하나 이상의 이벤트 핸들러를 추가할 수 있다.
- 캡처링과 버블링을 지원한다.
- HTML 요소뿐만아니라 모든 DOM 요소(HTML, XML, SVG)에 대해 동작한다. 브라우저는 웹 문서(HTML, XML, SVG)를 로드한 후, 파싱하여 DOM을 생성한다.

### **이벤트 핸들러 함수 내부의 this**

**인라인 이벤트 핸들러 방식의 경우, 이벤트 핸들러는 일반 함수로서 호출되므로 이벤트 핸들러 내부의 this는 전역 객체 window를 가리킨다.**

**이벤트 핸들러 프로퍼티 방식에서 이벤트 핸들러는 메소드이므로 이벤트 핸들러 내부의 this는 이벤트에 바인딩된 요소를 가리킨다.**

**이것은 이벤트 객체의 currentTarget 프로퍼티와 같다.**

### **addEventListener 메소드 방식**

**addEventListener 메소드에서 지정한 이벤트 핸들러는 콜백 함수이지만 이벤트 핸들러 내부의 this는 이벤트 리스너에 바인딩된 요소(currentTarget)를 가리킨다.**

**이것은 이벤트 객체의 currentTarget 프로퍼티와 같다.**

# 초생님

1. 이벤트 드리븐 프로그래밍은 프로그램의 흐름을 이벤트 중심으로 제어하는 프로그래밍 방식이다.
2. 이벤트 핸들러는 이벤트가 발생했을 때 브라우저에 의해 호출될 함수이다.
3. 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것은 이벤트 핸들러 등록이라고 한다.
4. 이벤트 핸들러 어트리뷰트 방식은 on 접두사와 이벤트 타입으로 이뤄져 있다. 이벤트 어트리뷰트값으로 함수 문을 할당하면 등록된다.
5. 이벤트 핸들러 프로퍼티 방식은 on접두사와 이벤트 종류를 나타내는 이벤트 타입으로 이뤄져 있다.
6. addEventListener 메서드로도 이벤트 핸들러를 등록할 수 있다.
