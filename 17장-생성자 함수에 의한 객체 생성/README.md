# 17장: 생성자 함수에 의한 객체 생성

## 17.1 Object 생성자 함수

- 생성자 함수란 new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수를 말한다.

- new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다.

  ```javascript
  // 빈 객체의 생성
  const person = new Object();
  ```

- 자바스크립트는 Object 생성자 함수 이외에도 String, Number, Boolean, Function, Array등의 빌트인 생성자 함수를 제공한다.

<br>

## 17.2 생성자 함수

### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

```js
const ractangle1 = {
  width: 4,
  height: 3,
};

const ractangle2 = {
  width: 4,
  height: 3,
};
```

- 객체 리터럴에 의한 객체 생성 방식은 직관적이고 간편하지만 단 하나의 객체만 생성한다.
  - 따라서 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적이다.

<br>

### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

- 생성자 함수에 의한 객체 생성 방식은 마치 객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

  ```javascript
  // 생성자 함수
  function Circle(radius) {
    // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.raduis = radius;
    this.getDiameter = function () {
      return 2 * this.radius;
    };
  }

  // 인스턴스의 생성
  const circle1 = new Circle(5);
  const circle2 = new Circle(10);
  ```

  > `this`는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수다. this가 가리키는 값은 함수 호출 방식에 따라 동적으로 결정된다.

- 자바와 같은 클래스 기반 객체지향언어의 생성자와는 다르게 그 형식이 정해져 있는 것이 아니라 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 `new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.` 만약, new 연산자와 함께 생성자 함수를 호출하지 않으면 일반 함수로 동작한다.

여기서 잠깐! this 바인딩을 간단히 결정해보자

1. 함수가 `new`(new binding)와 함께 호출된다면 `this`는 새로 생성된 객체이다.
   `var bar = new foo()`
2. `call` 또는 `apply`(명시적 바인딩)로 호출된 함수가 `bind` 하드 바인딩 안에 숨겨져 있다면 `this`는 명시적으로 지정된 객체이다.
   `var bar = foo.call( obj2 )`
3. 함수가 컨텍스트(암시 적 바인딩) 혹은 소유/포함하고 있는 객체에서 호출된다면 `this`는 바로 그 컨텍스트 객체이다.
   `var bar = obj1.foo()`
4. 그렇지 않으면 `this`를 기본값으로 설정한다(기본 바인딩). `strict mode`인 경우 `undefined` 가 선택되고 그렇지 않으면 `global` 객체가 바인딩된다.
   `var bar = foo()`

<br>

### 17.2.3 생성자 함수의 인스턴스 생성 과정

- 먼저, 생성자 함수의 역할은 프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿(클래스)으로서 동작하여 `인스턴스`를 생성하는 것과 `생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당)`하는 것이다.
- new 연산자와 함께 생성자 함수를 호출하면 자바스크립트 엔진은 `아래와 같은 과정`을 거쳐 암묵적으로 인스턴스를 생성하고 인스턴스를 초기화한 뒤 암묵적으로 인스턴스를 반환한다.

1. 인스턴스 생성과 this 바인딩 (런타임 이전에 실행)
   - 암묵적으로 빈 객체(인스턴스)가 생성된다.
   - 인스턴스는 `this`에 바인딩된다. -> 생성자 함수 내부의 `this`가 생성자 함수가 생성할 인스턴스를 가리키는 이유
2. 인스턴스 초기화
   - 생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 `this`에 바인딩되어 있는 인스턴스를 초기화한다.
     - 즉, 인스턴스에 프로퍼티나 메서드 추가, 생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당한다.
3. 인스턴스 반환
   - 생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 `this`가 암묵적으로 반환된다.
   - 만약, `this`가 아닌 다른 객체를 명시적으로 반환하면 `this`가 아니라 return 문에 명시한 객체가 반환된다.
   - 하지만, 명시적으로 원시 값을 반환하면 원시 값은 무시되고 `this`가 반환된다.

<br>

### 17.2.4 내부 메서드 [[Call]]과 [[Construct]]

- 함수는 객체이지만 일반 객체와는 다르다 -> 일반 객체는 호출할 수 없지만 함수는 호출 가능하다.
- 따라서, 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드는 물론 함수로서 동작하기 위해 함수 객체만을 위한 [[Environment]], [[FormalParameters]]등의 `내부 슬롯`과 [[Call]]. [[Construct]] 같은 `내부 메서드`를 추가로 가지고 있다.
- 함수가 일반 함수로서 호출되면 함수 객체의 내부 메서드 `[[Call]]`이 호출된다.
- new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드 `[[Construct]]`가 호출된다.
- 모든 함수 객체는는 내부 메서드 `[[Call]]`를 가지고 있지만 모든 함수 객체가 `[[Construct]]`를 갖는 것은 아니다. (모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아니라는 뜻)

<br>

### 17.2.5 constructor와 non-constructor의 구분

- constructor(생성자 함수로서 호출 가능한 함수) - 함수 선언문, 함수 표현식, 클래스(클래스도 함수다)

```js
//함수 선언문
fucntion foo(){}

//함수 표현식
const bar = function () {}

//
const baz = {
  x: function() {}
}

new foo(); // foo {}
new bar(); // bar {}
new baz.x(); // x {}
```

- non-constructor(생성자 함수로서 호출할 수 없는 함수) - 메서드(ES6 메서드 축약 표현), 화살표 함수

```js
// 화살표 함수
const arrow = () => {};
new arrow(); //TypeError: arrow is not constructor

// 메서드
const obj = {
  x() {},
};
new obj.x(); //TypeError: obj.x is not constructor
```

<br>

### 17.2.6 new 연산자

- new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작한다.

```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}
```

- 위의 Circle 함수를 new 연산자와 함께 생성자 함수로서 호출하면 함수 내부의 `this`는 Circle 생성자 함수가 생성할 인스턴스를 가리킨다. 하지만 new 연산자없이 일반적인 함수로서 호출하면 `this`는 전역 객체 window를 가리킨다.

<br>

### 17.2.7 new.target

- 생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 ES6에서는 `new.target`을 지원한다.
- `new.target`은 `this`와 유사해게 constructor인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용되며 메타 프로퍼티라고 부른다.
- 함수 내부에서 `new.target`을 사용하면 new 연산자와 함께 생성자 함수로서 호출되었는지 확인할 수 있다.
  - new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 `new.target`은 함수 자신을 가리킨다. new 연산자 없이 일반 함수로서 호출된 함수 내부의 `new.target`은 undefined다.

```js
function Circle(radius) {
  if (!new.target) {
    return new Circle(radius);
  }
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle1 = Circle(5);
console.log(circle1.getDiameter()); // 10
```

new.target은 ES6에 도입된 최신 문법으로 IE에서는 지원되지 않는다.

```js
function Circle(radius) {
  if (!(this instanceof Circle)) {
    return new Circle(radius);
  }
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle1 = Circle(5);
console.log(circle1.getDiameter()); // 10
```
