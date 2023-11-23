# 모듈 번들러

## 1. 모듈이란
> `<script type="module" src="스크립트경로"></script>`
- 코드 재사용, 유지보수를 위해 여러개로 분리된 파일을 의미
- 모듈 시스템 있기 전엔 여러 js 파일을 사용할 경우 전역 스코프를 공유한다는 문제가 있었음.
- 모듈은 자신만의 스코프를 가지기에 이러한 문제를 해결할 수 있음.

<br/>

## 2. 모듈 시스템
모듈을 구성하고 관리하는 방법, 모듈을 불러오는 방법을 모듈 시스템이라고 함.

### 2-1. 모듈 시스템의 장점
- 유지보수용이 : 기능들이 모듈화가 잘 되어 있는 경우, 의존성을 줄일 수 있기 때문에 기능을 개선이나 수정하기 용이
- 네임스페이스화 : 코드양이 많아질수록 전역 스코프에 존재하는 변수명 겹치는 경우 발생, 모듈 분리시 모듈만의 네임스페이스 갖기 때문에 문제 해결
- 재사용성 : 같은 코드를 반복하지 않고 모듈로 분리시켜 필요할 때마다 재활용 가능

### 2-2. 대표적 모듈 시스템
### 2-2-1. CommonJS
- NodeJS 환경을 위해 만들어진 모듈 시스템
- 모듈을 외부에서 사용할 수 있도록 내보낼 때 exports, module.export 키워드 사용
- 외부에서 모듈 불러올 땐 require 사용

### 2-2-2. ES Module
- ES6에 도입된 자바스크립트 모듈 시스템
- `<script>` 태그에 `type="module"` 속성을 추가해주면 해당 파일은 모듈로서 동작
- 모듈을 외부에서 사용할 수 있도록 내보낼 때 export, default export 키워드 사용
- 외부에서 모듈 불러올 때는 import 사용

> NodeJS 13.2버전부터 ESModule 사용할 수 있음<br/>
> package.json -> `type="module"` 선언

<br/>

## 3. 모듈을 번들링하는 '모듈번들러'
> 웹 애플리케이션을 구성하는 자원(HTML, CSS, JavaScript, Images 등)을 모두 각각의 모듈로 보고 <br/>
이를 조합해서 병합된 하나의 결과물을 만드는 도구 

- 웹 어플리케이션이 완성되면 웹 서버라는 공간에 배포하고 사용자들이 브라우저를 통해 웹사이트에 접근하게 되면<br/>
브라우저는 사용자들에게 ui를 보여주기 위해 웹 서버에 html, css, image, javaScript 파일과 같은 자원을 요청한다.
- 웹서버는 준비된 자원을 브라우저에 응답함으로써 사용자들은 ui를 볼 수 있게 된다.
- 하지만 개발 편의성을 위해 모듈을 계속 분리하다보면 그만큼 브라우저에서 서버에 요청하는 자원의 개수가 늘어나고,<br/>
이러한 네트워크 비용 증가는 페이지 로딩시간을 길어지게해 사용자경험 측면에서 좋지 못하다.

- 개발 편의를 위해 모듈로 분리하여 개발하되, 웹서버 배포전 모듈을 하나의 파일로 묶어서 배포하여<br/>
브라우저에서 서버로 요청하는 http개수를 줄이고, 페이지 로딩시간을 줄이는 것이 모듈 번들러의 목적

- 이렇게 웹 애플리케이션을 구성하는 몇십, 몇백개의 자원(모듈)들을 하나의 파일로 병합 및 압축 해주는 작업을 **번들링**이라고 한다.<br/>
- 여러개의 파일을 하나로 묶어주는 도구를 **모듈 번들러**라고 하며, 프론트엔드의 대표적인 모듈 번들러는 웹팩이다.<br/>
<image src='https://joshua1988.github.io/webpack-guide/assets/img/webpack-bundling.e79747a1.png' width="500"/>

<br/>

## 4. webpack
- 주로 js 파일을 위한 모듈 번들러지만 플러그인을 포함하는 경우 HTML, CSS, JavaScript, Images 등의 자원들도 변환 가능
- 배포 전 모듈을 번들링하고, 배포할 땐 dist 디렉토리에 번들링한다음 해당 디렉토리에있는 JavaScript 파일만 배포
- 모듈번들러 사용으로 프론트엔드에서 서버로 http 요청 개수를 줄여줌으로써 퍼포먼스를 확장시키고<br/>
공백을 줄임으로써 리소스를 최적화함 결과적으로 사용자 경험이 좋아짐

### 4-1. 웹팩의 4가지 주요 속성
> entry : 모듈 시작점<br/>
> output : 번들 파일 저장 경로<br/>
> loader : 기타 파일 모듈 취급<br/>
> plugin : 번들 파일 관련 후처리<br/>

#### 4-1-1. entry 
- 웹팩 실행시 해당 파일을 대상으로 웹팩이 빌드를 수행
- 이때 엔트리 포인트는 여러개가 될수 있으며 엔트리 포인트를 분리하는 경우 멀티 페이지 애플리케이션에 적합
```js
entry: {
    login: './src/LoginView.js',
    main: './src/MainView.js'
}
```

#### 4-1-2. output 
- 웹팩을 돌리고 난 결과물의 파일 경로, 일반적으로 filename과 path를 정의
    - filename 속성은 웹팩으로 빌드한 파일의 이름을 의미
    - path 속성은 해당 파일의 경로를 의미
    - path 속성에서 사용된 path.resolve() 코드는 인자로 넘어온 경로들을 조합하여 유효한 파일 경로를 만들어주는 Node.js API

#### 4-1-3. loader 
- 자바스크립트 파일이 아닌 웹 자원을 웹팩이 인식할 수 있게 추가하는 속성
- 로더를 여러개 사용하는 경우 rules 배열에 로더 옵션을 추가하면 됨.
- 로더는 오른쪽에서 왼쪽 순으로 적용
    - test : loader를 적용할 파일 유형 (일반적으로 정규 표현식 사용)
    - use :  해당 파일에 적용할 로더의 이름

#### 4-1-4. plugin
- 웹팩의 기본적인 동작에 추가적인 기능을 제공하는 속성
- 로더는 파일을 해석하고 변환하는 과정에 관여한다면 플러그인은 해당 결과물의 형태를 바꾸는 역할 수행
- 플러그인 배열에는 생성자 함수로 생성한 객체 인스턴스만 추가할 수 있음.

```js
// webpack.config.js
const path = require('path'); //  Node.js의 내장 모듈인 path를 가져와서 파일 경로를 조작하는 데 사용
var webpack = require('webpack'); // 웹팩 모듈을 가져오기
var HtmlWebpackPlugin = require('html-webpack-plugin'); // - HTML 파일을 생성하는 웹팩 플러그인

module.exports = {
    mode: 'production', // or development : 해당 모드에 따라 웹팩의 최적화 및 빌드 방식이 달라짐
    entry: './src/index.js', // 번들링 진입점 설정
    output: { // 번들링 파일이 위치할 path, 파일명 지정
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js',
    },
    module: {
        rules: [ // CSS 파일을 처리하기 위한 규칙
            {
                test: /\.css$/,
                use: ['css-loader'] 
                // 해당 프로젝트의 모든 css 파일에 대해 css 적용하겠다는 의미
            }
        ]
    },
    plugins: [
        new HtmlWebpackPlugin(), // 웹팩으로 빌드한 결과물로 HTML 파일을 생성해주는 플러그인
        new webpack.ProgressPlugin() // 웹팩의 빌드 진행율을 표시해주는 플러그인
    ]
};
```
<br/>

### 참고자료
- https://joshua1988.github.io/webpack-guide/webpack/what-is-webpack.html#%EC%9B%B9%ED%8C%A9%EC%97%90%EC%84%9C%EC%9D%98-%EB%AA%A8%EB%93%88
- https://www.youtube.com/watch?v=NGVc-zw2FG8