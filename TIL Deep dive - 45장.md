### 프로미스의 생성

promise 생성자 함수를 new 연산자와 함께 호출하면 프로미스(Promise 객체)를 생성한다.

ES6에서 도입된 promise는 호스트 객체가 아닌 ECMAScript 사양에 정의된 **표준 빌트인 객체**다.

Promise 생성자 함수는 비동기 처리를 수행할 콜백 함수를 인수로 전달받는데 이 **콜백 함수는 resolve와 reject 함수를 인수로 전달받는다.**

```js
const promise = new Promise((resolve,reject) => {
  //
  if (  ){
  resolve('result');
}else{ 
	reject('failure reason');
}
});
```
Promise 생성자 함수가 인수로 전달받은 콜백 함수 내부에서 **비동기 처리를 수행한다.**

이때 비동기 처리가 성공하면 콜백 함수의 인수로 전달받은 resolve 함수를 호출하고, 비동기 처리가 실패하면 reject 함수를 호출한다.

앞에서 살펴본 비동기 함수 get을 프로미스를 사용해 다시 구현해 보자.

```js 
//GET 요청을 위한 비동기 함수 
const promiseGet = url => {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open('GET',url);
    xhr.send();
    
    xhr.onload = () => {
      if ( xhr.status === 200 ) {
        //성공적으로 응답을 전달받으면 resolve 함수를 호출한다.
        resolve(JSON.parse(xhr.response));
      } else {
        //에러 처리를 위해 reject 함수를 호출한다.
        reject(new Error(xhr.status));;
      }
    }
  });
};


//promiseGet 함수는 promise를 반환한다.
promiseGet('https://www.abc.def/post/1');
```

비동기 함수인 promiseGet은 함수 내부에서 프로미스를 생성하고 반환한다.

**비동기 처리는 Promise 생성자 함수가 인수로 전달받은 콜백 함수 내부에서 수행한다.** 

비동기 처리가 성공하면 비동기 처리 결과를 resolve 함수에 인수로 전달하면서 호출하고, 비동기 처리가 실패하면 에러를 reject 함수에 인수로 전달하면서 호출한다.

> ##### resolve 함수에 인수로 전달하면서 호출?
 ```js
reselve((res) => console.log(res));
reject((err) => console.error(err));
```
##### res,err에 비동기 처리 결과가 담긴다는 의미.

___


#### 프로미스의 상태

생성된 직후의 프로미스는 기본적으로 pending 상태다.
이후 비동기 처리가 수행되면 비동기 처리 결과에 따라 프로미스의 상태가 변경된다.

- ##### 비동기 처리 성공 : resolve 함수를 호출 해 프로미스를 fulfilled 상태로 변경한다
- ##### 비동기 처리 실패 : reject 함수를 호출 해 프로미스를 rejected 상태로 변경한다.

fulfilled 또는 rejected 상태를 **settled 상태**라고 한다. 

settled 상태는 fulfilled 또는 rejected 상태와 상관없이 pending이 아닌 상태로 비동기 처리가 수행된 상태를 말한다.

프로미스는 pending 상태에서 fulfilled 또는 rejected 상태, 즉 settled 상태로 변화할 수 있다.
하지만 일단 settled 상태가 되면 더는 다른 상태로 변화할 수 없다.

** 프로미스는 비동기 처리 상태와 더불어 비동기 처리 결과도 상태로 갖는다.**

비동기 처리가 실패하면 프로미스는 pending 상태에서 rejected 상태로 변화한다.
그리고 비동기 처리 결과인 Error객체를 값으로 갖는다.

**즉 프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체다.**

### 프로미스의 후속 처리 메서드

프로미스의 비동기 처리 상태가 변화하면 이에 따른 후속 처리를 해야 한다.

예를 들어, 프로미스가 fulfilled 상태가 되면 프로미스의 처리 결과를 가지고 무언가를 해야 하고, 프로미스가 rejected 상태가 되면 프로미스의 처리 결과를 가지고 에러 처리를 해야한다.

이를 위해 프로미스는 **후속 메서드 then,catch finally를 제공한다.**

프로미스의 비동기 처리 상태가 변화하면 후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출 된다.

이때 후속 처리 메서드의 콜백 함수에 프로미스의 처리 결과가 인수로 전달된다.

모든 후속 처리 메서드는 프로미스를 반환하며 비동기로 동작한다.

프로미스의 후속 처리 메서드는 다음과 같다
___

### Promise.prototype.then

then 메서드는 두 개의 콜백 함수를 인수로 전달받는다.

- 첫 번째 콜백 함수는 프로미스가 fulfilled 상태(resolve 함수가 호출된 상태)가 되면 호출된다. 이때 콜백 함수는 프로미스의 비동기 처리 결과를 인수로 전달받는다.
- 두 번째 콜백 함수는 프로미스가 rejected 상태(reject 함수가 호출된 상태)가 되면 호출된다. 이때 콜백 함수는 프로미스의 에러를 인수로 전달 받는다.

```js
//fulfilled
new Promise(resolve => resolve('fulfilled'))
.then( v => console.log(v), e => console.log(e)); // fulfilled

//rejected

new Promise ((_, reject) => reject(new Error('rejected')))
.then(v => console.log(v), e => console.error(e)); // Error: rejected
```

** then 메서드는 언제나 프로미스를 반환한다. **
then 메서드의 콜백 함수가 프로미스를 반환하면 그 프로미스를 그대로 반환하고 콜백 함수가 프로미스가 아닌 값을 반환하면 그 값을 암묵적으로 resolve 또는 reject하여 프로미스를 생성해 반환한다.

> ##### 프로미스가 아닌 값을 반환한다?
인수로 받은 결과를 조작하지 않고 다른 값으로 resolve,reject를 구분할 수도 있다.

___

### Promise.prototype.catch
catch 메서드는 한 개의 콜백 함수를 인수로 전달받는다. catch 메서드의 콜백 함수는 프로미스가 rejected 상태인 경우만 호출된다.
```js
new Promise((_, reject) => reject(new Error('rejected')))
.catch(e => console.log(e)); // Error : rejected
```

catch 메서드는 then(undefined, onRejected)과 동일하게 동작한다.
따라서 then 메서드와 마찬가지로 언제나 프로미스를 반환한다.
```js
new Promise((_, reject) => reject(new Error('rejected')))
.then(undefined, e => console.log(e)); // Error: rejected
```
___

### Promise.prototype.finally

finally 메서드는 한개의 콜백 함수를 인수로 전달 받으며 성공과 실패에 관계없이 무조건 한 번 호출된다. finally 메서드는 프로미스의 상태와 상관없이 공통적으로 수행해야 할 처리 내용이 있을 때 유용하다.
finally 메서드도 then/catch 메서드와 마찬가지로 언제나 프로미스를 반환한다. 

```js
new Promise(() => {})
.finally(() => console.log('finally')); // finally
```

___

### 프로미스의 에러 처리

```js

const wrongURL = 'https://wqeq.qwe.rqw/2323/1';

//부적절한 URL의 지정으로 에러 발생
promiseGet(wrongUrl).then(
  res => console.log(res),
  err => console.error(err)
  ); // Error: 404
```
비동기 처리에서 발생한 에러는 프로미스의 후속 처리 메서드 catch를 사용해 처리할 수도 있다.

```js
promiseGet(wrongUrl)
.then(res => console.log(res));
.catch(err => console.error(err)); // Error: 404
```

catch 메서드를 호출하면 내부적으로 then(undefined, onRejected)을 호출한다
따라서 위 예제는 내부적으로 다음과 같이 처리된다.

```js

promiseGet(wrongUrl)
.then(res => console.log(res))
.then(undefined, err => console.error(err)); // Error : 404

```
단, then 메서드의 두 번쨰 콜백 함수는 ㅓㅅ 번쨰 콜백 함수에서 발생한 에러를 캐치하지 못하고 코드가 복잡해져서 가독성이 좋지 않다.
catch 메서드를 모든 then 메서드를 호출한 이후에 호출하면 비동기 처리에서 발생한 에러 뿐만 아니라 then 메서드 내부에서 발생한 에러까지 모두 캐치할 수 있다.

또한 then 메서드에 두 번쨰 콜백 함수를 전달하는 것보다 catch 메서드를 사용하는 것이 가독성이 좋고 명확하다.

___

### 마이크로 태스크 큐

```js
setTimeout(() => console.log(1), 0);

promise.resolve()
.then(() => console.log(2));
.then(() => console.log(3));
```
프로미스의 후속 처리 메서드도 비동기로 동작하므로 1 2 3 순으로 출력될 것처럼 보이지만 2 3 1의 순으로 출력된다.
그 이유는 프로미스의 후속 처리 메서드의 콜백 함수는 태스크 큐가 아니라 마이크로 테스크 큐에 저장되기 때문이다.

마이크로 테스크 큐는 태스크큐와는 별도의 큐다
마이크로 테스크 큐에는 프로미스의 후속 처리 메서드의 콜백 함수가 일시 저장된다. 그 외의 비동기 함수의 콜백 함수나 이벤트 핸들러는 태스크 큐에 일시 저장된다.
콜백 함수나 이벤트 핸들러를 일시 저장한다는 점에서 태스크 큐와 동일하지만 마이크로태스크 큐는 태스크 큐보다 우선순위가 높다. 즉 이벤트 루프는 콜 스택이 비면 먼저 마이크로태스크 큐에서 대기하고 있는 함수를 가져와 실행한다. 이후 마이크로 태스크 큐가 비면 태스크 큐에서 대기하고 있는 함수를 가져와 실행한다.

___

### fetch

fetch 함수는 XMLHttpRequest 객체와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API다.
XMLHttpRequest 객체보다 사용법이 간단하고 프로미스를 지원하기 때문에
비동기 처리를 위한 콜백 패턴의 단점에서 자유롭다.
```js
const promise = fetch(url,option)
```

fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체를 반환한다.

fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 프로미스를 반환하므로 후속 처리 메서드 then을 통해 resolve한 Response 객체를 전달받을 수 있다. Response 객체는 HTTP 응답을 나타내는 다양한 프로퍼티를 제공한다.

예를 들어 Response.prototype.json 메서드는 Json.parse와 같이 응답 몸체를 역직렬화한다.

#### Post 요청

```js
request.post('https://some/'), {
  userId: 1,
  title: 'Javascript',
  completed: false
}).then(response => {
  if(!response.ok) throw new Error(response.statusText);
  return response.json();
})
.then(todos => console.log(todos))
.catch(err => console.error(err));
// { userId: 1. title: "Javascript". completed: false, id:201 }
```

#### PATCH 요청
```js
request.patch('https://some/'), {
  completed: true
}).then(response => {
  if(!response.ok) throw new Error(response.statusText);
  return response.json();
})
.then(todos => console.log(todos))
.catch(err => console.error(err));
// { userId:1, id:1, title:"delectus aut autom", completed:true }
```

#### DELETE 요청
```js
request.delete('https://some/')
.then(response => {
  if(!response.ok) throw new Error(response.statusText);
  return response.json();
})
.then(todos => console.log(todos))
.catch(err => console.error(err));