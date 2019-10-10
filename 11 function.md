# 11. 함수

## 1. 함수란?

- 자바스크립트의 가장 중요한 핵심 개념
- 스코프, 실행 컨텍스트, 클로저, 생성자 함수에 의한 객체 생성, 메소드, this, 프로토타입, 모듈화 등이 모두 함수와 깊은 관련
- 수학의 함수는 "입력(input)"을 받아서 "출력(output)"을 내보내는 일련의 과정 정의
- 프로그래밍 언어의 함수도 수학의 함수와 같은 개념
  - 함수 f(x,y) = x + y 를 자바스크립트 함수로 표현해 보자

```javascript
// f(x, y) = x + y
function add(x, y) {
  return x + y;
}

// f(2, 5) = 7
add(2, 5); // 7
```

- 프로그래밍 언어의 함수는 일련의 과정을 문(statement)들로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것
- 프로그래밍 언어의 함수도 입력을 받아서 출력을 내보냄
  - 입력을 전달받는 변수를 매개변수(parameter)
  - 입력을 인수(argument)
  - 출력을 반환값(return value)
  - 함수는 여러개 존재할 수 있으므로 특정 함수를 구별하기 위한 식별자인 함수 이름

![](https://poiemaweb.com/assets/fs-images/11-2.png)

- 함수는 함수 정의(Function Definition)를 통해 생성
  - 다양한 방법으로 정의할 수 있음
- 함수 선언문을 통한 함수 정의

```javascript
// 함수 정의
function add(x, y) {
  return x + y;
}

// 함수 호출
var result = add(2, 5);

// 함수 add에 인수 2, 5를 전달하면서 호출하면 7을 반환한다.
console.log(result); // 7
```

- 함수 정의만으로 함수가 실행되지 않으므로 함수 **호출(Fuction call/invoke)** : 미리 정의된 일련의 과정을 실행하기 위해 필요한 입력(인수, argument)를 매개변수를 통해 함수에게 전달하면서 함수의 실행 지시



## 2. 함수의 사용 이유

- 함수는 필요할 때 여러번 호출 가능 : 동일한 작업을 반복 수행해야 한다면 같은 코드를 중복해서 여러번 작성하는 것이 아니라 미리 정의된 함수를 재사용 하는 것이 효율적
  - 함수는 몇 번이든 호출할 수 있으므로 **코드의 재사용** 측면에서 매우 유용
- 함수를 사용하지 않고 같은 코드를 중복하여 여러 번 작성하면 수정이 필요할 시, 중복된 횟수만큼 코드를 수정해야 하므로 횟수에 비례하여 코드 수정에 걸리는 시간 증가, 실수 가능성 증가
  - 코드 중복을 억제하고 재사용성을 높이는 함수는 유지보수의 편의성을 높이고 실수를 줄여 코드의 신뢰성을 높이는 효과
- 함수는 변수처럼 이름(식별자)을 붙일 수 있음
  - **코드의 가독성**을 위해 함수 자신의 역할을 잘 설명해야 함(코드 내부를 이해하지 않아도 함수 역할 파악 가능하도록)



## 3. 함수 리터럴

- 객체는 객체 리터럴, 함수도 함수 리터럴로 생성 가능
- 함수 리터럴 : function 키워드, 함수 이름, 매개변수 목록, 함수 몸체로 구성

```javascript
// 변수에 함수 리터럴을 할당
var add = function add(x, y) {
  return x + y;
};
```

- 함수 이름
  - 함수 이름은 식별자이므로 식별자 네이밍 규칙 준수
  - 함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자
  - 함수 이름 생략 가능
    - 함수 이름이 있는 함수 : 기명 함수(named function)
    - 함수 이름이 없는 함수 : 익명 함수(anonymous function)
- 매개변수 목록
  - 0개 이상의 매개변수를 괄호로 감싸고 쉼표로 구분
  - 매개변수에는 인수 할당
  - 매개변수는 함수 몸체 내에서 변수와 동일하게 취급되므로 매개변수도 변수와 마찬가지로 식별자 네이밍 규칙 준수
- 함수 몸체
  - 함수가 호출되었을 때 일괄적으로 실행될 문들을 하나의 실행 단위로 정의한 코드 블록
  - 함수 몸체는 함수 호출에 의해 실행
- 리터럴 표기법(Literal notation) : 값을 생성하는 가장 기본적인 방법
  - **함수 리터럴도 평가되어 값을 생성**하며 이 값은 객체 -> **함수는 객체**
  - 함수 리터럴이 평가되어 생성된 객체를 변수에 할당
- 다른 프로그래밍 언어와 구별되는 자바스크립트의 중요한 특징 : 함수가 객체
- 함수는 객체이지만 일반 객체와는 다름
  - 일반 객체는 호출할 수 없지만 함수는 호출 가능
  - 일반 객체에는 없는 함수 객체의 고유한 프로퍼티를 갖음



## 4. 함수 정의

> 함수를 정의하는 4가지 방법

- 함수 선언문(Function Declaration/Function Statement)

```javascript
function add(x, y) {
  return x + y;
}
```

- 함수 표현식(Function Expresstion)

```javascript
var add = function (x, y) {
  return x + y;
};
```

- Function 생성자 함수(Function Constructor)

```javascript
var add = new Function('x', 'y', 'return x + y');
```

- 화살표 함수(Arrow Function): ES6

```javascript
var add = (x, y) => x + y;
```

- 각각의 함수 정의 방식은 함수를 정의한다는 면에서는 동일하지만 미묘한 차이가 있음
  - 변수는 선언(Declaration)한다고 했지만 함수는 정의(Definition)
  - 함수 선언문이 평가되면 식별자가 암묵적으로 생성되고 함수 객체가 할당되므로 ECMAScript 사양에서도 변수에는 선언, 함수에는 정의라고 표현
  - 5.8 undefined 타입의 선언과 정의

**선언(Declaration)과 정의(Definition)**

- 자바스크립트의 undefined(정의되지 않은)에서 말하는 정의 : 변수에 값을 할당하여 변수의 실체를 명확히 하는 것
- 다른 언어에서 컴파일러에게 식별자의 존재만을 알리는 것 : 선언
  컴파일러에게 변수를 생성하도록 하여 식별자와 메모리 주소가 연결되는 것 : 정의



### 4.1 함수 선언문

- 함수 선언문(Function Declaration) : 함수 리터럴 표기법과 형태가 동일하지만, **함수 선언문은 함수 이름을 생략할 수 없음**

```javascript
// 함수 선언문
function add(x, y) {
  return x + y;
}

// 함수 참조
// console.dir은 console.log와는 달리 함수 객체의 프로퍼티까지 출력한다.
// 단, Node.js 환경에서는 console.log와 같은 결과가 출력된다.
console.dir(add); // ƒ add(x, y)

// 함수 호출
console.log(add(2, 5)); // 7
```

- 함수 리터럴에서 "함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자다"라고 언급 : 함수 이름 add를 함수 몸체 외부에서는 참조할 수 없다는 의미
  - 그렇다면 **함수를 참조하고 호출할 때 사용한 add는 도대체 무엇**인가?
  - 함수 이름은 함수 몸체 외부에서 참조할 수 없는 식별자임에도 불구하고 함수 몸체 외부에서 참조하고 있음
- 함수 선언문을 함수 문(Function Statement)이라고도 부름
  - 말 그대로 함수 선언문은 문
  - 함수 리터럴과 형식은 동일하지만 함수 선언문은 함수를 정의하는 표현식이 아닌 문으로 해석
  - 함수 선언문은 실행되어 함수 객체를 생성 -> 생성된 함수 객체를 할당할 변수가 필요
  - 함수 객체를 변수에 할당하지 않으면 생성된 함수 객체를 사용할 수 없고 아무도 참조하고 있지 않는 함수 객체는 가비지 컬렉터에 의해 메모리에서 해제
- **자바스크립트 엔진은 함수 이름과 동일한 이름의 식별자를 암묵적으로 선언하고 생성된 함수 객체 할당**

- 이를 의사 코드(pseudo code)로 표현하면 아래와 같음

```javascript
var add = function add(x, y) {
  return x + y;
};

console.log(add(2, 5)); // 7
```

![](https://poiemaweb.com/assets/fs-images/11-3.png)

- 함수는 함수 이름으로 호출하는 것이 아니라 함수 객체를 가리키는 변수로 호출
  - 함수 선언문 방식으로 생성된 함수를 호출한 것은 함수 이름 add이 아니라 자바스크립트 엔진이 암묵적으로 생성한 변수 add
  - 함수 이름과 변수 이름이 일치하므로 함수 이름으로 호출되는 듯 보이지만 사실은 변수로 호출된 것
- 위 의사코드가 바로 다음에 살펴볼 함수 표현식
- 자바스크립트 엔진은 함수 선언문을 함수 표현식으로 변환하여 함수 객체를 생성한다고 생각할 수 있음



### 4.2 함수 표현식

- 자바스크립트의 함수는 객체
- 자바스크립트의 객체 : 값처럼 변수에 할당 할 수도 있고 프로퍼티의 값이 될 수도 있으며 배열의 요소가 될 수도 있음
- 이러한 객체를 일급 객체(first-class object) : **자바스크립트의 함수는 일급 객체로 함수를 값처럼 자유롭게 사용할 수 있다는 의미**
- 함수는 일급 객체이므로 리터럴로 생성한 함수 객체를 변수에 할당 가능
  - 이러한 함수 정의 방식을 함수 표현식(Function expression)
- 함수 선언문으로 정의한 함수 add를 함수 표현식으로 바꾸어 정의하면 아래와 같음

```javascript
// 함수 표현식
var add = function (x, y) {
  return x + y;
};

console.log(add(2, 5)); // 7
```

- 함수 이름을 생략한 경우 이는 익명 함수
- 함수 표현식의 함수 리터럴은 함수 이름을 생략하는 것이 일반적
- 함수 선언문에서 살펴본 바와 같이, 함수를 호출할 때는 함수 이름이 아니라 함수 객체를 가리키는 변수를 사용해야 함
  - 함수 이름은 함수 몸체 내부에서만 유효한 식별자이므로 함수 이름으로 함수를 호출할 수 없음

```javascript
// 기명 함수 표현식
var add = function foo (x, y) {
  return x + y;
};

// 함수 객체를 가리키는 변수로 호출
console.log(add(2, 5)); // 7

// 함수 이름으로 호출하면 ReferenceError가 발생한다.
// 함수 이름은 함수 몸체 내부에서만 유효한 식별자이다.
console.log(foo(2, 5)); // ReferenceError: foo is not defined
```

- 자바스크립트 엔진은 함수 선언문의 함수 이름으로 변수를 암묵적 선언하고 생성된 객체를 할당하므로 함수 표현식과 유사하게 동작하는 것처럼 보이지만 정확히 동일하게 동작하지는 않음
  - 함수 선언문 : 표현식이 아닌 문
  - 함수 표현식 : 표현식인 문



### 4.3 함수 생성 시점과 함수 호이스팅

```javascript
// 함수 참조
console.dir(add); // ƒ add(x, y)
console.dir(sub); // undefined

// 함수 호출
console.log(add(2, 5)); // 7
console.log(sub(2, 5)); // TypeError: sub is not a function

// 함수 선언문
function add(x, y) {
  return x + y;
}

// 함수 표현식
var sub = function (x, y) {
  return x - y;
};
```

- 함수 선언문으로 정의한 함수 : 함수 선언문 이전에 호출 가능
- 함수 표현식으로 정의한 함수 : 함수 표현식 이전에 호출할 수 없음
- **함수 선언문으로 정의한 함수와 함수 표현식으로 정의한 함수의 생성 시점이 다름**
- 모든 선언문처럼 함수 선언문은 코드가 순차적으로 실행되기 이전에 자바스크립트 엔진에 의해 먼저 실행
  - 함수 선언문으로 함수 정의시 자바스크립트 엔진에 의해 다른 코드가 실행되기 이전에 함수 이름과 동일한 이름의 변수를 암묵적으로 선언하고 함수 객체를 생성하여 할당
  - 다른 코드가 실행되기 이전에 이미 함수 객체가 생성되고 함수 이름과 동일한 변수에 할당까지 완료된 상태이므로 함수 선언문 이전에 함수를 참조할 수 있으며 호출할 수도 있음
- 함수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징을 함수 호이스팅(Function Hoisting)
- 함수 호이스팅과 변수 호이스팅의 차이
  - 변수 호이스팅은 선언 단계와 초기화 단계가 동시에 진행되며 다른 코드가 실행되기 이전에 자바스크립트 엔진에 의해 암묵적으로 수행
  - 함수 호이스팅은 선언 단계와 초기화 단계, 그리고 할당 단계(암묵적으로 선언된 변수에 함수 객체를 할당)까지 동시에 진행
- 함수 표현식 : 변수 할당문의 값이 함수 리터럴인 문
  - 변수 선언문과 변수 할당문을 한번에 기술한 축약 표현과 동일하게 동작
  - 변수 선언은 런타임 이전에 실행되어 undefined로 초기화되지만, 변수 할당문의 값은 할당문이 실행되는 시점, 즉 런타임에 평가되므로 함수 표현식의 함수 리터럴도 할당문이 실행되는 시점에 평가되어 함수 객체가 됨

![](https://poiemaweb.com/assets/fs-images/11-4.png)

- 따라서 함수 표현식 이전에 함수를 참조하면 undefined 반환
  - 이 때 함수 호출시 undefined를 호출하는 것이므로 타입 에러(Type Error) 발생
  - 함수 표현식으로 정의한 함수는 반드시 함수 표현식 이후에 참조 또는 호출해야 함
- 함수 호이스팅은 함수 호출 전에 반드시 함수를 선언해야 한다는 당연한 규칙을 무시하므로 코드의 구조를 엉성하게 함
  - 더글라스 크락포드가 함수 표현식만을 사용할 것을 권고



### 4.4 Function 생성자 함수

- 자바스크립트가 기본 제공하는 빌트인 함수인 Function 생성자 함수는 매개변수 목록과 함께 함수 몸체를 문자열로 전달 받음
- new 연산자와 함께 호출하며 생성된 함수 객체 반환(new 연산자 없이 호출해도 결과는 동일)

- 생성자 함수(Constructor Function) : 객체를 생성하는 함수로 객체를 생성하는 방식은 객체 리터럴 이외에 다양한 방법이 있음

```javascript
var add = new Function('x', 'y', 'return x + y');

console.log(add(2, 5)); // 7
```

- Function 생성자 함수로 함수를 생성하는 방식은 일반적이지도, 바람직하지도 않음
- Function 생성자 함수로 생성한 함수는 함수 선언문이나 함수 표현식으로 생성한 함수와 다르게 동작
  - Function 생성자 함수 방식으로 생성한 함수는 클로저를 생성하지 않음
  - 클로저는 아직 살펴보지 않은 내용으로 지금은 함수 선언문이나 함수 표현식으로 생성한 함수와 Function 생성자 함수로 생성한 함수가 동일하지 않는다는 것에 주목

```javascript
var add1 = (function () {
  var a = 10;
  return function (x, y) {
    return x + y + a;
  };
}());

console.log(add1(1, 2)); // 13

var add2 = (function () {
  var a = 10;
  return new Function('x', 'y', 'return x + y + a;');
}());

console.log(add2(1, 2)); // ReferenceError: a is not defined
```



### 4.5 화살표 함수

- ES6에서 새롭게 도입된 화살표 함수(Arrow function) : 키워드 대신 화살표(=>, Fat arrow)를 사용하여 보다 간략한 방법으로 함수를 선언할 수 있음
  - 화살표 함수는 항상 익명 함수로 정의

```javascript
// 화살표 함수
const add = (x, y) => x + y;

console.log(add(2, 5)); // 7
```

- 기존의 함수 선언문 또는 함수 표현식을 완전히 대체하기 위해 디자인된 것은 아님
  - 모든 상황에서 화살표 함수 함수를 사용할 수 있는 것은 아님
  - 기존의 함수와 this 바인딩 방식이 다르고, prototype 프로퍼티가 없으며 arguments 객체를 생성하지 않음
  - 화살표 함수에 대해서는 this, 프로토타입, arguments 객체를 알아보고 나서 자세히 알아볼 것



## 5. 함수 호출

- 함수는 함수를 참조하는 변수와 한 쌍의 소괄호인 함수 호출 연산자로 호출
- 함수 호출 연산자 내에는 0개 이상의 인수(argument)를 쉼표로 구분하여 나열
  - 이 인수는 매개변수에 할당할 수 있는 값이어야 함
- 함수를 호출하면 현재 실행 흐름을 중단하고 호출된 함수로 컨트롤을 넘김
  - 이 때 매개변수에 의해 인수가 할당되고 함수 몸체의 문들이 실행되기 시작



## 6. 매개변수와 인수

- 함수의 실행을 위해 함수 외부에서 함수 내부로 값을 전달할 필요가 있는 경우, 매개변수(parameter, 인자)를 통해 인수(argument)를 전달
- 인수는 함수를 호출할 때 지정하며 개수와 타입에 제한이 없음

```javascript
function add(x, y) {
  return x + y;
}

var result = add(2, 5);

console.log(result); // 7
```

- 매개변수는 함수를 정의할 때 선언하며 함수 몸체 내부에서 변수와 동일하게 취급
  - 함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 생성되고 변수와 마찬가지로 undefined로 초기화된 이후 인수 할당
  - 함수가 호출될 때마다 매개변수는 이와 같은 단계를 거침

![](https://poiemaweb.com/assets/fs-images/11-5.png)

- 매개변수는 함수 몸체 내부에서만 참조할 수 있고 함수 몸체 외부에서는 참조할 수 없음
  - 매개변수의 스코프(유효 범위)는 함수 내부

```javascript
function add(x, y) {
  console.log(x, y); // 2 5
  return x + y;
}

add(2, 5);

console.log(x, y); // ReferenceError: x is not defined
```

- 함수를 호출할 때 매개변수의 개수만큼 인수를 전달하는 것이 일반적이지만 그렇지 않은 경우에도 에러는 발생하지 않음
  - 함수는 매개변수의 개수와 인수의 개수를 체크하지 않음
  - 인수가 부족한 경우 매개변수의 값은 undefined

```javascript
function add(x, y) {
  return x + y;
}

console.log(add(2)); // NaN
console.log(add(2, 5, 10)); // 7
```

- 위 예제의 매개변수 x에는 인수 2가 전달되지만 매개변수 y에는 전달할 인수가 없으므로 매개변수 y는 undefined가 초기화된 상태 그대로
  - x + y는 2 + undefined 이므로 NaN 반환
- 인수가 매개변수보다 많은 경우 초과되는 인수는 무시됨
  - arguments 객체 : 사실 초과된 인수가 그냥 버려지는 것은 아니고 모든 인수는 암묵적으로 arguments객체의 프로퍼티로 보관

```javascript
function add(x, y) {
  console.log(arguments);
  // Arguments(3) [2, 5, 10, callee: ƒ, Symbol(Symbol.iterator): ƒ]

  return x + y;
}

add(2, 5, 10);
```



## 7. 인수 확인

```javascript
function add(x, y) {
  return x + y;
}

console.log(add(2));        // NaN
console.log(add('a', 'b')); // 'ab'
```

- 자바스크립트 문법상 문제가 없으므로 자바스크립트 엔진은 에러를 발생시키지 않고 위 코드를 실행시킴(이유)
  1. 자바스크립트 함수는 매개변수와 인수의 개수가 일치하는지 확인하지 않음
  2. 자바스크립트는 동적 타입 언어로 매개변수의 타입을 사전에 지정하지 않음

- 따라서 자바스크립트 함수는 적절한 인수가 전달되었는지 확인 필요

```javascript
function add(x, y) {
  if (typeof x !== 'number' || typeof y !== 'number') {
    throw new TypeError('매개변수에 숫자 타입이 아닌 값이 할당되었습니다.');
  }

  return x + y;
}

console.log(add(2));        // TypeError: 매개변수에 숫자 타입이 아닌 값이 할당되었습니다.
console.log(add('a', 'b')); // TypeError: 매개변수에 숫자 타입이 아닌 값이 할당되었습니다.
```



## 8. 매개변수의 개수

- 매개변수의 최대 개수 제한이 ECMAScript 스펙에 정해져 있지 않으므로 브라우저마다 구현도 제각각
  - 매개변수는 최대 몇개까지 사용하는 것이 좋을까?
- 매개변수는 순서에 의미가 있음
  - 매개변수가 많아지면 함수를 호출할 때 전달해야 할 인수의 순서를 고려해야 함
  - 이는 함수 사용 방법을 어렵게 만들고 실수 발생 가능성을 높임
  - 매개변수의 개수나 순서가 변경되면 함수의 호출 방법도 변경되므로 함수를 사용하는 코드 전체가 영향을 받음
- 함수의 매개변수는 코드 이해에 방해가 되는 요소이므로 이상적인 매개변수 개수는 0개이며 적을 수록 좋음
  - 매개변수의 개수가 많다는 것은 함수가 여러가지 일을 한다는 증거이므로 바람직하지 않음
  - 이상적인 함수는 한가지 일만 해야 하며 가급적 작게 만들어야 함
- 매개변수는 최대 3개 이상을 넘지 않는 것을 권장
  - 이 이상의 매개변수가 필요하다면 하나의 매개변수를 선언하고 객체를 인수로 전달받는 것이 유리
  - 아래는 jQuery의 ajax 메소드에 객체를 인수로 전달하는 예제

```javascript
$.ajax({
  method: 'POST',
  url: '/user',
  data: { id: 1, name: 'Lee' },
  cache: false
});
```

- 객체를 인수로 사용하는 경우, 프로퍼티 키만 정확히 지정하면 매개변수의 순서를 신경쓰지 않아도 됨
- 주의할 것 : 함수 외부에서 함수 내부로 전달한 객체를 함수 내부에서 변경하면 함수 외부의 객체가 변경되는 부작용 발생



## 9. 외부 상태의 변경과 함수형 프로그래밍

- 원시 값은 값에 의한 전달(Pass by value), 객체는 참조에 의한 전달(Pass by reference) 방식으로 동작
- 매개변수도 함수 몸체 내부에서 변수와 동일하게 취급되므로 매개변수 또한 타입에 따라 값에 의한 전달, 참조에 의한 전달 방식을 그대로 따름
- 함수의 매개변수에 값을 전달하는 방식을 값에 의한 호출(Call by value), 참조에 의한 호출(Call by reference)로 구별하여 부르는 경우도 있으나 동작 방식은 값에 의한 전달, 참조에 의한 전달과 동일

```javascript
function changeVal(primitive, obj) {
  primitive += 100;
  obj.name = 'Kim';
}

// 외부 상태
var num = 100;
var person = { name: 'Lee' };

console.log(num); // 100
console.log(person); // {name: "Lee"}

// 원시값은 값 자체가 복사되어 전달되고 객체는 참조값이 복사되어 전달된다.
changeVal(num, person);

// 원시 값은 원본이 훼손되지 않는다.
console.log(num); // 100

// 객체는 원본이 훼손된다.
console.log(person); // {name: "Kim"}
```

- changeVal 함수는 원시 타입 인수와 객체 타입 인수를 전달 받아 함수 몸체에서 매개변수의 값을 변경
  - 원시 타입 인수는 값 자체가 복사되어 매개변수에 전달되기 때문에 함수 몸체에서 그 값을 변경하여도 원본은 훼손되지 않음. 함수 외부에서 함수 내부로 전달한 원시값의 원본을 변경하는 어떠한 부수효과도 발생하지 않음
  - 객체 타입 인수는 참조값이 복사되어 매개변수에 전달되기 때문에 함수 몸체에서 참조값으로 참조한 객체를 변경할 경우 원본이 훼손. 함수 외부에서 함수 몸체 내부로 전달한 참조값에 의해 원본 객체가 변경되는 부수효과 발생

![](https://poiemaweb.com/assets/fs-images/11-6.png)

- 이 때 외부의 상태(참조 타입의 변수 person)를 변경시키는 것은 코드의 복잡성을 증가시키고 가독성을 해치는 원인
  - 논리가 간단해야 버그가 줄어듦
- 이런 현상은 객체가 변경할 수 있는 값이며 참조에 의한 전달방식으로 동작하기 때문에 발생하는 부작용
  - 참조에 의한 전달 방식을 통해 참조값이 공유되어 있는 객체는 언제든 변경될 수 있음
  - 복잡한 코드에서 의도치 않은 객체의 변경을 추적하는 것은 어려운 일
  - 객체의 변경을 추적하려면 Observer 패턴으로 참조를 통해 객체를 공유하는 모든 이들에게 변경 사실을 통지하고 이에 대처하는 추가 대응 필요
- 문제 해결 방법 중 하나는 객체를 불변 객체(immutable object)로 만들어 사용하는 것
  - 비용은 들지만 객체를 마치 원시 값처럼 변경 불가능한 값으로 동작하게 만드는 것
  - 객체 상태 변경을 원천봉쇄하고 객체 상태 변경이 필요한 경우 참조가 아닌 객체의 방어적 복사(defensive copy)를 통해 원본 객체를 완전히 복제, 즉 깊은 복사(Deep copy)를 통해 새로운 객체를 생성한 후 변경
  - 이를 통해 외부 상태가 변경되는 부수효과를 없앨 수 있음
- 함수형 프로그래밍에서는 어떤 외부 상태도 변경시키지 않는(부수 효과가 없는) 함수를 순수 함수(Pure function), 외부 상태를 변경시키는(부수 효과가 있는) 함수를 비순수 함수(Impure function)이라 부름
  - 위 예제의 changeVal 함수와 같은 비순수 함수는 코드의 복잡성 증가
  - 비순수 함수를 최대한 줄이는 것은 부수 효과를 최대한 억제하는 것과 같음
- 함수형 프로그래밍 : 변수의 사용을 억제하여 상태 변경을 피하고 순수 함수와 보조 합수의 조합을 통해 로직 내에 존재하는 조건문과 반복문을 제거하여 복잡성을 해결하려는 프로그래밍 패러다임
  - 변수 값은 누군가에 의해 언제든지 변경될 수 있고, 조건문이나 반복문은 로직의 흐름을 이해하기 어렵게 만들어 가독성을 해치고 오류 발생의 근본적 원인이 될 수 있기 때문
  - 순수 함수를 통해 부수 효과(Side effect)를 최대한 억제하여 오류를 피하고 프로그램의 안정성을 높이려는 노력의 한 방법



## 10. 반환문

- 함수는 return 키워드와 반환값으로 이루어진 반환문을 사용하여 실행 결과를 반환(return)할 수 있음

```javascript
function multiply(x, y) {
  return x * y; // 값의 반환
}

// 함수는 반환값으로 평가된다.
var result = multiply(3, 5);

console.log(result); // 15
```

- 함수는 return 키워드를 통해 자바스크립트에서 사용 가능한 모든 값을 반환할 수 있음
- 함수 호출은 표현식
  - 함수 호출 표현식은 return 키워드가 반환한 값, 즉 반환값으로 평가
- 반환문의 두 가지 역할
  - 첫 번째 : 반환문은 함수의 실행을 중단하고 함수 몸체를 빠져나가므로 반환문 이후에 다른 문이 존재하면 그 문은 실행되지 않고 무시됨
  - 두 번째 : 반환문은 return 키워드 뒤에 지정한 값을 반환하는데, 반환값을 명시적으로 지정하지 않으면 undefined가 반환

- 함수는 반환문 생략 가능 : 함수 몸체의 마지막 문까지 실행한 후 암묵적으로 undefined 반환

- return 키워드와 반환문 사이에 줄바꿈이 있으면 세미콜론 자동 삽입 기능(ASI)에 의해 세미콜론이 추가되어 뒤쪽 문은 무시됨

```javascript
function multiply(x, y) {
  // return 키워드와 반환값 사이에 줄바꿈이 있으면
  return // 세미콜론 자동 삽입 기능(ASI)에 의해 세미콜론이 추가된다.
  x * y; // 무시된다.
}

console.log(multiply(3, 5)); // undefined
```



## 11. 다양한 함수의 형태

### 11.1 즉시실행함수

- 즉시 실행 함수(IIFE, Immediately Invoke Function Expression) : 함수의 정의와 동시에 즉시 호출되는 함수
  - 한 번만 호출되며 다시 호출할 수 없음
  - 함수 이름이 없는 익명 즉시 실행 함수를 사용하는 것이 일반적
  - 기명 즉시 실행 함수도 사용 가능하지만 함수 이름은 함수 몸체에서만 참조할 수 있는 식별자이므로 실행 함수를 다시 호출할 수 없음

```javascript
// 익명 즉시 실행 함수
(function () {
  var a = 3;
  var b = 5;
  return a * b;
}());

// 기명 즉시 실행 함수
(function foo() {
  var a = 3;
  var b = 5;
  return a * b;
}());

foo(); // ReferenceError: foo is not defined
```

- 즉시 실행 함수는 반드시 그룹 연산자 (...)로 감싸 주어야 함

```javascript
function () { // ① SyntaxError: Unexpected token (
  // ...
}();

function foo() {
  // ...
}(); // ② SyntaxError: Unexpected token )
```

- 두 예제 모두 문법 에러가 발생하지만 에러가 발생하는 지점이 다름
  - 1에러 : 첫 번째 라인에서 에러 발생. 함수 선언문의 형식이 맞지 않기 때문 - 함수 선언문은 함수 이름을 생략할 수 없음
  - 2에러 : 마지막 라인에서 에러 발생. 자바스크립트 엔진이 함수 선언문이 끝나는 위치, 즉 함수 코드 블록의 닫는 중괄호 뒤에 암묵적으로 ;를 추가하기 때문

```javascript
function foo() {}(); // => function foo() {};();

(); // SyntaxError: Unexpected token )

console.log(typeof (function f(){})); // function
console.log(typeof (function (){}));  // function
```

- 함수 선언문 뒤의 그룹 연산자 때문에 에러 발생

- 함수 선언문이나 함수 표현식을 그룹 연산자로 감싸면 함수가 평가되어 함수 객체가 됨
- 따라서 그룹 연산자로 먼저 함수를 평가하여 함수 객체를 생성한 다음 함수를 호출
  - 그룹 연산자 뿐 아니라 함수를 평가하여 함수 객체를 생성할 수 있는 방법은 다양
  - 가장 일반적인 방법은 하단의 첫 번째 방식

```javascript
(function () {
  // ...
}());

(function () {
  // ...
})();

!function () {
  // ...
}();

+function () {
  // ...
}();
```

- 즉시 실행 함수는 일반 함수처럼 값을 반환할 수 있고 인수를 전달할 수도 있음

```javascript
var res = (function () {
  var a = 3;
  var b = 5;
  return a * b;
}());

console.log(res); // 15

res = (function (a, b) {
  return a * b;
}(3, 5));

console.log(res); // 15
```

- 즉시 실행 함수 내에 코드를 모아 두면 혹시 있을 수도 있는 변수나 함수 이름이 충돌하는 것을 막을 수 있음
  - 이를 목적으로 즉시 실행 함수를 사용하기도 함(12단원에서 배울 것)



### 11.2 재귀 함수

- 재귀 호출(recursive call) : 함수가 자기 자신을 호출하는 것
- 재귀 함수(recursive function) : 자기 자신을 호출하는 행위, 즉 재귀 호출을 수행하는 함수
- 재귀 호출은 반복 연산을 간단하게 구현할 수 있는데, 예를 들어 팩토리얼은 재귀 호출로 간단히 구현 가능

```javascript
// 팩토리얼(계승)은 1부터 자신까지의 모든 양의 정수의 곱이다.
// n! = 1 * 2 * ... * (n-1) * n
function factorial(n) {
  // 탈출 조건: n이 1 이하일 때 재귀 호출을 멈춘다.
  if (n <= 1) return 1;
  return factorial(n - 1) * n;
}

console.log(factorial(0)); // 0! = 1
console.log(factorial(1)); // 1! = 1
console.log(factorial(2)); // 2! = 1 * 2 = 2
console.log(factorial(3)); // 3! = 1 * 2 * 3 = 6
console.log(factorial(4)); // 4! = 1 * 2 * 3 * 4 = 24
console.log(factorial(5)); // 5! = 1 * 2 * 3 * 4 * 5 = 120
```

- 재귀 함수는 자신을 무한히 연쇄 호출 하므로 반드시 호출을 멈출 수 있는 탈출 조건을 만들어야 함
  - 위 함수는 인수가 1 이하일 때 재귀 호출 중지
  - 탈출 조건이 없는 경우, 함수가 무한 호출되어 stack overflow 에러 발생
- 재귀 함수 장단점
  - 장점 : 반복 연산 간단 구현
  - 단점 : 무한 반복에 빠질 수 있고, 이로 인해 stack overflow 에러 발생 가능
- 대부분의 재귀 함수는 for문이나 while문으로 구현 가능

```javascript
// 위쪽의 팩토리얼 예제 반복문으로 구현
function factorial(n) {
  if (n <= 1) return 1;

  var res = n;
  while (--n) res *= n;
  return res;
}

console.log(factorial(0)); // 0! = 1
console.log(factorial(1)); // 1! = 1
console.log(factorial(2)); // 2! = 1 * 2 = 2
console.log(factorial(3)); // 3! = 1 * 2 * 3 = 6
console.log(factorial(4)); // 4! = 1 * 2 * 3 * 4 = 24
console.log(factorial(5)); // 5! = 1 * 2 * 3 * 4 * 5 = 120
```

- 재귀 함수는 반복문을 사용하는 것보다 재귀 함수를 사용하는 것이 보다 직관적으로 이해하기 쉬울 때에만 한정적으로 사용하는 것이 바람직



### 11.3 중첩 함수

- 중첩 함수(nested function) : 함수 내부에 정의된 함수
  - 내부 함수(Inner function)이라고도 함
- 일반적으로 중첩 함수는 자신을 포함하는 외부 함수(outer function)를 돕는 헬퍼 함수(helper function)의 역할
- 아래 예제의 중첩 함수 inner는 자신을 포함하고 있는 외부 함수 outer의 변수에 접근 가능
  - 그러나 외부 함수는 중첩 함수의 변수에 접근 불가능

```javascript
function outer() {
  var x = 1;

  // 중첩 함수
  function inner() {
    var y = 2;
    // 외부 함수의 변수를 참조할 수 있다.
    console.log(x + y); // 3
  }

  inner();
  // 중첩 함수의 변수를 참조할 수 없다.
  console.log(x + y); // ReferenceError: y is not defined
}

outer();
```

- 이와 같은 개념을 스코프라 함(다음장에서)



### 11.4 콜백 함수

- 자바스크립트의 함수는 일급 객체이므로 함수의 매개 변수에게 함수를 전달할 수 있음

```javascript
// 콜백 함수를 전달받는 함수
function print(f) {
  var string = 'Hello';
  // 콜백 함수를 전달받는 함수가 콜백 함수의 호출 시기를 결정하고 호출
  return f(string);
}

// print 함수에 콜백 함수를 전달하면서 호출
var res1 = print(function (str) {
  return str.toUpperCase();
});

// print 함수에 콜백 함수를 전달하면서 호출
var res2 = print(function (str) {
  return str.toLowerCase();
});

console.log(res1, res2); // HELLO hello
```

- print 함수는 함수를 인수로 전달 받음
  -> print 함수에 인수로 전달된 함수는 print 함수가 호출할 시기를 결정하여 호출
  - 이 때 print 함수에 인수로 전달된 함수 : 콜백 함수(Callback function)
- 콜백 함수는 콜백 함수를 인수로 전달 받은 함수가 호출 시점을 결정하여 호출
- 콜백 함수가 콜백 함수를 전달받는 함수 내부에만 호출된다면 콜백 함수를 익명 함수 리터럴로 정의하면서 인수로 곧바로 전달하는 것이 일반적
  - 이 때 콜백 함수로서 전달된 함수 리터럴은 콜백 함수를 전달받은 함수가 호출될 때 평가되어 생성됨

```javascript
// 익명 함수 리터럴을 전달. print 함수를 호출할 때 마다 콜백 함수가 생성된다.
var res = print(function (str) {
  return str.toUpperCase(); // HELLO
});

console.log(res); // HELLO
```

- 단, 콜백 함수를 다른 곳에서도 호출할 필요가 있거나, 콜백 함수를 전달받는 함수가 자주 호출된다면 함수 외부에서 콜백 함수를 정의한 후 콜백 함수를 전달하는 편이 효율적

```javascript
// toUpperCase 함수는 단 한번만 생성된다.
var toUpperCase = function (str) {
  return str.toUpperCase();
};

// 콜백 함수를 전달
var res = print(toUpperCase);
console.log(res); // HELLO
```

- 이 예제의 toUpperCase 함수는 단 한 번만 생성
  - 하지만 콜백 함수를 익명 함수 리터럴로 정의하면서 인수로 곧바로 전달하면 콕백 함수를 전달받는 함수가 호출될 때 마다 콜백 함수가 생성
- 중첩 함수가 외부 함수를 돕는 헬퍼 함수의 역할을 하는 것처럼 콜백 함수는 함수에 전달되어 헬퍼 함수의 역할을 함
  - 단, 중첩 함수는 고정되어 있어서 교체할 수 없지만 콜백 함수는 외부에서 인수로 주입하기 때문에 자유롭게 교체할 수 있는 장점 존재

```javascript
// 콜백 함수를 사용하지 않으면 함수를 분리해야 한다.
function printToUpperCase() {
  var string = 'Hello';
  return string.toUpperCase();
}

console.log(printToUpperCase()); // HELLO

function printToLowerCase() {
  var string = 'Hello';
  return string.toLowerCase();
}

console.log(printToLowerCase()); // hello

// 콜백 함수를 외부에서 전달하면 콜백 함수에 따라 다양한 동작을 하는 함수를 만들 수 있다.
function print(f) {
  var string = 'Hello';
  return f(string);
}

console.log(print(function (str) {
  return str.toUpperCase();
})); // HELLO

console.log(print(function (str) {
  return str.toLowerCase();
})); // hello
```

- 콜백 함수는 비동기 처리를 위해 사용하는 일반적인 패턴으로 주로 이벤트 처리나 Ajax 통신에 사용

```javascript
// 콜백 함수를 사용한 이벤트 처리
// myButton 버튼을 클릭하면 콜백 함수를 실행한다.
document.getElementById('myButton').addEventListener('click', function () {
  console.log('button clicked!');
});

// 콜백 함수를 사용한 비동기 처리
// 1초 후에 메시지를 출력한다.
setTimeout(function () {
  console.log('1초 경과');
}, 1000);
```

- 콜백 함수는 고차 함수(Higher-order Function)에서도 사용하는 패턴으로 사용 빈도가 매우 높고 중요한 패턴

```javascript
// 콜백 함수를 사용하는 고차 함수 map
var res = [1, 2, 3].map(function (item) {
  return item * 2;
});

console.log(res); // [ 2, 4, 6 ]

// 콜백 함수를 사용하는 고차 함수 filter
res = [1, 2, 3].filter(function (item) {
  return item % 2;
});

console.log(res); // [ 1, 3 ]
```



모든 출처 : https://poiemaweb.com/