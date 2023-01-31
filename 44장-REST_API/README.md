# 44장 REST API

## REST API란?

- REST는 무엇인가?
  - REpresentational State Transfer의 줄임말.
  - HTTP/1.0 과 1.1 스펙 작성 및 아파치 HTTP 서버 프로젝트의 공동 설립자 로이 필딩의 논문에서 처음 소개되었다.
  - HTTP 프로토콜의 장점을 최대한 활용할 수 있는 아키텍쳐로 소개되어 HTTP 프로토콜을 의도에 맞게 디자인하도록 유도하고 있다.
- 엄격한 의미 vs 간단한 의미
  - 엄격한 의미  
    네트워크 아키텍쳐 원리의 모음, 즉 자원을 정의하고 자원에 대한 주소를 지정하는 방법 전반을 의미한다.
  - 간단한 의미  
    웹 상의 자료를 HTTP 위에서 별도의 전송계층 없이 전송하기 위한 아주 간단한 인터페이스
    - SOAP(Simple Object Access Protocol : HTTP, HTTPS 등을 통해 XML 기반 메세지를 교환하는 프로토콜) 이나 쿠키를 통한 세션 트랙킹 같은 전송계층 것들 없이 통신
- RESTFul과 REST API는 무엇인가?

  - REST의 기본 원칙을 성실히 지킨 서비스 다지인을 RESTful하다고 표현한다.
  - REST를 기반으로 서비스 API를 구현한 것을 의미한다.

> REST는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍쳐고, REST API는 REST를 기반으로 서비스 API를 구현한 것을 의미한다.

## 44.1 REST API의 구성

> REST API는 자원(resource), 행위(verb), 표현(representation)의 세가지 요소로 구성된다.
> REST는 자체 표현 구조로 구성되어 REST API 만으로 HTTP 요청의 내용을 이해할 수 있다.

| 구성요소 | 내용                           | 표현방법         |
| -------- | ------------------------------ | ---------------- |
| 자원     | 자원                           | URI(엔드포인트)  |
| 행위     | 자원에 대한 행위               | HTTP 요청 메서드 |
| 표현     | 자원에 대한 행위의 구체적 내용 | 페이로드         |

- 페이로드 : 큰 데이터 중에 '흥미있는' 데이터를 구별하는 데 사용.
  - 프로그래밍에서 주로 메세지 프로토콜 중에 프로토콜 오버헤드와 원하는 데이터(즉 페이로드)를 구분할 때 사용한다
  - 예시 - JSON 형식의 웹 서비스 응답
  ```json
  {
    "status": "OK",
    "data": {
      "message": "Hello, world!"
    }
  }
  ```
  여기에서 "Hello, world!"가 클라이언트가 관심을 가지는 페이로드이고, 나머지 부분은 중요하긴 하지만 프로토콜 오버헤드이다.  
   출처 : [wiki]https://ko.wikipedia.org/wiki/%ED%8E%98%EC%9D%B4%EB%A1%9C%EB%93%9C_(%EC%BB%B4%ED%93%A8%ED%8C%85)

## 44.2 REST API 설계 원칙

REST 에서 가장 중요한 기본직인 원칙은 두가지이다!!

    1. URI는 리소스를 표현하는데 집중한다.
    2. HTTP 요청 메서드를 통해 행위를 정의한다.

1. URI는 리소스를 표현해야 한다.

```
#bad
GET /getTodos/1    - (1)번 예시
GET /todos/show/1  - (2)번 예시

# good
GET /todos/1

```

URI는 리소스를 표현하는데에 중점을 두어야 한다.

- (1)번 예시 -> 예시처럼 get 같은 행위에 대한 표현이 들어가서는 안된다
- (2)번 예시 -> 리소스를 식별할 수 있는 이름은 동사보다는 명사를 사용한다.

2.  리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.

HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적(리소스에 대한 행위)을 알리는 방법이다.
|HTTP 요청 메서드| 종류| 목적| 페이로드|
|--|--|--|--|
|GET|index / retrieve | 모든 특정 리소스 취득 | X|
|POST|create|리소스 생성| O|
|PUT|replace|리소스의 전체 교체| O|
|PATCH|modify|리소스의 일부 수정|O|
|DELETE|delete|모든 / 특정 리소스 삭제|X|

```
  # bad
  GET /todos/delete/1 - (1)번 예시

  # good
  DELETE /todos/1

```

리소스에 대한 행위는 HTTP 요청 메서드를 통해 표현하며, URI에 표현하지 않는다.

- (1)번 예시 - GET은 리소스를 취득할 때 사용하는 메서드이다. 삭제 요청을 하려면 DELETE 메서드를 사용한다.

## 44.3 REST API 실습

### 44.3.1 ~ 44.3.3 JSON Server환경 세팅 (생략)

- 다음 JSON 파일 데이터를 기준으로 todo에 대한 데이터를 CRUD 할 수 있는 서버라고 생각해보자.

```json
{
  "todos": [
    {
      "id": 1,
      "content": "JS",
      "completed": true
    },
    {
      "id": 2,
      "content": "React",
      "completed": false
    },
    {
      "id": 1,
      "content": "FrontEnd",
      "completed": false
    }
  ]
}
```

### 44.3.4 GET 요청

- 모든 todos 리소스를 취득 할 때

  ```javascript
  const xhr = new XMLHttpRequest();

  /**G
   * 1. 행위는 메서드로 GET 요청으로 취득하겠다
   * 2. 리소스는 URI로 표현, todos 리소스를 취득하겠다(index)
   */
  xhr.open("GET", "/todos");

  xhr.send();

  xhr.onload = () => {
    if (xhr.status === 200) {
      console.log(xhr.response);
    } else {
      console.error("Error", xhr.status, xhr.statusText);
    }
  };
  ```

  - response
    ```json
    {
      "todos": [
        {
          "id": 1,
          "content": "JS",
          "completed": true
        },
        {
          "id": 2,
          "content": "React",
          "completed": false
        },
        {
          "id": 3,
          "content": "FrontEnd",
          "completed": false
        }
      ]
    }
    ```

- 특정 todos 리소스를 취득

  ```javascript
  const xhr = new XMLHttpRequest();

  /**
   * 1. 행위는 메서드로 GET 요청으로 취득하겠다
   * 2. 리소스는 URI로 표현, todos/1 로 id:1인 todo 리소스를 취득하겠다
   *
   * todos 리소스에서 특정 todo를 취득(retrieve)
   */
  xhr.open("GET", "/todos/1");

  xhr.send();

  xhr.onload = () => {
    if (xhr.status === 200) {
      console.log(xhr.response);
    } else {
      console.error("Error", xhr.status, xhr.statusText);
    }
  };
  ```

  - response

    ```json
    {
      "todos": [
        {
          "id": 1,
          "content": "JS",
          "completed": true
        }
      ]
    }
    ```

### 44.3.5 POST 요청

```javascript
const xhr = new XMLHttpRequest();

/**
 * 1. 행위는 메서드로 POST 요청으로 생성하겠다
 * 2. 리소스는 URI로 표현, 새로운 todo를 생성하겠다
 */
xhr.open("POST", "/todos");

// 요청 몸체에 담아 서버로 보낼 페이로드의 MIME 타입 지정
xhr.setRequestHeader("Content-Type", "application/json");

// 페이로드를 담아서 서버로 전송
xhr.send(JSON.stringify({ id: "4", content: "Angular", completed: false }));

xhr.onload = () => {
  if (xhr.status === 200) {
    console.log(xhr.response);
  } else {
    console.error("Error", xhr.status, xhr.statusText);
  }
};
```

- response
  ```json
  {
    "todos": [
      {
        "id": 1,
        "content": "JS",
        "completed": true
      },
      {
        "id": 2,
        "content": "React",
        "completed": false
      },
      {
        "id": 3,
        "content": "FrontEnd",
        "completed": false
      },
      {
        "id": 4,
        "content": "Angular",
        "completed": false
      }
    ]
  }
  ```

### 44.3.6 PUT 요청

```javascript
const xhr = new XMLHttpRequest();

/**
 * 1. 행위는 메서드로 PUT 요청으로 리소스전체를 교체하겠다
 * 2. 리소스는 URI로 표현, id로 todo를 특정하여 id를 제외한 리소스 전체를 교체
 */
xhr.open("PUT", "/todos/4");

// 요청 몸체에 담아 서버로 보낼 페이로드의 MIME 타입 지정
xhr.setRequestHeader("Content-Type", "application/json");

// 페이로드를 담아서 서버로 전송
xhr.send(JSON.stringify({ id: "4", content: "Svelte", completed: false }));

xhr.onload = () => {
  if (xhr.status === 200) {
    console.log(xhr.response);
  } else {
    console.error("Error", xhr.status, xhr.statusText);
  }
};
```

- response
  ```json
  {
    "todos": [
      {
        "id": 4,
        "content": "Svelte",
        "completed": false
      }
    ]
  }
  ```

### 44.3.7 PATCH 요청

```javascript
const xhr = new XMLHttpRequest();

/**
 * 1. 행위는 메서드로, PATCH 요청으로 리소스전체를 교체하겠다
 * 2. 리소스는 URI로 표현, id로 todo를 특정하여 원하는 부분만 교체
 */
xhr.open("PATCH", "/todos/2");

// 요청 몸체에 담아 서버로 보낼 페이로드의 MIME 타입 지정
xhr.setRequestHeader("Content-Type", "application/json");

// 페이로드를 담아서 서버로 전송
xhr.send(JSON.stringify({ completed: true }));

xhr.onload = () => {
  if (xhr.status === 200) {
    console.log(xhr.response);
  } else {
    console.error("Error", xhr.status, xhr.statusText);
  }
};
```

- response

  ```json
  {
    "todos": [
      {
        "id": 2,
        "content": "React",
        "completed": true
      }
    ]
  }
  ```

### 44.3.8 DELETE 요청

```javascript
/**
 * 1. 행위는 메서드로 DELETE 요청으로 리소스를 삭제하겠다
 * 2. 리소스는 URI로 표현, id로 todo를 특정하여 해당 todo 삭제
 */
xhr.open("PATCH", "/todos/4");

// 페이로드를 담아서 서버로 전송
xhr.send(JSON.stringify({ completed: true }));

xhr.onload = () => {
  if (xhr.status === 200) {
    console.log(xhr.response);
  } else {
    console.error("Error", xhr.status, xhr.statusText);
  }
};
```

- response

  ```json
  {}
  ```
