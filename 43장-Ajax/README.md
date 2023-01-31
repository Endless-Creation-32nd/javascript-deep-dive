# 43장 Ajax

## 43.1 Ajax란?

`Ajax(Asynchronous JavaScript and Xml)`이란

- 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹 페이지를 동적으로 갱신하는 프로그래밍 방식이다.
- Ajax는 브라우저에서 제공하는 WEB API인 XMLHttpRequest 객체를 기반으로 동작한다. XMLHttpRequest는 HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공한다.
- 변경이 필요한 부분만 한정적으로 렌더링 하는 방식이 가능해졌다.

![Traditional](https://user-images.githubusercontent.com/108210492/212834544-dec0e54a-7312-4088-a198-ff2215a6add2.png)

![Ajax](https://user-images.githubusercontent.com/108210492/212834540-e8370f58-7708-4ce6-b491-b54a804a0f35.png)

전통적 방식과 비교했을 때의 Ajax 장점

- 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않는다.
- 변경할 필요가 없는 부분은 다시 렌더링하지 않는다. 따라서 화면이 순간적으로 깜빡이는 현상이 발생하지 않는다.
- 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청보낸 이후 블로킹이 발생하지 않는다.

## 43.2 JSON

JSON(JavaScript Object Notation)은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다. JS에 종속되지 않는 언어 독립형 데이터 포맷이기때문에 대부분의 프로그래밍 언어에서 사용할 수 있다.

### 43.2.1 JSON 표기 방식

JSON은 자바스크립트의 객체 리터럴과 유사하게 키와 값으로 구성된 순수 텍스트다.

```json
{
  "name": "Lee",
  "age": 24,
  "alive": true,
  "hobby": ["traveling", "tennis"]
}
```

JSON의 키는 반드시 큰따옴표로 묶어야 한다. 값은 객체 리터럴과 같은 표기법을 그대로 사용할 수 있다. 하지만 문자열은 반드시 큰따옴표로 묶어야한다. 작은 따옴표는 사용이 불가하다.

### 43.2.2 JSON.stringify

**JSON.stringify** 메서드는 `객체를 JSON 포맷의 문자열로 변환`한다. 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화 해야 하는데 이를 직렬화(serializing)이라 한다.

```cpp
// value : JSON을 문자열로 반환할 값, replacer : 문자열화 동작 방식을 변경하는 함수,
// 혹은 JSON 문자열에 포함될 값 객체의 속성들을 서낵하기 위한 화이트리스트로 쓰이는 String, Number 객체들의 배열
// 이 값이 null 이거나 제공되지 않으면, 객체의 모든 속성들이 JSON 문자열 결과에 포함된다.
// space : 가독성을 위한 공백
JSON.stringify(value[, replacer[, space]])
```

### 43.2.3 JSON.parse

**JSON.parse** 메서드는 JSON 포맷의 `문자열을 객체로 변환`한다.

- 서버 → 클라이언트로 전송된 JSON데이터는 문자열
- 이 문자열을 객체로 사용하려면 객체화(역직렬화) 해야된다.
- `JSON`에 문자열로 반환되어 있는 경우, `JSON.parse`는 **문자열 → 배열 객체**로 변환

```cpp
const obj = {
    name:"Lee",
    age: 24,
    alive: true,
    hobby:["traveling", "tennis"]
}

const json = JSON.stringify(obj); // 객체 -> JSON 포맷의 문자열
const parsed = JSON.parse(json); // JSON 포맷의 문자열 -> 객체

console.log(typeof parsed, parsed); //object {name:"Lee", age: 24, alive: true, hobby:["traveling", "tennis"]}
```

<aside>
💡 배열이 JSON 포맷의 문자열로 변환되어 있는 경우 JSON.parse는 문자열을 배열 객체로 변환한다. 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환한다.

</aside>

```cpp
const todos = [
	{id : 1, content : 'HTML', completed: false},
	{id : 2, content : 'CSS', completed: true},
	{id : 3, content : 'JS', completed: false},
]

const json = JSON.stringify(todos); // 배열 -> JSON 포맷의 문자열로 변환
const parsed = JSON.stringify(json); // JSON 포맷의 문자열을 배열로 변환, 배열의 요소까지 객체로 변환
```

## 43.3 XMLHttpRequest

브라우저는 주소창이나 HTML의 form 태그 또는 a 태그를 통해 HTTP 요청 전송 기능을 기본 제공한다. 자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다.

### 43.3.1 XMLHttpRequest 객체 생성

- XMLHttpRequest 객체는 XMLHttpRequest 생성자 함수를 호출하여 생성한다.
- 이 객체는 브라우저에서 제공하는 Web API이므로 브라우저 환경에서만 정상 실행된다.

```tsx
const xhr = new XMLHttpRequest();
```

### 43.3.2 XMLHttpRequest 객체의 프로퍼티와 메서드

- XMLHttpRequest는 다양한 프로퍼티와 메서드를 제공한다.
- 대표적인 프로퍼티와 메서드는 다음과 같다.

**XMLHttpRequest 객체의 프로토타입 프로퍼티**

| 프로토타입 프로퍼티 | 설명                                                                                             |
| ------------------- | ------------------------------------------------------------------------------------------------ |
| readyState          | HTTP 요청의 현재 상태를 나사태는 정수. 다음과 같은 정적 프로퍼티를 값으로 갖는다. (맨 밑에 나옴) |
| status              | HTTP 요청에 대한 응답 상태를 나타내는 정수 EX) 200                                               |
| statusText          | HTTP 요청에 대한 응답 메시지를 나타내는 문자열 EX) “OK”                                          |
| responseType        | HTTP 응답 타입 EX) document, json                                                                |
| response            | HTTP 요청에 대한 응답 몸체, responseType에 따라 타입이 다르다.                                   |

**XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티**

| 이벤트 핸들러 프로퍼티 | 설명                                 |
| ---------------------- | ------------------------------------ |
| onreadystatechange     | readyState프로퍼티 값이 변경 된 경우 |
| onerror                | HTTP 요청에 에러가 발생한 경우       |
| onload                 | HTTP 요청이 성공적으로 완료한 경우   |

**XMLHttpRequest 객체의 메서드**

| 메서드           | 설명                            |
| ---------------- | ------------------------------- |
| open             | HTTP 요청 초기화                |
| send             | HTTP 요청 전송                  |
| abort            | 이미 전송된 요청 중단           |
| setRequestHeader | 특정 HTTP 요청 헤더의 값을 설정 |

**XMLHttpRequest 객체의 정적 프로퍼티**

| 정적 프로퍼티    | 값  | 설명                  |
| ---------------- | --- | --------------------- |
| UNSENT           | 0   | open 메서드 호출 이전 |
| OPEND            | 1   | open 메서드 호출 이후 |
| HEADERS_RECEIVED | 2   | send 메서드 호출 이후 |
| LOADING          | 3   | 서버 응답 중          |
| DONE             | 4   | 서버 응답 완료        |

### 43.3.3 HTTP 요청 전송

HTTP 요청을 전송하는 경우 다음 순서를 따른다.

1. XMLHttpRequestp.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정한다.
2. 필요에 따라서 XMLHttpRequest.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정한다
3. XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송한다.

```tsx
const xhr = new XMLHttpRequest(); // 객체 생성
xhr.open("GET", "/user"); // HTTP 요청 초기화
xhr.setRequestHeader("content-type", "application/json"); // HTTP 요청헤더 설정, 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정 : json
xhr.send(); // HTTP 요청 전송
```

- [**XMLHttpRequest.prototype.open**](http://XMLHttpRequest.prototype.open)
  open 메서드는 서버에 전송할 HTTP 요청을 초기화한다. open 메서드를 호출하는 방법은 다음과 같다
  ```tsx
  xhr.open(method, url[, async])
  ```
  | 매개변수 | 설명                                                                    |
  | -------- | ----------------------------------------------------------------------- |
  | method   | HTTP 요청 메서드 (”GET”, “POST”, “PUT”, “DELETE”등 )                    |
  | url      | HTTP 요청을 전송할 URL                                                  |
  | async    | 비동기 요청 여부, 옵션으로 기본 값은 true이며 비동기 방식으로 동작한다. |
- 주로 5가지 요청 메서드를 이용하여 CRUD를 구현한다.

| HTTP 요청 메서드 | 종류           | 목적                  | 페이로드 (전송 될 데이터 유무) |
| ---------------- | -------------- | --------------------- | ------------------------------ |
| GET              | index/retrieve | 모든/특정 리소스 취득 | X                              |
| POST             | create         | 리소스 생성           | O                              |
| PUT              | replace        | 리소스 전체 교체      | O                              |
| PATCH            | modify         | 리소스 일부 수정      | O                              |
| DELETE           | delete         | 모든/특정 리소스 삭제 | X                              |

- **XMLHttpRequest.prototye.send**

  - send 메서드는 open 메서드로 초기화된 HTTP 요청을 서버에 전송한다. 기본적으로 서버로 전송할 때는 GET, POST 요청 메서드에 따라 전송 방식에 차이가 있다.
  - GET 요청 데이터를 URL의 일부인 쿼리 문자열로 서버에 전송한다.
  - POST 요청 메서드의 경우 데이터를 요청 몸체에 담아 전송한다.
  - send 메서드에는 전송할 데이터를 인수로 전달할 수 있다. 페이로드가 객체인 경우 반드시 JSON.stringfy 메서드를 사용해 직렬화한 다음 전달해야한다.

  ```tsx
  xhr.send(JSON.stringify({ id: 1, content: "HTML", completed: false }));
  ```

- **XMLHttpRequest.prototype.setRequestHeader**
  - setRequestHeader 메서드는 특정 HTTP 요청의 헤더 값을 설정한다.
  - 반드시 open 메서드를 호출한 이후에 호출해야 한다.
  - Content-type은 요청 몸체에 담아 전송할 데이터의 MIME 타입의 정보를 표현한다.
    | MIME 타입 | 서브타입 |
    | ----------- | -------------------------------------------------- |
    | text | text/plain, text/html, text/css, text/javascript |
    | application | application/json, application/x-www-form-urlencode |
    | multipart | multipart/form-data |

### 43.3.4 HTTP 응답 처리

- 서버가 전송한 응답을 처리하려면 XMLHttpRequest 객체가 발생 시키는 이벤트를 캐치해야 한다.
- 이 이벤트 핸들러 프로퍼티 중, HTTP 요청이 현재 상태를 나타내는 readyState 프로퍼티 값이 변경된 경우 발생하는 readystatechange 이벤트를 캐치하여 다음과 같이 응답을 처리할 수 있다.

  ```tsx
  const xhr = new XMLHttpRequest();

  // HTTP 요청 초기화
  // Fake REST API 제공 서비스이다
  xhr.open('GET', "https://jsonplaceholder.typicode.com/todos/1');

  // HTTP 요청 전송
  xhr.send();

  // 현재 상태를 나타내는 readyState 프로퍼티가 변경될 때마다 이벤트 발생
  // XMLHttpRequest.DONE == 4
  xhr.onreadystatechange = () => {
  	if(xhr.readyState !== XMLHttpRequest.DONE) return;
  	if(xhr.status === 200) {
  		console.log(JSON.parse(xhr.response));
  	} else{
  		console.error('Error', xhr.status, xhr.statusText);
  	}
  }
  ```
