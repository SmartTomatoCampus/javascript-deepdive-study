### Date
#### 표준 빌트인 객체인 Date는 날짜와 시간을 위한 메서드를 제공하는 빌트인 객체이면서 생성자 함수이다.
___

### Date 생성자 함수
##### Date 생성자 함수로 생성한 Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖는다.

#### new Date()
##### Date 생성자 함수를 인수 없이 new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환한다. Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖지만 Date 객체를 콘솔에 출력하면 기본적으로 날짜와 시간 정보를 출력한다.
```js
new Date(); // Mon Jul 06 2020 01:03:18 GMT+0900 (대한민국 표준시)
```
##### new 연산자 없이 호출하면 Date 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 문자열을 반환한다.
```js
Date(); // "Mon Jul 06 2020 01:03:18 GMT+0900 (대한민국 표준시)"
```
___

### new Date(milliseconds)
```js
new Date(0); // Thu Jul 06 2020 01:03:18 GMT+0900 (대한민국 표준시)
new Date(86400000); // Fri Jul 06 2020 01:03:18 GMT+0900 (대한민국 표준시)
```
___
### new Date(dataString)
##### Date 생성자 함수에 날짜와 시간을 나타내는 문자열을 인수에 전달하면 날짜와 시간을 나타내는 Date 객체를 반환한다.
```js
new Date('2020/03/26/10:00:00'); // Thu Mar 26 2020 10:00:00 GMT+0900
new Date('May 26, 2020 10:00:00') // Thu mar 26 2020 10:00:00 GMT+0900
```
##### new Date(year,month[,day,hour,minute,second,millisecond])순으로 인자를 전달할 수 있다.
```js
new Date(2020,2); // Sun Mar 01 2020 00:00:00 GMT+0900
new Date(2020,2,2,22,22) //Mon Mar 02 2020 22:22:00 GMT+0900 (한국 표준시)
```
___

### Date 메서드

#### Date.now
##### 1970년부터 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환한다.

```js
const now = Date.now() // 162344932419
```
#### Date.parse
##### 1970년부터 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.
```js
Date.parse('1970/01/02/09:00:00'); // 86400000
```
___
### Date.prototype.getFullYear/setFullYear
##### Date 객체의 연도를 나타내는 정수를 반환한다.
```js
new date('2020/07/24').getFullYear(); // 2020

const today = new Date();

today.setFullYear(2000);
today.getFullYear(); // 2000

today.setFullYear(1900,0,1);
today.getFullYear(); // 1900
```

##### 같은 형식으로 Month,Date,day,Hour,Minutes,Seconds,Milliseconds을 사용할 수 있다.
___
### Date.prototype.getTime()/setTime()
```js
new Date('2020/07/24/12:30').getTime(); // 1934934344
```
___

### Date를 활용한 시계 예제
```js
(function printNow() {
  const today = new Date();
  const dayNames = ['일','월','화','수','목','금','토'];
  
  //getDay 메서드는 해당 요일(0~6)을 나타내는 정수를 반환한다.
  const day = dayNames[today.getDay()];
  const year = today.getFullYear();
  const month = today.getMonth() + 1;
  const date = today.getDate();
  let hour = today.getHours();
  let minute = today.getMinutes();
  let second = today.getSeconds();
  const ampm = hour >= 12 ? 'PM':'AM';
  
  // 12시간제로 변경
  hour %= 12;
  hour = hour || 12; // hour가 0이면 12를 재할당
  
  //10 미만인 분과 초를 2자리로 변경
  minute = minute < 10 ? '0' + minute : minute;
  second = second < 10 ? '0' + second : second;
  
  const now = `${year}년${month}월${date}일${day}${hour}:${minute}:${second}`;
  
  console.log(now);
  // 1초마다 printNow 함수를 재귀 호출한다. 
  setTimeout(printNow,1000);
}());