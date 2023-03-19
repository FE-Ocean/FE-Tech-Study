# DOM (Document Object Model)
- 자바스크립트 등의 프로그래밍 언어가 HTML/XML 문서의 노드에 접근 및 노드를 추가/수정/제거하는 등 조작할 수 있게 하는 인터페이스
- 웹 페이지의 객체 지향 표현
- 부모/자식의 관계를 가지는 트리 형태
- `document.getElementById(id)` 등의 메서드를 이용하여 접근 및 조작 가능
    |모델|대상|
    |:-:|:-:|
    |Core DOM|모든 종류의 문서|
    |HTML DOM|HTML 문서|
    |XML DOM|XML 문서|

## DOM 노드
- 각 노드는 `nodeType`, `nodeValue`, `nodeName` 속성을 가짐
- 노드의 종류에 따라 클래스가 다름 -> 지원하는 `property` 가 달라짐
- 클래스는 상위 노드로부터 상속받음
- 루트 노드는 `document`, `document`의 유일한 자식 노드는 `<html>` 노드
    |종류|대상|nodeType|nodeValue|nodeName|클래스|
    |:-:|:-:|:-:|:-:|:-:|:-:|
    |Element Node|문서의 요소|1|-|요소 이름 (대문자)|HTMLElement (요소에 따라 세분화됨)|
    |Attribute Node|요소의 속성 / 스타일|2|속성 / 스타일 값|속성 이름|Attr|
    |Text Node|요소 안의 텍스트|3|텍스트|#text|Text|
    |Comment Node|주석|8|주석 내용|#comment|Comment|
    |Document Node|전체 문서 (루트 노드)|9|-|#document|HTMLDocument|

## `attribute` 와 `property`
- HTML/XML 요소의 `attribute` <-> DOM의 `property`
- 각 HTML 요소의 표준 `attribute` 는 자동으로 같은 이름의 `property` 로 변환되어 `element.attributeName` 으로 접근할 수 있지만, 해당 요소의 비표준 `attribute` 가 있는 경우에는 `property`로 변환되지 않으므로 `element.getAttribute(attributeName)` 등을 이용하여 접근해야 함
- 보통 한 쪽이 변경되면 다른 쪽도 변경되지만, 그렇지 않은 경우도 있음

```html
<input id="message" value="hello" />
```
```javascript
const message = document.querySelector("#message");
// input의 값을 변경했을 때
message.addEventListener("change", () => {
    console.log(message.getAttribute("value")); // 값이 변경되어도 hello 그대로 출력됨
    console.log(message.value); // 변경 사항이 반영됨
});
```

## `NodeList`
- `element.childNodes`, `document.getElementsByClassName(class)` 등의 메서드를 사용했을 때 반환되는 노드들의 목록
- 변경 사항이 실시간으로 반영됨
- **배열 아님** -> 배열처럼 `forEach()`, `length` 를 사용하거나 인덱스로 접근 가능하지만, `Array` 의 메서드를 모두 사용할 수는 없음

## 가상 요소
- `::before`, `::after` 등의 가상 요소는 DOM이 아니므로 JavaScript로 조작할 수 없음

# Shadow DOM
- DOM 노드에 개별적으로 생성할 수 있는 DOM 트리 -> 캡슐화
- 전역 스타일에 영향을 받지 않음
- `element.attachShadow()` 메서드를 이용하여 생성 가능
- 루트 노드는 #shadow-root
- `<iframe>` 과 같은 역할
- `<slot>` 을 이용하여 커스텀 가능 -> 컴포넌트화

# 가상 DOM (Virtual DOM)
- DOM을 추상화한 자바스크립트 객체
- DOM 트리의 복사본을 메모리에 저장
- 브라우저의 렌더링 횟수를 줄이고 성능 최적화 및 유지 보수를 간단하게 하기 위함
- 일부만 바뀌어도 전체를 다시 렌더링하는 DOM과 달리, 변경 사항을 가상 DOM에 반영 후 DOM과 비교하여 바뀐 부분만 렌더링 -> 효율적이고 빠름
- React, Vue 등의 SPA 프레임워크/라이브러리에서 사용
- 메모리 사용량 증가

## 참고 사이트
- https://developer.mozilla.org/ko/docs/Glossary/DOM
- https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction
- https://ko.javascript.info/dom-nodes
- https://ko.javascript.info/basic-dom-node-properties
- https://ko.javascript.info/dom-attributes-and-properties
- https://wit.nts-corp.com/2019/02/14/5522
- http://jun.hansung.ac.kr/CWP/Javascript/JavaScript%20HTML%20DOM.html
- http://www.ktword.co.kr/test/view/view.php?m_temp1=6104
- https://ui.toast.com/posts/ko_20170721
- https://www.educative.io/answers/what-is-the-difference-between-virtual-and-real-dom-react
- https://www.geeksforgeeks.org/difference-between-virtual-dom-and-real-dom/