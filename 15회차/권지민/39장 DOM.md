# 📚 DOM

DOM은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조다.

## 🎀 노드

### 📌 HTML 요소와 노드 객체

<img width="1273" alt="image" src="https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/b2c9164f-1573-4c28-be24-1fba3ba8e077">

HTML 요소는 HTML 문서를 구성하는 개별적인 요소를 의미한다.

HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다.

<img width="1239" alt="image" src="https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/b8180812-3d58-4c98-89a4-17e4b5c28b8f">

각각의 요소들은 중첩관계를 갖을 수 있어서, 요소 간의 부모-자식 관계를 반영하여 트리 자료 구조를 구성한다.

노드 객체들로 구성된 트리 자료구조를 DOM이라 한다.

### 📌 노드 객체의 타입

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script src="app.js"></script>
  </body>
</html>
```

렌더링 엔진은 위 코드를 파싱하여 아래와 같은 DOM을 생성한다.

<img width="1450" alt="image" src="https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/da49354b-81ff-40df-a7e0-357abf5a2736">

- `문서 노드 (document node)` : 
DOM 트리의 최상단에 존재하는 루트노드, document 객체를 가르킨다
document 객체는 브라우저가 렌더링한 HTML 문서 전체를 가르킴, 전역 객체의 document프로퍼티에 바인딩되어 있다-> document로 참조 가능
최상단에 있기 때문에 DOM 트리의 노드들에 접근하기 위한 진입점 역할을 함

- `요소 노드 (element node)` : 
HTML 요소를 가르키는 객체
HTML 요소들의 중첩관계를 통해 문서의 구조를 표현한다

- `어트리뷰트 노드 (attribute node)` : 
HTML 요소의 어트리뷰트를 가르키는 객체
어트리뷰트 노드는 해당 요소 노드에만 연결되어 있다

- `텍스트 노드 (text node)` : 
HTML 요소의 텍스트를 가르키는 객체
해당 요소 노드의 자식 노드이며 말단 노드임

### 📌 노드 객체의 상속 구조

노드 객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 갖는다. 노드 객체의 상속 구조는 다음과 같다.

<img width="1445" alt="image" src="https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/e1c5269a-764a-4429-a4ad-d5765c390b44">

노드 객체는 `Object`, `EventTarget`, `Node` 인터페이스를 상속받는다.
