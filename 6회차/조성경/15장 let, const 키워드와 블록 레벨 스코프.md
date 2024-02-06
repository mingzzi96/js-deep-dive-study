## 15.1 var 키워드로 선언한 변수의 문제점
---
var 키워드로 선언된 변수는 다른 언어와 구별되는 독특한 특징이 있다. 그렇기 때문에 주의를 기울이지 않으면 심각한 문제를 발생 시킬 수 있다.

### 15.1 변수 중복 선언 허용
---
```javascript
var x = 1;
var y = 1;

//var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
//초기화 문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 행동한다.

var x = 100;
//초기화 문이 없는 선언문은 무시된다.
var y;

console.log(x); //100
console.log(y); //1
```

var 키워드로 선언한 변수를 중복 선언하면 초기화문(변수 선언과 동시에 초기값을 할당하는 문) 유무에 따라 다르게 동작한다. 만약 동일한 이름의 변수가 이미 선언되었는 지를 모르고 중복 선언하고 값까지 할동 되면 사이드 이펙트가 발생한다.

### 15.1.2 함수 레벨 스코프
---
var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다.
따라서 **함수 외부에서 var 키워드로 선언한 변수는 모두 전역변수**가 된다.

```javascript
var x = 1;

if ( true) {
	//x는 전역 변수다. 이미 선언된 전역 변수 x가 있으므로 중복 선언되었다.
	//이는 의도치 않게 변수 값이 변경되는 부작용을 발생시킨다.
	var x = 10;
}

console.log(x); // 10
```
외부에서 선언하였다면 for 문의 변수 선언문에서 var 키워드로 선언한 변수도 전역 변수가 된다.
```javascript
var i = 10;

//for문에서 선언한 i는 전역 변수다. 이미 선언된 전역변수가 i가 있으므로 중복 선언된다.

for ( var i = 0; i < 5; i++){
	console.log(i); // 0 1 2 3 4 
}

//의도치 않게 i변수의 값이 변경되었다.
console.log(i); // 5
```
### 15.1.3 변수 호이스팅
---
var 키워드로 선언하면 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어 올려진것처럼 동작한다.
즉, 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변서 선언문 이전에 참조할 수 있다. **단, 할당문 이전에 변수를 참조하면 언제나 undefined를 반환**한다.

## 15.2 let 키워드
---
앞에 말한 var 키워드의 단점을 보완하기 위해 ES6에서는 새로운 변수 선언 키워드인 let과 const를 도입했다. 먼저 let부터!

### 15.2.1 변수 중복 선언 금지
---
var은 이름이 동일한 변수를 중복 서넌하면 아무런 에러가 발생하지 않았다. 이 때 변수를 중복 선언하고 값까지 할당했다면 의도치 않게 먼저 선언된 변수값이 재할당 되어 변경되는 무작용이 있었다.

하지만 let 키워드는 동일한 변수를 중복 선언하면 문법 에러(syntax error)가 발생한다.

```javascript
var foo = 123;
// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용
// 아래 var 변수 선언문은 자바스크립트 엔진에 의해 이전의 var로 선언한 변수가 없는 것처럼 동작한다.
var foo = 456;

let bar = 123;
// let이나 const 키워드로 선언되 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
let bar = 456; //SyntaxError : Identifier 'bar' has already been declared
```

### 15.2.2 블록 레벨 스코프
---
var 키워드로 선언한 변수는 오직 함수의 코드 블록만을 지역 스코프로 인정하는 함수 레벨 스코프를 따른다.
let 키워드로 선언한 변수는 모든 코드 블룩(함수, if문, for문, while문, try/catch 문 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

```javascript
let foo = 1 ; //전역 변수

{
	let foo = 2;
	let bar = 3;
}

console.log(foo); // 1
console.log(bar) ; //ReferenceError : bar is not defined
```

### 15.2.3 변수 호이스팅
---
var 키워드로 선언한 변수와 달리 let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처림 동작한다.
```javascript
console.log(foo); //Reference Error : foo is not defined
let foo;
```
var 키워드로 선언한 변수는 런타임 이전에 자바스크립트 엔진에서 암묵적으로 "선언 단계", "초기화 단계"가 한번에 진행된다.
즉, 선언 단계에서 스코프(실행 컨텍스트의 렉시컬 환경)에 변수 식별자를 등록해 자바스크립트 엔진에 변수의 존재를 알린다. 그리고 초기화 단계에서 undefined로 변수를 초기화 한다.

따라서 변수 선언문 이전에 변수에 접근해도 스코프에 변수가 존재하기 때문에 에러를 발생하지는 않지만 undefined를 반환한다.
![](https://i.imgur.com/1floK9j.png)


**let 키워드로 선언한 변수는 " 선언 단계"와 "초기화 단계"가 분리되어 진행된다.**
즉, 초기화 단계는 변수 선언문에 도달했을 때 실행된다.
만약 초기화 단계까 실행되기 이전에 변수에 접근하려고 하면 참조 에러(Reference Error)가 발생한다.
let 키워드로 선언한 변수는 스코프의 시작 지점부터 초기화 단계 시작 지점(변수 선언문) 까지는 변수를 참조할 수 없다. 이 구간을 **일시적 사각지대(Temporal Dead Zone)** 라고 부른다.

![](https://i.imgur.com/J9sxFhE.png)

```javascript
//런타임 이전에 선언 단계까 실행된다. 아직 변수가 초기화되지 않았다.
//초기화 이전의 일시적 사각지대에서는 변수를 참조할 수 없다.
console.log(foo); //referenceError : foo is not defined

let foo; // 변수 선언문에서 초기화 단계가 실행된다.
console.log(foo); //undefined

foo = 1; //할당문에서 할당 단계까 실행된다.
console.log(foo) // 1
```
let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 보인다. 하지만 다음 예제를 살펴보자.

```javascript
let foo = 1;
{
	console.log(foo) //ReferenceError : cannot access 'foo' before initialization
	let foo = 2; //지역 변수
}
```
let 키워드로 선언한 변수가 호이스팅이 발생하지 않는다면 위 예제는 전역 변수 foo 값을 출력해야 한다. 하지만 호이스팅이 발생하기 때문에 참조 에러(Reference Error)가 발생한다.

자바스크립트는 ES6에 도입된 let, const를 포함해서 모든 선언 var, let, const, function, function*, class 등)을 호이스팅 한다. 단 ES6에 도입된 let, const, class를 사용한 선언문은 호이스팅이 발생하지 않는 것처럼 동작한다.

### 15.2.4 전역 객체와 let
---
var  키워드로 선언한 전역 변수와 전역 함수 그리고 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역 객체 windows의 프로퍼티가 된다. 

```javascript
//이 예제는 브라우저 환경에서 실행해야 한다.

//전역 변수
var x = 1;
// 암묵적 전역
y = 2;

// 전역 함수
function foo() {}

//var 키워드로 선언한 전역변수는 전역 객체 windows의 프로퍼티다.
console.log(window.x);
//전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있따.
console.log(X); // 1

//암묵적 전역은 전역 객체 windows의 프로퍼티다.
console.log(window.y); // 2
console.log(y) // 2

//함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티다.
console.log(window.foo); // f foo() {}
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(foo);
```

let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다. 즉 window.foo와 같이 접근할 수가 없다.
let 전역 변수는 보이지 않는 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드) 내에 존재하게 된다.

```javascript
//이 예제는 브라우저 환경에서 실행해야 한다.
let x = 1;

//let, const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아니다.
console.log(window.x); //undefined
console.log(X); // 1
```

## 15.3 const 키워드
---
const 키워드는 상수를 선언하기 위해 사용한다. 하지만 반드시 상수만을 위해 사용하지는 않는다.
let 키워드와 대부분 동일하므로 다른 점을 중심으로 살펴보자.

### 15.3.1 선언과 초기화
---
const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화 해야 한다.
```javascript
const foo = 1;
```
그렇지 않으면 다음과 같이 문법 에러가 발생한다.
```javascript
const foo; //SyntaxError : Missing initializer in const declaration
```
const 키워드로 선언한 변수는 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작 한다.
```javascript
{
	//변수 호이스팅이 발생하지 않는 것처럼 동작한다.
	console.log(foo); //referenceError : cannot access 'foo' before initialization
	const foo = 1;
	console.log(foo); // 1
}

//블록 레벨 스코프를 갖는다.
console.log(foo); // ReferenceError: foo is not defined
```
### 15.3.2 재할당 금지
var 또는 let 키워드로 선언한 변수는 재할당이 자유로우나  **const 키워드로 선언한 변수는 재할당이 금지 된다.**

```javascript
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable
```

### 15.3.3 상수
const 키워드로 선언한 변수에 원시 값을 할당한 경우 변수 값을 변경할 수 없다.
**즉 상수는 재할당이 금지된 변수를 말한다.** 상수도 값을 저장하기 위한 메모리 공간이 필요하므로 변수라고 할 수 있다. 

상수는 상태 유지와 가독성, 유지보수의 편의를 위해 적극적으로 사용해야 한다.
```javascript
//세전 가격
let preTaxPrice = 100;

//세후 가격
//0.1의 의미를 명확히 알기 어렵기 때문에 가독성이 좋지 않다.
let afterTaxPrice = preTaxPrice + (preTaxPrice * 0.1);
console.log(afterTaxPrice); //110
```
코드 내에서 사용한 0.1은 어떤 의미로 사용했는지 명확히 알기 어렵기 때문에 가독성이 좋지 않다.
세율의 0.1 은 쉽게 바뀌지 않는 값이며, 프로그램 전체에서 고정된 값ㅇ르 사용해야 한다.

const 키워드로 선언된 변수에 원시 값을 할당한 경우 원시 값은 변경할 수 없는 값(immutable value)이고 const 키워드에 의해 재할당이 금지되므로 할당된 값을 변경할 수 있기 때문이다.
```javascript
// 세율을 의미하는 0.1은 변경할 수 없는 상수로서 사용될 값이다.
// 변수 이름을 대문자로 선언해 상수임을 명확히 나타낸다.
const TAX_RATE = 0/1;

//세전 가격
let preTaxPrice = 100;

//세후 가격
let afterTaxPrice = preTaxPrice + (preTaxPrice * TAX_RATE);

console.log(afterTaxPrice);
```

### 15.3.4 const 키워드와 객체
const 키워드로 선언된 변수에 원시 값을 할당한 경우 값은 변경할 수 없다. 하지만 **const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다.**

```javascript
const person = {
	name: 'Lee'
};

//객체는 변경 가능한 값이다. 따라서 재할당 없이 변경이 가능하다.
person.name = 'Kim';

console.log(person); // {name: "Kim"}
```
**const 키워드는 재할당을 금지할 뿐 "불변"을 의미하지는 않는다.**
즉 새로운 값을 재할당 하는 것은 불가능하지만 프로퍼티 동적 생성, 삭제, 프로퍼티 값의 변경을 통해 객체를 변경하는 것은 가능하다.

## 15.4 var vs let vs const
---
변수 선언에는 기본적으로 const를 사용하고 let은 재할당이 필요한 경우에 한정해 사용하는 것이 좋다.
const 키워드를 사용하면 의도치 않은 재할당을 방지하기 때문에 좀 더 안전하다.

var와 let, const 키워드는 다음과 같이 사용하는 것을 권장하다.
- ES6를 사용한다면 var 키워드는 사용하지 않는다.
- 재할당이 필요한 경우에 한정해 let 키워드를 사용한다. 이때 변수의 스코프는 최대한 좁게 만든다.
- 변경이 발생하지 않고 읽기 전용으로 사용하는 원시값과 객체에는 const 키워드를 사용한다.


