# DOM
- HTML 문서의 계층 구조와 정보를 표현
- 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API를 제공하는 트리 자료조 이다.


## 39.1 노드
### 39.1.1 HTML 요소와 노드 객체

![](https://i.imgur.com/MfGiy8m.png)
- HTML 요소는 HTML 문서를 구성하는 개별적인 요소
- html 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변
- html 요소의 어트리뷰트는 어트리뷰트 노드
- html 요소의 텍스트 콘텐츠는 텍스트 노드



### 39.1.2 노드 객체의 타입
#### 39.1.2.1 Document node
- DOM 트리의 최상위에 존재하는 루트 노드 이다.
- `document` 객체는 브라우저가 렌더링한 HTML 문서 전체를 가르키는 객체
- `window.document`를 바인딩 하고 있고 `<script>` 태그에 의해 js 파일이 분리되어있어도 단 하나의 `window`를 가르키고 잇다.
#### 39.1.2.2 요소 노드
- HTML 요소를 가리키는 객체 
- 문서의 구조를 표현한다.
![](https://i.imgur.com/gHIoML6.png)

#### 39.1.2.3 어트리뷰트 노드
- HTML 요소의 attribute를 가르키는 객체
- 요소의 부모요소는 아니기 때문에 형제 노드는 아니다.
![](https://i.imgur.com/LEZ0F44.png)
#### 39.1.2.4 텍스트 노드
![](https://i.imgur.com/3TdBn3a.png)
- html 요소의 텍스트를 가르키는 객체
- 문서의 정보를 표현한다.
- 요소노드의 자식 노드이며 리프 노드 이다.(즉 끝)

### 39.1.3 노드 객체의 상속 구조
- DOM을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 DOM API를 사
- 부모, 자식, 형제, 자식을 탐색이 가능하고
- 자신의 어트리뷰트 노드와 텍스트 노드를 조작할 수도 있다.

이러한 노드 객체는 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 갖는다.

![](https://i.imgur.com/yDmEQvZ.png)

상속구조를 프로토체인 관점에서 본다면 `input` 요소의 노드는
`Object`, `EventTarget`, `Node` , `Element`, `HTMLElement`, `HTMLInputElement`의 프로토타입에 바인딩되어있는 프로토타입 객체를 상속 받는다.

![](https://i.imgur.com/caOUH9A.png)

즉 `input` 요소 노드 객체는 자신의 프로토타입 체인에 있는 모든 프로토타입의 프로퍼티나 메서드를 상속받아 사용할 수 있다.

- `HTMLInputElement` – 입력 관련 프로퍼티를 제공하는 클래스
- `HTMLElement` – HTML 요소 메서드와 getter, setter를 제공하는 클래스
- `Element` – 요소 노드 메서드를 제공하는 클래스
- `Node` – 공통 DOM 노드 프로퍼티를 제공하는 클래스
- `EventTarget` – 이벤트 관련 기능을 제공하는 클래스
- `Object` – `hasOwnProperty`같이 ‘일반 객체’ 메서드를 제공하는 클래스

이렇게 DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.


## 39.2 요소 노드 찾기

### 39.2.1 ID를 이용한 요소 노도 찾기

```js
const $elem = document.getElementById('banana');
```
- `id`는 유일해야한다.
- 다만 에러를 발생시키지는 않기때문에 여러개의 id가 있을 경우 첫번째 요소 노드만 반환한다.
- 만약 없을 경우 `null`을 반환한다.

### 39.2.2 태그 이름을 이용한 요소 노드 찾기
```js
const $elems = document.getElementsByTagName('li');
```
- `getElementsByTagName` 메서드가 반환하는 HTMLCollection 객체는 유사 배열 객체이며 이러블이다.
- 만약 찾는 요소가 없을 경우 빈 `HTMLCollection` 객체를 반환한다.
- `getElementsByTagName` 메서드는 2개의 종류가 있다.
	- `document.getElementsByTagName('li')` : 전체 문서에서 `li`를 찾아서 반환한다.
	- `Element.getElementsByTagName('li')` : 해당 Element의 하위에서 찾아서 반환한다.

### 39.2.3 class를 이용한 요소 노드 찾기

```js
const $elems = document.getElementsByClassName('fruit');
```
- `class` 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색으로 반환한다.
- 여러개의 요소 노드 객체를 갖는 `HTMLCollection` 객체를 반환
- 만약 찾는 값이 없을 경우 빈 객체를 반환한다.
- 공백으로 구분하여 여러개 `class`를 지정할 수 있다.
```js
const $apples = document.getElementsByClassName('fruit apple');
```
- `Document.prototype` 에 정의된 메서드와 `Element.prototype`에 정의된 메소드 두 종류가 있다.

### 39.2.4 CSS 선택자를 이용한 요소 노드 찾기
```JS
document.querySelector('.banana');
```
- 인수로 전달한 CSS 선택자를 만족시키는 요소노드가 여러개인 경우 첫번째 요소 노드만 반환
- 만약 없을 경우 `null` 반환
- css 선택자가 문법에 맞지 않는 경우 `DOMException` 에러가 발생한다.

만약 여러개가 필요한 경우
```js
const $elems = document.querySelectorAll('ul > li');
```
- 인수로 전달된 CSS 선택자를 만족시키는 요소가 없을 경우 빈 `NodeList` 객체를 반환
- 만약 css 선택자가 문법에 맞지 않는 경우 `DOMException` 에러 발생
### 39.2.5 특정 요소 노드를 취득할수 있는지 확인
```js
const $apple = document.querySelector('.apple');

console.log($apple.matches('#fruits > li.apple')); // true
```
 $apple 노드는 '#fruits > li.apple'로 취득할수 있는지 확인

```html
<ul id="fruits">
	<li class="apple">Apple</li>
	<li class="banana">Banana</li>
	<li class="orange">Orange</li>
</ul>
```

### 39.2.6 HTMLCollection과 NodeList
- 2개 모두 다 유사 배열 객체이며 이터러블이다.
- `for... of` 로 순회하거나 스프레드 문법을 사용하여 간단히 배열로 변환할 수 있다.
- 가장 큰 특징은 객체이다.

#### HTMLCollection
- `getElementsByTagName`, `getElementsByClassName` 메서드가 반환한다.
- 노드 객체의 상태 변화를 실시간으로 반영한다.

```html
<ul id="fruits">
	<li class="apple">Apple</li>
	<li class="banana">Banana</li>
	<li class="orange">Orange</li>
</ul>
```
```js
const $elems = document.getElementsByClassName('red')

for(let i = 0; i < $elem.length; i++) {
	$elem[i].className = 'blue';
}
```

이렇게 하면 `li` 모두가 `blue`로 변환될꺼 같지만 예상대로 동작하지 않는다.
왜냐하면 `HTMLCollection`은 실시간으로 노드 객체의 상태를 반영한다.

한번 반복할때마다 증가하는 `i`값과 줄어드는 `length`로 인한 예상치 못한 결과다.

```js
[...$elems].forEach(elem => elem.className = 'blue');
```
이렇게 스프레드 연산자로 객체로 변환하여 사용하면 해결된다.

#### NodeList
`querySelectAll`메서드는 `NodeList`객체를 반환한다.
`NodeList`는 실시간으로 노드 객체 상태 변경을 반영하지 않기 때문에 위와 같은 문제를 해결할 수 있다.
```js
const $elems = document.querySelectorAll('.red');

$elems.forEach(elem => elem.className = 'blue')'
```


`HtmlCollection`과 `NodeList`는 유사 배열 객체 이면서 이터러블이다. 따라서 스프레드 문법이나 `Array.from` 메서드로 간단히 배열로 변경할 수 있다.

## 39.3 노드 탐색
노트 탐색은 취득한 요소 노드 기점으로 DOM 트리의 노드를 옮겨 다닌다.

### 39.3.2 자식 노드 탐색

```js
const $fruits = document.getElementById('fruits');

//자식 노드를 모두 탐색하여 NodeList에 담아 반환한다.
//텍스트 노드도 포함될 수 있다.
console.log($fruits.childNodes);

//모든 자식 노드를 탐색한다
//HTMLCollection에 자식 객체를 담는다. (텍스트노드는 포함하지 않는다.)
console.log($fruits.children);

//첫번째 자식 노드를 반환 (텍스트가 들어갈 수도 있다.)
console.log($fruits.firstChild);//text

//마지막 자식 노드를 반환(텍스트가 들어갈 수도 있다.)
console.log($fruits.lastChild);//text

//첫번째 자식 요소 노드를 반환한다.
console.log($fruits.firstElementChild); //li.apple

//마지막 자식 요소 노드를 반환한다.
console.log($fruits.lastElementChild);//li.orange
```

### 39.3.3 자식 노드 존재 확인

- `hasChildNodes`로 자식 노드가 존재 하면 `true` 없으면 `false`이다.
- 텍스트 노드도 포함하여 자식 노드도 확인한다.
```js
const $fruits = document.getElementById('fruits');
console.log($fruits.hasChildNodes());
```

만약 요소 노드가 있는지 확인하려면
`children.length`, `.childElementCount`를 사용한다.
```js
console.log($fruits.children.length);
console.log($fruits.childElementCount);
```

### 39.3.4 요소 노드의 텍스트 노드의 탐색
정확히 텍스트 노드만을 탐색하는 건 없다.

### 39.3.5 부모 노드 탐색
`parentNode` 프로퍼티를 사용한다.

```js
const $banana = document.querySelector('banana');
console.log($banana.parentNode); //ul#fruits
```

### 39.3.6 형제 노드 탐색

```js
const $banana = document.querySelector('.banana');


// 부모노드가 같은 형제 요소 노드 중에서 자신의 이전 형제 요소를 탐색하여 반환
// 요소 노드만 반환한다.
const previousSibling = $banana.previousElementSibling;
// 부모노드가 같은 형제 요소 노드 중에서 자신의 다음 형제 요소를 탐색하여 반환
// 요소 노드만 반환한다.
const nextSibling = $banana.nextElementSibling;

```


## 39.4 노드 정보 취득

```js
console.log(document.nodeType); //9
console.log(document.nodeName); // #document

const $foo = document.getElementById('foo');
console.log($foo.nodeType); // 1
console.log($foo.nodeName); // DIV
```

[`Node.ELEMENT_NODE`](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType#node.element_node) (`1`)
[`Node.ATTRIBUTE_NODE`](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType#node.attribute_node) (`2`)
[`Node.TEXT_NODE`](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType#node.text_node) (`3`)
[`Node.CDATA_SECTION_NODE`](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType#node.cdata_section_node)(`4`)
[`Node.PROCESSING_INSTRUCTION_NODE`](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType#node.processing_instruction_node) (`7`)
[`Node.COMMENT_NODE`](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType#node.comment_node) (`8`)
[`Node.DOCUMENT_NODE`](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType#node.document_node) (`9`)
[`Node.DOCUMENT_TYPE_NODE`](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType#node.document_type_node) (`10`)
[`Node.DOCUMENT_FRAGMENT_NODE`](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType#node.document_fragment_node) (`11`)


## 39.5 요소 노드의 텍스트 조작
### 39.5.1 nodeValue
- 노드 객체의 값을 반환한다.
- 값은 텍스트 노드의 텍스트
- 즉 텍스트 노드가 아닌 다른 노드의`nodeValue`를 참조 하면 `null`을반환한다.

```js
console.log(document.nodeValue); // null

const $textNode = $foo.firstChild;
console.log($textNode.nodeValue); //Hello
```

이렇게 참조할수 있을 뿐만 아니라 값도 변경이 가능하다.

```js
const $textNode = $foo.firstChild;
$textNode.nodeValue = 'world';

console.log($textNode.nodeValue); //world
```

### 39.5.2 textContent
- 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 취득하거나 변경 한다.
- 만약 `HTML NODE`가 들어있따면 무시한다.

```html
<div id='foo'> Hello <span> world!</span></div>
```
```js
console.log(document.getElementById('foo').textContent); // Hello world!
```

```html
<div id='foo'> Hello <span> world!</span></div>
```
```js
document.getElementById('foo').textContent = 'HI <sppn> there!</span>';
```

`innerText` 프로퍼티가 `textContent`와 비슷한 역할을 하지만 속도가 느리다.


## 39.6 DOM 조작
- 새로운 노드를 생성하여 DOM 에 추가 하거나
- 기존 노드를 삭제 또는 교체
이렇게 노드가 변경될경우 리플로우와 리페인트가 발생한다.


### 39.6.1 innerHTML
- 요소노드의 콘텐츠 영역(시작 태그와 종료 태그 사이) 내에 포함된 모든 HTML 마크업을 문자로 반환한다.
```js
document.getElementById('foo').innerHTML = 'Hi <span>there!</span>';
```
- 또한 DOM 조작도 가능하다.
```js
//노드 추가
$fruits.innerHTML += '<li class="banana"> Banana </li>';
//노드 변경
$fruits.innerHTML = '<li class="orange"> Orange </li>';

//노드 삭제
$fruits.innerHTML = '';
```

- 사용자로부터 입력받은 데이터를 그대로 `innerHTML` 프로퍼티에 할당하는 것은 크로스 사이트 스크립팅 공격에 취약하다.
- 악성코드가 그대로 실행될 가능성이 있기 때문이다.
- 또한 `=`을 사용할 경우 유지해야할 자식 노드까지 모두 제거 한다.
- 또한 삽입할 위치를 조정하기 어렵다.


### 39.6.2 insertAdjacentHTML 메서드
- 기존 요소를 제거하지 않고 위치를 지정해 새로운 요소를 삽입한다.
![](https://i.imgur.com/EMIIDib.png)
```js
$foo.insertAdjacentHTML('beboforebegin','<p>beforebegin</p>')
```
- 기존 요소에 영향을 주지 않고 새롭게 삽입될 요소만을 파싱하여 자식 요소로 추가 한다.
- `innerHTML`과 동일하게 HTML 마크업 문자열을 파싱하므로 크로스사이팅 스크립팅 공격에 취약하다.

### 39.6.3 노드 생성과 추가
html 마크업 문자열을 파싱하여 노드를 생성하는것 뿐만 아니라 DOM에서 직접도 가능하다.


```js
// 요소 노드 생성
const $li = document.createElement('li');

// 텍스트 노드 생성
const textNode = document.createTextNode('Banana');

//텍스트 노드를 요소 노드의 자식 노드로 추가.
$li.appendChild(textNode);

//이렇게도 추가가 가능하다.
$li.textContent = 'Banana';

//요소 노드를 DOM에 추가
$fruits.appendChild($li);

```

여러개의 노드를 추가할 수도 있다.

```js
const $fragment = document.createDocumentFragment();

['Apple', 'Banana', 'Orange'].forEach(text => {

	const $li = document.createElement('li');
	const textNode = document.createTextNode(text);

	$li.appendChild(textNode);
	$fragment.appendChild($li);
});

$fruits.appendChild($fragment);
```

### 39.6.5 노드 삽입

```js
//요소 노드의 마지막 자식 노드에 추가
document.getElementById('fruits').appendChild($li);

// $li 요소를 $fruits 마지막 요소 앞에 삽입
$fruits.inserBefore($li, $fruits.lastElementChild);
```
- 당연히 두번째 인수로 전달 받은 노드는 반드시 호출한 노드의 자식노드여야 한다.
- 만약 2번째 인수로 전달 받은 노드가 `null`일 경우 마지막에 삽입한다.

### 39.6.6 노드 이동
`DOM`에 이미 존재하고 있는 노드를 `appendChild` 또는 `insertBefore` 을 이용하여 DOM에 다시 추가 하면 기존 노드를 지우고 새로 추가 즉 이동한다.

```js
const [$apple, $banana, ] = $fruits.children;

$fruits.appendChild($apple); // Banana - orange - apple
$fruits.insertBefore($banana, $fruits.lastElementChild); // Orange, Banana, Apple
```

### 39.6.7 노드 복사

```js
//얕은 복사를 하여 노드 복사 즉 textnode가 없다.
$shallowClone = $apple.cloneNode();
$shallClone.textContent = 'Banana';

$fruits.appendChild($shallowClone); // Apple - Banana

// 모든 자손 노드까지 다 복사
const $deepClone = $fruits.cloneNode(true);


```

### 39.6.9 노드 교체
```js
const $newChild = document.createElement('li');
$newChild.textContent = 'Banana';

$fruits.replaceChild($newChild, $fruits.firstElementChild);
```

### 39.6.9 노드 삭제
```js
const $fruits = document.getElementById('fruits');

$fruits.removeChild($fruits.lastElementChild);
```

## 39.7 어트리뷰트
### 39.7.2 어트리뷰트 조작
- `getAttribute` 어트류비트 를 가져온다
- `setAttribute` 어트리뷰트에 값을 지정한다.
```js
const $input = document.getElementById('user');

const inputValue = $input.getAttribute('value');
$input.setAttribute('value', 'foo');
```

- `hasAttribute` HTML 어트리뷰트가 존재하는지 확인
- `removeAttribute` HTML 어트리뷰트를 삭제한다.

### 39.7.3 HTML 어트리뷰트 VS DOM 프로퍼티

- HTML 어트리뷰트 : HTML 요소의 초기 상태를 지정, 변하지 않는다.
- DOM 프로퍼티 : 요소 노드의 최신 상태를 관리

초기상태를 지정해야 웹 페이지를 처음 표시하거나 새로 고침할때 초기 상태를 표시할 수 없기 때문이다.

### 39.7.4 data 어트리뷰트와 dataset 프로퍼티
- HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터를 교환 가능
- `data-user-id`, `data-role` 같이 `data` 접두사를 붙여서 사용 가능하다.
```html
<ul class="users">
	<li id ="1" data-user-id="7621" data-role="admin">Lee</li>
	<li id ="2" data-user-id="9524" data-role="subscriber">Kim</li>
</ul>
```
```js
const users = [...document.querySelector('.users').children];
const user = users.find(user => user.dataset.userId ==='7621');

console.log(user.dataset.role); //admin
```

## 39.8 스타일

### 39.8.1 인라인 스타일 조작

```js
const $div = document.querySelector('div');

$div.style.color = 'blue';
$div.style.width = '100px';
$div.style.height = '100px';
```

### 39.8.2 클래스 조작

- `className` 프로퍼티를 참조하면 값을 문자열로 반환, 문자열을 할당하면 값을 할당
```html
<div class="box red">Hello World</div>
```
```js
const $box = document.querySelector('.box');
console.log($box.className); // 'box red'

$box.className = $box.className.replace('red', 'blue');
```

- `classList` class 어트리뷰트의 정보를 담은 `DOMTokenList` 객체를 반환한다.

```js
const $box = document.querySelector('.box');

console.log($box.classList);
// DOMTokenList(2) [length : 2, value = "box blue" , 0: "box", 1: "blue"]
```

#### `classList`의 유용한 메서드
- `add`
- `remove`
- `item(index)` : 인수로 전달한 index에 해당하는 클래스를 반환
- `contains(className)` :  인수로 전달한 문자열과 일치하는 클래스가 있는지 확인(boolean)
- `replace(oldClassName, newClassName)`:  class 어트리뷰트 변경
- `toggle(className)`  : class 어트리뷰트로 인수로 전달한 문자열과 일치하는 클래스가 있으면 제거, 없으면 추가

### 39.8.3 요소에 적용되어 있는 스타일 참조
- `style` 프로퍼티는 인라인 스타일만 참조
- 클래스를 적용, 상속을 통한 style 은 참조할수 없다.
- `getComputedStyle` 메서드로 참조가 가능하다.
```js
const $box = document.querySelector('.box');
const computedStyle = window.getComputedStyle($box);

console.log(computedStyle.width);

//의사코드로 지정하는 문자열을 전달받을수 있다.
$computedStyle = window.getComputedStyle($box, ':before');
console.log(computedStyle.content); //"hello"
```
