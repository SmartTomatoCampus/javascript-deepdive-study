# 발표문 - 브라우저의 렌더링 과정

# 꽁치님

1. 잘 모르겠는 것
css display: none 속성은 렌더 트리에 포함되지 않는다고 하는데 그럼 이벤트를 걸어서 보였다 안보였다 할 때 렌더 트리에 해당 노드가 포함되는 과정에서 그 부분만 재 렌더링이 이루어질까 아니면 그 하위의 노드들은 다시 새롭게 렌더링이 될까
2. 최종적으로는 HTML 문서를 읽어들여가며 DOM 을 생성해가는 과정에서 CSS 가 더해지면 CSSOM 을, 자바스크립트가 끼얹어지면 AST 를 생성해가며 최종적으로 DOM 을 완성해간다는 것을 알았다.
3. 그리고 자바스크립트에게 제어권이 넘어갈 때 DOM 을 동적으로 조작할 수 있도록 하는 것이 DOM API 라는 것을 알았다.

# 루피님

1. HTML, CSS 자바스크립트 등 리소스를 요청한 후 서버로부터 응답을 받음 -> 서버로부터 받은 정보를 브라우저가 파징해서 DOM과 CSSOM을 생성한 뒤 결합해서 렌더 트리를 생성한다
-> 브라우저 엔진이 자바스크립트를 파싱한 뒤 DOM API로 DOM이나 CSSOM을 변경함 -> 렌더 트리로 결합됨 -> 브라우저 화면에 요소 페인팅
2. 바이트 -> 문자 -> 토큰 -> 노드 ->DOM의 과정을 거쳐 DOM이 생성된다.
3. 렌더 트리는 렌더링을 위한 트리구조의 자료구조로 DOM과 CSSOM이 결합된 것이다.
4. 자바스크립트의 위치를 body 태그 아래에 위치하면 DOM이 미완성 된 상태에서 DOM을 조작하는 경우가 발생하지 않기 때문에 에러가 생길 우려도 없다.
5. async 어트리뷰트는 script 태그의 순서와 관계 없이 로드가 완료되며 defer 어트리뷰트는 DOM 생성이 완료된 직후에 자바스크립트가 진행하게 한다.

# 초생님

1. 파싱 : 문서를 읽고 토큰으로 분해해서 트리를 생성하는 과정
2. 렌더링 : 브라우저에 시각적으로 출력하는 것
3. 브라우저 렌더링
    - 리소스 요청하고 서버에 응답받음
    - 브라우저 렌더링 에진은 html , css 파싱해서 dom이랑 ccsom 를 생성하고 결합해서 렌더 트리 생성한다.
    - 브라우저 자바스크립트 엔진은 자바스크립트를 파싱하여 실행한다.
    - DOM API 를 통해 DOM이나 CSSOM 조작한다.
    - 렌더 트리를 기반으로 HTML 요소 레이아웃을 계산해서 화면에 페인팅한다.
4. CSSOM있는 줄 처음 알았는데 CSS 상속관계를 트리로 만든것
5. 렌터 트리는 DOM + CSSOM 화면에 렌더링 되는 노드만 구성된다.

# 애한님

### 브라우저 렌더링 과정에 대해서 학습.

브라우저의 렌더링 수행 과정

1. 브라우저는 HTML, CSS, 자바스크립트, 이미지, 폰트 파일 등 렌더링에 필요한 리소스를 요청하고 서버로부터 응답을 받는다.
2. 브라우저의 렌더링 엔진은 서버로부터 응담된 HTML과 CSS를 파싱하여 DOM과 CSSOM을 생성하고 이를 결합하여 렌더 트리를 생성한다.
3. 브라우저의 자바스크립트 엔진은 서버로부터 응답된 자바스크립트를 파싱하여 AST(Abstract Syntax Tree)를 생성하고 바이트 코드로 변환하여 실행한다. 이 때 자바스크립트는 DOM API를 통해 DOM이나 CSSOM을 변경할 수 잇다. 변경된 DOM과 CSSOM은 다시 렌더 트리로 결합된다.
4. 렌더 트리를 기반으로 HTML요소와 레이아웃(위치와 크기)을 계산하고 브라우저 화면에 HTML 요소를 페인팅한다.
- 레이아웃 계산과 페인팅을 다시 실행하는 리렌더링은 비용이 많이드는, 즉 성능에 악영향을 주는 작업이다. 따라서 가급적 리렌더링이 빈번하게 발생하지 않도록 주의할 필요가 있다.
=> (%, vh, vw)같은 속성은 매번 달라지는가? yes....

### 진짜 요약...

1. 렌더링과정에 대해 이해
a. 필요한 리소스 요청
b. html,css 파싱/ dom, cssom 생성 / 렌더트리 생성
c. 레이아웃 계산
d. 페인팅
2. link, script, img 등 호출할 때마다 html 파싱을 멈춘다.
3. html을 전부 파싱받은 후에 js파일을 호출할 수 있다. (js파일은 defer를 사용하여 호출하면 좋다.)
4. 레이아웃 게산과 페인팅을 다시하는 리렌더링은 비용이 많이든다. -> 반응형 웹을 구현하기위해 주로 (%, vh, vw 같은 속성 이용시) 성능에 악영향을 준다.

# 명성님

1. 브라우저와 서버의 리소스 요청(Request)과 응답(Response)
브라우저는 서버에게 리소스를 요청하고 서버로부터 응답받는다.
2. 리소스 중에서 HTML/CSS의 parsing과 렌더 트리 생성
HTML파싱 중 HTML 내의 link,img,script 를 발견하면 HTML 파싱을 일시 중단하고 해당 리소스 파일을 서버로 요청하고 응답받는다.
브라우저는 응답받은 리소스를 파싱하고 DOM/CSSOM 생성하여 렌더 트리를 생성한다.
3. 자바스크립트를 파싱하여 DOM / CSSOM 조작/변경 과정 후 렌더트리로 결합
자바스크립트를 파싱하여 AST생성, 바이트코드 변환 과정을 거친 뒤 실행하고, 실행하면서 변경되는 DOM/CSSOM을 다시 렌더트리로 결합한다.
4. 변경된 리소스를 통해 HTML에 렌더링
렌더 트리를 기반으로 HTML 요소의 레이아웃을 계산하고 브라우저 화면에 HTML 요소를 페인팅한다.

# 뽀또님

1. HTML 파일과 CSS 파일을 파싱해서 각각 Tree를 만든다. (Parsing)
2. 두 Tree를 결합하여 Rendering Tree를 만든다. (Style)
3. Rendering Tree에서 각 노드의 위치와 크기를 계산한다. (Layout)
4. 계산된 값을 이용해 각 노드를 화면상의 실제 픽셀로 변환하고, 레이어를 만든다. (Paint)
5. 레이어를 합성하여 실제 화면에 나타낸다. (Composite)

### 리플로우(Reflow)가 일어나는 대표적인 속성

position, width, height, margin, padding, border, border-width, font-size, font-weight, line-height, text-align, overflow

### 리페인트(Repaint)만 일어나는 대표적인 속성

background, color, text-decoration, border-style, border-radius
