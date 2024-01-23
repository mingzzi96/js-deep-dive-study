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


