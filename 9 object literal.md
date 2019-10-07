# 9. 객체 리터럴

## 1. 객체란?

- 자바스크립트는 객체(object) 기반의 프로그래밍 언어이며 자바스크립트를 이루고 있는 거의 "모든 것"이 객체
- 원시 타입을 제외한 나머지 값들(함수, 배열, 정규표현식 등)은 모두 객체
- 원시 타입은 단 하나의 값
- 객체 타입(object / reference type)은 다양한 타입의 값(원시 타입의 값 또는 다른 객체)들을 하나의 단위로 구성한 복합적인 자료 구조(Data structure)
- 원시 타입의 값, 즉 원시 값은 변경 불가능한 값(immutable value)
- 객체 타입의 값, 즉 객체는 변경 가능한 값(mutable value)
- **자바스크립트의 객체**는 키(key)와 값(value)으로 구성된 프로퍼티(property)들의 집합

```javascript
var person = {
	name: 'Lee', // 프로퍼티
	gender: 'male' //프로퍼티
};
```

- name, gender : 프로퍼티 키
- 'Lee', 'male' : 프로퍼티 값
- 자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값이 될 수 있음
- 자바스크립트의 함수는 일급 객체이므로 값으로 취급할 수 있음
  - 따라서 프로퍼티 값으로 함수를 사용할 수도 있음
  - 프로퍼티 값이 함수일 경우, 일반함수와 구분하기 위해 메소드(Method)라 부름

```javascript
var counter = {
  num: 0, // 프로퍼티
  increase: function() { // 해당 코드 블록을 메소드라 함
    this.num++;
  }
};
```

- 객체는 프로퍼티와 메소드로 구성된 집합체
  - 프로퍼티 : 객체의 상태를 나타내는 값(data)
  - 메소드 : 프로퍼티를 참조하고 조작할 수 있는 동작(behavior)
- 객체는 객체의 상태를 나타내는 값(프로퍼티)과 프로퍼티를 참조하고 조작할 수 있는 동작(메소드)를 모두 포함할 수 있기 때문에 상태와 동작을 하나의 단위로 구조화할 수 있어 유용

------

**객체와 함수**

- 자바스크립트의 객체는 함수와 밀접한 관계
- 함수로 객체를 생성하기도 하며 함수 자체가 객체

------

## 2. 객체 리터럴에 의한 객체 생성

- C++이나 Java같은 클래스 기반 객체지향 언어는 클래스를 사전에 정의하고 필요한 시점에 new연산자와 함께 생성자(constructor)를 호출하여 인스턴스를 생성하는 방식으로 객체 생성
- 인스턴스(instance) : 클래스에 의해 생성되어 메모리에 저장된 실체
  - 객체지향 프로그래밍에서는 객체는 클래스와 인스턴스를 포함한 개념
  - 클래스는 인스턴스를 생성하기 위한 템플릿의 역할
  - 인스턴스는 객체가 메모리에 저장되어 실제로 존재하는 것에 초점을 맞춘 용어

- 자바스크립트는 프로토타입 기반 객체지향 언어로서 클래스 기반 객체지향 언어와는 다른 다양한 객체 생성 방법이 존재
  - 객체 리터럴
  - Object 생성자 함수
  - 생성자 함수
  - Object.reate 메소드
  - 클래스(ES6)

- 가장 일반적이고 간단한 방법 : 객체 리터럴
- 리터럴 표기법(Literal notation) : 값을 생성하는 가장 기본적인 방법으로 자바스크립트에서 사용할 수 있는 다양한 타입의 값(숫자, 문자, 불리언, null, undefined, 객체, 배열, 함수, 정규 표현식 등)을 생성할 수 있음

- 객체 리터럴은 중괄호 내에 0개 이상의 프로퍼티를 정의
  - 변수에 할당이 이루어지는 시점에 객체 리터럴은 해석되고 그 결과 객체 생성
  - 중괄호 내에 프로퍼티를 정의하지 않으면 빈 객체 생성

```javascript
var empty = {}; // 빈 객체
console.log(typeof empty); // object

// 할당이 이루어지는 시점에 객체 리터럴이 해석되고 그 결과 객체가 생성된다.
var person = {
  name: 'Lee',
  sayHello: function () {
    console.log(`Hello! My name is ${this.name}.`);
  }
};

console.log(typeof person); // object
console.log(person); // {name: "Lee", sayHello: ƒ}
```

- 객체 리터럴의 중괄호는 코드 블록을 의미하지 않음에 주의하자!
- 코드 블록의 닫는 중괄호 뒤에는 세미 콜론을 붙이지 않지만, 객체 리터럴은 표현식이므로 객체 리터럴의 닫는 중괄호 뒤에는 세미 콜론을 붙임
- 객체 리터럴은 자바스크립트의 유연함과 강력함을 대표하는 객체 생성 방식
  - 객체를 생성하기 위해 클래스를 먼저 정의하고 new 연산자와 함께 생성자를 호출할 필요가 없음
  - 숫자 값이나 문자열 값을 만드는 것과 유사하게 리터럴 표기법만으로 객체 생성
  - 객체 리터럴에 프로퍼티를 포함시켜 객체 생성과 동시에 프로퍼티를 만들 수도 있고, 객체 생성 이후 프로퍼티 동적 추가 가능
- 객체 리터럴 이외의 객체 생성 방식은 모두 함수를 사용해 객체를 생성(추후 배울 것)



## 3. 프로퍼티

- 객체는 프로퍼티(property)들의 집합이며 프로퍼티는 키(key)와 값(value)로 구성
- 프로퍼티를 나열할 때는 쉼표(,)로 구분
- 일반적으로 마지막 프로퍼티 뒤에는 쉼표를 사용하지 않으나 사용해도 됨

```javascript
var person = {
  // 프로퍼티 키는 name, 프로퍼티 값은 'Lee'
  name: 'Lee',
  // 프로퍼티 키는 gender, 프로퍼티 값은 'male'
  gender: 'male'
};
```

- 프로퍼티 키와 프로퍼티 값으로 사용할 수 있는 값
  - 프로퍼티 키 : 빈 문자열을 포함하는 모든 문자열 또는 symbol 값
  - 프로퍼티 값 : 자바스크립트에서 사용할 수 있는 모든 값
- 프로퍼티 키는 프로퍼티 값에 접근할 수 있는 이름으로 식별자 역할을 하지만 반드시 식별자 네이밍 규칙을 따라야 하는 것은 아님
  - 단, 식별자 네이밍 규칙을 준수하는 프로퍼티 키와 그렇지 않은 프로퍼티 키는 미묘한 차이가 존재
- symbol 값도 프로퍼티 키로 사용할 수 있지만 일반적으로 문자열(빈 문자열 포함)을 사용
  - 프로퍼티 키는 문자열이므로 따옴표로 묶어야 함
  - 식별자 네이밍 규칙을 준수하는 경우 따옴표 생략 가능
  - 식별자 네이밍 규칙을 따르지 않는 이름에는 반드시 따옴표를 사용하여야 함

```javascript
var person = {
  first_name: 'Ung-mo', // 유효한 이름
  'last-name': 'Lee'    // 유효하지 않은 이름
};

console.log(person); // {first_name: "Ung-mo", last-name: "Lee"}

// 자바스크립트 엔진은 last-name에서 -를 연산자가 있는 표현식으로 인식
var person = {
  first_name: 'Ung-mo',
  last-name: 'Lee'  // SyntaxError: Unexpected token -
};
```

- 문자열 또는 문자열로 변환 가능한 값을 반환하는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수 있음
  - 이 경우 프로퍼티 키로 사용할 표현식을 대괄호로 묶어야 함
  - 이를 계산된 프로퍼티 이름(Computed Property name)이라고 함

```javascript
var obj = {};
var key = 'hello';

// ES5: 프로퍼티 키 동적 생성
obj[key] = 'world';
// ES6: 프로퍼티 키 동적 생성
// var obj = { [key]: 'world' };

console.log(obj); // {hello: "world"}
```

- 빈 문자열을 프로퍼티 키로 사용해도 에러가 발생하지 않지만, 키로서의 의미를 갖지 못하므로 권장하지 않음

```javascript
var foo = {
  '': ''  // 빈문자열도 사용할 수 있다.
};

console.log(foo); // {"": ""}
```

- 프로퍼티 키에 문자열이나 symbol 값 이외의 값을 사용하면 암묵적 타입 변환을 통해 문자열이 됨
  - 프로퍼티 키로 숫자 리터럴을 사용하면 따옴표는 붙지 않지만 내부적으로는 문자열로 변환

```javascript
var foo = {
  0: 1,
  1: 2,
  2: 3
};

console.log(foo); // {0: 1, 1: 2, 2: 3}
```

- 예약어(Reserved word)를 프로퍼티 키로 사용해도 에러가 발생하지 않지만 예상치 못한 에러를 발생시킬 여지가 있으므로 권장하지 않음
- 이 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓰는데 에러가 발생하지 않는 것에 주의하자

```javascript
// 예약어를 프로퍼티 키로 사용
var foo = {
  var: '',
  function: ''
};

console.log(foo); // {var: "", function: ""}

// 프로퍼티 키 중복 선언
var foo = {
  name: 'Lee',
  name: 'Kim'
};

console.log(foo); // {name: "Kim"}
```



## 4. 메소드

- 자바스크립트에서 사용할 수 있는 모든 값을 프로퍼티 값으로 사용 가능
  - 자바스크립트의 함수는 객체(일급 객체, First-class object)
  - 함수는 값으로 취급할 수 있으며 프로퍼티의 값이 될 수 있음
- 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메소드(Method)라 부름
  - 메소드는 객체에 제한되어 있는 함수를 의미

```javascript
var circle = {
  radius: 5, // ← 프로퍼티

  // 원의 지름
  getDiameter: function () { // ← 메소드
    return 2 * this.radius; // this는 circle를 가리킨다.
  }
};

console.log(circle.getDiameter());  // 10
```

- this 키워드는 객체 자신(위 예제에서 circle 객체)을 가리키는 참조 변수



## 5. 프로퍼티 접근

- 마침표 표기법(Dot notation) : 프로퍼티 값에 접근하기 위해 마침표를 사용하는 기법
- 대괄호 표기법(Bracket notation) : 프로퍼티 값에 접근하기 위해 대괄호를 사용하는 표기법
- 마침표 또는 대괄호 좌측에는 객체로 평가할 수 있는 표현식을 기술, 마침표 우측 또는 대괄호 내부에는 프로퍼티 키를 지정

```javascript
var person = {
  name: 'Lee'
};

// 마침표 표기법에 의한 프로퍼티 접근
console.log(person.name); // Lee

// 대괄호 표기법에 의한 프로퍼티 접근
console.log(person['name']); // Lee
```

- 대괄호 표기법을 사용하는 경우, 대괄호 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야 함
  - 자바스크립트 엔진은 대괄호 내의 따옴표로 감싸지 않은 이름을 프로퍼티 키로 인식하지 않고 식별자로 취급
  - 객체에 존재하지 않는 프로퍼티에 접근하면 undefined 반환, 참조 에러(ReferenceError)가 발생하지 않는 것에 주의

```javascript
var person = {
  name: 'Lee'
};

console.log(person[name]); // ReferenceError: name is not defined

// 객체에 존재하지 않는 프로퍼티에 접근
var person = {
  name: 'Lee'
};

console.log(person.age); // undefined
```

- 프로퍼티 키가 식별자 네이밍 규칙을 준수하지 않는 이름, 즉 자바스크립트에서 사용가능한 이름이 아니면 반드시 대괄호 표기법을 사용해야 함
  - 단 ,프로퍼티 키가 숫자로 이루어진 문자열인 경우 따옴표 생략 가능

```javascript
var person = {
  'last-name': 'Lee',
  1: 10
};

console.log(person.'last-name');  // SyntaxError: Unexpected string
console.log(person.last-name);    // ReferenceError: name is not defined
console.log(person[last-name]);   // ReferenceError: last is not defined
console.log(person['last-name']); // Lee

// 프로퍼티 키가 숫자로 이루어진 문자열인 경우, 따옴표를 생략 가능하다.
console.log(person.1);     // SyntaxError: missing ) after argument list
console.log(person.'1');   // SyntaxError: Unexpected string
console.log(person[1]);    // 10 : person[1] -> person['1']
console.log(person['1']);  // 10
```

- console.log(person.last-name);의 결과가 ReferenceError:name is not defined 이유는?
  - last-name은 대괄호 안에 따옴표로 묶어주어야 함(내 답)
  - 자바스크립트 엔진은 먼저 person.last를 평가하는데 이 결과는 undefined
    - person 객체에는 프로퍼티 키가 last인 프로퍼티가 없으므로 undefined 반환
  - 다음으로 자바스크립트 엔진은 name이라는 식별자를 찾는데 이 때 name은 프로퍼티 키가 아니라 식별자인 것에 주의
    - 현재 어디에도 name이라는 식별자(변수, 함수 등의 이름) 선언이 없으므로 ReferenceError:name is not defined 에러 발생



## 6. 프로퍼티 값 갱신

- 이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신

```javascript
var person = {
  name: 'Lee'
};

// person객체에 name 프로퍼티가 존재하므로 name 프로퍼티의 값이 갱신된다.
person.name = 'Kim';

console.log(person);  // {name: "Kim"}
```



## 7. 프로퍼티 동적 생성

- 존하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당

```javascript
var person = {
  name: 'Lee'
};

// person객체에 address 프로퍼티가 존재하지 않는다.
// 따라서 person객체에 address 프로퍼티가 동적으로 생성되고 값이 할당된다.
person.address = 'Seoul';

console.log(person); // {name: "Lee", address: "Seoul"}
```



## 8. 프로퍼티 삭제

- delete 연산자는 객체의 프로퍼티를 삭제
- delete 연산자의 피연산자는 프로퍼티 값에 접근할 수 있는 표현식이어야 함
- 만약 존재하지 않는 프로퍼티를 삭제하면 아무런 에러없이 무시

```javascript
var person = {
  name: 'Lee'
};

// 프로퍼티 동적 추가
person.address = 'Seoul';

// person 객체에 address 프로퍼티가 존재한다.
// 따라서 delete 연산자로 address 프로퍼티를 삭제할 수 있다.
delete person.address;

// person 객체에 age 프로퍼티가 존재하지 않는다.
// 따라서 delete 연산자로 age 프로퍼티를 삭제할 수 없다. 이때 에러가 발생하지 않는다.
delete person.age;

console.log(person); // {name: "Lee"}
```



## 9. ES6에서 추가된 객체 리터럴의 확장 기능

### 9.1 프로퍼티 축약 표현

- 객체 리터럴의 프로퍼티는 프로퍼티 키와 프로퍼티 값으로 구성
  - 프로퍼티의 값은 변수에 할당된 값, 즉 식별자 표현식일 수 있음

```javascript
// ES5
var x = 1, y = 2;

var obj = {
  x: x,
  y: y
};

console.log(obj); // {x: 1, y: 2}
```

- ES6에서는 프로퍼티 값으로 변수를 사용하는 경우, 변수 이름과 프로퍼티 키가 동일한 이름일 때, 프로퍼티 키를 생략(Property shorthand)할 수 있음
  - 이 때 프로퍼티 키는 변수 이름으로 자동 생성

```javascript
// ES6
let x = 1, y = 2;

// 프로퍼티 축약 표현
const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```



### 9.2 프로퍼티 키 동적 생성

- 계산된 프로퍼티 이름(Computed property name) : 문자열 또는 문자열 변환 가능한 값을 반환하는 표현식을 이용해 프로퍼티 키를 동적으로 생성할 수 있음
  - 단, 프로퍼티 키로 사용할 표현식을 대괄호로 묶어야 함
- ES6에서 프로퍼티 키를 동적으로 생성하려면 객체 리터럴 외부에서 대괄호 표기법을 사용해야 함

```javascript
// ES5
var prefix = 'prop';
var i = 0;

var obj = {};

// 프로퍼티 키 동적 생성
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

- ES6에서는 객체 리터럴 내부에서도 프로퍼티 키를 동적으로 생성 가능

```javascript
// ES6
const prefix = 'prop';
let i = 0;

// 객체 리터럴 내부에서 프로퍼티 키 동적 생성
var obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i
};

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```



### 9.3 메소드 축약 표현

- ES5에서 메소드를 정의하려면 프로퍼티 값으로 함수를 할당

```javascript
// ES5
var obj = {
  name: 'Lee',
  sayHi: function() {
    console.log('Hi! ' + this.name);
  }
};

obj.sayHi(); // Hi! Lee
```

- ES6에서는 메소드를 정의할 때, function 키워드를 생략한 축약 표현을 사용할 수 있음

```javascript
// ES6
const obj = {
  name: 'Lee',
  // 메소드 축약 표현
  sayHi() {
    console.log('Hi! ' + this.name);
  }
};

obj.sayHi(); // Hi! Lee
```

- ES6의 메소드 축약 표현으로 정의한 메소드는 프로퍼티에 할당한 함수와 다르게 동작(26.2 메소드에서 살펴보기로 함)



모든 출처 : https://poiemaweb.com/