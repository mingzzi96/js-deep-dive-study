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

## 🎀 요소 노드 취득

### 📌 id를 이용한 요소 노드 취득

`Document.prototype.getElementById` 메서드는 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환한다.

id 값이 중복되더라도 첫 번째 id값만 반환한다.

요소가 존재하지 않는 경우 null을 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="banana">Apple</li>
      <li id="banana">Banana</li>
      <li id="banana">Orange</li>
    </ul>
    <script>
      // getElementById 메서드는 언제나 단 하나의 요소 노드를 반환한다.
      // 첫 번째 li 요소가 파싱되어 생성된 요소 노드가 반환된다.
      const $elem = document.getElementById('banana');

      // 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
      $elem.style.color = 'red';
    </script>
  </body>
</html>
```

### 📌 태그 이름을 이용한 요소 노드 취득

`Document.prototype/Element.prototype.getElementsByTagName` 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      // 태그 이름이 li인 요소 노드를 모두 탐색하여 반환한다.
      // 탐색된 요소 노드들은 HTMLCollection 객체에 담겨 반환된다.
      // HTMLCollection 객체는 유사 배열 객체이면서 이터러블이다.
      const $elems = document.getElementsByTagName('li');

      // 취득한 모든 요소 노드의 style.color 프로퍼티 값을 변경한다.
      // HTMLCollection 객체를 배열로 변환하여 순회하며 color 프로퍼티 값을 변경한다.
      [...$elems].forEach(elem => { elem.style.color = 'red'; });
    </script>
  </body>
</html>
```

### 📌 class를 이용한 요소 노드 취득

`Document.prototype/Element.prototype.getElementsByClassName` 메서드는 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li class="fruit apple">Apple</li>
      <li class="fruit banana">Banana</li>
      <li class="fruit orange">Orange</li>
    </ul>
    <script>
      // class 값이 'fruit'인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환한다.
      const $elems = document.getElementsByClassName('fruit');

      // 취득한 모든 요소의 CSS color 프로퍼티 값을 변경한다.
      [...$elems].forEach(elem => { elem.style.color = 'red'; });

      // class 값이 'fruit apple'인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환한다.
      const $apples = document.getElementsByClassName('fruit apple');

      // 취득한 모든 요소 노드의 style.color 프로퍼티 값을 변경한다.
      [...$apples].forEach(elem => { elem.style.color = 'blue'; });
    </script>
  </body>
</html>
```

### 📌 querySelector / querySelectorAll

`Document.prototype/Element.prototype.querySelector` 메서드는 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <script>
      // class 어트리뷰트 값이 'banana'인 첫 번째 요소 노드를 탐색하여 반환한다.
      const $elem = document.querySelector('.banana');

      // 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
      $elem.style.color = 'red';
    </script>
  </body>
</html>
```

`Document.prototype/Element.prototype.querySelectorAll` 메서드는 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드를 탐색하여 반환한다. 

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <script>
      // ul 요소의 자식 요소인 li 요소를 모두 탐색하여 반환한다.
      const $elems = document.querySelectorAll('ul > li');
      // 취득한 요소 노드들은 NodeList 객체에 담겨 반환된다.
      console.log($elems); // NodeList(3) [li.apple, li.banana, li.orange]

      // 취득한 모든 요소 노드의 style.color 프로퍼티 값을 변경한다.
      // NodeList는 forEach 메서드를 제공한다.
      $elems.forEach(elem => { elem.style.color = 'red'; });
    </script>
  </body>
</html>
```

### 📌 특정 요소 노드를 취득할 수 있는지 확인

`Element.prototype.matches` 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 true,false 반환한다.

이벤트 위임을 사용할 때 유용하다.

```js
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="pizza">pizza</li>
      <li>noddle</li>
      <li>rice</li>
    </ul>
    <script>
      const $elems=document.querySelector('ul > li');
      console.log($elems.matches('#pizza')); // true
      console.log($elems.matches('li')); // true
      console.log($elems.matches('.pizza')); // false
    </script>
  </body>
</html>
```

### 📌 HTMLCollection과 NodeList

#### HTMLCollection
HTMLCollection은 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 객체이다.

NodeList는 경우에 따라 live 객체로 동작허고, 대부분은 non-live다.

```html
<!DOCTYPE html>
<head>
  <style>
    .red{
      color : red;
    }
    .blue{
      color : blue;
    }
  </style>
</head>
<html>
  <body>
    <ul>
      <li class="red">pizza</li>
      <li class="red">noddle</li>
      <li class="red">rice</li>
    </ul>
    <script>
      const $elems=document.getElementsByClassName('red');
      for(let i = 0;i<$elems.length;i++){
        $elems[i].className='blue';
      }
    </script>
  </body>
</html>
```

$elems에는 class명이 red인 li요소 3개가 담겨있는 HTMLCollection 객체가 할당되었다.
for문을 통해 각각 li요소들의 class명을 blue로 바꿔줄 때 발생하는 일을 알아보자.

1. 첫번째 li요소의 class명을 blue로 바꾼다. 그 이후 더이상 클래스명이 red가 아니기 때문에 해당 요소는 `$elems`에서 실시간으로 제거된다.
2. `$elems`에는 li 두번째와, 세번째 요소만 있으니 i=1이 되면 세번째 요소가 제거된다. 그 후 해당 li요소 역시 `$elems`에서 제거된다.
3. `i=2`인데 `$elems.length`는 1이므로 for문이 종료된다.

이런 일을 방지하려면 `HTMLCollection` 객체를 배열로 변환하여 사용하는게 좋다

#### NodeList

querySelectorAll 메서드는 NodeList 객체를 반환한다.

NodeList 객체는 실시간으로 노드 객체의 상태 변경을 반영하지 않는 non-live 객체이다.

하지만 childNodes 프로퍼티가 반환하는 NodeList 객체는 live 객체이다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>pizza</li>
      <li>noddle</li>
    </ul>
    <script>
      const $fruits= document.getElementById('fruits');
      const {childNodes} = $fruits;
      console.log(childNodes);
      for(let i = 0;i<childNodes.length;i++){
        $fruits.removeChild(childNodes[i]);
      }
      console.log(childNodes);
    </script>
  </body>
</html>
```

$fruits는 live 객체이므로 자식노드를 제거할 때마다 새롭게 갱신되어 모든 자식노드가 삭제되지 않는다

결국 배열로 변환해서 사용하는 것이 좋다.

## 🎀 DOM 조작

### 📌 innerHTML
```html
<!DOCTYPE html>
<html>
  <body>
    <div id="box">
    </div>
    <script>
      const box = document.getElementById('box');
      box.innerHTML = '<ul> <li>사과</li><li>바나나</li></ul>';
    </script>
  </body>
</html>
```

- 모든 노드의 자식을 제거하고 새롭게 할당하므로 비효율적
- 삽입될 위치를 정할 수도 없다
- 크로스 사이트 스크립팅 공격에 취약

### 📌 insertAdjacentHTML 메서드
```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="1">1번</li>
      <li id="3">3번</li>
    </ul>
    <script>
      document.getElementById('1').insertAdjacentHTML("afterend",'<li id="2">2번</li>');
    </script>
  </body>
</html>
```
- 기존 요소를 제거하지 않고 위치를 지정해 새로운 요소를 삽입
- position은 총 4 가지 : 'beforebegin', 'afterbegin', 'beforeend', 'afterend'
- innerHTML 프로퍼티보다 효율적이고 빠르다
- 크로스 사이트 스크립팅 공격에 취약

### 📌 노드 생성과 추가

- `요소 노드 생성` : document.prototype.createElement(tagName)
- `텍스트 노드 생성` : document.prototype.createTextNode(text)
- `마지막 자식 노드로 추가` : Node.prototype.appendChild(childNode)

## 🎀 어트리뷰트

### 📌 HTML 어트리뷰트 조작

attributes 프로퍼티를 통하지 않고 요소 노드에서 메서드로 바로 HTML 어트리뷰트 값 접근 가능하다.

- Element.prototype.getAttribute(attributeName)
- Element.prototype.setAttribute(attributeName,attributeValue)
- Element.prototype.hasAttribute(attributeName)
- Element.prototype.removeAttribute(attributeName)

### 📌 HTML 어트리뷰트 vs DOM 프로퍼티

- `HTML 어트리뷰트` : HTML 요소의 초기 상태를 지정하고 이는 변하지 않는다. 초기 상태 값을 취득하거나 변경 하려면 getAttribute / setAttribute 메서드를 사용
- `DOM 프로퍼티` : 요소 노드의 최신 상태를 관리한다. 단 사용자 입력에 의한 상태 변화와 관계있는 DOM 프로퍼티만 최신 상태 값을 관리( ex- input)

### 📌 data 어트리뷰트와 dataset 프로퍼티

- data 어티리뷰트와 dataset 프로퍼티를 사용해 자바스크립트 간에 데이터를 교환 가능하다.
- data 어트리뷰트는 `data- 접두사 + 임의의 이름`을 붙여 사용한다.
- data 어트리뷰트 값은 HTMLElement.dataset 프로퍼티로 취득
- 존재 하지 않는 이름을 키로 dataset 프로퍼티에 할당하면 HTML 요소에 data 어트리뷰트가 추가


## 🎀 스타일

### 📌 인라인 스타일 조작

```html
<!DOCTYPE html>
<html>
<body>
  <div style="color: red">Hello World</div>
  <script>
    const $div = document.querySelector('div');
 
    <-- 인라인 스타일 취득 -->
    console.log($div.style); // CSSStyleDeclaration { 0: "color", ... }
 
    <-- 인라인 스타일 변경 -->
    $div.style.color = 'blue';
 
    <-- 인라인 스타일 추가 -->
    $div.style.width = '100px';
    $div.style.height = '100px';
    $div.style.backgroundColor = 'yellow';
  </script>
</body>
</html>
```

# 📚 면접 예상 질문

## 🎀 HTMLCollection vs NodeList

HTMLCollection은 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 객체이다.

여기서 '살아있다'라는 의미는 객체가 스스로 실시간 노드 객체의 상태 변경을 반영함을 의미한다.

querySelectorAll 등의 메서드가 반환하는 NodeList 객체는 노드 객체의 상태 변화를 반영하지 않는 non-live DOM 컬렉션 객체입니다. NodeList는 앞의 HTMLCollection과 다르게 노드가 변경되도 그 상태를 반영하지 않습니다.

하지만 childNodes는 live라서 실시간 반영된다.

live 객체는 반복문을 순회하면서 노드가 변경되는 경우, 개발자의 의도와는 다른 결과가 발생할 수 있으므로 배열로 바꾸어 사용하는 것이 바람직합니다.(Array.from과 스프레드 연산자)

<img width="968" alt="image" src="https://github.com/mingzzi96/js-deep-dive-study/assets/134386378/d5d28553-0453-444c-bfa9-41aaebe92fe6">


## 🎀 innerHTML vs insertAdjacentHTML

### innerHTML

- 모든 노드의 자식을 제거하고 새롭게 할당하므로 비효율적
- 삽입될 위치를 정할 수도 없다
- 크로스 사이트 스크립팅 공격에 취약

### insertAdjacentHTML

- 기존 요소를 제거하지 않고 위치를 지정해 새로운 요소를 삽입
- position은 총 4 가지 : 'beforebegin', 'afterbegin', 'beforeend', 'afterend'
- innerHTML 프로퍼티보다 효율적이고 빠르다
- 크로스 사이트 스크립팅 공격에 취약

## 🎀 HTML 어트리뷰트 vs. DOM 프로퍼티

```html
<div id="content" name="content-name" custom="custom"/><div> 
```

### HTML 어트리뷰트

- HTML에 의해 정의된다.
    
    요소는 값이 "content"인 id 애트리뷰트, 값이 "content-name"인 name 애트리뷰트, 값이 "custom"인 custom 애트리뷰트를 가집니다.
    
- **애트리뷰트의 타입은 문자열(string)입니다.**

### DOM 프로퍼티

- id, name, class 등과 같은 프로퍼티는 점 표기법 및 대괄호 표기법으로 가져올 수 있지만, custom과 같은 사용자 정의 프로퍼티는 가져올 수 없습니다.
- 기본 값이 존재하는 애트리뷰트는 값을 변경할 수 없습니다.
- DOM 프로퍼티는 모든 데이터 타입을 가질 수 있습니다.


