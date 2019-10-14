# 15. 프로퍼티 정의

## 1. 프로퍼티 정의란?

- 프로퍼티 정의란 프로퍼티 어트리뷰트의 값을 정의하여 프로퍼티의 상태를 관리하는 것을 말함
  - 예) 프로퍼티 값을 갱신 가능하도록 할 것인가, 프로퍼티를 열거 가능하도록 할 것인가, 프로퍼티를 재정의 가능하도록 할 것인가
  - 이를 통해 객체의 프로퍼티가 어떻게 동작해야 하는지를 명확히 정의할 수 있음
- **자바스크립트 엔진은 프로퍼티를 생성(객체 리터럴의 평가, 프로퍼티 동적 생성)할 때, 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의**

```javascript
const obj = {};

// 자바스크립트 엔진은 프로퍼티를 생성할 때, 프로퍼티의 상태를 나타내는
// 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.
obj.prop = 10;

// 정의된 프로퍼티 어트리뷰트는 Object.getOwnPropertyDescriptor 메소드로 확인할 수 있다.
var descriptor = Object.getOwnPropertyDescriptor(obj, 'prop');
console.log(descriptor);
// obj 객체의 prop 프로퍼티는 value, writable, enumerable, configurable
// 어트리뷰트가 정의되어 있다.
// {value: 10, writable: true, enumerable: true, configurable: true}
```

- 프로퍼티 정의와 프로퍼티 동적 생성은 의미가 다름
  - 위 예제의 두번째 라인에 작성한 코드 obj.prop = 10;의 주목적은 프로퍼티에 값을 할당하는 것
    - 하지만, 프로퍼티가 없을 경우 프로퍼티를 동적으로 생성하여 추가하는 부수 효과
    - 즉, 프로퍼티 유무를 먼저 확인하여 존재하는 프로퍼티에 값을 할당할 것인지, 프로퍼티를 동적으로 생성한 후 값을 할당한 것인지 결정

- 프로퍼티 동적 생성 : 프로퍼티가 존재하지 않을 때, 프로퍼티를 생성하여 추가하는 것
- 프로퍼티 정의 : 프로퍼티 어트리뷰트를 정의하는 것
  - 프로퍼티 어트리뷰트는 프로퍼티의 상태를 나타냄
  - 프로퍼티 상태 : 프로퍼티 값(value), 값의 생신 가능 여부(writable), 열거 가능 여부(enumerable), 재정의 가능 여부(configurable)
- 프로퍼티 어트리 뷰트는 Object.getOwnPropertyDscriptor 메소드를 사용해 참조할 수 있음
  - 이 메소드는 프로퍼티 어트리뷰트 정보를 제공하는 객체인 프로퍼티 디스크립터(PropertyDescriptor)를 반환
  - Object.getOwnPropertyDscriptor 메소드는 이름에서 알 수 있듯, 자신의 프로퍼티 디스크립터 객체 반환
    - 존재하지 않는 프로퍼티나 상속받은 프로퍼티에 대한 프로퍼티 디스크립터를 요구하면 undefined 반환
    - 인수는 객체의 참조와 데이터 프로퍼티키의 키를 문자열로 전달

```javascript
// 하나의 프로퍼티 어트리뷰트를 표현하는 프로퍼티 디스크립터 객체를 취득한다.
var descriptor = Object.getOwnPropertyDescriptor(obj, 'prop');
console.log(descriptor);
// {value: 10, writable: true, enumerable: true, configurable: true}

// ES8
// 모든 프로퍼티 어트리뷰트를 표현하는 프로퍼티 디스크립터 객체를 취득한다.
var descriptors = Object.getOwnPropertyDescriptors(obj);
console.log(descriptors);
// {
//   prop: { value: 10, writable: true, enumerable: true, configurable: true }
// }
```

- 프로퍼티가 동적 생성될 때 자바스크립트 엔진은 프로퍼티 어트리뷰트를 기본값으로 정의
  - 이미 정의된 프로퍼티 어트리뷰트를 재정의하는 것도 가능



## 2. 내부 슬롯/메소드

- 내부 슬롯(Internal slot)과 내부 메소드(Internal method) : ECMAScript 스펙에서 요구하는 객체와 관련된 내부 상태와 내부 동작을 정의한 것
  - 내부 슬롯과 내부 메소드는 자바스크립트 엔진이 코드를 실행하는 알고리즘을 설명하기 위해 ECMAScript 스펙에서 사용하는 의사 프로퍼티(Pseudo property)와 의사 메소드(Pseudo method)
  - ECMAScript 스펙에 등장하는 이중 대괄호 [[...]]로 감싼 이름들이 내부 슬롯과 내부 메소드

![](https://poiemaweb.com/assets/fs-images/15-1.png)

- 내부 슬롯과 내부 메소드는 자바스크립트 엔진의 내부 구현 사양을 정의한 것
  - 자바스크립트 엔진은 ECMAScript 스펙에서 정의한 내부 슬롯과 내부 메소드의 사양을 충실히 만족시키는 것이 요구될 뿐 이를 외부로 노출시키지 않음
- 내부 슬롯과 내부 메소드는 객체의 프로퍼티가 아님
  - 내부 슬롯과 내부 메소드는 직접적으로 접근하거나 호출할 수 있는 방법을 원칙적으로 제공하지는 않으나 일부 내부 슬롯과 내부 메소드(의 구현체)에 간접적으로 접근할 수 있는 수단은 있음
- 모든 객체가 갖고 있는 내부 메소드 [[Get]]에 대해 살펴보자

![](https://poiemaweb.com/assets/fs-images/15-2.png)

- 이 내부 메소드는 프로퍼티 키로 프로퍼티 값에 접근하면 내부적으로 호출됨
- 내부 메소드 [[Get]]의 동작을 파악하면 자바스크립트 엔진이 어떻게 프로퍼티 키로 프로퍼티 값에 접근하는지 파악 가능
- [[Get]] 내부 메소드는 아래와 같이 동작
  1. 프로퍼티 키가 유효한지 확인 -> 프로퍼티 키는 문자열 또는 심볼이어야 함
  2. 프로토타입 체인에서 프로퍼티 검색
  3. 검색된 프로퍼티가 "데이터 프로퍼티(Data property)"라면 프로퍼티 값, 즉 데이터 프로퍼티의 프로퍼티 어트리뷰트 [[Value]]의 값을 그대로 반환
  4. 만약 프로퍼티가 "접근자 프로퍼티(Accessor property)"라면 접근자 프로퍼티의 프로퍼티 어트리뷰트 [[Get]]의 값, 즉 getter 함수를 호출하여 그 결과 반환
- 내부 슬롯과 내부 메소드에 대해 관심이 있다면 ECMAScript 사양을 읽어볼 것

#### 프로토타입과 프로토타입 체인

- 프로토타입 : 어떤 객체의 상위(부모) 객체의 역할을 하는 객체
  - 프로토타입은 하위(자식) 객체에게 자신의 프로퍼티와 메소드를 상속
  - 프로토타입 객체의 프로퍼티나 메소드를 상속받은 하위객체는 자신의 프로퍼티 또는 메소드 인것처럼 자유롭게 사용 가능
- 프로토 타입 체인 : 프로토타입이 단방향 링크드 리스트 형태로 연결되어 있는 상속 구조
  - 객체의 프로퍼티나 메소드에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티 또는 메소드가 없다면 프로포타입 체인을 따라 프로토타입 프로퍼티나 메소드를 차례대로 검색



## 3. 접근자 프로퍼티

- 프로퍼티는 데이터 프로퍼티와 접근자 프로퍼티로 구분 가능
  - 데이터 프로퍼티(Data property) : 키와 값으로 구성된 일반적인 프로퍼티(지금까지 살펴본 모든 프로퍼티)
  - 접근자 프로퍼티(Accessor property) : 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수(Accessor function)로 구성된 프로퍼티
- 접근자 함수는 getter/setter 함수라고도 부름
  - 접근자 프로퍼티는 getter와 setter 함수를 모두 정의할 수도 있고 하나만 정의할 수도 있음

```javascript
const person = {
  // 데이터 프로퍼티
  firstName: 'Ungmo',
  lastName: 'Lee',

  // fullName은 접근자 함수로 구성된 접근자 프로퍼티이다.
  // getter 함수
  get fullName() {
    return this.firstName + ' ' + this.lastName;
  },
  // setter 함수
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(' ');
  }
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조.
console.log(person.firstName + ' ' + person.lastName); // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
person.fullName = 'Heegun Lee';
console.log(person); // {firstName: "Heegun", lastName: "Lee"}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
console.log(person.fullName); // Heegun Lee

// firstName는 데이터 프로퍼티이다.
// 데이터 프로퍼티는 value, writable, enumerable, configurable 프로퍼티 어트리뷰트를 갖는다.
let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log(descriptor);
// {value: "Heegun", writable: true, enumerable: true, configurable: true}

// fullName는 접근자 프로퍼티이다.
// 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 갖는다.
descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log(descriptor);
// {get: ƒ, set: ƒ, enumerable: true, configurable: true}
```

- person 객체의 firstName과 lastName 프로퍼티는 일반적인 데이터 프로퍼티

- 메소드 앞에 get, set이 붙은 메소드가 있는데 이것들이 바로 getter와 setter 함수이고 getter/setter 함수의 이름 fullName이 접근자 프로퍼티

  - 접근자 프로퍼티는 자체적으로 값(value 프로퍼티 어트리뷰트)을 가지지 않으며 다만 데이터 프로퍼티의 값을 읽거나 저장할 때 관여할 뿐

- 이를 내부 슬롯/메소드 관점에서 설명하면 다음과 같음

  - 접근자 프로퍼티 fullName으로 프로퍼티 값에 접근하면 내부적으로 [[Get]] 내부 메소드가 호출되어 아래와 같이 동작

  1. 프로퍼티 키가 유효한지 확인 -> 프로퍼티 키 "fullName"은 문자열이므로 유효한 프로퍼티 키
  2. 프로토타입 체인에서 프로퍼티 검색 -> person 객체에 fullName 프로퍼티 존재
  3. 검색된 fullName 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인 -> fullName 프로퍼티는 접근자 프로퍼티
  4. 접근자 프로퍼티 fullName의 프로퍼티 어트리뷰트 [[Get]]의 값, 즉 getter 함수를 호출하여 그 결과 반환
  5. 프로퍼티 fullName의 프로퍼티 어트리뷰트 [[Get]]의 값은 Object.getOwnPropertyDescriptor 메소드가 반환하는 프로퍼티 디스크립터(PropertyDescriptor) 객체의 get 프로퍼티 값과 같음

> ECMAScript 스펙에서 정의한 [[Get]] 내부 메소드의 사양을 만족시키는 구현체가 자바스크립트 엔진에 존재한다는 것이 중요하지 [[Get]]이란 이름으로 실제 자바스크립트 엔진이 구현되었는지는 중요하지 않음(어차피 [[Get]]내부 메소드에 직접 접근 불가능)

- 접근자 프로퍼티와 데이터 프로퍼티 구별 방법

```javascript
// 일반 객체의 __proto__는 접근자 프로퍼티이다.
Object.getOwnPropertyDescriptor(Object.prototype, '__proto__');
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}

// 함수 객체의 prototype은 데이터 프로퍼티이다.
Object.getOwnPropertyDescriptor(function() {}, 'prototype');
// {value: {…}, writable: true, enumerable: false, configurable: false}
```

- Object.getOwnPropertyDescriptor 메소드가 반환한 프로퍼티 어트리뷰트를 객체로 표현한 프로퍼티 디스크립터 객체를 유심히 살펴보면 접근자 프로퍼티와 데이터 프로퍼티의 프로퍼티 디스크립터 객체의 프로퍼티가 다른 것을 알 수 있음



## 4. 프로퍼티 어트리뷰트

- 프로퍼티 어트리뷰트(property attribute) : 모든 프로퍼티, 즉 데이터 프로퍼티와 접근자 프로퍼티는 자신의 상태와 동작을 정의한 내부 슬롯/메소드를 가지고 있음
  - 프로퍼티 어트리뷰트는 자바스크립트 엔진이 프로퍼티를 생성할 때, 기본값으로 자동 정의
  - 이후 정의된 프로퍼티 어트리뷰터를 설정하는 것으로 각각의 프로퍼티 세부 동작(프로퍼티 값의 변경 가능 여부, 열거 가능 여부, 재정의 가능 여부)를 제어할 수 있음
- 데이터 프로퍼티는 아래와 같은 프로퍼티 어트리뷰트를 갖음

데이터 프로퍼티는 아래와 같은 프로퍼티 어트리뷰트를 갖음

| 프로퍼티 어트리뷰트 | 설명                                                         | 프로퍼티 디스크립터 객체의 프로퍼티 |
| :------------------ | :----------------------------------------------------------- | :---------------------------------- |
| [[Value]]           | - 프로퍼티 키로 프로퍼티 값에 접근하면 내부 메소드 [[Get]]에 의해 반환되는 값이다. - 프로퍼티 키로 프로퍼티 값을 저장하면 [[Value]]에 값을 저장한다. 이때 프로퍼티가 없으면 프로퍼티를 생성하고 생성된 프로퍼티의 [[Value]]에 값을 저장한다. | value                               |
| [[Writable]]        | - 프로퍼티 값의 변경 가능 여부를 나타내며 불리언 값을 갖는다. - [[Writable]]의 값이 false인 경우, 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없다. | writable                            |
| [[Enumerable]]      | - 프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖는다. - [[Enumerable]]의 값이 false인 경우, 해당 프로퍼티는 for…in 문이나 Object.keys 메소드 등으로 열거할 수 없다. | enumerable                          |
| [[Configurable]]    | - 프로퍼티의 재정의 가능 여부를 나타내며 불리언 값을 갖는다. - [[Configurable]]의 값이 false인 경우, 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지된다. 단, [[Writable]]이 true인 경우, [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용된다. | configurable                        |

- 접근자 프로퍼티는 아래와 같은 프로퍼티 어트리뷰트를 갖음

접근자 프로퍼티는 아래와 같은 프로퍼티 어트리뷰트를 갖는다.

| 프로퍼티 어트리뷰트 | 설명                                                         | 프로퍼티 디스크립터 객체의 프로퍼티 |
| :------------------ | :----------------------------------------------------------- | :---------------------------------- |
| [[Get]]             | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수이다. 즉, 접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 [[Get]]의 값, 즉 getter 함수가 호출되고 그 결과가 프로퍼티 값으로 반환된다. | get                                 |
| [[Set]]             | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수이다. 즉, 접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 [[Set]]의 값, 즉 setter 함수가 호출되고 그 결과가 프로퍼티 값으로 저장된다. | set                                 |
| [[Enumerable]]      | 데이터 프로퍼티의 [[Enumerable]]와 같다.                     | enumerable                          |
| [[Configurable]]    | 데이터 프로퍼티의 [[Configurable]]와 같다.                   | configurable                        |

```javascript
const obj = { prop: 1 };

// 프로퍼티 어트리뷰트를 표현하는 프로퍼티 디스크립터 객체를 취득한다.
const descriptor = Object.getOwnPropertyDescriptor(obj, 'prop');
console.log(descriptor);
// → { value: 1, writable: true, enumerable: true, configurable: true }
```

- Object.getOwnPropertyDescriptor 메소드가 반환한 프로퍼티 디스크립터 객체를 살펴보면 value 프로퍼티 값은 1
  - 이는 프로퍼티 어트리뷰트 [[Value]]의 값이 1인 것을 의미
  - writable, enumerable, configurable 프로퍼티 모두 ture를 갖음
  - 이는 프로퍼티 어트리뷰트 [[Writable]], [[Enumerable]], [[Configurable]] 값이 모두 ture인 것을 말함
- 별도의 프로퍼티 정의 없이 기본적으로 생성된 프로퍼티의 [[Value]] 값은 프로퍼티 값을 나타내며 [[Writable]], [[Enumerable]], [[Configurable]]의 기본값은 true
  - 프로퍼티를 동적으로 추가해도 마찬가지임

```javascript
const obj = { prop1: 1 };

// 프로퍼티 동적 추가
obj.prop2 = 2;

// 프로퍼티 어트리뷰트를 표현하는 프로퍼티 디스크립터 객체를 취득한다.
const descriptor = Object.getOwnPropertyDescriptor(obj, 'prop2');
console.log(descriptor);
// → { value: 2, writable: true, enumerable: true, configurable: true }

Object.defineProperty 메소드를 사용하면 프로퍼티의 어트리뷰트를 정의할 수 있다. 인수는 객체의 참조와 데이터 프로퍼티의 키인 문자열 그리고 프로퍼티 디스크립터 객체를 전달한다.
const person = {};

// 데이터 프로퍼티 정의
Object.defineProperty(person, 'firstName', {
  value: 'Ungmo',
  writable: true,
  enumerable: true,
  configurable: true
});

Object.defineProperty(person, 'lastName', {
  value: 'Lee'
});

let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log('firstName', descriptor);
// firstName {value: "Ungmo", writable: true, enumerable: true, configurable: true}

// 디스크립터 객체의 프로퍼티를 누락시키면 undefined, false가 기본값이다.
descriptor = Object.getOwnPropertyDescriptor(person, 'lastName');
console.log('lastName', descriptor);
// lastName {value: "Lee", writable: false, enumerable: false, configurable: false}

// [[Enumerable]]의 값이 false인 경우,
// 해당 프로퍼티는 for…in 문이나 Object.keys 등으로 열거할 수 없다.
// lastName 프로퍼티는 [[Enumerable]]의 값이 false이므로 열거되지 않는다.
console.log(Object.keys(person)); // ["firstName"]

// [[Writable]]의 값이 false인 경우, 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없다.
// 에러는 발생하지 않고 무시된다.
// lastName 프로퍼티는 [[Writable]]의 값이 false이므로 값을 변경할 수 없다.
person.lastName = 'Kim';

// [[Configurable]]의 값이 false인 경우, 해당 프로퍼티를 삭제할 수 없다.
// 에러는 발생하지 않고 무시된다.
// lastName 프로퍼티는 [[Configurable]]의 값이 false이므로 삭제할 수 없다.
delete person.lastName;

// [[Configurable]]의 값이 false인 경우, 해당 프로퍼티를 재정의할 수 없다.
// Object.defineProperty(person, 'lastName', { enumerable: true });
// Uncaught TypeError: Cannot redefine property: lastName

descriptor = Object.getOwnPropertyDescriptor(person, 'lastName');
console.log('lastName', descriptor);
// lastName {value: "Lee", writable: false, enumerable: false, configurable: false}

// 접근자 프로퍼티 정의
Object.defineProperty(person, 'fullName', {
  // getter 함수
  get: function () { // === get() {
    return this.firstName + ' ' + this.lastName;
  },
  // setter 함수
  set: function (name) { // === set(name) {
    [this.firstName, this.lastName] = name.split(' ');
  },
  enumerable: true,
  configurable: true
});

descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log('fullName', descriptor);
// fullName {get: ƒ, set: ƒ, enumerable: true, configurable: true}

person.fullName = 'Heegun Lee';
console.log(person); // {firstName: "Heegun", lastName: "Lee"}
```

- Object.defineProperty 메소드로 프로퍼티를 정의할 때 프로퍼티 디스크립터 객체의 프로퍼티를 일부 누락할 수 있음
  - 프로퍼티 디스크립터 객체에서 누락된 어트리뷰터는 아래와 같이 기본값이 적용

| 프로퍼티 디스크립터 객체의 프로퍼티 | 대응하는 프로퍼티 어트리뷰트 | 디스크립터 객체의 프로퍼티 누락 시의 기본값 |
| :---------------------------------- | :--------------------------- | :------------------------------------------ |
| value                               | [[Value]]                    | undefined                                   |
| get                                 | [[Get]]                      | undefined                                   |
| set                                 | [[Set]]                      | undefined                                   |
| writable                            | [[Writable]]                 | false                                       |
| enumerable                          | [[Enumerable]]               | false                                       |
| configurable                        | [[Configurable]]             | false                                       |

- Object.defineProperty 메소드는 한 번에 하나의 프로퍼티만 정의할 수 있음
- Object.defineProperties 메소드를 사용하면 여러 개의 프로퍼티를 한 번에 정의할 수 있음

```javascript
const person = {};

Object.defineProperties(person, {
  // 데이터 프로퍼티 정의
  firstName: {
    value: 'Ungmo',
    writable: true,
    enumerable: true,
    configurable: true
  },
  lastName: {
    value: 'Lee',
    writable: true,
    enumerable: true,
    configurable: true
  },
  // 접근자 프로퍼티 정의
  fullName: {
    // getter 함수
    get: function () {
      return this.firstName + ' ' + this.lastName;
    },
    // setter 함수
    set: function (name) {
      [this.firstName, this.lastName] = name.split(' ');
    },
    enumerable: true,
    configurable: true
  }
});

person.fullName = 'Heegun Lee';
console.log(person); // {firstName: "Heegun", lastName: "Lee"}
```



모든 출처 : https://poiemaweb.com/