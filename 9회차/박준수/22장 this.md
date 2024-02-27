객체는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조다.

메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 한다.

메서드가 자신이 속한 객체의 프로퍼티를 참조하려면?
먼저 <span style='color: #50bcdf'>자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.</span>

## 🍀 this를 사용하는 이유
객체 리터럴 방식으로 생성한 객체의 경우 메서드 내부에서 메서드 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다.

```js
const circle = {
    radius: 5;
    getDiameter() {
      // 이 메서드가 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면
      // 자신이 속한 객체인 circle을 참조할 수 있어야 한다.
      return 2 * circle.radius
    }
};
console.log(circle.getDiameter()); // 10
```

하지만 자기 자신이 속한 객체를 재귀적으로 참조하는 방식은 일반적이지도 바람직하지도 않다.
  
 <br> 

다음으로 생성자 함수 방식으로 인스턴스를 생성하는 경우를 살펴보자.

생성자 함수 내부에서는 프로퍼티 또는 메서드를 추가하기 위해 자신이 생성할 인스턴스를 참조할 수 있어야 한다.
하지만 생성자 함수에 의한 객체 생성 방식은 먼저 생성자 함수를 정의한 이후, new 연산자와 함께 생성자 함수를 호출하는 단계가 추가로 필요하다. 즉, 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수가 존재해야 한다.

생성자 함수를 정의하는 시점에는 아직 인스턴스를 생성하기 이전이므로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
따라서 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 특수한 식별자가 필요하다.

이를 위해 자바스크립트는 `this`라는 특수한 식별자를 제공한다.


## 🍀 `this` 
> this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.
this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

<span style='color: #50bcdf'>this가 가리키는 값은 함수 호출 방식에 의해 동적으로 결정된다.</span>

this 사용 예제:
```js
const circle = {
  radius: 5,
  getDiameter() {
    // this는 메서드를 호출한 객체를 가리킨다.
    return 2 * this.radius;
  }
};

console.log(circle.getDiameter()); // 10
```
생성자 함수 this 사용 예제:
```js
function Circle(radius) {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
}

Circle.prototype.getDiameter = function () {
  return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
````

생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다. 이처럼 this는 상황에 따라 가리키는 대상이 다르다.

strict mode역시 this 바인딩에 영향을 준다.

this는 코드 어디에서든 참조 가능하다. 전역에서도 함수 내부에서도 참조할 수 있다.

하지만 this는 일반적으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다. strict mode가 적용된 일반 함수 내부의 this에는 undefined가 바인딩된다. 일반 함수 내부에서 this를 사용할 필요가 없기 때문이다.

## 🍀 함수 호출 방식과 `this` 바인딩
<span style='color: #50bcdf'>this 바인딩(this에 바인딩될 값)은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.</span>

❗️주의
동일한 함수도 다양한 방식으로 호출될 수 있다.
 - 일반 함수 호출
 - 메서드 호출
 - 생성자 함수 호출
 - Function.prototype.appply/call/bind 메서드에 의한 간접 호출
 
<br>

함수 호출 방식에 따라 this 바인딩이 어떻게 결정되는지 알아보자.

### 🌱 일반 함수 호출
<span style='color: #50bcdf'>기본적으로 this에는 전역 객체가 바인딩된다.</span>

<span style='color: #50bcdf'>전역 함수를 물론이고 중첩 함수를 일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩된다.</span>

<span style='color: #50bcdf'>일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 this에는 전역 객체가 바인딩된다.</span>


하지만 메서드 내에서 정의한 중첩 함수 또는 메서드에게 전달한 콜백 함수가 일반적으로 호출될 때 메서드 내의 중첩 함수 또는 콜백 함수의 this가 전역 객체를 바인딩하는 것은 문제가 있다.

중첩 함수 또는 콜백 함수는 외부 함수를 돕는 헬퍼 함수의 역할을 하므로 외부 함수의 일부 로직을 대신하는 경우가 대부분이다. 

하지만 외부 함수인 메서드와 중첩 함수 또는 콜백 함수의 this가 일치하지 않는다는 것은 중첩 함수 또는 콜백 함수를 헬퍼 함수로 동작하기 어렵게 만든다.

### 🌱 메서드 호출
메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 메서드 이름 앞의 마침료(.) 연산자 앞에 기술한 객체가 바인딩된다.

❗️주의
메서드 내부의 this는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다는 것이다.

### 🌱 생성자 함수 호출
생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.

### 🌱 Function.prototype.appply/call/bind 메서드에 의한 간접 호출
apply, call, bind 메서드는 Function.prototype의 메서드다.
즉, 이들 메서드는 모든 함수가 상속받아 사용할 수 있다.

#### 1. Function.prototype.appply/call 메서드
this로 사용할 객체와 인수 리스트릴 인수로 전달받아 함수를 호출한다.

<img src='https://velog.velcdn.com/images/kozel/post/9e700da6-e7b9-4ae1-b896-4a3d7d996171/image.jpeg' width='600'>

apply와 call 메서드의 사용법:
```js
/**
 * 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수를 호출한다.
 * @param thisArg - this로 사용할 객체
 * @param argArray - 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
 * @returns 호출된 함수의 반환값
 */
Function.prototype.apply(thisArg[, argsArray])

/**
 * 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수를 호출한다.
 * @param thisArg - this로 사용할 객체
 * @param arg1, agr2, ... - 함수에게 전달할 인수 리스트
 * @returns 호출된 함수의 반환값
 */
Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])
```

```js
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}
```

> apply와 call 메서드의 본질적인 기능은 함수를 호출하는 것이다.

apply와 call 메서드는 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩한다.

#### apply와 call의 차이?
apply와 call 메서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 동일하게 동작한다.

- apply: 호출할 함수의 인수를 배열로 묶어 전달한다.

- call: 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.

#### 용도
대표적인 용도는 arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우다. arguments 객체는 배열이 아니기 때문에 Array.prototype.slice 같은 배열의 메서드를 사용할 수 없으나 apply와 call 메서드를 이용하면 가능하다.


#### 2. Function.prototype.bind 메서드
apply와 call 메서드와 달리 함수를 호출하지 않는다.
다만 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.
```js
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// bind 메서드는 첫 번째 인수로 전달한 thisArg로 this 바인딩이 교체된
// getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding

// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```
bind함수는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 유용하게 사용된다.

```js
const person = {
  name: 'Lee',
  foo(callback) {
    // ①
    setTimeout(callback, 100);
  }
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // ② Hi! my name is .
  // 일반 함수로 호출된 콜백 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
  // 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
  // Node.js 환경에서 this.name은 undefined다.
});
````
`person.foo` 메서드가 호출되는 ①의 시점, 콜백 함수가 호출되기 직전의 `this`값은 `foo`메서드를 호출한 객체, 즉 `person`이다.

하지만 콜백 함수가 호출된 ②의 시점에서 `this`는 전역 객체 `window`를 가리킨다. 따라서 `person.foo`의 콜백 함수 내부에서 `this.name`은 `window.name`과 같다.

`person.foo`의 콜백 함수는 외부 함수 `person.foo`를 돕는 헬퍼 함수(보조 함수) 역할을 하기 때문에 외부 함수 `person.foo` 내부의 this와 콜백 함수 내부의 this가 상이하면 문맥상 문제가 된다.

이러한 경우 `bind`메서드를 사용해 이를 해결한다.


```js
const person = {
  name: 'Lee',
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    setTimeout(callback.bind(this), 100);
  }
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Lee.
});
```

혹은, 콜백 함수를 화살표 함수로 선언해 해결이 가능하다.
참고로 화살표 함수는 자체의 `this`, `arguments`, `super`, `new.target` 바인딩을 갖지 않기 때문에 스코프 체인을 따라 상위 스코프의 `this`를 그대로 참조 한다. 이를 `lexical this`라고 한다.

아래와 같은 예제는 화살표 함수를 인수로 전달하기 때문에 `person.foo`메서드가 호출되면, `foo`함수 객체가 생성되고, 그에 따라 매개변수(`callback`)도 평가되고 화살표 함수 객체가 생성된다.(이때 렉시컬 스코프가 결정된다.)

따라서 화살표 함수(`callback`)의 상위 스코프는 `person.foo`함수가 되어, 화살표 함수의 `this`는 `person.foo`함수에 바인딩된 `this`가 되고, 이`this`는 `person`을 가리키게 된다.

```js
const person = {
  name: 'Lee',
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    setTimeout(callback, 100);
  }
};

person.foo(() => {
  console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Lee.
});
```

<br>
<br>
<br>


## 📝 정리
지금까지 살펴본 함수 호출 방식에 따라 this 바인딩이 동적으로 결정되는 것에 대해 정리해보자.

|함수 호출 방식 |	this 바인딩 |
| -| -|
|일반 함수 호출|	전역 객체
|메서드 호출	|메서드를 호출한 객체
|생성자 함수 호출	|생성자 함수가 (미래에) 생성할 인스턴스
|Function.prototype.apply/call/bind 메서드에 의한 간접 호출|	Function.prototype.apply/call/bind 메서드에 첫 번째 인수로 전달한 객체
