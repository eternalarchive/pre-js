# 12. 스코프

## 1. 스코프란?

- 스코프(Scope, 유효 범위) : 자바스크립트를 포함한 모든 언어의 기본적이며 중요한 개념
- 자바스크립트의 스코프는 다른 언어의 스코프와 다르게 동작하므로 주의가 필요
  - var 키워드로 선언한 변수와 let 또는 const 키워드로 선언한 변수의 스코프도 다르게 동작
  - 함수의 매개 변수는 함수 몸체 내부에서만 참조할 수 있고, 함수 몸체 외부에서는 참조할 수 없음
  - 매개변수를 참조할 수 있는 유효한 범위, 즉 매개변수의 스코프가 함수 몸체 내부로 한정

```javascript
function add(x, y) {
  // 매개변수는 함수 몸체 내부에서만 참조할 수 있다.
  // 즉, 매개변수의 스코프(유효범위)는 함수 몸체 내부이다.
  console.log(x, y); // 2 5
  return x + y;
}

add(2, 5);

// 매개변수는 함수 몸체 내부에서만 참조할 수 있다.
console.log(x, y); // ReferenceError: x is not defined
```

- 프로그래밍 언어는 변수를 선언하고 참조, 할당하는 기본적인 기능을 제공하며 이것으로 프로그램의 상태(state)를 관리

```javascript
var num = 0; // 현재 카운트를 나타내는 상태: increase 함수에 의해 변화한다.

function increase() {
  return ++num; // 외부 상태를 변경한다.
}

console.log(increase()); // 1
console.log(increase()); // 2
```

- 비순수 함수(impure function) : Increase 함수처럼 함수 내부의 상태(함수 내부에서 선언한 변수 또는 원시 타입의 매개 변수)만이 아니라 함수 외부 상태(num)까지 변경하는 함수
  - 함수가 외부 상태를 변경하면 상태 변화를 추적하기 어려워지므로 외부 상태의 변경을 지양하는 순수 함수(pure function)를 사용하는 것이 좋음
- 변수는 코드의 가장 바깥 영역뿐 아니라 코드 블록이나 함수 내에서도 선언 가능
  - 코드 블록이나 함수는 중첩될 수 있음

```javascript
var var1 = 1; // 코드의 가장 바깥 영역에서 선언된 변수

if (true) {
  var var2 = 2; // 코드 블록 내에서 선언된 변수
  if (true) {
    var var3 = 3; // 중첩된 코드 블록 내에서 선언된 변수
  }
}

function foo() {
  var var4 = 4; // 함수 내에서 선언된 변수

  function bar() {
    var var5 = 5; // 중첩된 함수 내에서 선언된 변수
  }
}

console.log(var1); // 1
console.log(var2); // 2
console.log(var3); // 3
console.log(var4); // ReferenceError: var4 is not defined
console.log(var5); // ReferenceError: var5 is not defined
```

- 변수는 자신이 선언된 위치에 따라 자신이 유효한 범위, 즉 다른 코드 변수가 변수 자신을 참조할 수 있는 범위 결정
  - **모든 식별자(변수 이름, 함수 이름, 클래스 이름 등)는 자신이 선언된 위치에 의해 다른 코드가 식별자 자신을 참조할 수 있는 유효 범위 결정**
  - 이를 스코프(Scope, 유효범위)라 함
- **스코프 : 식별자가 유효한 범위**

```javascript
var x = 'global';

function foo() {
  var x = 'local';
  console.log(x); // ① local
}

foo();

console.log(x); // ② global
```

- 코드의 가장 바깥 영역과 함수 foo 내부에 같은 이름을 갖는 변수 x를 선언하였고 1과 2에서 변수 x 참조
  - 자바스크립트 엔진은 이름이 같은 두 변수 중 어떤 변수를 참조해야 할 것인지 결정해야 함
  - 자바스크립트 엔진은 스코프를 통해 어떤 변수를 참조해야 할지 결정
  - **스코프**란 자바스크립트 엔진이 **식별자를 검색할 때 사용하는 규칙**

- 자바스크립트 엔진은 코드를 실행할 때 코드의 문맥(Context)을 고려
  - 코드가 어디서 실행되며 주변에 어떤 코드들이 있는지에 따라 위 예제의 1과 2처럼 동일한 코드도 다른 결과를 만들어 냄
- 코드의 문맥(Context)과 환경(Environment)
  - 환경 : 코드가 어디서 실행되며 주변에 어떤 코드들이 있는지
  - 코드의 문맥은 환경으로 이루어짐
  - 이를 구현한것이 실행 컨텍스트(Execution context)
  - 모든 코드는 실행 컨텍스트에서 평가되고 실행되므로 스코프와 깊은 관련
- 위 예제의 두 변수 x는 식별자 이름이 동일하지만 자신이 유효한 범위, 즉 스코프가 다른 별개의 변수
  - 스코프는 네임 스페이스
- 스코프라는 개념이 없다면? 같은 이름을 갖는 변수를 충돌을 일으키므로 프로그램 전체에서 하나밖에 사용할 수 없음
- 식별자는 어떤 값을 구별할 수 있어야 하므로 유일(unique)해야 함
  - 식별자인 변수 이름은 중복될 수 없음
  - 하나의 값은 유일한 식별자에 연결(Name binding) 되어야 함
  - 예를들어 우리 컴퓨터 파일명은 하나의 파일을 구별할 수 있는 식별자인데 식별자인 파일명을 중복해서 사용하는 것이 가능한 이유는 폴더(디렉토리)라는 개념이 있기 때문 -> 폴더가 없다면 파일명은 유일해야 함
- 프로그래밍 언어에서는 스코프(유효 범위)를 통해 식별자인 변수 이름의 충돌을 방지하여 같은 이름의 변수를 사용할 수 있도록 함
  - 스코프 내에서 식별자는 유일해야 하지만 다른 스코프에는 같은 이름의 식별자를 사용할 수 있음
- var 키워드로 선언한 변수의 중복 선언
  - var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언이 허용되는데, 이는 의도치 않게 변수값이 변경되는 부작용을 발생시킴

```javascript
function foo() {
  var x = 1;
  // var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
  // 아래 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
  var x = 2;
  console.log(x); // 2
}
foo();
```

- 하지만 let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않음

```javascript
function bar() {
  let x = 1;
  // let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
  let x = 2; // SyntaxError: Identifier 'x' has already been declared
}
bar();
```



## 2. 스코프의 종류

- 코드는 전역(global)과 지역(local)로 구분

| 구분 | 설명                  | 스코프      | 변수      |
| :--: | :-------------------- | :---------- | :-------- |
| 전역 | 코드의 가장 바깥 영역 | 전역 스코프 | 전역 변수 |
| 지역 | 함수 몸체 내부        | 지역 스코프 | 지역 변수 |

- 변수는 자신이 선언된 위치에 의해 스코프가 결정
  - 전역에서 선언된 변수는 전역 스코프를 갖는 전역 변수
  - 지역에서 선언된 변수는 지역 스코프를 갖는 지역 변수

### 2.1 전역과 전역 스코프

![](https://poiemaweb.com/assets/fs-images/12-2.png)

- 전역 스코프(global scope) : 전역이란 코드의 가장 바깥 영역을 말하는데, 전역에 변수를 선언하면 전역 스코프를 갖는 전역 스코프가 됨
  - 어디서든 참조 가능
  - 위 예제의 x, y는 전역 변수 - 어디서든 참조할 수 있으므로 함수 내부에서도 참조 가능

### 2.2 지역과 지역 스코프

- 지역이란? 함수 몸체 내부
- 지역은 지역 스코프(local scope)를 만듦
- 지역에 변수를 선언하면 지역 스코프를 갖는 지역 변수(local variable)가 됨
  - 지역 변수는 자신이 선언된 지역과 하위 지역(중첩 함수)에서만 참조할 수 있음
  - 지역 변수는 자신의 지역 스코프와 하위 지역 스코프에서 유효
- 위 예제에서 함수 outer 내부에서 선언된 변수 z는 지역 변수
  - 지역 변수 z는 자신의 지역 스코프 함수 outer 내부와 하위 지역 스코프인 inner 함수 내부에서 참조할 수 있음
  - 지역 변수 z를 전역에서 참조하면 참조 에러 발생
- 함수 inner 내부에서 선언된 변수 x도 지역 변수
  - 지역 변수 x는 자신의 지역 스코프인 함수 inner 내부에서만 참조할 수 있음
  - 지역 변수 x를 전역 또는 inner 내부 이외의 지역에서 참조하면 참조 에러 발생
- 전역 변수 x가 존재하는데, 함수 inner 내부에서 같은 이름의 변수 x를 참조하면 전역 변수 x를 참조하는 것이 아니라 함수 inner 내부에서 선언된 변수 x를 참조
  - 자바스크립트 엔진이 스코프 체인을 통해 참조할 변수를 검색했기 때문



## 3. 스코프 체인

- 함수는 전역에서 정의할 수도 있고 함수 몸체 내부에서 정의할 수도 있음
  - 함수 몸체 내부에서 함수가 정의된 것 : '함수의 중첩'
  - 함수 몸체 내부에서 정의한 함수를 '중첩 함수(nested function)'
  - 중첩 함수를 포함하는 함수 '외부 함수(outer function)'
- 함수는 중첩될 수 있으므로 함수의 지역 스코프도 중첩될 수 있음
  - 스코프는 함수에 의해 계층적 구조를 갖음
  - 중첩 함수의 지역 스코프는 중첩 함수를 포함하는 외부 함수의 지역 스코프와 계층적 구조
  - 외부 함수의 지역 스코프를 중첩 함수의 상위 스코프

- 위 예제에서 지역은 outer함수의 지역과 inner 함수의 지역이 있음
  - inner함수는 outer 함수의 중첩 함수
  - outer 함수가 만든 outer 함수의 지역 스코프는 inner 함수가 만든 inner 함수 지역 스코프의 상위 스코프
  - outer 함수의 지역 스코프의 상위 스코프는 전역 스코프

![](https://poiemaweb.com/assets/fs-images/12-3.png)

- 스코프 체인(Scope chain) : 스코프가 계층적으로 연결된 것
  - 최상위 스코프 : 전역 스코프
  - 전역에서 선언된 함수 outer의 지역 스코프
  - outer내부에서 선언된 함수 inner의 지역 스코프

- 변수를 참조할 때, 자바스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작하여 상위 스코프 방향으로 이동하며 선언된 변수 검색
  - 이를 통해 **상위 스코프에서 선언한 변수를 하위 스코프에서도 참조 가능**
- 자바스크립트 엔진은 코드를 실행하기 앞서 위 그림과 유사한 데이터(렉시컬 환경 Lexical Environment)를 실제로 생성하고 변수의 할당이 일어나면 이 데이터의 값을 변경
  - 변수의 검색도 이 데이터 상에서 이루어짐

- 렉시컬 환경(Lexical Environment) : 스코프 체인은 실행 컨텍스트(Execution Context)의 렉시컬 환경을 연결(Channing)한 것
  - 전역 렉시컬 환경은 코드가 로드되고 곧바로 생서되고 함수의 렉시컬 환경은 함수가 호출되고 곧바로 생성



## 4. 스코프 체인에 의한 변수 검색

- 2.1 예제의 4,5,6을 살펴보면 자바스크립트 엔진이 스코프 체인을 통해 어떻게 변수를 찾아내는지 이해할 수 있음
  - 4 변수 x를 참조하는 코드의 스코프인 inner 함수의 지역 스코프에서 변수 x가 선언되었는지 검색 -> 함수 inner 내에는 선언된 변수 x 존재 -> 검색된 변수 참조 후 검색 종료
  - 5 변수 y를 참조하는 코드의 스코프인 inner 함수의 지역 스코프에서 변수 y가 선언되었는지 검색 -> 함수 Inner 내에는 변수 y 선언이 존재하지 않으므로 상위 스코프인 함수 outer의 지역 스코프로 이동 -> 함수 outer내에도 없으므로 상위 스코프인 전역 스코프로 이동 -> 전역 스코프에 변수 y 선언 존재하므로 검색된 변수 참조 후 검색 종료
  - 6 변수 z를 참조하는 코드의 스코프인 inner 함수의 지역 스코프에서 변수 z가 선언되었는지 검색 -> 함수 inner 내에 변수 z 선언이 존재하지 않으므로 상위 스코프인 함수 outer의 지역 스코프로 이동 -> 함수 outer내에 변수 z선언 존재하므로 검색된 변수 참조 후 검색 종료
- 자바스크립트 엔진은 스코프 체인을 따라 변수를 참조하는 코드의 스코프에서 상위 스코프 방향으로 이동하며 선언된 변수 검색
  - 절대! 하위 스코프로 내려가며 식별자를 검색하는 일 없음
  - **상위 스코프에서 유효한 변수는 하위 스코프에서 자유롭게 참조할 수 있지만 하위 스코프에서 유효한 변수를 상위 스코프에서 참조할 수 없음**

- 스코프 체인으로 연결된 스코프 계층적 구조는 부자 관계로 이루어진 상속(Inheritance)과 유사
  - 상속을 통해 부모의 자산을 자식이 자유롭게 사용할 수 있지만 자식의 자산을 부모가 사용할 수 없음



## 5. 스코프 체인에 의한 함수 검색

- 전역에서 정의된 함수 foo 함수와 bar 함수 내부에서 정의된 foo 함수

```javascript
// 전역 함수
function foo() {
  console.log('global function foo');
}

function bar() {
  // 중첩 함수
  function foo() {
    console.log('local function foo');
  }

  foo(); // ① local function foo
}

bar();
```

- 함수 선언문으로 함수를 정의하면 자바스크립트 엔진에 의해 다른 코드가 실행되기 이전 함수 객체 먼저 생성
  - 자바스크립트 엔진은 함수 이름과 동일한 이름의 변수를 암묵적으로 선언하고 생성된 함수 객체 할당
  - 1에서 함수 foo를 호출하면 자바스크립트 엔진은 함수를 호출하기 위해 먼저 함수를 가리키는 변수 foo 검색
  - 함수도 변수에 할당되기 때문에 스코프를 갖음 : 함수는 변수에 함수 객체가 할당된 것 외에는 일반 변수와 다를 게 없음
- 스코프를 변수를 검색할 때 사용하는 규칙 이라고 표현하기 보다는 "**식별자를 검색하는 규칙**"이라고 표현하는 것이 보다 적합



## 6. 함수 레벨 스코프

- 지역은 함수 몸체 내부를 말하고 지역은 지역 스코프를 만듦
  - 코드 블록이 아닌 함수에 의해서만 지역 스코프가 생성된다는 의미
- 블록 레벨 스코프(Block level scope) : 다른 프로그래밍 언어는 함수 몸체 뿐 아니라 모든 코드 블록이 지역 스코프를 만듦
- 함수 레벨 스코프(Function level scope) : var 키워드로 선언된 변수는 오로지 함수의 코드 블록 만들 지역 스코프로 인정

> ES6에서 도입된 let, const 키워드는 블록 레벨 스코프를 지원

```javascript
var x = 1;

if (true) {
  // var 키워드로 선언된 변수는 함수의 코드 블록 만을 지역 스코프로 인정한다.
  // 함수 밖에서 선언된 변수는 코드 블록 내에서 선언되었다 할 지라도 모두 전역 변수이다.
  // 따라서 x는 전역 변수이다. 이미 선언된 전역 변수 x가 있으므로 변수 x는 중복 선언된다.
  // 이는 의도치 않게 변수값이 변경되는 부작용을 발생시킨다.
  var x = 10;
}

console.log(x); // 10
```

- 전역 변수 x 선언, if문 코드 내 변수 x 선언
  - 이 때 if문 코드 블록 내 변수 x도 전역 변수
  - var 키워드로 선언된 변수는 블록 레벨 스코프를 인정하기 때문에 함수 밖에서 선언된 변수는 코드 블록 내에서 선언되었다 할지라도 모두 전역 변수
  - 따라서 전역 변수 x는 중복 선언되고 그 결과 의도치 않은 전역 변수의 값이 재할당

```javascript
var i = 10;

// for문에서 선언한 i는 전역 변수이다. 이미 선언된 전역 변수 i가 있으므로 중복 선언된다.
for (var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}

// 의도치 않게 변수의 값이 변경되었다.
console.log(i); // 5
```

- 블록 레벨 스코프를 지원하는 프로그래밍 언어에서는 for문에서 반복을 위해 선언된 변수 i가 for문의 코드 블록 내에서만 유효한 지역 변수
  - 그러나 var 키워드로 선언된 변수는 블록 레벨 스코프를 인정하지 않기 때문에 변수 i는 전역 변수
  - 전역 변수 i는 중복 선언되고 그 결과 의도치 않은 전역 변수 값 재할당



## 7. 렉시컬 스코프

```javascript
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // ? 1
bar(); // ? 1
```

- 실행 결과를 예측해보자. 함수 bar의 상위 스코프가 무엇인지에 따라 결정됨
  1. **함수를 어디서 호출**했는지에 따라 함수의 상위 스코프 결정
  2. **함수를 어디서 정의**했는지에 따라 함수의 상위 스코프 결정

- 프로그래밍 언어는 일반적으로 이 두가지 방식 중 하나의 방식으로 상위 스코프 결정
  - 첫 번째 방식으로 함수 상위 스코프 결정 : 함수 bar의 상위 스코프는 함수 foo와 전역
  - 두 번째 방식으로 함수 상위 스코프 결정 : 함수 bar의 상위 스코프는 전역
- 첫 번째 방식. 동적 스코프(Dynamic scope) : 함수는 어디서 호출될 지 함수 정의 시점에서는 알 수 없으므로 함수가 호출되는 시점에 동적으로 스코프를 결정해야 하기 때문에 동적 스코프라 부름
- 두 번째 방식. 렉시컬 스코프(Lexical scope) 또는 정적 스코프(Static scope) : 동적 스코프 방식처럼 상위 스코프가 동적으로 변하지 않고 함수 정의가 평가되는 시점에 상위 스코프가 정적으로 결정되기 때문에 정적 스코프라고 부름
  - 자바스크립트를 비롯한 대부분의 언어는 렉시컬 스코프를 따름
- 자바스크립트는 렉시컬 스코프를 따르므로 함수를 어디서 호출했는지가 아니라 어디서 정의했는지에 따라 상위 스코프 결정
  - 모든 함수 정의(함수 선언문 또는 함수 표현식)는 평가되어 함수 객체를 생성할 때, 자신이 정의된 스코프를 기억
  - 함수가 호출되면 언제나 자신이 기억하고 있는 자신이 정의된 스코프를 상위 스코프로 사용
  - 함수가 호출된 위치는 상위 스코프 결정에 어떠한 영향을 주지 않음
- 위 예제의 bar 함수는 전역에서 정의된 함수
  - bar 함수 정의는 전역 코드가 실행되기 전에 먼저 평가되거 함수 객체 생성
    - 이 때 생성된 bar 함수 객체는 자신이 정의된 스코프, 즉 전역 스코프를 기억
    - bar 함수가 호출되면 호출된 곳이 어디인지 관계없이 언제나 자신이 기억하고 있는 전역 스코프를 상위 스코프로 사용
  - 위 예제는 전역 변수 x의 값 1을 두 번 출력
- 렉시컬 환경과 함수 객체의 내부 슬롯 [[Environment]]
  - 함수 객체가 기억하는 것은 렉시컬 환경(Lexical Environment)
  - 함수 정의가 평가되어 함수 객체를 생성할 때, 자신이 정의된 스코프 즉 렉시컬 환경을 생성한 함수 객체의 내부 슬롯 [[Environment]]에 저장
  - 렉시컬 환경은 자바스크립트 엔진이 식별자와 스코프를 관리하기 위해 사용하는 일종의 자료 구조로 스코프를 구분하여 식별자를 등록하고 관리하는 저장소 역할



## 8. 암묵적 전역 변수

```javascript
function foo() {
  // 선언하지 않은 변수에 값을 할당하면 암묵적 전역 변수가 된다.
  x = 10;
}

foo();

console.log(x); // 10
```

- 선언하지 않은 변수에 값을 할당하면 자바스크립트 엔진은 아무런 에러없이 암묵적으로 전역 변수를 선언하고 값을 할당
- 함수 foo가 호출되면 변수 x에 값을 할당하기 위해 자바스크립트 엔진은 스코프 체인을 따라 함수 foo의 스코프부터 변수 x 선언 찾음 -> 없으므로 상위스코프인 전역 스코프에서 찾음 -> 없으므로 참조 에러가 발생해야 하지만 자바스크립트 엔진은 암묵적으로 전역 변수 x 선언
  - **암묵적 전역 변수(implicit global)**라 함
- 선언하지 않은 변수에 값을 할당하는 것은 문법적으로 허용되지만 의도하지 않게 전역 변수를 생성하고 이로 인해 전역 변수가 중복 선언될 수 있음
  - 변수가 중복 선언되면 의도치 않게 먼저 선언된 전역 변수의 값 변경

```javascript
// x.js
function foo() {
  i = 0; // 암묵적 전역 변수
  // ...
}

// y.js
// var 키워드로 선언된 변수는 함수의 코드 블록 만을 지역 스코프로 인정한다.
// 따라서 for문에서 선언한 i는 전역 변수이다.
// 이미 암묵적 전역 변수 i가 있으므로 변수 i는 중복 선언된다.
for (var i = 0; i < 5; i++) {
  foo();
  console.log(i);
}
```

- 두 개의 자바스크립트 파일이 있다고 가정하면

```html
<!DOCTYPE html>
<html>
<body>
  <script src='x.js'></script>
  <script src='y.js'></script>
</body>
</html>
```

- html에서 이 자바스크립트 파일 로드시 변수 i는 중복
- 자바스크립트는 파일마다 독립적 파일 스코프를 갖지 않음 : 파일이 여러개로 분리되어 있더라도 하나의 전역 스코프를 공유함
- 이러한 특징은 자바스크립트에 모듈이라는 개념이 없기 때문
  - 전역 변수가 중복되는 문제 발생
  - 위 예제의 x.js와 y.js에 모두 변수 i가 존재하는데 x.js의 변수 i는 암묵적 전역 변수
  - y.js의 변수 i는 for문을 위한 변수이지만 var 키워드로 선언하였으므로 블록 레벨 스코프를 따르지 않아 전역 변수
- 위 예제의 전역 변수 i는 중복되고 나중에 선언된 변수 i의 값이 먼저 선언된 변수 i의 값을 덮어씀
  - 자바스크립트는 변수의 중복 선언을 허용하므로 어떠한 에러도 발생하지 않고 무한 반복 상태



모든 출처 : https://poiemaweb.com/