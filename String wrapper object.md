# String 래퍼 객체

- String 객체는 원시 타입인 문자열을 다룰 때 유용한 프로퍼티와 메소드를 제공하는 레퍼(wrapper) 객체
- 변수 또는 객체 프로퍼티가 문자열을 값으로 가지고 있다면 String 객체의 별도 생성없이 String 객체의 프로퍼티와 메소드를 사용할 수 있음
- 원시 타입이 wrapper 객체의 메소드를 사용할 수 있는 이유는 원시 타입으로 프로퍼티나 메소드를 호출할 때 원시 타입과 연관된 wrapper 객체로 일시적으로 변환되어 프로토타입 객체를 공유하게 되기 때문

```javascript
const str = 'Hello world!';
console.log(str.toUpperCase()); // 'HELLO WORLD!'
```

- 위에서 원시 타입 문자열을 담고 있는 변수 str이 String.prototype.toUpperCase() 메소드를 호출할 수 있는 것은 변수 str의 값이 일시적으로 wrapper객체로 변환되었기 때문

- 사용 빈도가 높은 String 객체의 프로퍼티와 메소드에 대해 살펴볼 것



## 1. String Constructor

- String 객체는 String 생성자 함수를 통해 생성할 수 있음
  - 이때 전달된 인자는 모두 문자열로 변환

```javascript
let strObj = new String('Lee');
console.log(strObj); // String {0: 'L', 1: 'e', 2: 'e', length: 3, [[PrimitiveValue]]: 'Lee'}

strObj = new String(1);
console.log(strObj); // String {0: '1', length: 1, [[PrimitiveValue]]: '1'}

strObj = new String(undefined);
console.log(strObj); // String {0: 'u', 1: 'n', 2: 'd', 3: 'e', 4: 'f', 5: 'i', 6: 'n', 7: 'e', 8: 'd', length: 9, [[PrimitiveValue]]: 'undefined'}
```

- new 연산자를 사용하지 않고 String 생성자 함수를 호출하면 String 객체가 아닌 문자열 리터럴을 반환
  - 이때 형 변환이 발생할 수 있음

```javascript
var x = String('Lee');

console.log(typeof x, x); // string Lee
```

- 일반적으로 문자열을 사용할 때는 원시 타입 문자열을 사용

```javascript
const str = 'Lee';
const strObj = new String('Lee');

console.log(str == strObj);  // true
console.log(str === strObj); // false

console.log(typeof str);    // string
console.log(typeof strObj); // object
```



## 2.String Property

### 2.1 String.length

- 문자열 내의 문자 갯수를 반환
- String 객체는 length 프로퍼티를 소유하고 있으므로 유사 배열 객체

```javascript
const str1 = 'Hello';
console.log(str1.length); // 5

const str2 = '안녕하세요!';
console.log(str2.length); // 6
```



## 3. String Method

- String 객체의 모든 메소드는 언제나 새로운 문자열을 반환
- 문자열은 변경 불가능(immutable)한 원시 값

### 3.1 String.prototype.charAt(pos: number): string ES1

- 인수로 전달한 index를 사용하여 index에 해당하는 위치의 문자를 반환
- index는 0 ~ (문자열 길이 - 1) 사이의 정수
- 지정한 index가 문자열의 범위(0 ~ (문자열 길이 - 1))를 벗어난 경우 빈문자열을 반환

![](https://poiemaweb.com/img/index.png)

```javascript
const str = 'Hello';

console.log(str.charAt(0)); // H
console.log(str.charAt(1)); // e
console.log(str.charAt(2)); // l
console.log(str.charAt(3)); // l
console.log(str.charAt(4)); // o
// 지정한 index가 범위(0 ~ str.length-1)를 벗어난 경우 빈문자열을 반환한다.
console.log(str.charAt(5)); // ''

// 문자열 순회. 문자열은 length 프로퍼티를 갖는다.
for (let i = 0; i < str.length; i++) {
  console.log(str.charAt(i));
}

// String 객체는 유사 배열 객체이므로 배열과 유사하게 접근할 수 있다.
for (let i = 0; i < str.length; i++) {
  console.log(str[i]); // str['0']
}
```



###  3.2 String.prototype.concat(…strings: string[]): string ES3

- 인수로 전달한 1개 이상의 문자열과 연결하여 새로운 문자열을 반환

- concat 메소드를 사용하는 것보다는 `+`, `+=` 할당 연산자를 사용하는 것이 성능상 유리

```javascript
/**
 * @param {...string} str - 연결할 문자열
 * @return {string}
 */
str.concat(str1[,str2,...,strN])
```

```javascript
console.log('Hello '.concat('Lee')); // Hello Lee
```



### 3.3 String.prototype.indexOf(searchString: string, fromIndex=0): number ES1

- 인수로 전달한 문자 또는 문자열을 대상 문자열에서 검색하여 처음 발견된 곳의 index를 반환
  - 발견하지 못한 경우 -1을 반환

```javascript
/**
 * @param {string} searchString - 검색할 문자 또는 문자열
 * @param {string} [fromIndex=0] - 검색 시작 index (생략할 경우, 0)
 * @return {number}
 */
str.indexOf(searchString[, fromIndex])
```

```javascript
const str = 'Hello World';

console.log(str.indexOf('l'));  // 2
console.log(str.indexOf('or')); // 7
console.log(str.indexOf('or' , 8)); // -1

if (str.indexOf('Hello') !== -1) {
  // 문자열 str에 'hello'가 포함되어 있는 경우에 처리할 내용
}

// ES6: String.prototype.includes
if (str.includes('Hello')) {
  // 문자열 str에 'hello'가 포함되어 있는 경우에 처리할 내용
}
```



### 3.4 String.prototype.lastIndexOf(searchString: string, fromIndex=this.length-1): number ES1

- 인수로 전달한 문자 또는 문자열을 대상 문자열에서 검색하여 마지막으로 발견된 곳의 index를 반환
  - 발견하지 못한 경우 -1을 반환

- 2번째 인수(fromIndex)가 전달되면 검색 시작 위치를 fromIndex으로 이동하여 역방향으로 검색을 시작
  - 이때 검색 범위는 0 ~ fromIndex이며 반환값은 indexOf 메소드와 동일하게 발견된 곳의 index

```javascript
const str = 'Hello World';

console.log(str.lastIndexOf('World')); // 6
console.log(str.lastIndexOf('l'));     // 9
console.log(str.lastIndexOf('o', 5));  // 4
console.log(str.lastIndexOf('o', 8));  // 7
console.log(str.lastIndexOf('l', 10)); // 9

console.log(str.lastIndexOf('H', 0));  // 0
console.log(str.lastIndexOf('W', 5));  // -1
console.log(str.lastIndexOf('x', 8));  // -1
```

> lastIndexOf

![](https://poiemaweb.com/img/lastindexof.png)



### 3.5 String.prototype.replace(searchValue: string | RegExp, replaceValue: string | replacer: (substring: string, …args: any[]) => string): string): string ES3

- 첫번째 인수로 전달한 문자열 또는 정규표현식을 대상 문자열에서 검색하여 두번째 인수로 전달한 문자열로 대체
- 원본 문자열은 변경되지 않고 결과가 반영된 새로운 문자열을 반환

- 검색된 문자열이 여럿 존재할 경우 첫번째로 검색된 문자열만 대체

```javascript
/**
 * @param {string | RegExp} searchValue - 검색 대상 문자열 또는 정규표현식
 * @param {string | Function} replacer - 치환 문자열 또는 치환 함수
 * @return {string}
 */
str.replace(searchValue, replacer)
```

```javascript
const str = 'Hello world';

// 첫번째로 검색된 문자열만 대체하여 새로운 문자열을 반환한다.
console.log(str.replace('world', 'Lee')); // Hello Lee

// 특수한 교체 패턴을 사용할 수 있다. ($& => 검색된 문자열)
console.log(str.replace('world', '<strong>$&</strong>')); // Hello <strong>world</strong>

/* 정규표현식
g(Global): 문자열 내의 모든 패턴을 검색한다.
i(Ignore case): 대소문자를 구별하지 않고 검색한다.
*/
console.log(str.replace(/hello/gi, 'Lee')); // Lee Lee

// 두번째 인수로 치환 함수를 전달할 수 있다.
// camelCase => snake_case
const camelCase = 'helloWorld';

// /.[A-Z]/g => 1문자와 대문자의 조합을 문자열 전체에서 검색한다.
console.log(camelCase.replace(/.[A-Z]/g, function (match) {
  // match : oW => match[0] : o, match[1] : W
  return match[0] + '_' + match[1].toLowerCase();
})); // hello_world

// /(.)([A-Z])/g => 1문자와 대문자의 조합
// $1 => (.)
// $2 => ([A-Z])
console.log(camelCase.replace(/(.)([A-Z])/g, '$1_$2').toLowerCase()); // hello_world

// snake_case => camelCase
const snakeCase = 'hello_world';

// /_./g => _와 1문자의 조합을 문자열 전체에서 검색한다.
console.log(snakeCase.replace(/_./g, function (match) {
  // match : _w => match[1] : w
  return match[1].toUpperCase();
})); // helloWorld
```

- 첫번째 인자에는 문자열 또는 정규표현식이 전달
  - 문자열의 경우 첫번째 검색 결과만이 대체되지만 정규표현식을 사용하면 다양한 방식으로 검색할 수 있음

- 위 예제에서 `/hello/`는 패턴이라하며 검색할 문자열을 의미
  - gi는 flag라 하는데 g(global)는 문자열 내에 패턴과 일치하는 모든 문자열을 검색하라는 의미이고 i(ignore)는 대소문자를 구분하지 않는다는 의미

- 자세한 내용은 [RegExp](https://poiemaweb.com/js-regexp)를 참조



###  3.6 String.prototype.split(separator: string | RegExp, limit?: number): string[] ES3

- 첫번째 인수로 전달한 문자열 또는 정규표현식을 대상 문자열에서 검색하여 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환
  - 원본 문자열은 변경되지 않음

- 인수가 없는 경우, 대상 문자열 전체를 단일 요소로 하는 배열을 반환

```javascript
/**
 * @param {string | RegExp} [separator] - 구분 대상 문자열 또는 정규표현식
 * @param {number} [limit] - 구분 대상수의 한계를 나타내는 정수
 * @return {string[]}
 */
str.split([separator[, limit]])
```

```javascript
const str = 'How are you doing?';

// 공백으로 구분(단어로 구분)하여 배열로 반환한다
console.log(str.split(' ')); // [ 'How', 'are', 'you', 'doing?' ]

// 정규 표현식
console.log(str.split(/\s/)); // [ 'How', 'are', 'you', 'doing?' ]

// 인수가 없는 경우, 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.
console.log(str.split()); // [ 'How are you doing?' ]

// 각 문자를 모두 분리한다
console.log(str.split('')); // [ 'H','o','w',' ','a','r','e',' ','y','o','u',' ','d','o','i','n','g','?' ]

// 공백으로 구분하여 배열로 반환한다. 단 요소수는 3개까지만 허용한다
console.log(str.split(' ', 3)); // [ 'How', 'are', 'you' ]

// 'o'으로 구분하여 배열로 반환한다.
console.log(str.split('o')); // [ 'H', 'w are y', 'u d', 'ing?' ]
```



### 3.7 String.prototype.substring(start: number, end=this.length): string ES3

- 첫번째 인수로 전달한 start 인덱스에 해당하는 문자부터 두번째 인자에 전달된 end 인덱스에 해당하는 문자의 **바로 이전 문자까지**를 모두 반환
  - 이때 첫번째 인수 < 두번째 인수의 관계가 성립

> substring

![](https://poiemaweb.com/img/substring.png)

- 첫번째 인수 > 두번째 인수 : 두 인수는 교환
- 두번째 인수가 생략된 경우 : 해당 문자열의 끝까지 반환
- 인수 < 0 또는 NaN인 경우 : 0으로 취급
- 인수 > 문자열의 길이(str.length) : 인수는 문자열의 길이(str.length)으로 취급

```javascript
/**
 * @param {number} start - 0 ~ 해당문자열 길이 -1 까지의 정수
 * @param {number} [end=this.length] - 0 ~ 해당문자열 길이까지의 정수
 * @return {string}
 */
str.substring(start[, end])
```

```javascript
const str = 'Hello World'; // str.length == 11

console.log(str.substring(1, 4)); // ell

// 첫번째 인수 > 두번째 인수 : 두 인수는 교환된다.
console.log(str.substring(4, 1)); // ell

// 두번째 인수가 생략된 경우 : 해당 문자열의 끝까지 반환한다.
console.log(str.substring(4)); // o World

// 인수 < 0 또는 NaN인 경우 : 0으로 취급된다.
console.log(str.substring(-2)); // Hello World

// 인수 > 문자열의 길이(str.length) : 인수는 문자열의 길이(str.length)으로 취급된다.
console.log(str.substring(1, 12)); // ello World
console.log(str.substring(11)); // ''
console.log(str.substring(20)); // ''
console.log(str.substring(0, str.indexOf(' '))); // 'Hello'
console.log(str.substring(str.indexOf(' ') + 1, str.length)); // 'World'
```



### 3.8 String.prototype.slice(start?: number, end?: number): string ES3

- String.prototype.substring과 동일
- 단, String.prototype.slice는 음수의 인수를 전달할 수 있음

```javascript
const str = 'hello world';

// 인수 < 0 또는 NaN인 경우 : 0으로 취급된다.
console.log(str.substring(-5)); // 'hello world'
// 뒤에서 5자리를 잘라내어 반환한다.
console.log(str.slice(-5)); // 'world'

// 2번째부터 마지막 문자까지 잘라내어 반환
console.log(str.substring(2)); // llo world
console.log(str.slice(2)); // llo world

// 0번째부터 5번째 이전 문자까지 잘라내어 반환
console.log(str.substring(0, 5)); // hello
console.log(str.slice(0, 5)); // hello
```



###  3.9 String.prototype.toLowerCase(): string ES1

- 대상 문자열의 모든 문자를 소문자로 변경

```javascript
console.log('Hello World!'.toLowerCase()); // hello world!
```



### 3.10 String.prototype.toUpperCase(): string ES1

- 대상 문자열의 모든 문자를 대문자로 변경

```javascript
console.log('Hello World!'.toUpperCase()); // HELLO WORLD!
```



### 3.11 String.prototype.trim(): string ES5

- 대상 문자열 양쪽 끝에 있는 공백 문자를 제거한 문자열을 반환

```javascript
const str = '   foo  ';

console.log(str.trim()); // 'foo'

// String.prototype.replace
console.log(str.replace(/\s/g, ''));   // 'foo'
console.log(str.replace(/^\s+/g, '')); // 'foo  '
console.log(str.replace(/\s+$/g, '')); // '   foo'

// String.prototype.{trimStart,trimEnd} : Proposal stage 3
console.log(str.trimStart()); // 'foo  '
console.log(str.trimEnd());   // '   foo'
```



### 3.12 String.prototype.repeat(count: number): string ES6

- 인수로 전달한 숫자만큼 반복해 연결한 새로운 문자열을 반환한다. count가 0이면 빈 문자열을 반환하고 음수이면 RangeError를 발생시킴

```javascript
console.log('abc'.repeat(0));   // ''
console.log('abc'.repeat(1));   // 'abc'
console.log('abc'.repeat(2));   // 'abcabc'
console.log('abc'.repeat(2.5)); // 'abcabc' (2.5 → 2)
console.log('abc'.repeat(-1));  // RangeError: Invalid count value
```



###  3.13 String.prototype.includes(searchString: string, position?: number): boolean ES6

- 인수로 전달한 문자열이 포함되어 있는지를 검사하고 결과를 불리언 값으로 반환
- 두번째 인수는 옵션으로 검색할 위치를 나타내는 정수

```javascript
const str = 'hello world';

console.log(str.includes('hello')); // true
console.log(str.includes(' '));     // true
console.log(str.includes('wo'));    // true
console.log(str.includes('wow'));   // false
console.log(str.includes(''));      // true
console.log(str.includes());        // false

// String.prototype.indexOf 메소드로 대체할 수 있다.
console.log(str.indexOf('hello')); // 0
```



Reference : https://poiemaweb.com

