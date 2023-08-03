# React와 JSX

## JSX

> JSX는 XML 같은 ECMAScript(Javascript 표준)의 문법 확장이다.
> 브라우저에서 실행하기 전에 코드가 번들링되는 과정에서 바벨을 통해 React.createElement을 쓰는 JavaScript 코드로 변환된다.

## React에서 JSX를 사용하는 목적

> JavaScript 코드 내에서 HTML과 유사한 마크업을 직접 작성할 수 있어 익숙하고 가독성이 좋다.
> JSX로 div, span 등의 태그를 사용할 수 있으며, 개발자가 만든 컴포넌트도 JSX안에서 작성할 수 있어 활용도가 높다.

❕ 하지만 React를 사용할 때 JSX가 꼭 필수는 아니라는것

## Syntactic sugar

> 사람이 이해 하고 표현하기 쉽게 디자인된 프로그래밍 언어 문법
> 더욱 더 간결하고 명확하게 표현이 가능한 문법을 뜻 한다.

다음의 JSX는

```jsx
const element = <h1 className='greeting'>Hello, world!</h1>;
```

다음 자바스크립트 코드로 매칭된다.

```jsx
const element = React.createElement(
  'h1',
  { className: 'greeting' },
  'Hello, world!'
);
```

## React.createElement

> React.createElement는 화면에 표시하려는 React Element를 생성한다.

JSX 코드

```jsx
<div>
  <p>Count: {count}!</p>
  <button type='button' onClick={() => setCount(count + 1)}>
    Increase
  </button>
</div>
```

변환된 JS 코드

```jsx
React.createElement(
  'div',
  null,
  React.createElement('p', null, 'Count: ', count, '!'),
  React.createElement(
    'button',
    { type: 'button', onClick: () => setCount(count + 1) },
    'Increase'
  )
);
```

## React Element

> 화면에 표시할 내용으로, 브라우저 DOM 엘리먼트와 달리 일반 객체다.
> React DOM은 React Element와 일치하도록 DOM을 최신으로 업데이트한다.

[createElement]('https://react.dev/reference/react/createElement') [Element]('https://ko.legacy.reactjs.org/docs/rendering-elements.html')

## React StrictMode

> StrictMode는 개발시에만 활성화 되며, 컴포넌트 트리 내부에 잠재적 문제를 알아내기 위한 도구. 앱 전체에서 StrictMode를 사용할 수도 있고, 부분적으로도 사용할 수 있다.

```jsx
// 앱 전체에서 사용
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'));
root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

- 재 렌더링을 하여 모든 컴포넌트가 항상 동일한 JSX를 반환하는 순수한 함수인지 검사합니다.

- React 컴포넌트가 마운트될 때(화면에 추가됨) setup을 호출, 구성 요소가 마운트 해제될 때(화면에서 제거됨) cleanup을 호출하는 과정을 한번 더 실행하여 버그를 검사합니다.

- 더 이상 사용되지 않는 API 사용에 대해 컴포넌트를 확인합니다.

[StrictMode]('https://react.dev/reference/react/StrictMode#fixing-bugs-found-by-double-rendering-in-development')
