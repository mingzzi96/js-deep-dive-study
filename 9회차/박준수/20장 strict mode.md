## strict mode란?
> strict mode(엄격 모드)는 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 
- 오류를 발생시킬 가능성이 높거나 
- 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드
>
에 대해 명시적인 에러를 발생한다.

strict mode는 ES5에 추가되었고 ESLint 같은 린트 도구를 사용해도 유사한 효과를 얻을 수 있다.
```
린트 도구

정적 분석 기능을 통해 소스코드를 실행하기 전에 소스코드를 스캔하여 문법적 오류만이 아니라 잠재적인 오류까지 찾아내고 오류의 원인을 리포팅해주는 유용한 도구.
린트 도구는 strict mode가 제한하는 오류는 물론 코딩 컨벤션도 설정할 수 있다.
```

<br>
<br>

## strict mode의 적용
전역의 선두 또는 함수 몸체의 선두에 `use strict`를 추가한다.

   - 전역의 선두: 스크립트 전체 적용
   - 함수 몸체의 선두: 해당 함수와 중첩 함수에 적용  
   <br>
   
### 전역에 적용하는 것은 피하자
스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주기 않고 해당 스크립트에 한정되어 적용된다.

❗️ <span style='color: orange'>하지만 strict mode와 non-strict mode 스크립트를 혼용하면 오류를 발생시킬 수 있다.</span>
특히 외부 서드파티 라이브러리 중에 non-strict mode인 경우가 있다.

그래서 만약 혼용해서 사용하고 있다면
```js
(function () {
	'use strict`;
    
    // Do something...
 }());
```
위 처럼 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분해주자.
 <br>

### 함수 단위로 적용하는 것도 피하자
위와 마찬가지로 어떤 함수는 strict mode를 어떤 함수는 non-strict mode를 사용하는 것은 바람직하지 않다.
그렇다고 모든 함수에 다 적용을 해주자니 너무나도 번거롭다.
그리고 strict mode가 적용된 함수가 참조할 외부 컨텍스트에 strict mode를 적용시키지 않는다면 문제가 발생할 수 있다.

<br>
<br>

## strict mode가 발생시키는 에러
### 1. 암묵적 전역
선언하지 않은 변수를 참조하면 ReferenceError가 발생한다.
```js
(function () {
  'use strict';
  
  x = 1;
  console.log(x); // ReferenceError: x is not defined
}()}
```

### 2. 변수, 함수, 매개변수의 삭제
delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError가 발생한다.
```js
(function () {
  'use strict';
  
  var x = 1;
  delete x; // SyntaxError: Delete of an unqualified identifier in strict mode.
  
  function foo(a) {
    delete a; // SyntaxError: Delete of an unqualified identifier in strict mode.
  }
  delete foo; // SyntaxError: Delete of an unqualified identifier in strict mode.
}());
```

### 3. 매개변수 이름의 중복
중복된 매개변수 이름을 사용하면 SyntaxError가 발생한다.
```js
(function () {
  'use strict';
  
  // SyntaxError: Duplicate parameter name not allowed in this context
  function foo(x, x) {
    return x + x;
  }
  console.log(foo(1, 2));
}());
```

### 4. with 문의 사용
with 문을 사용하면 SyntaxError가 발생한다.
```
with 문은 전달된 객체를 스코프 체인에 추가한다. with 문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서 코드가 간단해지지만 성능과 가독성이 나빠진다. 따라서 사용하지 않는 것이 좋다.
```
```js
(function () {
  'use strict';
  
  // SyntaxError: Strict mode code may not include a with statement
  with({ x: 1 }) {
    console.log(x);
  }
}());
```
<br>
<br>

## strict mode 적용에 의한 변화
### 일반 함수의 this
strict mode에서 함수를 일반 함수로서 호출하면 this에 undefined가 바인딩된다. 생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문이다. 이때 에러는 발생하지않는다.
```js
(function () {
  'use strict';
  
  function foo(x, x) {
    console.log(this) // undefined
  }
  foo();
  
  function foo() {
    console.log(this); // Foo
  }
  new Foo();
}());
```

### arguments 객체
strict mode에서는 매개변수에 전달된 인수를 재할당하려 변경해도 arguments 객체에 반영되지 않는다.
```js
(function () {
  'use strict';
  
  // 매개변수에 전달된 인수를 재할당하여 변경
  a = 2;
  
  // 변경된 인수가 arguments 객체으ㅔ 반영되지 않는다.
  console.log(arguments); // { 0: 1, length: 1 }
}(1));
```
