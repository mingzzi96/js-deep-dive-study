## 22.1 this 키워드

자바스크립트에서 `this` 는  어떤 객체를 가르키는 키워드
그런데 그게 무슨 객체일까...?

이런 `this`가 가리키는 객체 함수 호출 방식에 의해 자바스크립트 엔진이 동적으로 결정된다.

미리 이야기 하자면 `this` 는 **함수를 호출한 객체**이다.

```js
console.log(this);
```
를 호추한 경우 브라우저에 대한 정보를 가지고 있는 **전역 객체(window)** 가 출력




#### 객체 리터럴
```js
//객체 리터럴
const circle = {
	radius: 5,
	getDiameter() {
	return 2 * this. radius;
	}
};

console.log(circle.getDiameter()); //10
```

객체 리터럴의 메서드 내부에서 `this`는 메서드를 호출한 객체, 즉 circle을 가르킨다.

#### 생성자 함수
```js
//생성자 함수
const Circle(radius) {
	this.radius = radius;
}

Circle.prototype.getDiameter = function () {
	return 2 * this.radius;
	
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

생성자 함수 내부의 `this`는 생성자 함수가 생성할 인스턴스를 가르킨다.

이렇게 자바스크립트의 `this`는 함수가 호출되는 방식에 따라 this 바인딩이 동적으로 결정된다.

### 22.2 함수 호출 방식과 this 바인딩


### 22.2.1 일반 함수 호출
기본적으로 this에는 전역 객체(global object)가 바인딩 된다.

```js
function foo() {
	console.log("foo's this: "  this);
	function bar() {
		console.log("bar's this: " , this);
	}
	bar();
}
foo();
```


### 22.2.2 메서드 호출
메서드 내부의 `this`에는 메서드를 호출한 객체 즉 메서드를 호출할때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술된 객체가 바인딩 된다.

```js
const person = {
	name: 'Lee',
	getName() {
	return this.name;
	}
}

console.log(person.getName()); //Lee
```




![](https://i.imgur.com/XIMiUfx.png)
![](https://i.imgur.com/lzE6Kwj.png)


### 22.2.3 생성자 함수 호출
생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩 된다.

```js
// 새성자 함수

function Circle(radius) {
	this.radius = radius;
	this.getDiameter = function() {
	return 2 * this radius;
	}
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

생성자 함수는 객체(인스턴스)를 생성하는 함수 이다.


### 22.2.4 Function.prototype.apply / call / bind 메서드에 의한 간접호출


#### apply와 call 메서드

기본적으로 함수를 호출한다.
```js
const person = {
  fullName: function() {
    return this.firstName + " " + this.lastName;
  }
}

const person1 = {
  firstName: "John",
  lastName: "Doe"
}

const person2 = {
  firstName: "Mary",
  lastName: "Smith"
}

// call 메서드 예제
console.log(person.fullName.call(person1)); // 출력: John Doe
console.log(person.fullName.call(person2)); // 출력: Mary Smith

// apply 메서드 예제
console.log(person.fullName.apply(person1)); // 출력: John Doe
console.log(person.fullName.apply(person2)); // 출력: Mary Smith

```

`call` 및 `apply` 메서드를 사용하여 함수를 호출할 때, 첫 번째 매개변수로 전달한 객체가 해당 함수 내에서 `this`로 지정

`person.fullName.call(person1)`을 호출할 때, `person1` 객체의 `firstName`과 `lastName` 속성을 이용하여 `fullName` 메서드를 실행하고자 합니다. 이때 `this`는 `person1` 객체를 가리킵니다.

#### bind

함수에 우리가 원하는 객체를 묶어주는 기능이다.

```js
function main() {
	console.log(this);
}

main(); // window가 출력
```

우리는 this값이 window가 아니라 우리가 원하는 객체로 설정할 수 있다.
다른 객체로 바인딩 할 수 있다.

```js
const mainBind = main.bind({name: 'hi'})
mainBind(); // {name: 'hi'}

const object = {
	mainBind,
};

object.mainBind(); // {name: 'hi'} 
```

심지어 `mainBind`는 object의 객체임에도 불구하고 `this`는 `{name: 'hi'}` 가 출력된다.

`bind()`는 새로운 함수를 반환해 주는 함수이다. 
우리가 넣어준 객체가 `this` 값으로 설정된 새로운 함수를 반환해 준다.



```js
const person = {
  fullName: function() {
    return this.firstName + " " + this.lastName;
  }
}

const person1 = {
  firstName: "John",
  lastName: "Doe"
}

const person2 = {
  firstName: "Mary",
  lastName: "Smith"
}

const fullNamePerson1 = person.fullName.bind(person1);
const fullNamePerson2 = person.fullName.bind(person2);

console.log(fullNamePerson1()); // 출력: John Doe
console.log(fullNamePerson2()); // 출력: Mary Smith

```

`bind` 메서드는 함수의 `this` 값을 특정 객체로 설정하여 새로운 함수를 반환하는 JavaScript의 내장 메서드

`bind` 메서드를 사용하여 `person.fullName`을 `person1`과 `person2` 객체에 각각 바인딩한 새로운 함수를 생성해 준다.

#### 결론!

| 함수 호출 방식 | this 바인딩 |
| -------------- | ------------ |
| 일반 함수 호출 | 전역 객체 |
| 메서드 호출 | 메서드를 호출한 객체 |
| 생성자 함수 호출 | 생성자 함수(인스턴스)가 생성한 인스턴스 |
| Function.prototype.apply/call/bind 메서드에 의한 | Function.prototype.apply/call/bind 메서드에 전달된 인수로 전달한 객체 |
