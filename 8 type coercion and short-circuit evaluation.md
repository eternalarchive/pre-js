# 8. 타입 변환과 단축 평가

## 1. 타입 변환이란?

- 명시적 타입 변환(Explicit coercion) 또는 타입 캐스팅(Type casting) : 자바스크립트의 모든 값은 타입이 있는데, 개발자가 의도적으로 값의 타입을 변환하는 것
- 암묵적 타입 변환(Implicit coercion) 또는 타입 강제 변환(Type coercion) : 동적 타입 언어인 자바스크립트는 개발자의 의도와는 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변한되는 것
- 두 타입 변환 모두 변수 x의 값을 직접 변경하는 것은 아님
  - 변수 값이 원시 타입의 값인 경우, 변수 값을 변경하려면 재할당 이외에는 변경할 방법이 없음

```javascript
var x = 10;

// 명시적 타입 변환
// 숫자를 문자열로 타입 캐스팅한다.
var str = x.toString();
console.log(typeof str, str); // string 10

// 변수 x의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10

// 암묵적 타입 변환
// 문자열 연결 연산자는 숫자 타입 x의 값을 바탕으로 새로운 문자열을 생성한다.
var str = x + '';
console.log(typeof str, str); // string 10

// 변수 x의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10
```

- 연산자의 부수효과 : 단항 산술 연산자인 증가/감소(++/--) 연산자는 피연산자의 값을 변경하는 부수효과가 있는데, 이도 내부적으로 연산 결과를 피연산자에 재할당 하는 것

- 암묵적 타입 변환은 변수의 값을 재할당해서 변경하는 것이 아니라 자바스크립트 엔진이 표현식을 에러없이 평가하기 위해 피수산자의 값을 바탕으로 새로운 타입의 값을 만들어 단 한 번 사용하고 버림
- 자바스크립트 엔진은 표현식 x + ''을 평가하기 위해 변수 x의 숫자 값을 바탕으로 새로운 문자열 값 '10'을 생성하고 이것으로 표현식 '10' + ''을 평가
  - 자동 생성된 문자열 '10'은 변수 x에 할당되는 것이 아니므로 표현식의 평가가 끝나면 아무도 참조하지 않으므로 가비지 컬렉터에 의해 메모리에서 해제
- 명시적 타입 변환은 타입을 변경하겠다는 개발자의 의지가 코드에 명백히 드러나지만, 암묵적 타입 변환은 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되기 때문에 타입을 변경하겠다는 개발자의 의지가 코드에 명백히 나타나지 않음
  - 자신이 작성한 코드에서 암묵적 타입 변환이 발생하는지, 발생한다면 어떤 타입의 어떤 값으로 변환되는지, 그리고 타입 변환된 값으로 표현식은 어떻게 평가될 것인지 예측 가능해야 함
  - 예측하지 못하거나 예측한 내용이 결과와 일치하지 않는다면 오류확률 높아짐
- 명시적 타입 변환보다 암묵적 타입 변환이 가독성 측면에서 더 좋을 경우가 있으므로 반드시 명시적 타입 변환이 좋은 것은 아님
  - 중요한 것 : 코드를 예측할 수 있어야 함



## 2. 암묵적 타입 변환

- 자바스크립트 엔진은 표현식을 평가할 때 코드의 문맥을 고려하여 암묵적 타입 변환을 실행

```javascript
// 피연산자가 모두 문자열 타입이여야 하는 문맥
'10' + 2  // '102'

// 피연산자가 모두 숫자 타입이여야 하는 문맥
5 * '10'  // 50

// 피연산자 또는 표현식이 불리언 타입이여야 하는 문맥
!0 // true
if (1) { }
```

- 표현식을 평가할 때 코드의 문맥에 부합하지 않는 다양한 상황이 발생할 수 있는데, 자바스크립트는 가급적 에러를 발생시키지 않도록 암묵적 타입 변환을 통해 표현식을 평가
- 암묵적 타입 변환이 발생하면 문자열, 숫자, 불리언과 같은 원시 타입 중 하나로 타입 자동 변환



### 2.1 문자열 타입으로 변환

- 자바스크립트 엔진은 문자열 연결 연산자 표현식을 평가하기 위해 문자열 연결 연산자의 피연산자 중에서 문자열 타입이 아닌 피연산자를 문자열 타입으로 암묵적 타입 변환
- 연산자 식의 피연산자(피연산자도 표현식)만이 암묵적 타입 변환의 대상이 되는 것은 아님
  - 예를 들어 ES6에서 도입된 템플릿 리터럴의 문자열 인터폴레이션(String Interpolation)은 표현식의 평가 결과를 문자열 타입으로 암묵적 타입 변환

```javascript
console.log(`1 + 1 = ${1 + 1}`); // "1 + 1 = 2"
```

- 자바스크립트 엔진은 문자열 타입 아닌 값을 문자열 타입으로 암묵적 타입 변환으로 수행할 때 아래와 같이 동작

```javascript
// 숫자 타입
0 + ''              // "0"
-0 + ''             // "0"
1 + ''              // "1"
-1 + ''             // "-1"
NaN + ''            // "NaN"
Infinity + ''       // "Infinity"
-Infinity + ''      // "-Infinity"

// 불리언 타입
true + ''           // "true"
false + ''          // "false"

// null 타입
null + ''           // "null"

// undefined 타입
undefined + ''      // "undefined"

// 심볼 타입
(Symbol()) + ''     // TypeError: Cannot convert a Symbol value to a string

// 객체 타입
({}) + ''           // "[object Object]"
Math + ''           // "[object Math]"
[] + ''             // ""
[10, 20] + ''       // "10,20"
(function(){}) + '' // "function(){}"
Array + ''          // "function Array() { [native code] }"
```



### 2.2 숫자 타입으로 변환

- 산술 연산자의 역할은 숫자 값을 만드는 것
  - 산술 연산자의 모든 피연산자는 코드의 문맥 상 모두 숫자 타입이어야 함
- 자바스크립트 엔진은 산술 연산자 표현식을 평가하기 위해 산술 연산자의 피연산자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 변환
  - 피연산자를 숫자 타입으로 변환할 수 없는 경우는 산술 연산을 수행할 수 없으므로 표현식의 평가 결과는 NaN이 됨

```javascript
1 - '1'    // 0
1 * '10'   // 10
1 / 'one'  // NaN
```

- 피연산자를 숫자 타입으로 변환해야 할 문맥은 산술 연산자 뿐이 아님
  - 비교 연산자의 역할은 불리언 값을 만드는 것이므로 비교 연산자의 피연산자는 코드 문맥 상 모두 숫자 타입이어야 함

```javascript
'1' > 0   // true
```

- +단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환 수행

```javascript
// 문자열 타입
+''             // 0
+'0'            // 0
+'1'            // 1
+'string'       // NaN

// 불리언 타입
+true           // 1
+false          // 0

// null 타입
+null           // 0

// undefined 타입
+undefined      // NaN

// 심볼 타입
+Symbol()       // TypeError: Cannot convert a Symbol value to a number

// 객체 타입
+{}             // NaN
+[]             // 0
+[10, 20]       // NaN
+(function(){}) // NaN
```

- 빈 문자열, 빈 배열, null, false는 0으로 true는 1로 변환
- 객체와 빈 배열이 아닌 배열, undefined는 변환되지 않아 NaN이 된다는 것에 주의



### 2.3 불리언 타입으로 변환

- if문이나 for문 같은 제어문 또는 삼항 조건 연산자의 조건식(conditional expression)은 불리언 값, 즉 논리적 참 거짓을 반환해야 하는 표현식
- 자바스크립트 엔진은 조건식의 평가 결과를 불리언 타입으로 암묵적 타입 변환

```javascript
if ('')    console.log('1');
if (true)  console.log('2');
if (0)     console.log('3');
if ('str') console.log('4');
if (null)  console.log('5');

// 2 4
```

- 자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy 값(참으로 인식할 값) 또는 Falsy 값(거짓으로 인식할 값)으로 구분
  - Truthy값 -> true 로 변환
  - Falsy값 -> false 로 변환
- 제어문의 조건식과 같이 불리언 값으로 평가되어야 할 문맥에서 false로 평과되는 Falsy 값
  - false
  - undefined
  - null
  - 0, -0
  - NaN
  - ' '(빈문자열)

```javascript
// 아래의 조건문은 모두 코드 블록을 실행한다.
if (!false)     console.log(false + ' is falsy value');
if (!undefined) console.log(undefined + ' is falsy value');
if (!null)      console.log(null + ' is falsy value');
if (!0)         console.log(0 + ' is falsy value');
if (!NaN)       console.log(NaN + ' is falsy value');
if (!'')        console.log('' + ' is falsy value');
```

- Falsy값 이외의 값은 제어문의 조건식과 같이 불리언 값으로 평가되어야 할 문맥에서 모두 true로 평가되는 Truthy 값
- 아래는 Truthy/Falsy 값 판별 함수
- 함수 : 어떤 작업을 수행하기 위해 필요한 문들의 집합을 정의한 코드 블록으로 이름과 매개변수를 갖으며 필요한 때 호출하여 코드 블록에 담긴 문들을 일괄적으로 실행 (-> 나중에 자세히 살펴볼 것)

```javascript
// 주어진 인자가 Falsy 값이면 true, Truthy 값이면 false를 반환한다.
function isFalsy(v) {
  return !v;
}

// 주어진 인자가 Truthy 값이면 true, Falsy 값이면 false를 반환한다.
function isTruthy(v) {
  return !!v;
}

// 모두 true를 반환한다.
console.log(isFalsy(false));
console.log(isFalsy(undefined));
console.log(isFalsy(null));
console.log(isFalsy(0));
console.log(isFalsy(NaN));
console.log(isFalsy(''));

// 모두 true를 반환한다.
console.log(isTruthy(true));
// 빈 문자열이 아닌 문자열은 Truthy 값이다.
console.log(isTruthy('0'));
console.log(isTruthy({}));
console.log(isTruthy([]));
```



## 3. 명시적 타입 변환

- 개발자의 의도에 따라 명시적으로 타입을 변경하는 방법
  - 래퍼 객체를 생성하기 위해 사용하는 표준 빌트인 생성자 함수(String, Number, Boolean)를 연산자 없이 호출하는 방법
  - 자바스크립트에서 제공하는 빌트인 메소드를 사용하는 방법
  - 암묵적 타입 변환을 이용하는 방법
- 래퍼 객체 : 원시 값을 객체처럼 사용했을 때 자바스크립트 엔진이 원시값을 감싸는 객체
  - 원시값은 객체가 아니지만 아래 예제는 정상 동작

```javascript
var x = 10;

// 숫자를 문자열로 타입 캐스팅한다.
// x는 숫자 타입의 원시 값이지만 객체처럼 동작한다.
var str = x.toString();

console.log(str); // "10"
```



### 3.1 문자열 타입으로 변환

- 문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법
  1. String 생성자 함수를 new 연산자 없이 호출하는 방법
  2. Object.prototype.toString 메소드를 사용하는 방법
  3. 문자열 연결 연산자를 이용하는 방법

```javascript
// 1. String 생성자 함수를 new 연산자 없이 호출하는 방법
// 숫자 타입 => 문자열 타입
console.log(String(1));        // "1"
console.log(String(NaN));      // "NaN"
console.log(String(Infinity)); // "Infinity"
// 불리언 타입 => 문자열 타입
console.log(String(true));     // "true"
console.log(String(false));    // "false"

// 2. Object.prototype.toString 메소드를 사용하는 방법
// 숫자 타입 => 문자열 타입
console.log((1).toString());        // "1"
console.log((NaN).toString());      // "NaN"
console.log((Infinity).toString()); // "Infinity"
// 불리언 타입 => 문자열 타입
console.log((true).toString());     // "true"
console.log((false).toString());    // "false"

// 3. 문자열 연결 연산자를 이용하는 방법
// 숫자 타입 => 문자열 타입
console.log(1 + '');        // "1"
console.log(NaN + '');      // "NaN"
console.log(Infinity + ''); // "Infinity"
// 불리언 타입 => 문자열 타입
console.log(true + '');     // "true"
console.log(false + '');    // "false"
```



### 3.2 숫자 타입으로 변환

- 숫자 타입이 아닌 값을 숫자 타입으로 변환하는 방법
  1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
  2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
  3. +단한 연산자를 이용하는 방법
  4. *산술 연산자를 이용하는 방법

```javascript
// 1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 숫자 타입
console.log(Number('0'));     // 0
console.log(Number('-1'));    // -1
console.log(Number('10.53')); // 10.53
// 불리언 타입 => 숫자 타입
console.log(Number(true));    // 1
console.log(Number(false));   // 0

// 2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
// 문자열 타입 => 숫자 타입
console.log(parseInt('0'));       // 0
console.log(parseInt('-1'));      // -1
console.log(parseFloat('10.53')); // 10.53

// 3. + 단항 연결 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
console.log(+'0');     // 0
console.log(+'-1');    // -1
console.log(+'10.53'); // 10.53
// 불리언 타입 => 숫자 타입
console.log(+true);    // 1
console.log(+false);   // 0

// 4. * 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
console.log('0' * 1);     // 0
console.log('-1' * 1);    // -1
console.log('10.53' * 1); // 10.53
// 불리언 타입 => 숫자 타입
console.log(true * 1);    // 1
console.log(false * 1);   // 0
```



### 3.3 불리언 타입으로 변환

- 불리언 타입이 아닌 값을 불리언 타입으로 변환하는 방법
  1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
  2. ! 부정 논리 연산자를 두 번 사용하는 방법

```javascript
// 1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 불리언 타입
console.log(Boolean('x'));       // true
console.log(Boolean(''));        // false
console.log(Boolean('false'));   // true
// 숫자 타입 => 불리언 타입
console.log(Boolean(0));         // false
console.log(Boolean(1));         // true
console.log(Boolean(NaN));       // false
console.log(Boolean(Infinity));  // true
// null 타입 => 불리언 타입
console.log(Boolean(null));      // false
// undefined 타입 => 불리언 타 입
console.log(Boolean(undefined)); // false
// 객체 타입 => 불리언 타입
console.log(Boolean({}));        // true
console.log(Boolean([]));        // true

// 2. ! 부정 논리 연산자를 두번 사용하는 방법
// 문자열 타입 => 불리언 타입
console.log(!!'x');       // true
console.log(!!'');        // false
console.log(!!'false');   // true
// 숫자 타입 => 불리언 타입
console.log(!!0);         // false
console.log(!!1);         // true
console.log(!!NaN);       // false
console.log(!!Infinity);  // true
// null 타입 => 불리언 타입
console.log(!!null);      // false
// undefined 타입 => 불리언 타입
console.log(!!undefined); // false
// 객체 타입 => 불리언 타입
console.log(!!{});        // true
console.log(!![]);        // true
```



## 4. 단축 평가

- 논리합(||) 연산자와 논리곱(&&) 연산자의 연산 결과는 불리언 값이 아닐 수도 있음
- 이 두 연산자는 언제나 피연산자 중 어느 한 쪽 값을 반환

```
'Cat' && 'Dog' // 'Dog'
'Cat' || 'Dog' // 'Cat'
```

- 논리곱 연산자는 두 개의 피연산자가 모두 true로 평가될 때 true 반환하며, 왼쪽에서 오른쪽으로 평가 진행

  1. 첫 번째 피연산자 'Cat'은 Truthy값이므로 true로 평가된다. 하지만 이 시점까지는 위 표현식을 평가할 수 없다. 두 번째 피연산자까지 평가하도록 한다.
  2. 두 번째 피연산자 'Dog'은 Truthy 값이므로 true로 평가된다. 이 때 두개의 연산자가 모두 true로 평가되었으므로 논리곱 연산의 결과를 결정한 것은 두 번째 피연산자 'Dog'이다.
  3. 논리곱 연산자는 논리 연산의 결과를 결정한 두 번째 피연산자 즉, 문자열 'Dog'를 그대로 반환한다.

- 논리합 연산자도 논리곱 연산자와 동일하게 동작하는데, 두 개의 피연산자 중 하나만 true로 평가되어도 true를 반환하며 왼쪽에서 오른쪽으로 평가가 진행

  1. 첫번째 피연산자 ‘Cat’은 Truthy 값이므로 true로 평가된다. 이 시점에 두번째 피연산자까지 평가해 보지 않아도 위 표현식을 평가할 수 있다.

  2. 논리합 연산자는 논리 연산의 결과를 결정한 첫번째 피연산자 즉, 문자열 ‘Cat’를 그대로 반환한다.

- 단축 평가(Short-Circuit evaluation) : 논리곱 연산자와 논리합 연산자는 이와 같이 논리 평가를 결정한 피연산자를 그대로 반환

  - 단축 평가 규칙

| 단축 평가 표현식    | 평가 결과 |
| :------------------ | :-------- |
| true \|\| anything  | true      |
| false \|\| anything | anything  |
| true && anything    | anything  |
| false && anything   | false     |

```javascript
// 논리합(||) 연산자
'Cat' || 'Dog'  // 'Cat'
false || 'Dog'  // 'Dog'
'Cat' || false  // 'Cat'

// 논리곱(&&) 연산자
'Cat' && 'Dog'  // Dog
false && 'Dog'  // false
'Cat' && false  // false
```

- 단축 평가가 유용하게 사용되는 상황

  - 객체가 null인지 확인하고 프로퍼티를 참조할 때
    - 객체는 키(key)와 값(value)으로 구성된 프로퍼티(property)들의 집합
    - 만약 객체가 null인 경우 객체의 프로퍼티를 참조하면 타입 에러(TypeError)가 발생하는데, 이 때 단축평가를 사용하면 에러를 발생시키지 않음

  - 함수 매개변수에 기본값을 설정할 때
    - 함수를 호출할 때 인수를 전달하지 않으면 매개변수는 undefined를 갖음
    - 이 때 단축평가를 사용하여 매개변수의 기본값을 설정하면 undefined로 인해 발생할 수 있는 에러를 방지할 수 있음

```javascript
// 객체가 null인지 확인하고 프로퍼티를 참조할 때
var elem = null;

console.log(elem.value); // TypeError: Cannot read property 'value' of null
console.log(elem && elem.value); // null

// 함수 매개변수에 기본값을 설정할 때
// 단축 평가를 사용한 매개변수의 기본값 설정
function getStringLength(str) {
  str = str || '';
  return str.length;
}

getStringLength();     // 0
getStringLength('hi'); // 2

// ES6의 매개변수의 기본값 설정
function getStringLength(str = '') {
  return str.length;
}

getStringLength();     // 0
getStringLength('hi'); // 2
```



모든 출처 : https://poiemaweb.com/