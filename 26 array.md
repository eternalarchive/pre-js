# 26. 배열

## 1. 배열이란?

- 배열(array) : 순서가 있는 값들의 연속적인 나열, 사용 빈도가 매우 높은 자료 구조
  - 배열을 사용하는 이유는 순서가 있는 여러 개의 값을 하나의 자료 구조로 묶어 관리하기 위함
  - 배열은 하나의 변수에 여러 개의 값을 저장할 수 있는 장점

- 배열이 가지고 있는 값 : 요소(element)
  - 자바스크립트에서 값으로 인정하는 모든 것은 배열의 요소가 될 수 있음
  - 배열에서 요소는 배열에서 자신의 위치를 나타내는 0이상의 정수인 인덱스(index)를 가짐 : 0부터 시작

#### 배열 리터럴로 생성

```javascript
const arr = ['apple', 'banana', 'orange'];
```

- 요소에 접근할 때는 대괄호 표기법 사용

```javascript
arr[0] // -> 'apple'
arr[1] // -> 'banana'
arr[2] // -> 'orange'

arr.length // -> 3
```

- 배열은 요소의 개수, 즉 배열의 길이를 나타내는 length 프로퍼티를 가짐

```javascript
typeof arr // -> object
```

- 자바스크립트에 배열이라는 타입은 없고, 객체로 인식

#### Array 생성자 함수로 생성

- 배열의 생성자 함수는 Array이며 배열의 프로토타입 객체는 Array.prototype
- Array.prototype은 배열을 위한 빌트인 메소드 들을 제공

```javascript
const arr = [1, 2, 3];

arr.constructor === Array // -> true
Object.getPrototypeOf(arr) === Array.prototype // -> true
```

#### 배열과 객체의 차이점

- 배열은 객체이지만 일반 객체와 다른 독특한 특징 존재

| 구분            |           객체            |     배열      |
| :-------------- | :-----------------------: | :-----------: |
| 구조            | 프로퍼티 키와 프로퍼티 값 | 인덱스와 요소 |
| 값의 참조       |        프로퍼티 키        |    인덱스     |
| 값의 순서       |             x             |       ○       |
| length 프로퍼티 |             x             |       ○       |

- 일반 객체와 배열을 구분하는 가장 명확한 차이는 “값의 순서”와 “length 프로퍼티”
- 값의 순서와 length 프로퍼티를 갖는 배열은 반복문을 통해 순차적으로 값에 접근하기 적합한 자료 구조

```javascript
const arr = [1, 2, 3];

// 반복문으로 자료 구조를 순회하기 위해서는 자료 구조의 길이를 알아야 한다.
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]); // 1 2 3
}
```

- 배열의 장점은 처음부터 순차적으로 요소에 접근할 수도 있고, 마지막부터 거꾸로 요소에 접근할 수도 있으며 특정 위치부터 순차적으로 요소에 접근할 수도 있음
  - 배열이 인덱스, 즉 값의 순서와 length 프로퍼티를 갖기 때문에 가능



## 2. 자바스크립트 배열은 배열이 아니다

- 일반적으로 배열이라는 자료 구조의 개념은 밀집 배열(Dense array)
  - 동일한 크기의 메모리 공간이 빈틈없이 연속적으로 나열된 자료 구조로 즉, 배열의 요소는 하나의 타입으로 통일되어 있으며 서로 연속적으로 인접

![](https://poiemaweb.com/assets/fs-images/26-1.png)

- 배열의 요소는 동일한 크기를 갖으며 빈틈없이 연속적으로 이어져 있으므로 아래와 같이 인덱스를 통해 단 한번의 연산으로 임의의 요소에 접근(임의 접근(Random access), 시간 복잡도 O(1))할 수 있음
  - 이는 매우 효율적이며 고속으로 동작

```
검색 대상 요소의 메모리 주소 = 배열의 시작 메모리 주소 + 인덱스 * 요소의 바이트 수
```

- 예) 위 그림처럼 메모리 주소 1000에서 시작하고 각 요소의 크기가 8byte인 배열을 생각해 보자
  - 인덱스가 0인 요소의 메모리 주소 : 1000 + 0 * 8 = 1000
  - 인덱스가 1인 요소에 메모리 주소 : 1000 + 1 * 8 = 1008
  - 인덱스가 2인 요소에 메모리 주소 : 1000 + 2 * 8 = 1016

- 배열은 인덱스를 통해 효율적으로 요소에 접근할 수 있다는 장점
- 하지만 정렬되지 않은 배열에서 특정한 값을 탐색하는 경우, 모든 배열 요소를 처음부터 값을 발견할 때까지 차례대로 탐색(선형 탐색(Linear search), 시간 복잡도 O(n))해야 함

- 또한 배열에 요소를 삽입하거나 삭제하는 경우, 배열 요소를 연속적으로 유지하기 위해 요소를 이동시켜야 하는 단점

![](https://poiemaweb.com/assets/fs-images/26-2.png)

#### 자바스크립트의 배열은 일반적인 배열과 다름

> 자바스크립트의 배열은 지금까지 살펴본 일반적인 의미의 배열과 다름

- 즉, 배열의 요소를 위한 각각의 메모리 공간은 동일한 크기를 갖지 않아도 되며 연속적으로 이어져 있지 않을 수도 있음
- 배열의 요소가 연속적으로 이어져 있지 않는 배열을 **희소 배열(sparse array)**이라 함
- 이처럼 자바스크립트의 배열은 엄밀히 말해 일반적 의미의 배열이 아니고, 자바스크립트의 배열은 일반적인 배열의 동작을 흉내낸 특수한 객체

```javascript
const arr = [1, 2, 3];

console.log(Object.getOwnPropertyDescriptors(arr));
/*
{
  '0': { value: 1, writable: true, enumerable: true, configurable: true },
  '1': { value: 2, writable: true, enumerable: true, configurable: true },
  '2': { value: 3, writable: true, enumerable: true, configurable: true },
  length: { value: 3, writable: true, enumerable: false, configurable: false }
}
*/
```

- 자바스크립트 배열은 **인덱스를 프로퍼티 키**로 갖으며 **length 프로퍼티를 갖는 특수한 객체**
- **자바스크립트 배열의 요소는 사실 프로퍼티 값**
  - 자바스크립트에서 사용할 수 있는 모든 값은 객체의 프로퍼티 값이 될 수 있으므로 어떤 타입의 값이라도 배열의 요소가 될 수 있음

```javascript
const arr = [
  'string',
  10,
  true,
  null,
  undefined,
  NaN,
  Infinity,
  [ ],
  { },
  function () {}
];
```

- 일반적인 배열과 자바스크립트 배열의 장단점
  - **일반적인 배열**은 인덱스로 배열 요소에 빠르게 접근할 수 있지만 요소를 삽입하거나 삭제하는 경우에는 효율적이지 않음
  - **자바스크립트 배열**은 기본적으로 해시 테이블로 구현된 객체이므로 인덱스로 배열 요소에 접근할 때 성능적인 면에서 일반적인 배열보다 느릴 수밖에 없는 구조적인 단점을 갖지만, 배열 요소를 삽입하거나 삭제하는 경우에는 일반적인 배열보다 빠른 성능을 기대할 수 있음

- 따라서 자바스크립트 배열은 인덱스로 배열 요소에 접근하는 경우에는 일반적인 배열보다 느리지만 배열 요소를 삽입하거나 삭제하는 경우에는 일반적인 배열보다 빠름
  - 즉, 자바스크립트 배열은 인덱스로 접근하는 경우의 성능 대신 배열 요소를 삽입하거나 삭제하는 경우의 성능을 선택한 것

- 이처럼 인덱스로 배열 요소에 접근할 때 일반적인 배열보다 느릴 수 밖에 없는 구조적인 단점을 보완하기 위해 대부분의 모던 자바스크립트 엔진은 배열을 일반 객체와 구별하여 보다 배열처럼 동작하도록 최적화하여 구현

- 아래와 같이 배열과 객체의 성능을 테스트 해보면 배열이 객체보다 약 2배 정도 빠른 것을 알 수 있음

```javascript
const arr = [];

console.time('Array Performance Test');

for (let i = 0; i < 10000000; i++) {
  arr[i] = i;
}
console.timeEnd('Array Performance Test');
// 약 340ms

const obj = {};

console.time('Object Performance Test');

for (let i = 0; i < 10000000; i++) {
  obj[i] = i;
}

console.timeEnd('Object Performance Test');
// 약 600ms
```



## 3. length 프로퍼티와 희소 배열

- length 프로퍼티는 요소의 개수, 즉 배열의 길이를 나타내는 정수를 값으로 가짐
  - length 프로퍼티의 값은 빈 배열일 경우, 0이며 빈 배열이 아닐 경우, 가장 큰 인덱스에 1을 더한 것과 같음

```javascript
[].length        // -> 0
[1, 2, 3].length // -> 3
```

- length 프로퍼티 값은 0과 2의32승 -1 미만의 양의 정수
- 배열에는 요소를 최대 2의 32승 - 1만큼 가질 수 있음
- length 프로퍼티의 값은 배열에 요소를 추가하거나 삭제하면 자동 갱신

```javascript
const arr = [1, 2, 3];

console.log(arr.length); // 3

// length 프로퍼티의 값은 배열에 요소를 추가하거나 삭제하면 자동 갱신된다.
// 요소 추가
arr.push(4);
console.log(arr.length); // 4

// 요소 삭제
arr.pop();
console.log(arr.length); // 3
```

- length 프로퍼티의 값은 요소의 개수, 즉 배열의 길이를 바탕으로 결정되지만 임의의 숫자 값을 명시적으로 할당할 수도 있음
- 현재 length 프로퍼티 값보다 작은 숫자 값을 할당하면 배열의 길이가 줄어듦

```javascript
const arr = [1, 2, 3, 4, 5];

// length 프로퍼티에 현재 length 프로퍼티 값보다 작은 숫자 값을 할당
arr.length = 3;

// 배열의 길이가 줄어든다.
console.log(arr); // [1, 2, 3]
```

- 주의할 것은 현재 length 프로퍼티 값보다 큰 숫자 값을 할당하는 경우로, 이때 length 프로퍼티 값은 변경되지만 실제로 배열의 길이가 늘어나지는 않음

```javascript
const arr = [1];

// length 프로퍼티에 현재 length 프로퍼티 값보다 큰 숫자 값을 할당
arr.length = 3;

// length 프로퍼티 값은 변경되지만 실제로 배열의 길이가 늘어나지는 않는다.
console.log(arr.length); // 3
console.log(arr); // [1, empty × 2]
```

- 위 예제의 출력 결과에서 empty × 2는 실제로 추가된 배열의 요소가 아니므로arr[1]과 arr[2]에는 값이 존재하지 않음
- 이와 같이 length 프로퍼티에 현재 length 프로퍼티 값보다 큰 숫자 값을 할당하는 경우, length 프로퍼티 값은 성공적으로 변경되지만 실제 배열에는 아무런 변함이 없음
  - 값이 없이 비어있는 요소를 위해 메모리 공간을 확보하지 않으며 빈 요소를 생성하지도 않음

```javascript
console.log(Object.getOwnPropertyDescriptors(arr));
/*
{
  '0': { value: 1, writable: true, enumerable: true, configurable: true },
  length: { value: 3, writable: true, enumerable: false, configurable: false }
}
*/
```

- 이처럼 배열의 요소가 연속적으로 위치하지 않고 일부가 비어있는 배열을 **희소 배열**이라 함
  - 자바스크립트는 희소 배열을 문법적으로 허용
  - 위 예제는 배열의 뒷부분만이 비어 있어서 요소가 연속적으로 위치하는 것처럼 보일 수 있으나 중간이나 앞 부분이 비어 있을 수도 있음

```javascript
// 희소 배열
const sparse = [, 2, , 4];

// 희소 배열의 length 프로퍼티 값은 요소의 개수와 일치하지 않는다.
console.log(sparse.length); // 4
console.log(sparse); // [empty, 2, empty, 4]

// 배열 arr에는 인덱스가 0, 2인 요소가 존재하지 않는다.
console.log(Object.getOwnPropertyDescriptors(sparse));
/*
{
  '1': { value: 2, writable: true, enumerable: true, configurable: true },
  '3': { value: 4, writable: true, enumerable: true, configurable: true },
  length: { value: 4, writable: true, enumerable: false, configurable: false }
}
*/
```

- 일반적인 배열의 length는 배열 요소의 개수, 즉 배열의 길이와 언제나 일치하지만, 희소 배열은 length와 배열 요소의 개수가 일치하지 않음
  - 희소 배열은 length는 배열의 실제 요소 개수보다 언제나 큼

- 자바스크립트는 문법적으로 희소 배열을 허용하지만 의도적으로 희소 배열을 만들어야 하는 상황은 없으므로 희소 배열은 사용하지 않는 것이 좋음
  - 희소 배열은 연속적인 값의 집합이라는 배열의 기본적인 개념과 맞지 않으며 성능에도 좋지 않은 영향

- 배열을 생성할 경우에는 희소 배열을 생성하지 않도록 주의
  - 배열에는 같은 타입의 요소를 연속적으로 위치시키는 것이 최선



## 4. 배열 생성

### 4.1 배열 리터럴

- 객체와 마찬가지로 배열도 다양한 생성 방식이 있는데, 가장 일반적이고 간편한 배열 생성 방식은 배열 리터럴을 사용하는 것
  - 배열 리터럴은 0개 이상의 요소를 쉼표로 구분하여 대괄호([])로 묶음
  - 배열 리터럴은 객체 리터럴과 달리 프로퍼티 이름이 없고 값만이 존재

```javascript
const arr = [1, 2, 3];
console.log(arr.length); // 3
```

- 배열 리터럴에 요소를 하나도 추가하지 않으면 배열의 길이, 즉 length 프로퍼티 값이 가 0인 빈 배열

```javascript
const arr = [];
console.log(arr.length); // 0
```

- 배열 리터럴에 요소를 생략하면 희소 배열이 생성

```javascript
const arr = [1, , 3]; // 희소 배열

// 희소 배열은 length는 배열의 실제 요소 개수보다 언제나 크다.
console.log(arr.length); // 3
console.log(arr);        // [1, empty, 3]
console.log(arr[1]);     // undefined
```

- 위 예제의 배열은 인덱스가 1인 요소를 갖지 않는다. arr[1]이 undefined인 이유는 사실은 객체인 arr에 프로퍼티 키가 ‘1’인 프로퍼티가 존재하지 않기 때문



### 4.2 Array 생성자 함수

- Object 생성자 함수를 통해 객체를 생성할 수 있듯이 Array 생성자 함수를 통해 배열을 생성할 수도 있음
  -  Array 생성자 함수는 전달된 인수의 개수에 따라 다르게 동작
  - 전달된 인수가 1개이고 숫자인 경우, 인수를 length 프로퍼티의 값으로 갖는 배열을 생성

```javascript
const arr = new Array(10);

console.log(arr); // [empty × 10]
console.log(arr.length); // 10
```

- 이때 생성된 배열은 희소 배열
  - length 프로퍼티의 값은 0이 아니지만 실제로 배열의 요소는 존재하지 않음

```javascript
console.log(Object.getOwnPropertyDescriptors(arr));
/*
{
  length: { value: 10, writable: true, enumerable: false, configurable: false }
}
*/
```

- 전달된 인수가 0 ~ 2의 32승 -1 을 벗어나면 RangeError가 발생

```javascript
// 전달된 인수가 음수이면 에러가 발생한다.
new Array(-1); // RangeError: Invalid array length

// 배열에는 요소를 최대 4294967295개 갖을 수 있다.
```

- 전달된 인수가 없는 경우, 빈 배열을 생성한다. 즉, 배열 리터럴 []과 같음

```javascript
const empty = new Array();

console.log(empty); // []
```

- 전달된 인수가 2개 이상이거나 숫자가 아닌 경우, 인수를 요소로 갖는 배열을 생성

```javascript
// 전달된 인수가 1개이지만 숫자가 아니면 인수를 요소로 갖는 배열을 생성한다.
const arr1 = new Array({});

console.log(arr1); // [{}]

// 전달된 인수가 2개 이상이면 인수를 요소로 갖는 배열을 생성한다.
const arr2 = new Array(1, 2, 3);

console.log(arr2); // [1, 2, 3]
```

- Array 생성자 함수는 new 연산자와 함께 호출하지 않더라도, 즉 함수로 호출하더라도 배열을 생성하는 생성자 함수로 동작
  - 이는 Array 생성자 함수 내부에서 new.target을 확인하기 때문



### 4.3 Array.of

- ES6에서 새롭게 도입된 Array.of 메소드는 전달된 인수를 요소로 갖는 배열을 생성
  - Array.of는 Array 생성자 함수와 다르게 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성

```javascript
// 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성한다.
const arr1 = Array.of(1);
console.log(arr1); // [1]

const arr2 = Array.of(1, 2, 3);
console.log(arr2); // [1, 2, 3]

const arr3 = Array.of('string');
console.log(arr3); // 'string'
```



### 4.4 Array.from

- ES6에서 새롭게 도입된 Array.from 메소드는 유사 배열 객체(array-like object) 또는 이터러블 객체(iterable object)를 변환하여 새로운 배열을 생성

```javascript
// 문자열은 이터러블이다.
const arr1 = Array.from('Hello');
console.log(arr1); // [ 'H', 'e', 'l', 'l', 'o' ]

// 유사 배열 객체를 새로운 배열을 변환하여 반환한다.
const arr2 = Array.from({ length: 2, 0: 'a', 1: 'b' });
console.log(arr2); // [ 'a', 'b' ]
```

- Array.from을 사용하면 두번째 인수로 전달한 함수를 통해 값을 만들면서 요소를 채울 수 있음
  - 두번째 인수로 전달한 함수는 첫번째 인수에 의해 생성된 배열의 요소값과 인덱스를 순차적으로 전달받아 새로운 요소를 생성

```javascript
// Array.from의 두번째 인수로 배열의 모든 요소에 대해 호출할 함수를 전달할 수 있다.
// 이 함수는 첫번째 인수에 의해 생성된 배열의 요소값괴 인덱스를 순차적으로 전달받아 호출된다.
const arr3 = Array.from({ length: 5 }, function (v, i) { return i; });
console.log(arr3); // [ 0, 1, 2, 3, 4 ]
```

- 유사 배열 객체와 이터러블 객체
  - 유사 배열 객체(Array-like Object)는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체
  - 유사 배열 객체는 마치 배열처럼 인덱스를 통해 프로퍼티에 접근할 수 있으며 length 프로퍼티를 갖기 때문에 for 문으로 순회할 수도 있음

```javascript
// 유사 배열 객체
const arrayLike = {
  '0': 'apple',
  '1': 'banana',
  '2': 'orange',
  length: 3
};

// 유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.
// 유사 배열 객체는 length 프로퍼티를 갖기 때문에 for 문으로 순회할 수도 있다.
for (let i = 0; i < arrayLike.length; i++) {
  console.log(arrayLike[i]); // apple banana orange
}
```

- 이터러블 객체(iterable object)는 Symbol.iterator 메소드를 구현하여 for…of 문으로 순회할 수 있으며 Spread 문법의 대상으로 사용할 수 있는 객체
  - ES6에서 제공하는 빌트인 이터러블은 Array, String, Map, Set, DOM data structure(NodeList, HTMLCollection), Arguments 등이 있음



## 5. 배열 요소의 참조

- 배열 요소를 참조할 때에는 대괄호([]) 표기법을 사용
- 대괄호 안에는 인덱스가 와야 하는데, 정수로 평가되는 표현식이라면 인덱스로 사용할 수 있음
  - 인덱스는 값을 참조할 수 있다는 의미에서 객체의 프로퍼티 키와 같은 역할

```javascript
const arr = [1, 2];

// 인덱스가 0인 요소를 참조
console.log(arr[0]); // 1
// 인덱스가 1인 요소를 참조
console.log(arr[1]); // 2
```

- 존재하지 않는 요소에 접근하면 undefined가 반환

```javascript
const arr = [1, 2];

// 인덱스가 2인 요소를 참조
// 배열 arr에 인덱스가 2인 요소는 존재하지 않는다.
console.log(arr[2]); // undefined
```

- 배열은 사실 인덱스를 프로퍼티 키로 갖는 객체
  - 따라서 존재하지 않는 프로퍼티 키로 객체의 프로퍼티에 접근했을 때 undefined를 반환하는 것처럼 배열도 존재하지 않는 요소를 참조하면 undefined 반환
  - 같은 이유로 희소 배열의 존재하지 않는 요소를 참조하여도 undefined가 반환

```javascript
// 희소 배열
const arr = [1, , 3];

// 배열 arr에는 인덱스가 1인 요소가 존재하지 않는다.
console.log(Object.getOwnPropertyDescriptors(arr));
/*
{
  '0': { value: 1, writable: true, enumerable: true, configurable: true },
  '2': { value: 3, writable: true, enumerable: true, configurable: true },
  length: { value: 3, writable: true, enumerable: false, configurable: false }
*/

// 존재하지 않는 요소를 참조하면 undefined가 반환된다.
console.log(arr[1]); // undefined
console.log(arr[3]); // undefined
```



## 6. 배열 요소의 추가와 갱신

- 객체에 프로퍼티를 동적으로 추가할 수 있는 것처럼 배열에도 요소를 동적으로 추가할 수 있음
- 요소가 존재하지 않는 인덱스의 배열 요소에 값을 할당하면 새로운 요소 추가
  - 이때 length 프로퍼티 값은 자동 갱신

```javascript
const arr = [0];

// 배열 요소의 추가
arr[1] = 1;

console.log(arr); // [ 0, 1 ]
console.log(arr.length); // 2
```

- 만약 현재 배열의 length 프로퍼티 값보다 큰 인덱스로 새로운 요소를 추가하면 희소 배열이 됨

```javascript
// 현재 배열의 length 프로퍼티 값보다 큰 인덱스로 새로운 요소를 추가하면 희소 배열이 된다.
arr[100] = 100;

console.log(arr); // [0, 1, empty × 98, 100]
console.log(arr.length); // 101
```

- 인덱스로 요소에 접근하여 명시적으로 값을 할당하지 않은 요소는 생성되지 않는다는 것에 주의

```javascript
// 명시적으로 값을 할당하지 않은 요소는 생성되지 않는다.
console.log(Object.getOwnPropertyDescriptors(arr));
/*
{
  '0': { value: 0, writable: true, enumerable: true, configurable: true },
  '1': { value: 1, writable: true, enumerable: true, configurable: true },
  '100': { value: 100, writable: true, enumerable: true, configurable: true },
  length: { value: 101, writable: true, enumerable: false, configurable: false }
*/
```

- 이미 요소가 존재하는 요소에 값을 재할당하면 요소값이 갱신

```javascript
// 요소값의 갱신
arr[1] = 10;

console.log(arr); // [0, 10, empty × 98, 100]
```

- 인덱스는 요소의 위치를 나타내므로 반드시 0 이상의 정수(또는 정수 형태의 문자열)를 사용
  - 만약 정수 이외의 값을 인덱스처럼 사용하면 요소가 생성되는 것이 아니라 프로퍼티가 생성
  - 이때 추가된 프로퍼티는 length 프로퍼티의 값에 영향을 주지 않음

```javascript
const arr = [];

// 배열 요소의 추가
arr[0] = 1;
arr['1'] = 2;

// 프로퍼티 추가
arr['foo'] = 3;
arr[1.1] = 4;
arr[-1] = 5;

console.log(arr); // [1, 2, foo: 3, 1.1: 4, -1: 5]

// 프로퍼티는 length에 영향을 주지 않는다.
console.log(arr.length); // 2
```



## 7. 배열 요소의 삭제

- 배열은 사실 객체이기 때문에 배열의 특정 요소를 삭제하기 위해 delete 연산자 사용 가능

```javascript
const arr = [1, 2, 3];

// 배열 요소의 삭제
delete arr[1];
console.log(arr); // [1, empty, 3]

// length 프로퍼티에 영향을 주지 않는다. 즉, 희소 배열이 된다.
console.log(arr.length); // 3
```

- delete 연산자는 객체의 프로퍼티를 삭제하므로 위 예제의 delete arr[1]은 arr에서 프로퍼티 키가 ‘1’인 프로퍼티를 삭제
  - 이때 배열은 희소 배열이 되며 length 프로퍼티 값은 변하지 않으므로 희소 배열을 만드는 delete 연산자는 사용하지 않는 것이 좋음

- 희소 배열을 만들지 않으면서 배열의 특정 요소를 완전히 삭제하려면 Array.prototype.splice 메소드를 사용

```javascript
const arr = [1, 2, 3];

// Array.prototype.splice(삭제를 시작할 인덱스, 삭제할 요소수)
// arr[1]부터 1개의 요소를 제거
arr.splice(1, 1);
console.log(arr); // [1, 3]

// length 프로퍼티에 변경이 반영된다.
console.log(arr.length); // 2
```



## 8. 배열 메소드

- 배열의 프로토타입인 Array.prototype는 배열을 다룰 때 필요한 메소드를 제공
  - 배열은 사용 빈도가 높은 자료 구조이므로 배열 메소드의 사용 방법을 잘 알아둘 것

- 배열 메소드는 결과물을 반환하는 패턴이 2가지이므로 주의가 필요
  - 배열에는 원본 배열(배열 메소드 내부에서 this가 가리키는 객체)을 을 직접 변경하는 메소드(mutator method)
  - 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메소드(accessor method)가 있음

```javascript
const arr = [1];

// push 메소드는 원본 배열, 즉 arr을 직접 변경한다.
arr.push(2);
console.log(arr); // [1, 2]

// concat 메소드는 원본 배열, 즉 arr을 직접 변경하지 않고 새로운 배열을 생성하여 반환한다.
const result = arr.concat([3]);
console.log(arr);    // [1, 2]
console.log(result); // [1, 2, 3]
```

- ES5부터 도입된 배열 메소드는 대부분 원본 배열을 직접 변경하지 않지만 초창기 배열 메소드는 원본 배열을 직접 변경하는 경우가 많음
- 원본 배열을 직접 변경하는 메소드는 외부 상태를 직접 변경하는 부수 효과(side effect)가 있으므로 사용에 주의
  - 가급적 원본 배열을 직접 변경하지 않는 메소드를 사용

### 8.1 Array.isArray

- Array.isArray는 Array 생성자 함수의 정적 메소드(“18.13 정적 프로퍼티/메소드” 참고)
  - Array.of, Array.from도 Array 생성자 함수의 정적 메소드

- Array.isArray는 주어진 인수가 배열이면 true, 배열이 아니면 false를 반환

```javascript
// true
Array.isArray([]);
Array.isArray([1, 2]);
Array.isArray(new Array());

// false
Array.isArray();
Array.isArray({});
Array.isArray(null);
Array.isArray(undefined);
Array.isArray(1);
Array.isArray('Array');
Array.isArray(true);
Array.isArray(false);
```

### 8.2 Array.prototype.push

- push 메소드는 인수로 전달받은 모든 값을 원본 배열(this)의 마지막 요소로 추가하고 변경된 length 값을 반환
  - push 메소드는 원본 배열(this)을 직접 변경

```javascript
const arr = [1, 2];

// 인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가하고 변경된 length 값을 반환한다.
let result = arr.push(3, 4);
console.log(result); // 4

// push 메소드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 2, 3, 4]
```

- push 메소드는 성능면에서 좋지 않음
  - push 메소드는 배열의 마지막에 요소를 추가하므로 length 프로퍼티를 사용하여 직접 요소를 추가할 수도 있는데, 이 방법이 push 메소드보다 빠름

```javascript
const arr = [1, 2];

// arr.push(3)와 동일한 처리를 한다. 이 방법이 push 메소드보다 빠르다.
arr[arr.length] = 3;

console.log(arr); // [1, 2, 3]
```

- push 메소드는 원본 배열(this)을 직접 변경하는 부수 효과가 있으므로 push 메소드보다는 ES6의 Spread 문법을 사용하는 편이 좋음
  - Spread 문법은 나중에 살펴볼 것

```javascript
const arr = [1, 2];

// ES6 Spread 문법
const newArr = [...arr, 3];

console.log(newArr); // [1, 2, 3]
```

### 8.3 Array.prototype.pop

- pop 메소드는 원본 배열(this)에서 마지막 요소를 제거하고 제거한 요소를 반환
  - 원본 배열(this)이 빈 배열이면 undefined를 반환
  - pop 메소드는 원본 배열(this)을 직접 변경

```javascript
const arr = [1, 2];

// 원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환한다.
let result = arr.pop();
console.log(result); // 2

// pop 메소드는 원본 배열을 직접 변경한다.
console.log(arr); // [1]
```

- pop 메소드와 push 메소드를 사용하면 스택을 쉽게 구현
  - 스택(stack)은 데이터를 마지막에 밀어 넣고, 마지막에 밀어 넣은 데이터를 먼저 꺼내는 후입 선출(LIFO - Last In First Out) 방식의 자료 구조
  - 스택은 언제나 가장 마지막에 밀어 넣은 최신 데이터를 취득
    - 스택에 데이터를 밀어 넣는 것을 푸시(push)라 하고 스택에서 데이터를 꺼내는 것을 팝(pop)

```javascript
const Stack = (function () {
  function Stack(array = []) {
    if (!Array.isArray(array)) {
      throw new TypeError(`${array} is not an array.`);
    }
    this.array = array;
  }

  // 스택의 가장 마지막에 데이터를 밀어 넣는다.
  Stack.prototype.push = function (value) {
    return this.array.push(value);
  };

  // 스택의 가장 마지막 데이터, 즉 가장 나중에 밀어 넣은 최신 데이터를 꺼낸다.
  Stack.prototype.pop = function () {
    return this.array.pop();
  };

  return Stack;
}());

const stack = new Stack([1, 2]);
console.log(stack); // [1, 2]

stack.push(3);
console.log(stack); // [1, 2, 3]

stack.pop(); // -> 3
console.log(stack); // [1, 2]
```

### 8.4 Array.prototype.unshift

-  unshift 메소드는 인수로 전달받은 모든 값을 원본 배열(this)의 선두에 요소로 추가하고 변경된 length 값을 반환
  - unshift 메소드는 원본 배열(this)을 직접 변경

```javascript
const arr = [1, 2];

// 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 변경된 length 값을 반환한다.
let result = arr.unshift(3, 4);
console.log(result); // 4

// unshift 메소드는 원본 배열을 직접 변경한다.
console.log(arr); // [3, 4, 1, 2]
```

- unshift 메소드는 원본 배열(this)을 직접 변경하는 부수 효과가 있으므로 unshift 메소드보다는 ES6의 Spread 문법을 사용하는 편이 좋음

```javascript
const arr = [1, 2];

// ES6 Spread 문법
const newArr = [3, ...arr];

console.log(newArr); // [3, 1, 2]
```

### 8.5 Array.prototype.shift

- shift 메소드는 원본 배열(this)에서 첫번째 요소를 제거하고 제거한 요소를 반환
  - 원본 배열(this)이 빈 배열이면 undefined를 반환하므로 shift 메소드는 원본 배열(this)을 직접 변경

```javascript
const arr = [1, 2];

// 원본 배열에서 첫번째 요소를 제거하고 제거한 요소를 반환한다.
let result = arr.shift();
console.log(result); // 1

// shift 메소드는 원본 배열을 직접 변경한다.
console.log(arr); // [2]
```

- shift 메소드와 push 메소드를 사용하면 큐를 쉽게 구현
  - 큐(queue)는 데이터를 마지막에 밀어 넣고, 처음 데이터, 즉 가장 먼저 밀어 넣은 데이터를 먼저 꺼내는 선입 선출(FIFO - First In First Out) 방식의 자료 구조
  - 스택은 언제나 마지막에 밀어 넣은 최신 데이터를 취득하지만 큐는 언제나 데이터를 밀어 넣은 순서대로 취득

```javascript
const Queue = (function () {
  function Queue(array = []) {
    if (!Array.isArray(array)) {
      throw new TypeError(`${array} is not an array.`);
    }
    this.array = array;
  }

  // 큐의 가장 마지막에 데이터를 밀어 넣는다.
  Queue.prototype.push = function (value) {
    return this.array.push(value);
  };

  // 큐의 가장 처음 데이터, 즉 가장 먼저 밀어 넣은 데이터를 꺼낸다.
  Queue.prototype.shift = function () {
    return this.array.shift();
  };

  return Queue;
}());

const queue = new Queue([1, 2]);
console.log(queue); // [1, 2]

queue.push(3);
console.log(queue); // [1, 2, 3]

queue.shift(); // -> 1
console.log(queue); // [2, 3]
```

### 8.6 Array.prototype.concat

- concat 메소드는 인수로 전달된 값들(배열 또는 값)을 원본 배열(this)의 마지막 요소로 추가한 새로운 배열을 반환
  - 인수로 전달한 값이 배열인 경우, 배열을 해체하여 새로운 배열의 요소로 추가한다. 원본 배열(this)은 변경되지 않음

```javascript
const arr1 = [1, 2];
const arr2 = [3, 4];

// 배열 arr2를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환
// 인수로 전달한 값이 배열인 경우, 배열을 해체하여 새로운 배열의 요소로 추가한다.
let result = arr1.concat(arr2);
console.log(result); // [1, 2, 3, 4]

// 숫자를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환
result = arr1.concat(3);
console.log(result); // [1, 2, 3]

//  배열 arr2와 숫자를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환
result = arr1.concat(arr2, 5);
console.log(result); // [1, 2, 3, 4, 5]

// 원본 배열은 변경되지 않는다.
console.log(arr1); // [1, 2]
```

- push와 unshift 메소드는 concat 메소드로 대체할 수 있는데, push와 unshift 메소드는 concat 메소드는 유사하게 동작하지만 미묘한 차이 존재
  - push와 unshift 메소드는 원본 배열(this)을 직접 변경하지만 concat 메소드는 원본 배열(this)을 변경하지 않고 새로운 배열을 반환
  - 따라서 push와 unshift 메소드를 사용할 경우, 원본 배열을 반드시 변수에 할당해야 하며 concat 메소드를 사용할 경우, 반환값을 반드시 변수에 할당 받아야 함

```javascript
const arr1 = [3, 4];

// unshift 메소드는 원본 배열을 직접 변경한다.
arr1.unshift(1, 2);
// unshift 메소드를 사용할 경우, 원본 배열을 반드시 변수에 할당해야 결과를 확인할 수 있다.
console.log(arr1); // [1, 2, 3, 4]

// push 메소드는 원본 배열을 직접 변경한다.
arr1.push(5, 6);
// push 메소드를 사용할 경우, 원본 배열을 반드시 변수에 할당해야 결과를 확인할 수 있다.
console.log(arr1); // [1, 2, 3, 4, 5, 6]

// unshift와 push 메소드는 concat 메소드로 대체할 수 있다.
const arr2 = [3, 4];

// concat 메소드는 원본 배열을 변경하지 않고 새로운 배열을 반환한다.
// arr1.unshift(1, 2)를 아래와 같이 대체할 수 있다.
let result = [1, 2].concat(arr2);
console.log(result); // [1, 2, 3, 4]

// arr1.push(5, 6)를 아래와 같이 대체할 수 있다.
result = result.concat(5, 6);
console.log(result); // [1, 2, 3, 4, 5, 6]
```

- 인수로 전달받은 값이 배열인 경우, push와 unshift 메소드는 배열을 그대로 원본 배열(this)의 마지막/첫번째 요소로 추가하지만 concat 메소드는 인수로 전달받은 배열을 해체하여 새로운 배열의 마지막 요소로 추가

```javascript
const arr = [3, 4];

// unshift와 push 메소드는 인수로 전달받은 배열을 그대로 원본 배열의 요소로 추가한다
arr.unshift([1, 2]);
arr.push([5, 6]);
console.log(arr); // [[1, 2], 3, 4,[5, 6]]

// concat 메소드는 인수로 전달받은 배열을 해체하여 새로운 배열의 요소로 추가한다
let result = [1, 2].concat([3, 4]);
result = result.concat([5, 6]);

console.log(result); // [1, 2, 3, 4, 5, 6]
```

- concat 메소드는 ES6의 Spread 문법으로 대체 가능

```javascript
let result = [1, 2].concat([3, 4]);
console.log(result); // [1, 2, 3, 4]

// concat 메소드는 ES6의 Spread 문법으로 대체할 수 있다.
result = [...[1, 2], ...[3, 4]];
console.log(result); // [1, 2, 3, 4]
```

- 결론적으로 push/unshif 메소드와 concat 메소드를 사용하는 대신 ES6의 Spread 문법을 일관성 있게 사용하는 것을 추천

## 8.7 Array.prototype.splice

- push, pop, unshift, shift 메소드는 모두 원본 배열(this)을 직접 변경하는 메소드(mutator method)이며 원본 배열의 처음이나 마지막에 요소를 추가하거나 제거

![](https://poiemaweb.com/assets/fs-images/26-3.png)

- 원본 배열(this)의 중간에 요소를 추가하거나 중간에 있는 요소를 제거하는 경우, splice 메소드를 사용할 수 있는데, splice 메소드는 3개의 매개변수가 있으며 원본 배열을 직접 변경
  - start : 원본 배열의 요소를 제거하기 시작할 인덱스로, start 만을 지정하면 원본 배열의 start부터 모든 요소를 제거
  - deleteCount : 원본 배열의 요소를 제거하기 시작할 인덱스인 start부터 제거할 요소의 개수로, deleteCount가 0인 경우, 아무런 요소도 제거되지 않음 (옵션)
  - items : 제거한 위치에 삽입될 요소들의 목록. 생략할 경우, 원본 배열에서 지정된 요소들을 제거 (옵션)

```javascript
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 2개의 요소를 제거하고 그 자리에 새로운 요소 20, 30을 삽입한다.
const result = arr.splice(1, 2, 20, 30);

// 제거한 요소가 배열로 반환된다.
console.log(result); // [2, 3]
// splice 메소드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 20, 30, 4]
```

- splice 메소드에 3개의 인수를 빠짐없이 전달하면
  - 첫번째 인수, 즉 시작 인덱스부터
  - 두번째 인수, 즉 제거할 요소의 개수만큼 원본 배열에서 요소를 제거
  - 세번째 인수, 즉 제거한 위치에 삽입할 요소들을 삽입

![](https://poiemaweb.com/assets/fs-images/26-4.png)

- splice 메소드의 두번째 인수, 즉 제거할 요소의 개수를 0으로 지정하면 아무런 요소도 제거하지 않고 새로운 요소들을 삽입

```javascript
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 0개의 요소를 제거하고 그 자리에 새로운 요소 100을 삽입한다.
const result = arr.splice(1, 0, 100);

// 원본 배열이 변경된다.
console.log(arr); // [1, 100, 2, 3, 4]
// 제거한 요소가 배열로 반환된다.
console.log(result); // []
```

- splice 메소드의 세번째 인수, 즉 제거한 위치에 추가할 요소들의 목록을 전달하지 않으면 원본 배열에서 지정된 요소만을 제거

```javascript
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 2개의 요소를 제거한다.
const result = arr.splice(1, 2);

// 원본 배열이 변경된다.
console.log(arr); // [1, 4]
// 제거한 요소가 배열로 반환된다.
console.log(result); // [2, 3]
```

![](https://poiemaweb.com/assets/fs-images/26-5.png)

- splice 메소드의 두번째 인수, 즉 제거할 요소의 개수를 생략하면 첫번째 인수로 전달된 시작 인덱스부터 모든 요소를 제거

```javascript
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 모든 요소를 제거한다.
const result = arr.splice(1);

// 원본 배열이 변경된다.
console.log(arr); // [1]
// 제거한 요소가 배열로 반환된다.
console.log(result); // [2, 3, 4]
```

### 8.8 Array.prototype.slice

- slice 메소드는 인수로 전달된 범위의 요소들을 복사하여 반환하며, 원본 배열은 변경되지 않고 2개의 매개변수를 가짐
  - start : 복사를 시작할 인덱스로 음수인 경우, 배열의 끝에서의 인덱스를 나타냄
    - 예를 들어 slice(-2)는 배열의 마지막 2개의 요소를 반환
  - end : 복사를 종료할 인덱스로 이 인덱스에 해당하는 요소는 복사되지 않음
    - 옵션이며 기본값은 length 값

```javascript
const arr = [1, 2, 3];

// arr[0]부터 arr[1] 이전(arr[1] 미포함)까지 복사하여 반환한다.
let result = arr.slice(0, 1);
console.log(result); // [1]

// arr[1]부터 arr[2] 이전(arr[2] 미포함)까지 복사하여 반환한다.
result = arr.slice(1, 2);
console.log(result); // [2]

// 원본은 변경되지 않는다.
console.log(arr); // [1, 2, 3]
```

- slice 메소드는 첫번째 매개변수 start에 해당하는 인덱스를 갖는 요소부터 매개변수 end에 해당하는 인덱스를 가진 요소 이전(end 미포함)까지 요소들을 복사하여 반환

![](https://poiemaweb.com/assets/fs-images/26-6.png)

- slice 메소드의 두번째 인수를 생략하면 첫번째 인수에 해당하는 인덱스부터 모든 요소를 복사하여 반환

```javascript
const arr = [1, 2, 3];

// arr[1]부터 이후의 모든 요소를 복사하여 반환한다.
const result = arr.slice(1);
console.log(result); // [2, 3]

slice 메소드의 첫번째 인수가 음수인 경우, 배열의 끝부터 요소를 복사하여 반환한다.
const arr = [1, 2, 3];

// 전달된 인수가 음수인 경우, 배열의 끝부터 요소를 복사하여 반환한다.
let result = arr.slice(-1);
console.log(result); // [3]

result = arr.slice(-2);
console.log(result); // [2, 3]
```

- slice 메소드의 인수를 모두 생략하면 원본 배열의 새로운 복사본을 생성하여 반환

```javascript
const arr = [1, 2, 3];

// 인수를 생략하면 원본 배열의 복사본을 생성하여 반환한다.
const copy = arr.slice();
console.log(copy); // [1, 2, 3]
console.log(copy === arr); // false

이때 생성된 복사본은 얕은 복사(shallow copy)를 통해 생성된다.
const todos = [
  { id: 1, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'Javascript', completed: false }
];

// 앝은 복사(shallow copy)
const _todos = todos.slice();
// const _todos = [...todos];

// _todos와 todos는 참조값이 다른 별개의 객체를 가리킨다.
console.log(_todos === todos); // false

// 배열의 요소는 참조값이 같다. 즉, 얕은 복사되었다.
console.log(_todos[0] === todos[0]); // true
```

- 얕은 복사와 깊은 복사
  - Spread 문법과 Object.assign 메소드는 원본을 앝은 복사(shallow copy)
  - 깊은 복사(deep copy)를 위해서는 lodash의 deepClone을 사용하는 것을 추천
- slice 메소드가 복사본을 생성하는 것을 이용하여 arguments, HTMLCollection, NodeList와 같은 유사 배열 객체(Array-like Object)를 배열로 변환 가능

```javascript
function sum() {
  // 유사 배열 객체를 배열로 변환(ES5)
  var arr = Array.prototype.slice.call(arguments);
  console.log(arr); // [1, 2, 3]

  return arr.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum(1, 2, 3)); // 6
```

- ES6의 Spread 문법을 사용하면 보다 간단하게 유사 배열 객체(Array-like Object)를 배열로 변환

```javascript
function sum() {
  // 유사 배열 객체를 배열로 변환(ES6 Spread 문법)
  const arr = [...arguments ];
  console.log(arr); // [1, 2, 3]

  return arr.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2, 3)); // 6
```

### 8.9 Array.prototype.indexOf

- indexOf 메소드는 원본 배열(this)에서 인수로 전달된 요소를 검색하여 인덱스를 반환
  - 중복되는 요소가 있는 경우, 첫번째 인덱스를 반환
  - 해당하는 요소가 없는 경우, -1을 반환

```javascript
const arr = [1, 2, 2, 3];

// 배열 arr에서 요소 2를 검색하여 첫번째 인덱스를 반환
arr.indexOf(2);    // -> 1
// 배열 arr에서 요소 4가 없으므로 -1을 반환
arr.indexOf(4);    // -1
// 두번째 인수는 검색을 시작할 인덱스이다. 두번째 인수를 생략하면 처음부터 검색한다.
arr.indexOf(2, 2); // 2
```

- indexOf 메소드는 배열에 요소가 존재하는지 확인할 때 유용

```javascript
const foods = ['apple', 'banana', 'orange'];

// foods 배열에 'orange' 요소가 존재하는지 확인
if (foods.indexOf('orange') === -1) {
  // foods 배열에 'orange' 요소가 존재하지 않으면 'orange' 요소를 추가
  foods.push('orange');
}

console.log(foods); // ["apple", "banana", "orange"]
```

- indexOf 메소드 대신 ES7에서 새롭게 도입된 Array.prototype.includes 메소드를 사용하면 보다 가독성이 좋음

```javascript
const foods = ['apple', 'banana'];

// ES7: Array.prototype.includes
// foods 배열에 'orange' 요소가 존재하는지 확인
if (!foods.includes('orange')) {
  // foods 배열에 'orange' 요소가 존재하지 않으면 'orange' 요소를 추가
  foods.push('orange');
}

console.log(foods); // ["apple", "banana", "orange"]
```

### 8.10 Array.prototype.join

- join 메소드는 원본 배열(this)의 모든 요소를 문자열로 변환한 후, 인수로 전달받은 값, 즉 구분자(separator)로 연결한 문자열을 반환
  - 구분자는 생략 가능하며 기본 구분자는 ','

```javascript
const arr = [1, 2, 3, 4];

// 기본 구분자는 ','이다.
// 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 기본 구분자 ','로 연결한 문자열을 반환
let result = arr.join();
console.log(result); // '1,2,3,4';

// 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 빈문자열로 연결한 문자열을 반환
result = arr.join('');
console.log(result); // '1234'

// 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 구분자 ':'로 연결한 문자열을 반환
result = arr.join(':');
console.log(result); // '1:2:3:4'
```

### 8.11 Array.prototype.reverse

- reverse 메소드는 원본 배열(this)의 요소 순서를 반대로 변경
  - 이때 원본 배열이 변경. 반환값은 변경된 배열

```javascript
const arr = [1, 2, 3];
const result = arr.reverse();

// reverse 메소드는 원본 배열을 직접 변경한다.
console.log(arr); // [3, 2, 1]
// 반환값은 변경된 배열이다.
console.log(result); // [3, 2, 1]
```

### 8.12 Array.prototype.fill

- ES6에서 새롭게 도입된 fill 메소드는 인수로 전달 받은 값을 요소로 배열의 처음부터 끝까지 채우며, 원본 배열이 변경

```javascript
const arr = [1, 2, 3];

// 인수로 전달 받은 값 0을 요소로 배열의 처음부터 끝까지 채운다.
arr.fill(0);

// fill 메소드는 원본 배열을 직접 변경한다.
console.log(arr); // [0, 0, 0]
```

- 두번째 인수로 요소 채우기를 시작할 인덱스를 전달할 수 있음

```javascript
const arr = [1, 2, 3];

// 인수로 전달 받은 값 0를 요소로 배열의 인덱스 1부터 끝까지 채운다.
arr.fill(0, 1);

// fill 메소드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 0, 0]
```

- 세번째 인수로 요소 채우기를 멈출 인덱스를 전달할 수 있음

```javascript
const arr = [1, 2, 3, 4, 5];

// 인수로 전달 받은 값 0를 요소로 배열의 인덱스 1부터 3 이전(인덱스 3 미포함)까지 채운다.
arr.fill(0, 1, 3);

// fill 메소드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 0, 0, 4, 5]
```

- fill 메소드를 사용하면 배열을 생성하면서 특정 값으로 요소를 채울 수 있음

```javascript
const arr = new Array(3);
console.log(arr); // [empty × 3]

// 인수로 전달 받은 값 1을 요소로 배열의 처음부터 끝까지 채운다.
const result = arr.fill(1);

// fill 메소드는 원본 배열을 직접 변경한다.
console.log(result); // [1, 1, 1]
// fill 메소드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 1, 1]
```

- fill 메소드로 요소를 채울 경우, 모든 요소를 하나의 값만으로 채울 수 밖에 없다는 단점
- Array.from을 사용하면 두번째 인수로 전달한 함수를 통해 값을 만들면서 요소를 채울 수 있음
  - 두번째 인수로 전달한 함수는 첫번째 인수에 의해 생성된 배열의 요소값과 인덱스를 순차적으로 전달받아 새로운 요소를 생성

```javascript
// 인수로 전달받은 정수만큼 요소를 생성하고 0부터 1씩 증가하면 요소를 채운다.
function generateSequences(length = 0) {
  return Array.from(new Array(length), (v, i) => i);
}

console.log(generateSequences(3)); // [0, 1, 2]
```

### 8.13 Array.prototype.includes

- ES7에서 새롭게 도입된 includes 메소드는 배열 내에 특정 요소가 포함되어 있는지 확인하여 true 또는 false를 반환
  - 두번째 인수로 검색을 시작할 인덱스를 전달할 수 있음

```javascript
const arr = [1, 2, 3];

// 배열에 요소 2가 포함되어 있는지 확인한다.
let result = arr.includes(2);
console.log(result); // true

// 배열에 요소 100이 포함되어 있는지 확인한다.
result = arr.includes(100);
console.log(result); // false

// 두번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.
// 배열에서 요소 1가 포함되어 있는지 인덱스 1부터 확인한다.
result = arr.includes(1, 1);
console.log(result); // false
```

- 배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환하는 indexOf 메소드를 사용하여도 배열 내에 특정 요소가 포함되어 있는지 확인할 수 있지만 indexOf 메소드는 결과값 -1을 비교해 보아야 하고 배열에 NaN이 포함되어 있는지 확인할 수 없는 문제

```javascript
console.log([NaN].indexOf(NaN) !== -1); // false
console.log([NaN].includes(NaN)); // true
```



## 9. 배열 고차 함수

- 고차 함수(High Order Function, HOF)는 함수를 인자로 전달받거나 함수를 반환하는 함수
- 다시 말해, 고차 함수는 인자로 받은 함수를 필요한 시점에 호출하거나 클로저를 생성하여 반환

- 자바스크립트의 함수는 일급 객체이므로 값처럼 인자로 전달할 수 있으며 반환할 수도 있음
- 함수를 인수로 전달받고 클로저를 반환하는 고차 함수 예제

```javascript
// 고차 함수 makeCounter는 함수를 인수로 전달받고 클로저를 반환한다.
function makeCounter(predicate) {
  // 자유 변수. num의 상태는 유지되어야 한다.
  let num = 0;
  // num의 상태를 유지하는 클로저를 반환한다.
  return function () {
    // predicate를 호출하여 자유 변수 num의 상태를 변화시킨다.
    num = predicate(num);
    return num;
  };
}

// 보조 함수
function increase(n) {
  return ++n;
}

// 보조 함수
function decrease(n) {
  return --n;
}

// 고차 함수 makeCounter는 함수를 인수로 전달받고 클로저를 반환한다.
const increaser = makeCounter(increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

// 고차 함수 makeCounter는 함수를 인수로 전달받고 클로저를 반환한다.
const decreaser = makeCounter(decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

- 고차 함수는 외부 상태 변경이나 가변(mutable) 데이터를 피하고 불변성(Immutability)을 지향하는 함수형 프로그래밍에 기반

  - 함수형 프로그래밍은 순수 함수(Pure function)와 보조 함수의 조합을 통해 로직 내에 존재하는 조건문과 반복문을 제거하여 복잡성을 해결하고 변수의 사용을 억제하여 상태 변경을 피하려는 프로그래밍 패러다임
  - 조건문이나 반복문은 로직의 흐름을 이해하기 어렵게 하여 가독성을 해치고, 변수의 값은 누군가에 의해 언제든지 변경될 수 있어 오류 발생의 근본적 원인이 될 수 있음
  - 함수형 프로그래밍은 결국 순수 함수를 통해 부수 효과(Side effect)를 최대한 억제하여 오류를 피하고 프로그램의 안정성을 높이려는 노력의 한 방법

  - 자바스크립트는 고차 함수를 다수 지원하는데, 특히 배열은 매우 유용한 고차 함수를 제공함
    - 이들 배열 고차 함수는 매우 활용도가 높으므로 사용 방법에 대해 잘 이해할 것

### 9.1 Array.prototype.sort

- sort 메소드는 배열의 요소를 적절하게 정렬하고 원본 배열을 직접 변경하며 정렬된 배열을 반환
- sort 메소드는 기본적으로 오름차순으로 요소를 정렬

```javascript
const fruits = ['Banana', 'Orange', 'Apple'];

// 오름차순(ascending) 정렬
fruits.sort();

// sort 메소드는 원본 배열을 직접 변경한다.
console.log(fruits); // ['Apple', 'Banana', 'Orange']
```

- 한글 문자열인 요소도 오름차순으로 정렬

```javascript
const fruits = ['바나나', '오렌지', '사과'];

// 오름차순(ascending) 정렬
fruits.sort();

// sort 메소드는 원본 배열을 직접 변경한다.
console.log(fruits); // ['바나나', '사과', '오렌지']
```

- sort 메소드는 기본적으로 오름차순으로 요소를 정렬하므로 내림차순으로 요소를 정렬하려면 sort 메소드로 오름차순으로 정렬한 후, reverse 메소드를 사용하여 요소의 순서를 뒤집음

```javascript
const fruits = ['Banana', 'Orange', 'Apple'];

// 오름차순(ascending) 정렬
fruits.sort();

// sort 메소드는 원본 배열을 직접 변경한다.
console.log(fruits); // ['Apple', 'Banana', 'Orange']

// 내림차순(descending) 정렬
fruits.reverse();

// reverse 메소드도 원본 배열을 직접 변경한다.
console.log(fruits); // ['Orange', 'Banana', 'Apple']
```

- 문자열 요소들로 이루어진 배열의 정렬은 아무런 문제가 없지만 숫자 요소들로 이루어진 배열을 정렬할 때는 주의가 필요

```javascript
const points = [40, 100, 1, 5, 2, 25, 10];

points.sort();

// 숫자 요소들로 이루어진 배열은 의도한 대로 정렬되지 않는다.
console.log(points); // [1, 10, 100, 2, 25, 40, 5]
```

- sort 메소드의 기본 정렬 순서는 문자열 Unicode 코드 포인트 순서를 따름
- 배열의 요소가 숫자 타입이라 할지라도 배열의 요소를 일시적으로 문자열로 변환한 후, 정렬
  - 예를 들어, 문자열 ‘1’의 Unicode 코드 포인트 순서가 문자열 ‘2’의 Unicode 코드 포인트 순서보다 앞서므로 1과 2를 sort 메소드로 정렬하면 1이 2보다 앞으로 정렬
  - 하지만 문자열 ‘10’의 Unicode 코드 포인트는 U+0031U+0030이므로 2와 10를 sort 메소드로 정렬하면 10이 2보다 앞으로 정렬

- 따라서 숫자 요소를 정렬하기 위해서는 sort 메소드에 정렬 순서를 정의하는 비교 함수를 인수로 전달
- 비교 함수를 생략하면 배열의 각 요소는 일시적으로 문자열로 변환되어 Unicode 코드 포인트 순서에 따라 정렬

```javascript
const points = [40, 100, 1, 5, 2, 25, 10];

// 숫자 배열 오름차순 정렬
// 비교 함수의 반환값이 0보다 작은 경우, a를 우선하여 정렬한다.
points.sort(function (a, b) { return a - b; });
// ES6 화살표 함수
// points.sort((a, b) => a - b);
console.log(points); // [1, 2, 5, 10, 25, 40, 100]

// 숫자 배열에서 최소값 취득
console.log(points[0]); // 1

// 숫자 배열 내림차순 정렬
// 비교 함수의 반환값이 0보다 큰 경우, b를 우선하여 정렬한다.
points.sort(function (a, b) { return b - a; });
// ES6 화살표 함수
// points.sort((a, b) => b - a);
console.log(points); // [100, 40, 25, 10, 5, 2, 1]

// 숫자 배열에서 최대값 취득
console.log(points[0]); // 100
```

- 객체를 요소로 갖는 배열을 정렬하는 예제

```javascript
const todos = [
  { id: 4, content: 'JavaScript' },
  { id: 1, content: 'HTML' },
  { id: 2, content: 'CSS' }
];

// 비교 함수
function compare(key) {
  return function (a, b) {
    // 프로퍼티 값이 문자열인 경우, - 산술 연산으로 비교하면 NaN이 나오므로 비교 연산을 사용한다.
    return a[key] > b[key] ? 1 : (a[key] < b[key] ? -1 : 0);
  };
}

// id를 기준으로 정렬
todos.sort(compare('id'));
console.log(todos);
/*
[
  { id: 1, content: 'HTML' },
  { id: 2, content: 'CSS' },
  { id: 4, content: 'JavaScript' }
]
*/

// content를 기준으로 정렬
todos.sort(compare('content'));
console.log(todos);
/*
[
  { id: 2, content: 'CSS' },
  { id: 1, content: 'HTML' },
  { id: 4, content: 'JavaScript' }
]
*/
```

- Array.prototype.sort 메서드의 알고리즘
  - Array.prototype.sort 메서드는 10개 이상의 요소가 있는 배열을 정렬할 때 불안정하다고 알려진 quicksort 알고리즘을 사용
  - ECMAScript 2019(ES10)에서는 배열이 올바르게 정렬되도록 Timsort 알고리즘을 적용하도록 변경

### 9.2 Array.prototype.forEach

- 함수형 프로그래밍은 순수 함수(Pure function)와 보조 함수의 조합을 통해 로직 내에 존재하는 조건문과 반복문을 제거하여 복잡성을 해결하고 변수의 사용을 억제하여 상태 변경을 피하려는 프로그래밍 패러다임

  - 조건문이나 반복문은 로직의 흐름을 이해하기 어렵게 하는데, 특히 for 문은 반복을 위한 변수를 선언해야 하며 증감식과 조건식으로 이루어져 있어서 함수형 프로그래밍이 추구하는 바와 맞지 않음

  - forEach 메소드는 for 문을 대체할 수 있는 메소드로 배열을 순회하며 배열의 각 요소에 대하여 인자로 주어진 콜백 함수를 실행

```javascript
const numbers = [1, 2, 3];
let pows = [];

// for 문으로 순회
for (let i = 0; i < numbers.length; i++) {
  pows.push(numbers[i] ** 2);
}

console.log(pows); // [1, 4, 9]

pows = [];

// forEach 메소드로 순회
numbers.forEach(item => pows.push(item ** 2));

console.log(pows); // [1, 4, 9]
```

- forEach 메소드의 콜백 함수는 요소값, 인덱스, forEach 메소드를 호출한 배열, 즉 this를 전달 받을 수 있음

```javascript
// forEach 메소드는 전달받은 콜백 함수를 호출하면서 3개(요소값, 인덱스, this)의 인수를 전달한다.
[1, 2, 3].forEach((item, index, self) => {
  console.log(`요소값: ${item}, 인덱스: ${index}, this: ${self}`);
});
/*
요소값: 1, 인덱스: 0, this: 1,2,3
요소값: 2, 인덱스: 1, this: 1,2,3
요소값: 3, 인덱스: 2, this: 1,2,3
*/
```

- forEach 메소드는 원본 배열(this)을 변경하지 않지만 콜백 함수가 원본 배열(this)을 변경할 수는 있음

```javascript
const numbers = [1, 2, 3];

// forEach 메소드는 원본 배열(this)을 변경하지 않는다.
// 하지만 콜백 함수가 원본 배열(this)을 변경할 수는 있다.
// 원본 배열을 직접 변경하려면 콜백 함수의 3번째 인자(this)를 사용한다.
numbers.forEach((item, index, self) => self[index] = Math.pow(item, 2));

console.log(numbers); // [1, 4, 9]
```

- forEach 메소드의 반환값은 언제나 undefined

```javascript
const result = [1, 2, 3].forEach(console.log);

console.log(result); // undefined
```

- forEach 메소드의 동작을 이해하기 위해 forEach 메소드의 폴리필을 살펴보자

```javascript
// 만약 Array.prototype에 forEach 메소드가 존재하지 않으면 폴리필을 추가한다.
if (!Array.prototype.forEach) {
  Array.prototype.forEach = function (callback, thisArg) {
    // 전달받은 첫번째 인수가 함수가 아니면 TypeError를 발생시킨다.
    if (typeof callback !== 'function') {
      throw new TypeError(callback + ' is not a function');
    }

    // this로 사용할 두번째 인수가 전달받지 못하면 this로 전역 객체를 사용한다.
    thisArg = thisArg || window;

    // for 문으로 배열을 순회하면서 콜백 함수를 호출한다.
    for (var i = 0; i < this.length; i++) {
      // call 메소드를 통해 두번째 인수가 전달받은 this를 전달하면서 콜백 함수를 호출한다.
      // 이때 콜백 함수의 인수로 배열 요소, 인덱스, 배열 자신을 전달한다.
      callback.call(thisArg, this[i], i, this);
    }
  };
}
```

- 이처럼 forEach 메소드도 내부에서는 반복문(for 문)을 통해 배열을 순회할 수 밖에 없음
  - 단, 반복문을 메소드 내부로 은닉하여 로직의 흐름을 이해하기 쉽게 하고 복잡성을 해결

- forEach 메소드는 for 문과는 달리 break, continue 문을 사용할 수 없음
  - 다시 말해, 배열의 모든 요소를 빠짐없이 모두 순회하며 중간에 순회를 중단할 수 없음

```javascript
// forEach 메소드는 for 문과는 달리 break 문을 사용할 수 없다.
[1, 2, 3].forEach(function (item) {
  console.log(item);
  if (item > 1) break; // SyntaxError: Illegal break statement
});

// forEach 메소드는 for 문과는 달리 continue 문을 사용할 수 없다.
[1, 2, 3].forEach(function (item) {
  console.log(item);
  if (item > 1) continue;
  // SyntaxError: Illegal continue statement: no surrounding iteration statement
});
```

- 희소 배열의 존재하지 않는 요소는 순회 대상에서 제외
  - 이는 앞으로 살펴볼 배열을 순회하는 map, filter, reduce 메소드 등에서도 마찬가지

```javascript
// 희소 배열
const arr = [1, , 3];

// for 문으로 희소 배열을 순회
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]); // 1, undefined, 3
}

// forEach 메소드는 희소 배열의 존재하지 않는 요소를 순회 대상에서 제외한다.
arr.forEach(v => console.log(v)); // 1, 3
```

- forEach 메소드는 for 문에 비해 성능이 좋지는 않지만 for 문보다 가독성이 좋으므로 높은 성능이 필요한 경우를 제외하고 for 문 대신 forEach 메소드의 사용을 권장
  - forEach 메소드에 두번째 인자로 forEach 메소드 내부에서 this로 사용될 객체를 전달할 수 있음

```javascript
class Numbers {
  numberArray = [];

  multiply(arr) {
    arr.forEach(function (item) {
      // 일반 함수로 호출되는 콜백 함수 내부의 this는 전역 객체를 가리킨다.
      // TypeError: Cannot read property 'numberArray' of undefined
      this.numberArray.push(item * item);
    });
  }
}

const numbers = new Numbers();
numbers.multiply([1, 2, 3]);
console.log(numbers.numberArray);
```

- forEach 메소드의 콜백 함수는 일반 함수로 호출되므로 콜백 함수 내부의 this는 전역 객체를 가리킴
  - 콜백 함수 내부의 this와 multiply 메소드 내부의 this를 일치시키려면 forEach 메소드에 두번째 인자로 forEach 메소드 내부에서 this로 사용될 객체를 전달

```javascript
class Numbers {
  numberArray = [];

  multiply(arr) {
    arr.forEach(function (item) {
      // 외부에서 this를 전달하지 않으면 this는 전역 객체를 가리킨다.
      this.numberArray.push(item * item);
    }, this); // forEach 메소드 내부에서 this로 사용될 객체를 전달
  }
}

const numbers = new Numbers();
numbers.multiply([1, 2, 3]);
console.log(numbers.numberArray); // [1, 4, 9]
```

- 보다 나은 방법은 ES6의 화살표 함수를 사용하는 것

```javascript
class Numbers {
  numberArray = [];

  multiply(arr) {
    // 화살표 함수 내부에서 this를 참조하면
    // 상위 컨텍스트, 즉 multiply 메소드 내부의 this를 그대로 참조한다.
    arr.forEach(item => this.numberArray.push(item * item));
  }
}

const numbers = new Numbers();
numbers.multiply([1, 2, 3]);
console.log(numbers.numberArray); // [1, 4, 9]
```

### 9.3 Array.prototype.map

- map 메소드는 배열을 순회하며 배열의 각 요소에 대하여 인자로 주어진 콜백 함수를 실행
  - 그리고 콜백 함수의 반환한 값들이 요소로서 추가된 새로운 배열을 반환하는데 이때 원본 배열은 변경되지 않음

```javascript
const numbers = [1, 4, 9];

// 배열을 순회하며 배열의 각 요소에 대하여 인자로 주어진 콜백 함수를 실행한다.
// 그리고 콜백 함수의 반환한 값들이 요소로서 추가된 새로운 배열을 반환한다.
const roots = numbers.map(item => Math.sqrt(item));

// 위 코드의 축약표현은 아래와 같다.
// const roots = numbers.map(Math.sqrt);

// map 메소드는 새로운 배열을 반환한다
console.log(roots);   // [ 1, 2, 3 ]
// map 메소드는 원본 배열은 변경하지 않는다
console.log(numbers); // [ 1, 4, 9 ]
```

- forEach 메소드는 배열을 순회하며 요소 값을 참조하여 무언가를 하기 위한 함수이며 map 메소드는 배열을 순회하며 요소 값을 다른 값으로 맵핑하기 위한 함수
  - 따라서 map 메소드가 생성하여 반환하는 새로운 배열의 length는 map 메소드를 호출한 배열, 즉 this의 length와 반드시 일치

![](https://poiemaweb.com/assets/fs-images/26-7.png)

- forEach 메소드와 마찬가지로 map 메소드의 콜백 함수는 요소값, 인덱스, map 메소드를 호출한 배열, 즉 this를 전달 받을 수 있음

```javascript
// map 메소드는 전달받은 콜백 함수를 호출하면서 3개(요소값, 인덱스, this)의 인수를 전달한다.
[1, 2, 3].map((item, index, self) => {
  console.log(`요소값: ${item}, 인덱스: ${index}, this: ${self}`);
  return item;
});
/*
요소값: 1, 인덱스: 0, this: 1,2,3
요소값: 2, 인덱스: 1, this: 1,2,3
요소값: 3, 인덱스: 2, this: 1,2,3
*/
```

- forEach 메소드와 마찬가지로 map 메소드에 두번째 인자로 map 메소드 내부에서 this로 사용될 객체를 전달할 수 있음

```javascript
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  prefixArray(arr) {
    return arr.map(function (item) {
      // 외부에서 this를 전달하지 않으면 this는 전역 객체를 가리킨다.
      return this.prefix + item;
    }, this); // map 메소드 내부에서 this로 사용될 객체를 전달
  }
}

const pre = new Prefixer('-webkit-');
const preArr = pre.prefixArray(['linear-gradient', 'border-radius']);
console.log(preArr);
// ['-webkit-linear-gradient', '-webkit-border-radius']
```

- 보다 나은 방법은 ES6의 화살표 함수를 사용하는 것

```javascript
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  prefixArray(arr) {
    // 화살표 함수 내부에서 this를 참조하면
    // 상위 컨텍스트, 즉 multiply 메소드의 this를 그대로 참조한다.
    return arr.map(item => this.prefix + item);
  }
}

const pre = new Prefixer('-webkit-');
const preArr = pre.prefixArray(['linear-gradient', 'border-radius']);
console.log(preArr);
// ['-webkit-linear-gradient', '-webkit-border-radius']
```

### 9.4 Array.prototype.filter

- filter 메소드는 배열을 순회하며 배열의 각 요소에 대하여 인자로 주어진 콜백 함수를 실행
  - 그리고 콜백 함수의 실행 결과가 true인 배열 요소의 값만을 추출한 새로운 배열을 반환하는데 이때 원본 배열은 변경되지 않음

```javascript
const numbers = [1, 2, 3, 4, 5];

// 홀수만을 필터링한다 (1은 true로 평가된다)
const odds = numbers.filter(item => item % 2);

console.log(odds); // [1, 3, 5]
```

- 배열에서 특정 요소만을 필터링 조건으로 추출하여 새로운 배열을 만들고 싶을 때 사용
- 위 예제에서 filter 메소드의 콜백 함수는 요소값을 2로 나눈 나머지를 반환
  - 이때 반환값이 true인 요소만을 추출하여 새로운 배열을 반환
  - 따라서 filter 메소드가 생성하여 반환하는 새로운 배열의 length는 filter 메소드를 호출한 배열, 즉 this의 length와 같거나 작음

![](https://poiemaweb.com/assets/fs-images/26-8.png)

- forEach, map 메소드와 마찬가지로 filter 메소드의 콜백 함수는 요소값, 인덱스, filter 메소드를 호출한 배열, 즉 this를 전달 받을 수 있음

```javascript
// filter 메소드는 전달받은 콜백 함수를 호출하면서 3개(요소값, 인덱스, this)의 인수를 전달한다.
[1, 2, 3].filter((item, index, self) => {
  console.log(`요소값: ${item}, 인덱스: ${index}, this: ${self}`);
  return item % 2;
});
/*
요소값: 1, 인덱스: 0, this: 1,2,3
요소값: 2, 인덱스: 1, this: 1,2,3
요소값: 3, 인덱스: 2, this: 1,2,3
*/
```

- forEach, map 메소드와 마찬가지로 filter 메소드에 두번째 인자로 filter 메소드 내부에서 this로 사용될 객체를 전달할 수 있음
  - 보다 나은 방법은 화살표 함수를 사용하는 것

- filter 메소드는 배열의 특정 요소를 추출하기 위해 사용할 수 있지만 배열의 특정 요소를 제거하기 위해 사용할 수도 있음

```javascript
class Users {
  constructor() {
    this.users = [
      { id: 1, name: 'Lee' },
      { id: 2, name: 'Kim' }
    ];
  }

  // 요소 추출
  findById(id) {
    // id가 일치하는 사용자만 반환
    return this.users.filter(user => user.id === id);
  }

  // 요소 제거
  remove(id) {
    // id가 일치하지 않는 사용자를 모두 반환
    this.users = this.users.filter(user => user.id !== id);
  }
}

const users = new Users();

let user = users.findById(1);
console.log(user); // [{ id: 1, name: 'Lee' }]

users.remove(1);

user = users.findById(1);
console.log(user); // []
```

### 9.5 Array.prototype.reduce

- reduce 메소드는 배열을 순회하며 콜백 함수의 이전 반환값과 배열의 각 요소에 대하여 인자로 주어진 콜백 함수를 실행하여 하나의 결과값을 반환하는데, 이때 원본 배열은 변경되지 않음

- reduce 메소드는 첫번째 인수로 콜백 함수, 두번째 인수로 초기값을 전달받음
  - reduce 메소드의 콜백 함수에는 4개의 인수, 초기값 또는 콜백 함수의 이전 반환값, 요소값, 인덱스, reduce 메소드를 호출한 배열, 즉 this가 전달

- 아래 예제의 reduce 메소드는 2개의 인수, 즉 콜백 함수와 초기값 0을 전달받아 배열의 모든 요소의 누적을 구함

```javascript
// 1부터 4까지 누적을 구한다.
const sum = [1, 2, 3, 4].reduce((pre, cur, index, self) => pre + cur, 0);

console.log(sum); // 10
```

- 첫번째 인수로 전달받은 콜백 함수는 4개의 인수를 전달받아 배열의 length만큼 총 4회 호출
  - 이때 콜백 함수로 전달되는 인수와 반환값은 아래와 같음

|    구분     | 콜백 함수에 전달된 인수 |      |       |              | 콜백 함수의 반환값 |
| :---------: | :---------------------: | :--: | :---: | ------------ | ------------------ |
|             |           pre           | cur  | index | self         |                    |
| 첫번째 순회 |       0 (초기값)        |  1   |   0   | [1, 2, 3, 4] | 1 (pre + cur)      |
| 두번째 순회 |            1            |  2   |   1   | [1, 2, 3, 4] | 3 (pre + cur)      |
| 세번째 순회 |            3            |  3   |   2   | [1, 2, 3, 4] | 6 (pre + cur)      |
| 네번째 순회 |            6            |  4   |   3   | [1, 2, 3, 4] | 10 (pre + cur)     |

![](https://poiemaweb.com/assets/fs-images/26-9.png)

- 이처럼 reduce 메소드는 초기값과 첫번째 요소를 콜백 함수에게 전달하여 호출하고 다음 순회에는 콜백 함수의 반환값을 다시 콜백 함수의 첫번째 인수로 전달
  - 이러한 과정을 통해 reduce 메소드는 단일값을 반환
  - reduce 메소드는 배열을 순회하며 단일값을 구해야 하는 경우 사용
- 평균 구하기

```javascript
const values = [1, 2, 3, 4, 5, 6];

const average = values.reduce((pre, cur, i, self) => {
  //  마지막 순회인 경우, 누적값으로 평균을 구해 반환
  if (i === self.length - 1) {
    return (pre + cur) / self.length;
  }
  // 마지막 순회가 아닌 경우, 누적값을 반환
  return pre + cur;
});

console.log(average); // 3
```

- 최대값 구하기

```javascript
const values = [1, 2, 3, 4, 5];

const max = values.reduce((pre, cur) => (pre > cur ? pre : cur), 0);
console.log(max); // 5

하지만 Math.max 메소드를 사용하는 방법이 보다 직관적이다.
const values = [1, 2, 3, 4, 5];

const max = Math.max(...values);
console.log(max); // 5
```

- 중복된 요소의 개수 구하기

```javascript
const fruits = ['banana', 'apple', 'orange', 'orange', 'apple'];

const count = fruits.reduce((pre, cur) => {
  // 첫번째 순회: pre => {}, cur => 'banana'
  // 빈 객체에 요소값을 프로퍼티 키로 추가하고 프로퍼티 값을 할당
  // 만약 프로퍼티 값이 undefined이면 0으로 초기화
  pre[cur] = (pre[cur] || 0) + 1;
  return pre;
}, {});

console.log(count); // { banana: 1, apple: 2, orange: 2 }
```

- 중첩 배열 평탄화

```javascript
const values = [1, [2, 3], 4, [5, 6]];

const flatten = values.reduce((pre, cur) => pre.concat(cur), []);
// [1] => [1, 2, 3] => [1, 2, 3, 4] => [1, 2, 3, 4, 5, 6]

console.log(flatten); // [1, 2, 3, 4, 5, 6]
```

- ES10 (ECMAScript 2019)에서 새롭게 도입된 Array.prototype.flat 메소드를 사용할 수도 있음

- 중복 요소 제거

```javascript
const values = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];

const result = values.reduce((pre, cur, i, self) => {
  // 순회중인 요소의 인덱스가 자신의 인덱스라면 처음 순회하는 요소이다.
  // 이 요소만 배열에 담아 반환한다.
  // 순회중인 요소의 인덱스가 자신의 인덱스가 아니라면 중복된 요소이다.
  // 3번째 순회: [1, 2], 1, 2, [1, 2, 1, 3, 5, 4, 5, 3, 4, 4]
  // if ([1, 2, 1, 3, 5, 4, 5, 3, 4, 4].indexOf(1) === 2) => if(0 === 2)
  if (self.indexOf(cur) === i) pre.push(cur);
  return pre;
}, []);

console.log(result); // [1, 2, 3, 5, 4]
```

- 하지만 filter 메소드를 사용하는 방법이 보다 직관적

```javascript
const values = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];

// 순회중인 요소의 인덱스가 자신의 인덱스라면 처음 순회하는 요소이다. 이 요소만 반환한다.
const result = values.filter((v, i, self) => self.indexOf(v) === i);
console.log(result); // [1, 2, 3, 5, 4]
```

- 이처럼 map, filter, some, every, find와 같은 모든 배열 고차 함수는 reduce로 구현할 수 있음
  - 앞서 살펴보았듯이 recude 메소드에 두번째 인수로 전달하는 초기값은 첫번째 순회에 콜백 함수의 첫번째 인수로 전달
  - 주의할 것은 두번째 인수로 전달하는 초기값이 옵션이라는 것으로 즉, recude 메소드에 두번째 인수로 전달하는 초기값은 생략할 수 있음

```javascript
// reduce 메소드의 두번째 인수, 즉 초기값을 생략하였다.
const sum = [1, 2, 3, 4].reduce((pre, cur, index, self) => pre + cur);

console.log(sum); // 10
```

![](https://poiemaweb.com/assets/fs-images/26-10.png)

- 하지만 reduce 메소드를 호출할 때는 언제나 초기값을 전달하는 것이 안전

```javascript
const sum = [].reduce((pre, cur) => pre + cur);
// TypeError: Reduce of empty array with no initial value
```

- 이처럼 빈 배열로 reduce 메소드를 호출하면 에러가 발생
  - 이때 reduce 메소드에 초기값을 전달하면 에러가 발생하지 않음

```javascript
const sum = [].reduce((pre, cur) => pre + cur, 0);
console.log(sum); // 0
```

- reduce 메소드로 객체의 프로퍼티 값을 합산하는 경우

```javascript
const products = [
  { id: 1, price: 100 },
  { id: 2, price: 200 },
  { id: 3, price: 300 }
];

// 2번째 순회시, 콜백 함수에 숫자값이 전달된다. 이때 pre.price는 undefined이다.
// 1번째 순회 : pre => { id: 1, price: 100 }, cur => { id: 2, price: 200 }
// 2번째 순회 : pre => 300, cur => { id: 3, price: 300 }
const priceSum = products.reduce((pre, cur) => pre.price + cur.price);

console.log(priceSum); // NaN
```

- 이처럼 객체의 프로퍼티 값을 합산하는 경우에는 반드시 초기값을 전달해야 함

```javascript
const products = [
  { id: 1, price: 100 },
  { id: 2, price: 200 },
  { id: 3, price: 300 }
];

// 1번째 순회 : pre => 0, cur => 100
// 2번째 순회 : pre => 100, cur => 200
// 3번째 순회 : pre => 300, cur => 300
const priceSum = products.reduce((pre, cur) => pre + cur.price, 0);

console.log(priceSum); // 600
```

- 이처럼 reduce 메소드를 호출할 때는 초기값을 생략하지 말고 언제나 전달하는 것이 안전

### 9.6 Array.prototype.some

- some 메소드는 배열을 순회하며 요소 중 하나라도 콜백 함수의 테스트를 통과하면 true, 모든 요소가 콜백 함수의 테스트를 통과하지 못하면 false를 반환
- forEach, map, filter 메소드와 마찬가지로 some 메소드의 콜백 함수는 요소값, 인덱스, 메소드를 호출한 배열, 즉 this를 전달 받을 수 있음

```javascript
// 배열의 요소 중에 10보다 큰 요소가 1개 이상 존재하는지 확인
let result = [5, 10, 15].some(item => item > 10);
console.log(result); // true

// 배열의 요소 중에 0보다 작은 요소가 1개 이상 존재하는지 확인
result = [5, 10, 15].some(item => item < 0);
console.log(result); // false

// 배열의 요소 중에 'banana'가 1개 이상 존재하는지 확인
result = ['apple', 'banana', 'mango'].some(item => item === 'banana');
console.log(result); // true
```

- forEach, map, filter 메소드와 마찬가지로 some 메소드에 두번째 인자로 some 메소드 내부에서 this로 사용될 객체를 전달할 수 있음

### 9.7 Array.prototype.every

- every 메소드는 배열을 순회하며 모든 요소가 콜백 함수의 테스트를 통과하면 true, 요소 중 하나라도 콜백 함수의 테스트를 통과하지 못하면 false를 반환

- forEach, map, filter 메소드와 마찬가지로 every 메소드의 콜백 함수는 요소값, 인덱스, 메소드를 호출한 배열, 즉 this를 전달 받을 수 있음

```javascript
// 배열의 모든 요소가 3보다 큰지 확인
let result = [5, 10, 15].every(item => item > 3);
console.log(result); // true

// 배열의 모든 요소가 10보다 큰지 확인
result = [5, 10, 15].every(item => item > 10);
console.log(result); // false
```

- forEach, map, filter 메소드와 마찬가지로 every 메소드에 두번째 인자로 every 메소드 내부에서 this로 사용될 객체를 전달할 수 있음

### 9.8 Array.prototype.find

- ES6에서 새롭게 도입된 find 메소드는 배열을 순회하며 각 요소에 대하여 인자로 주어진 콜백 함수를 실행하여 그 결과가 참인 첫번째 요소를 반환
  - 콜백 함수의 실행 결과가 참인 요소가 존재하지 않는다면 undefined를 반환

- forEach, map, filter 메소드와 마찬가지로 find 메소드의 콜백 함수는 요소값, 인덱스, 메소드를 호출한 배열, 즉 this를 전달 받을 수 있음

```javascript
const users = [
  { id: 1, name: 'Lee' },
  { id: 2, name: 'Kim' },
  { id: 2, name: 'Choi' },
  { id: 3, name: 'Park' }
];

// id가 2인 요소를 반환한다.
const result = users.find(item => item.id === 2);

// Array#find는 배열이 아니라 요소를 반환한다.
console.log(result); // {id: 2, name: 'Kim'}
```

- filter 메소드는 콜백 함수의 실행 결과가 true인 요소만을 추출한 새로운 배열을 반환하므로 filter의 반환값은 언제나 배열
  - 하지만 find 메소드는 콜백 함수를 실행하여 그 결과가 참인 첫번째 요소를 반환하므로 find의 결과값은 해당 요소값

### 9.9 Array.prototype.findIndex

- ES6에서 새롭게 도입된 findIndex 메소드는 배열을 순회하며 각 요소에 대하여 인자로 주어진 콜백 함수를 실행하여 그 결과가 참인 첫번째 요소의 인덱스를 반환
  - 콜백 함수의 실행 결과가 참인 요소가 존재하지 않는다면 -1를 반환

- forEach, map, filter 메소드와 마찬가지로 findIndex 메소드의 콜백 함수는 요소값, 인덱스, 메소드를 호출한 배열, 즉 this를 전달 받을 수 있음

```javascript
const users = [
  { id: 1, name: 'Lee' },
  { id: 2, name: 'Kim' },
  { id: 2, name: 'Choi' },
  { id: 3, name: 'Park' }
];

function predicate(key, value) {
  // key와 value를 기억하는 클로저를 반환
  return item => item[key] === value;
}

// Array#findIndex는 콜백 함수를 실행하여 그 결과가 참인 첫번째 요소의 인덱스를 반환한다.
// id가 2인 요소의 인덱스를 구한다.
let index = users.findIndex(predicate('id', 2));
console.log(index); // 1

// name이 'Park'인 요소의 인덱스를 구한다.
index = users.findIndex(predicate('name', 'Park'));
console.log(index); // 3
```

- forEach, map, filter 메소드와 마찬가지로 findIndex 메소드에 두번째 인자로 findIndex 메소드 내부에서 this로 사용될 객체를 전달할 수 있음



Reference : https://poiemaweb.com/