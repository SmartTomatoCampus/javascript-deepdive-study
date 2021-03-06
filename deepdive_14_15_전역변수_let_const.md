# 딥다이브 14~15장, 전역변수, let, const 등 발표록

### 애한

1. 블록 레벨 스코프에 대해서 알았다
2. 변수의 생명주기에 대해 알았고, 이게 없으면 영원히 메모리를 차지하기 때문에, 변수의 생명 주기의 필요성
3. 전역 변수로 인해서 해당 스코프를 참조하는 기간 동안 메모리를 점유하므로 사용을 자제해야 한다.
4. ES6 가 있지만 아직 많은 회사에서 레거시로 ES5 를 쓰는 부분이 있을 수 있기 때문에 이 또한 알아야할 필요성도 있다.

### 꽁치

1. 자바스크립트 파일은 파일간의 전역 변수가 영향을 준다는 것을 알았다.
2. `type="module"` 을 붙여주면 ES6 모듈로 자바스크립트가 동작하게 된다는 것을 알았다.
3. 또한 ES6 모듈의 자바스크립트 파일의 확장자는 `.mjs` 로 권장된다는 것을 알았다.
4. ‘변수 호이스팅이 발생하지 않는 것처럼 보인다'라는 말이 이전에 딥다이브를 볼 때는 이해하지 못 했는데 var 를 제대로 이해하고 나서 보니 이제 이해가 갔다.
    
    선언 단계와 초기화 단계가 분리되어 있기 때문에 let, const 는 이전에 참조하려면 에러가 발생하는데,
    선언 단계는 여전히 런타임 이전에 실행되기 때문에 지역 스코프 안에서 전역 변수를 참조하지 않고 지역 변수 이전에 실행되는 부분에 대해서 에러를 내보내는 것이었다.
    

### 초생

1. 변수의 생명주기!! 를 알게 됐다
2. 지역변수는 함수 생명주기랑 같고, 전역변수는 애플리케이션의 생명주기와 같다
3. `var` 키워드 전역변수는 전역 객체의 생명주기와 일치한다.
    
    → 전역 객체란?? 자바스크립트 엔진이 런타임 이전에 만드는 객체!!
    
    `Object`, `String` 등의 빌트인 객체랑 Web API 랑 `var` 키워드 전역 변수 등을 프로퍼티로 갖는! 객체다
    
    클라이언트 사이드에선 `window`, 서버 사이드에선 `global`
    
4. `let` 과 `const` 는 블록 레벨 스코프를 지원하고,
`const` 에서 상수의 개념이 조금 애매했지만 재할당이 금지된거지 값이 변경되는 걸 금지한테 아니라는 것을 알았다.

### 뽀또

1. 암묵적 결합 전역 변수를 선언한 의도는 전역, 즉 코드 어디서든 참조하고 할당할 수 있는 변수를 사용하겠다는 것이다.
2. 스코프 체인상에서 종점에 존재할때 전역 변수의 검색 속도가 가장 느리다.
3. ES6의 모듈 기능을 사용하는것보다 Webpack 등의 모듈 번듈러를 사용하는 것이 일반적이다.

```jsx
// 스폰지밥.js

var 스폰지밥 = { 
	name = '스폰지밥'
 }

스폰지밥.name

// 뚱이.js

var 뚱이 = {
	name = '뚱이'
}

뚱이.name
```

```jsx
<head>
	<script src="./main.js"></script>
</head>

<body>
	...
	<script src="./main.js"></script>
</body>

<head>
	<script defer src="./main.js"></script>
</head>
```

⇒ 차이 각자 공부하기

**⇒ 공부해서 나 주자**
