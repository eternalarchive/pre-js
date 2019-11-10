# 모듈(module)

- 모듈이란 애플리케이션을 구성하는 개별적 요소로서 재사용 가능한 코드 조각
- 모듈은 세부 사항을 캡슐화하고 공개가 필요한 API만을 외부에 노출

![module-pattern](https://poiemaweb.com/img/module-pattern.png)

- 일반적으로 **모듈은 파일 단위로 분리**되어 있으며 애플리케이션은 필요에 따라 **명시적으로 모듈을 로드하여 재사용**
  - 즉, 모듈은 애플리케이션에 분리되어 개별적으로 존재하다가 애플리케이션의 로드에 의해 비로소 애플리케이션의 일원이 됨
  - 모듈은 기능별로 분리되어 작성되므로 코드의 단위를 명확히 분리하여 애플리케이션을 구성할 수 있으며 재사용성이 좋아서 개발 효율성과 유지보수성을 높일 수 있음
- 클라이언트 사이드 자바스크립트는 script 태그를 사용하여 외부의 스크립트 파일을 가져올 수는 있지만, 파일마다 독립적인 파일 스코프를 갖지 않고 하나의 전역 객체(Global Object)에 바인딩되기 때문에 전역 변수가 중복되는 등의 문제가 발생할 수 있음

- 자바스크립트를 클라이언트 사이드에 국한하지 않고 범용적으로 사용하고자 하는 움직임이 생기면서 모듈 기능은 반드시 해결해야 하는 핵심 과제가 되었고 이런 상황에서 제안된 것이 [CommonJS](http://www.commonjs.org/)와 [AMD(Asynchronous Module Definition)](https://github.com/amdjs/amdjs-api/wiki/AMD)

- 결국, 자바스크립트의 모듈화은 크게 CommonJS 진영과 AMD 진영으로 나뉘게 되었고 브라우저에서 모듈을 사용하기 위해서는 CommonJS 또는 AMD를 구현한 모듈 로더 라이브러리를 사용해야 하는 상황

- 서버 사이드 자바스크립트인 Node.js는 사실상 모듈 시스템의 사실상 표준(de facto standard)인 CommonJS를 채택하였고 현재는 독자적인 진화를 거쳐 CommonJS 사양과 100% 동일하지는 않지만 기본적으로 CommonJS 방식을 따르고 있음

- 이러한 상황에서 ES6에서는 클라이언트 사이드 자바스크립트에서도 동작하는 모듈 기능을 추가
  - 2019년 5월 현재, 모던 브라우저(Chrome 61, FF 60, SF 10.1, Edge 16 이상)에서 ES6 모듈을 사용할 수 있음
  - [브라우저의 ES6 모듈 지원 현황](https://caniuse.com/#search=module)

- script 태그에 `type="module"` 어트리뷰트를 추가하면 로드된 자바스크립트 파일은 모듈로서 동작
  - 모듈의 파일 확장자는 mjs를 권장

```html
<script type="module" src="lib.mjs"></script>
<script type="module" src="app.mjs"></script>
```

- 단, 아래와 같은 이유로 아직까지는 브라우저가 지원하는 ES6 모듈 기능보다는 [Webpack](https://webpack.js.org/) 등의 모듈 번들러를 사용하는 것이 일반적
  - IE를 포함한 구형 브라우저는 ES6 모듈을 지원하지 않는다.
  - 브라우저의 ES6 모듈 기능을 사용하더라도 트랜스파일링이나 번들링이 필요하다.
  - 아직 지원하지 않는 기능(Bare import 등)이 있다. ([ECMAScript modules in browsers](https://jakearchibald.com/2017/es-modules-in-browsers/) 참고)
  - 점차 해결되고는 있지만 아직 몇가지 이슈가 있다. ([ECMAScript modules in browsers](https://jakearchibald.com/2017/es-modules-in-browsers/) 참고)

- ES6를 사용하여 프로젝트를 진행하려면 ES6로 작성된 코드를 IE를 포함한 모든 브라우저에서 문제 없이 동작시키기 위한 개발환경을 구축하는 것이 필요
  - 이 문제에 대해서는 [Babel과 Webpack을 이용한 ES6 개발환경 구축①과 Babel과 Webpack을 ES6 개발환경 구축②에서 알아볼 것
  - 이 장에서는 ES6 모듈의 기본 문법에 대해서만 살펴보도록 하자

## 1. 파일 스코프

- 모듈은 **파일 스코프**를 가짐
  - 즉, 모듈 내에서 var 키워드로 선언한 변수는 더 이상 전역 변수가 아니며 window 객체의 프로퍼티도 아님

```javascript
// foo.js
var x = 'foo';

console.log(x);
// bar.js
// 중복 선언이다.
var x = 'bar';

console.log(x);
<!DOCTYPE html>
<html>
<body>
  <script src="foo.js"></script>
  <script src="bar.js"></script>
</body>
</html>
```

- 위와 같이 2개의 자바스크립트 파일을 로드하면 하나의 전역을 공유
  - 다시 말해 2개의 자바스크립트 파일은 하나의 전역 객체를 공유하며 하나의 전역 스코프를 가지므로따라서 전역 변수 x는 중복되며 변수 x의 값은 덮어 써짐

- ES6 모듈은 파일 자체의 스코프를 제공
  - 즉, 파일별로 구분한 모듈은 파일 스코프를 가짐

```javascript
// foo.js
var x = 'foo';

console.log(x);
// bar.js
// 중복 선언이 아니다. 스코프가 다른 변수이다.
var x = 'bar';

console.log(x);
<!DOCTYPE html>
<html>
<body>
  <script type="module" src="foo.js"></script>
  <script type="module" src="bar.js"></script>
</body>
</html>
```

- 모듈 내에서 선언한 변수는 모듈 외부에서 참조할 수 없음
  - 스코프가 다르기 때문

```javascript
// foo.js
const x = 'foo';

console.log(x); // foo
// bar.js
// 다른 모듈에서 선언한 변수는 모듈 외부에서 참조할 수 없다. 스코프가 다르기 때문이다.
console.log(x); // ReferenceError: x is not defined
<!DOCTYPE html>
<html>
<body>
  <script type="module" src="foo.js"></script>
  <script type="module" src="bar.js"></script>
</body>
</html>
```

## 2. export 키워드

- 모듈은 독립적인 파일 스코프를 갖기 때문에 모듈 안에 선언한 모든 것들은 기본적으로 해당 모듈 내부에서만 참조할 수 있음
- 만약 모듈 안에 선언한 항목을 외부에 공개하여 다른 모듈들이 사용할 수 있게 하고 싶다면 export해야 함
  - 선언된 변수, 함수, 클래스 모두 export할 수 있음

- 모듈을 공개하려면 선언문 앞에 export 키워드를 사용
- 여러 개를 export할 수 있는데 이때 각각의 export는 이름으로 구별할 수 있음

```javascript
// lib.js
// 변수의 공개
export const pi = Math.PI;

// 함수의 공개
export function square(x) {
  return x * x;
}

// 클래스의 공개
export class Person {
  constructor(name) {
    this.name = name;
  }
}
```

- 선언문 앞에 매번 export 키워드를 붙이는 것이 싫다면 export 대상을 모아 하나의 객체로 구성하여 한번에 export할 수도 있음

```javascript
// lib.js
const pi = Math.PI;

function square(x) {
  return x * x;
}

class Person {
  constructor(name) {
    this.name = name;
  }
}

// 변수, 함수 클래스를 하나의 객체로 구성하여 공개
export { pi, square, Person };
```

## 3. import 키워드

- export한 모듈을 로드하려면 export한 이름으로 import

```javascript
// app.js
// 같은 폴더 내의 lib.js 모듈을 로드. 확장자 js는 생략 가능.
// 단, 브라우저 환경에서는 모듈의 파일 확장자를 생략할 수 없다.
import { pi, square, Person } from './lib';

console.log(pi);         // 3.141592653589793
console.log(square(10)); // 100
console.log(new Person('Lee')); // Person { name: 'Lee' }
```

- 각각의 이름을 지정하지 않고 하나의 이름으로 한꺼번에 import할 수도 있음
  - 이때 import되는 항목은 as 뒤에 지정한 이름의 변수에 할당

```javascript
// app.js
import * as lib from './lib';

console.log(lib.pi);         // 3.141592653589793
console.log(lib.square(10)); // 100
console.log(new lib.Person('Lee')); // Person { name: 'Lee' }
```

- 이름을 변경하여 import할 수도 있음

```javascript
// app.js
import { pi as PI, square as sq, Person as P } from './lib';

console.log(PI);    // 3.141592653589793
console.log(sq(2)); // 4
console.log(new P('Kim')); // Person { name: 'Kim' }
```

- 모듈에서 하나만을 export할 때는 default 키워드를 사용할 수 있음
  - 다만, default를 사용하는 경우, var, let, const는 사용할 수 없음

```javascript
// lib.js
function (x) {
  return x * x;
}

export default;
```

- 위 코드를 아래와 같이 축약 표현할 수 있음

```javascript
// lib.js
export default function (x) {
  return x * x;
}
```

- default 키워드와 함께 export한 모듈은 {} 없이 임의의 이름으로 import

```javascript
// app.js
import square from './lib';

console.log(square(3)); // 9
```

- 브라우저가 지원하는 ES6 모듈 기능을 이용하여 import와 export가 동작하는지 확인해보자

```javascript
// lib.js
export default x => x * x;
// app.js
// 브라우저 환경에서는 모듈의 파일 확장자를 생략할 수 없다.
// 모듈의 파일 확장자는 .mjs를 권장한다.
import square from './lib.js';

console.log(square(10)); // 100
<!DOCTYPE html>
<html>
<body>
  <script type="module" src="./lib.js"></script>
  <script type="module" src="./app.js"></script>
</body>
</html>
```

- 위 HTML을 실행해보면 콘솔에 100이 출력



Reference : https://poiemaweb.com