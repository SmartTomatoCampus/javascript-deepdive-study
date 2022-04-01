# [JavaScript] Math 객체

- **Math.PI : 원주율 PI값을 반환해줌**
    
    ```jsx
    console.log(Math.PI) // 3.141592653589793
    ```
    
- **Math.abs() : 숫자의 절대값을 반환**
    1. 인자로 전달된 숫자의 절대값을 반환함
    2. 절대값은 반드시 0 또는 양수이어야 함
    
    ```jsx
    Math.abs(-1)            // 1
    Math.abs('-1')          // 1
    Math.abs('')            // 0
    Math.abs([])            // 0
    Math.abs(null)          // 0
    Math.abs(undefined)     // NaN
    Math.abs({})            // NaN
    Math.abs('string')      // NaN
    Math.abs()              // NaN
    ```
    
- **Math.round() : 인수로 전달된 숫자의 소수점 이하를 반올림함 정수를 반환**
    
    ```jsx
    Math.round(1.4)  // 1
    Math.round(1.6)  // 2
    Math.round(-1.4) // -1
    Math.round(-1.6) // -2
    Math.round(1)    // 1
    Math.round()     // NaN
    ```
    
- **Math.ceil() : 인수로 전달된 숫자의 소수점 이하를 올림한 정수를 반환**
    
    ```jsx
    Math.ceil(1.4)    // 2
    Math.ceil(1.6)    // 2
    Math.ceil(-1.4)   // -1
    Math.ceil(-1.6)   // -1
    Math.ceil(1)      // 1
    Math.ceil()       // NaN
    ```
    
- **Math.floor() : 인수로 전달된 숫자의 소수점 이하를 내림한 정수를 반환**
    
    ```jsx
    Math.floor(1.9)  // 1
    Math.floor(9,1)  // 9
    Math.floor(-1.9) // -2
    Math.floor(-9.1) // -10
    Math.floor(1)    // 1
    Math.floor()     // NaN
    ```
    
- **Math.sqrt() : 인수로 전달된 숫자의 제곱근을 반환**
    
    ```jsx
    Math.sqrt(9)   // 3
    Math.sqrt(-9)  // NaN
    Math.sqrt(2)   // 1.41421356...
    Math.sqrt(1)   // 1
    Math.sqrt(0)   // 0
    Math.sqrt()    // NaN
    ```
    
- **Math.random() : 임의의 난수(랜덤 숫자)를 반환**
    
    ⇒ Math.rondom 메서드가 반환한 난수에는 0에서 1 미만의 실수. 즉 0은 포함되지만 1은 포함되지 않음
    
    ```jsx
    Math.random()  // 0이상 1미만 랜덤 실수(0.84258294385928)
    ```
    
    ### 1에서 10 범위의 랜덤 정수 취득
    
    1. **Math.random**으로 0에서 1 미만의 랜덤 실수를 구한 뒤, **10을 곱해 0에서 10 미만의 실수를 구함**
    2. 0에서 10 미만의 랜덤 실수에 **1을 더해서 1에서 10 범위의 랜덤 실수를 구함**
    3. **Math.floor** 로 1에서 10 범위의 랜덤 실수의 소수점 이하를 떼어 버린 다음 **정수를 반환함**
    
    ```jsx
    const random = Math.floor( Math.random() *10 +1 )
    console.log(random) // 1에서 10 범위의 정수
    
    const random = Math.floor( Math.random() * 10 )
    console.log(random)  // 0에서 9 범위의 정수
    ```
    
- **Math.pow() : 인자를 거듭제곱한 결과 반환**
    1. 첫번째 인수를 밑(base)으로, 두번째 인수를 지수(exponent)로 거듭제곱한 결과를 반환함
    
    ```jsx
    Math.pow(2, 8)  // 256
    Math.pow(2, -1) // 0.5
    Math.pow(2)     // NaN
    ```
    
    Math.pow() 대신 ES7에서 도입된 지수연산자를 사용하면 가독성이 좋음
    
    ```jsx
    2 ** 2          // 4
    2 ** 2 ** 2     // 16
    Math.pow(Math.pow(2, 2), 2) // 16
    ```
    
- **Math.max() : 전달받은 인수 중 가장 큰 수를 반환**
    
    ```jsx
    Math.max(1)       // 1
    Math.max(1, 2)    // 2
    Math.max(1, 2, 3) // 3
    Math.max()        // -Infinity
    ```
    
    ### 배열 요소 중에서 최대값 취득
    
    ```jsx
    const arr = [1, 2, 3]
    
    Math.max.apply(null, arr)  // 3
    Math.max(...arr)           // 3
    ```
    
- **Math.min() : 전달받은 인수 중 가장 작은 수를 반환**
    
    ```jsx
    Math.min(1)          // 1
    Math.min(1, 2)       // 1
    Math.min(1, 2, 3)    // 1
    Math.min()           // Infinity
    
    // 배열 요소 중 최소값 구하기
    const arr = [1, 2, 3]
    
    Math.min.apply(null, arr)  // 1
    Math.min(...arr)           // 1
    ```