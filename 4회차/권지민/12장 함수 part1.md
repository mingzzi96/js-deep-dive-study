# 📚 함수
함수는 일련의 과정을 문으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것이다.

이때 함수 내부로 입력을 전달 받는 변수를 매개 변수(parameter), 입력을 인수(argument), 출력을 반환값(return value)라 한다.

자바스크립트의 함수는 객체 타입의 값이다

## 🎀 함수 정의
변수는 "선언", 함수는 "정의"라고 표현한다.

함수 선언문이 평가되면 식별자가 암묵적으로 생성되고 함수 객체가 할당되기 때문이다.

### 📌 함수 선언문
자바스크립트 엔진은 생성된 함수를 호출하기 위해 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하고, 거기에 함수 객체를 할당한다.

즉, 함수는 함수 이름으로 호출하는 것이 아니라, 함수 객체를 가리키는 식별자로 호출한다.

```js
function add(x, y) {
  return x+y;
}

console.log(add(2, 5)); // 7
```


함수 선언문으로 생성한 함수를 호출한 것은 함수 이름 add가 아니라 자바스크립트 `엔진이 암묵적으로 생성한 식별자 add`인 것이다.


### 📌 함수 표현식
자바스크립트의 함수는 값처럼 변수에 할당 할 수도 있고 프로퍼티 값이 되거나 배열의 요소가 될 수 있는데, 이런 객체를 두고 일급 객체라 한다.

함수는 일급 객체이므로 함수 리터럴로 생성한 함수 객체를 변수에 할당할 수 있다. 이를 함수 표현식이라 한다.

```js
var add = function (x,y){
  return x+y;
}

console.log(add(2, 5)); // 7
```

함수 리터럴의 함수 이름은 생략할 수 있다. 이러한 함수를 익명 함수라 한다.



```js
var add = function(x, y){
  return x+y;
}

console.log(add(2, 5)); // 7
```
### 📌 함수 생성 시점과 함수 호이스팅
```js
console.log(add(2, 5)); // 7

function add(x, y) {
  return x+y;
}
```
함수 선언문으로 정의한 함수는 호이스팅이 일어난다.

```js
console.log(add(2, 5)); // TypeError: add is not a function

var add = function (x,y){
  return x+y;
}
```
하지만 함수 표현식으로 정의한 함수는 호이스팅이 없다. **함수 선언문으로 정의한 함수와 함수 표현식으로 정의한 함수의 생성 시점이 다르다.**

함수 선언문 대신 함수 표현식을 사용할 것을 권장한다.

### 📌 Function 생성자 함수
```js
var add = new Function('x', 'y', 'return x+y');
```

Function 생성자 함수로 함수를 생성하는 방식은 일반적이지 않고 바람직하지도 않다.

### 📌 화살표 함수
```js
var add = (x,y) => x + y;
```
화살표 함수는 항상 익명 함수로 정의한다.


## 🎀 함수 호출

### 📌 매개변수와 인수

<img width="530" alt="image" src="https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/636ef489-5e76-48fd-82c3-8e57fbf7b078">


1. 인수가 부족해서 할당되지 않은 매개변수의 값은 `undefined`이다.
2. 인수 갯수가 초과된다면 무시된다. (실제로는 버려지는 것이 아니라 프로퍼티로 보관되어지긴 한다.)
3. 자바스크림트 함수는 매개변수와 인수의 개수가 일치하는지 확인하지 않는다.
4. 자바스크립트 함수는 매개변수의 타입을 사전에 지정할 수 없다.
  ```js
  function add(x, y) {
    if(typeof x !== 'number' || typeof y !== 'number'){
      // 매개변수를 통해 전달된 인수의 타입이 부적절한 경우를 잡아낸다.
    }
  }
  ```
   5. arguments 객체는 함수를 정의할 때 매개변수 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용하게 사용된다.

  ```js
  function add(x, y) {
    console.log(arguments);
    // Arguments(3) [2, 5, 10, callee: f, Symbol(Symbol.iterator):f]
    return x + y;
  }
  
  add(2, 5, 10)
  ```
6. 이상적 매개변수 개수는 0개. 매개변수 개수가 많다는 것은 함수가 여러 가지 일을 한다는 증거이므로 바람직하지 않다. **이상적인 함수는 가급적 작게 만들어야 한다.**

### 📌 반환문
```js
function multiply(x, y) {
  return x * y; // 반환문
  console.log("실행되지 않는다.");
}

console.log(multiply(3, 5); // 15
```
1. 반환문은 함수의 실행을 중단하고 함수를 빠져나간다.
2. 반환문은 return 키워드 뒤에 오는 표현식을 평가해 반환한다.
3. 반환문은 생략할 수 있다. 이때 함수는 몸체의 마지막 문까지 실행한 후 암묵적으로 undefined를 반환한다.
4. 반환문은 함수 몸체 내부에서만 사용 가능하다. 전역에서 반환문 사용하면 `SyntaxError: Illegal return statement` 발생한다.
