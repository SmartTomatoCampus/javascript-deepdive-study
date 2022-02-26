## 제어문

#### 제어문을 바르게 이해하는 것은 코딩 스킬에 많은 영향을 준다.특히 for문을 매우 중요하므로 확실히 이해하자.

---

#### 블록문

##### 자바스크립트에서 블록문은 하나의 실행 단위로 취급한다. 단독으로도 사용이 가능하나 일반적으로 제어문이나 함수를 정의할 때 사용한다. 블록문은 자체 종결성을 갖기에 문 끝에 세미콜론을 붙이지 않는다.

```js
{
  const foo = 10;
}
```

---

#### 조건문

##### if...else 문과 switch문이 있다. if...else문에서 else는 옵션이다. 여러번 조건을 붙이기 위해서는 else if를 사용한다. 만약 코드 블록 내의 문이 하나뿐이라면 중괄호를 생략할 수 있다.

---

##### if...else문

```js
let x = 2;
let kind;
if (num > 0) kind = '양수';
else kind = '음수';
```

##### 대부분의 if...else문은 삼항연산자로 바꿔 쓸 수 있는데, 경우의 수가 3가지라면 삼항연산자를 다음과 같이 사용 가능하다.

```js
const num = 2;
const kind = num ? (num > 0 ? '양수' : '음수') : '영';

console.log(kind); // output : 양수


위의 (num > 0 ? '양수' : '음수')는 표현식이다.
즉 삼항연산자는 값으로 평가되는 표현식을 만든다.
따라서 삼항 조건 연산자 표현식은 값처럼 사용할 수 있기에 변수에 할당할 수 있다.
하지만 if...else는 문이기 때문에 값으로 할당할 수 없다.
```

#### switch문

##### switch문은 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case문으로 실행 흐름을 옮긴다.

##### switch문의 표현식과 일치하는 case문이 없다면 실행 순서는 default문으로 이동한다. default 문은 선택사항이다. if...else문은 불리언 값으로 평가되지만, switch문은 다양한 상황(case)에 따라 실행할 코드 블록을 결정할 때 사용한다.

#### switch문의 fall through(폴 스루)

##### switch문의 case문에서 break;를 선언하지 않는다면 switch문을 탈출하지 않고 switch문이 끝날 때까지 이후의 모든 case 문과 default문을 실행하게 되는데, 이런 흐름을 fall through라고 한다.

---

#### fall through의 활용

```js
const year = 2000;
const month = 2;
const day = 0;

switch (month) {
  case 1: case 3: case 5: case 7: case:8 case 10: case 12:
    day = 31;
    break;
  case 4: case 6: case 9: case 11:
    day = 30;
    break;
  case 2:
    days = ((year % 4 === 0 && year % 100 ! == 0) || (year % 400 === 0)) ? 29 : 28;
    break;
    default :
    console.log('Invaild month')
}

console.log(days); // output : 29
```

###### switch문은 다양한 키워드와 폴스루 등 문법이 복잡하기에 if...else 문으로 해결할 수 있다면 switch문보다 if...else문을 사용하는 것이 좋다. 조건의 복잡성을 보고 어떤 조건문을 활용할 수 있는지 능력을 기르자.

---

#### 반복문

##### 반복문은 조건식의 평과 결과가 참인 경우 코드블록을 실행하고, 그 후 조건식을 다시 평가하여 실행 여부를 결정한다.

##### 반복문은 for, while문, do .. while문을 제공한다.

---

###### 반복문을 알아보기 이전에 대체할 수 있는 다양한 기능들이 있는데, 자주 쓰이는 메서드들로 알아 두면 좋을것이다.

###### - forEach : 배열을 순회할 때 사용한다.

###### - for ...in : 객체의 프로퍼티를 열거할 때 사용한다.

###### - for ...of : 이터러블(반복 가능한 값)을 순회할 수 있다.

---

```js
function googoodan(num = 9) {
  for (let i = 1; i <= num; i++) {
    for (let j = 1; j <= num; j++) {
      console.log(i + 'X' + j + '=' + i * j);
    }
  }
}
googoodan();
```

**반복문은 조건식의 평과 결과가 참인 경우 코드블록을 실행하고, 그 후 조건식을 다시 평가하여 실행 여부를 결정한다.**
위 반복문의 정의를 상기하면 구구단 함수가 어떻게 작성되는지 어렵지 않게 느껴질 것이다.

---

#### while문

##### while문은 반복 횟수가 불명확할때 사용한다.

##### while 무한 루프를 방지하기 위해 if문으로 탈출 조건을 만들고, break문으로 코드 블록을 탈출한다.

```js
let count = 0;
while(true{
      console.log(count);
		count++;
	if(count === 3) break;
	}
```

---

#### break

##### break문은 레이블 문, 반복문, switch문의 코드 블록을 탈출한다. for문 안에 있는 if문으로 이루어진 제어문에 사용될 수 있다.

```js
let string = 'Hello Worild';
let search = 'l';
let index = "";


//문자열은 유사 배열이므로 for문으로 순회할 수 있다.
for(let i = 0; i < string.length; i+){

		if(string[i] === search){
  		index = i;
  			break;
}
}
console.log(index) // output : 2
```

##### 만약 코드블럭을 탈출하지 않기 원한다면, continue를 사용할 수 있다.
