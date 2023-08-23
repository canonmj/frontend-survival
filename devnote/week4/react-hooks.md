# React Hook

## React Hook 이란

> React에서 기존의 Class 바탕의 코드를 작성할 필요 없이 함수형 컴포넌트로 상태값을 관리하고, 생명주기 함수를 이용할 수 있게 해주는 React의 요소이다.

[Hooks](https://ko.legacy.reactjs.org/docs/hooks-intro.html)

### Hook의 종류(Built in)

### useState

> 상태를 초기화 하고 제어하는 훅이다. state 변수와 state에 대한 setter 함수를 제공한다. generic으로 타입을 보장할 수도 있다.

```jsx
const [count, setCount] = useState < number > 0;

const [text, setText] = useState < string > "";
```

### useEffect

> React 컴포넌트 안에서 데이터를 가져오거나 구독하고, **DOM을 직접 조작**하는 작업을 “side effects”(또는 짧게 “effects”)라고 한다. 다른 컴포넌트에 영향을 줄 수도 있고, 렌더링 과정에서는 구현할 수 없는 작업이기 때문이다.
> useEffect는 함수 컴포넌트 내에서 렌더링 이후에 side effects를 수행할 수 있게 해준다.

기본적으로 React는 매 렌더링 이후에 effects를 실행한다.
다음 예시에서는 React가 DOM을 업데이트한 뒤에 문서의 타이틀을 바꾼다.

```js
import React, { useState, useEffect } from "react";

function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    // 브라우저 API를 이용해 문서의 타이틀을 업데이트합니다
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

Effect를 “해제”할 필요가 있다면, 해제하는 함수를 반환해주면 된다.
클린업 함수는 useEffect의 return 값으로 정의하는데, 컴포넌트가 종료(삭제)될 경우 함수가 실행된다.

```js
// clearInterval을 통해 종료처리한다
useEffect(() => {
  const savedTitle = document.title;

  const id = setInterval(() => {
    document.title = `Now: ${new Date().getTime()}`;
  }, 100);

  return () => {
    document.title = savedTitle;
    clearInterval(id);
  };
});
```

의존성 배열을 통해 변화시에 rerendering할 state를 지정할 수 있다. 빈 배열일 경우 한번만 실행하고, 아닌 경우에는 하나씩 하나씩 추가해줄 수 있다.

```js
const [count, setCount] = useState();

useEffect(() => {
  alert("count changed");
}, [count]);

useEffect(() => {
  alert("mount");
}, []);
```

따라서 useEffect에 빈 배열을 넣는 경우는 처음으로 변수가 생성될 때, 즉 컴포넌트가 최초로 렌더링 되는 마운트 (mount)시에 단 한번 실행될 때를 위해 사용한다.
이는 클래스 컴포넌트의 라이프사이클 메소드에서 componentDidMount()의 기능과 같다고 할 수 있다.

### useContext

[useContext](https://react.dev/reference/react/useContext)는 component의 [context](https://react.dev/learn/passing-data-deeply-with-context) 를 읽고 구독하는 hook이다.

> React 공식 문서에 쓰여있는 설명에는, 'context를 이용하면 단계마다 일일이 props를 넘겨주지 않고도 컴포넌트 트리 전체에 데이터를 제공할 수 있습니다'라고 적혀있다.
> props를 넘겨주기 위해 state를 책임지는 상위 component와 실제 조작하는 component간의 계층이 멀어지면서 props drilling이 발생하게 된다. props drilling은 사용하지도 않는 component인데 중간에 있다는 것만으로 props를 받고 넘겨주는 현상을 말한다.

context API를 사용하기 위해서는 Provider , Consumer , createContext 이렇게 세가지 개념을 알고 있으면 된다.

- createContext : context 객체를 생성한다.
- Provider : 생성한 context를 하위 컴포넌트에게 전달하는 역할을 한다.
- Consumer : context의 변화를 감시하는 컴포넌트이다.

```js
const MyContext = React.createContext(defaultValue);
<MyContext.Provider value={/_ 넣고 싶은 값_/}>
  {/_ Provider가 제공하는 component tree 범위_/}
</MyContext.Provider>;

// tree 범위 내에서 마음껏 사용 가능
const value = useContext(MyContext);
```

**hook의 규칙?**

Hook 호출은 규칙이 있어서 단순하게 쓰도록 노력해야 한다.

1. Function Component 바로 안쪽(함수의 최상위)에서만 호출.
2. Function Component 또는 Custom Hook에서만 호출.

state를 사용할 때 여러 곳에서 사용하는 경우 lifting state up을 하게 되고, 그 과정에서 state를 책임지는 상위 component와 실제 조작하는 component간의 계층이 멀어지면서 props drilling이 발생하게 된다. props drilling은 사용하지도 않는 component인데 중간에 있다는 것만으로 props를 받고 넘겨주는 현상을 말한다.

그걸 해결 하기 위해 context를 만들고 그걸 전역적으로 사용 할 수 있다.

```jsx
const MyContext = React.createContext(defaultValue);

<MyContext.Provider value={/* 넣고 싶은 값*/}>

{/* Provider가 제공하는 component tree 범위*/}

</MyContext.Provider>

// tree 범위 내에서 마음껏 사용 가능

const value = useContext(MyContext);
```

### useRef

[useRef](https://react.dev/reference/react/useRef)는 렌더링에 필요하지 않은 값을 참조할 수 있는 hook이다. 초기화 후에는 `변수.current` 로 접근 가능하다. 해당 값은 변경할 수 있으며, 변경해도 state와는 달리 rerendering을 수행하지 않는다.

### useLayoutEffect

useEffect와 비슷하나 browser가 DOM 업데이트 이전에 시행한다. DOM 구성 이전에 수행되므로 DOM을 `useLayoutEffect`에서 수정할 수 있다. 서버측의 렌더링하고 호환되지 않는 경우 에러 발생할 수 있으므로 그때는 useEffect를 사용할 수 있다.

[Hooks API](https://ko.legacy.reactjs.org/docs/hooks-reference.html#usecontext)
