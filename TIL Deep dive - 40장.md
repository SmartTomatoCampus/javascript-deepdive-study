브라우저는 처리해야 할 특정 사건이 발생하면 이를 감지하여 이벤트를 발생시킨다. 예를 들어 클릭,키보드입력, 마우스 이동 등이 일어나면 브라우저는 이를 감지하여 특정한 타입의 이벤트를 발생시킨다.

만약 어플리케이션이 특정 타입의 이벤트에 대해 반응하여 어떤 일을 하고 싶다면 해당하는 타입의 이벤트가 발생했을 때 호출될 함수를 `브라우저에게 알려 호출을 위임`한다.

이때 `이벤트가 발생했을 때 호출될 함수를 이벤트 핸들러`라 하고 `이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것을 이벤트 핸들러 등록`이라 한다.

사용자가 버튼을 클릭했을 때 함수를 호출하여 어떤 처리를 하고 싶다고 가정해보자. 이때 문제는 '언제 함수를 호출해야 하는가'다. 사용자가 언제 버튼을 클릭할지 알 수 없으므로 언제 함수를 호출해야 할 지 알 수 없기 때문이다.

다행히 브라우저는 사용자의 버튼클릭을 감지하여 클릭 이벤트를 발생시킬 수 있다. 그리고 특정 버튼 요소에서 클릭 이벤트가 발생하면 특정 함수(이벤트 핸들러)를 호출하도록 `브라우저에게 위임(이벤트 핸들러 등록)할 수 있다.`

**즉 함수를 언제 호출할지 알 수 없으므로 개발자가 명시적으로 함수를 호출하는 것이 아니라 브라우저에게 함수 호출을 위임하는 것이다.**

```jsx

<button>Click me!</button>

// 사용자가 버튼을 클릭하면 함수를 호출하도록 요청 
$button.onclick = () => { alert('button click'); };
```
버튼 요소 $button의 onclick 프로퍼티에 함수를 할당했다. Window,Document,HTMLElement 타입의 객체는 onclick과 같이 특정 이벤트에 대응하는 다양한 이벤트 헨들러 프로퍼티를 가지고 있다.
이 이벤트 핸들러 프로퍼티에 함수를 할당하면 해당 이벤트가 발생했을 때 할당한 함수가 브라우저에 의해 호출된다.
이벤트와 그에 대응하는 함수(이벤트 핸들러)를 통해 사용자와 애플리케이션은 상호작용을 할 수 있다.
이와 같이 프로그램의 흐름을 이벤트 중심으로 제어하는 프로그래밍 방식을 이벤트 드리븐 프로그래밍이라 한다.
___

#### 이벤트 타입

이벤트 타입은 약 200여가지가 있다.
`DOMContentLoaded` : HTML 문서의 로드와 파싱이 완료되어 DOM 생성이 완료되었을 때.

`load` : DOMContentLoaded 이벤트가 발생한 이후, 모든 리소스의 로딩이 완료되었을 때.

#### 이벤트 핸들러 등록

이벤트 핸들러(이벤트 리스너)는 이벤트가 발생했을 때 브라우저에 호출을 위임한 함수다. 다시 말해 이벤트가 발생하면 브라우저에 의해 호출될 함수가 이벤트 핸들러다.
이벤트가 발생했을 때, 브라우저에게 이밴트 핸들러의 호출을 위임하는 것을 이벤트 핸들러 등록이라 한다.

#### 이벤트 핸들러를 등록하는 방법 3가지

#### 1. 이벤트 핸들러 어트리뷰트 방식.

이벤트 핸들러 어트리뷰트 방식으로 이벤트를 등록하기 위해서는 함수 참조가 아닌 함수 호출문 등의 문을 할당한다.
```jsx
<button onclick="sayHi('Lee')">Click</button>
```
이떄 이벤트 핸들러 어트리뷰트 값은 사실 암묵적으로 생성될 이벤트 핸들러의 함수 몸체를 의미한다. 즉 위의 `onclick="sayHi('Lee')` 어트리뷰트는 파싱되어 다음과 같은 함수를 암묵적으로 생성하고 이벤트 핸들러 어트리뷰트 이름과 동일한 키 onclick 이벤트 핸들러 프로퍼티에 할당한다.
```js
function onclick(event){
  sayHi('Lee');
}
```
이처럼 동작하는 이유는 이벤트 핸들러에 인수를 전달하기 위해서다. 만약 이벤트 핸들러 어트리뷰트 값으로 함수 참조를 할당해야 한다면 이벤트 핸들러에 인수를 전달하기 곤란하다.

결국 이벤트 핸들러 어트리뷰트 값으로 할당한 문자열은 암묵적으로 생성되는 이벤트 핸들러의 함수 몸체다. 따라서 이벤트 핸들러 어트리뷰트 값으로 여러 개의 문을 할당할 수 있지만 JS와 HTML은 분리하는 것이 좋다.
하지만 모던 자바스크립트에서는 이벤트 핸들러 어트리뷰트 방식을 사용하는 경우가 있다. CBD 방식의 Ang/React/Svelte/Vew 같은 프레임워크/라이브러리에서는 이벤트 핸들러 어트리뷰트 방식으로 이벤트를 처리한다. CBD에서는 HTML,CSS,JS 모두 뷰를 구성하기 위한 구성 요소로 보기 때문에 관심사가 다르다고 생각하지 않는다.

#### 2. 이벤트 핸들러 프로퍼티 방식

window 객체와 Document, HTMLElement 타입의 DOM 노드 객체는 이벤트에 대응하는 이벤트 핸들러 프로퍼티를 가지고 있다.
이벤트 핸들러 프로퍼티의 키는 이벤트 핸들러 어트리뷰트와 마찬가지로 onclick과 같이 on 접두사와 이벤트의 종류를 나타네는 이벤트 타입으로 이루어져 있다. 이벤트 핸들러 프로퍼티에 함수를 바인딩하면 이벤트 핸들러가 등록된다.
```js
$button.onclick = function(){
  console.log('foo');
};
```
이벤트 핸들러는 대부분 이벤트를 발생시킬 이벤트 타깃에 바인딩하지만 반드시 이벤트 타깃에 핸들러를 바인딩해야 하는 것은 아니다.
이벤트 핸들러 프로퍼티 방식에는 하나의 이벤트 핸들러만 바인딩 할 수 있다는 단점이 있다.

#### 3. addEventListener 메서드 방식
EventTarget.prototype.addEventListner 메서드를 사용하여 이벤트 핸들러를 등록할 수 있다. 이벤트 프로퍼티 방식과는 달리 on 접두사를 부팅지 않는다. 두 번째 매개변수에는 이벤트 핸들러를 전달한다. 마지막 매개변수에는 이벤트를 캐치할 이벤트 전파단계 (캡쳐링/버블링)를 지정한다. 생략하거나 false를 지정하면 버블링 단계에서 이벤트를 캐치하고, true를 지정하면 캡쳐링 단계에서 이벤트를 캐치한다.

` 이벤트 핸들러 프로퍼티 방식은 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩하지만 addEventListener 메서드에는 이벤트 핸들러를 인수로 전달한다.`
만약 동일한 HTML 요소에서 발생한 동일한 이벤트에 대해 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식을 모두 사용하여 이벤트 핸들러를 등록하면 어떻게 동작할지 생각해보자.

addEventListner 메서드 방식은 이벤트 핸들러 프로퍼티에 바인딩된 이벤트 핸들러에 아무런 영향을 주지 않는다. 즉 이벤트 프로퍼티 방식에 영향을 주지 않으며 하나 이상의 이밴트 핸들러를 등록할 수 있게 된다. 
하지만 참조가 동일한 이밴트 핸들러를 중복 등록하면 하나의 핸들러만 등록된다.

#### 이밴트 핸들러 제거

EventTarget.prototype.removeEventListener 메서드를 사용하며 전달된 인수가 일치해야 이벤트 핸들러가 제거된다.
만약 무명함수를 이밴트핸들러로 등록한 경우 제거할 수 없다. 이벤트 핸들러를 제거하려면 이벤트 핸들러의 참조를 변수나 자료구조에 저장하고 있어야 한다.
___

### 이벤트 객체

이벤트가 발생하면 이벤트에 관련한 다양한 정보를 담고 있는 이벤트 객체가 동적으로 생성된다.
생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달된다.
```js
// 클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달된다.
function showCoords(e){
$msg.textContent = `${e.clientX}, ${e.clientY}`;
}
```

#### 이벤트 객체의 상속 구조
이벤트가 발생하면 이벤트 타입에 따라 다양한 타입의 이벤트 객체가 생성된다. Event,UIEvent,MouseEvent 등 모두는 생성자 함수다. 따라서 생성자 함수를 호출하여 이벤트 객체를 생성할 수 있다.
```js
//Event 생성자 함수를 호출하여 foo 이벤트 타입의 Event 객체를 생성한다.
let e = new Event('foo');
console.log(e) // {isTrusted: false, type:"foo", target:null ...}
e.type // foo

e = new FocusEvent('focus')
console.log(e) // FocusEvent { isTrusted: false, relatedTarget: null, view: null, ...}
```
이처럼 이벤트가 발생하면 암묵적으로 생성되는 이벤트 객체도 생성자 함수에 의해 생성된다. 그리고 생성된 이벤트 객체는 생성자 함수와 더불어 생성되는 프로토타입으로 구성된 프로토타입 체인의 일원이된다. 예를 들어 click 이벤트가 발생하며 암묵적으로 생성되는 MouseEvent 타입의 이벤트 객체는 UIEvent - Event 프로토타입 체인의 일원이된다.

#### 이벤트 객체의 공통 프로퍼티

Event 인터페이스, 즉 Event.prototype에 정의되어 있는 이벤트 관련 프로퍼티는 UIEvent, CoustomEvent, MouseEvent등 모든 파생 이벤트 객체에 상속된다. 즉, Event 인터페이스의 이벤트 관련 프로퍼티는 모든 이벤트 객체가 상속받는 공통 프로퍼티다. 이벤트 객체의 공통 프로퍼티는 다음과 같다.

- type : 이벤트 타입 : string
- target : 이벤트를 발생시킨 DOM 요소 : DOM 요소 노드
- currentTarget : `이벤트 핸들러가 바인딩된 DOM 요소` : DOM 요소 노드
- eventPhase : 이벤트 전파 단계 : number
- bubbles : 이벤트를 버블링으로 전파하는지 여부.
몇가지 이벤트는 bubbles:false로 버블링되지 않는다.
	- 포커스 이벤트 `focus` `blur`
    - 리소스 이벤트 `load` `unload` `abort` `error`
    - 마우스 이벤트 `mouseenter` `mouseleave`
- calcelable : preventDefault를 통해 기본 동작을 취소 시킬 수 있는지 여부
몇가지 이벤트는 calcleable:false로 취소할 수 없다.
	- 포커스 이벤트 `focus` `blur`
    - 리소스 이벤트 `load` `unload` `abort` `error`
    - 마우스 이벤트 `dblclick` `mouseenter` `mouseleave`
- defaultPrevented : preventDefault 메서드를 호출하여 이벤트를 취소했는지 여부
- isTrusted : 사용자의 행위에 의해 발생한 이벤트인지 여부.
예를 들어, click 메서드 또는 dispatchEvent 메서드를 통해 인위적으로 발생시킨 경우에 isTrusted는 false이다.
- timeStamp : 이벤트가 발생한 시각 : number

이벤트 객체의 `target 프로퍼티는 이벤트를 발생시킨 객체`를 나타내고 target 프로퍼티가 가리키는 객체는 어떤 이벤트를 발생시킨 DOM 요소일 것이다. 이 객체는 현재 상태를 나타낸다.
이밴트 객체의 `currentTarget 프로퍼티는 이벤트 핸들러가 바인딩 된 DOM 요소`를 가리킨다. 

#### 마우스 이벤트가 갖는 고유 프로퍼티

마우스 포인터의 좌표 정보를 나타내는 프로퍼티
`screenX/screenY, clientX/clientY, pageX/pageY, offsetX/offsetY`

버튼 정보를 나타내는 프로퍼티
`altKey, ctrlKey, shiftKey, button`

#### 키보드 이벤트가 갖는 고유 프로퍼티
`altKey, ctrlKey, shiftKey, metaKey, key, keyCode`

keyup 이벤트가 발생하면 생성되는 이벤트 객체는 입력한 키 값은 문자열로 반환하는 key 프로퍼티를 제공한다.
참고로 한글같은 경우는 이벤트 핸들러가 두 번 호출되는 현상이 발생하므로 keydown 이벤트를 캐치하는것이 좋다.
___

### 이벤트 전파 event propagation
DOM 트리 상에 존재하는 DOM 요소 노드에서 발생한 이벤트는 DOM 트리를 통해 전파된다. 이를 이벤트 전파라고 한다.
```jsx
<ul>
  <li></li>
  <li></li>
  <li></li>
</ul>
```
ul에 addEventListner를 달아주었다면
ul 요소의 두 번째 자식 요소인 li 요소를 클릭하면 클릭 이벤트가 발생한다. 이떄 생성된 이벤트 객체는 이벤트를 발생시킨 DOM 요소인 이벤트 타깃을 중심으로 DOM 트리를 통해 전파된다. 이벤트 전파는 이벤트 객체가 전파되는 방향에 따라 3단계로 구분할 수 있다.
- 캡쳐링 단계 : 이벤트가 상위 요소에서 하위 요소 방향으로 전파.
- 타깃 단계 : 이벤트가 이벤트 타깃에 도달.
- 버블링 단계: 이벤트가 하위 요소에서 상위 요소 방향으로 전파.

이때 이밴트 타겟은 li 요소이고 커런트타겟은 ul 요소이다.

li 요소를 클릭하면 클릭 이벤트가 발생하여 클릭 이벤트 객체가 생성되고 클릭된 li 요소가 이벤트 타겟이 된다.
이때 클릭 이벤트 객체는 window에서 시작하여 이벤트 타깃 방향으로 전파된다. 이것이 캡쳐링 단계다.
이후 이벤트 객체는 이벤트를 발생시킨 이벤트 타겟에 도달한다. 이것이 타겟단계다 이후 이벤트 객체는 이벤트 타겟에서 시작해서 윈도우 방향으로 전파된다. 이것이 버블링 단계다.

이벤트 핸들러 어트리뷰트/프로퍼티 방식으로 등록한 이벤트 핸들러는 타깃 단계와 버블링 단계의 이벤트만 캐치할 수 있다.

하지만 addEventListener 메서드 방식으로 등록한 이베느 핸들러는 타깃 단계와 버블링 단계뿐만 아니라 캡쳐링 단계의 이벤트도 선별적으로 캐치할 수 있다. 캡처링 단계의 이벤트를 캐치하려면 addEventListener메서드의 3번째 인수로 true를 전달해야 한다. 3번째 인수를 생략하거나 false 를 전달하면 타깃 단계와 버블링 단계의 이벤트만 캐치할 수 있다.

만약 이벤트 핸들러가 캡처링 단계의 이벤트를 캐치하도록 설정되어 있다면 이벤트 헨들러는 window에서 시작해서 이벤트 타겟 방향으로 전파되는 이벤트 객체를 캐치하고, 이벤트를 발생시킨 이벤트 타겟과 이벤트 핸들러가 바인딩된 커런트 타깃이 같은 DOM 요소라면 이벤트 핸들러는 타겟 단계의 이벤트 객체를 캐치한다.

>  1. window에서 이벤트 타겟으로 capturing 되는 이벤트 객체를 catch
2. 이벤트를 발생시킨 이벤트 타겟과 이벤트 핸들러가 바인딩 된 커런트 타겟을 비교
3. 타겟과 커런트타겟이 같은 DOM 요소 노드라면 이밴트 핸들러는 `target`단계의 이밴트 객체를 catch

이처럼 이벤트는 이벤트를 발생시킨 이벤트 타겟은 물론 상위 DOM 요소에서도 catch할 수 있다.
즉 DOM트리를 통해 전파되는 이벤트는 이벤트 패스에 위치한 모든 DOM요소에서 캐치할 수 있다.


![](https://velog.velcdn.com/images/tchaikovsky/post/55049ad1-9c3b-4444-b814-b95af072a6d6/image.gif)





```html
<body>
    
    <button>click</button>
    
</body>
```

```js
document.body.addEventListener("click", (e) => {
  if (e.target !== e.currentTarget){
 	return
  }else{
    console.log('body');
}});

document.querySelector("button").addEventListener("click", () => {
  console.log("button");
});

```

위 예제는 이벤트의 버블링을 막는 방법이다.
typeguard를 사용함으로써 button을 누를 때 body의 실행문이 실행되지 않게 만들 수 있다.


### DOM 요소의 기본 동작 조작

DOM 요소는 저마다 기본 동작이 있다. 예를 들어 a요소를 클릭하면 href 어트리뷰트에 지정된 링크로 이동하고 checkbox 또는 ratio 요소를 클릭하면 체크 또는 해제된다.


#### preventDefault();

이벤트 객체의 preventDefault 메서드는 이러한 DOM 요소의 기본 동작을 중단시킨다.

```js

document.querySelector('a').onclick = e => {
  e.preventDefault();
}

document.querySelector(input[type=checkbox]').onclick = e => {
           //checkbox 요소의 기본 동작을 중단한다.
           e.preventDefault();
		};
```

#### 이벤트 전파 방지
이벤트 객체의 stopPropagation 메서드는 이벤트 전파를 중지시킨다. 다음 예제를 살펴보자
```html
<div class="container">
  <button class="btn1">Button 1</button>
  <button class="btn2">Button 2</button>
  <button class="btn3">Button 3</button>
</div>
```
```js
//이벤트 위임. 클릭된 하위 버튼 요소의 color를 변경한다.
document.querySelector('.container').onclick({ target }) => {
  if(!target.matches('.container > button')) return;
};

// .btn2 요소는 이벤트를 전파하지 않으므로 상위 요소에서 이벤트를 캐치할 수 없다.
document.querySelector('.btn2').onclick = e => {
  e.stopPropagation(); // 이벤트 전파 중단
  e.target.style.color = 'blue';
};
```
위 예제를 살펴보면 상위 DOM 요소인 Container 요소에 이벤트를 위임했다. 따라서 하위 DOM 요소에서 발생한 클릭 이벤트를 상위 DOM 요소인 container 요소가 캐치하여 이벤트를 처리한다.

하지만 하위 요소중에서 btn2 요소는 자체적으로 이벤트를 처리한다. 이때 btn2 요소는 자신이 발생시킨 이벤트가 전파되는 것을 중단하여 자신에게 바인딩된 이벤트 헨들러만 실행되도록 한다.

이처럼 stopPropagation 메서드는 하위 DOM 요소의 이벤트를 개별적으로 처리하기 위해 이벤트의 전파를 중단시킨다.

> event delagation한 요소의 하위 요소에 개별적인 이벤트를 부여하고 싶다면 stopPropagation을 통해 다른 요소로 전파되지 않게 설정한다.

___

### 이벤트 핸들러 내부의 this

#### 이벤트 핸들러 어트리뷰트 방식

다음 예제의 handleClick 함수 내부의 this는 전역 객체 window를 가리킨다.

```html
<body>
  <button onclick="handleClick()">Click me </button>
<body>
```
```js
function handleCLick() {
  console.log(this); // window
}
```
이벤트 핸들러 어트리뷰트의 값으로 지정한 문자열은 사실 암묵적으로 생성되는 이벤트 핸들러의 문이라고 했다. 따라서 handleClick 함수는 이벤트 핸들러에 의해 일반 함수로 호출된다.

일반 함수로서 호출되는 함수 내부의 this는 전역 객체를 가리킨다. 따라서 handleClick 함수 내부의 this는 전역 객체 window를 가리킨다.
단 이벤트 핸들러를 호출할 때 인수로 전달한 this는 이벤트를 바인딩한 DOM요소를 가리킨다.
```html
<body>
  <button onclick="handleClick(this)">Click me </button>
<body>
```
```js
function handleClick(button) {
  console.log(button); // 이벤트를 바인딩한 button 요소
  console.log(this); // window
}
```
위 예제에서 handleClick 함수에 전달한 this는 암묵적으로 생성된 이벤트 핸들러 내부의 this다. 즉 이벤트 핸들러 어트리뷰트 방식에 의해 암묵적으로 생성된 이벤트 핸들러 내부의 this는 이벤트를 바인딩한 DOM 요소를 가리킨다.
이는 이벤트 핸들러 프로퍼티 방식과 동일하다.

### 이벤트 핸들러 프로퍼티 방식과<br/>addEventListener 메서드 방식

이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식 모두 **이벤트 핸들러 내부의 this 이벤트를 바인딩한 DOM 요소를 가리킨다.**
즉 이벤트 핸들러 내부의 **this**는 **이벤트 객체의 currentTarget** 프로퍼티와 같다.

```jsx
<button class="btn1">0</button>
<button class="btn2">0</button>


const $button1 = document.querySelector('.btn1');
const $button2 = document.querySelector('.btn2');

// 이벤트 핸들러 프로퍼티 방식
$button1.onclick = function (e) {
console.log(this) // $button1
console.log(e.currentTarget) // $button1
console.log(this === e.currentTarget) // true
  ++this.textContent;
}

$button2.addEventListener('click', function(e){
  console.log(this); // $button2
  console.log(e.currentTarget); // $button2
  console.log(this === e.currentTarget); // true
  ++this.textContent;
})
```

화살표 함수로 정의한 이벤트 핸들러 내부의 this는 **상위 스코프의 this**를 가리킨다.
화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다.

```jsx

<button class="btn1">0</button>
<button class="btn2">0</button>


const $button1 = document.querySelector('.btn1');
const $button2 = document.querySelector('.btn2');

// 이벤트 핸들러 프로퍼티 방식
$button1.onclick = e => {
console.log(this) // window
console.log(e.currentTarget) // $button1
console.log(this === e.currentTarget) // false
  //this는 window를 가리키므로 window.textContent에 Nan을 할당한다
  //undefined + 1;
  ++this.textContent;
}

$button2.addEventListener('click', function(e){
  console.log(this); // $button2
  console.log(e.currentTarget); // $button2
  console.log(this === e.currentTarget); // true
  
  //this는 window를 가리키므로 window.textContent에 Nan을 할당한다.
  //undefined + 1;
  ++this.textContent;
});

```

#### 클래스에서 이벤트 핸들러를 바인딩 하는 경우

다음 예제는 이벤트 핸들러 프로퍼티 방식을 사용하고 있으나 addEventListener 메서드 방식을 사용하는 경우와 동일하다.
```jsx
<button class="btn">0</button>

class App {
  constructor() {
    this.$button = document.querySelector('.btn');
    this.count = 0;
    
    //increase 메서드를 이벤트 핸들러로 등록
    this.$button.onclick = this.increase;
  }
  
  increase() {
    //이벤트 핸들러 $increase 내부의 this는 DOM 요소(this.$button)을 가리킨다.
    //따라서 this.$button은 this.$button.$button과 같다.
    this.$button.textContent = ++this.count;
    // Type Error : textContent of undefined
  }
}

new App();
```

위 예제의 increase 메서드 내부의 this는 클래스가 생성할 인스턴스를 가리키지 않는다.
이벤트 핸들러 내부의 this는 이벤트를 바인딩할 DOM 요소를 가리키기 때문에 increase 메서드 내부의 this는 this.$button을 가리킨다. 따라서 increase 메서드를 이벤트 핸들러로 바인딩할 때 bind 메서드를 사용해 this를 전달하여 increase 메서드 내부의 this가 클래스가 생성할 인스턴스를 가리키도록 해야한다.

```js
class App {
  constructor() {
    this.$button = document.querySelector('.btn');
    this.count = 0;
    
    //increase 메서드를 이벤트 핸들러로 등록.
    // this.$button.onclick = this.increase;
    
    // increase 메서드 내부의 this가 인스턴스를 가리키도록 한다. 
    this.$button.onclick = this.increase.bind(this);
  }
  
  increase() {
    this.$button.textContent = ++this.count;
  }
}

new App();
```

또는 클래스 필드에 할당한 화살표 함수를 이벤트 핸들러로 등록하여 이벤트 핸들러 내부의 this가 인스턴스를 가리키도록 할 수도 있다.
다만 이때 이벤트 핸들러 increase는 프로토타입 메서드가 아닌 인스턴스 메서드가 된다.
```jsx
<button class="btn">0</button>

class App {
  constructor() {
    this.$button = document.querySelector('.btn');
    this.count = 0;
    
    // 화살표 함수인 increase를 이벤트 핸들러로 등록
    this.$button.onclick = this.increase;
  }
  
  // 클래스 필드 정의
  // increase는 인스턴스 메서드이며 내부의 this는 인스턴스를 가리킨다.
  increase = () => this.$button.textContent = ++this.count;
}
new App();
```



### 이벤트 핸들러에 인수 전달

함수에 인수를 전달하려면 함수를 호출할 때 전달해야 한다.
이벤트 핸들러 어트리뷰트 방식은 함수 호출문을 사용할 수 있기 때문에 인수를 전달할 수 있지만 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식의 경우 이벤트 핸들러를 브라우저가 호출하기 때문에 함수 호출문이 아닌 함수 자체를 등록해야 한다.
따라서 인수를 전달할 수 없다.
그러나 인수를 전달할 방법이 전혀 없는 것은 아니다.
다음 예제와 같이 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달할 수 있다.

```jsx 
<label> User name <input type='text'></label>
  <em class="message"></em>
  
  const MIN_USER_NAME_LENGTH = 5; // 이름의 최소 길이
  const $input = document.querySelector('input[type=text]');
  const $msg = document.querySelector('.message');
  
  const checkUserNameLength = min => {
    $msg.textContent
      = $input.value.length < min
      ? ` 이름은 ${min}자 이상 입력해주세요 `
      : '';
    };
  
  // 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달한다.
  $input.onblur = () => {
    checkUserNameLength(Min_USER_NAME_LENGTH);
    };
  ```
  
  또는 이벤트 헨들러를 반환하는 함수를 호출하면서 인수를 전달할 수도 있다.
  
  ```jsx
const checkUserNAmeLength = min = e => {
  $msg.textContent
      = $input.value.length < min ? ` 이름은 ${min}자 이상 입력해주세요 ` : '';
}

$input.blur = checkUserNameLength(MIN_USER_NAME_LENGTH);
```

#### 커스텀 이벤트

이벤트 객체의 상속 구조에서 살펴본 바와 같이 이벤트 객체는 Event, UIEvent, MouseEvent 같은 이벤트 생성자 함수로 생성할 수 있다.
이벤트가 발생하면 암묵적으로 생성되는 이벤트 객체는 이벤트의 종류에 따라 이벤트 타입이 결정된다. 하지만 Event, UIEvent, MouseEvent 같은 이벤트 생성자 함수를 호출하여 명시적으로 생성한 이벤트 객체는 임의의 이벤트 타입을 지정할 수 있다. 이처럼 개발자의 의도로 생성된 이벤트를 커스텀 이벤트라 한다.

이벤트 생성자 함수는 첫 번째 인수로 이벤트 타입을 나타내는 문자열을 전달받는다. 이때 이벤트 타입을 나타내는 문자열은 기존 이벤트 타입을 사용할 수도 있고, 기존 이벤트 타입이 아닌 임의의 문자열을 사용하여 새로운 이벤트 타입을 지정할 수도 있다. 이 경우 일반적으로 customEvent 이벤트 생성자 함수를 사용한다.

```jsx
// keyboardEvent 생성자 함수로 keyup 이벤트 타입의 커스텀 이벤트 객체를 생성
const keyboardEvent = new keyboardEvent('keyup');
console.log(keyboardEvent.type); // keyup


//CustomEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new customEvent('foo');
console.log(customEvent.type); // foo
```

생성된 커스텀 이벤트 객체는 버블링되지 않으며 preventDefault 메서드로 취소할 수도 없다.
즉 커스텀 이벤트 객체는 bubbles와 cancelable 프로퍼티의 값이 false로 기본 설정된다.

```jsx
const customEvent = new MouseEvent('click');
console.log(customEvent.type) // click;
console.log(customEvent.bubbles) // false;
console.log(customEvent.cancleable) // false;
```

커스텀 이벤트 객체의 bubbles 또는 cancelable 프로퍼티를 true로 설정하려면 이벤트 생성자 함수의 두 번쨰 인수로 bubbles,cancelable 프로퍼티를 갖는 객체를 전달한다.

```js
const customEvent = new MouseEvent('click',{
  bubbles: true,
  cancelable: true
});
```
커스텀 이벤트 객체에는 bubbles,cancelable 프로퍼티 뿐만 아니라 이벤트 타입에 따라 가지는 이베트 고유의 프로퍼티 값을 지정할 수 있다.

MouseEvent 생성자 함수로 생성한 마우스 이벤트 객체는
마우스 포인터의 좌표 정보를 나타내는 마우스 이벤트 객체 고유의 프로퍼티와
버튼 정보를 나타내는 프로퍼티를 갖는다.
이러한 이벤트 객체 고유의 프로퍼티 값을 지정하려면 당므과 같이 이벤트 생성자 함수의 두 번째 인수로 프로퍼티를 전달한다.

```js
const mouseEvent = new MouseEvent('click',{
  clientX: 50,
  clientY: 100
});


// KeyboardEvent 생성자 함수로 keyup 이벤트 타입의 커스텀 이벤트 객체를 생성
const KeyboardEvent = new KeyboardEvent('keyup', { key: 'Enter' });

console.log(keyboardEvent.key) // Enter
```

이벤트 생성자 함수로 생성한 커스텀 이벤트는 isTrusted 프로퍼티의 값이 언제나 false다.
커스텀 이벤트가 아닌 사용자의 행위에 의해 발생한 이벤트에 의해 생성된 이벤트 객체의 isTrusted 프로퍼티 값은 언제나 true다.

```js
const customEvent = new InputEvent('foo');
console.log(customEvent.isTrusted); // false
```
#### 커스텀 이벤트 디스패치
생성된 커스텀 이벤트는 dispatchEvent 메서드로 디스패치 (이벤트를 발생시키는 행위) 할 수 있다.
dispatchEvent 메서드에 이벤트 객체를 인수로 전달하면서 호출하면 인수로 전달한 이벤트 타입의 이벤트가 발생한다.

```jsx
<button class="btn">Click me </button>

const $button = document.querySelector('.btn');

// 버튼 요소에 foo 커스텀 이벤트 핸들러를 등록
// 커스텀 이벤트를 디스패치하기 이전에 이벤트 핸들러를 등록해야 한다.
$button.addEventListener('click', e => {
  console.log(e) // MouseEvent {isTrusted: false, screenX:0, ... }
  alert(`${e} Clicked!');
});

// 커스텀 이벤트 생성

const customEvent = new MouseEvent('click');

//커스텀 이벤트 디스패치(동기 처리), click 이벤트가 발생한다.
$button.despatchEvent(customEvent);
```
일반적으로 이벤트 핸들러는 비동기처리 방식으로 동작하지만 dispatchEvent 메서든느 이벤트 핸들러를 동기처리 방식으로 호출한다. 다시말해 dispatchEvent 메서드를 호출하면 커스텀 이벤트에 바인딩된 이벤트 핸들러를 직접 호출하는 것과 같다. 따라서 dispatchEvent 메서드로 이벤트를 디스페치 하기 이전에 커스텀 이벤트를 처리할 이벤트 핸들러를 등록해야 한다.

기존 이벤트 타입이 아닌 임의의 이벤트 타입을 지정하여 이벤트 객체를 생성하는 경우 일반적으로 CustomEvent 이벤트 생성자 함수를 사용한다.
```js
const customEvent = new CustomEvent('foo');
console.log(customEvent.type) // foo
```
이때 CustomEvent 이벤트 생성자 함수에는 두번째 인수로 이벤트와 함꼐 전달하고 싶은 정보를 담은 detail 프로퍼티를 포함하는 객체를 전달할 수 있다. 이 정보는 이벤트 객체의 detail 프로퍼티(e.detail)에 담겨 전달된다.


```jsx
<button>Click me</button>

// 버튼 요소에 foo 커스텀 이벤트 핸들러를 등록
  // 커스텀 이벤트를 디스패치 하기 이전에 이벤트 핸들러에 등록해야 한다.
$button.addEventListener('foo', e => {
  // e.detail에는 CustomEvent 함수의 두 번째 인수로 전달한 정보가 담겨 있다.
  alert(e.detail.message);
});


const customEvent = new CustomEvent('foo', {
  detail: { message: 'Hello'} 
});

$button.dispatchEvent(customEvent);
```
기존 이벤트 타입이 아닌 임의의 이벤트 타입을 지정하여 커스텀 이벤트 객체를 생성한 경우 반드시 addEventListener 메서드 방식으로 이벤트 핸들러를 등록해야 한다. 이벤트 핸들러 어트리뷰트/프로퍼티 방식을 사용할 수 없는 이유는 'on + 이벤트 타입'으로 이루어진 이벤트 핸들러 어트리뷰트/프로퍼티가 요소노드에 존재하지 않기 떄문이다.

예를들어 'foo'라는 임의의 이벤트 타입으로 커스텀 이벤트를 생성한 경우 'onfoo'라는 핸들러 어트리뷰트/프로퍼티가 요소 노드에 존재하지 않기 떄문에 이벤트 핸들러 어트리뷰트/프로퍼티 방식으로는 이벤트 핸들러를 등록할 수 없다.
