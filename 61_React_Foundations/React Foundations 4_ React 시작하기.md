# 4장: React 시작하기 - Getting Started with React

새로 생성된 프로젝트에서 React를 사용하려면, [unpkg.com](https://unpkg.com/)이라는 외부 웹사이트에서 두 개의 React 스크립트를 로드하세요:

- **react**는 핵심 React 라이브러리입니다.
- **react-dom**은 DOM에 특화된 메소드를 제공하여 DOM과 함께 React를 사용할 수 있게 해줍니다.

`index.html`

```
<html>
  <body>
    <div id="app"></div>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script type="text/javascript">
      const app = document.getElementById('app');
      const header = document.createElement('h1');
      const text = 'Develop. Preview. Ship.';
      const headerContent = document.createTextNode(text);
      header.appendChild(headerContent);
      app.appendChild(header);
    </script>
  </body>
</html>
```

순수 자바스크립트로 직접 DOM을 조작하는 대신, 이전에 추가한 DOM 메소드를 제거하고, 특정 DOM 요소를 대상으로 하여 React 컴포넌트를 표시할 루트를 생성하는 `ReactDOM.createRoot()` 메소드를 추가하세요. 그 다음, React 코드를 DOM에 렌더링하기 위해 `root.render()` 메소드를 추가하세요.
> - **ReactDOM.createRoot()**
> https://react.dev/reference/react-dom/client/createRoot
> - **root.render()**
> https://react.dev/reference/react-dom/client/hydrateRoot#root-render

이것은 React에게 `#app` 요소 내부에 `<h1>` 타이틀을 렌더링하라고 지시합니다.

`index.html`

```
<html>
  <body>
    <div id="app"></div>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/jsx">
      const app = document.getElementById('app');
      const root = ReactDOM.createRoot(app);
      root.render(<h1>Develop. Preview. Ship.</h1>);
    </script>
  </body>
</html>
```

이 코드를 브라우저에서 실행하려고 하면 문법 오류가 발생합니다:

`Terminal`

```
Uncaught SyntaxError: expected expression, got '<'
```

이는 `<h1>...</h1>`이 유효한 자바스크립트가 아니기 때문입니다. 이 코드 조각은 **JSX**입니다.

---

## JSX란 무엇인가? - What is JSX?

JSX는 자바스크립트의 문법 확장으로, 익숙한 *HTML과 비슷한* 문법으로 UI를 설명할 수 있게 해줍니다. JSX의 좋은 점은 **세 가지 JSX 규칙**을 따르는 것 외에 HTML과 자바스크립트 외에 새로운 기호나 문법을 배울 필요가 없다는 것입니다.
> **세 가지 JSX 규칙**
> https://react.dev/learn/writing-markup-with-jsx#the-rules-of-jsx

하지만 브라우저는 기본적으로 JSX를 이해하지 못하므로, JSX 코드를 일반 자바스크립트로 변환하기 위해 **Babel**과 같은 자바스크립트 컴파일러가 필요합니다.
> https://babeljs.io/

---

## 프로젝트에 Babel 추가하기 - Adding Babel to your project

프로젝트에 Babel을 추가하려면, `index.html` 파일에 다음 스크립트를 복사하여 붙여넣으세요:

`index.html`

```
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
```

또한, Babel이 변환할 코드를 알리기 위해 스크립트 타입을 `type=text/jsx`로 변경해야 합니다.

`index.html`

```
<html>
  <body>
    <div id="app"></div>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <!-- Babel Script -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/jsx">
      const domNode = document.getElementById('app');
      const root = ReactDOM.createRoot(domNode);
      root.render(<h1>Develop. Preview. Ship.</h1>);
    </script>
  </body>
</html>
```

정상적으로 작동하는지 확인하기 위해 브라우저에서 HTML 파일을 엽니다.

방금 작성한 **선언적** React 코드:

`index.html`

```
<script type="text/jsx">
  const domNode = document.getElementById("app");
  const root = ReactDOM.createRoot(domNode);
  root.render(<h1>Develop. Preview. Ship.</h1>);
</script>
```

이전 섹션에서 작성한 **명령적** 자바스크립트 코드와 비교하면:

`index.html`

```
<script type="text/javascript">
  const app = document.getElementById('app');
  const header = document.createElement('h1');
  const text = 'Develop. Preview. Ship.';
  const headerContent = document.createTextNode(text);
  header.appendChild(headerContent);
  app.appendChild(header);
</script>
```

React를 사용하면 많은 반복적인 코드를 줄일 수 있음을 볼 수 있습니다.

React는 바로 이것을 합니다. 당신을 대신하여 작업을 수행하는 재사용 가능한 코드 조각을 포함한 라이브러리입니다 - 이 경우 UI를 업데이트하는 것입니다.

> **추가 자료:**
> 
> React가 UI를 어떻게 업데이트하는지 정확히 알 필요는 없지만, 더 배우고 싶다면 다음 추가 자료를 참고하세요:
>
> - UI 트리
> https://react.dev/learn/preserving-and-resetting-state#the-ui-tree
> - JSX로 마크업 작성하기
> https://react.dev/learn/writing-markup-with-jsx
> - React 문서의 react-dom/server 섹션
> https://react.dev/reference/react-dom/server

---

## React를 위한 필수 자바스크립트 - Essential JavaScript for React

자바스크립트와 React를 동시에 배울 수는 있지만, 자바스크립트에 익숙하다면 React 학습 과정을 더 쉽게 만들 수 있습니다.

다음 섹션에서는 자바스크립트 관점에서 React의 몇 가지 핵심 개념을 소개할 것입니다. 언급될 자바스크립트 주제의 요약은 다음과 같습니다:

- 함수
https://developer.mozilla.org/docs/Web/JavaScript/Guide/Functions
- 화살표 함수https://developer.mozilla.org/docs/Web/JavaScript/Reference/Functions/Arrow_functions
- 객체 https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object
- 배열 및 배열 메소드 https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array
- 구조 분해 할당 https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment
- 템플릿 리터럴 https://developer.mozilla.org/docs/Web/JavaScript/Reference/Template_literals
- 삼항 연산자 https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Conditional_Operator
- ES 모듈 및 Import / Export 문법 https://developer.mozilla.org/docs/Web/JavaScript/Guide/Modules

이 과정에서 자바스크립트에 깊게 들어가지는 않지만, 자바스크립트의 최신 버전을 계속해서 익히는 것이 좋은 실습입니다. 하지만 자바스크립트에 아직 능숙하지 않다면, React로 빌드를 시작하는 데 방해받지 않도록 하세요!