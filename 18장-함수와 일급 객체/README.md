# 18장: 함수와 일급 객체

## 18.1 일급 객체

- 다음과 같은 조건을 만족하는 객체를 `일급 객체`라 한다.

  - 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
  - 변수나 자료구조(객체, 배열등)에 저장할 수 있다.
  - 함수의 매개변수에 전달할 수 있다.
  - 함수의 반환값으로 사용할 수 있다.

- 자바스크립트의 함수는 위의 조건을 모두 만족하므로 `일급 객체`이다.

## 18.2 함수 객체의 프로퍼티

- 함수는 객체다. 따라서 함수도 프로퍼티를 가질 수 있다.
- `arguments`, `caller`, `length`, `name`, `prototype` 프로퍼티는 모두 함수 객체의 데이터 프로퍼티이고 일반 객체에는 없는 함수 객체 고유의 프로퍼티다.
- 하지만 **proto**는 접근자 프로퍼티이며 함수 객체 고유의 프로퍼티가 아닌 Object.prototype 객체의 프로퍼티를 상속받은 것이다.

<br>

### 18.2.1 arguments 프로퍼티

- 함수 객체의 `arguments` 프로퍼티 값은 `arguments` 객체다. `arguments` 객체는 함수 호출 시 전달된 인수(argument)들의 정보를 담고 있는 순회 가능한(iterable) 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용된다. 즉, 함수 외부에서는 참조할 수 없다.
- 모든 인수는 암묵적으로 `arguments` 객체의 프로퍼티로 보관된다.

  - 인수를 프로퍼티 값으로 소유하며 프로퍼티 키는 인수의 순서를 나타낸다.

- `arguments` 객체는 매개변수 개수를 확정할 수 없는 `가변 인자 함수`를 구현할 때 유용하다.

```js
function foo(x, y) {
  console.log(x);
  console.log(y);
  console.log(arguments);
}

foo(1, 2, 3);
// 1
// 2
// Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
```

```js
function sum() {
  const array = Array.prototype.slice.call(arguments);
  return array.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum(1, 2)); //3
console.log(sum(1, 2, 3, 4, 5)); //15
```

Arguments는 유사 배열 객체라 배열처럼 다루긴 어렵고(reduce같은 배열 내장 메서드같은게 없다) ES6에 추가된 Rest 파라미터를 도입했다.

```js
function sum(...args) {
  return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2)); //3
console.log(sum(1, 2, 3, 4, 5)); //15
```

<br>

### 18.2.2 caller 프로퍼티

- 함수 객체의 `caller` 프로퍼티는 함수 자신을 호출한 함수를 가리킨다.
- ECMAScript 사양에 포함되어 있지 않고 표준화 될 예정도 없으니 간단히 넘어가자

<br>

### 18.2.3 length 프로퍼티

- 함수 객체의 `length` 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.
- 단, `arguments` 객체의 length 프로퍼티는 인자(argument)의 개수를 나타낸다.

```js
function sum(a, b) {
  console.log(sum.length);
  console.log(arguments.length);
}

sum(1, 2, 3, 4);
// 2
// 4
```

<br>

### 18.2.4 name 프로퍼티

- 함수 객체의 name 프로퍼티는 함수 이름을 나타낸다.

```javascript
// 기명 함수 표현식
var namedFunc = function foo() {};
console.log(namedFunc.name); // foo

//익명 함수 표현식
var anonymousFunc = function () {};
// ES5: name 프로퍼티는 빈 문자열을 값으로 갖는다.
// ES6: name 프로퍼티는 함수 객체를 가리키는 변수 이름을 값으로 갖는다.
console.log(anonymousFunc.name); // anonymousFunc

// 함수 선언문
function bar() {}
console.log(bar.name); //bar
```

<br>

### 18.2.5 `__proto__` 접근자 프로퍼티

- 모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다. [[Prototype]] 내부 슬롯은 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.
- `__proto__` 프로퍼티는 [[Prototpye]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티다. (내부슬롯에는 직접 접근할 수 없기 때문)

```js
const obj = { a: 1 };
console.log(obj.__proto__ === Object.prototype); //true
Object.prototype.hello = "hi";

console.log(obj.hello); // "hi"
function sum() {}
console.log(sum.hello); // "hi"
```

<br>

### 18.2.6 prototype 프로퍼티

- prototype 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor만이 소유하는 프로퍼티다.
  - 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.

```js
(function () {}.hasOwnProperty("protortype")); //true
({}.hasOwnProperty("protortype")); //false
```
