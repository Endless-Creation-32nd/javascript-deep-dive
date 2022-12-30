# 25장 클래스

## 25.1 클래스는 프로토타입의 문법적 설탕인가?

> ✅ 클래스는 프로토타입 기반 객체 생성 패턴의 문법적 설탕이기 보다는 새로운 객체 생성 메커니즘으로 보는게 합당하다.

- ES5에서의 프로토타입 기반 객체 생성 방식
  프로토타입 기반 객체지향 언어는 클래스를 사용하지 않아도 생성자 함수와 프로토타입을 통해 객체지향의 상속을 구현할 수 있었다

  ```javascript
  var Person = function () {
    function Person(name) {
      this.name = name;
    }

    Person.prototype.sayHi = function () {
      console.log("hello! my name is " + this.name);
    };

    return Person;
  };

  var me = new Person("Lee");
  me.sayHi(); //Hi! my name is Lee
  ```

  이는 클래스기반 언어에 익숙한 프로그래머는 프로토타입 기반 프로그래밍 방식에 혼란을 느끼고 자바스크립트를 어렵게 하는 하나의 장벽처럼 인식되었다.

- ES6에서 도입된 클래스

  - 자바나 C# 같은 클래스 기반 객체지향 프로그래밍 언어와 흡사한 객체 생성 메커니즘 제시
  - 그렇다고 기존의 프로토타입 기반 객체지향 모델을 폐지하고 클래스 기반 객체지향 모델을 제공하는것은 아님
  - 클래스는 함수이며, 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있는 문법적 설탕이라고 볼 수 있다.
  - 클래스와 생성자 함수 모두 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작하지 않는다.
  - 클래스는 생서자 함수보다 엄격하고 생성자 함수에서 제공하지 않는 기능도 제공한다.

- 클래스와 생성자 함수의 차이
  클래스는 생성자 함수와 유사하게 동작하지만 다음과 같은 차이가 있다

  1. new 생성자
     클래스는 new 생성자가 없으면 에러 발생, 생성자 함수는 new 키워드가 없으면 일반 함수로 호출된다.
  2. extends, super
     클래스는 상속을 지원하는 extends와 super키워드를 제공한다. 생성자 함수는 없음
  3. 호이스팅
     클래스는 호이스팅이 발생하지 않는 것처럼 동작한다. 함수 선언문으로 선언된 생성자 함수는 함수 호이스팅, 함수 표현식(변수에 할당) 으로 정의한 함수는 변수 호이스팅이 발생한다.
  4. strict mode
     클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 있다.
  5. 프로퍼티 어트리뷰트 [[Enumerable]] 차이
     클래스의 constructor, 프로토타입 메서드, 정적 메서드 모두 [[Enumerable]] 값이 false이다.

  클래스는 프로토타입 기반의 객체지향을 구현했다는 점에서 생성자 함수와 유사하지만. 생성자 함수 기반의 객체 생성 방식보다 견고하고 명로하다.

  **💡 문법적 설탕이 아니라 새로운 객체 생성 메커니즘으로 보는 것이 합당하다.**

## 25.2 클래스 정의

1. 클래스 선언문, 클래스 표현식
   > ✅ class 키워드를 사용해서 정의
   > ✅ 클래스 이름은 파스칼 케이스 (안 지켜도 에러가 발생하지는 않음)

```javascript
//클래스 선언문
class Person {}

// 함수와 마찬가지로 표현식으로 클래스를 정의할 수도 있다
// 익명 클래스 표현식
const Person = class {};
//기명 클래스 표현식
const Person = class MyClass {};
```

클래스를 표현식으로 정의한다는 것은 클래스가 값으로 사용할 수 있는 일급 객체라는것을 의미한다. 다라서 다음과 같은 특징을 갖는다

- 무명의 리터럴로 생성할 수 있다. (런타임에 생성이 가능하다)
- 변수나 자료구조(객체,배열 등)에 저장할 수 있다
- 함수의 매개변수에 전달할 수 있다
- 함수의 반환값으로 사용할 수 있다.
  클래스는 함수이고, 값처럼 사용할 수 있는 일급 객체이다

2. 메서드 정의
   > ✅ 클래스 몸체에는 0개 이상의 메서드만 정의할 수 있다.
   > ✅ constructor(생성자), 프로토타입 메서드, 정적 메서드 세 종류의 메서드를 정의할 수 있다

```javascript
// 클래스 선언문
class Person {
  //생성자
  consturctor(name) {
    this.name = name; //public
  }
  //인스턴스 생성 및 초기화

  //프로토타입 메서드
  sayHello() {
    console.log("Hi! my name is " + this.name);
  }

  // 정적 메서드
  static sayHi() {
    console.log("hello");
  }
}

const me = new Person("lee");
console.log(me.name); //Lee
me.sayHi(); //HI! my name is Lee
Person.sayHello(); // hello
```

클래스와 생성자 함수 정의 방식을 비교하면 다음과 같다
![image](images/class_constructor_function_vs_class.jpeg)
클래스와 생성자함수 정의 방식은 형태적으로 매우 유사하다

## 25.3 클래스 호이스팅

**클래스는 함수로 평가된다**

- 클래스 선언문으로 정의한 클래스는 함수 선언문처럼 소스코드 평가 과정(런타임 이전)에 먼저 평가되어 함수 객체를 생성한다
- 이때 평가되어 생성된 함수 객체는 생성자 함수로 호출할 수 있는 함수 constructor이다
- 생성자 함수로 호출되는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입 또한 더불어 생성된다

- 단 클래스는 정의 이전에 참조할 수 없다

  예제

  - 평가 과정에서 생성자 함수 객체를 평가한다

  ```javascript
  class Person {}
  console.log(typeof Person); // function
  ```

  - 선언 전에 참조할 수 없다

  ```javascript
  console.log(Person); //ReferenceError
  class Person {}
  ```

  - 호이스팅은 일어난다

  ```javascript
  const Person = ""
  {
      console.log(Person); //호이스팅이 발생하지 않는다면 ''가 출력되어야 한다.
      //ReferenceError
      class Person{}

  }


  💡 클래스 선언문도 변수, 함수 정의처럼 호이스팅이 발생하지만 let, const 키워드처럼 일시적 사각지대에 빠지기 때문에 호이스팅이 발생하지 않는것처럼 동작한다.
  ```

## 25.4 인스턴스 생성

**클래스는 생성자 함수이며 new 연산자와 함께 호출되어 인스턴스를 생성한다.**

- 함수는 new 연산자 여부로 생성자 함수 또는 일반 함수로 사용하지만, 클래스는 인스턴스 생성만 하므로 반드시 new 연산자와 호출해야한다.

  ```javascript
  class Person {}

  const me = new Person();
  const meme = Person(); //TypeError: Class constructor Person cannt be invoked without 'new'
  ```

  **클래스 표현식은 식별자 이름으로 호출해야 한다**

- 클래스 표현식으로 정의된 클래스는 클래스를 가리키는 식별자가 아닌 기명클래스 이름으로 인스턴스를 생성하면 오류가 발생한다.

```javascript
const Person = class MyClass {};

const me = new Person();
const meme = new MyClass(); //Myclass is not defined
```

기명 함수 표현식과 마찬가지로 클래스 표현식에서 사용한 클래스 이름은 외부 코드에서 접근이 불가능하기 때문이다.

## 25.5 메서드

### 25.5.1 constructor

> 📌 constructor는 인스턴스를 생성하고 초기화 하기 위한 특수한 메서드이다  
> 📌 이름을 변경할 수 없다

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
}
```

1. 클래스는 인스턴스를 생성하기 위한 생성자 함수이다

   - 클래스는 함수 객체 고유의 프로퍼티를 모두 가지고 있다.
     -> 함수와 동일하게 프로토타입과 연결되어 있으며, 자신의 스코프체인을 구성한다.
   - 모든 함수 객체가 가지고 있는 프로토타입 객체의 constructor 프로퍼티는 클래스 자신을 가리킨다
     -> 클래스가 인스턴스를 생성하는 생성자 함수라는것을 의마한다
   - constructor 내부의 this는 클래스가 생성한 인스턴스를 가리킨다.

2. 클래스 몸체에 정의한 constructor 는 단순한 메서드가 아니다

   - 클래스의 constructor는 메서드로 해석되는게 아니라, 클래스가 생성한 함수 객체 코드의 일부가 된다.
     -> 클래스 정의가 평가 될 때, consturctor 안에 기술된 동작을 하는 함수 객체가 생성된다.
   - 클래스의 consturctor와 프로토타입의 constructor 프로퍼티는 직접적인 관련은 없다.

3. constructor의 특징
   - 클래스 내에 최대 한개만 존재할 수 있다.
   - 2개 이상의 constructor 포함 시 에러가 발생한다
   ```javascript
   class Person{
       constructor(){}
       constructor(){}
   } //SyntaxError : A class may only have one constructor
   ```

- 생략할 수 있다
  constructor를 생략하면 클래스 내에 빈 constructor가 암묵적으로 정의된다.
  ```javascript
  class Person {}
  /* 아래와 같이 빈 constructor가 암묵적으로 정의된다.
  class Person{
      constructor() {}
  }
  */
  ```
- 프로퍼티가 추가되어 초기화된 인스턴스 생성시 , constructor 내부에서 this에 인스턴스 프로퍼티를 추가

  ```javascript
  class Person {
    constructor() {
      this.name = "sangjun"; // 인스턴스생성시 name 프로퍼티 추가해서 초기화함.
    }
  }
  ```

- 매개변수로 인스턴스 프로퍼티 초기화 가능

  ```javascript
  class Person {
    constructor(nane) {
      this.name = name; // 전달받은 파라미터로 인스턴스 초기화
    }
  }

  // 인수로 초기값을 전달한다, 인수는 cosntructor에 전달된다,
  const me = new Person("Lee");
  console.log(me); //Person{ name: "Lee"};
  ```

- 🔥**construcor는 별도의 변환문을 갖지 않아야 한다**

  - 왜 why? -> new 연산자로 호출되니까  
    _17.2.3절 "생성자 함수의 인스턴스 생성 과정"에 내용이 있음_  
    new 연산자로 생성자 함수를 호출하면 암묵적으로 this, 즉 인스턴스를 반환함, 클래스도 new로 호출되므로 암묵적으로 this로 정의된 인스턴스 반환
  - this가 아닌 다른 객체를 명시적으로 반환하는 경우 인스턴스가 반환되지 않고 return 문에 명시된 객체가 반환된다

    ```javascript
    class Person {
      constructor(nane) {
        this.name = name;

        return {};
      }
    }

    // constructor에서 명시적으로 반환한 빈 객체가 반환된다.
    const me = new Person("Lee");
    console.log(me); // {};
    ```

    - 하지만 원시값을 명시적으로 반환하면 원시값 반환은 무시되고 this가 반환된다.

    ```javascript
    class Person {
      constructor(nane) {
        this.name = name;

        return 100;
      }
    }

    // constructor에서 명시적으로 반환한 빈 객체가 반환된다.
    const me = new Person("Lee");
    console.log(me); // Person{name: "Lee"};
    ```

### 25.5.2 프로토타입 메서드

> 💡 클래스 몸체에서 정의한 메서드는 기본적으로 프로토타입 메서드가 된다.

1. 생성자 함수와 차이

   ```javascript
   var Person = function () {
     function Person(name) {
       this.name = name;
     }
     // 생성자 함수에서는 prototype 프로퍼티에 메서드를 추가해야 한다
     Person.prototype.sayHi = function () {
       console.log("hello! my name is " + this.name);
     };

     return Person;
   };
   const me = new Person("Lee");
   me.sayHi(); //Hi! my name is Lee
   ```

   ```javascript
   class Person {
     constructor(name) {
       this.name = name;
     }
     // 클래스에서는 몸체에 정의하면 프로토타입 메서드가 된다
     sayHi() {
       console.log("hello! my name is " + this.name);
     }
   }
   const me = new Person("Lee");
   me.sayHi(); //Hi! my name is Lee
   ```

2. 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 된다.

   ```javascript
   class Person {...}
   const me = new Person('Lee');
   //me 객체의 프로토타입은 Person.prototype이다.

   Object.getPrototypeOf(me) === Person.prototype; // true
   me instanceof Person // true;

   Object.getPrototypeOf(Person.prototype) === Object.prototype;
   me instanceof Object // true;

    me.constructor = Person; //true;
   ```

   ![image](images/class_prototype_chain_instace.png);

   🔥 클래스와 프로토타입

   - 클래스 몸체에서 정의한 메서드는 인스턴스 프로토타입에 존재하는 프로토타입 메서드가 된다.
   - 프로토타입 체인은 모든 객체 생성 방식 뿐만 아니라 클래스에 의해 생성된 인스턴스에도 동일하게 적용된다.(생성자 함수의역할을 클래스가 한다)
   - 클래스는 생성자 함수처럼 인스턴스를 생성하는 생성자 함수로 볼 수 있다.
     ->클래스는 프로토타입 기반의 객체 생성 메커니즘이다.

### 25.5.3 정적 메서드

> 📌 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말한다.

1. 생성자 함수와 클래스에서 정적 메서드 정의 차이

```javascript
function Person(name) {
  this.name = name;
}
// 생성자 함수 에서의 정적 메서드
Person.sayHi = function () {
  console.log("Hi!");
};
Person.sayHi(); //Hi! my name is Lee
```

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  // 클래스에서는 몸체에 정의하면 프로토타입 메서드가 된다
  static sayHi() {
    console.log("Hi");
  }
}
Person.sayHi(); //Hi!
```

Person 클래스는 다음과 같이 프로토타입 체인을 생성한다.
![image](images/class_prototype_chain_static.png)

- 정적 메서드는 클래스에 바인딩된 메서드가 된다
- 클래스는 함수 객체로 평가되므로, 자신의 프로퍼티, 메서드를 소유할 수 있다.
- 클래스는 클래스 정의가 평가되는 시점에 함수 객체가 되므로, 인스턴스와 달리 별다른 생성이 필요 없다 그래서 정적 메서드는 클래스 정의 이후 인스턴스를 생성하지 않고도 호출할 수 있다.

### 25.5.4 정적 메서드와 프로토타입 메서드의 차이

- 정적 메서드와 프로토타입 메서드 차이 생각해보기
  1.  정적 메서드와 프로토타입 메서드는 자신이 속해있는 프로토타입 체인이 다르다
  2.  정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다
  3.  정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

> ✅ 정적 메서드는 애플리케이션 전역헤서 사용할 유틸림시 함수를 메서드로 구조화할 떄 사용
> ✅ 프로토타입 메서드는 메서드 내부에서 인스턴스 프로퍼티를 참조할 필요가 있을 때 사용

```javascript
class Square {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  // 메서드 내부의 this는 메서드를 소유한 객체가 아니라 메서드를 호출한 객체, . 앞에 기술된 객체에 바인딩 된다.
  area() {
    return this.width + this.height;
  }
}
const square_1 = new Square(10, 10); //인스턴스 생성

//area 메서드 내의 this는 square_1 객체에 바인딩.
square_1.area(); //프로토타입 메서드 사용,
```

```javascript
//표준 빌트인 객체의 정적 메서드
//애플리케이션 전역에서 사용되는 유틸 함수들을 하나의 네임스페이스 안에 모아서 구조화 할 수 있는 효과가 있다.
Math.max(1, 2, 3);
Number.isNaN(NaN);
JSON.stringify({ a: 1 });
Object.is({}, {});
Reflect.has({ a: 1 }, "a");
```

### 25.5.5 클래스에서 정의한 메서드의 특징

1. function 키워드를 생략한 메서드 축약 표현을 사용한다
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요없다
3. 암묵적으로 strict mode로 실행된다
4. 프로퍼티 어트리뷰트[[Enumerable]] 값이 false이다. for .. in Object.keys 로 열거할 수 없다.
5. 내부메서드 [[Construct]]를 갖지 않는다. new 연산자와 함께 호출할 수 없다

## 25.6 클래스의 인스턴스 생성 과정

> 📌 new 연산자와 함께 클래스를 호출하면 생성자 함수와 마찬가지로 클래스의 내부 메서드 [[Construct]]가 호출된다.
> 📌 17.2.3절 생성자 함수의 인스턴스 생성 과정 과 유사한 과정을 거쳐 인스턴스가 생성된다. [Link](https://github.com/Endless-Creation-32nd/javascript-deep-dive/tree/main/17%EC%9E%A5-%EC%83%9D%EC%84%B1%EC%9E%90%20%ED%95%A8%EC%88%98%EC%97%90%20%EC%9D%98%ED%95%9C%20%EA%B0%9D%EC%B2%B4%20%EC%83%9D%EC%84%B1)

1. 인스턴스 생성과 this 바인딩
   - new 연산자와 함꼐 클래스를 호출하면 constructor 메서드의 내부 코드가 실행되며 암묵적으로 빈 객체가 생성된다
   - 이 빈 객체가 클래스가 생성한 인스턴스이다
   - 클래스가 생성한 인스턴스의 프로토타입으로 클래스의 prototype 프로퍼티가 가리키는 객체가 설정된다
   - 인스턴스가 this에 바인딩된다.
     - consturctor 내부의 this는 클래스가 생성한 인스턴스를 가리키게 된다.
2. 인스턴스 초기화
   - consturctor 내부 코드가 실행되며 this에 바인딩 된 인스턴스를 초기화한다.
     - this에 바인딩 된 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스 프로퍼티 값을 초기화한다.
3. 인스턴스 반환
   - 모든 처리가 끝나면 인스턴스가 바인딩 되어있는 this가 암묵적으로 반환된다.

```javascript
class Person {
  constructor(name) {
    // 1. 암묵적으로 빈 객체의 인스턴스 생성, this에 바인딩.
    console.log(this);
    console.loo(Object.getPrototypeOf(this) === Person.prototype); //ture

    //2. this에 바인딩 된 인스턴스 프로퍼티를 초기화
    this.name = name;
    //3. 암묵적으로 인스턴스가 바인딩 된 this가 반환.
  }
}
```

## 25.7 프로퍼티

### 25.7.1 인스턴스 프로퍼티

- constructor 내부에서 this에 추가한 프로퍼티는 언제나 클래스가 생성한 인스턴스의 프로퍼티가 된다

### 25.7.2 접근자 프로퍼티

- 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티 값을 읽거나 사용할 때 쓰는 접근자 함수. getter, setter 로 구성되어 있다
- 메서드 이름 앞에 get, set 키워드를 사용해 정의한다
- getter, setter 이름은 인스턴스 프로퍼티처럼 사용된다.
- 클래스의 메서드는 기본적으로 프로토타입 메서드가 된다. 따라서 접근자 프로퍼티 또한 인스턴스 프로퍼티가 아닌 프로토타입의 프로퍼티가 된다.

### 25.7.3 클래스 필드 정의 제안

> 📌 클래스 필드란 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어이다.

- 주의사항
  - 자바는 클래스 필드를 몸체 내에 바로 선언 가능
  - 자바스크립트는 constructor 내부this에 프로퍼티를 추가해야만 사용 가능
    - 클래스 몸체에서 클래스 필드를 정의할 수 있는 클래스 필드 정의 제안이 아직 표준 사양으로 승급되지 않았지만 브라우저 및 node 12에서는 클래스 몸체에 필드를 생성해도 정의가 된다
    - 표준 사양으로 승급이 확실시되는 이 제안을 미리 구현해 두어서 그렇다.

**클래스 필드 정의 관련 정리**

1. 클래스 몸체 내에서 this에 바인딩 금지

- this는 클래스 constructor와 메서드 내에서만 유효하다.

  ```javascript
  class Person {
      this.name = ''; //Syntax error
      constructor(name) {}
  }
  ```

2. 클래스 필드를 참조하는 경우 반드시 this 사용하기

```javascript
class Person {
  name = "Lee";
  constructor(name) {
    console.log(name); // ReferenceErrorl
  }
}
```

3. 클래스 필드에 초기값이 없으면 undefined

```javascript
class Person {
  name;
}
const me = new Person();
me.name; // undefined;
```

4. 함수 클래스 필드에 할당 가능

   - 함수는 일급 객체니까 필드(프로퍼티)에 할당 가능

   ```javascript
   class Person {
     name = "Lee";
     getName = function () {
       return this.name;
     };
   }
   const me = new Person();
   me.getName(); // undefined;
   ```

   - 클래스 필드에 함수를 할당하는 경우, 프로토타입 메서드가 아닌 인스턴스 메서드가 된다. 모든 클래스 필드는 인스턴스 프로퍼티가 되기 때문이다.
   - 따라서 이 방법은 권장되지 않음.
   - 화살표 함수를 클래스 필드에 할당하여 화살표 함수 내부의 This가 인스턴스를 가리키게 할 수 있다
   - 권장되지 않음..

### 25.7.4 private 필드 정의 제안

> 💡 private 필드 선두에는 #을 붙여서 사용한다

- 현재 표준 사양은 아니지만, 표준 사양으로 승급이 확실시 되어 있어서 최신 브라우저, node 12 에서는 이미 구현되어 있다.

```javascript
class Person {
  #name = "";
  constructor(name) {
    this.#name = name;
  }
}
const me = new Person("Lee");
me.#name(); //SyntaxError : Private Ffiled '#name' must be declared in an enclosing class
```

- 클래스 내부에서 private 필드에 직접 접근할 수는 없다. 접근자 프로퍼티를 통해 간접적으로 접근할 수는 있다.

```javascript
class Person {
  #name = "";
  constructor(name) {
    this.#name = name;
  }
  get name() {
    //private 참조하여 trim 한 뒤 반환
    return this.#name.trim();
  }
}
const me = new Person("Lee");
me.name; //"Lee"
```

- private 필드는 반드시 몸체 내에 정의해야 한다.

### 25.7.5 static 필드 정의 제안

> 이것도 아직 표준 사양은 아니지만, 최신 브라우저에 정의되어 있음.
> staric 메서드 사용한 것처럼 전체 애플리케이션에서 인스턴스 생성 없이 사용할 떄 static 필드를 정의해서 사용

```javascript
class MyMath {
  //static 필드 정의
  static PI = 22 / 7;
  //static private 필드 정의
  static #num = 10;
  //static 메서드
  static increament() {
    return ++MyMath.#num;
  }
}

console.log(MyMath.PI);
console.log(MyMath.increament());
console.log(MyMath.#num);
```

## 25.8 상속에 의한 클래스 확장

### 25.8.1 클래스 상속과 생성자 함수 상속

- 프로토타입 기반 상속은 프로토타입 체인을 통해 다른 객체의 자산을 상속받는 개념이지만, `상속에 의한 클래스 확장`은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것!
- 클래스와 생성자 함수는 `인스턴스`를 생설할 수 있는 함수라는 점에서 매우 유사하지만, 클래스는 상속을 통해 기존 클래스를 확장할 수 있는 문법이 기본적으로 제공되지만 생성자 함수는 그렇지 않다는 차이점이 존재한다.
- 클래스는 상속을 통해 다른 클래스를 확장할 수 있는 문법인 `extends` 키워드가 기본적으로 제공된다.

<br>

### 25.8.2 extends 키워드

- 상속을 통해 클래스를 확장하려면 `extends` 키워드를 사용하여 상속받을 클래스를 정의한다.
- **상속을 통해 확장된 클래스** - 서브클래스 또는 자식 클래스라고 불림
- **서브클래스에게 상속된 클래스** - 수퍼클래스 또는 부모 클래스라고 불림
- `extends` 키워드의 역할은 수퍼클래스와 서브클래스 간의 상속 관계를 설정하는 것
  - 인스턴스의 프로토타입 체인뿐 아니라 클래스 간의 프로토타입 체인도 생성한다.
    - 이를 통해 프로토타입 메서드, 정적 메서드 모두 상속이 가능하다.
- 클래스도 프로토타입을 통해 상속 관계를 구현한다.

<br>

### 25.8.3 동적 상속

- `extends` 키워드는 클래스뿐만 아니라 생성자 함수를 상속받아 클래스를 확장할 수도 있다. 단, `extends` 키워드 앞에는 반드시 클래스가 와야한다.

- `extends` 키워드 다음에는 클래스뿐만 아니라 [[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다. 이를 통해 동적으로 상속받을 대상을 결정할 수 있다.

  ```javascript
  function Base1() {}

  class Base2 {}

  let condition = true;

  // 조건에 따라 동적으로 상속 대상을 결정하는 서브클래스
  class Derived extends (condition ? Base1 : Base2) {}

  const derived = new Derived();
  console.log(derived); // Derived {}

  console.log(derived instanceof Base1); // true
  console.log(derived instanceof Base2); // false
  ```

<br>

### 25.8.4 서브클래스의 constructor

- 수퍼 클래스와 서브클래스 모두 `constructor`를 생략하면 빈 객체가 생성된다. 프로퍼티를 소유하는 인스턴스를 생성하려면 `constructor` 내부에서 인스턴스에 프로퍼티를 추가해야한다.

  ```javascript
  // 수퍼 클래스
  class Base {
    constructor() {}
  }

  // 서브 클래스
  class Derived extends Base {
    constructor(...args) {
      super(...args);
    }
  }

  const derived = new Derived();
  console.log(derived); // Derived {}
  ```

<br>

## 25.8.5 super 키워드

- super 키워드는 함수처럼 호출할 수도 있고 `this`와 같이 식별자처럼 참조할 수 있는 특수한 키워드다.

<br>

### super 호출

- super를 호출하면 수퍼클래스의 constructor를 호출한다.

- 만약, 수퍼클래스의 constructor 내부에서 추가한 프로퍼티를 그대로 갖는 인스턴스를 생성한다면 서브클래스의 `constructor`를 생략할 수 있다.

  - 이때, new 연산자와 함께 서브클래스를 호출하면서 전달한 인수는 모두 서브클래스에 암묵적으로 정의된 `constructor`의 super 호출을 통해 수퍼클래스의 `constructor`에 전달된다.

    ```javascript
    // 수퍼 클래스
    class Base {
      constructor(a, b) {
        this.a = a;
        this.b = b;
      }
    }

    // 서브 클래스
    class Derived extends Base {
      // 다음과 같이 암묵적으로 constructor가 정의된다.
      // constructor(...args) { super(...args); }
    }

    const derived = new Derived(1, 2);
    console.log(dervied); // Derived { a:1, b:2 }
    ```

<br>

- 위와 다르게, 수퍼클래스에서 추가한 프로퍼티와 서브클래스에서 추가한 프로퍼티를 갖는 인스턴스를 생성한다면 서브클래스의 `constructor`를 생략할 수 없다.

  - 이때, new 연산자와 함께 서브클래스를 호출하면서 전달한 인수 중에서 수퍼클래스의 constructor에 전달할 필요가 있는 인수는 서브클래스의 `constructor`에서 호출하는 super를 통해 전달된다.

    ```javascript
    class Base {
      constructro(a, b) {
        this.a = a;
        this.b = b;
      }
    }

    // 서브 클래스
    class Derived extends Base {
      constructor(a, b, c) {
        super(a, b);
        this.c = c;
      }
    }

    const derived = new Derived(1, 2, 3);
    console.log(derived); // Derived {a : 1, b: 2, c: 3}
    ```

<br>

- `super`를 호출할 때 주의할 사항들은 다음과 같다.
  - 서브클래서에서 `constructor`를 생략하지 않는 경우 서브클래스와 constructor에서는 반드시 super를 호출해야한다.
  - 서브클래스의 `constructor`에서 super를 호출하기 전에는 this를 참조할 수 없다.
  - super는 반드시 서브클래스의 `constructor`에서만 호출한다. 서브클래스가 아닌 클래스의 constructor나 함수에서 super를 호출하면 에러가 발생한다.

<br>

### super 참조

- 메서드 내에서 super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.

  - 서브클래스의 프로토타입 메서드 내에서 super.sayHi는 수퍼클래스의 프로토타입 메서드 sayHi를 카리킨다.

    ```javascript
    // 수퍼클래스
    class Base {
      constructor(name) {
        this.name = name;
      }

      sayHi() {
        retunr`Hi ${this.name}`;
      }
    }

    // 서브클래스
    class Derived extends Base {
      sayHi() {
        // super.sayHi는 수퍼클래스의 프로토타입 메서드를 가리킨다.
        return `${super.sayHi()}`;
      }
    }
    ```

    - super 참조를 통해 수퍼클래스의 메서드를 참조하려면 super가 수퍼클래스의 메서드가 바인딩된 객체, 즉 수퍼클래스의 prototype 프로퍼티에 바인딩된 프로토타입을 참조할 수 있어야한다.

  - 서브 클래스의 정적 메서드는 수퍼클래스의 정적 메서드를 가리킨다.

<br>

### 25.8.6 상속 클래스의 인스턴스 생성 과정

### 1. 서브클래스의 super 호출

- 자바스크립트 엔진은 클래스를 평가할 때 수퍼클래스와 서브클래스를 구분하기 위해 'base' 또는 'derived'를 값으로 갖는 내부 슬롯 [[ConstructorKind]]를 갖는다.
- 다른 클래스를 상속받지 않는 클래스(그리고 생성자함수)는 new 연산자와 함께 호출되었을 때 암묵적으로 빈 객체, 즉 인스턴스를 생성하고 이를 `this`에 바인딩한다.
- 하지만 서브클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼클래스에게 인스턴스 생성을 위임한다.
  - 이것이 바로 서브클래스의 `constructor`에 반드시 super를 호출해야하는 이유다.
  - 서브클래스가 new 연산자와 함께 호출되면 서브클래스 constructor 내부의 super 키워드가 함수처럼 호출된다. -> super가 호출되면 수퍼클래스의 `constructor`가 호출된다.

<br>

### 2. 수퍼클래스의 인스턴스 생성과 this 바인딩

- 수퍼클래스의 constructor 내부의 코드가 실행되기 이전에 암묵적으로 빈 객체를 생성한다. 이 빈 객체가 바로 클래스가 생성한 인스턴스다.
- 그리고 암묵적으로 생성된 빈 객체, 즉 인스턴스는 `this`에 바인딩된다. -> 따라서 수퍼클래스의 constructor 내부의 `this`는 생성된 인스턴스를 가리킨다.
- 이때, 인스턴스는 수퍼클래스가 생성한 것이지만, new 연산자와 함께 호출된 클래스가 서브클래스이기 때문에 호출된 함수를 가리키는 `new.target`이 서브클래스를 가리킨다.
  - 따라서, 인스턴스는 `new.target`이 가리키는 서브클래스가 생성한 것으로 처리된다.
  - 최종적으로, 생성된 인스턴스의 프로토타입은 서브클래스의 `prototype` 프로퍼티가 가리키는 객체다.

<br>

### 3. 수퍼클래스의 인스턴스 초기화

- 수퍼클래스의 `constructor`가 실행되어 `this`에 바인딩 되어 있는 인스턴스를 초기화한다.
- 즉, this에 바인딩 되어 있는 인스턴스에 프로퍼티를 추가하고 `constructor`가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티를 초기화한다.

<br>

### 4. 서브클래스 constructor로의 복귀와 this 바인딩

- super의 호출이 종료되고 제어 흐름이 서브클래스 `constructor`로 돌아온다. 이때 super가 반환한 인스턴스가 `this`에 바인딩된다.
  - 서브클래스는 별도의 인스턴스를 생성하지 않고 super가 반환한 인스턴스를 `this`에 바인딩하여 그대로 사용한다.

<br>

### 5. 서브클래스의 인스턴스 초기화

- super 호출 이후, 서브클래스의 constructor에 기술되어 있는 인스턴스 초기화가 실행된다.

<br>

### 6. 인스턴스 반환

- 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 `this`가 암묵적으로 반환된다.
