# 📚 함수

## 🎀 재귀 함수

함수가 자기 자신을 호출하는 것을 재귀 호출(recursive call)이라 한다.

재귀 함수(recursive function)는 자기 자신을 호출하는 행위, 즉 재귀 호출을 수행하는 함수를 말한다.

```js
var factorial = function foo(n) {
  if (n <= 1) return 1;

  return n * factorial(n - 1);
}

console.log(factorial(5)) // 5! = 5 * 4 * 3 * 2 * 1 = 120
```

1. 자기 자신을 호출하는 재귀 함수를 사용하면 반복되는 처리를 반복문 없이 구현할 수 있다.
2. 무한에 빠질 수 있으니 재귀 호출을 멈출 수 있는 탈출 조건을 반드시 만들어야 한다.
3. 재귀 함수는 반복문으로 대체될 수 있으니 한정적으로 사용하자.

## 🎀 중첩 함수

함수 내부에 정의된 함수를 중첩 함수 또는 내부 함수라 한다.

```js
function outer(){
  var x = 1;

  // 중첩 함수
  function inner(){
    var y = 2;
    // 외부 함수의 변수를 참조할 수 있다.
    console.log(x + y); // 3
  }

  inner();
}

outer();
```

1. 중첩 함수를 포함하는 함수는 외부 함수라 부른다.
2. 중첩 함수는 외부 함수 내부에서만 호출 가능하다.

## 🎀 콜백 함수

```js
function repeat(n, f) {
  for(var i = 0; i < n; i++){
    f(i); // i를 전달하면서 f를 호출
  }
}

var logAll = function(i){
  console.log(i);
}

repeat(5, logAll); // 0 1 2 3 4

var logOdds = function (i) {
  if (i % 2 ) console.log(i);
}

repaet(5, logOdds); // 1 3
```

함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수를 콜백 함수라고 하며,

매개 변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수를 고차 함수라고 한다.

콜백 함수는 고차 함수에 의해 호출되며 이때 고차 함수는 필요에 따라 콜백 함수에 인수를 전달 할 수 있다.

### 📌 비동기 처리

```js
document.getElementById('myButton').addEventListener('click', function () {
  console.log('button clicked!');
});

// 콜백 함수를 사용한 비동기 처리
setTimeout(function () {
  console.log('1초 경과');
}, 1000);
```
### 📌 배열 고차 함수

```js
var res = [1, 2, 3].map(function (item) {
  return item * 2;
});

console.log(res); // [2, 4, 6]
```

`map` 이외에 `filter`, `reduce` 고차 함수가 존재한다.


## 🎀 순수 함수와 비순수 함수

### 📌 순수 함수

```js
var count = 0; // 현재 카운트

function increase(n) {
  return ++n;
}
// 순수 함수가 반환한 결과값을 변수에 재할당해서 상태를 변경
count = increase(count);
console.log(count); // 1
```

1. 순수 함수는 동일한 인수가 전달되면 언제나 동일한 값을 반환하는 함수다. (결국 상수와 마찬가지다.)
2. 일반적으로 최소 하나 이상의 인수를 전달 받는다.
3. 함수의 외부 상태를 변경하지 않는다.

### 📌 비순수 함수

```js
var count = 0; // 현재 카운트 : increase 함수에 의해 변한다.

function increase() {
  return ++count; // 외부에 의존하며 외부 상태를 변경한다.
}

increase();
console.log(count); // 1
```

1. 외부 상태에 의존하거나 외부 상태를 변경하는 함수다.
2. 외부 상태를 변경하면 상태 변화를 추적하기 어려워진다.
