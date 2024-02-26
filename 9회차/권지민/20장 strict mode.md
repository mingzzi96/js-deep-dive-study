# 📚 strict mode

## 🎀 strict mode란?

잠재적인 오류를 발생시키기 어려운 개발 환경을 만들고 그 환경에서 개발하는 것이다.

## 🎀 strict mode의 적용

```js
'use strict';

function foo(){
	x = 10; // RefenceError: x is not defined
}
foo();
```
함수 몸체의 선두에 추가하면 해당 함수와 중첩 함수에 strict mode가 적용된다.

```js
function foo(){
  	'use strict';
	x = 10; // RefenceError: x is not defined
}
foo();
```
코드의 선두에 `'use strict';`를 위치시키지 않으면 제대로 동작하지 않는다.

## 🎀 전역에 strict mode를 적용하는 것은 피하자

전역에 적용한 strict mode는 스크립트 단위로 적용된다.

```html
<!DOCTYPE html>
<html>
  <body>
    <script>
      'use strict';
    </script>
    <script>
      x = 1; // 에러를 발생시키지 않는다.
      console.log(x); // 1
    </script>
    <script>
      'use strict';
      y = 1; // RefenceError: y is not defined
    </script>
  </body>
</html>
```

strict mode 스크립트와 non-strict mode 스크립트를 혼용하는 것은 오류를 발생시킬 수도 있고

외부 서드파티 라이브러리를 사용하는 경우 라이브러리가 non-strict mode 인 경우도있기 때문에 전역 strict mode를 적용하는 것은 바람직하지 않다.

```js
(function () {
	'use strict`;
  	// do something...
}());
```
이러한 경우 즉시 실행함수로 스크립트 전체를 감사서 스코프를 구분하고

즉시 실행 함수 선두에 strict mode를 적용한다.

## 🎀 함수 단위로 strict mode를 적용하는 것도 피하자

일부 함수에만 strict mode를 적용하는 것도 바람직하지 않다.

따라서 strict mode는 즉시 실행함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

```js
(function () {
  // non-strict mode
  var let = 10; // 에러가 발생하지 않는다.
  
  function foo() {
    'use strict';
    let = 20;// SyntaxError: Unexpected strict mode reserve word
  }
  foo();
}());
```

## 🎀 strict mode가 발생시키는 에러

### 📌 암묵적 전역

선언하지 않은 변수를 참조하면 ReferenceError가 발생한다.

```js
(function () {
'use strict';

x = 1;
console.log(x); // / RefenceError: x is not defined
```

### 📌 변수, 함수, 매개변수의 삭제

delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError가 발생한다.

```js
(function () {
'use strict';

var x = 1;
delete x; // SyntaxError: Delete of an unqualified identifier in strict mode
function foo(a) {
  delete a; // SyntaxError: Delete of an unqualified identifier in strict mode
}
delete foo; // SyntaxError: Delete of an unqualified identifier in strict mode
}());
```
### 📌 매개변수 이름의 중복

중복된 매개변수 이름을 사용하면 SyntaxError가 발생한다.

```js
(function () {
'use strict';
// SyntaxError: Duplicate parameter name not allowed in this context
function foo(x, x){
  return x + x;
}
console.log(foo(1,2));
}());
```
### 📌 with 문의 사용

with 문을 사용하면 SyntaxError가 발생한다.

```js
(function () {
'use strict';

// SyntaxError: Strict mode code may not include a with statment
with({x:1}) {
  console.log(x);
}
}());
```

## 🎀 strict mode 적용에 의한 변화

### 📌 일반 함수의 this

strict mode에서 함수를 일반 함수로서 호출하면 this에 `undefined`가 바인딩된다.

생성자 함수가 아닌 일반 함수에서는 this를 사용할 필요가 없기 때문이다. 이때 에러는 발생하지 않는다.

```js
(function() {
  'use strict';
  
  function foo(){
    console.log(this); // undefined
  }
  foo();
  
  function Foo(){
    console.log(this); // Foo
  }
  new Foo();
}());
```
### 📌 arguments 객체

strict mode 에서는 매개변수에 전달된 인수를 재할당하여 변경해도 `arguments` 객체에 반영되지 않는다.

```js
(function (a) {
  'use strict';
  // 매개 변수에 전달된 인수를 재할당하여 변경
  a = 2;
  
  // 변수 값은 재할당이 된다.
  console.log(a); // 2 
  // 변경된 인수가 arguments 객체에 반영되지 않는다.
  console.log(arguments); // { 0: 1, length: 1 }
}(1));
```
