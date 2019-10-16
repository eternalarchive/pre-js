# 16. 생성자 함수에 의한 객체 생성

- 객레 리터럴 표기법 : 객체를 생성하는 방법 중 가장 일반적이고 간단한 방법

## 1. Object 생성자 함수

- new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환
  - 빈 객체를 생성한 이후 프로퍼티 또는 메소드를 추가하여 객체 완성할 수 있음
- 생성자(constructor) 함수 : new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수
- 인스턴스(instance) : 생성자 함수에 의해 생성된 객체
  - 객체가 메모리에 저장되어 실제로 존재하는 것에 초점을 맞춘 용어
  - 생성자 함수도 객체이기 때문에 생성자 함수나 클래스가 생성한 객체를 다른 객체와 구분하기 위해 인스턴스라 함

```javascript
// 빈 객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = 'Lee';
person.sayHello = function () {
  console.log('Hi! My name is ' + this.name);
};

console.log(person); // {name: "Lee", sayHello: ƒ}
person.sayHello(); // Hi! My name is Lee
```

- 자바스크립트는 Object 생성자 함수 이외에도 String, Number, Boolean, Function, Array, Date, RegExp 등의 빌트인(intrinsic 내장) 생성자 함수를 제공

```javascript
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee');
console.log(typeof strObj); // object
console.log(strObj);        // String {"Lee"}

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123);
console.log(typeof numObj); // object
console.log(numObj);        // Number {123}

// Boolean 생성자 함수에 의한 Boolean 객체 생성
const boolObj= new Boolean(true);
console.log(typeof boolObj); // object
console.log(boolObj);        // Boolean {true}

// Function 생성자 함수에 의한 Function 객체(함수) 생성
const func = new Function('x', 'return x * x');
console.log(typeof func); // function
console.dir(func);        // ƒ anonymous(x )

// Array 생성자 함수에 의한 Array 객체(배열) 생성
const arr = new Array(1, 2, 3);
console.log(typeof arr); // object
console.log(arr);        // (3) [1, 2, 3]

// RegExp 생성자 함수에 의한 RegExp 객체(정규 표현식) 생성
const regExp = new RegExp(/ab+c/i);
console.log(typeof regExp); // object
console.log(regExp);        // /ab+c/i

// Date 생성자 함수에 의한 Date 객체 생성
const date = new Date();
console.log(typeof date); // object
console.log(date);        // Tue Mar 19 2019 02:38:26 GMT+0900 (한국 표준시)
```

- 반드시 Object 생성자 함수를 사용해 빈 객체를 생성해야 하는 것은 아님
  - 객체 리터럴이 더 간편
  - 특별한 이유가 없다면 그다지 유용하지는 않음



## 2. 생성자 함수

### 2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

- 객체 리터럴에 의한 객체 생성 방식은 직관적이고 간편
- 객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만을 생성
- 객체 리터럴에 의한 객체 생성 방식의 문제점 : 동일한 프로퍼티를 갖는 객체를 여러개 생성해야 하는 경우, 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적

```javascript
const circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  }
};

console.log(circle1.getDiameter()); // 10

const circle2 = {
  radius: 10,
  getDiameter() {
    return 2 * this.radius;
  }
};

console.log(circle2.getDiameter()); // 20
```

- 객체는 프로퍼티를 통해 객체 고유의 상태(state)를 표현
- 메소드를 통해 상태 데이터인 프로퍼티를 참조하고 조적하는 동작(behavior)을 표현
  - 프로퍼티는 객체마다 프로퍼티 값이 다를 수는 있지만 메소드는 내용이 동일한 경우가 일반적
- 위의 circle1, circle2 두 객체는 프로퍼티 구조가 동일
  - 객체 고유의 상태 데이터인 radius 프로퍼티 값은 객체마다 다를 수 있지만 getDiameter 메소드는 완전히 동일
  - 하지만 객체 리터럴에 의해 객체를 생성하는 경우, 프로퍼티 구조가 동일함에도 불구하고 매번 같은 프로퍼티와 메소드를 기술해야 함
  - 한 두개라면 넘어갈수도 있겠지만 만약 수십개의 객체를 생성해야 한다면 문제



### 2.2 생성자 함수에 의한 객체 생성 방식의 장점

- 생성자 함수에 의한 객체 생성 방식의 장점 : 마치 객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있음

```javascript
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스의 생성
const circle1 = new Circle(5);  // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

- this : this는 객체 자신의 프로퍼티나 메소드를 참조하기 위한 자기 참조 변수(Self-referencing variable)
  - this가 가리키는 값, 즉 this 바인딩은 함수 호툴 방식에 따라 동적으로 결정

| 함수 호출 방식       | this가 가리키는 값                     |
| :------------------- | :------------------------------------- |
| 일반 함수로서 호출   | 전역 객체                              |
| 메소드로서 호출      | 메소드를 호출한 객체                   |
| 생성자 함수로서 호출 | 생성자 함수가 (미래에) 생성할 인스턴스 |

```javascript
// 함수는 다양한 방식으로 호출될 수 있다.
function foo() {
  console.log(this);
}

// 일반적인 함수로서 호출
// 전역 객체는 브라우저 환경에서는 window, Node.js 환경에서는 global을 가리킨다.
foo(); // window

// 메소드로서 호출
const obj = { foo }; // ES6 프로퍼티 축약 표현
obj.foo(); // obj

// 생성자 함수로서 호출
const inst = new foo(); // inst
```

- 생성자 함수는 이름 그대로 객체(인스턴스)를 생성하는 함수 : 자바스크립트의 생성자 함수는 자바와 같은 클래스 기반 객체지향 언어의 생성자(constructor)와는 다르게 그 형식이 정해져 있는 것이 아니라 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 **new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작**
  - new 연산자와 함께 생성자 함수를 호출하지 않으면 일반 함수로 동작

```javascript
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다.
// 즉, 일반적인 함수의 호출이다.
const circle3 = Circle(15);

// 일반 함수 Circle은 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3); // undefined

// 일반 함수 Circle내의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```



### 2.3 내부 메소드 [[Call]]과 [[Construct]]

- 함수 선언문 또는 함수 표현식으로 정의한 함수는 일반적인 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있음
  - 생성자 함수로서 호출한다는 것은 new연산자와 함께 호출하여 객체를 생성하는 것을 의미
- 함수는 객체이므로 일반 객체(Ordinary object)와 동일하게 동작 가능
  - 함수 객체는 일반 객체의 내부 슬롯과 내부메소드를 모두 가지고 있음

```javascript
// 함수는 객체이다.
function foo() {}

// 함수는 객체이므로 프로퍼티를 소유할 수 있다.
foo.prop = 10;

// 함수는 객체이므로 메소드를 소유할 수 있다.
foo.method = function () {
  console.log(this.prop);
};

foo.method(); // 10
```

- 함수 객체는 함수로 동작하기 위해 일반 객체의 내부 슬롯과 내부 메소드 이외에 아래와 같은 내부 슬롯과 내부 메소드를 추가적으로 갖고 있음

> 함수 객체의 내부 슬롯

![](https://poiemaweb.com/assets/fs-images/16-1.png)

> 함수 객체의 내부 메소드

![](https://poiemaweb.com/assets/fs-images/16-2.png)

- 내부 메소드 [[Call]]을 갖는 함수 객체를 callable
  - 호출할 수 있는 객체, 즉 함수
- 내부 메소드인 [[Construct]]를 갖는 함수 객체를 constructor
  - 생성자 함수로서 호출할 수 있는 객체로, 생성자 함수로 호출할 수 있다는 것은 new 연산자(또는 super 연산자)와 함께 호출하는 것을 의미
- [[Construct]]를 갖지 않는 함수 객체를 non-constructor
- 함수가 일반적 함수로 호출되면 함수 객체 내부 메소드 [[Call]]이 호출되고 new 연산자(또는 super 연산자)와 함께 생성자 함수로 호출되면 내부 메소드 [[Construct]]가 호출

```javascript
function foo() {}

// 일반적인 함수로서 호출: [[Call]]이 호출된다.
foo();

// 생성자 함수로서 호출: [[Construct]]가 호출된다.
new foo();
```

- super 키워드 : super는 ES6에서 도입된 클래스에서 부모 클래스를 참조(this와 유사)할 때 부모 클래스의 생성자(constructor)를 호출할 때 사용

- 호출할 수 없는 객체는 함수 객체가 아니므로 함수로서 기능하는 객체, 즉 함수 객체는 반드시 callable이어야 함
  - 따라서 모든 함수 객체는 내부 메소드 [[Call]]을 가지고 있으므로 호출 가능
  - 하지만 모든 함수 객체가 [[Construct]]를 갖는 것은 아님
    - 함수 객체는 constructor일 수도, non-constructor 일수도 있음
- 함수 객체는 callable이면서 constructor이거나 callable이면서 non-constructor
  - 즉, 모든 함수 객체는 호출할 수 있지만 모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아님

![](https://poiemaweb.com/assets/fs-images/16-3.png)

- 자바스크립트 엔진이 어떻게 둘을 구분하는지 살펴보자



### 2.4 constructor와 non-constructor의 구분

- 자바스크립트 엔진은 함수를 생성할 때, FunctionCreate라는 추상 연산(abstract operation)을 사용

- 추상 연산(abstract operation) : ECMAScript 사양에서 내부 동작의 구현 알고리즘을 표현한 것으로 ECMAScript 사양에서 설명을 위해 사용되는 함수와 유사한 의사 코드라고 이해하자

![](https://poiemaweb.com/assets/fs-images/16-4.png)

- 추상 연산 FunctionCreate는 함수 정의가 평가될 때 호출
  - 이 때 함수 정의 방식에 따라 FunctionCreate의 첫 번째 매개 변수 kind에 함수의 종류를 나타내는 문자열이 전달

| 구분                                                 | 함수의 종류를 나타내는 문자열 |
| :--------------------------------------------------- | :---------------------------- |
| 일반 함수 정의(함수 선언문, 함수 표현식)를 평가할 때 | Normal                        |
| 화살표 함수 정의를 평가할 때                         | Arrow                         |
| 메소드 정의를 평가할 때                              | Method                        |

- 이 때 주의할 것은 ECMAScript 사양에서 "메소드 정의"로 인정하는 범위가 일반적인 메소드보다 좁다는 것

```javascript
// 일반 함수 정의 : kind = 'Normal'
function foo() {}
const bar = function () {};
// 프로퍼티 x에 할당된 것은 일반 함수 정의이다. 메소드 정의로 인정하지 않는다.
const baz = {
  x: function () {}
};

// 일반 함수로 정의된 함수만이 constructor이다.
new foo(); // OK
new bar(); // OK
new baz.x(); // OK

// 화살표 함수 정의 : kind = 'Arrow'
const arrow = () => {};

new arrow(); // TypeError: arrow is not a constructor

// 메소드 정의 : kind = 'Method'
// ES6의 메소드 축약 표현만을 메소드 정의로 인정한다.
const obj = {
  x() {}
};

new obj.x(); // TypeError: obj.x is not a constructor
```

- 일반적으로 프로퍼티의 값인 함수는 모두 메소드로 통칭
  - 하지만 ECMAScript 사양에서 "메소드 정의"란 ES6의 메소드 축약 표현만을 의미
  - 함수가 어디에 할당되어 있는지에 따라 메소드 정의인지를 판단하는 것이 아니라 함수 정의 방식에 따라 함수의 종류 구분
- 일반 함수로 정의된 함수만이 constructor
  - 함수의 종류가 Arrow, Method인 함수는 non-constructor
  - 일반 함수로 정의된 함수만이 생성자 함수로서 호출될 수 있음
- 함수가 일반적인 함수로 호출되면 함수 객체 내부 메소드 [[Call]]이 호출되고 new 또는 super 연산자와 함께 생성자 함수로 호출되면 내부 메소드 [[Construct]]가 호출

```javascript
function foo() {}

// 일반적인 함수로서 호출: [[Call]]이 호출된다.
// 모든 함수 객체는 [[Call]]이 구현되어 있다.
foo();

// 생성자 함수로서 호출: [[Construct]]가 호출된다.
// 이때 [[Construct]]를 갖지 않는다면 에러가 발생한다.
new foo();
```

- non-constructor인 함수는 내부 메소드 [[Construct]]를 갖지 않으므로 non-constructor인 함수 객체를 생성자 함수로 호출하면 에러 발생
- 주의할 것은 생성자 함수로 호출될 것을 기대하고 정의하지 않은 일반 함수(callable이면서 constructor)에 new 연산자를 붙여 호출하면 생성자 함수처럼 동작할 수 있다는 것(위 예제와 같이 일반 함수를 생성자 함수로 호출)
- 생성자 함수는 일반적으로 첫문자를 대문자로 기술하는 파스칼 케이스로 명명



### 2.5 생성자 함수의 인스턴스 생성 과정

- 먼저 생성자 함수의 함수 몸체에서 수행해야 하는것이 무엇인가?
  - 생성자 함수의 역할은 1)프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿(클래스)로 동작하여 인스턴스를 생성하는 것
  - 2)생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당)하는 것

- 생상자 함수가 인스턴스를 생성하는 것은 필수이고, 생성된 인스턴스를 초기화 하는 것은 옵션

```javascript
// 생성자 함수
function Circle(radius) {
  // 인스턴스 초기화
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스 생성
const circle1 = new Circle(5);  // 반지름이 5인 Circle 객체를 생성
```

- 생성자 함수 내부 코드를 살펴보면 this에 프로퍼티를 추가하고 필요에 따라 인수로 전달된 초기값을 프로퍼티 값으로 할당
  - 하지만 인스턴스를 생성하고 반환하는 코드는 보이지 않음
- 자바스크립트 엔진은 암묵적인 처리를 통해 인스턴스를 생성하고 반환
  - new 연산자와 함께 생성자 함수가 호출되면 자바스크립트 엔진은 아래와 같은 과정을 거쳐 인스턴스를 생성하고 초기화한 후 인스턴스 반환

1. 인스턴스 생성과 this 바인딩 암묵적으로 빈 객체 생성

- 이 빈 객체가 바로 (아직 완성되진 않았지만) 생성자 함수가 생성한 인스턴스
  -> 이 때 생성자 함수가 생성한 인스턴스의 프로토타입으로 생성자 함수의 prototype 프로퍼티가 가리키는 객체가 설정
  -> 그리고 암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩
  생성자 함수 내부의 this가 생성자 함수가 생성할 인스턴스를 가리키는 이유가 바로 이 것

- 바인딩(binding) : 식별자와 값을 연결하는 과정. 예를들어 변수는 할당에 의해 값이 바인딩

```javascript
function Circle(radius) {
  // 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩된다.
  console.log(this); // Circle {}
  console.log(Object.getPrototypeOf(this) === Circle.prototype); // true


  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
```

2. 인스턴스 초기화

- 생성자 함수에 기술되어 있는 코드가 실행되어 this에 바인딩되어 있는 인스턴스 초기화
- 즉, this에 바인딩되어 있는 인스턴스에 프로퍼티나 메소드를 추가하고 생성자 함수가 인스로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값 할당 -> 이 처리는 개발자가 기술

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}
```

- 인스턴스 반환
  - 생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: ƒ}
```

- 만약 this가 아닌 다른 객체를 명시적으로 반환하면 this가 반환되지 못하고 return 문에 명시한 객체가 반환

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 3. 암묵적으로 this를 반환한다.
  // 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다.
  return {};
}

// 인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
const circle = new Circle(1);
console.log(circle); // {}
```

- 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 3. 암묵적으로 this를 반환한다.
  // 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.
  return 100;
}

// 인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: ƒ}
```

- 이처럼 생성자 함수 내부에서 명시적으로 this가 아닌 다른 값을 반환하는 것은 생성자 함수의 기본 동작 훼손
  - 따라서 생성자 함수 내부에서 return문 반드시 생략



### 2.6 new 연산자

- 일반 함수와 생성자 함수에 특별한 형식적 차이는 없음
- new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작
  - 단, new 연산자와 함께 호출하는 함수는 non-constructor가 아닌 constructor여야 함

```javascript
// 생성자 함수로서 정의하지 않은 일반 함수
function add(x, y) {
  return x + y;
}

// 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출
let inst = new add();
// 함수가 객체를 반환하지 않았으므로 반환문이 무시된다. 따라서 빈 객체가 생성되어 반환된다.
console.log(inst); // {}

// 객체를 반환하는 일반 함수
function createUser(name, role) {
  return { name, role };
}

// 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출
inst = new createUser('Lee', 'admin');
// 함수가 생성한 객체를 반환한다.
console.log(inst); // {name: "Lee", role: "admin"}
```

- 반대로 new 연산자 없이 생성자 함수를 호출하면 일반 함수로 호출
  - 다시 말해, 함수 객체의 내부 메소드 [[Construct]]가 호출되는 것이 아니라 [[Call]]이 호출

```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수 호출하면 일반 함수로서 호출된다.
const circle = Circle(5);
console.log(circle); // undefined

// 일반 함수 내부의 this는 전역 객체 window를 가리킨다.
console.log(radius); // 5
console.log(getDiameter()); // 10

circle.getDiameter(); //
```

- Circle 함수를 new 연산자와 함께 생성자 함수로서 호출하면 함수 내부의 this는 Circle 생성자 함수가 생성할 인스턴스를 가리킴
  - 하지만 Circle 함수를 일반적인 함수로 호출하면 함수 내부의 this는 전역 객체 window를 가리킴
- 위 예제의 Circle 함수는 일반 함수로 호출되었으므로 Circle 함수 내부의 this는 전역 객체 window를 가리킴
  - radius프로퍼티와 getDiameter 메소드는 전역 객체의 프로퍼티와 메소드가 됨
- 일반 함수와 생성자 함수에 특별한 형식적 차이는 없음
  - 생성자 함수는 파스칼 케이스로 명명하여 일반 함수와 구별할 수 있도록 노력



### 2.7 new.target

- new 연산자 없이 생성자 함수를 호출하는 것을 방지하기 위해 파스칼 케이스 컨벤션 사용한다 하더라도 실수는 언제나 발생 가능
  - 이러한 위험성 회피하기 위해 ES6에서는 new.target을 지원
- new.target은 this와 유사하게 모든 함수 내부에서 지역 변수와 같이 사용되며 메타 프로퍼티(meta property)라고 부름
  - IE는 new.target을 지원하지 않으므로 주의
- 함수 내부에서 new.target을 사용하면 new 연산자와 함께 함수가 호출되었는지 확인할 수 있음
  - 함수가 new 연산자와 함께 호출되면 함수 내부의 new.target은 함수 자신을 가리킴
  - new 연산자 없이 호출된 함수 내부의 new.target은 undefined

```javascript
// 생성자 함수
function Circle(radius) {
  // 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined이다.
  if (!new.target) {
    // new 연산자와 함께 호출하여 생성된 인스턴스를 반환한다.
    return new Circle(radius);
  }

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출하여도 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter());
```

- 스코프 세이프 생성자(Scope-Safe Constructor) 패턴
  - new.target은 ES6에서 도입된 최신 문법으로 IE에서는 지원하지 않음
  - new.target을 사용할 수 없는 상황이라면 스코프 세이프 생성자(Scope-Safe Constructor) 패턴 사용 가능

```javascript
// Scope-Safe Constructor Pattern
function Circle(radius) {
  // 생성자 함수가 new 연산자와 함께 호출되면 함수의 선두에서 빈 객체를 생성하고
  // this에 바인딩한다. 이때 this와 Circle은 프로토타입에 의해 연결된다.

  // 이 함수가 new 연산자와 함께 호출되지 않았다면 이 시점의 this는 전역 객체 window를 가리킨다.
  // 즉, this와 Circle은 프로토타입에 의해 연결되지 않는다.
  if (!(this instanceof Circle)) {
    // new 연산자와 함께 호출하여 생성된 인스턴스를 반환한다.
    return new Circle(radius);
  }

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출하여도 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter()); // 10
```

- new 연산자와 함께 생성자 함수에 의해 생성된 객체(인스턴스)는 프로토타입에 의해 생성자 함수와 연결
  - 이를 이용해 new 연산자와 함께 호출되었는지 확인 가능
  - 프로토타입과 instanceof 연산자에 대해서는 나중에 살펴볼 것
- 대부분의 빌트인 생성자 함수(Object, String, Number, Boolean, Function, Array, Date, RegExp, Promise 등)는 new 연산자와 함께 호출되었는지를 확인한 후 적절한 값을 반환
  - 예를 들어, Object 또는 Function 생성자 함수는 new 연산자 없이 호출해도 new 연산자와 함께 호출했을 때와 동일하게 동작

```javascript
let obj = new Object();
console.log(obj); // {}

obj = Object();
console.log(obj); // {}

let f = new Function('x', 'return x ** x');
console.log(f); // ƒ anonymous(x) { return x ** x }

f = Function('x', 'return x ** x');
console.log(f); // ƒ anonymous(x) { return x ** x }
```

- 하지만 String 생성자 함수는 new 연산자와 함께 호출했을 때 String 객체를 생성하여 반환하지만 new 연산자 없이 호출하면 문자열 리터럴 반환
  - 즉, 문자열로 타입을 변환

```javascript
let s = new String('abc');
console.log(s); // String {"abc"}

s = String('abc');
console.log(s); // abc
```



Reference : https://poiemaweb.com/