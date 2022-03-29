# 발표문 - Number, Math, Date

# 애한님

1. Number, Math, Date 메소드들에 대해 학습
2. 각각의 메소드는 그때그때 찾아서보자
3. Number는 소수점단위 계산이 잘 안되는데 Number.EPSILON를 이용해 비교가능하다.
4. Math메소드는 외워두면 편하다.(자주 쓰임)
5. Date에서 month는 0부터 시작값을 갖는다. (0 = 1월)

// 예전에 달력을 구현해봤는데 PTSD가 왔다.

# 초생님

- 빌트인 전역 함수 isNaN과는 차이가 있다.
- isNaN 빌트인 함수는 전달받은 인수를 타입 변환하여 검사를 수행하지만 Number 메서드는 암묵적으로 타입 변환하지 않는다.

# 너두님

### 알게된것 number

- 무수히 많은 메서드가 있었다

### 모르겠는것

- 무한대를 판별할 일이 얼마나 있을까
- Exponential 언제쓰이는 걸까요

### 알게된것 math

- 모든 연산은 실수까지의 범위만 해당한다
- 랜덤은 0과 1사이의 숫자의 값이다(그래서 추가 연산이 필요했던것)

### 알게된것 date

- new Date자체가 현재시간이라 Date.now()를 찾을 필요가 없었다
- 너무많은 메서드가 있었지만 Date 수정할때 왜 수정이 안된지 알게되었음

# 꽁치님

1. 자바스크립트에서 부동소수점 연산은 이진법 변환의 구조적 한계로 인해 오차가 발생한다는 것을 알았다. 때문에 Number.EPSILON 으로 확인하는 것을 알았다.
2. 메서드 이름이 헷갈릴 때는 영어를 검색하자
- round : 둥글둥글, 어림재다, 대략 0이나 5로 숫자 뭉뚱그리기 -> 반올림하다 (round off)
- ceil: 천장 -> 올림
- floor: 바닥 -> 내림
1. Date 객체를 new 연산자 없이 호출하면 직빵으로 문자열로 나타나는 것을 처음 알았다. 지금까지 멍청하게 new Date().toString() 을 했는데 이제 더 효율적으로 써야겠다.

# 루피님

1. Number.prototype.toExpotential은 숫자를 지수 표기법으로 변환 한 뒤에 문자열로 반환한다.
2. Number.prototype.toFixed는 숫자를 반올림하여 문자열로 반환한다.
3. Date.prototype.getMonth는 월을 나타내는 0~11의 정수를 반환한다.
4. Date.prototype.getDay는 0~6의 정수로 요일을 반환한다.
5. Date.prototype.toLocaleString은 전달받은 인수로 날자와 시간을 표현하는 문자열을 반환한다.

# 명성님

1. Number.prototype.toPrecision를 사용할 때 인수에 전체 자리수를 전달하면 유효한 자리수를 만들어준다는 것을 알게 되었습니다
2. Math.max와 Math.min을 통해 배열에서 가장 큰 값 또는 가장 작은 값을 알 수 있다는 것을 알게 되었읍니다.

# 뽀또님

## Number

### Number.EPSILON

1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같다.

### Number.MAX_VALUE

자바스크립트에서 표현할 수 있는 가장 큰 양수값이다.

### Number.MIN_VALUE

자바스크립트에서 표현할 수 있는 가장 작은 양수 값이다.

### Number.MAX_SAFE_INTEGER

자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값이다.

### Number.MIN_SAFE_INTEGER

자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값이다.

### Number.POSITIVE_INFINITY

양의 무한대를 나타내는 숫자값 Infinity와 같다.

### Number.NEGATIVE_INFINITY

음의 무한대를 나타내는 숫자값 -Infinity와 같다.

### Number.NaN

Not-a-Number을 나타내는 숫자값으로 window.NaN과 같다.

### Number.isFinite

정적 메서드는 인수로 전달된 숫자값이 정상적인 유한수 즉 , Infinity 또는 -Infinity가 아닌지 검사하여 그 결과를 불리언으로 반환한다.

### Number.isInterger

정적 메서드는 인수로 전달된 숫자값이 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.

### Number.isNaN

인수로 전달된 숫자값이 NaN인지 검사하여 그 결과를 불리언 값으로 반환한다.

### Number.isSafeInteger

안전한 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.

### Number.prototype.toExponential

숫자를 지수 표기법으로 변환하여 문자열로 반환하며, 10의 n승을 곱하는 형식으로 수를 나타내는 방식이다.

### Number.prototype.toFixed

숫자를 반올림하여 문자열로 반환한다.

### Number.prototype.toPrecision

인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환한다.

### Number.prototype.toString

숫자를 문자열로 변환하여 반환한다. 인수를 생략하면 기본값 10진법이 지정된다.

## Math

### Math.PI

Math.PI; // -> 3.141592653589793

### Math.abs

절대값 반환

### Math.round

소수점 반올림 반환

### Math.ceil

소수점 올림한 정수 반환

### Math.floor

소수점 내림 정수 반환

### Math.sqrt

숫자의 제곱근 반환

### Math.random

0 에서 1 미만의 실수를 랜덤으로 나타낸다.

### Math.max

전달 받은 인수중에서 가장 큰 수를 반환

### Math.min

전달 받은 인수중에서 가장 작은 수를 반환

## Date

### new Date()

new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 반환한다.

### new Date(milliseconde)

Date 생성자 함수에 숫자 타입의 밀리초를 인수로 전달하면 기준을 기점으로 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체 반환

### new Date(dateString)

Date 생성자 함수에 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체 반환

### Date.now

1970년 1월 1일 00:00:00(UTC)를 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환한다.

### Date.parse

기준을 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.

### Date.UTC

1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.

### Date.prototype.getFullYear

Date 객체의 연도를 나타내는 정수를 반환한다.

### Date.prototype.setFullYear

Date 객체에 연도를 나타내는 정수를 설정한다. 연도 이외에 옵션으로 월 일도 설정 할 수 있다.

### Date.prototype.getMonth

Date 객체의 월을 나타내는 0~ 11의 정수를 반환한다.

1월은 0 , 12월 11이다.

### Date.prototype.setMonth

Date 객체에 월을 나타내는 0 ~ 11의 정수를 설정한다. 1월은 0, 12월은 11이다. 월 이에외 옵션으로 일도 설정할 수 있다.

### Date.prototype.getDate

Date 객체의 날짜를 나타내는 정수를 반환한다.

### Date.prototype.setDate

Date 객체의 날짜를 나타내는 정수를 반환한다.

### Date.prototype.getDay

Date 객체의 요일을 나타내는 정수를 반환한다. 0은 일요일이며, 6은 토요일이다.

### Date.prototype.getHours

Date 객체의 시간 0 ~ 23 을 나타내는 정수를 반환한다.

### Date.prototype.setHours

Date 객체의 시간 0 ~ 23을 나타내는 정수를 설정한다.  시간 이외에 옵션으로 분, 초, 밀리초도 설정할 수 있다.

### Date.prototype.getMinutes

Date 객체의 분 0 ~59 을 나타내는 정수를 반환한다.

### Date.prototype.setMinutes

Date 객체의 분 0 ~ 59 을 나타내는 정수를 설정한다. 분 이외에 옵션으로 초, 밀리초도 설정할 수 있다.

### Date.prototype.getSeconds

Date 객체의 초 0 ~ 59를 나타내는 정수를 반환한다.

### Date.prototype.setSeconds

Date 객체의 초 0 ~ 59를 나타내는 정수를 설정한다. 초 이외에 옵션으로 밀리초도 설정할 수 있다.

### Date.prototype.getMilliseconds

Date 객체의 밀리초 0 ~ 999를 나타내는 정수를 반환한다.

### Date.prototype.setMilliseconds

Date 객체의 밀리초 0 ~ 999를 나타내는 정수를 설정한다.

### Date.prototype.getTime

1970년 1월 1일 00:00:00 UTC 를 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환한다.

### Date.prototype.setTime

Date 객체에 1970년 1월 1일 00:00:00 를 기점으로 경과된밀리초를 설정한다.

### Date.prototype.getTimezoneOffset

UTC와 Date 객체에 지정된 로컬 시간과의 차이를 분 단위로 반환한다.

### Date.prototype.toDateString

사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜를 반환한다.

### Date.prototype.toTimeString

사람이 읽을 수 있는 형식으로 Date 객체의 시간을 표현한 문자열을 반환한다.

### Date.prototype.toISOString

ISO8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.

### Date.prototype.toLocalString

인수로 전달한 로컬을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.

### Date.prototype.toLocaleTimeString

인수로 전달한 로컬을 기준으로 Date 객체의 시간을 표현한 문자열을 반환한다.
