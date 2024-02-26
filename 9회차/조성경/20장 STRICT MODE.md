
## 20.1 strict mode(엄격모드)

ES5부터 strict mode(엄격 모드)가 추가되었다. strict mode는 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 최적화 문제를 일으키는 코드에 명시적인 에러를 발생시킨다.

## 20.2 strict mode의 적용

strict mode를 적용하기 위해서 전역의 선두 또는 함수 선두에 `use strict`를 추가 한다.

```js
'use strict';

function foo() {
	x = 10; // ReferenceError: x is not defined
}
```
만약 `strict mode`가 아니었다면 javascript 엔진은 전역에 임의로 변수 x를 선언했다.

## 20.3 전역에 strict mode를 적용하는 것은 피하자
```html
<body>
	<script>
		'use strict';
	</script>
	<script>
		x = 1; //에러가 발생하지 않는다.
	</script>
```

이렇게 혼용할 경우 원치 않는 오류가 발생할수 있다.
그래서 전역으로 적용하지 않는다.

## 20.4 함수 단위로도 피하자
함수 단위로 적용은 가능하다 어떤 함수는 적용하고 어떤 함수는 적용하지 않고도 바람직하지 않음
모든 함수에 하나하나 `strict mode`를 적용하는 것도 번거롭다

따라서 strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.
```js
(function () {
	'use strict';
// Do something ...
}());
```

## 20.5 strict mode가 발생시키는 에러

### 20.5.1 암묵적 전역
선언하지 않은 변수를 참조하면 `Reference Error`가 발생한다.

### 20.5.2 변수, 함수, 매개변수의 삭제
`delete` 연산자로 변수, 함수, 매개변수를 삭제하면 `SyntaxError` 가 발생한다.

### 20.5.3. 매개 변수 이름의 중복
중복된 매개 변수 이름을 사용하면 `Syntax Error`가 발생

### 20.5.4 with 문의 사용
`with` 문을 사용하면 `SyntaxError`가 발생한다.
`with` 문은 전달된 객체를 스코프 체인에 추가 한다. 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있다.
하지만 성능, 가독성 문제로 사용하지 않는ㄱ ㅔ좋다.


## 20.6 strict mode 적용에 의한 변화

### 20.6.1 일반 함수의 this
`strict mode` 에서 함수를 일반 함수로서 호출하면 `this`에 `undefined`가 바인딩
생성자 함수가 아닌 일반 함수 내부에서는 `this`를 사용할 필요가 없기 때문이다.

```js
(function () {
	'use strict';
	function foo() {
		console.log(this);//undefined
	}
	foo(); 

	function Foo() {
		console.log(this); // Foo
	}
	new Foo();
}());
```

### 20.6.2 arguments 객체
`strict mode`에서는 매개변수에 전달된 인수를 재할당하여도 `arguments` 객체에 반영되지 않는다.

```js
(function (a) {
	'use strict';
	//매개 변수에 전달된 인수를 재할당하여 변경
	a = 2;
//변경된 인수를 arguments 객체에 반영되지 않는다.

	console.log(arguments); // {0: 1, length: 1);}
}(1));
```
