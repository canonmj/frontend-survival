# React Hook

## React Hook 이란

[Hooks](https://ko.legacy.reactjs.org/docs/hooks-intro.html)는 클래스 바탕 코드 작성 없이 사용할 수 있는 react의 요소이며 상태, 외부효과 등의 여러 기능을 제공한다. 커스텀 훅을 통해 직접 원하는 훅을 제작해서 사용할 수도 있다.

16.8 버전에서 새롭게 추가되었고, 필요한 곳에만 선택적으로 사용이 가능하다. React 개념에 맞는 직관적인 API를 제공한다.

이전에는 횡단 관심사(Cross-Cutting Concerns : 여러 컴포넌트에서 공통으로 사용되며 중복되고 얽혀있는 관심사) 를 처리하기 위해 [HOC(High Order Component : 고차 컴포넌트)](https://ko.legacy.reactjs.org/docs/higher-order-components.html) 를 씌워서 새 컴포넌트를 만들어서 해결했다. 하지만 이를 위해 컴포넌트를 계속 다시 만들면서 코드 흐름을 따라가기 어렵게 만든다. 이런 문제가 중첩이 되면 HOC, render props, 추상화된 레이어가 겹쳐서 Wrapper Hell을 유발한다. 계층이 깊어져서 생기는 비슷한 지옥(callback Hell)이 Javascript에서도 있었던것 같다. 뭐든 정도껏 해야지 지나치면 일을 그르치게 된다. 어쨌든, HOC의 문제를 해결하기위해 나온 hook은 계층 변화 없이 상태 관련 로직을 재사용할 수 있으며 customize도 가능하다.

### Hook의 종류(Built in)

### useState

상태를 초기화 하고 제어하는 훅이다. state 변수와 state에 대한 setter 함수를 제공한다. generic으로 타입을 보장할 수도 있다.

```jsx
const [count, setCount] = useState < number > 0;

const [text, setText] = useState < string > '';
```

### useEffect

렌더링 후에 수행되어야 할 side effect(외부 효과) 에 대해 정의가 필요할 때 `[useEffect](https://ko.legacy.reactjs.org/docs/hooks-effect.html#tip-use-multiple-effects-to-separate-concerns)`를 수행한다.

[추가 문서](https://ko.legacy.reactjs.org/docs/hooks-reference.html#useeffect)

effect의 시행 시점은 렌더링 이후, 모든 업데이트에서 수행된다

Clean-up 하지 않는 effect : DOM 업데이트 후 추가 코드 실행 - NW request, DOM 수동 조작, 로깅

Clean-up 필요 effect : 외부 데이터 구독 설정 같은 경우 - 메모리 누수 방지 필요

clean up 이 필요할 때에는 return 시에 정리 함수를 반환 한다.

구독, 타이머, 로깅 같은 부수적인 효과가 필요할 때. 외부의 변화를 동기화 할 때 필요하다.

react component가 뭘하던간에 1초 1초가 지나면 갱신되기에 시간또한 갱신되어야 할 외부효과에 해당한다.

의존성 배열을 통해 변화시에 rerendering할 state를 지정할 수 있다. 빈 배열일 경우 한번만 실행하고, 아닌 경우에는 하나씩 하나씩 추가해줄 수 있다.

### useContext

[useContext](https://react.dev/reference/react/useContext)는 component의 [context](https://react.dev/learn/passing-data-deeply-with-context) 를 읽고 구독하는 hook이다.

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
