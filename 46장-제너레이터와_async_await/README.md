# 46장 제너레이터와 async/await

## 46.1 제너레이터란?

ES6에서 도입된 제너레이터는 코드블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수이다. 일반함수와 차이는 다음과 같다

1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도(yield)할 수 있다.
   - 일반 함수를 호출하면 제어권이 호출된 함수로 넘어가고 함수 코드를 일괄 실행한다.  
     즉 함수 호출자는 함수 호출 이후 함수 실행권을 제어할 수 없다.
   - 제너레이터 함수는 함수 실행을 함수 호출자가 제어할 수 있다.  
     다시말해 함수 호출자가 함수 실행을 일시 중지시키거나 재개시킬 수 있다. 이는 함수의 제어권을 함수가 독점하는 것이 아니라 함수호출자에게 **양도 (yield)** 할수 있다는것을 의미한다
2. 제너레이터 함수는 함수 호출자와 함수 상태를 주고받을 수 있다
   - 일반 함수를 호출하면 매개변수를 통해 함수 외부에서 값을 주입받고, 함수내 코드 일괄 실행후 결과값을 외부로 반환한다. 함수가 실행되는 동안은 외부에서 내부로 값을 전달하여 함수의 상태를 변경할 수 없다
   - 제너레이터 함수는 함수 호출자와 **양방향으로** 함수의 상태를 주고받을 수 있다. 제너레이터 함수는 함수 호출자에게 상태를 전달할 수 있고 호출자로부터 상태를 전달받을수도 있다
3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다
   - 제너레이터 함수를 호출하면 함수 코드를 실행하는게 아니라 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다

## 46.2 제너레이터 함수의 정의

> 제너레이터는 function\* 키워드로 선언하고, 하나 이상의 yield 표현식을 사용한다

```javascript
//제너레이터 함수 선언문
function* genDecFunc() {
  yield 1;
}
// 제너레이터 함수 표현식
const genExpFunct = function* () {
  yield 1;
};

//제너레이터 메서드
const obj = {
  *genObjMethod() {
    yield 1;
  },
};

//제너레이터 클래스 메서드
class Myclass {
  *genClsMethod() {
    yield 1;
  }
}
```

- \*의 위치는 function 키워드와 함수 이름 사이 어디든 넣어도 상관 없다. 단 일관성을 위해 function 바로 뒤에 붙이는 것을 권장한다

  ```javascript
  function* genFunc() {
    yield 1;
  }
  function *genFunc() {
    yield 1;
  }
  function * genFunc() {
    yield 1;
  }
  ```

* 제너레이터 함수는 화살표 함수로 정의할 수 없다
* 제너레이터 함수는 new 연산자와 함께 생성자 함수로 호출할수 없다

## 46.3 제너레이터 객체

> 💡 제러네이터 함수를 호출하면 일반 함수처럼 코드 블록을 실행하는 게 아니라 제너레이터 객체를 생성하여 반환한다
> 💡 제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서 동시에 이터레이터 이다

- 이터러블이면서 이터레이터?
  - 이터러블 : Symbol.iterator 메서드를 가진 객체
  - 이터레이터 : next() 메서드를 소유한 이터레이터, next 메서드 실행 시 {value : any, done:boolean} 형태의 이터레이터 리절트 객체를 반환

```javascript
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

const generator = genFunc();

console.log(Symbol.iterator(generator)); //true
console.log("next" in generator); //true;
```

- 제너레이터 객체 메서드 호출 시 동작방식

  > 제너레이터 객체는 next 메서드를 갖고 있는 이터레이터지만 이터레이터에 없는 return, throw 메서드를 가진다


- next  
  실행 시 제너레이터의 yield 표현식까지 코드블록을 실행하고, value에 yield된 값을, done에는 false값을 가진 이터레이터 리절트 객체를 반환한다

- return
  실행 시 value에는 인수로 전달받은 값, done에는 true 값을 가진 이터레이터 리절트 객체를 반환

- throw
  인수로 전달받은 에러를 발생시키고, value에는 undefined, done에는 true값을 가진 이터레이터 리절트 객체 반환

  ```javascript
  function* genFunc() {
    yield 1;
    yield 2;
    yield 3;
  }

  const generator = genFunc();

  console.log(generator.next()); // {value:1, done:false}
  console.log(generator.return("End")); // {value:'End',done:true}

  const generator2 = genFunc();
  console.log(generator2.next()); // {value:1, done:false}
  console.log(generator2.throw(new Error("에라")));
  // Error: 에라
  // {value:undefined, done:true}
  ```

## 46.4 제너레이터의 일시 중지와 재개

> generator.next() -> yield -> generator.next() -> yield -> .... -> generator.next() -> return;
>
> 1. 제너레이터 함수는 제너레이터 객체를 리턴한다
> 2. 제너레이터의 next메서드 실행시 제너레이터 함수 안의 코드블록을 실행한다
> 3. 한번에 모든 코드를 실행하지 않고, yield 표현식까지만 실행한다. yield 키워드는 제너레이터 함수 실행을 중지하거나, yield 뒤의 표현식 값의 평과결과를 제너레이터 함수 호출자에게 반환한다

```javascript
function* genFunc() {
  let arr = [];
  console.log("first");
  let a = 1;
  arr.push(1);
  yield arr;

  console.log("second");
  let b = 2;
  arr.push(2);
  yield arr;

  console.log("third");
  arr.push(a * a + b * b);
  yield arr;
}

//이터러블이면서 이터레이터인 제너레이터 객체 반환
const generator = genFunc();

generator.next();
// first
// {value: Array(1), done: false}
generator.next();
// second
//{value: Array(2), done: false}
generator.next();
// third
//{value: Array(3), done: false}

generator.next();
// {value: undefined, done: true}
```

next() 메서드는이전 yield ~ 다음 yield 이전까지 코드를 실행, 더이상 yield 표현식이 없다면 {value:undefined, done :true};로 마무리된다.

- 제너레이터 함수 안에 인수 전달하기
  이터레이터의 next와는 달리 제너레이터 객체의 next에는 인수를 전달할 수 있다  
   next 메서드에 전달한 인수는 **yield 표현식을 할당받는 변수**에 할당된다

  > yield 표현식을 할당받는 변수에 yield 표현식의 평과 결과가 할당되지 않는다!!!

  ```javascript
  function* genFunc() {
    const a = yield 1;

    const b = yield a * 10;

    return a * b;
  }

  const generator = genFunc();

  // 처음과 마지막 next의 인수는 의미가 없다
  generator.next(2);
  // 인수로 넣어준 2는 아무 효과가 없다
  // {value: 1, done: false}

  generator.next(10);
  // 인수 10은 첫번째 yield가 할당해주는 값, 즉 a에 할당됨 
  // {value: 100, done: false}


  generator.next(20);
  // 인수로 넣어준 20은 두번째 yield가 할당해주는 값, 즉 b에 할당됨
  // {value: 200, done: true}

  // 처음과 마지막 next의 인수는 의미가 없다
  generator.next(1000);
  // {value: undefined, done:true}  
  ```

  이처럼 제너레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 상태를 주고받을 수 있다
  함수 호출자는 next 메서드를 통해 yield 된 값을 꺼내오고, 전달한 인수를 통해 객체에 상태를 넣어줄 수 있다

## 46.5 제너레이터의 활용

### 46.5.1 이터러블의 구현

제너레이터를 사용해 간단하게 이터러블을 구현할 수 있다.

```javascript
// 무한 피보나치 수열 생성하는 함수, 이터러블
const infiniteFibonacci = (function () {
  let [pre, cur] = [0, 1];
  return {
    [Symbol.iterator]() {
      return this;
    }
    next(){
      [pre,cur] = [cur, pre+cur];
      return {value: cur};
    }
  };
})();

for( const num of infiniteFibonacci){
  if(num > 10000) break;
  console.log(num)
}
```

제너레이터를 사용해서 구현하면

```javascript
const infiniteFibonacci = (function* () {
  let [pre, cur] = [0, 1];

  while (true) {
    [pre, cur] = [cur, pre + cur];
    yield cur;
  }
})();

for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num);
}
```

### 46.5.2 비동기 처리

제너레이터 함수의 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있다. 이걸 활용해서 프로미스를 사용한 비동기 처리를 동기처럼 구현할 수 있다

```javascript
const async = (generatorFunc) => {
  const generator = generatorFunc();

  const onResolved = (arg) => {
    const result = generator.next(arg);
    console.log("result: ", result);
    return result.done
      ? result.value
      : result.value.then((res) => onResolved(res));
  };
  return onResolved;
};

function* fetchTodo() {
  const url = "https://jsonplaceholder.typicode.com/todos/1";

  const response = yield fetch(url);
  const todo = yield response.json();
  console.log("todo : ", todo);
}
async(fetchTodo)();
```

## 46.6 async / await

제너레이터를 쓰면 비동기 처리를 동기처럼 동작하도록 구현할 수 있지만 코드가 장황하고 가독성이 나빠졌다. ES8에서는 제너레이터보타 간단하고 가독성 좋게 비동기처리를 동기처럼 동작하도록 구현할 수 있는 async/await이 도입되었다

async/await은 프로미스를 기반으로 동작한다. then, cath, finally에 메서드를 전달할 필요 없이 동기처리처럼 사용 가능하다

```javascript
async function fetchTodo() {
  const url = "https://jsonplaceholder.typicode.com/todos/1";

  const response = await fetch(url);
  const todo = await response.json();
  console.log("todo : ", todo);
}

fetchTodo();
```

### 46.6.1 async 함수

- await 키워드는 반드시 async 내부에서 사용해야한다
- async 함수는 언제나 프로미스를 반환한다, 명시적으로 프로미스를 반환하지 않더라도 암묵적으로 반환값을 resolve하는 프로미스를 반환한다

### 46.6.2 await 키워드

- await 키워드는 프로미스가 settled 상태가 될 때까지 대기하다가, settled 상태가 되면 resolve한 처리 결과를 반환한다. await은 반드시 프로미스 앞에서 사용해야 한다.

- 모든 프로미스에 await 주의!!

  ```javascript
  const a = await Promise((resolve) => setTimeout(resolve(1), 3000));
  const b = await Promise((resolve) => setTimeout(resolve(2), 2000));
  const c = await Promise((resolve) => setTimeout(resolve(3), 1000));

  console.log(a, b, c);

  //a,b,c가 순차적으로 진행되어 6초가 걸린다
  ```

  ```javascript
    async function foo(){
      const res = await Promise all([
        await Promise((resolve) => setTimeout(resolve(1), 3000)),
        await Promise((resolve) => setTimeout(resolve(2), 2000)),
        await Promise((resolve) => setTimeout(resolve(3), 1000)),
      ]);

  // 약 3초 소요
    }
  ```

### 46.6.3 에러 처리

- 비동기 처리를 위한 콜백 패턴의 단점중 가장 심각한 것은 에러처리가 곤란하다는 것이다. 45.1.2절 에러처리의 한계애서 본것처럼 콜스택 아래방향으로 전파된다. 하지만 비동기 함수 콜백함수를 호출한게 비동기함수가 아니기 때문에 try-catch문을 사용할 수 없다

- async await에서는 try catch를 사용할 수 있다.

- async 함수 내에서 catch문을 사용해서 에러처리를 하지 않으면 async 함수는 에러를 reject 하는 프로미스를 반환한다. 따라서 async함수를 호출하고 catch 후속 처리 메서드를 통해 에러를 캐치할 수도 있다.
