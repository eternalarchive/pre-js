# 7. 제어문

- 제어문(Control flow statement) : 주어진 조건에 따라 코드 블록을 실행(조건문)하거나 반복 실행(반복문)할 때 사용
- 제어문을 사용하면 위에서 아래 방향으로 실행되는 코드 실행 흐름을 인위적으로 지어 가능
- 제어문의 단점 : 코드의 흐름을 이해하기 어렵게 만들어 가독성을 해친다(오류 발생의 원인)



## 1. 블록문

- 블록문(Block statement/Compound statement) : 0개 이상의 문을 중괄호로 묶은 것으로 코드 블록 또는 블록이라고 부르기도 함
  - 자바스크립트는 블록문을 하나의 실행 단위로 취급
- 단독으로 사용 가능하나, 일반적으로 제어문이나 함수 선언문에서 사용하는 것이 일반적
- 문의 끝에는 세미 콜론을 붙이는 것이 일반적이지만, 블록문의 끝에는 세미클론을 붙이지 않음

```javascript
// 블록문
{
  var foo = 10;
  console.log(foo);
}

// 제어문
var x = 0;
while (x < 10) {
  x++;
}
console.log(x); // 10

// 함수 선언문
function sum(a, b) {
  return a + b;
}
console.log(sum(1, 2)); // 3
```



## 2. 조건문

- 조건문(conditional statement) : 주어진 조건식(conditional exprsstion)의 평가 결과에 따라 코드 블럭(블록문)의 실행을 결정
  - 불리언 값으로 평가될 수 있는 표현식

### 2.1 if...else 문

- 주어진 조건식(불리언 값으로 평가될 수 있는 표현식)의 평가 결과, 즉 논리적 참, 거짓에 따라 실행할 코드 블록 결정
  - 조건식의 결과가 불리언 값이 아닐 경우 강제 변환되어 논리적 참, 거짓 구별
- 조건식을 추가하고 싶으면 else if 문을 사용함

```javascript
if (조건식1) {
  // 조건식1이 참이면 이 코드 블록이 실행된다.
} else if (조건식2) {
  // 조건식2이 참이면 이 코드 블록이 실행된다.
} else {
  // 조건식1과 조건식2가 모두 거짓이면 이 코드 블록이 실행된다.
}
```

- if문과 else문은 2번이상 사용할 수 없음
  - else if문은 여러번 사용 가능

```javascript
var num = 2;
var kind;

if (num > 0)      kind = '양수';
else if (num < 0) kind = '음수';
else              kind = '영';

console.log(kind); // 양수
```

- 코드 블록 내의 문이 하나뿐이라면 중괄호 생략 가능

- 대부분의 if...else문은 "연산자"에서 살펴본 삼항 조건 연산자로 바꿔 쓸 수 있음

```javascript
// x가 짝수이면 문자열 '짝수'를 반환하고 홀수이면 문자열 '홀수'를 반환한다.
var x = 2;
var result;

if (x % 2) { // 2 % 2는 0이고 0은 false로 취급된다
  result = '홀수';
} else {
  result = '짝수';
}

console.log(result); // 짝수

// x가 짝수이면 문자열 '짝수'를 반환하고 홀수이면 문자열 '홀수'를 반환한다.
var x = 2;

// 0은 false로 취급된다.
var result = x % 2 ? '홀수' : '짝수';

console.log(result); // 짝수

// 세 가지 경우(양수, 음수, 영)를 갖는 경우
var num = 2;

// 0은 false로 취급된다.
var kind = num ? (num > 0 ? '양수' : '음수') : '영';

console.log(kind); // 양수
```

- num > 0 ? '양수' : '음수'는 표현식
- 두 방식의 차이 : 삼항 연산자는 값으로 평가되는 표현식을 만들지만 if...else문은 표현식이 아닌 문
  - 따라서 **삼항 조건 연산자 표현식**은 값처럼 사용할 수 있으므로 **변수에 할당 가능**
  - **if...else 문**은 값처럼 사용할 수 없기 때문에 **변수에 할당할 수 없음*



### 2.2 switch문

- switch문 : 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case문으로 실행 순서를 이동
- case문 : 상황(case)을 의미하는 표현식을 지정하고 콜론으로 마치며, 그 뒤에 실행할 문들을 위치시킴
- switch 문의 표현식과 일치하는 표현식을 갖는 case문이 없다면 실행 순서는 default 문으로 이동
  - default는 옵션으로 사용할수도 사용하지 않을수도 있음

```javascript
switch (표현식) {
  case 표현식1:
    switch 문의 표현식과 표현식1이 일치하면 실행될 문;
    break;
  case 표현식2:
    switch 문의 표현식과 표현식2가 일치하면 실행될 문;
    break;
  default:
    switch 문의 표현식과 일치하는 표현식을 갖는 case 문이 없을 때 실행될 문;
}
```

- if...else문과 swich문의 차이점
  - if...else 문의 조건식은 반드시 불리언 값으로 평가되지만,
    switch문의 표현식은 불리언 값보다는 문자열, 숫자 값인 경우가 많음
  - if...else 문은 논리적 참, 거짓으로 실행할 코드 블록을 결정하지만, 
    switch문은 논리적 참, 거짓보다는 다양한 상황(case)에 따라 실행할 코드 블록을 결정할 때 사용

- 폴스루(fall through) : switch문의 표현식의 평가 결과와 일치하는 case문으로 실행순서가 이동하여 문을 실행한 후, switch문을 탈출하지 않고 이후의 모든 case문과 default문을 실행하는 것
  - break 키워드로 구성된 break문을 사용하여 코드 블록에서 탈출해야 함
  - default문은 switch문의 가장 마지막에 위치하므로 별도의 break문 불필요

```javascript
// 월을 영어로 변환한다. (11 → 'November')
// 폴스루 발생
var month = 11;
var monthName;

switch (month) {
  case 1:
    monthName = 'January';
// 생략
  case 11:
    monthName = 'November';
  case 12:
    monthName = 'December';
  default:
    monthName = 'Invalid month';
}

console.log(monthName); // Invalid month

// 월을 영어로 변환한다. (11 → 'November')
// 올바른 switch 문
var month = 11;
var monthName;

switch (month) {
  case 1:
    monthName = 'January';
    break;
// 생략
  case 11:
    monthName = 'November';
    break;
  case 12:
    monthName = 'December';
    break;
  default:
    monthName = 'Invalid month';
}

console.log(monthName); // November
```

- 폴스루를 이용하는 경우 : 윤년인지 판별하여 2월의 일수 계산 예제 등

```javascript
var year = 2000; // 2000년은 윤년으로 2월이 29일이다.
var month = 2;
var days = 0;

switch (month) {
  case 1: case 3: case 5: case 7: case 8: case 10: case 12:
    days = 31;
    break;
  case 4: case 6: case 9: case 11:
    days = 30;
    break;
  case 2:
    // 윤년 계산 알고리즘
    // 1. 년도가 4로 나누어 떨어지는 해는 윤년(2000, 2004, 2008, 2012, 2016, 2020…)
    // 2. 그 중에서 년도가 100으로 나누어 떨어지는 해는 평년(2000, 2100, 2200...)
    // 3. 그 중에서 년도가 400으로 나누어 떨어지는 해는 윤년(2000, 2400, 2800...)
    days = ((year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0)) ? 29 : 28;
    break;
  default:
    console.log('Invalid month');
}

console.log(days); // 29
```

- switch문은 case, default, break 등 다양한 키워드를 사용해야 하고 상황에 따라 실행될 코드 블록이 중괄호로 묶여있지 않으며 폴스루가 발생하는 등 문법이 복잡
- 만약 if...else문으로 해결할 수 있다면 switch문보다 if...else을 사용하는 편이 좋음
  - if...else문보다 switch문을 사용했을 때 가독성이 더 좋다면 switch문을 사용하는 편이 좋음



## 3. 반복문

- 반복문(Loop statement) : 주어진 조건식의 평가 결과가 참인 경우 코드 블럭을 실행. 조건식이 거짓일 때까지 반복
- 자바스크립트 반복문 종류 3가지 : for문, while문, do...while문
  - 이 외에도 for...in문과 ES6에서 새롭게 되입된 for...of문 존재 -> 차후 살펴볼 것

###3.1 for문

- for문은 조건식이 거짓으로 판별될 때까지 코드 블록 실행

```javascript
for (var i = 0; i < 2; i++) {
  console.log(i);
}
```

- 위 예제의 for문은 변수 i가 0으로 초기화된 상태에서 시작하여 i가 2보다 작을 때까지 코드 블록을 2번 반복 실행(0,1)

1. for문 실행시 가장 먼저 변수 선언문 var i = 0실행. 변수 선언문은 단 한 번만 실행.
2. 변수 선언문이 종료되면 조건식으로 이동. 현재 변수 i는 0이므로 조건식의 평가결과는 true
3. 조건식의 평가결과가 true이므로 실행 순서가 코드 블록으로 이동하여 실행(증감문으로 실행 순서가 이동하는 것이 아니라 코드 블록으로 실행 순서가 이동하는 것에 주의)
4. 코드 블록의 실행이 종료되면 증감식으로 실행 순서 이동하여 실행, i는 1이 됨
5. 증감식 실행 종료시 조건식으로 이동(변수 선언문으로 이동하지 않고 조건식으로 이동하는 것에 주의) 현재 변수 i는 1이므로 조건식의 평가 결과는 true
6. 조건식의 평가 결과가 true이므로 실행 순서가 코드 블록으로 이동 실행
7. 코드 블록의 실행이 종료하면 증감식으로 실행 순서 이동. 증감식 i++가 실행되어 i는 2가 됨
8. 증감식 실행이 종료되면 다시 조건식으로 실행 순서 이동. 현재 변수 i는 2이므로 조건식 평가결과 false. 조건식의 평가 결과가 false이므로 for문 실행 종료

- for문의 변수 선언문, 조건식, 증감식은 모두 옵션이므로 어떤 식도 선언하지 않을 수 있고, 그럴시 무한 루프가 된다.

- for문 내에 for문을 중첩해 사용할 수 있음

```javascript
// 두 개의 주사의를 던졌을 때 두 눈의 합이 6이 되는 모든 경우의 수 출력
for (var i = 1; i <= 6; i++) {
  for (var j = 1; j <= 6; j++) {
    if (i + j === 6) console.log(`[${i}, ${j}]`);
  }
}
// 출력 결과
[1, 5]
[2, 4]
[3, 3]
[4, 2]
[5, 1]
```



### 3.2 while문

- while문 : 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행하다 거짓이 되면 실행 종료
  - 만약 조건식의 평가 결과가 불리언 값이 아니면 불리언 값으로 강제 변환되어 논리적 참, 거짓을 구별

```javascript
var count = 0;

// count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
while (count < 3) {
  console.log(count);
  count++;
} // 0 1 2
```

- 조건식의 평가 결과가 언제나 참이면 무한 루프가 됨
- 무한 루프를 탈출하기 위해서는 코드 블럭 내에 if문으로 탈출 조건을 만들고 break문으로 코드 블록 탈출

```javascript
var count = 0;

// 무한루프
while (true) {
  console.log(count);
  count++;
  // count가 3이면 코드 블록을 탈출한다.
  if (count === 3) break;
} // 0 1 2
```



### 3.3 do...while문

- do...while문 : 코드 블록을 먼저 실행하고 조건식을 평가, 따라서 코드 블록은 무조건 한 번 이상 실행

```javascript
var count = 0;

// count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
do {
  console.log(count);
  count++;
} while (count < 3); // 0 1 2
```



## 4. break문

- break문 : 레이블 문, 반복 문 또는 switch문의 코드 블록 탈출
- 레이블 문, 반복문, switch문의 코드 블록 이외에 break문을 사용하면 SyntaxError(문법 에러) 발생

```javascript
if (true) {
  break; // Uncaught SyntaxError: Illegal break statement
}
```

- 레이블 문(Label statement)  : 식별자가 붙은 문
- 레이블 문은 프로그램의 실행 순서를 제어하기 위해 사용
  - switch문의 case문과 default문도 레이블 분
  - 레이블 문을 탈출하려면 vreak문에 레이블 식별자를 지정

```javascript
// foo라는 레이블 식별자가 붙은 레이블 문
foo: console.log('foo');

// foo라는 식별자가 붙은 레이블 블록문
foo: {
  console.log(1);
  break foo; // foo 레이블 블록문을 탈출한다.
  console.log(2);
}

console.log('Done!');
```

- 중첩된 for문 내부 for문에서 break문을 실행하면 내부 for문을 탈출하여 외부 for문으로 진입
  - 이 때 내부 for문이 아닌 외부 for문을 탈출하려면 레이블 문을 사용

```javascript
// outer라는 식별자가 붙은 레이블 for 문
outer: for (var i = 0; i < 3; i++) {
  for (var j = 0; j < 3; j++) {
    // i + j === 3이면 outer라는 식별자가 붙은 레이블 for 문을 탈출한다.
    if (i + j === 3) break outer;
    console.log('inner ' + j);
  }
}

console.log('Done!');
```

- 중첩된 for문을 외부로 탈출할 때 레이블 문은 유용하지만 그 외의 경우 레이블 문은 일반적으로 권장하지 않음 : 프로그램 흐름이 복잡해져 가독성이 나빠지고 오류 발생 가능성 높임
- break문은 레이블 문 뿐 아니라 반복문, switch문에서도 사용 가능
  - 이 경우 break문에 레이블 식별자 지정하지 않음
  - break문은 반복문을 더이상 진행하지 않아도 될 때 불필요한 반복을 회피할 수 있어 유용

- 문자열에서 특정 문자의 인덱스(위치)를 검색하는 예제

```javascript
var string = 'Hello World.';
var search = 'l';
var index;

// 문자열은 유사배열이므로 for 문으로 순회할 수 있다.
for (var i = 0; i < string.length; i++) {
  // 문자열의 개별 문자가 'l'이면
  if (string[i] === search) {
    index = i;
    break; // 반복문을 탈출한다.
  }
}

console.log(index); // 2

// 참고로 String.prototype.indexOf 메소드를 사용해도 같은 동작을 한다.
console.log(string.indexOf(search)); // 2
```



## 5. continue문

- continue문 : 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 이동
  - break문처럼 반복문을 탈출하지는 않음
- 문자열에서 특정 문자의 개수를 카운트하는 예제

```javascript
var string = 'Hello World.';
var search = 'l';
var count = 0;

// 문자열은 유사배열이므로 for 문으로 순회할 수 있다.
for (var i = 0; i < string.length; i++) {
  // 'l'이 아니면 현 지점에서 실행을 중단하고 반복문의 증감식으로 이동한다.
  if (string[i] !== search) continue;
  count++; // continue 문이 실행되면 이 문은 실행되지 않는다.
}

console.log(count); // 3

// 참고로 String.prototype.match 메소드를 사용해도 같은 동작을 한다.
const regexp = new RegExp(search, 'g');
console.log(string.match(regexp).length); // 3

위 예제의 for 문은 아래와 동일하게 동작한다.
for (var i = 0; i < string.length; i++) {
  // 'l'이면 카운트를 증가시킨다.
  if (string[i] === search) count++;
}
```

- if문 내에서 실행해야 할 코드가 한 줄이라면 continue문을 사용하지 않는 것이 간편하며 가독성도 좋음
  - 하지만 if문 내에서 실행해야 할 코드가 길다면 들여쓰기가 한 단계 깊어지므로 continue문을 사용하는 것이 가독성이 더 좋음

```javascript
// continue 문을 사용하지 않으면 if 문 내에 코드를 작성해야 한다.
for (var i = 0; i < string.length; i++) {
  // 'l'이면 카운트를 증가시킨다.
  if (string[i] === search) {
    count++;
    // code
    // code
    // code
  }
}

// continue 문을 사용면 if 문 밖에 코드를 작성할 수 있다.
for (var i = 0; i < string.length; i++) {
  // 'l'이 아니면 카운트를 증가시키지 않는다.
  if (string[i] !== search) continue;

  count++;
  // code
  // code
  // code
}
```



모든 출처 : https://poiemaweb.com/

