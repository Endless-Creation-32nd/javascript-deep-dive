# 26장 ES6 함수의 추가 기능
## 26.1 함수의 구분

ES6 이전까지 자바스크립트의 함수는 별다른 구분 없이 다양한 목적으로 사용되었다.

1. 일반적 함수로서의 호출
2. new 연산자와 함께 생성자 함수로서의 호출
3. 메서드로서의 호출

ES6 이전의 함수는 사용 목적에 따라 명확히 구분되지 않았다. 즉, 모든 함수는 callable이면서 constructor이다. 이는 실수를 유발할 수도 있고, 성능 면에서도 손해이다. 

<aside>
💡 이러한 문제를 해결하기 위해 ES6에서는 함수를 사용 목적에 따라 세 가지로 명확히 구분했다.

</aside>

| ES6 함수의 구분 | contructor | prototype | super | arguments |
| --- | --- | --- | --- | --- |
| 일반 함수(Normal) | O | O | X  | O |
| 메서드 (Method) | X | X | O | O |
| 화살표 함수 (Arrow) | X | X | X | X |

## 26.2 메서드

ES6 이전 사양에서는 메서드에 대한 명확한 정의가 없었다.

**ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다.**

```jsx
const obj = {
	x: 1;

	//foo는 메서드이다.
	foo(){return this.x;},

	//bar에 바인딩된 함수는 메서드가 아닌 일반함수이다.
	bar: function(){return this.x;}
};

```

- ES6 메서드는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고, 프로토타입도 생성하지 않는다.
- ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부슬롯 [[HomeObject]]를 갖는다.
- ES6 메서드가 아닌 함수는 super키워드를 사용할 수 없다. 메서드가 아닌 함수는 내부 슬롯을 갖지 않기 때문이다.

## 26.3 화살표 함수

화살표 함수는 function 키워드 대신 화살표를 사용하여 기존의 함수 정의 방식보다 간략하게 함수를 정의할 수 있다. 표현 뿐 아니라 내부 동작도 기존 함수보다 간략하다.

### 26.3.1 화살표 함수 정의

**함수 정의**

- 화살표 함수는 함수 선언문으로 정의할 수 없고 함수 표현식으로 정의해야 한다.

```jsx
const mul = (x, y) => x*y;
mul(2, 3) = 6;
```

**매개변수 선언**

- 매개변수가 여러 개인 경우 소괄호 ( ) 안에 매개변수를 선언한다.
- 매개변수가 한 개인 경우 소괄호 ( )를 생략할 수 있다.
- 매개변수가 없는 경우 소괄호 ( )를 생략할 수 없다.

```jsx
const arrow1 = (x, y) => {...}
const arrow2 = x => {...}
const arrow3 = () => {...}
```

**함수 몸체 정의**

함수 몸체가 하나의 문으로 구성된다면 함수 몸체를 감싸는 중괄호 {}를 생략할 수 있다. 이때 함수 몸체 내부의 문이 값으로 평가 되는 경우 암묵적으로 반환된다.

```jsx
const power = x => x**2;
power(2); // -> 4

//위 표현은 다음과 동일하다
const power = x => {return x**2;};

```

- 함수 몸체 내부의 문이 표현식이 아닌 문이라면 에러가 발생한다.
- 표현문이 아닌 문은 반환될 수 없기 때문이다.
- 함수 몸체가 하나의 문으로 구성된다 해도 함수 몸체의 문이 표현식이 아닌 문이라면 중괄호를 생략할 수 없다.
- 객체 리터럴을 반환하는 경우에도 객체리터럴을 소괄호로 감싸주어야 한다.
- 함수 몸체가 여러 개의 문으로 구성된다면, 함수 몸체를 감싸는 중괄호 생략이 불가하다.
- 화살표 함수도 즉시 실행 함수로 사용가능하다.

### 26.3.2 화살표 함수와 일반 함수의 차이

화살표 함수와 일반 함수의 차이

1. 화살표 함수는 인스턴스 생성이 불가한 non-constructor
2. 중복된 매개변수 이름을 선언해도 에러가 발생하지 않는다.
3. 화살표 함수는 함수 자체의 this, arguments, super, [new.target](http://new.target) 바인딩을 갖지 않는다. 

    a. 따라서 화살표 함수 내부에 this, arguments, super, new.target을 참조하면 상위 스코프의 것을 참조한다.

### 26.3.3 this

- 화살표 함수가 일반함수와 구별되는 가장 큰 특징은 **this**이다.
- 화살표 함수는 다른 함수의 인수로 전달되어 콜백 함수로 사용되는 경우가 많다.
- 콜백함수 내부의 this 문제, 즉 콜백 함수 내부의 this가 외부 함수의 this와 다르기 때문에 발생하는 문제를 해결하기 위해 의도적으로 설계된 것이다.
- 일반 함수로서 호출되는 모든 함수 내부의 this는 전역 객체를 가리킨다.

주의할 것은 일반 함수로서 호출되는 콜백 함수의 경우다. 고차함수의 인수로 전달되어 고차 함수 내부에서 호출되는 콜백 함수도 중첩 함수라고 할 수 있다. 

```jsx
class Prefixer {
	constructor(prefix){
		this.prefix = prefix;	
	}
	
	add(arr) {
		// add 메서드는 인수로 전달된 배열 arr을 순회하여 배열의 모든 요소에 prefix를 추가한다.
		// (1)
		return arr.map(function(item) {
			return this.prefix + item; // (2)
				//->typeError: Cannot read property 'prefix' of undefined
		});
	}
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-select']));
```

위 예제를 실행했을 때 기대하는 결과는 [’-webkit-transition’, ‘-webkit-user-select’]다. 하지만 TypeError가 발생한다. 

- 프로토타입 메서드 내부인 //(1)에서 this는 메서드를 호출한 객체를 가리킨다.
- 그런데 [Array.prototype.map](http://Array.prototype.map)의 인수로 전달한 콜백함수의 내부인 //(2) 에서 this는 undefined를 가리킨다. Array.prototype.map 메서드가 콜백함수를 일반 함수로서 호출하기 때문이다.
- 그런데 클래스 내부의 모든 코드는 strict mode가 암묵적으로 적용된다. 따라서 [Array.prototype.map](http://Array.prototype.map) 메서드의 콜백함수에도 strict mode가 적용 된다.
- strict mode에서 일반 함수로서 호출되는 모든 함수 내부의 this에는 전역 객체가 아니라 undefined가 바인딩되므로 일반 함수로서 호출되는 [Array.prototype.map](http://Array.prototype.map) 메서드의 콜백함수 내부의 this에는 undefined가 바인딩 된다.

ES6에서는 화살표 함수를 사용하여 콜백함수 내부의 this 문제를 해결할 수 있다. 

```jsx
class Prefixer {
	constructor(prefix) {
		this.prefix = prefix;
	}
	
	add(arr){
		return arr.map(item => this.prefix + item);
	}
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-select']));
```

- 화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다.
- 따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다. 이를 **lexical this**라고 한다.
- 이는 마치 렉시컬 스코프처럼 화살표 함수의 this가 함수가 정의된 위치에 의해 결정된다는 것을 의미한다.

```jsx
//화살표 함수는 상위 스코프의 this를 참조한다.
() => this.x;

//익명 함수에 상위 스코프의 this를 주입한다. 위 화살표 함수와 동일하게 동작한다.
(function () {return this.x;}).bind(this);
```

- 만약 화살표 함수끼리 중첩되어 있다면 상위 화살표 함수에도 this 바인딩이 없으므로 스코프 체인 상에서 가자 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 this를 참조한다.
- 프로퍼티에 할당한 화살표 함수도 스코프 체인상에서 가장 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 this를 참조한다.
- 화살표 함수는 함수 자체의 this 바인딩을 갖지 않기 때문에 Function.prototype.call, Function.prototype.apply, Function.prototype.bind 메서드를 사용해도 화살표 함수 내의 this를 교체할 수 없다.

```jsx
//Good Ex)
const person = {
	name: 'Lee',
	sayHi() {
		console.log(`Hi, I'm ${this.name}`);
	}
};
person.sayHi(); //Hi, I'm Lee

//Bad  Ex)
function Person(name) {
	this.name = name;
}

Person.prototype.sayHi = () => console.log(`Hi, I'm ${this.name}`);
//브라우저에서 실행하면 this.name은 빈 문자열을 갖는 window.name과 같다
person.sayHi(); // Hi
```

Bad 예제의 경우 sayHi 프로퍼티에 할당한 화살표 함수 내부의 this는 메서드를 호출한 객체인 person을 가리키지 않고, 상위스코프인 전역을 가리킨다. 

따라서 화살표 함수로 메서드를 정의하는 것은 바람직하지 않다. 메서드를 정의할 땐 ES6 메서드를 사용하는 것이 좋다.

### 26.3.4 super

- 화살표 함수는 함수 자체에 super 바인딩을 갖지 않는다.
- 따라서 화살표 함수 내부에서 super을 참조하면 this와 마찬가지로 상위 스코프의 super을 참조한다.

### 26.3.5 arguments

- 화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않는다.
- 따라서 화살표 함수 내부에서 arguments 객체를 참조하면 상위 스코프의 arguments를 참조한다.
    - arguments 객체는 함수 정의시 매개변수의 개수를 정할 수 없는 가변인자 함수를 구현할 때 유용하다.
- 상위 스코프의 arguments 객체는 참조 가능하지만 자신에게 전달된 인수 목록을 참조하지 못하므로 도움되지 않는다.
- 따라서 화살표 함수로 가변인자 함수를 구현할 때는 반드시 Rest 파라미터를 사용해야 한다.

## 26.4 Rest 파라미터

### 26.4.1 기본 문법

- Rest 파라미터는 매개변수 이름 앞에 세 개의 점 … 을 붙여서 정의한 매개변수를 의미한다.
- Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.
- 이름 그대로 먼저 선언된 매개변수에 할당된 인수를 제외한 나머지 인수들의 배열이기에 Rest 파라미터는 반드시 마지막 파라미터여야 한다.
- Rest 파라미터는 단 한번만 선언할 수 있다.
- Rest 파라미터는 함수 객체의 length 프로퍼티에 영향을 주지 않는다.

```jsx
function foo(param, ...rest) {
	//일반 매개변수와 Rest 파라미터는 함께 사용 가능하고, 순차적으로 할당된다.
	console.log(param); // 1
	//매개변수 rest는 인수를 배열로 전달받는 Rest 파라미터이다.
	console.log(rest); //[2,3,4,5]
}

foo(1,2,3,4,5);
console.log(foo.length); // 1
```

### 26.4.2 Rest 파라미터와 arguments 객체

- arguments 객체는 함수 호출 시 전달 된 인수들의 정보를 담고 있는 유사 배열 객체이다.
- 따라서 배열 메서드를 사용하려면 Function.prototype.call, Function.prototype.apply 메서드를 사용해 객체를 배열로 변환해야 한다.
- Rest 파라미터를 사용하면 가변인자 함수의 인수 목록을 배열로 직접 받을 수 있다.
- 화살표 함수는 arguments 객체를 갖지 않기 때무에 가변인자 함수를 구현해야 할 때는 반드시 Rest 파라미터를 사용해야 한다.

## 26.5 매개변수 기본값

자바스크립트 엔진이 매개변수의 개수와 인수의 개수를 체크하지 않기 때문에, 매개변수의 개수만큼 인수를 전달하지 않아도 에러는 발생하지 않는다. 

인수가 전달되지 않은 매개변수의 값은 undefined이다. 이를 방치하면 의도치 않은 결과가 나올 수 있다. 

```jsx
function sum (x, y) {
	return x+y;
}
console.log(sum(1)); //NaN
```

이처럼 의도치 않은 결과가 나오는 것을 방지하기 위해 방어코드가 필요하다.

```jsx
function sum (x, y) {
	x = x || 0;
	y = y || 0;

	return x+y;
}

console.log(sum(1)); // 1
console.log(sum(1,1)); // 2
```

ES6에서 도입된 매개변수 기본 값을 사용하면 코드를 간소화 할 수 있다.

```jsx
function sum(x=0, y=0) {
	return x + y;
}
console.log(sum(1)); // 1
console.log(sum(1,1)); // 2
```

<aside>
💡 매개변수의 기본 값은 매개변수에 인수를 전달하지 않은 경우와 undefined를 전달한 경우에만 유효하다.

</aside>

- Rest 파라미터에는 기본 값을 지정할 수 없다