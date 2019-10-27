# 29. 표준 빌트인 객체와 래퍼 객체

## 1. 객체의 분류

- 자바스크립트 객체는 아래와 같이 크게 3개의 객체로 분류

| 구분                                       | 설명                                                         |
| :----------------------------------------- | :----------------------------------------------------------- |
| 표준 빌트인 객체(standard built-in object) | Object, Sting, Number, Array, Function과 같이 ECMAScript 사양에 정의된 객체를 말하며 애플리케이션 전역의 공통 기능을 제공한다. |
| 호스트 객체(host object)                   | 브라우저 환경에서 제공하는 window, XmlHttpRequest, HTMLElement 등의 DOM 노드 객체와 같이 호스트 환경에 정의된 객체를 말한다. 예를 들어 브라우저에서 동작하는 환경과 브라우저 외부에서 동작하는 환경의 자바스크립트(Node.js)는 다른 호스트 객체를 사용할 수 있다. |
| 사용자 정의 객체(user-defined object)      | 표준 빌트인 객체와 호스트 객체처럼 외부에서 제공되는 객체가 아닌 사용자가 직접 정의한 객체를 말한다. |

## 2. 표준 빌트인 객체

- 표준 빌트인 객체(standard built-in object) : ECMAScript 사양에 정의된 객체를 말하며 애플리케이션 전역의 공통 기능을 제공
  - 표준 빌트인 객체는 40여개가 넘는데, 이중에서 중요한 표준 빌트인 객체는 Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map/Set, WeakMap/WeakSet, Function, Generator, Promise, Reflect, Proxy, JSON, Error 등이 있음

- 표준 빌트인 객체의 특징
  - 표준 빌트인 객체는 ECMAScript에 정의된 객체이므로 자바스크립트 실행 환경(브라우저 또는 Node.js 환경)과 관계없이 언제나 사용할 수 있음
  - 표준 빌트인 객체는 전역 객체의 프로퍼티로 언제나 참조가 가능하다

## 3. 표준 빌트인 객체는 생성자 함수이다

- Math를 제외한 표준 빌트인 객체는 모두 생성자 함수 객체
  - 예를 들어 표준 빌트인 객체인 String은 생성자 함수로 호출할 수 있으며 String 객체를 반환

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

- 이처럼 Math를 제외한 표준 빌트인 객체는 생성자 함수이므로 인스턴스를 생성할 수 있음
  - 표준 빌트인 객체가 생성자 함수로서 호출되어 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체
  - 예를 들어 표준 빌트인 객체인 String을 생성자 함수로서 호출하여 생성한 String 인스턴스의 프로토타입은 String.prototype

```javascript
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee');
console.log(typeof strObj); // object
console.log(strObj);        // String {"Lee"}

console.log(Object.getPrototypeOf(strObj) === String.prototype); // true
```

- 표준 빌트인 객체가 제공하는 다양한 기능의 메소드는 프로토타입 객체에 프로토타입 메소드로 존재
- 또는 표준 빌트인 객체의 메소드로 존재하여 인스턴스 없이 정적으로 호출할 수도 있음
  - 예를 들어 표준 빌트인 객체인 Number를 생성자 함수로 호출하여 생성한 Number 인스턴스는 Number.prototype이 제공하는 다양한 기능의 프로토타입 메소드를 사용할 수 있음

```javascript
// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(1.5);
console.log(typeof numObj); // object
console.log(numObj);        // Number {1.5}

// toFixed는 프로토타입 메소드이다.
// 소숫점자리를 반올림하여 문자열로 반환한다.
console.log(numObj.toFixed()); // 2
```

- 표준 빌트인 객체인 Number는 인스턴스 없이 정적으로 호출할 수 있는 정적 메소드도 제공

```javascript
// isInteger는 정적 메소드이다.
// 정수(Integer)인지 검사하여 그 결과를 Boolean으로 반환한다.
console.log(Number.isInteger(0.5)); // false
```

- 그렇다면 문자열이나 숫자, 불리언 등의 원시값이 있음에도 불구하고 문자열, 숫자, 불리언 객체를 생성하는 String, Number, Boolean 등의 표준 빌트인 생성자 함수가 존재하는 이유는 무엇일까?

## 4. 원시값과 래퍼 객체

> 래퍼 객체(wrapper object) : 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체

- 원시값은 객체가 아니므로 프로퍼티나 메소드를 가질 수 없음에도 불구하고 원시값인 문자열이 마치 객체처럼 동작

```javascript
const str = 'hello';

// 원시 타입인 문자열이 프로퍼티와 메소드를 갖고 있다.
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
```

- 표준 빌트인 객체가 제공하는 프로토타입 메소드를 사용하려면 반드시 인스턴스를 생성하고 인스턴스로 프로토타입 메소드를 호출해야 함
  - 그런데 위 예제를 살펴보면 원시값으로 표준 빌트인 객체의 프로토타입 메소드를 호출하면 정상적으로 동작

- 이는 원시값인 문자열, 숫자, 불리언 값의 경우, 마치 객체처럼 이들 원시값에 대해 마침표 표기법(또는 대괄호 표기법)으로 접근하면 자바스크립트 엔진은 일시적으로 원시값을 연관된 객체로 변환
  - 이때 생성된 객체로 프로퍼티에 접근하거나 메소드를 호출하고 다시 원시값으로 되돌림
  - 이처럼 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체 -> 레퍼 객체(wrapper object)

- 예를 들어, 문자열에 대해 마침표 표기법으로 접근하면 그 순간 레퍼 객체인 String 생성자 함수의 인스턴스가 생성되고 문자열은 레퍼 객체의 [[StringData]] 내부 슬롯에 할당

```javascript
const str = 'hi';

// 원시 타입인 문자열이 레퍼 객체인 String 인스턴스로 변환된다.
console.log(str.length); // 2
console.log(str.toUpperCase()); // HI

// 레퍼 객체로 프로퍼티 접근이나 메소드 호출한 후, 다시 원시값으로 되돌린다.
console.log(typeof str); // string
```

- 이때 문자열 레퍼 객체인 String 생성자 함수의 인스턴스는 String.prototype의 메소드를 상속받아 사용할 수 있음

> 문자열 래퍼 객체의 프로토타입 체인

![](https://poiemaweb.com/assets/fs-images/29-1.png)

- 그 후, 레퍼 객체의 처리를 종료하면 레퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 되돌리고 레퍼 객체는 가비지 컬렉션의 대상

```javascript
const str = 'hello';

// 래퍼 객체에 프로퍼티 추가
str.name = 'Lee';

// 이 시점에 str은 위 코드의 래퍼 객체가 아닌 새로운 래퍼 객체를 가리킨다.
console.log(str.name); // undefined
```

- 숫자도 마찬가지로 숫자에 대해 마침표 표기법으로 접근하면 그 순간 레퍼 객체인 Number 생성자 함수의 인스턴스가 생성되고 숫자는 레퍼 객체의 [[NumberData]] 내부 슬롯에 할당
  - 이때 레퍼 객체인 Number 객체는 당연히 Number.prototype의 메소드를 상속받아 사용할 수 있음
  - 그 후, 레퍼 객체의 처리가 종료하면 레퍼 객체의 [[NumberData]] 내부 슬롯에 할당된 원시값을 되돌리고 레퍼 객체는 가비지 컬렉션의 대상

```javascript
const num = 1.5;

// 원시 타입인 숫자가 레퍼 객체인 String 객체로 변환된다.
console.log(num.toFixed()); // 2

// 레퍼 객체로 프로퍼티 접근이나 메소드 호출한 후, 다시 원시값으로 되돌린다.
console.log(typeof num); // number
```

- 불리언 값도 문자열이나 숫자와 마찬가지나 다만 불리언 값으로 메소드를 호출할 일은 없음
  - 그리고 Boolean.prototype에는 toString과 valueOf 메소드만 존재하므로 그다지 유용하지도 않음
- valueOf 메소드와 toString 메소드
  - valueOf 메소드: 래퍼 객체의 내부 슬롯에 할당되어 있는 원시값을 반환
  - toString 메소드: 대상 객체를 문자열로 표현하여 반환

- 문자열, 숫자, 불리언 값 이외의 원시값은 레퍼 객체를 생성하지 않음
  - (ES6에서 새롭게 도입된 원시값인 심볼은 일반적인 원시값과는 달리 리터럴 표기법으로 생성할 수 없고 Symbol 함수를 통해 생성해야 하므로 이 논의에서는 제외하도록 함) 
  - 즉, 원시값 null과 undefined 값의 래퍼 객체가 없으므로 null과 undefined 값을 객체처럼 사용하면 에러가 발생

- 이처럼 문자열, 숫자, 불리언 값은 암묵적으로 생성되는 레퍼 객체에 의해 마치 객체처럼 사용할 수 있으며 표준 빌트인 객체인 String, Number, Boolean의 프로토타입 메소드 또는 프로퍼티를 참조할 수 있음
  - 따라서 String, Number, Boolean 생성자 함수를 new 연산자와 함께 호출하여 문자열, 숫자, 불리언 인스턴스를 생성할 필요가 없으며 권장하지도 않음



Reference : https://poiemaweb.com