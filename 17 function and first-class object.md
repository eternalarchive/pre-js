# 17. 함수와 일급 객체

## 1. 일급 객체

- 아래와 같은 조건을 만족하는 객체를 일급 객체(first-class object)라 함
  - 무명의 리터럴로 생성할 수 있음 - 즉, 런타임에 생성 가능
  - 변수나 자료 구조(객체, 배열)에 저장 가능
  - 함수의 매개 변수에 전달 가능
  - 함수의 결과값으로 반환 가능
- 자바스크립트의 함수는 아래 예제와 같이 위의 조건을 모두 만족하므로 일급 객체

```javascript
// 1. 함수는 무명의 리터럴로 생성할 수 있다.
// 2. 함수는 변수에 저장할 수 있다.
// 런타임(할당 단계)에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당된다.
const increase = function (num) {
  return ++num;
};

const decrease = function (num) {
  return --num;
};

// 2. 함수는 객체에 저장할 수 있다.
const predicates = { increase, decrease };

// 3. 함수의 매개 변수에게 전달할 수 있다.
// 4. 함수의 반환값으로 사용할 수 있다.
function makeCounter(predicate) {
  let num = 0;

  return function () {
    num = predicate(num);
    return num;
  };
}

// 3. 함수는 매개 변수에게 함수를 전달할 수 있다.
const increaser = makeCounter(predicates.increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

// 3. 함수는 매개 변수에게 함수를 전달할 수 있다.
const decreaser = makeCounter(predicates.decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

- 함수가 일급 객체라는 것은 함수를 객체와 동일하게 사용할 수 있다는 의미
  - 객체는 값이므로 함수는 값과 동일하게 취급 가능
  - 함수는 값을 사용할 수 있는 곳(변수 할당문, 객체의 프로퍼티, 배열의 요소, 함수 호출의 인수, 함수 반환문)이라면 어디서든 리터럴로 정의할 수 있으며 런타임(runtime)에 함수 객체로 평가
- 함수 객체는 몇 가지 고유한 프로퍼티를 갖는 것을 제외하면 일반 객체와 동일
  - 따라서 함수 객체는 일반 객체와 같이 함수의 매개 변수에 전달할 수 있으며 함수의 결과값으로 반환 가능
    - 함수형 프로그래밍을 가능케하는 자바스크립트의 장점 중 하나
- 함수형 프로그래밍 : 순수 함수(pure function)와 보조 함수의 조합을 통해 외부 상태를 변경하는 부수 효과(side-effect)를 최소화하여 불변성(immutability)을 지향하는 프로그래밍 패러다임
  - 로직 내 존재하는 조건문, 반복문을 제거하여 복잡성을 해결
  - 변수 사용 억제하거나 생명주기를 최소화하여 상태 변경을 피해 오류 최소화 하는것을 목표로 함
  - 고차 함수(High Order Function, HOF) : 함수형 프로그래밍 패러다임에서 매개 변수를 통해 함수를 전달받거나 반환값으로 함수를 반환하는 함수

- 함수는 객체이지만 일반 객체와 차이가 있음
  - 일반 객체는 호출할 수 없지만 함수 객체는 호출 가능
  - 함수 객체는 일반 객체에 없는 함수 고유의 프로퍼티 소유



## 2. 함수 객체의 프로퍼티

- 함수는 객체이므로 프로퍼티를 가질 수 있음
  - 브라우저 콘솔에서 console.dir 메소드를 사용하여 함수 객체의 내부를 보자

```javascript
function square(number) {
  return number * number;
}

console.dir(square);
```

![](https://poiemaweb.com/assets/fs-images/17-1.png)

- 일반 객체에는 없는 arguments, caller, length, name, prototype 프로퍼티가 함수 객체에는 존재
  - 이 프로퍼티들의 프로퍼티 어트리뷰트를 Object.getOwnPropertyDescriptor 메소드로 확인해보면 다음과 같음

```javascript
function square(number) {
  return number * number;
}

// arguments는 square 함수 객체의 데이터 프로퍼티이다.
Object.getOwnPropertyDescriptor(square, 'arguments');
// {value: null, writable: false, enumerable: false, configurable: false}

// caller는 square 함수 객체의 데이터 프로퍼티이다.
Object.getOwnPropertyDescriptor(square, 'caller');
// {value: null, writable: false, enumerable: false, configurable: false}

// length는 square 함수 객체의 데이터 프로퍼티이다.
Object.getOwnPropertyDescriptor(square, 'length');
// {value: 1, writable: false, enumerable: false, configurable: true}

// name은 square 함수 객체의 데이터 프로퍼티이다.
Object.getOwnPropertyDescriptor(square, 'name');
// {value: "square", writable: false, enumerable: false, configurable: true}

// prototype은 square 함수 객체의 데이터 프로퍼티이다.
Object.getOwnPropertyDescriptor(square, 'prototype');
// {value: {…}, writable: true, enumerable: false, configurable: false}

// __proto__는 square 함수 객체의 프로퍼티가 아니다.
Object.getOwnPropertyDescriptor(square, '__proto__');
// undefined

// __proto__는 Object.prototype 객체의 접근자 프로퍼티이다.
// square 함수 객체는 Object.prototype 객체로부터 __proto__ 접근자 프로퍼티를 상속받는다.
Object.getOwnPropertyDescriptor(Object.prototype, '__proto__');
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}
```

- arguments, caller, length, name, prototype 프로퍼티는 모두 함수 객체의 데이터 프로퍼티
  - 하지만 `__proto__`는 접근자 프로퍼티이며 함수 객체의 프로퍼티가 아닌 Object.prototype 객체 프로퍼티를 상속받은 것



## 2.1 arguments 프로퍼티

- 함수 객체의 arguments 프로퍼티 값은 arguments 객체
- arguments 객체는 함수 호출시 전달된 인수(argument)들의 정보를 담고 있는 순회 가능한(iterable) 유사 배열 객체(array-like object)이며 함수 내부에서 지역 변수처럼 사용
  - 즉, 함수 외부에서는 사용할 수 없음
- 함수 객체의 arguments 프로퍼티는 현재 일부 브라우저에서 지원하고 있지만 ES3부터 표준에서 폐지
  - Function.arguments와 같은 사용 방법은 권장되지 않으며 함수 내부에서 지역 변수처럼 사용할 수 있는 arguments 객체를 참조하도록 함

- 자바스크립트는 함수 호출 시 함수 정의에 따라 인수를 전달하지 않아도 에러가 발생하지 않음

```javascript
function multiply(x, y) {
  console.log(arguments);
  return x * y;
}

console.log(multiply());        // NaN
console.log(multiply(1));       // NaN
console.log(multiply(1, 2));    // 2
console.log(multiply(1, 2, 3)); // 2
```

- 함수를 정의할 때 선언한 매개변수는 함수 몸체 내부에서 변수와 동일 취급
  - 즉, 함수가 호툴되면 함수 몸체 내에서 암묵적으로 매개변수가 선언되고 undefined로 초기화된 이후 인수 할당
- 선언된 매개변수의 개수보다 인수를 적게 전달했을 경우 인수가 전달되지 않은 매개변수는 undefined로 초기화된 상태 유지
- 매개변수 개수보다 초과된 인수는 무시되는 것처럼 보여지나 버려지지는 않고, 암묵적으로 arguments 객체의 프로퍼티로 보관

![](https://poiemaweb.com/assets/fs-images/17-2.png)

- arguments 객체는 인수를 프로퍼티 값으로 소유하며 프로퍼티 키는 인수의 순서를 나타냄
  - 이 객체의 callee 프로퍼티는 호출된 함수, 즉 arguments 객체를 생성한 함수를 가리키고 arguments 객체의 length 프로퍼티는 인수의 개수를 가리킴
- arguments 객체의 Symbol(Symbol.iterator) 프로퍼티
  - 이 프로퍼티는 arguments 객체를 순회 가능한 자료 구조인 이터러블(iterable)로 만들기 위한 프로퍼티
  - Symbol.iterator를 프로퍼티 키로 사용한 메소드를 구현하는 것에 의해 이터러블이 됨

```javascript
function multiply(x, y) {
  // 이터레이터
  const iterator = arguments[Symbol.iterator]();

  // 이터레이터의 next 메소드를 호출하여 이터러블 객체 arguments를 순회
  console.log(iterator.next()); // {value: 1, done: false}
  console.log(iterator.next()); // {value: 2, done: false}
  console.log(iterator.next()); // {value: 3, done: false}
  console.log(iterator.next()); // {value: undefined, done: true}

  return x * y;
}

multiply(1, 2, 3);
```

- 선언된 매개변수의 개수와 함수 호출 시 전달하는 인수의 개수를 확인하지 않는 자바스크립트의 특성때문에 런타임시 호출된 함수 인자 개수를 확인하고 이에따라 함수의 동작을 달리 정의할 필요가 있음
  - 이 때 유용하게 사용되는 것이 arguments객체
  - arguments 객체는 매개변수 개수를 확정할 수 없는 **가변 인자 함수**를 구현할 때 유용하게 사용

```javascript
function sum() {
  let res = 0;

  // arguments 객체는 length 프로퍼티가 있는 유사 배열 객체이므로 for 문으로 순회할 수 있다.
  for (let i = 0; i < arguments.length; i++) {
    res += arguments[i];
  }

  return res;
}

console.log(sum());        // 0
console.log(sum(1, 2));    // 3
console.log(sum(1, 2, 3)); // 6
```

- arguments 객체는 배열의 형태로 인자 정보를 담고 있지만 실제 배열이 아닌 유사배열객체(array-like object)
  - 유사배열객체란 length 프로퍼티를 가진 객체로 for 문으로 순회할 수 있는 객체

> ES6에서 도입된 이터레이션 프로토콜을 준수하면 순회 가능한 자료 구조인 이터러블이 됨 - 이터러블의 개념이 없었던 ES5에서 arguments 객체는 유사 배열 객체로 구분되었지만 이터러블이 도입된 ES6부터 arguments 객체는 유사 배열 객체이면서 이터러블

- 유사 배열 객체는 배열이 아니므로 배열 메소드를 사용할 경우 에러가 발생
  - 따라서 배열 메소드를 사용하려면 Function.prototype.call, Function.prototype.apply를 사용해 간접 호출해야 하는 번거로움 존재

```javascript
function sum() {
  if (!arguments.length) return 0;

  // arguments 객체를 배열로 변환
  const array = Array.prototype.slice.call(arguments);
  return array.reduce(function (pre, cur) {
    return pre + cur;
  });
}

console.log(sum(1, 2));          // 3
console.log(sum(1, 2, 3, 4, 5)); // 15
```

- ES6에서는 Rest 파라미터 도입

```javascript
// ES6 Rest parameter
function sum(...args) {
  return !args.length ? 0 : args.reduce((pre, cur) => pre + cur);
}

console.log(sum(1, 2));          // 3
console.log(sum(1, 2, 3, 4, 5)); // 15
```

- ES6의 Rest 파라미터의 도입으로 모던 자바스크립트에서는 arguments 객체의 중요성이 이전 같지는 않지만 언제나 ES6만을 사용하지는 않을 수 있기 때문에 알아둘 필요가 있음



### 2.2 caller 프로퍼티

- caller프로퍼티는 ECMAScript 스펙에 포함되지 않은 비표준 프로퍼티
  - 표준화될 예정도 없으므로 사용하지말고 참고로만 알아둘 것
- 함수 객체의 caller 프로퍼티는 함수 자신을 호출한 함수를 가리킴

```javascript
function foo(func) {
  return func();
}

function bar() {
  return 'caller : ' + bar.caller;
}

// 브라우저에서의 실행한 결과
console.log(foo(bar)); // caller : function foo(func) {...}
console.log(bar());    // caller : null
```

- bar를 호출한 함수는 없으므로 caller 프로퍼티는 null 가리킴
- 위 결과는 브라우저에서 실행한 것으로 Node.js 환경에서 위 예제를 실행하면 다른 결과가 나옴 - 모듈과 관계(Webpack 모듈 번들러에서 자세히)



### 2.3 length 프로퍼티

- 함수 객체의 length 프로퍼티는 함수 정의시 선언한 매개변수의 개수를 가리킴

```javascript
function foo() {}
console.log(foo.length); // 0

function bar(x) {
  return x;
}
console.log(bar.length); // 1

function baz(x, y) {
  return x * y;
}
console.log(baz.length); // 2
```

- arguments 객체의 length 프로퍼티와 함수 객체의 length 프로퍼티의 값은 다를 수 있으므로 주의
  - arguments 객체의 length 프로퍼티는 인자의 개수를 가리키고, 함수 객체의 length 프로퍼티는 매개변수의 개수



### 2.4 name 프로퍼티

- 함수 객체의 name 프로퍼티는 함수 이름을 나타냄
  - ES6부터 정식 표준
- name 프로퍼티는 ES5와 ES6에서 동작을 달리 하므로 주의해야 함
  - 익명 함수 표현식의 경우, ES5에서 name 프로퍼티는 빈 문자열을 값으로 갖지만 ES6에서는 함수 객체를 가리키는 변수 이름을 값으로 가짐

```javascript
// 기명 함수 표현식
var namedFunc = function foo() {};
console.log(namedFunc.name); // foo

// 익명 함수 표현식
var anonymousFunc = function() {};
// ES5: name 프로퍼티는 빈 문자열을 값으로 갖는다.
// ES6: name 프로퍼티는 함수 객체를 가리키는 변수 이름을 값으로 갖는다.
console.log(anonymousFunc.name); // anonymousFunc

// 함수 선언문(Function declaration)
function bar() {}
console.log(bar.name); // bar
```

- 함수 이름과 함수 객체를 가리키는 변수 이름은 의미가 다름
- 함수를 호출할 때는 함수 이름이 아닌 함수 객체를 가리키는 변수 이름으로 호출



###  2.5. `__proto__` 접근자 프로퍼티

- 모든 객체는 [[Prototype]]이라는 내부 슬롯을 가짐
  - [[Prototype]] 내부 슬롯은 객체 지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킴
  - `__proto__`프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티
    - 내부 슬롯에는 직접 접근할 수 없고 간접적인 접근 방법을 제공하는 경우에 한하여 접근
    - [[Prototype]] 내부 슬롯에도 직접 접근할 수 없으며 `__proto__` 접근자 프로퍼티를 통해 간접적으로 프로토타입 객체에 접근

```javascript
const obj = { a: 1 };

// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype이다.
console.log(obj.__proto__ === Object.prototype); // true

// 객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype의 프로퍼티를 상속받는다.
// hasOwnProperty 메소드는 Object.prototype의 메소드이다.
console.log(obj.hasOwnProperty('a'));         // true
console.log(obj.hasOwnProperty('__proto__')); // false
```



### 2.6 prototype 프로퍼티

- prototype 프로퍼티는 함수 객체만이 소유하는 프로퍼티로 일반 객체에는 없음

```javascript
// 함수 객체는 prototype 프로퍼티를 소유한다.
console.log((function() {}).hasOwnProperty('prototype')); // true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
console.log(({}).hasOwnProperty('prototype')); // false
```

- prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 사용될 때, 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킴



Reference : https://poiemaweb.com/