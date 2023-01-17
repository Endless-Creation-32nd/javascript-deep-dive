## 47.1 에러 처리의 필요성

에러가 발생하지 않는 코드를 작성하는 것은 불가능하다. 에러는 언제나 발생할 수 있는 데, 이런 에러를 잘 처리하지 않으면 프로그램이 강제 종료 되므로 적절히 대응해야 한다.

에러가 생기는 상황들

- Reference Error
  ```jsx
  console.log("[Start]");
  foo(); // ReferenceError: foo is not defined
  console.log("[End]");
  ```
- querySelector 메서드에 인수로 전달한 문자열이 CSS 선택자 문법에 맞지 않는 경우
  ```jsx
  // querySelector 메서드에 인수로 전달한 CSS 선택자 문자열로 DOM 요소 노드를 찾을 수 없는 경우
  const $button = document.querySelector("button");
  $button.classList.add("disabled");
  ```

## 47.2 try...catch...finally 문

에러 처리 구현 방법은 2가지 있다.

- 단축 평가 또는 옵셔널 체이닝 연산자 이용 방법
  - 예외적인 상황이 발생하면 반환하는 값(null or -1)을 if문이나 단축 평가 또는 옵셔널 체이닝 연산자를 통해 처리한다.
- 에러 처리 방법
  - 에러 처리 코드를 미리 등록 후 에러 처리 코드로 점프하도록 하는 방법이다.
  - 일반적으로 에러 처리라고 많이 부른다.
  - `try…catch…finally`문이 그 예이다.
  - `try…catch…finally` 문을 사용해 에러를 처리하면 프로그램이 강제 종료되진 않는다.

```jsx
try {
  // 실행할 코드
  foo();
} catch (err) {
  // try 코드블록에서 에러가 발생하면 이 코드 블록의 코드가 실행된다.
  // err에는 try 코드 블록에서 발생한 Error의 객체가 전달된다.
  console.log(err); // ReferenceError: foo is not defined
} finally {
  // 에러 발생과 상관 없이 반드시 한 번 실행된다.
  console.log("finally");
}
console.log("[End]"); // 프로그램이 강제 종료되지 않는다.
```

## 47.3 Error 객체

Error 생성자 함수는 에러 객체를 생성한다. Error 생성자 함수에는 에러를 상세히 설명하는 에러 메시지를 인수로 전달할 수 있다.

```jsx
const error = new Error("invalid");
```

- Error 생성자 함수가 생성한 객체는 message 프로퍼티와 stack 프로퍼티를 갖는다.
  - message 프로퍼티의 값은 Error 생성자 함수에 인수로 전달한 에러 메시지이다.
  - stack 프로퍼티의 값은 에러를 발생시킨 콜스택의 호출 정보를 나타내는 문자열이다.
- 자바스크립트의 에러는 총 7가지의 에러 객체를 생성할 수 있다.
  - 모두 Error.prototype을 상속받는다.

| 생성자 함수    | 인스턴스                                                                       |
| -------------- | ------------------------------------------------------------------------------ |
| Error          | 일반적인 에러 객체                                                             |
| SyntaxError    | 자바스크립트 문법에 맞지 않는 문을 해석할 때 발생하는 객체                     |
| ReferenceError | 참조할 수 없는 식별자를 참조했을 때 발생하는 에러 객체                         |
| TypeError      | 피연산자 또는 인수의 데이터 타입이 유효하지 않을 때 발생하는 에러 객체         |
| RangeError     | 숫자값의 허용 범위를 벗어났을 때 발생하는 에러 객체                            |
| URIError       | encodeURI 또는 decodeURI 함수에 부적절한 인수를 전달했을 때 발생하는 에러 객체 |
| EvalError      | eval 함수에서 발생하는 에러 객체                                               |

## 47.4 throw 문

Error 생성자 함수로 에러 객체를 생성한다고 에러가 발생하는 것은 아니다. 즉, 에러 객체 생성과 발생은 의미가 다르다

```jsx
try {
  new Error("someting wrong"); // 에러 생성
} catch (error) {
  console.log(error);
}
```

- 에러를 발생시키고자 한다면 throw 문으로 에러 객체를 던져야 한다.

```jsx
throw 표현식;
```

throw문의 표현식은 어떤 값이라도 상관 없지만 일반적으로 에러 객체를 지정한다.

에러를 던지면 catch문의 에러 변수가 생성되고, 던져진 에러 객체가 할당된다. 그러고 catch 코드 블록이 실행된다.

```jsx
// 외부에서 전달받은 콜백 함수를 n번만큼 반복 호출한다.
const repeat = (n, f) => {
	// 매개변수 f에 전달된 인수가 함수가 아니면 TypeError를 발생시킨다.
	if(typeor f !== 'function') throw new TypeError('f must be a function');

	for(var i = 0; i < n; i++) {
		f(i); // i를 전달하면서 f 호출
	}
};

try{
	repeat(2, 1);
} catch(err) {
	console.log(err); // TypeError: 'f must be a function'
```

## 47.5 에러의 전파

에러는 호출자 방향으로 전파된다. 즉, 콜 스택의 아래 방향 (실행중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파된다.

```jsx
const foo = () => {
  throw Error("foo에서 발생한 에러"); // (4) 에러를 throw
};

const bar = () => {
  foo(); // (3) foo 함수가 호출
};

const baz = () => {
  bar(); // (2) bar 함수가 호출
};

try {
  baz(); // (1) baz 함수를 호출
} catch (err) {
  console.log(err);
}
```

- foo함수가 throw한 에러는 다음과 같이 호출자에게 전파되어 전역에서 캐치된다.

![errorSpreadToCaller](https://user-images.githubusercontent.com/108210492/212852336-f167358a-8827-41bc-9510-8ef46d0a17b3.png)

- throw된 에러를 캐치하지 않으면 호출자 방향으로 전파된다.
- 이때 throw된 에러를 캐치하여 적절히 대응하면 프로그램을 강제 종료시키지 않고 코드의 실행 흐름을 복구할 수 있다.
- throw된 에러를 어디에도 캐치하지 않으면 프로그램은 강제 종료된다.
- 주의할 것은 비동기 함수인 setTimeout이나 프로미스 후속 처리 메서드의 콜백 함수는 호출자가 없다는 것이다.
- setTimeout이나 프로미스 후속 처리 메서드의 콜백 함수는 태스크 큐나 마이크로태스크 큐에 일시 저장되었다가
- 콜 스택이 비면 이벤트 루프에 의해 콜 스택으로 푸쉬되어 실행된다.
- 이때 콜 스택에 푸시된 콜백 함수의 실행 컨텍스트는 콜 스택의 가장 하부에 존재하게 된다.
- 따라서 에러를 전파할 호출자가 존재하지 않는다.
