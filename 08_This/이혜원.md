# this
- 호출 방식에 따라 동적으로 결정되는 **객체**
- 값을 임의로 변경할 수 없음

## 전역에서 호출 시
- `window` (브라우저), `module.exports` (노드)를 가리킴

```javascript
// 브라우저 (개발자 도구)
console.log(this); // 출력: Window {window: Window, self: Window, …}

// 노드 (터미널)
console.log(this); // 출력: {}

this.name = "global this";
// 노드의 경우 module.exports.name = "global this"; 도 동일

// 브라우저 (개발자 도구)
console.log(this); // 출력: Window {window: Window, self: Window, …}

// 노드 (터미널)
console.log(this); // 출력: { name: 'global this' }
```

## 일반 함수에서 호출 시
- **비엄격 모드**에서는 **전역 객체** ( `window` (브라우저), `global` (노드) )를 가리킴
- **엄격 모드**에서는 `undefined`
- `call()`, `apply()`, `bind()`를 이용하여 `this`를 직접 **지정** 가능

```javascript
function nonStrictFunc() {
    console.log(this);
}

function strictFunc() {
    "use strict";
    console.log(this);
}

// 브라우저 (개발자 도구)
nonStrictFunc(); // 출력: Window {window: Window, self: Window, …}

// 노드 (터미널)
nonStrictFunc(); // 출력: <ref *1> Object [global] {global: [Circular *1], ...}

// 브라우저, 노드 동일
strictFunc(); // 출력: undefined
```

### `call()`
- 인자를 **나열**하는 방식으로 전달
- 함수를 **즉시 실행**
```javascript
functionName.call( this로 지정할 객체, 인자 1, 인자 2, ... );
```

```javascript
function f(n1, n2){
    console.log(this);
    console.log(n1 + n2);
}

f.call({name: "call"}, 1, 2); // 출력: {name: 'call'}, 3
```

### `apply()`
- 인자를 **배열**으로 전달
- 함수를 **즉시 실행**
```javascript
functionName.apply( this로 지정할 객체, [인자 1, 인자 2, ...] );
```

```javascript
function f(n1, n2){
    console.log(this);
    console.log(n1 + n2);
}

f.apply({name: "apply"}, [1, 2]); // 출력: {name: 'apply'}, 3
```

### `bind()`
- 함수에 인자를 전달 후 그 **함수를 반환**
- 함수를 즉시 실행하지 않기 때문에 **나중에 실행할 수 있음**
```javascript
const newFunction = functionName.bind( this로 지정할 객체, 인자 1, 인자 2, ...);
...
newFunction();

// bind 시 원래 function의 인자보다 적게 준 경우에는 추가 인자를 넘겨줄 수도 있음
// 원래 function의 인자의 개수보다 많이 넘겨주면 나머지 인자는 무시됨
newFunction(인자 3, 인자 4, ...);
```

```javascript
function f(n1, n2){
    console.log(this);
    console.log(n1 + n2);
}

const bind = f.bind({name: "bind"}, 1, 2);
bind(); // 출력: {name: 'bind'}, 3

const bind2 = f.bind({name: "bind2"}, 3);
bind2(4); // 출력: {name: 'bind2'}, 7
bind2(5); // 출력: {name: 'bind2'}, 8
```

## 화살표 함수에서 호출 시
- 자신을 감싸고 있는 **바로 상위의 렉시컬 환경**을 가리킴 (상위 객체가 전역인 경우 전역 객체를 가리킴)
- `call()`, `apply()`, `bind()`를 사용하여 인자를 지정해 줄 수는 있으나 `this`**의 지정은 불가**

```javascript
this.name = "global this";

const f = () => {
    console.log(this.name);
}

const outerFunc = () => {
    this.name = "arrow this";
    const innerFunc = () => console.log(this.name);
    innerFunc();
}

f(); // 출력: global this
outerFunc(); // 출력: arrow this
```

## 객체의 메서드에서 호출 시
- 메서드를 **호출한 객체**를 가리킴
- **getter**, **setter**에서도 동일

```javascript
const obj = {
    name: "obj",
    getThis: function() {
        console.log(this);
    },
    setThis: function(str) {
        this.name = str;
    }
}

obj.getThis(); // 출력: {name: 'obj', getThis: ƒ, setThis: ƒ} (= obj 객체)
obj.setThis("obj this");
obj.getThis(); // 출력: {name: 'obj this', getThis: ƒ, setThis: ƒ} (= obj 객체)
```

## 생성자 함수에서 호출 시
- **생성자로 만들어진 새 객체**를 가리킴

```javascript
function obj() {
    this.name = "obj";
    this.printThis = function() {
        console.log(this);
    }
}

const newObj = new obj();
newObj.name = "new obj";
newObj.printThis(); // 출력: {name: 'new obj', printThis: ƒ} (= newObj 객체)
```

## 이벤트 핸들러에서 호출 시
- **이벤트가 연결된 객체**를 가리킴 (콜백 함수가 화살표 함수인 경우는 해당되지 않음)
- HTML에서의 이벤트 핸들러 호출은 **해당 HTML 요소**를 가리킴

```html
<button id="test">click</button>
<p id="result"></p>
```
```javascript
const button = document.getElementById("test");
const result = document.getElementById("result");
button.addEventListener("click", function () {
    console.log(this); // 출력: <button id="test">click</button>
    result.textContent = this.outerHTML;
});
```

```html
<button id="test" onclick="console.log(this);">click</button>
<!-- 출력: <button id="test" onclick="console.log(this);">click</button> -->
```

## 참고 사이트
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/this
- https://poiemaweb.com/js-this
- https://www.w3schools.com/js/js_this.asp
- https://www.zerocho.com/category/JavaScript/post/5b0645cc7e3e36001bf676eb
- https://www.zerocho.com/category/NodeJS/post/5b67e8607bbbd3001b43fd7b