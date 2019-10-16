# 18. 프로토타입

- 자바스크립트는 명령형(Imperative), 함수형(Functional), 프로토타입 기반(Prototype_based) 객체지향 프로그래밍(OOP, Object Oriented Programming)을 지원하는 멀티 패러다임 프로그래밍 언어
- 자바스크립트는 클래스와 상속, 캡슐화를 위한 키워드 public, private, protected 등이 없어서 객체지향 언어가 아니라고 오해받는 경우도 있지만 클래스 기반 객체지향 프로그래밍 언어보다 효율적이며 더 강력한 객체지향 프로그래밍 능력을 지니고 있는 **프로토타입 기반의 객체지향** 프로그래밍 언어

- 클래스(class) : ES6에서 새롭게 도입
  - ES6의 클래스가 기존의 프로토타입 기반 객체지향 모델을 폐지하고 새로운 객체지향 모델을 제공하는 것은 아님
  - 하지만 클래스와 생성자 함수 모두 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작하지는 않음
    - 클래스는 생성자 함수보다 엄격하며 클래스 생성자 함수에는 제공하지 않는 기능도 제공

- 자바스크립트는 객체 기반의 프로그래밍 언어이며 자바스크립트를 이루고 있는 거의 모든것이 객체(원시타입을 제외한 값들이 모두)



## 1. 객체지향 프로그래밍

- 객체지향 프로그래밍(Object Oriented Programming, OOP) : 프로그램을 명령어 또는 함수의 목록으로 보는 전통적인 명령형 프로그래밍의 절차지향적 관점에서 벗어나 프로그램을 여러 개의 독립적 단위, 즉 객체(object)들의 집합으로 표현하려는 프로그래밍 패러다임
  - 객체지향 프로그래밍은 사람이 주변의 실세계에서 사물이나 개념을 이해하는 방식을 프로그래밍에 접목하려는 사상에서 시작

- 사람은 어떤 사물이나 개념을 이해할 때 속성(attribute, property), 즉 특성이나 성질을 통해 이해하는 경향
  - 즉, 속상을 통해 어떠한 사물이나 개념을 구체화
  - 예) 사람은 이름, 주소, 성별 나이 등 다양한 속성을 갖고 있으며 A라는 사람을 설명할 때 속성을 나열하여 설명할 수 있음
  - **추상화(abstraction)** : 다양한 속성 중에서 프로그램에 필요한 속성만을 간추려 내어 표현하는 것

```javascript
// 이름과 주소라는 속성을 갖는 객체
const person = {
  name: 'Lee',
  address: 'Seoul'
};

console.log(person); // {name: "Lee", address: "Seoul"}
```

- 원(Circle)이라는 개념을 객체로 만들어 보면
  - 원에는 반지름이라는 속성이 있음
  - 이를 가지고 원의 지름, 둘레, 넓이를 구할 수 있음
  - 이 때 반지름 : **원의 상태(state)**
  - 원의 지름, 둘레, 넓이를 구하는 것은 **동작(behavior)**

```javascript
const circle = {
  radius: 5, // 반지름

  // 원의 지름: 2r
  getDiameter() {
    return 2 * this.radius;
  },

  // 원의 둘레: 2πr
  getPerimeter() {
    return 2 * Math.PI * this.radius;
  },

  // 원의 넓이: πrr
  getArea() {
    return Math.PI * Math.pow(this.radius, 2);
  }
};

console.log(circle);
// {radius: 5, getDiameter: ƒ, getPerimeter: ƒ, getArea: ƒ}

console.log(circle.getDiameter());  // 10
console.log(circle.getPerimeter()); // 31.41592653589793
console.log(circle.getArea());      // 78.53981633974483
```

- 객체지향 프로그래밍은 객체의 상태(state)를 나타내는 데이터와 상태 데이터를 조작할 수 있는 동작(behavior)을 하나의 논리적인 단위로 묶어 생각
- 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료 구조를 객체(object)라 함

- 객체의 상태 데이터를 프로퍼티(property), 동작을 메소드(method)라 부름

- 객체는 각각 고유의 기능을 갖는 독립적인 부품으로 볼 수 있지만 자신의 고유한 기능을 수행하면서 다른 객체와 관계성(relationship)을 갖을 수 있음
  - 다른 객체와 메시지를 주고 받거나 데이터를 처리할 수도 있음
  - 또는 다른 객체의 상태 데이터나 동작을 상속받아 사용하기도 함



## 2. 상속과 프로토타입

- 상속(Inheritance) : 객체지향 프로그래밍의 핵심 개념으로 어떤 객체의 프로퍼티 또는 메소드를 다른 객체가 상속받아 그대로 사용할 수 있는 것
  - 프로토타입 기반으로 상속을 구현하여 불필요한 중복 제거
  - 중복을 제거하는 방법은 기존의 코드 적극 재사용
  - 코드 재사용은 개발 비용을 현저히 줄일 수 있는 잠재력이 있으므로 매우 중요

```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    // Math.PI는 원주율을 나타내는 상수이다.
    // Math.pow는 첫번째 인수를 두번째 인수로 거듭제곱한 값을 반환한다.
    return Math.PI * Math.pow(this.radius, 2);
  };
}

// 인스턴스 생성
// 반지름이 1인 인스턴스 생성
const circle1 = new Circle(1);
// 반지름이 2인 인스턴스 생성
const circle2 = new Circle(2);

// Circle 생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는
// getArea 메소드를 중복 생성하고 모든 인스턴스가 중복 소유한다.
// 따라서 getArea 메소드는 하나만 생성하여 모든 인스턴스가 공유하는 것이 바람직하다.
console.log(circle1.getArea === circle2.getArea); // false

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

- 생성자 함수는 동일한 프로퍼티(메소드 포함) 구조를 갖는 객체를 여러개 생성할 때 유용
  - 하지만 위 생성자 함수는 문제 존재
    - Circle 생성자 함수가 생성하는 모든 객체(인스턴스)는 radius 프로퍼티와 getArea 메소드를 가짐
    - radius 프로퍼티 값은 일반적으로 인스턴스마다 다름(같은 상태를 갖는 여러 개의 인스턴스가 필요하다면 radius 프로퍼티 값이 같을 수도 있음)
    - 하지만 getArea 메소드는 모든 인스턴스가 동일한 내용의 메소드를 사용하므로 하나만 생성하여 모든 인스턴스가 공유하는 것이 바람직
    - 그런데 Circle 생성자 함수는 인스턴스를 생성할 때마다 getArea 메소드를 중복 생성하고 모든 인스턴스가 중복 소유

![](https://poiemaweb.com/assets/fs-images/18-1.png)

- 이처럼 동일한 생성자 함수에 의해 생성된 모든 인스턴스가 동일한 메소드를 중복 소유하는 것은 메모리를 불필요하게 낭비
  - 인스턴스를 생성할 때마다 메소드를 생성하므로 퍼포먼스에도 악영향(만약 10개 인스턴스 생성시 동일 메소드 10개 생성)
- 상속을 통해 불필요한 중복을 제거해보자
  - 자바스크립트는 프로토타입 기반으로 상속 구현

```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 공유할 수 있도록 getArea 메소드를 프로토타입에 추가한다.
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
Circle.prototype.getArea = function () {
  return Math.PI * Math.pow(this.radius, 2);
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
// 프로토타입 Circle.prototype로부터 getArea 메소드를 상속받는다.
// 즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메소드를 공유한다.
console.log(circle1.getArea === circle2.getArea); // true

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

![](https://poiemaweb.com/assets/fs-images/18-2.png)

- Circle 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입, 즉 상위(부모) 객체 역할을 하는 Circle.prototype의 모든 프토퍼티와 메소드를 상속 받음
- getArea 메소드는 단 하나만 생성되어 프로토타입인 Circle.prototype의 메소드로 할당
  - 따라서 Circle 생성자 함수가 생성하는 모든 인스턴스는 getArea 메소드를 상속받아 사용할 수 있음
  - 즉, 자신의 상태를 나타내는 radius 프로퍼티만을 개별적으로 소유하고 내용이 동일한 메소드는 상속을 통해 공유해 사용

- 상속은 코드의 재사용이란 관점에서 매우 유용
  - 생성자 함수가 생성할 모든 인스턴스가 공통적으로 사용할 프로퍼티나 메소드를 프로토타입에 미리 구현해놓으면 생성자 함수가 생성할 모든 인스턴스는 별도의 구현없이 상위(부모) 객체인 프로토타입의 자산을 공유하여 사용할 수 있음



## 3. 프로토타입 객체

- 프로토타입 객체(줄여서 프로토타입) : 객체 지향 프로그래밍의 근간을 이루는 객체간 상속(inheritance)을 구현하기 위해 사용
- 프로토타입은 어떤 객체의 상위 객체의 역할을 하는 객체로 다른 객체에 공유 프로퍼티(메소드 포함)를 제공
  - 프로토타입을 상속받은 하위객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용할 수 있음
- 모든 객체는 [[Prototype]] 이라는 내부 슬롯을 가짐
  - 모든 객체는 생성될 때 [[Prototype]] 내부 슬롯의 값으로 프로토타입의 참조를 저장
- 즉, 모든 객체는 하나의 프로토타입을 가지며 프로토타입은 객체의 생성 방식에 의해 결정
  - 객체 리터럴에 의해 생성된 객체의 프로토타입 : Object.prototype
  - 생성자 함수에 의해 생성된 객체의 프로토타입 : 생성자 함수의 prototype 프로퍼티에 바인딩 되어있는 객체
- 모든 객체는 하나의 프로토타입을 가짐
  - 프로토타입은 null이거나 객체
  - 모든 프로토타입은 생성자 함수와 연결되어 있음
  - 즉, 객체와 프로토타입과 생성자 함수는 서로 연결되어 있음

![](https://poiemaweb.com/assets/fs-images/18-3.png)

- 위 그림처럼 객체는 `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 자신의 [[Prototype]] 내부 슬롯이 가리키는 객체에 접근할 수 있음
  - 프로토타입은 constructor 프로퍼티를 통해 생성자 함수에 접근할 수 있음
  - 생성자 함수는 prototype 프로퍼티를 통해 프로토타입에 접근 가능



### 3.1 `__proto__` 접근자 프로퍼티

- 모든 객체는 `__proto__` 접근자 프로퍼티를 통해 자신의 프로토 타입, 즉 [[Prototype]] 내부 슬롯에 접근 가능

```javascript
const person = { name: 'Lee' };
```

![](https://poiemaweb.com/assets/fs-images/18-4.png)

- 위 그림의 빨간 박스로 표시한 것이 person 객체의 프로토타입인 Object.prototype
- 이는 `__proto__` 접근자 프로퍼티를 통해 person 객체의 [[Prototype]] 내부 슬롯이 가리키는 객체인 Object.prototype에 접근한 결과를 크롬 브라우저가 콘솔에 표시한 것
- 모든 객체는 프로토타입을 가리키는 [[Prototype]] 내부 슬롯에 접근하기 위해 `__proto__` 접근자 프로퍼티를 사용할 수 있음

#### `__proto__` 는 접근자 프로퍼티다

- 내부 슬롯은 프로퍼티가 아니므로 내부 슬롯에는 직접 접근할 수 없고 간접적인 방법을 제공하는 경우에 한해 접근 가능
  - [[Prototype]] 내부 슬롯에도 직접 접근할 수 없으며 `__proto__`접근자 프로퍼티를 통해 간접적으로 [[Prototype]] 내부 슬롯의 값, 즉 프로토타입에 접근 가능
- 접근자 프로퍼티는 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티

![](https://poiemaweb.com/assets/fs-images/18-5.png)

- Object.prototype의 프로퍼티인 `__proto__`접근자 프로퍼티는 getter/setter 함수라고 부르는 접근자 함수를 통해 [[Prototype]] 내부 슬롯의 값, 즉 프로토타입을 취득하거나 할당
  - `__proto__`접근자 프로퍼티를 통해 프로토타입에 접근하면 내부적으로 `__proto__`접근자 프로퍼티의 getter 함수인 `get__proto__`가 호출
  - `__proto__`접근자 프로퍼티를 통해 새로운 프로토타입을 할당하면 `__proto__`접근자 프로퍼티의 setter 함수인 `set__proto__`가 호출

```javascript
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__가 호출되어 obj 객체의 프로토타입을 취득
obj.__proto__;
// setter함수인 set __proto__가 호출되어 obj 객체의 프로토타입을 교체
obj.__proto__ = parent;

console.log(obj.x); // 1
```

- 내부 메소드 [[GetPrototypeOf]]와 [[SetPrototypeOf]]
  - `get__proto__`은 [[GetPrototypeOf]] 내부 메소드를 호출하여 자신의 프로토타입을 취득하고, `set__proto__`은 [[SetPrototypeOf]] 내부 메소드를 호출하여 새로운 프로토타입을 할당

![](https://poiemaweb.com/assets/fs-images/18-6.png)

#### `__proto__`접근자 프로퍼티는 상속을 통해 사용된다

- `__proto__`접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype의 프로퍼티
  - 모든 객체는 상속을 통해 `Object.prototype.__proto__`접근자 프로퍼티를 사용할 수 있음

```javascript
const person = { name: 'Lee' };

// person 객체는 __proto__ 프로퍼티를 소유하지 않는다.
console.log(person.hasOwnProperty('__proto__')); // false

// __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티이다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있다.
console.log({}.__proto__ === Object.prototype); // true
```

#### `__proto__`접근자 프로퍼티를 통해 프로토타입에 접근하는 이유

- [[Prototype]] 내부 슬롯의 값, 즉 프로토타입에 접근하기 위해 접근자 프로퍼티를 사용하는 이유는 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위함

```javascript
const parent = {};
const child = {};

// child의 프로토타입을 parent로 지정
child.__proto__ = parent;
// parent의 프로토타입을 child로 설정
parent.__proto__ = child; // TypeError: Cyclic __proto__ value
```

- 위 예제는 parent 객체를 child객체의 프로토타입으로 지정한 후, child 객체를 parent 객체의 프로토타입으로 지정하였음
  - 이러한 코드가 에러없이 정상적으로 처리되면 서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인이 만들어지므로 `__proto__`접근자 프로퍼티는 에러를 발생시킴

![](https://poiemaweb.com/assets/fs-images/18-8.png)

- 프로토타입 체인은 단방향 링크드 리스트로 구현되어야 함
  - 즉, 프로퍼티 검색 방향이 한쪽 방향으로만 흘러가야 함
  - 하지만 위 그림처럼 순환참조(circular reference)적인 프로토타입 체인이 만들어지면 프로토타입 체인 종점이 존재하지 않으므로 프토토타입 체인에서 프로퍼티를 검색할 때 무한 루프에 빠짐
  - 따라서 아무런 체크없이 무조건적으로 프로토타입을 교체할 수 없도록 `__proto__`접근자 프로퍼티를 통해 프로토타입에 접근하고 교체하도록 구현되어 있음

#### `__proto__`접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 비추천이다

- `__proto__`접근자 프로퍼티는 ES5까지 ECMAScript 사양에 포함되지 않은 비표준이었으나 일부 브라우저에서 지원하고 있었기 떄문에 ES6에서 브라우저 호환성을 고려하여 표준으로 채택
  - 현재 대부분의 브라우저가 지원
- 하지만, 코드 내에서 `__proto__`를 직접 사용하는 것은 추천하지 않음
  - 모든 객체가 `__proto__`접근자 프로퍼티를 사용할 수 있는 것은 아님
  - 직접 상속을 통해 아래와 같이 Object.prototype을 상속받지 않는 객체를 생성할 수도 있음

```javascript
// obj는 프로토타입 체인의 종점이다. 따라서 Object.__proto__를 상속받을 수 없다.
const obj = Object.create(null);

// obj는 Object.__proto__를 상속받을 수 없다.
console.log(obj.__proto__); // undefined
// 따라서 Object.getPrototypeOf 메소드를 사용하는 편이 좋다.
console.log(Object.getPrototypeOf(obj)); // null
```

- 따라서 `__proto__`접근자 프로퍼티 대신 프로토타입의 참조를 취득할 경우는 Object.getPrototypeOf메소드를, 프로토타입을 교체하는 경우는 Object.setPrototypeOf메소드를 사용하는 것을 권장

```javascript
const obj = {};
const parent = { x: 1 };

// obj 객체의 프로토타입을 취득
Object.getPrototypeOf(obj); // obj.__proto__;
// obj 객체의 프로토타입을 교체
Object.setPrototypeOf(obj, parent); // obj.__proto__ = parent;

console.log(obj.x); // 1
```

- Object.getPrototypeOf 메소드와 Object.setPrototypeOf 메소드는 `get Object.prototype.__proto__`와 `set Object.prototype.__proto__`의 처리 내용과 정확히 일치

![](https://poiemaweb.com/assets/fs-images/18-7.png)

- Object.getPrototypeOf 메소드는 ES5에서 도입된 메소드이며 IE9 이상을 지원한다. Object.setPrototypeOf 메소드는 ES6에서 도입된 메소드이며 IE11 이상을 지원



### 3.2 함수 객체의 prototype 프로퍼티

- 함수 객체는 `__proto__`접근자 프로퍼티 외 prototype 프로퍼티도 소유
  - 함수 객체의 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토 타입을 가리킴
- prototype 프로퍼티는 함수 객체만이 소유하는 프로퍼티로 일반 객체에는 없음

```javascript
// 함수 객체는 prototype 프로퍼티를 소유한다.
console.log((function () {}).hasOwnProperty('prototype')); // true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
console.log({}.hasOwnProperty('prototype')); // false
```

- prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로서 사용될 때, 생성자 함수가 생성할 객체(인스턴스)의 프로토타입을 가리킴
  - 따라서 생성자 함수로서 호출할 수 없는 함수, 즉 함수의 종류가 Arrow, Method인 함수인 non-constructor는 프로토타입이 생성되지 않으며 prototype 프로퍼티도 소유하지 않음

```javascript
// 화살표 함수는 non-constructor이다.
const Person = name => {
  this.name = name;
};

// non-constructor는 프로토타입이 생성되지 않는다.
console.log(Person.prototype); // undefined

// non-constructor는 prototype 프로퍼티도 소유하지 않는다.
console.log(Person.hasOwnProperty('prototype')); // false

// ES6의 메소드 축약 표현으로 정의한 메소드는 non-constructor이다.
const obj = {
  foo() {}
};

// non-constructor는 프로토타입이 생성되지 않는다.
console.log(obj.foo.prototype); // undefined

// non-constructor는 prototype 프로퍼티도 소유하지 않는다.
console.log(obj.foo.hasOwnProperty('prototype')); // false
```

- 생성자 함수가 아닌 일반 함수도 prototype 프로퍼티를 소유하지만 객체를 생성하지 않는 일반 함수의 prototype 프로퍼티는 아무런 의미가 없음
- 모든 객체가 가지고 있는(엄밀히 말하면 Object.prototype로부터 상속받은) `__proto__ `접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 **동일한 프로토타입을 가리킴**

| 구분                        | 소유      | 값                | 사용 주체   | 사용 목적                                                    |
| :-------------------------- | :-------- | :---------------- | :---------- | :----------------------------------------------------------- |
| `__proto__` 접근자 프로퍼티 | 모든 객체 | 프로토타입의 참조 | 모든 객체   | 모든 객체가 상속을 위해 자신의 프로토타입에 접근하기 위해 사용 |
| prototype 프로퍼티          | 함수 객체 | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 객체(인스턴스)의 프로토타입을 할당하기 위해 사용 |

- 생성자 함수로 객체를 생성한 후 `__proto__` 접근자 프로퍼티와 prototype 프로퍼티로 프로토타입 객체에 접근해보자

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// Person.prototype: Person 생성자 함수는 prototype 프로퍼티를 통해
// 자신이 생성할 인스턴스(이 경우에는 me)의 프로토타입을 할당
// me.__proto__: 객체 me의 __proto__ 접근자 프로퍼티를 통해 자신의 프로토타입에 접근
// 결국 Person.prototype와 me.__proto__는 결국 동일한 프로토타입을 가리킨다.
console.log(Person.prototype === me.__proto__);  // true
```



### 3.3 프로토타입의 constructor 프로퍼티와 생성자 함수

- 모든 프로토타입은 constructor 프로퍼티를 가짐
  - 이 constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킴
  - 즉 함수 객체가 생성될 때 이루어짐

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// me 객체의 생성자 함수는 Person이다.
console.log(me.constructor === Person);  // true
```

![](https://poiemaweb.com/assets/fs-images/18-10.png)

- 위 예제에서 Person 생성자 함수는 me 객체를 생성
  - 이때 me 객체는 프로토타입의 contructor 프로퍼티를 통해 생성자 함수와 연결
  - me 객체에는 constructor 프로퍼티가 없지만 me 객체의 프로토타입인 Person.prototye에 constructor 프로퍼티가 있음
  - me 객체는 프로토타입인 Person.prototye에 constructor 프로퍼티를 상속받아 사용할 수 있음



## 4. 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

- 생성자 함수에 의해 생성된 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 생성자 함수와 연결
  - 이때 생성자 함수는 인스턴스를 생성한 생성자 함수

```javascript
// obj 객체를 생성한 생성자 함수는 Object이다.
const obj = new Object();

// add 함수 객체를 생성한 생성자 함수는 Function이다.
const add = new Function('a', 'b', 'return a + b');

// 생성자 함수
function Person(name) {
  this.name = name;
}
// me 객체를 생성한 생성자 함수는 Person이다.
const me = new Person('Lee');
```

- 하지만 리터럴 표기법에 의한 객체 생성 방식과 같이, 명시적으로 new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하지 않는 객체 생성 방식도 존재

```javascript
// 객체 리터럴
const obj = {};

// 함수 리터럴
const add = function (a, b) { return a + b; };

// 배열 리터럴
const arr = [1, 2, 3];

// 정규표현식 리터럴
const regexr = /is/ig;
```

- 위 예제의 객체 obj는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴에 의해 생성된 객체
  - 하지만 객체 obj는 Object 생성자 함수와 constructor 프로퍼티로 연결
  - 렇다면 객체 리터럴에 의해 생성된 객체는 사실 Object 생성자 함수로 생성되는 것은 아닐까?
    - Object 생성자 함수는 new 연산자와 함께 호출하지 않아도 new 연산자와 함께 호출한 것과 동일하게 동작
    - 그리고 인수가 전달되지 않았을 때 추상 연산 ObjectCreate을 호출하여 빈 객체를 생성한다. 인수가 전달된 경우에는 인수를 객체로 변환

```javascript
// Object 생성자 함수에 의한 객체 생성
let obj = new Object();
console.log(obj); // {}

// Object 생성자 함수는 new 연산자와 함께 호출하지 않아도 new 연산자와 함께 호출한 것과 동일하게 동작한다.
// 인수가 전달되지 않았을 때 추상 연산 ObjectCreate을 호출하여 빈 객체를 생성한다.
obj = Object();
console.log(obj); // {}

// 인수가 전달된 경우에는 인수를 객체로 변환한다.
// Number 객체 생성
obj = new Object(123);
console.log(obj); // Number {123}

// String  객체 생성
obj = new Object('123');
console.log(obj); // String {"123"}
```

- 객체 리터럴이 평가될 때는 아래와 같이 추상 연산 ObjectCreate을 호출하여 빈객체를 생성하고 프로퍼티를 추가하도록 정의

> 객체 리터럴의 평가

![](https://poiemaweb.com/assets/fs-images/18-12.png)

- 이처럼 Object 생성자 함수와 객체 리터럴의 평가는 추상 연산 ObjectCreate을 호출하여 빈 객체를 생성하는 것은 동일하나 new.tartget 확인이나 프로퍼티를 추가하는 처리 등 세부 내용은 다름
  - 따라서 객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체가 아님
- 함수 객체의 경우, 더욱 차이가 명확
  -  “11.4.4 Function 생성자 함수”에서 살펴보았듯이 Function 생성자 함수 방식으로 생성한 함수는 렉시컬 스코프를 만들지 않고 전역 함수인 것처럼 스코프를 생성하며 클로저도 만들지 않음
  - 따라서 함수 선언문과 함수 표현식을 평가하여 함수 객체를 생성한 것은 Function 생성자 함수가 아니지만, 하지만 함수 foo의 의 생성자 함수는 Function 생성자 함수

```javascript
// 함수 객체 foo는 Function 생성자 함수로 생성한 함수 객체가 아니라 함수 선언문으로 생성하였다.
function foo() {}

// 하지만 함수 객체 foo의 생성자 함수는 Function 생성자 함수이다.
console.log(foo.constructor === Function); // true
```

- 리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요
  - 따라서 리터럴 표기법에 의해 생성된 객체도 가상적인 생성자 함수를 가짐
  - 프로토타입은 생성자 함수와 더불어 생성되며  prototype, constructor 프로퍼터에 의해 연결되어 있기 때문
  - **프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍(pair)으로 존재하기 때문**

- 리터럴 표기법(객체 리터럴, 함수 리터럴, 배열 리터럴, 정규 표현식 리터럴 등)에 의해 생성된 객체는 생성자 함수에 의해 생성된 객체는 아니지만 큰 틀에서 생각해 보면 리터럴 표기법으로 생성한 객체도 생성자 함수로 생성한 객체와 본질적인 면에서 큰 차이는 없음
  - 예를 들어, 객체 리터럴에 의해 생성한 객체와 Object 생성자 함수에 의해 생성한 객체는 생성 과정에 차이는 있지만 결국 객체로서 동일한 특성을 가짐
  - 함수 리터럴에 의해 생성한 함수와 Function 생성자 함수에 의해 생성한 함수는 생성 과정과 스코프, 클로저 등의 차이가 있지만 결국 함수로서 동일한 특성을 가짐

- 따라서 프로토타입의 constructor 프로퍼티로 연결되어 있는 생성자 함수를 리터럴 표기법으로 생성한 객체를 생성한 생성자 함수로 생각해도 크게 무리는 없음
  - 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입은 아래와 같음

| 리터럴 표기법      | 생성자 함수 | 프로토타입         |
| :----------------- | :---------- | :----------------- |
| 객체 리터럴        | Object      | Object.protptype   |
| 함수 리터럴        | Function    | Function.prototype |
| 배열 리터럴        | Array       | Array.prototype    |
| 정규 표현식 리터럴 | RegExp      | RegExp.protptype   |



## 5. 프로토타입의 생성 시점

- 리터럴 표기법에 의해 생성된 객체도 생성자 함수와 연결
  - 객체는 리터럴 표기법 또는 생성자 함수에 의해 생성되므로 결국 모든 객체는 생성자 함수와 연결되어 있음

------

**Object.create 메소드와 클래스에 의한 객체 생성**

- 아직 살펴보지 않았지만 Object.create 메소드와 클래스로 객체를 생성하는 방법도 있음.
  - Object.create 메소드와 클래스로 생성한 객체도 생성자 함수와 연결되어 있음

------

- 생성자 함수는 사용자가 직접 정의한 사용자 정의 생성자 함수와 자바스크립트가 기본 제공하는 빌트인 생성자 함수로 구분할 수 있음
  - 프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성



### 5.1 사용자 정의 생성자 함수로 프로토타입 생성 시점

- 내부 메소드 [[Construct]]를 갖는 함수 객체, 즉 화살표 함수나 ES6의 메소드 축약 표현으로 정의하지 않고 일반 함수(함수 선언문, 함수 표현식)로 정의한 함수 객체는 new 연산자와 함께 생성자 함수로서 호출
  - 생성자 함수로서 호출할 수 있는 함수, 즉 **constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성**

```javascript
// 함수 정의(constructor)가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.
console.log(Person.prototype); // {constructor: ƒ}

// 생성자 함수
function Person(name) {
  this.name = name;
}

생성자 함수로서 호출할 수 없는 함수, 즉 non-constructor는 프로토타입이 생성되지 않는다.
// 화살표 함수는 non-constructor이다.
const Person = name => {
  this.name = name;
};

// non-constructor는 프로토타입이 생성되지 않는다.
console.log(Person.prototype); // undefined
```

- 함수 선언문은 다른 코드가 실행되기 이전에 자바스크립트 엔진에 의해 먼저 실행
  - 따라서 함수 선언문으로 정의된 Person 생성자 함수는 어떤 코드보다 먼저 평가되어 함수 객체가 됨
  - 이때 프로토타입도 더불어 생성
  - 생성된 프로토타입은 Person 생성자 함수의 prototype 프로퍼티에 바인딩
  - Person 생성자 함수와 더불어 생성된 프로토타입의 내부를 살펴보자

![](https://poiemaweb.com/assets/fs-images/18-13.png)

- 생성된 프로토타입은 constructor 프로퍼티만을 갖는 객체
  - 프로토타입도 객체이고 모든 객체는 프로토타입을 갖으므로 프로토타입도 자신의 프로토타입을 가짐
  - 생성된 프로토타입의 프로토타입은 Object.prototype

![](https://poiemaweb.com/assets/fs-images/18-14.png)

- 이처럼 빌트인 생성자 함수가 아닌 사용자 정의 생성자 함수는 자신이 평가되어 함수 객체로 생성되는 시점에 프로토타입도 더불어 생성되며 생성된 프로토타입의 프로토타입은 언제나 Object.prototype



### 5.2 빌트인 생성자 함수와 프로토타입 생성 시점

- Object, String, Number, Function, Array, RegExp, Date, Promise 등과 같은 빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성
  - 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성
  - 전역 객체는 누구보다도 먼저 생성
  - 이때 빌트인 생성자 함수와 더불어 프로토타입이 생성
  - 생성된 프로토타입은 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩

![](https://poiemaweb.com/assets/fs-images/18-15.png)

- 전역 객체(Global Object)
  - 전역 객체는 모든 객체의 유일한 최상위 객체를 의미하며 일반적으로 클라이언트 사이드 환경(브라우저)에서는 window, 서버 사이드 환경(Node.js)에서는 global 객체를 의미
  - 전역 객체는 Object, String, Number, Function, Array, RegExp, Date, Math와 같은 표준 빌트인 객체(standard built-in object)를 프로퍼티로 가짐
  - Math를 제외한 표준 빌트인 객체는 생성자 함수 객체

```javascript
// window 객체는 브라우저에 종속적이므로 아래 코드는 브라우저에서 실행해야 한다.
// 빌트인 객체인 Object는 전역 객체 window의 프로퍼티이다.
window.Object === Object // true
```

- 전역 객체는 누구보다도 먼저 생성
  - 표준 빌트인 객체인 Object도 전역 객체의 프로퍼티이며 전역 객체가 생성되는 시점에 같이 생성
  - 정확히 말하자면 전역 객체는 다른 빌트인 객체를 포함하는 객체이므로 다른 빌트인 객체가 생성되기 이전에 먼저 생성되어야 함
  - 전역 객체가 생성된 이후, 빌트인 객체가 생성되어 프로퍼티로 추가

- 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화되어 존재하고 있음
  - 이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 [[prototype]] 내부 슬롯에 할당되고, 이로써 생성된 객체는 프로토타입을 상속받음



## 6. 객체 생성 방식과 프로토타입의 결정

- 객체 생성 방법은 아래와 같이 다양함
  - 객체 리터럴
  - Object 생성자 함수
  - 생성자 함수
  - Object.create 메소드
  - 클래스 (ES6)
- 다양한 방식으로 생성된 모든 객체는 각각의 방식마다 세부적인 객체 생성 방식의 차이는 있으나 추상 연산 ObjectCreate에 의해 생성된다는 공통점

![](https://poiemaweb.com/assets/fs-images/18-16.png)

- 런타임에 새로운 객체를 생성하는 추상 연산 ObjectCreate는 필수적으로 자신이 생성할 객체의 프로토타입(proto)을 인수로 전달받음
  - 그리고 자신이 생성할 객체에 추가할 프로퍼티 목록(internalSlotList)은 옵션으로 전달할 수 있음

- 추상 연산 ObjectCreate는 빈객체를 생성한 후, 객체에 추가할 프로퍼티 목록(internalSlotList)이 인수로 전달된 경우, 프로퍼티를 객체에 추가
  - 그리고 인수로 전달받은 프로토타입을 자신이 생성한 객체의 [[Prototype]] 내부 슬롯에 할당(4. Set obj.[[Prototype]] to proto)한 다음, 생성한 객체를 반환
- 즉, 프로토타입은 추상 연산 ObjectCreate에 전달되는 인수(proto)에 의해 결정
  - 이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정



### 6.1 객체 리터럴에 의해 생성된 객체의 프로토타입

- 자바스크립트 엔진은 객체 리터럴을 평가하여 객체를 생성할 때, 추상 연산 ObjectCreate를 호출

![](https://poiemaweb.com/assets/fs-images/18-17.png)

- 이때 추상 연산 ObjectCreate에 전달되는 프로토타입은 Object.prototype
  - 즉, 객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype

```javascript
const obj = { x: 1 };
```

- 위 객체 리터럴이 평가되면 추상 연산 ObjectCreate에 의해 아래와 같이 Object 생성자 함수와 Object.prototype과 생성된 객체 사이에 연결이 만들어 짐

> 객체 리터럴에 의해 생성된 객체의 프로토타입

![](https://poiemaweb.com/assets/fs-images/18-18.png)

- 이처럼 객체 obj는 Object.prototype을 프로토타입으로 갖게 되며 이로써 Object.prototype을 상속
  - obj 객체는 constructor 프로퍼티와 hasOwnProperty 메소드 등을 소유하지 않지만 자신의 프로토타입인 Object.prototype의 constructor 프로퍼티와 hasOwnProperty 메소드를 자신의 자산인 것처럼 자유롭게 사용할 수 있음
  - 이는 obj 객체가 자신의 프로토타입인 Object.prototype 객체를 상속받았기 때문임

```javascript
const obj = { x: 1 };

// 객체 obj는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x'));    // true
```



### 6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입

- 명시적으로 Object 생성자 함수를 호출하여 객체를 생성하면 빈 객체가 생성됨
  - Object 생성자 함수를 호출하면 객체 리터럴과 마찬가지로 추상 연산 ObjectCreate를 호출

> Object 생성자 함수

![](https://poiemaweb.com/assets/fs-images/18-19.png)

- 이때 추상 연산 ObjectCreate에 전달되는 프로토타입은 Object.prototype
  - 즉, Object 생성자 함수에 의해 생성되는 객체의 프로토타입은 Object.prototype

```javascript
const obj = new Object();
obj.x = 1;
```

- 위 코드가 실행되면 추상 연산 ObjectCreate에 의해 아래와 같이 Object 생성자 함수와 Object.prototype과 생성된 객체 사이에 연결이 만들어 짐
  - 객체 리터럴에 의해 생성된 객체와 동일한 구조를 가짐

> Object 생성자 함수에 의해 생성된 객체의 프로토타입

![](https://poiemaweb.com/assets/fs-images/18-20.png)

- 이처럼 객체 obj는 Object.prototype을 프로토타입으로 갖게 되며 이로써 Object.prototype을 상속받음

```javascript
const obj = new Object();
obj.x = 1;

// 객체 obj는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x'));    // true
```

- 객체 리터럴과 Object 생성자 함수에 의한 객체 생성 방식의 차이는 프로퍼티를 추가하는 방식에 있음
  - 객체 리터럴 방식은 객체 리터럴 내부에 프로퍼티를 추가하지만 Object 생성자 함수 방식은 일단 빈객체를 생성한 이후 프로퍼티를 추가해야 함



### 6.3 생성자 함수에 의해 생성된 객체의 프로토타입

- new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하면 다른 객체 방식과 마찬가지로 추상 연산 ObjectCreate를 호출

> 생성자 함수에 의한 객체 생성

![](https://poiemaweb.com/assets/fs-images/18-21.png)

- 이때 추상 연산 ObjectCreate에 전달되는 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체
  - 즉, 생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체

```javascript
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');
```

- 위 코드가 실행되면 추상 연산 ObjectCreate에 의해 아래와 같이 생성자 함수와 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체와 생성된 객체 사이에 연결이 만들어 짐

> 생성자 함수에 의해 생성된 객체의 프로토타입

![](https://poiemaweb.com/assets/fs-images/18-22.png)

- 빌트인 객체인 Object 생성자 함수와 더불어 생성된 프로토타입 Object.prototype은 다양한 빌트인 메소드(hasOwnProperty, propertyIsEnumerable 등)를 갖고 있음
  - 하지만 사용자 정의 생성자 함수 Person과 더불어 생성된 프로토타입 Person.prototype의 프로퍼티는 constructor 뿐
- 프로토타입 Person.prototype에 프로퍼티를 추가하여 하위(자식) 객체가 상속받을 수 있도록 구현해보자
  - 프로토타입은 객체이므로 따라서 일반 객체와 같이 프로토타입에도 프로퍼티를 추가/삭제할 수 있음
  - 이렇게 추가/삭제된 프로퍼티는 프로토타입 체인에 즉각 반영

```javascript
function Person(name) {
  this.name = name;
}

// 프로토타입 메소드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');
const you = new Person('Kim');

me.sayHello();  // Hi! My name is Lee
you.sayHello(); // Hi! My name is Kim
```

- Person 생성자 함수를 통해 생성된 모든 객체는 프로토타입에 추가된 sayHello 메소드를 상속받아 자신의 메소드처럼 사용할 수 있음

> 프로토타입의 확장과 상속

![](https://poiemaweb.com/assets/fs-images/18-23.png)

## 7. 프로토타입 체인

```javascript
function Person(name) {
  this.name = name;
}

// 프로토타입 메소드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');

// hasOwnProperty는 Object.prototype의 메소드이다.
console.log(me.hasOwnProperty('name')); // true
```

- Person 생성자 함수에 의해 생성된 me 객체는 Object.prototype의 메소드인 hasOwnProperty를 호출할 수 있음
  - 이것은 me 객체가 Person.prototype 뿐만 아니라 Object.prototype를 상속받았다는 의미
  - me 객체의 프로토타입은 Person.prototype

```javascript
console.log(Object.getPrototypeOf(me) === Person.prototype); // true

Person.prototype의 프로토타입은 Object.prototype이다. 프로토타입의 프로토타입은 언제나 Object.prototype이다.
console.log(Object.getPrototypeOf(Person.prototype) === Object.prototype); // true
```

> 프로토타입 체인

![](https://poiemaweb.com/assets/fs-images/18-24.png)

- 자바스크립트는 객체의 프로퍼티(메소드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 ->
  `__proto__` 접근자 프로퍼티가 가리키는 링크를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색
  - 이것을 프로토타입 체인이라 함
  - 프로토타입 체인은 자바스크립트가 객체 지향 프로그래밍의 상속을 구현하는 메커니즘

```javascript
// hasOwnProperty는 Object.prototype의 메소드이다.
// me 객체는 프로토타입 체인을 따라 hasOwnProperty 메소드를 검색하여 사용한다.
console.log(me.hasOwnProperty('name')); // true
```

- me.hasOwnProperty('name')와 같이 메소드를 호출하면 자바스크립트 엔진은 아래와 같은 과정을 거쳐 메소드를 검색(프로퍼티도 동일)

  1. 먼저 hasOwnProperty 메소드를 호출한 me 객체에서 hasOwnProperty 메소드를 검색 -> me 객체에는 hasOwnProperty 메소드가 없으므로 프로토타입 체인을 따라, 다시 말해 `__proto__` 접근자 프로퍼티에 바인딩되어 있는 프로토타입(위 예제의 경우, Person.prototype)으로 이동하여 hasOwnProperty 메소드를 검색

  2. Person.prototype에도 hasOwnProperty 메소드가 없으므로 프로토타입 체인을 따라, 다시 말해 `__proto__` 접근자 프로퍼티에 바인딩되어 있는 프로토타입(위 예제의 경우, Object.prototype)으로 이동하여 hasOwnProperty 메소드를 검색

  3. Object.prototype에는 hasOwnProperty 메소드가 존재 -> 자바스크립트 엔진은 Object.prototype.hasOwnProperty 메소드를 호출 -> 이때 Object.prototype.hasOwnProperty 메소드의 this에는 me 객체가 바인딩

```javascript
Object.prototype.hasOwnProperty.call(me, 'name');
```

- 프로토타입 체인의 최상위에 위치하는 객체는 언제나 Object.prototype
  - 따라서 모든 객체는 Object.prototype을 상속받음
  - Object.prototype을 프로토타입 체인의 종점(End of prototype chain)이라 함
  - Object.prototype의 프로토타입, 즉 [[Prototype]] 내부 슬롯의 값은 null

- 프로토타입 체인의 종점인 Object.prototype에서도 프로퍼티를 검색할 수 없는 경우, undefined를 반환한다. 이때 에러가 발생하지 않는 것에 주의

```javascript
console.log(me.foo); // undefined
```

- 이와 같이 자바스크립트 엔진은 프로토타입 체인을 따라 프로퍼티/메소드를 검색
  - 다시 말해, 자바스크립트 엔진은 객체 간의 상속 관계로 이루어진 프로토타입의 계층적인 구조에서 객체의 프로퍼티를 검색
  - 따라서 프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘

```javascript
me.hasOwnProperty('name');
```

- 위 예제의 경우, 먼저 스코프 체인에서 식별자 me를 검색 -> 식별자 me는 전역에서 선언되었으므로 전역 스코프에서 검색 -> 식별자 me를 검색한 다음, me 객체의 프로토타입 체인에서 hasOwnProperty 메소드를 검색
  - 이처럼 스코프 체인과 프로토타입 체인은 별도로 서로 연관없이 동작하는 것이 아니라 서로 협력하여 식별자와 프로퍼티를 찾아냄



## 8. 캡슐화

```javascript
const Person = (function () {
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메소드
  Person.prototype.sayHello = function () {
    console.log(`Hi! My name is ${this.name}`);
  };

  // 생성자 함수를 반환
  return Person;
}());

const me = new Person('Lee');
```

- 이와 같이 즉시 실행 함수를 사용하여 생성자 함수와 프로토타입을 확장하는 코드를 하나의 함수 내에 깔끔하게 모을 수 있음
  - 위 패턴화 사용시 캡슐화(encapsulation) 쉽게 구현 가능
    - 캡슐화란? 정보의 일부를 외부에 감추어 은닉하는 것을 말함
    - 외부에 공개할 필요가 없는 구현의 일부를 외부에 노출되지 않도록 감추어 적절치 못한 접근으로부터 정보를 보호하고 객체간의 상호 의존성, 즉 **결합도를 낮추는 효과**

- 자바스크립트는 접근 제한자를 제공하지 않지만 캡슐화가 불가능한 것은 아님

```javascript
// name 프로퍼티는 public하다. 즉, 외부에서 자유롭게 접근하고 변경할 수 있다.
me.name = 'Kim';
me.sayHello(); // Hi! My name is Kim
```

- 현재 name 프로퍼티는 외부에 노출되어 있어 자유롭게 변경 가능
- name 프로퍼티를 캡슐화하여 외부로 노출되지 않도록 수정

```javascript
const Person = (function () {
  // 자유 변수이며 private하다
  let _name = '';

  // 생성자 함수
  function Person(name) { _name = name; }

  // 프로토타입 메소드
  Person.prototype.sayHello = function () {
    console.log(`Hi! My name is ${_name}`);
  };

  // 생성자 함수를 반환
  return Person;
}());

const me = new Person('Lee');

// _name은 지역 변수이므로 외부에서 접근하여 변경할 수 없다. 즉, private하다.
me._name = 'Kim';
me.sayHello(); // Hi! My name is Lee
```

- 아직 살펴보지 않았지만 Person.prototype.sayHello 메소드는 클로저
  - 즉시 실행 함수가 반환하는 생성자 함수가 생성할 객체가 상속받아 호출할 sayHello 메소드는 즉시 실행 함수가 종료한 이후 호출
  - 하지만 sayHello 메소드는 이미 종료되어 소멸한 즉시 실행 함수의 지역 변수 _name을 참조할 수 있음



## 9. 오버라이딩과 프로퍼티 쉐도잉

```javascript
const Person = (function () {
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메소드
  Person.prototype.sayHello = function () {
    console.log(`Hi! My name is ${this.name}`);
  };

  // 생성자 함수를 반환
  return Person;
}());

const me = new Person('Lee');

// 인스턴스 메소드
me.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};

// 인스턴스 메소드가 호출된다. 프로토타입 메소드는 인스턴스 메소드에 의해 가려진다.
me.sayHello(); // Hey! My name is Lee
```

- 생성자 함수로 객체(인스턴스)를 생성한 다음, 인스턴스에 메소드를 추가하였음

> 오버라이딩과 프로퍼티 쉐도잉

![](https://poiemaweb.com/assets/fs-images/18-25.png)

- 프로토타입이 소유한 프로퍼티(메소드 포함)를 *프로토타입 프로퍼티*, 인스턴스가 소유한 프로퍼티를 *인스턴스 프로퍼티*라고 부름
  - 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 프로토타입 체인을 따라 프로토타입 프로퍼티를 검색하여 프로토타입 프로퍼티를 덮어쓰는 것이 아니라 인스턴스 프로퍼티로 추가
  - 이때 인스턴스 메소드 sayHello는 프로토타입 메소드 sayHello를 오버라이딩하였고 프로토타입 메소드 sayHello는 가려짐

------

**오버라이딩(Overriding)**

- 상위 클래스가 가지고 있는 메소드를 하위 클래스가 재정의하여 사용하는 방식

**오버로딩(Overloading)**

- 함수의 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메소드를 구현하고 매개변수에 의해 메소드를 구별하여 호출하는 방식
  - 자바스크립트는 오버로딩을 지원하지 않지만 arguments 객체를 사용하여 구현할 수는 있다.

------

- 프로퍼티를 삭제하는 경우도 마찬가지

```javascript
// 인스턴스 메소드를 삭제한다.
delete me.sayHello;
// 인스턴스에는 sayHello 메소드가 없으므로 프로토타입 메소드가 호출된다.
me.sayHello(); // Hi! My name is Lee
```

- 당연히 프로토타입 메소드가 아닌 인스턴스 메소드 sayHello가 삭제됨
  - 다시 한번 sayHello 메소드를 삭제하여 프로토타입 메소드의 삭제를 시도해보자

```javascript
// 프로토타입 메소드는 삭제되지 않는다.
delete me.sayHello;
// 프로토타입 메소드가 호출된다.
me.sayHello(); // Hi! My name is Lee
```

- 이와 같이 하위 객체를 통해 프로토타입의 프로퍼티를 변경 또는 삭제하는 것은 불가능

  - 다시 말해 하위 객체를 통해 프로토타입에 get 액세스는 허용되나 set 액세스는 허용되지 않음

  - 프로토타입 프로퍼티를 변경 또는 삭제하려면 하위 객체를 통해 프로토타입 체인으로 접근하는 것이 아니라 프로토타입에 직접 접근하여야 함

```javascript
// 프로토타입 메소드 변경
Person.prototype.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};
me.sayHello(); // Hey! My name is Lee

// 프로토타입 메소드 삭제
delete Person.prototype.sayHello;
me.sayHello(); // TypeError: me.sayHello is not a function
```



## 10. 프로토타입의 교체

- 프로토타입은 다른 임의의 객체로 변경할 수 있음
  - 이것은 부모 객체인 프로토타입을 동적으로 변경할 수 있다는 것을 의미
  - 이러한 특징을 활용하여 객체 간의 상속 관계를 동적으로 변경할 수 있음
  - 프로토타입은 생성자 함수 또는 인스턴스에 의해 교체할 수 있음

### 10.1 생성자 함수에 의한 프로토타입의 교체

```javascript
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // ① 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    }
  };

  return Person;
}());

const me = new Person('Lee');
```

- ①에서 Person.prototype에 객체 리터럴을 할당
  - 이는 Person 생성자 함수가 생성할 객체의 프로토타입을 객체 리터럴로 교체한 것

> 생성자 함수에 의한 프로토타입의 교체

![](https://poiemaweb.com/assets/fs-images/18-26.png)

- 프로토타입으로 교체한 객체 리터럴에는 constructor 프로퍼티가 없음
  - 따라서 me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나옴
  - constructor 프로퍼티는 자바스크립트 엔진이 프로토타입을 생성할 때 암묵적으로 추가한 프로퍼티

```javascript
// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 링크가 파괴된다.
console.log(me.constructor === Person); // false
// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색된다.
console.log(me.constructor === Object); // true
```

- 이처럼 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 링크가 파괴
  - 파괴된 constructor 프로퍼티와 생성자 함수 간의 링크를 되살려 보자
  - 프로토타입으로 교체한 객체 리터럴에 constructor 프로퍼티를 추가하여 프로토타입의 constructor 프로퍼티를 되살림

```javascript
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    // constructor 프로퍼티와 생성자 함수 간의 링크 설정
    constructor: Person,
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    }
  };

  return Person;
}());

const me = new Person('Lee');

// constructor 프로퍼티가 생성자 함수를 가리킨다.
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false
```



### 10.2 인스턴스에 의한 프로토타입 교체

- 프로토타입은 생성자 함수의 prototype 프로퍼티 뿐만 아니라 인스턴스의 `__proto__` 접근자 프로퍼티로 접근할 수 있음
  - 따라서 인스턴스의 `__proto__` 접근자 프로퍼티(또는 Object.setPrototypeOf 메소드)를 통해 프로토타입을 교체할 수 있음

- 생성자 함수의 prototype 프로퍼티에 다른 임의의 객체를 바인딩하는 것은 미래에 생성할 인스턴스의 프로토타입을 교체하는 것
  -  `__proto__` 접근자 프로퍼티를 통해 프로토타입을 교체하는 것은 이미 생성된 객체의 프로토타입을 교체하는 것

```javascript
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// 프로토타입으로 교체할 객체
const parent = {
  sayHello() {
    console.log(`Hi! My name is ${this.name}`);
  }
};

// ① me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
// 위 코드는 아래의 코드와 동일하게 동작한다.
// me.__proto__ = parent;

me.sayHello(); // Hi! My name is Lee
```

- ①에서 me 객체의 프로토타입을 parent 객체로 교체

> 인스턴스에 의한 프로토타입의 교체

![](https://poiemaweb.com/assets/fs-images/18-27.png)

- “17.9.1 생성자 함수에 의한 프로토타입 교체”와 마찬가지로 프로토타입으로 교체한 객체에는 constructor 프로퍼티가 없음
  - 따라서 프로토타입의 constructor 프로퍼티로 me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나옴

```javascript
// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 링크가 파괴된다.
console.log(me.constructor === Person); // false
// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색된다.
console.log(me.constructor === Object); // true
```

- 생성자 함수에 의한 프로토타입 교체와 마찬가지로 인스턴스에 의한 프로토타입 교체도 constructor 프로퍼티와 생성자 함수 간의 연결을 파괴

- 생성자 함수에 의한 프로토타입 교체와 인스턴스에 의한 프로토타입 교체는 별다른 차이가 없어 보임
  - 하지만 미묘한 차이가 있음. 아래 그림으로 설명

![](https://poiemaweb.com/assets/fs-images/18-28.png)

- 프로토타입으로 교체한 객체 리터럴에 constructor 프로퍼티를 추가하고 생성자 함수의 prototype 프로퍼티를 재설정하여 파괴된 생성자 함수와 프로토타입 간의 연결을 되살려 보자

```javascript
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// 프로토타입으로 교체할 객체
const parent = {
  // constructor 프로퍼티와 생성자 함수 간의 링크 설정
  constructor: Person,
  sayHello() {
    console.log(`Hi! My name is ${this.name}`);
  }
};

// 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 링크 설정
Person.prototype = parent;

// me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
// 위 코드는 아래의 코드와 동일하게 동작한다.
// me.__proto__ = parent;

me.sayHello(); // Hi! My name is Lee

// constructor 프로퍼티가 생성자 함수를 가리킨다.
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false

// 생성자 함수의 prototype 프로퍼티가 교체된 프로토타입을 가리킨다.
console.log(Person.prototype === Object.getPrototypeOf(me)); // true
```

- 이처럼 프로토타입 교체를 통해 객체 간의 상속 관계를 동적으로 변경하는 것은 번거로움
  - 하지만 ES6에서 도입된 클래스를 사용하면 간편하고 직관적으로 상속 관계를 구현할 수 있음



## 11. instanceof 연산자

- instanceof 연산자는 이항 연산자로서 좌변에 객체를 가기키는 식별자, 우변에 생성자 함수를 가리키는 식별자를 피연산자로 받음
  - 만약 우변의 피연산자가 함수가 아닌 경우, TypeError가 발생

```javascript
객체 instanceof 생성자 함수
```

- 좌변의 객체가 우변의 생성자 함수와 연결된 인스턴스라면 true로 평가되고 그렇지 않은 경우에는 false로 평가
  - instanceof 연산자는 상속 관계를 고려한다는 것에 주의

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// me 객체는 Person 생성자 함수에 의해 생성된 인스턴스이다.
console.log(me instanceof Person); // true
// instanceof 연산자는 상속 관계를 고려한다.
// me 객체는 Object.prototype을 상속받기 때문에 아래의 코드는 true로 평가된다.
console.log(me instanceof Object); // true
```

- instanceof 연산자가 어떻게 상속 관계를 파악하는지 이해하기 위해, 인스턴스에 의해 프로토타입을 교체한 경우, instanceof 연산자가 어떻게 동작하는지 살펴보자

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// 프로토타입으로 교체할 객체
const parent = {};

// 인스턴스에 의한 프로토타입의 교체
// 교체된 프로토타입에는 constructor 프로퍼티가 없기 때문에
// 프로토타입과 생성자 함수의 링크가 파괴된다.
Object.setPrototypeOf(me, parent);

// me 객체는 Person 생성자 함수에 의해 생성된 인스턴스이다.
// 그러나 instanceof 연산자는 false를 반환한다.
console.log(me instanceof Person); // false
// instanceof 연산자는 상속 관계를 고려한다.
// me 객체는 Object.prototype을 상속받기 때문에 아래의 코드는 true로 평가된다.
console.log(me instanceof Object); // true
```

- me 객체는 비록 프로토타입이 교체되어 프로토타입과 생성자 함수의 링크가 파괴되었지만 Person 생성자 함수에 의해 생성된 인스턴스

  - 그러나 me instanceof Person의 평가 결과는 false이다.

  - 프로토타입의 constructor 프로퍼티가 생성자 함수를 가리키지 않아서 이러한 문제가 발생할 지도 모르니 교체된 프로토타입의 constructor 프로퍼티가 생성자 함수를 가리키도록 재설정해보자

```javascript
...
// 프로토타입으로 교체할 객체
const parent = {
  constructor: Person
};
...

console.log(me instanceof Person); // false
console.log(me instanceof Object); // true
```

- 여전히  me instanceof Person의 평가 결과는  false
  - 생성자 함수의 prototype 프로퍼티가 교체된 프로토타입을 가리키도록 재설정해보자

```javascript
...
// 프로토타입으로 교체할 객체
const parent = {
  constructor: Person
};

// 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 링크 설정
Person.prototype = parent;
...

console.log(me instanceof Person); // true
console.log(me instanceof Object); // true
```

- me instanceof Person의 평가 결과가  true로 변경되었음
  - 이를 통해 **instanceof 연산자**는 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수를 찾는 것이 아니라 **프로토타입 체인 상에 존재하는 프로토타입에 영향**을 받는 것임을 알 수 있음

- instanceof 연산자는 생성자 함수의 prototype 프로퍼티가 가리키는 객체가 프로토타입 체인 상에 존재하는지 확인

![](https://poiemaweb.com/assets/fs-images/18-29.png)

- instanceof 연산자는 좌변 피연산자의 프로토타입 체인 상에 우변의 피연산자, 즉 생성자 함수의 prototype 프로퍼티에 바인딩된 객체가 존재하는 지 검색

  - me instanceof Person의 경우, me 객체의 프로토타입 체인 상에 Person.prototype에 바인딩된 객체가 객체가 존재하는지 확인

  - me instanceof Object의 경우도 마찬가지로, me 객체의 프로토타입 체인 상에 Object.prototype에 바인딩된 객체가 객체가 존재하는지 확인

- instanceof 연산자를 함수로 표현하면 아래와 같음

```javascript
function isInstanceof(instance, constructor) {
  // 프로토타입 취득
  const prototype = Object.getPrototypeOf(instance);

  // 재귀 탈출 조건
  // prototype이 null이면 프로토타입 체인의 종점에 다다른 것이다.
  if (prototype === null) return false;

  // 프로토타입이 생성자 함수의 prototype 프로퍼티에 바인딩된 객체라면 true를 반환한다.
  // 그렇지 않다면 재귀 호출로 프로토타입 체인 상의 상위 프로토타입으로 이동하여 확인한다.
  return prototype === constructor.prototype ? true : isInstanceof(prototype, constructor);
}

console.log(isInstanceof(me, Person)); // true
console.log(isInstanceof(me, Object)); // true
console.log(isInstanceof(me, Array));  // false
```

- 생성자 함수에 의해 프로토타입이 교체되어 constructor 프로퍼티와 생성자 함수 간의 링크가 파괴된 경우, 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 링크는 파괴되지 않으므로 instanceof는 아무런 영향을 받지 않음

```javascript
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    }
  };

  return Person;
}());

const me = new Person('Lee');

// constructor 프로퍼티와 생성자 함수 간의 링크가 파괴되어도
// instanceof는 아무런 영향을 받지 않는다.
console.log(me instanceof Person); // true
console.log(me instanceof Object); // true
```



## 12. 직접 상속

### 12.1 Object.create에 의한 직접 상속

- Object.create 메소드는 명시적으로 프로토타입을 지정하여 새로운 객체를 생성
  - Object.create 메소드도 다른 객체 생성 방식과 마찬가지로 추상 연산 ObjectCreate를 호출

- Object.create 메소드의 첫번째 매개변수에는 생성할 객체의 프로토타입으로 지정할 객체를 전달
- 두번째 매개변수에는 생성할 객체의 프로퍼티를 갖는 객체를 전달
  - 이 객체의 형식은 Object.defineProperties 메소드(“15.4. 프로퍼티 어트리뷰트” 참고)의 두번째 인수와 동일
  - 두번째 인수는 옵션이므로 생략 가능

```javascript
/**
 * 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체를 생성하여 반환한다.
 * @param {Object} prototype - 생성할 객체의 프로토타입으로 지정할 객체
 * @param {Object} [propertiesObject] - 생성할 객체의 프로퍼티를 갖는 객체
 * @returns {Object} 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체
 */
Object.create(prototype[, propertiesObject])
```

```javascript
// 프로토타입이 null인 객체를 생성한다.
// 즉, 생성된 객체는 프로토타입 체인의 종점이므로 프로토타입 체인이 생성되지 않는다.
// obj → null
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null); // true
// Object.prototype를 상속받지 못한다.
console.log(obj.toString()); // TypeError: obj.toString is not a function

// obj = {};와 동일하다.
// obj → Object.prototype → null
obj = Object.create(Object.prototype);
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

// obj = { x: 1 };와 동일하다.
// obj → Object.prototype → null
obj = Object.create(Object.prototype, {
  x: { value: 1 }
});
// 위 코드는 아래와 동일하다.
// obj = Object.create(Object.prototype);
// obj.x = 1;
console.log(obj.x); // 1
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

const myProto = { x: 10 };
// 객체를 직접 상속받는다.
// obj → myProto → Object.prototype → null
obj = Object.create(myProto);

console.log(obj.x); // 10
console.log(Object.getPrototypeOf(obj) === myProto); // true

// 생성자 함수
function Person(name) {
  this.name = name;
}

// obj = new Person('Lee')와 동일하다.
// obj → Person.prototype → Object.prototype → null
obj = Object.create(Person.prototype);
obj.name = 'Lee';
console.log(obj.name); // Lee
console.log(Object.getPrototypeOf(obj) === Person.prototype); // true
```

- 이처럼 Object.create 메소드는 첫번째 매개변수에 전달한 객체의 프로토타입 체인에 속하는 객체를 생성
  - 즉, 객체를 생성하면서 직접적으로 상속을 구현하는 것

- 이 메소드의 장점은 아래와 같음
  - new 연산자가 없이도 객체를 생성할 수 있음
  - 프로토타입을 지정하면서 객체를 생성할 수 있음. 이 때 생성자 함수와 프로토타입간의 링크가 파괴되지 않음
  - 객체 리터럴에 의해 생성된 객체도 특정 객체를 상속받을 수 있음
- Object.prototype의 빌트인 메소드인 Object.prototype.hasOwnProperty, Object.prototype.isPrototypeOf, Object.prototype.propertyIsEnumerable 등은 모든 객체의 프로토타입 체인의 종점, 즉 Object.prototype의 메소드이므로 모든 객체가 상속받아 호출할 수 있음

```javascript
const obj = { a: 1 };
const child = Object.create(obj);

console.log(obj.hasOwnProperty('a'));       // true
console.log(obj.isPrototypeOf(child));      // true
console.log(obj.propertyIsEnumerable('a')); // true
```

- 그런데 ESLint에서는 위 예제와 같이 Object.prototype의 빌트인 메소드를 객체가 직접 호출하는 것을 비추천
  - 그 이유는 Object.create 메소드를 통해 프로토타입 체인을 생성하지 않는 객체, 다시 말해 프로토타입의 종점에 위치하는 객체를 생성할 수 있기 때문
  - 이때 프로토타입 체인을 생성하지 않는 객체는 Object.prototype의 빌트인 메소드를 사용할 수 없음

```javascript
// 프로토타입이 null인 객체를 생성한다.
const obj = Object.create(null);
obj.a = 1;

// 즉, 생성된 객체는 프로토타입 체인의 종점이므로 프로토타입 체인이 생성되지 않는다.
console.log(Object.getPrototypeOf(obj) === null); // true

// obj는 Object.prototype의 빌트인 메소드를 사용할 수 없다.
console.log(obj.hasOwnProperty('a')); // TypeError: obj.hasOwnProperty is not a function
```

- 따라서 이같은 에러를 발생시키는 가능성을 없애기 위해 Object.prototype의 빌트인 메소드는 아래와 같이 간접적으로 호출하는 것이 좋음

```javascript
// 프로토타입이 null인 객체를 생성한다.
const obj = Object.create(null);
obj.a = 1;

// console.log(obj.hasOwnProperty('a')); // TypeError: obj.hasOwnProperty is not a function

// Object.prototype의 빌트인 메소드는 객체로 직접 호출하지 않는다.
console.log(Object.prototype.hasOwnProperty.call(obj, 'a')); // true
```



### 12.2 객체 리터럴 내부에서 `__proto__`에 의한 직접 상속

- Object.create 메소드는 직접 상속은 위와 같이 여러 장점이 있음
  - 하지만 두번째 인자로 프로퍼티를 정의하는 것은 번거로움
  - 일단 객체를 생성한 이후, 프로퍼티를 추가하는 방법도 있으나 이 또한 깔끔한 방법은 아님
- ES6에서는 객체 리터럴 내부에서 `__proto__` 접근자 프로퍼티를 사용하여 직접 상속을 구현할 수 있음

```javascript
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있다.
const obj = {
  y: 20,
  // 객체를 직접 상속받는다.
  // obj → myProto → Object.prototype → null
  __proto__: myProto
};
// 위 코드는 아래와 동일하다.
// const obj = Object.create(myProto, { y: { value: 20 } });

console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototypeOf(obj) === myProto); // true
```



## 13. 정적 프로퍼티/메소드

- 정적(static) 프로퍼티/메소드는 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메소드를 말함

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메소드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

// Person 생성자 함수는 객체이므로 자신의 프로퍼티/메소드를 소유할 수 있다.
// 정적 프로퍼티
Person.staticProp = 'static prop';
// 정적 메소드
Person.staticMethod = function () {
  console.log('staticMethod');
};

const me = new Person('Lee');

// 생성자 함수에 추가한 정적 프로퍼티/메소드는 생성자 함수로 참조/호출한다.
Person.staticMethod(); // staticMethod

// 정적 프로퍼티/메소드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.
// 인스턴스로 참조/호출할 수 있는 프로퍼티/메소드는 프로토타입 체인 상에 존재해야 한다.
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

- Person 생성자 함수는 객체이므로 자신의 프로퍼티/메소드를 소유할 수 있음
  - Person 생성자 함수 객체가 소유한 프로퍼티/메소드를 정적 프로퍼티/메소드라고 부름
  - 정적 프로퍼티/메소드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없음

> 정적 프로퍼티/메소드

![](https://poiemaweb.com/assets/fs-images/18-31.png)

- 생성자 함수가 생성한 인스턴스(모든 객체는 생성자 함수에 의해 생성)는 자신의 프로토타입 체인에 속한 객체의 프로퍼티/메소드에 접근할 수 있음
  - 정적 프로퍼티/메소드는 인스턴스의 프로토타입 체인에 속한 객체의 프로퍼티/메소드가 아니므로 인스턴스로 접근할 수 없음

- 앞에서 살펴본 Object.create 메소드는 Object 생성자 함수의 정적 메소드이고 Object.prototype.hasOwnProperty 메소드는 Object.prototype의 메소드
  - 따라서 Object.create 메소드는 인스턴스, 즉 Object 생성자 함수가 생성한 객체로 호출할 수 없음
  - 하지만 Object.prototype.hasOwnProperty 메소드는 모든 객체의 프로토타입 체인의 종점, 즉 Object.prototype의 메소드이므로 모든 객체가 호출할 수 있음

```javascript
// Object.create는 정적 메소드이다.
const obj = Object.create({});

// Object.prototype.hasOwnProperty는 프로토타입 메소드이다.
console.log(obj.hasOwnProperty('name'));
```

- 만약 인스턴스/프로토타입 메소드 내에서 this를 사용하지 않는다면 그 메소드는 정적 메소드로 변경할 수 있음
  - 인스턴스가 호출한 인스턴스/프로토타입 메소드 내에서 this는 인스턴스를 가리킴
  - 메소드 내에서 인스턴스를 참조할 필요가 없다면 정적 메소드로 변경하여도 동작
  - 프로토타입 메소드를 호출하려면 인스턴스를 생성해야 하지만 정적 메소드는 인스턴스를 생성하지 않아도 호출할 수 있음

```javascript
function Foo() {}

// 프로토타입 메소드 내에서 this를 참조하지 않는다.
// 이 메소드는 정적 메소드로 변경하여도 동일한 효과를 얻을 수 있다.
Foo.prototype.x = function () {
  console.log('x');
};

const foo = new Foo();
// 프로토타입 메소드를 호출하려면 인스턴스를 생성해야 한다.
foo.x(); // x

// 정적 메소드 내에서 this는 생성자 함수를 가리킨다.
Foo.x = function () {
  console.log('x');
};

// 정적 메소드는 인스턴스를 생성하지 않아도 호출할 수 있다.
Foo.x(); // x
```

- MDN과 같은 문서를 보면 아래와 같이 정적 프로퍼티/메소드와 프로토타입 프로퍼티/메소드를 구분하고 소개하고 있음
  - 따라서 표기법만으로도 정적 프로퍼티/메소드와 프로토타입 프로퍼티/메소드를 구별할 수 있어야 함

![](https://poiemaweb.com/assets/fs-images/18-32.png)

- 참고로 프로토타입 프로퍼티/메소드를 표기할 때 prototype을 #으로 표기(예를들어 Object.prototype.isPrototypeOf을 Object#isPrototypeOf으로 표기)하는 경우도 있으니 알아두도록 하자



## 14. 프로퍼티 존재 확인

- in 연산자는 객체 내에 프로퍼티가 존재하는지 여부를 확인

```javascript
/**
 * prop: 프로퍼티 키를 나타내는 문자열
 * object: 객체로 평가되는 표현식
 */
prop in object
```

```javascript
const person = {
  name: 'Lee',
  address: 'Seoul'
};

// person 객체에 name 프로퍼티가 존재한다.
console.log('name' in person);    // true
// person 객체에 address 프로퍼티가 존재한다.
console.log('address' in person); // true
// person 객체에 age 프로퍼티가 존재하지 않는다.
console.log('age' in person);     // false
```

- in 연산자는 확인 대상 객체(위 예제의 경우, person 객체)의 프로퍼티 뿐만 아니라 확인 대상 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인하므로 주의
  - person 객체에는 toString이라는 프로퍼티가 없지만 아래의 실행 결과는 true

```javascript
console.log('toString' in person); // true
```

- 이는 in 연산자가 person 객체가 속한 프로토타입 체인 상에 존재하는 모든 프로토타입에서 toString 프로퍼티를 검색했기 때문
  - toString은 Object.prototype의 메소드이다.

- Object.prototype.hasOwnProperty 메소드를 사용해도 객체의 프로퍼티의 존재 여부를 확인 가능

```javascript
console.log(person.hasOwnProperty('name')); // true
console.log(person.hasOwnProperty('age'));  // false
```

- Object.prototype.hasOwnProperty 메소드는 이름에서 알 수 있듯이 객체 고유의 프로퍼티인 경우에만 true를 반환하고 상속받은 프로토타입의 프로퍼티인 경우 false를 반환

```javascript
console.log(person.hasOwnProperty('toString')); // false
```



## 15. 프로퍼티 열거

- 객체의 모든 프로퍼티를 순회하며 열거(enumeration)하려면 for…in 문을 사용
  - for…in 문은 프로퍼티를 열거할 때 순서를 보장하지 않음

```javascript
for (변수선언문 in 객체) { … }
```

```javascript
const person = {
  name: 'Lee',
  address: 'Seoul'
};

// for...in 문의 변수 prop에 person 객체의 프로퍼티 키가 할당된다. 단, 순서는 보장되지 않는다.
for (const prop in person) {
  console.log(prop + ': ' + person[prop]);
}

// name: Lee
// address: Seoul
```

- for…in 문은 객체의 프로퍼티 개수만큼 반복하며 for…in 문의 변수 선언문에서 선언한 변수에 프로퍼티 키를 할당
  - 위 예제의 경우, person 객체에는 2개의 프로퍼티가 있으므로 객체를 2번 순회하면서 프로퍼티 키를 prop 변수에 할당한 후 코드 블록을 실행
  - 첫번째 순회에서는 프로퍼티 키 ‘name’을 prop 변수에 할당한 후 코드 블록을 실행하고 두번째 순회에서는 프로퍼티 키 ‘address’를 prop 변수에 할당한 후 코드 블록을 실행

- for…in 문은 in 연산자처럼 순회 대상 객체의 프로퍼티 뿐만 아니라 객체가 상속받은 모든 프로토타입의 프로퍼티를 열거
  - 하지만 위 예제의 경우, toString과 같은 Object.prototype의 프로퍼티가 열거되지 않음

```javascript
// in 연산자는 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인한다.
console.log('toString' in person); // true

// for...in 문도 객체가 상속받은 모든 프로토타입의 프로퍼티를 열거한다.
// 하지만 toString과 같은 Object.prototype의 프로퍼티가 열거되지 않는다.
for (const prop in person) {
  console.log(prop + ': ' + person[prop]);
}

// name: Lee
// address: Seoul
```

- 이는 toString 메소드가 열거할 수 없도록 정의되어 있는 프로퍼티이기 때문
  - 다시 말해, Object.prototype.string 프로퍼티의 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false이기 때문
  - 프로퍼티 어트리뷰트 [[Enumerable]]는 프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 가짐 (“15.4 프로퍼티 어트리뷰트” 참고)

```javascript
// Object.getOwnPropertyDescriptor 메소드는 프로퍼티의 Descriptor를 반환한다.
// Descriptor는 프로퍼티의 정보를 담고 있는 객체이다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, 'toString'));
// {value: ƒ, writable: true, enumerable: false, configurable: true}
```

- 따라서 for…in 문에 대해 좀 더 정확히 표현하면 아래와 같음
  - for…in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]]의 값인 ture인 프로퍼티를 순회하며 열거(enumeration)

```javascript
const person = {
  name: 'Lee',
  address: 'Seoul'
};

Object.setPrototypeOf(person, { age: 20 });

for (const prop in person) {
  console.log(prop + ': ' + person[prop]);
}

// name: Lee
// address: Seoul
// age: 20
```

- for…in 문은 프로퍼티 키가 심볼인 프로퍼티는 열거하지 않음

```javascript
const sym = Symbol();
const obj = {
  a: 1,
  [sym]: 10
};

for (const prop in obj) {
  console.log(prop + ': ' + obj[prop]);
}
// a: 1
```

- 상속받은 프로퍼티는 제외하고 객체 자신의 프로퍼티만을 열거하려면 Object.prototype.hasOwnProperty 메소드를 사용하여 객체 자신의 프로퍼티인지 확인해야 함

```javascript
const person = {
  name: 'Lee',
  address: 'Seoul'
};

Object.setPrototypeOf(person, { age: 20 });

for (const prop in person) {
  // 객체 자신의 프로퍼티인지 확인한다.
  if (!person.hasOwnProperty(prop)) continue;
  console.log(prop + ': ' + person[prop]);
}

// name: Lee
// address: Seoul

```

- 위 예제의 결과는 person 객체의 프로퍼티가 정의된 순서대로 열거되었음
  - 하지만 for…in 문은 프로퍼티를 열거할 때 순서를 보장하지 않으므로 주의
  - 또한 배열에는 for…in 문을 사용하지 않아야 함 -> 배열에는 for…in 문보다 일반적인 for 문이나 for…of 문 또는 Array.prototype.forEach 메소드를 사용하기를 권장



Reference : https://poiemaweb.com/