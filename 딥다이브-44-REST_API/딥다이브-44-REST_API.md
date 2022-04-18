# [JavaScript] 44장 REST API

REST(REpresentational State Transfer)는 HTTP/1.0 과 1.1의 스펙 작성에 참여했고 아파치 HTTP 서버 프로젝트의 공동 설립자인 로이 필딩의 2000년 논문에서 처음 소개되었다. 발표 당시  웹이 HTTP 를 제대로 사용하지 못하고 있는 상황을 보고 HTTP 의 장점을 최대한 활용할 수 있는 아키텍처로서 REST 를 소개했고 이는 HTTP 프로토콜을 의도에 맞게 디자인하도록 유도하고 있다.
**REST 의 기본원칙을 성실히 지킨 서비스 디자인을 “RESTful” 이라고 표현한다.**

즉, REST 는 HTTP 를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처이며,
REST API 는 REST 를 기반으로 서비스 API 를 구현한 것을 의미한다.

# 44.1 REST API의 구성

REST API 는 자원(resource), 행위(verb), 표현(representations)의 3가지 요소로 구성된다.
REST 는 자체 표현 구조로 구성되어 REST API 만으로 HTTP 요청의 내용을 이해할 수 있다.

| 구성요소 | 내용 | 표현 방법 |
| --- | --- | --- |
| 자원 resource | 자원 | URI (엔드포인트) |
| 행위 verb | 자원에 대한 행위 | HTTP 요청 메서드 |
| 표현 representations | 자원에 대한 행위의 구체적 내용 | 페이로드 |

# 44.2 REST API 설계 원칙

REST 에서 가장 중요한 기본적인 원칙은 두 가지다. (RESTful API 를 설계하는 중심 규칙)

1. **URI 는 리소스를 표현하는데 집중**
2. **행위에 대한 정의는 HTTP 요청 메서드를 통해 정의한다.**

### 1. URI 는 리소스를 표현해야 한다.

URI 는 리소스를 표현하는 데 집중해야 하며, 리소스를 식별할 수 있는 이름은 동사보다는 명사를 사용한다.
따라서 이름에 get 같은 행위에 대한 표현이 들어가서는 안된다.

ex) bad : `GET /getTodos/1`

ex) good: `GET /todos/1`

### 2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.

HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적(리소스에 대한 행위)을 알리는 방법이다.
주로 5가지 요청 메서드를 사용하여 CRUD 를 구현한다.

| HTTP 요청 메서드 | 종류 | 목적 | 페이로드 |
| --- | --- | --- | --- |
| GET | index/retrieve | 모든/특정 리소스 취득 | X |
| POST | create | 리소스 생성 | O |
| PUT | replace | 리소스의 전체 교체 | O |
| PATCH | modify | 리소스의 일부 수정 | O |
| DELETE | delete | 모든/특정 리소스 삭제 | X |

리소스에 대한 행위는 HTTP 요청 메서드를 통해 표현하여 URI 에 표현하지 않는다.

ex) bad: `GET /todos/delete/1`

ex) good: `DELETE /todos/1`

# 44.3 JSON Server 를 이용한 REST API 실습

HTTP 요청을 전송하고 응답을 받으려면 서버가 필요하다.
JSON Server 를 사용해 가상 REST API 서버를 구축하여 HTTP 요청을 전송하고 응답을 받을 수 있다.

## 44.3.1 JSON Server 설치

---

JSON Server 는 json 파일을 사용하여 가상 REST API 서버를 구축할 수 있는 툴이다.
npm 을 이용하여 설치한다.

```bash
npm install json-server --save-dev
```

## 44.3.2 db.json 파일 생성

---

프로젝트 루트 폴더에 db.json 파일을 생성한다.
db.json 파일은 리소스를 제공하는 DB 역할을 한다.

## 44.3.3 JSON Server 실행

---

터미널에 다음과 같이 명령어를 JSON Server 를 실행한다.
JSON Server 가 DB 역할을 하는 db.json 파일의 변경을 감지하게 하려면 `watch` 옵션을 추가한다.

```bash
## 기본포트(3000)사용 / watch 옵션 적용
$ json-server --watch db.json
```

기본 포트는 3000이며, 포트를 변경하려면 `port` 옵션을 추가한다.

```bash
## 포트 변경 / watch dhqtus wjrdyd
$ json-server --watch db.json --port 5000
```

매번 위와 같이 명령어를 입력하는 것이 번거로우므로 package.json 에 scripts 를 수정한다.

```jsx
// pacakge.json
{
	// ...
	"scripts": {
		"start": "json-server --watch db.json"
	}
}
```

## 44.3.4 GET 요청

---

```jsx
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest()

// HTTP 요청 초기화
// todos 리소스에서 모든 todo 를 취득(index)
xhr.open('GET', '/todos')

// HTTP 요청 전송
xhr.send()

// load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
xhr.onload = () => {
	// status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
	if (xhr.status === 200) {
		document.querySelector('pre').textContent = xhr.response
	} else {
		console.error('Error', xhr.status, xhr.statusText)
	}
}
```