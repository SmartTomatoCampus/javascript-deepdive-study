# 딥다이브 13장 스코프 발표록

### 초생님

1. 스코프는 네임스페이스라는 것을 알았다.
2. 네임스페이스란! 개체를 구분할 수 있는 범위로 하나의 이름 공간 안에는 하나의 이름만 있어야 하는 것을 말하는데,
이런 면에서 스코프는 네임스페이스라고 볼 수 있다.
3. var 는 스코프 내에선 중복 선언이 되는데 let 과 const 는 안된다.
4. var 는 함수에 의해서만 지역 스코프가 생긴다.
5. 자바스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 곳 부터 상위 스코프 방향으로 검색하는 것을 알았다.

### 루피님

1. 스코프는 함수의 중첩에 의해 구조를 갖고 상위 스코프 변수는 하위 스코프에서 참조 할 수 있지만 그 반대는 안된다
2. 렉시컬 스코프라는 것을 알았다
    
    → 렉시컬 스코프란? 함수를 어디서 정의했는지에 따라 스코프를 결정한다!
    

### 꽁치님

1. var 가 함수레벨 스코프이고, 함수 레벨 스코프에 대해 제대로 알았다.
2. 자바스크립트의 var 가 정적 스코프를 가진다는 것을 알았다.

### 뽀또님

1. 프로그래밍 언어는 렉시컬 스코프를 따른다. 
2. 정의가 평가되는 시점에 상위 스코프가 정적으로 결정되기 때문에 정적 스코프라고 부른다. 
3. 스코프 체인 - 전역 스코프 안에 지역 스코프가 있고 지역 스코프가 있다 이 느낌을 더 자세하게 앎