# 📚 비동기 프로그래밍과 Ajax

## 🎀 주요 키워드
- `실행 컨텍스트`
- `싱글 스레드`
- `이벤트 루프`와 `태스크 큐`

## 🎀 싱글 스레드이면서 어떻게 동시 처리가 가능해?

싱글 스레드 방식은 한 번에 하나의 태스크만 처리할 수 있다는 것을 의미한다.

하지만 브라우저가 동작하는 것을 살펴보면 많은 태스크가 동시에 처리되는 것 처럼 느껴진다.

이렇게 자바스크립트의 동시성을 가능하게 해주는 것은 이벤트 루프가 존재하기 때문이다.

### 📌 아래 코드 실행의 결과는?

<img width="284" alt="image" src="https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/d1ce440a-b7b1-4d26-816e-e92c7c00abf3">


### 📌 JS 비동기 처리는 이렇게 일어난다.

![image](https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/ce5cd44d-1683-4925-80e0-3765da9f62ec)

- console.log start 가 call stack에 쌓인다.


![image](https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/3f5f6675-3a80-4e5c-949f-042ebe0aba00)

- console.log start 가 실행이 된다.


![image](https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/822ae534-536b-42c6-9b0e-817cc157a807)

- 실행된 함수는 call stack에서 제거된다.


![image](https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/bd6c0a92-656c-4a1e-a17c-ff89b9d982a8)

- setTimeout call stack에 쌓인다.


![image](https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/3186aa3d-d0f7-482b-b2be-f2533855b0d4)

- call stack에 담긴 setTimeout 함수가 실행되면 타이머가 web API에 등록되고,  call stack에서는 사라진다.


![image](https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/dbf875c2-e291-4f60-bab7-37464924e579)

- 비어진 call stack에 console log end 들어옴


![image](https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/94aec32d-af0d-4906-9602-201ac833c91a)

- console.log가 실행되고, call stack에서 사라진다.


![image](https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/7f979907-af17-4a7d-9c36-137a08793168)

- timer가 끝나면 callback queue 안에 setTimeout 콜백함수 이동


![image](https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/256f5dbb-7b48-45d3-9559-981392d3ddf7)

- call stack이 비워졌으니 setTimeout 함수를 올려 보낸다.


![image](https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/5ff85b9f-58f5-474b-a9db-fd841b2a25a1)

- setTimeout안에 있는 console.log를 실행한다.


![image](https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/f35ad208-087e-41dc-87f4-a6bd3c3cb96c)

- console.log timeout 실행되고 call stack에서 제거


![image](https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/3afaaf64-a831-45c8-9b9a-eaa9504cc2a7)

- setTimeout 종료되었으니 사라짐

## 🎀 micro / macro task ([참고 링크](https://study-ihl.tistory.com/185))

콜백 함수 대기하는 공간인 callback queue는 task queue라고도 부른다.

그리고 이 task queue는 또 다시 micro / macro task 이렇게 두가지로 나뉘어져 있고 우선순위도 존재한다.

```js
setTimeout(() => {
    console.log('A');
}, 0);

Promise.resolve()
.then(() => {
    console.log('B');
})
.then(function() {
    console.log('C');
});

console.log('D');
```

위 코드의 예상 결과는 D > A > B > C 이다.

하지만 태스크 큐 안에서도 우선순위가 존재하기 때문에 D > B > C > A 순으로 결과가 출력된다.


- `Macro Task Queue`: setTimeout, setInterval, setImmediate, requestAnimationFrame, I/O, UI Rendering
- `Micro Task Queue`: process.nextTick, Promise, Object.observe, MutationObserver

1. Macro Task Queue에서 가장 오래된 작업 하나를 실행한다.
2. Micro Task Queue에 있는 모든 작업을 실행한다.
3. 1단계로 이동한다.
