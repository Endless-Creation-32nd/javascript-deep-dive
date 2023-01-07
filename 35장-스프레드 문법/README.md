#35장 스프레드 문법

- ES6에서 도입된 스프레드 문법 `…` 은 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만든다.
- 스프레드 문법 사용가능 대상 (for … of 문으로 순화가능한 이터러블)
  - Array
  - String
  - Map
  - Set
  - DOM 컬렉션
  - arguments

```jsx
// ...[1, 2, 3]은 [1, 2, 3]을 개별 요소로 분리한다. (-> 1, 2, 3)
console.log(...[1,2,3]); // 1 2 3
console.log(...new Map(['a', '1'],['b', '2']); //['a', '1'] ['b', '2']
console.log(...{a: 1, b: 2}); //TypeError: Spread syntax requires ...iterable[Symbol.iterator] to be a function
```

위 예제에서 …[1, 2, 3]은 이터러블인 배열을 펼쳐서 요소들을 개별적인 값들의 목록 1 2 3으로 만든다. `…` 이 피연산자를 연산하여 값을 생성하는 연산자가 아님을 의미한다.

<aside>
💡 스프레드 문법의 결과는 값이 아니다. 따라서 스프레드 문법의 결과는 변수에 할당할 수 없다.

</aside>

```jsx
const list = ...[1, 2, 3]; // SyntaxError: Unexpected token '...'
```

이처럼 스프레드 문법의 결과물은 값으로 사용할 수 없고, 다음과 같이 `쉼표로 구분한 값의 목록을 사용하는 문맥`에서 사용할 수 있다.

- 함수 호출문의 인수 목록
- 배열 리터럴의 요소 목록
- 객체 리터럴의 프로퍼티 목록

## 35.1 함수 호출문의 인수 목록에서 사용하는 경우

요소들의 집합인 배열을 펼처서 개별적인 값들의 목록을 만든 후, 이를 함수의 인수 목록으로 전달해야하는 경우가 있다.

```jsx
const arr = [1, 2, 3];

//배열 arr의 요소 중에서 최대값을 구하기 위해 Math.max를 사용한다.
const max = Math.max(arr); //NaN, Math.max 메서드에 숫자가 아닌 배열을 인수로 전달하면 최대값을 구할 수 없으므로 NaN을 반홚나다.

//이를 해결하기 위해선 배열 arr을 펼쳐 요소들을 개별적인 값의 목록으로 만든 후 인수를 전달하면 된다
const max = Math.max(...arr);
```

스프레드 문법은 앞에서 살펴본 Rest 파라미터와 형태가 동일하여 혼동할 수있으므로 주의가 필요하다.

**스프레드 문법**

- 여러 개의 값이 하나로 뭉쳐 있는 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만드는 것

**Rest 파라미터**

- 함수에 전달한 인수들의 목록을 배열로 전달 받기 위해 매개변수 이름 앞에 `…` 을 붙이는 것

```jsx
//Rest 파라미터는 인수들의 목록을 배열로 받는다.
function foo(...rest) {
  console.log(rest);
}

//스프레드 문법은 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만든다.
//[1, 2, 3] -> 1, 2, 3
foo(...[1, 2, 3]);
```

## 35.2 배열 리터럴 내부에서 사용하는 경우

스프레드 문법을 배열 리터럴에서 사용하면 ES5에서 사용하던 기존 방식보다 더욱 간결하고 가독성있게 표현 가능하다.

### 35.2.1 concat

- ES5에서 2개의 배열을 1개의 배열로 결합하고 싶은 경우, 배열 리터럴만으로 해결할 수 없고 concat 메서드를 사용해야한다.
- 스프레드 문법을 사용하면 별도의 메서드를 사용하지 않고 배열 리터럴 만으로 2개의 배열을 1개의 배열로 결합 가능하다.

```tsx
//ES5
var arr = [1, 2].concat([3, 4]);
console.log(arr); // [1, 2, 3, 4]

//ES6
const arr = [...[1, 2], ...[3, 4]];
consolr.log(arr); // [1, 2, 3, 4]
```

### 35.2.2 splice

- ES5에서 어떤 배열의 중간에 다른 배열의 요소들을 추가하거나 제거하려면 splice 메서드를 사용한다.
- 이때 splice 메서드의 세 번째 인수로 배열을 전달하면 배열 자체가 추가된다.

```tsx
//ES5
var arr1 = [1, 4];
var arr2 = [2, 3];

//세 번째 인수 arr2를 해체하여 전달해야 된다.
//그렇지 않으면 arr1에 arr2 배열 자체가 추가된다.
arr1.splice(1, 0, arr2); //기대한 결과인 [1, 2, 3, 4]가 아닌 아래 사진과 같은 결과가 나온다.
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/78c0623c-a567-4503-8d3f-ef301c19a4be/Untitled.png)

- 위에서 기대한 결과를 나타내려면 Function.prototype.apply 메서드를 사용하여 splice 메서드를 호출해야한다.

```tsx
var arr1 = [1, 4];
var arr2 = [2, 3];
Array.prototype.splice.apply(arr1, [1, 0].concat(arr2);
console.log(arr1); //[1, 2, 3, 4]
```

- 스프레드 문법을 사용하면 더욱 간결하고 가독성 좋게 표현가능하다.

```tsx
// ES6
var arr1 = [1, 4];
var arr2 = [2, 3];

arr.splice(1, 0, ...arr2);
console.log(arr1); //[1, 2, 3, 4]
```

### 35.2.3 배열 복사

- ES5에서 배열을 복사하려면 slice 메서드를 사용한다
- 스프레드 문법을 사용하면 더 간결한 표현이 가능하다
- 두 방법 모두 원본 배열의 각 요소를 `얕은 복사`하여 `새로운 복사본`을 생성한다.

```tsx
// ES5
var origin = [1, 2];
var copy = origin.slice();

console.log(copy); // [1, 2]
console.log(copy === origin); //false

// ES6
const origin = [1, 2];
const copy = [...origin];
console.log(copy); // [1, 2]
console.log(copy === origin); // false
```

### 35.2.4 이터러블을 배열로 변환

- ES5에서 이터러블을 배열로 변환하려면 Function.prototype.apply 또는 [Function.prototype.call](http://Function.prototype.call) 메서드를 사용하여 slice 메서드를 호출해야한다.
- 스프레드 문법을 사용하면 더 간결하게 변환이 가능하다.
- arguments 객체는 이터러블이면서 유사 배열객체이기에 스프레드 문법의 대상이 될 수 있다.

```tsx
// ES5
function sum() {
  // 이터러블이면서 유사 배열 객체인 arguments를 배열로 변환
  var args = Array.prototype.slice.call(arguments);
  return args.reduce(function (per, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum(1, 2, 3)); //6

// 이터러블이 아닌 유사 배열 객체
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 3,
};

const arr = Array.prototype.slice.call(arrayLike); // -> {1, 2, 3}
console.log(Array.isArray(arr)); //true
```

```tsx
function sum() {
	// 이터러블이면서 유사 배열 객체인 arguments를 배열로 변환
	return [...arguments].reduce((pre, cur) => pre + cur, 0);
}
console.log(sum(1, 2, 3);

// Rest 파라미터 args는 함수에 전달된 인수들의 목록을 배열로 전달받는다.
const sum = (...args) => args.reduce((pre, cur) => pre + cur, 0);
console.log(sum(1, 2, 3)); // 6

//Array.from 메서드는 유사 배열 객체 또는 이터러블을 인수로 전달받아 배열로 변환하여 반환한다.
Array.from(arrayLike); // -> [1, 2, 3]
```

## 35.3 객체 리터럴 내부에서 사용하는 경우

Rest 프로퍼티와 함께 2021년 1월 현재 TC39 프로세스의 stage 4(Finished) 단계에 제안되어 있는 스프레드 프로퍼티를 사용하면 객체 리터럴의 프로퍼티 목록에서도 스프레드 문법을 사용할 수 있다.

- 스프레드 문법의 대상은 이터러블이어야 하지만 스프레드 프로퍼트 제안은 일반 객체를 대상으로도 스프레드 문법의 사용을 허용한다.

```tsx
//스프레드 프로퍼티
//객체 복사(얕은 복사)
const obj = { x: 1, y: 2 };
const copy = { ...obj };
console.log(copy);
console.log(obj === copy); //false

//객체 병합
const merged = { x: 1, y: 2, ...{ a: 3, b: 4 } };
console.log(merged); //{x: 1, y: 2, a: 3, b: 4};
```

- 스프레드 프로퍼티가 제안되기 이전에는 ES6에서 도입된 Object.assign 메서드를 사용하여 여러 개의 객체를 병합하거나 특정 프로퍼티를 변경 또는 추가했다.

```tsx
// 객체 병합. 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = Obect.assign({}, { x: 1, y: 2 }, { y: 10, z: 3 });
console.log(merged);

//특정 프로퍼티로 변경
const changed = Object.assign({}, { x: 1, y: 2 }, { y: 100 });
console.log(changed);

//프로퍼티 추가
const added = Object.assign({}, { x: 1, y: 2 }, { z: 0 });
console.log(added); // {x: 1, y: 2, z: 0}
```

- 스프레드 프로퍼티는 Object.assign 메서드를 대체할 수 있는 간편한 문법이다.

```tsx
// 객체 병합. 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3 } };
console.log(merged);

//특정 프로퍼티로 변경
const changed = { ...{ x: 1, y: 2 }, ...{ y: 100 } };
console.log(changed);

//프로퍼티 추가
const added = { ...{ x: 1, y: 2 }, ...{ z: 0 } };
console.log(added); // {x: 1, y: 2, z: 0}
```
