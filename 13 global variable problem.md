# 13. 전역 변수의 문제점

- 전역변수의 무분별한 사용은 위험하므로 반드시 전역 변수를 사용해야 할 이유를 찾지 못한다면 지역분수를 사용해야 함



## 1. 변수의 생명 주기

### 1.1 지역 변수의 생명 주기

- 변수는 선언에 의해 생성되고 할당을 통해 값을 갖고, 언젠가 소멸
  - 생명 주기(Life cycle) 존재
  - 생명 주기가 없다면 한 번 선언된 변수는 프로그램을 종료하지 않는 한 영원히 메모리 공간 점유
- 변수는 자신이 선언된 위치에서 생성되고 소멸
  - 전역 변수의 생명 주기는 애플리케이션의 생명 주기와 같음
  - 하지만 함수 내부에서 선언된 지역 변수는 함수가 호출되면 생성되고 함수가 종료하면 소멸

```javascript
function foo() {
  var x = 'local';
  console.log(x); // local
  return x;
}

foo();
console.log(x); // ReferenceError: x is not defined
```

- 지역 변수 x는 foo 함수가 호출되기 이전까지는 생성되지 않음
  - foo 함수를 호출하지 않으면 함수 내부의 선언문이 실행되지 않기 때문
- 변수 선언은 다른 코드가 실행되기 이전에 변수 선언이 어디에 있던지 상관없이 가장 먼저 실행
  - 변수 선언은 코드가 한 줄씩 순차적으로 실행되는 시점인 런타임(run-time)이 아닌 런타임 이전 단계에서 자바스크립트 엔진에 의해 먼저 실행
  - 전역 변수에 한정된 설명
- **함수 내부에서 선언한 변수**는 **함수가 호출된 직후**에 함수 몸체의 다른 코드가 실행되기 이전에 자바스크립트 엔진에 의해 먼저 실행

- foo 함수 호출시 함수 몸체의 다른 문들이 순차적으로 실행되기 이전에 **변수 x의 선언문이 자바스크립트 엔진에 의해 가장 먼저 실행되어 변수 x가 선언되고 undefined로 초기화**

- 함수 몸체의 문들이 순차적으로 실행되기 시작하고 변수 할당문이 실행되면 변수 x에 값 'local'할당
- 함수 종료시 변수 x도 소멸되어 생성주기 종료
  - 따라서 **함수 내부에서 선언된 지역 변수 x는 foo함수가 호출되어 실행되는 동안에만 유효**
  - 지역 변수의 생명 주기는 함수의 생명 주기와 일치

```javascript
var x = 'gloabl';

function foo() {
  console.log(x); // ① undefined
  var x = 'local';
  return x;
}

foo();
console.log(x); // gloabl
```

- 함수 foo 내부에서 선언된 지역변수 x는 1 시점에 이미 선언되고 undefined로 초기화
- 전역 변수 x를 참조하는 것이 아니라 지역 변수 x를 참조하여 값 출력
  - 지역 변수는 함수 전체에서 유효(변수 할당문 실행 이전까지는 undefined 값)
- **변수 호이스팅은 스코프를 단위로 동작**
  - 전역 변수의 호이스팅은 전역 변수의 선언이 전역 스코프의 선두로 끌어 올려진 것처럼 동작 -> 전역 변수는 전역 전체에서 유효
  - 지역 변수의 호이스팅은 지역 변수의 선언이 지역 스코프의 선두로 끌어 올려진 것처럼 동작 -> 지역 변수는 함수 전체에서 유효
- 변수 호이스팅은 변수 선언이 스코프의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징



### 1.2 전역 변수의 생명 주기

- 함수와 달리 전역 코드는 명확한 호출 없이 실행
  - 전역 코드는 함수 호출과 같이 전역 코드를 실행하는 특별한 진입점(entry point)이 없고 코드가 로드되자마자 곧바로 해석되고 실행

> 진입점(entry point) : C나 Java으로 작성된 코드를 실행하면 가장 먼저 main함수가 호출. 이 main 함수는 프로그램이 시작되는 지점이므로 이를 진입점 또는 시작점이라고 함

- 함수는 함수 몸체의 마지막 문 또는 return 문이 실행되면 종료
  - 전역 코드에는 return문을 사용할 수 없으므로 마지막 문이 실행되어 더 이상 실행할 문이 없을 때 종료

- return 문의 위치 : return 문은 함수 몸체 내부에서만 사용할 수 있고, 전역에서 사용시 문법 에러(SyntaxError) 발생
  - 참고로 Node.js는 모듈 시스템에 의해 파일 별로 독립적인 파일 스코프를 갖으므로 파일의 가장 바깥 영역에 return문을 사용해도 에러가 발생하지 않음
- var키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 됨
  - 브라우저 환경에서 전역 객체는 window이므로 브라우저 환경에서 var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티
  - 이 전역 객체 window는 웹페이지가 종료하기 전까지 유효
  - 브라우저 환경에서 var 키워드로 선언한 전역 변수는 웹페이지를 종료할 때까지 유효

![](https://poiemaweb.com/assets/fs-images/13-2.png)



## 2. 전역 변수의 문제점

#### 암묵적 결합

- 전역 변수를 사용하는 의도는 코드 어디에서든지(전역) 전역 변수를 사용하겠다는 것
- 이는 모든 코드가 전역 변수를 참조하고 변경할 수 있는 암묵적 결합(Implicit coupling)을 허용하는 것으로 변수의 유효 범위가 크면 클 수록 코드 가독성은 나빠지고 의도치 않게 상태가 변경될 수 있는 위험성 상승

#### 긴 생명 주기

- 전역 변수는 생명 주기가 긺
  - 전역 변수의 상태를 변경할 수 있는 시간도 길고, 모든 함수가 참조할 수 있기 때문에 상태를 변경할 기회도 많음
  - 메모리 리소스도 오랜 기간 소비
- var 키워드는 변수의 중복 선언을 허용하므로 생명 주기가 긴 전역 변수는 변수 이름이 중복될 가능성이 있음
  - 변수 이름이 중복되면 의도치 않은 재할당 이루어짐

```javascript
var x = 1;

// ...

// 변수의 중복 선언. 기존 변수에 값을 재할당한다.
var x = 100;

console.log(x); // 100
```

- 지역 변수는 전역 변수보다 생명 주기가 훨씬 짧음
  - 크지 않은 함수의 지역 변수의 경우, 그 생존 기간은 극히 짧음
  - 따라서 지역 변수의 상태를 변경할 수 있는 시간도 짧고 기회도 적음
  - 이는 전역 변수보다 상태 변경에 의한 오류가 발생할 확률이 작다는 것을 의미하고 메모리 리소스도 짧은 기간만 소비

#### 스코프 체인 상에서 종점에 존재

- 전역 변수의 또 하나의 문제 : 스코프 체인 상에서 종점에 존재한다는 것
  - 변수를 검색할 때 전역 변수가 가장 마지막에 검색된다는 것을 말함
  - 즉, **전역 변수의 검색 속도가 가장 느림**(차이가 크진 않지만 존재)

#### 네임 스페이스 오염

- 자바스크립트에서 가장 큰 문제점 : 파일이 분리되어 있다해도 하나의 전역 스코프를 공유
  - 따라서 다른 파일 내에서 동일한 이름으로 명명된 변수나 함수가 같은 스코프 내에 존재할 경우 예상치 못할 결과를 가져올 수 있음



## 3. 전역 변수 사용 억제 방법

> 즉시 실행 함수, 네임 스페이스 객체, 모듈 패턴, ES6 모듈

- 변수의 스코프는 좁을수록 좋고, 전역 변수를 반드시 사용하여야 할 이유를 찾지 못한다면 지역 변수를 사용해야 함
  - 전역 변수를 절대 사용하지 말라는 의미가 아니라 남발을 억제하자는 것

### 3.1 즉시 실행 함수

- 함수의 정의와 동시에 호출되는 즉시 실행 함수는 단 한번만 호출
  - 모든 코드를 즉시 실행 함수로 감싸면 모든 변수는 즉시 실행 함수의 지역 변수가 됨 -> 이러한 특성을 이용해 전역 변수의 사용을 제한하는 방법

```javascript
(function () {
  var foo = 10; // 즉시 실행 함수의 지역 변수
  // ...
}());

console.log(foo); // ReferenceError: foo is not defined
```

- 이 방법을 사요하면 전역 변수를 생성하지 않으므로 라이브러리 등에 자주 사용



### 3.2 네임 스페이스 객체

- 전역에 네임 스페이스(Namespace) 역할을 담당할 객체를 생성하고 전역 변수처럼 사용하고 싶은 변수를 프로퍼티로 추가하는 방법

```javascript
var MYAPP = {}; // 전역 네임 스페이스 객체

MYAPP.name = 'Lee';

console.log(MYAPP.name); // Lee
```

- 네임 스페이스 객체에 또 다른 네임 스페이스 객체를 프로퍼티로 추가하여 네임 스페이스를 계층적으로 구성할 수도 있음

```javascript
var MYAPP = {}; // 전역 네임 스페이스 객체

MYAPP.person = {
  name: 'Lee',
  address: 'Seoul'
};

console.log(MYAPP.person.name); // Lee
```

- 네임 스페이스를 분리하여 식별자 충돌을 방지하는 효과는 있으나 네임 스페이스 객체 자체가 전역 변수에 할당되므로 그다지 유용해 보이지는 않음



### 3.3 모듈 패턴

- 모듈 패턴은 클래스를 모방하여 관련 있는 변수와 함수를 모아 즉시 실행 함수로 감싸 하나의 모듈 생성
  - 모듈 패턴은 자바스크립트의 강력한 기능인 클로저를 기반으로 동작
  - 모듈 패턴의 특징은 전역 변수의 억제는 물론 캡슐화까지 구현 가능
- 캡슐화 : 외부에 공개될 필요가 없는 정보를 외부에 노출시키지 않고 숨기는 것을 말하며 정보 은닉(information hiding)이라고도 함
  - Java의 경우, 클래스를 구성하는 멤버에 대하여 public, private, protected 등의 접근 제한자(Access modifier)를 사용해 공개 범위 한정 가능
  - 하지만, 자바스크립트는 public, private, protexted 등의 접근 제한자를 제공하지 않음
- 모듈 패턴은 전역 네임 스페이스의 오염을 막는 기능은 물론 한정적이기는 하지만 캡슐화를 구현하기 위해 사용

```javascript
var Counter = (function () {
  // private 변수
  var num = 0;

  // 외부로 공개할 데이터나 메소드를 프로퍼티로 추가한 객체를 반환한다.
  return {
    increase() {
      return ++num;
    },
    decrease() {
      return --num;
    }
  };
}());

// private 변수는 외부로 노출되지 않는다.
console.log(Counter.num); // undefined

console.log(Counter.increase()); // 1
console.log(Counter.increase()); // 2
console.log(Counter.decrease()); // 1
console.log(Counter.decrease()); // 0
```

- 이 예제의 즉시 실행 함수는 객체를 반환
  - 이 객체에는 외부에 노출하고 싶은 변수나 함수를 담아 반환
  - 이 떄 반환되는 객체의 프로퍼티는 외부에 노출되는 퍼블릭 멤버(public member)
  - 외부로 노출하고 싶지 않은 변수나 함수는 반환하는 객체에 추가하지 않으면 외부에서 접근할 수 없는 프라이빗 멤버(private member)가 됨



### 3.4 ES6 모듈

- 전역 변수의 남발을 억제하기 위해 ES6에서 도입된 모듈을 사용할 수도 있음
- 2019년 1월 기준 대부분의 브라우저가 ES6 모듈을 완전히 지원하지 않고 있음
  - 현재 브라우저에서 ES6 모듈을 사용하려면 SystemJS, RequireJS 등의 모듈 로더 또는 Webpack 등의 모듈 번들러를 사용해야 함



모든 출처 : https://poiemaweb.com/